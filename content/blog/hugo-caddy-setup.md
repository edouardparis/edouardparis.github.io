+++
title = "Hugo + Caddy = ❤️"
date = ""
draft = true
toc = false
+++

```ascii
   __ __                   __
  / // /_ _____ ____    __/ /_
 / _  / // / _ `/ _ \  /_  __/
/_//_/\_,_/\_, /\___/_  /_/       _   _
 / ___/__ /___/ /__/ /_ __  ____ /  \/ \
/ /__/ _ `/ _  / _  / // / /___/ \     /
\___/\_,_/\_,_/\_,_/\_, / /___/   \___/
                   /___/
```

* [Hugo](https://gohugo.io): one of the most popular open-source static site generators.
* [Caddy](https://caddyserver.com): a HTTP/2 web server with automatic HTTPS.

Hugo and Caddy are two powerful tools written in [Go](https://golang.org).
Easy for me to install and follow the current development.
They are both pretty fast and they are very good for simple tasks like
generating and hosting a static website with a [let's encrypt](https://letsencrypt.org)
certificate.

With a nice system of webhooks, Caddy and Hugo enable a smooth way to
deploy a current version of my website after a simple commit.
In this blog post, I will describe each of these tools and how to setup
the server as I did.

# Global architecture

```ascii
https://github.com                   https://edouard.paris
+------------------+ POST /webhook   +--------+----------+
|                  +---------------->+        |          |
| git@github.com:..|        git pull | Caddy +--> Hugo   |
|                  +<----------------+        |          |
+--------+---------+                 +---+----+----------+
         ^                               ^
         |                               |
         | git push                      | HTTP GET
         |                               |
+--------+--------+                  +---+----------+
| ~/edouard.paris |                  | Client       |
| .git ...        |                  +--------------+
+-----------------+
```

# Setup Hugo

# Setup GitHub settings

In the related repository you own:

"Settings" (in the repository tabs) > "Webhooks" (on the left panel) >
"Add WebHook".

Choose an url for example https://your-website.com/webhooks, and a
secret. Select:

- "application/json" as the content type.
- "Enable SSL verification"
- "Just the push event"
- "Active" for the delivery of event details when the hook is triggered

# Setup Caddy

```green
http://edouard.paris {
    redir https://{host}{uri}
}

https://edouard.paris {
    root /var/www/edouard.paris/public
    git git@github.com:edouardparis/edouard.paris {
        path ../
        hook /webhook-endpoint my-webhook-secret
        then /usr/bin/hugo --destination /var/www/edouard.paris/public
    }
}
```


