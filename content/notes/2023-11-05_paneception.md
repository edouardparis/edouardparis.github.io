+++ 
title = "Paneception" 
date = "2023-11-05"
slug = "paneception"
draft = true
+++

I reinstalled Tmux.

I was using i3 then sway since 2017 navigate through windows panel and open
terminal quickly on demand to throw commands quicky.  But after installing nixos
on my laptop, I wanted to encapsulate libraries, required services and
environment variables of my personal an work projects into nix files that I
could load with `nix-shell`.  When opening a new kitty terminal window with
sway, the new instance does not inherit the state of the previous shell. I can
have a simple script in sway config on new window event that move the user to
the working directory and trigger again the nix-shell command if a default.nix
file is present.  But I dont really like to rely on an extra configuration of my
window manager to manage my programming process.

{{< sidenote >}}
> I switched my primary dev environment to a graphical NixOS VM on a macOS host. It has been wonderful. I can keep the great GUI ecosystem of macOS, but all dev tools are in a full screen VM. One person said “it basically replaced your terminal app” which is exactly how it feels. - [@mitchellh](https://twitter.com/mitchellh/status/1346136404682625024)
{{</ sidenote >}}

In fact, I was really inspired by [Mitchell Hashimoto experience with nix](https://github.com/mitchellh/nixos-config).
Quicly explained, a nix configured virtual machine delivers portability and
reproducibility that guarantee a perfectly controlled experience on very good
hardware, in his case Apple macbook Pro. What if one day I wanted to develop
with my full neovim configuration, with all my shell aliases and scripts in a
virtual machine runned on a lightweight Ipad? The time I will do the step to buy
one, the performance will surpass that of my three years old laptop. What if I
tried to code from a distant vps ?

In order to keep the environment simple, I dont want to have window manager
dependency in it.  I want to try to develop and manage a project from one
workspace pane, hence good old friend `tmux`. A `tmux` session open and close
nested pane with the `nix-shell` inherited sandboxing.

I can also work again on my ossificated vim skills to manage everything from the
editor in multiple buffer, but for now courage is missing.

# [ [ [ vim ] tmux ] sway ]

|                     | vim       | tmux       | sway              |
|---------------------|-----------|------------|-------------------| 
| New vertical pane   | `:vsplit` | `Ctrl+a %` | `Mod+Enter`       | 
| New horizontal pane | `:split`  | `Ctrl+a "` | `Mod+v Mod+Enter` |
