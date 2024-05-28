+++
title = "Monitor Caddy logs with goaccess"
date = "2024-05-17"
draft = true
slug = "caddy-logs-and-goaccess"
topics = ["caddy", "goaccess", "logs"]
+++

This website has no javascript, no cookie, no tracker whatsoever,
but I finally wanted to keep track of the number of reads of my writings.
The problem is how to keep a

Caddy a way to keep logs and mask parts of visitors IP.

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

goaccess needs information about the specific format otherwise it will
fail to parse the logs files.

{{< code lang="shell" >}}
goaccess /var/log/caddy/edouardparis.log --log-format=CADDY
{{< / code >}}

{{<image src="/notes/images/2024-05-22_caddy-logs-and-goaccess/goaccess-cli.png" alt="goaccess console interface" >}}

If I want to have on my laptop a better overview with graphs,
I use goaccess to generate a HTML file with the output flag and fetch it through `SSH`.

{{< code lang="shell" >}}
ssh -n root@edouard.paris 'cat /var/log/caddy/edouardparis.log' | goaccess --log-format=CADDY -o stats.html -
{{< / code >}}

I read then the `stats.html` file using a local server (`python -m http.server`)
that let me access it at `localhost:8000/stats.html`

{{<image src="/notes/images/2024-05-22_caddy-logs-and-goaccess/goaccess-html-file.png" alt="goaccess generated HTML file" >}}


{{< code lang="shell" >}}
caddy hash-password --plaintext "<password>"
{{< / code >}}

It provides a `bcrypt` hash of the password that we use in the
caddy section of the website as an authentication requirement
using the [`basicauth` directive](https://caddyserver.com/docs/caddyfile/directives/basicauth).

Define the file server directory with authentication requirement:

{{< code lang="caddyfile" >}}
stats.edouard.paris {
    root * /var/www/stats.edouard.paris
    file_server

    basicauth {
        edouard <hash>
	}
}
{{< / code >}}

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
  description = "Run goaccess every 10 minutes";
  wantedBy = [ "timers.target" ];
  timerConfig = {
    OnBootSec = "5min";
    OnUnitActiveSec = "10min";
  };
};
{{< / code >}}
