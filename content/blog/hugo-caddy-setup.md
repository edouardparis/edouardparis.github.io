+++
title = "Hugo + Caddy = ❤️"
date = ""
draft = true
toc = true
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

For this website:

* [Hugo](https://gohugo.io): one of the most popular open-source static site generators.
* [Caddy](https://caddyserver.com): a HTTP/2 web server with automatic HTTPS.

Graph for explanation:

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

# Setup Caddy

```green
http://edouard.paris {
    redir https://{host}{uri}
}

https://edouard.paris {
    root /var/www/edouard.paris/public
    git git@github.com:edouardparis/edouard.paris {
        path ../
        hook /webhook-endpoint my-webhook-password
        then /usr/bin/hugo --destination /var/www/edouard.paris/public
    }
}
```


