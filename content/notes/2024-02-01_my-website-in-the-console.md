+++ 
title = "My website in the console" 
date = "2024-02-01"
slug = "my-website-in-console"
topics = ["nixos"]
draft = true
+++

# My website in the console

I like ascii art, I like hacker zine like [Phrack](http://phrack.org/) or [tmpout.sh](https://tmpout.sh).
The first version of my website was some sort of terminal in the browser.

{{< highlight toml >}}
[outputFormats.Txt]
name = "txt"
baseName = "index"
mediaType = "text/plain"
isPlainText = true

[outputs]
home = ["HTML", "txt"]
page = ["HTML", "txt"]
section = ["HTML", "txt", "RSS"]
{{</ highlight >}}

You have to create **TXT** templates following the look up order of `hugo`. 
and pass it the `{{ .RawContent }}`


Add to the HTML template

{{< highlight html >}}
<p>Read this page in your terminal with the command:</p>
<code>$ curl -L example.com{{ strings.TrimSuffix "/" (.RelPermalink) }} | less </code>
{{</ highlight >}}


in the Caddyfile.

{{< highlight caddyfile >}}
example.com {
    @isCurl header User-Agent *curl*
    handle @isCurl {
        rewrite * {path}/index.txt
    }

    root * /var/www/example.com
    file_server
}
{{</ highlight >}}
