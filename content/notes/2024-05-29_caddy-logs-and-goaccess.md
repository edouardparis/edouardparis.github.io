+++
title = "Monitor Caddy logs with goaccess"
date = "2024-05-29"
draft = false
slug = "caddy-logs-and-goaccess"
topics = ["caddy", "goaccess", "logs", "nixos"]
+++

#  Monitor Caddy logs with GoAccess

This website has no javascript, no cookie, no tracker whatsoever,
but I finally wanted to keep track of the number of reads of my writings.
The problem is how to keep records of some numbers like the total
of visitor for a day or the list of referring websites while keeping few
information about clients IPs.

Caddy is my first choice as an simple http server for hosting all my static
websites. It is easy to setup and it has HTTPS automatic support.

Caddy provides a straightforward way to log requests and mask parts of visitors' IP addresses.
Here’s how I configured Caddy to log visitor data while respecting user privacy:

{{< code lang="caddyfile" >}}
edouard.paris {
    ...

    log {
        output file /var/log/caddy/edouardparis.log
        format filter {
            wrap json
            fields {
                request>remote_ip ip_mask {
                    ipv4 16
                    ipv6 32
                }
            }
        }
    }
}
{{</ code >}}

`ipv4 16` masks the last 16 bits of IPv4 addresses, while
`ipv6 32` masks the last 32 bits of IPv6 addresses.

Looking for a tool for monitoring these logs, I found
[GoAccess](https://goaccess.io), a real-time web log analyzer and interactive viewer
written in the Go language.

GoAccess needs information about the specific format otherwise it will
fail to parse the logs files.

{{< code lang="shell" >}}
goaccess /var/log/caddy/edouardparis.log --log-format=CADDY
{{< / code >}}

GoAccess console interface is quite dense and seems to be quite a good
tool for a sysadmin that needs a tool for a quick overview of the traffic
that his server is handling.

{{< sidenote >}}
List of the panels:

* Unique visitors per day - Including spiders
* Requested Files (URLs)
* Static Requests
* Not Found URLs (404s)
* Visitor Hostnames and IPs
* Operating Systems
* Browsers
* Time Distribution
* Virtual Hosts
* Referring Sites
* HTTP Status Codes
* MIME Types
* Encryption settings
{{< / sidenote >}}

{{<image src="/notes/images/2024-05-29/goaccess-cli.png" alt="GoAccess console interface" >}}

If I want to have on my laptop a better overview with graphs,
I use `goaccess` cli to generate a HTML file with the output flag and fetch it through `SSH`.

{{< code lang="shell" >}}
ssh -n root@edouard.paris 'cat /var/log/caddy/edouardparis.log' | goaccess --log-format=CADDY -o stats.html -
{{< / code >}}

I read then the `stats.html` file using a local server (`python -m http.server`)
that let me access it at `localhost:8000/stats.html`

{{<image src="/notes/images/2024-05-29/goaccess-html-file.png" alt="GoAccess generated HTML file" >}}

Generating and reading the HTML report manually was tedious.
I wanted my server to automatically generate and serve the GoAccess HTML report,
protected by basic authentication. Here’s how I set it up.

Caddy provides a simple directive to protect access to files or
directories and NixOs has very simple configuration for setting up systemd services
and timers.

First, I generated a long password with my password manager, then used a command
from `caddy` cli:

{{< code lang="shell" >}}
caddy hash-password --plaintext "<password>"
{{< / code >}}

The output is a `bcrypt` hash of the password used in the
`Caddyfile` section of the website as an authentication requirement
using the [`basicauth` directive](https://caddyserver.com/docs/caddyfile/directives/basicauth).

{{< code lang="caddyfile" >}}
stats.edouard.paris {
    root * /var/www/stats.edouard.paris
    file_server

    basicauth {
        edouard <hash>
	}
}
{{< / code >}}

Here are the section of my `configuration.nix` I added:

{{< code lang="nix" >}}
system.activationScripts = {
  goaccessPermissions = ''
    mkdir -p /var/www/stats.edouard.paris
    chown caddy:caddy /var/www/stats.edouard.paris
    chmod 750 /var/www/stats.edouard.paris
  '';
};

systemd.services.goaccess = {
  description = "Run goaccess to generate website statistics";
  serviceConfig = {
    ExecStart = "${pkgs.bash}/bin/bash -c '${pkgs.goaccess}/bin/goaccess -f /var/log/caddy/edouardparis.log --log-format=CADDY -o /var/www/stats.edouard.paris/index.html --persist'";
    User = "caddy";
  };
};

systemd.timers.goaccess = {
  description = "Run goaccess every 2 hours";
  wantedBy = [ "timers.target" ];
  timerConfig = {
    OnBootSec = "5min";
    OnUnitActiveSec = "2h";
  };
};
{{< / code >}}

This configuration sets up a systemd service to run GoAccess every two hours,
generating an HTML report accessible only by me at https://stats.edouard.paris.
