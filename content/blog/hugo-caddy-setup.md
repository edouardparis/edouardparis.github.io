+++
title = "Hugo + Caddy = ❤️"
date = ""
draft = true
+++

# Setup Hugo

# Setup Caddy

```
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


