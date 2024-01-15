+++ 
title = "Install NixOs on an OVH vps with nixos-anywhere" 
date = "2024-01-15"
slug = "install-nixOs-on-an-ovh-vps-with-nixos-anywhere"
topics = ["nixos"]
+++

# Install NixOs on an OVH vps with nixos-anywhere

2023 was the year I was nix pilled. 2024 is the year for the experimentations.
I discovered the power of NixOs for server deployment with this talk from
Carl Dong: [The dark arts of NixOs deployments](https://www.youtube.com/watch?v=bKTbis4elR8&t=5519s).

{{< sidenote >}}
In his talk, Carl referenced a 
[blog post from Haskell for all](https://www.haskellforall.com/2023/01/announcing-nixos-rebuild-new-deployment.html) 
about nixos-rebuild usage for deployment.
{{</ sidenote >}}

He talked about the new nixos-rebuild command to easily change and deploy the
configuration of a server and how using [kexec(8)](https://www.man7.org/linux/man-pages/man8/kexec.8.html)
and [disko](https://github.com/nix-community/disko)
to install NixOs on it without the [nixos-infect](https://github.com/elitak/nixos-infect) scripts
(which are more a hack than a clean way to do it).

This inevitably leads me to [nixos-anywhere](https://github.com/nix-community/nixos-anywhere) 
a great tool using kexec and disko to deploy custom flakes !

The repository gives an example using Hetzner vps provider, but I wanted to try to install
nixos on an OVH vps. Here a quick guide of how I did it:

## 1. Create vps and get credentials

- Create an ovh account
- Order vps with debian 12
- Ovh send to you email with id, ip, and link to create a password 

you can connect with `ssh debian@<vps-ip>`.

## 2. Change password with `passwd` and add ssh key to authorized keys.

{{< highlight console >}}
$ sudo vim /root/.ssh/authorized_keys
$ sudo systemctl restart ssh
{{</ highlight >}}

## 3. Install nix

{{< highlight console >}}
$ sh <(curl -L https://nixos.org/nix/install) --daemon
{{</ highlight >}}

Nix won't work in active console sessions until you restart them.
exit and reconnect in a new root ssh session.

{{< highlight console >}}
$ nix-env -iE "_: with import <nixpkgs/nixos> { configuration = {}; }; \
  with config.system.build; [ nixos-generate-config ]"

$ nixos-generate-config --no-filesystems --root /mnt
{{</ highlight >}}

Copy from `/mnt` the `hardware-configuration.nix` file. 

The rest of the commands should be done on your local machine and not on the target host.

## 4. Prepare flake

You can find an example of the flake [here](https://github.com/edouardparis/nixos-ovh-vps-example).
Change the `hardware-configuration.nix` file with the one you copied and change `disk-config.nix`

## 5. Test flake

{{< highlight console >}}
$ nix run github:nix-community/nixos-anywhere -- --flake .#ovh-vps --vm-test
{{</ highlight >}}

## 6. Load nixos-anywhere

{{< highlight console >}}
$ nix run github:nix-community/nixos-anywhere -- --flake .#ovh-vps root@<vps-ip>
{{</ highlight >}}

## 7. Reload configuration

Add `screenfetch` to the flake configuration.nix `environment.systemPackages`

{{< highlight console >}}
$ nixos-rebuild switch --flake .#ovh-vps --target-host "root@<vps-ip>"
{{</ highlight >}}

Connect to the host

{{< highlight console >}}
$ ssh root@<vps-ip>
Last login: Tue Jan  2 15:18:52 2024 from <ip>
{{</ highlight >}}

then check that the `screenfetch` package was installed.

{{< highlight console >}}
[root@nixos:~]# screenfetch
          ::::.    ':::::     ::::'         root@nixos
          ':::::    ':::::.  ::::'          OS: NixOS 24.05.20231221.d6863cb
            :::::     '::::.:::::           Kernel: x86_64 Linux 6.1.69
      .......:::::..... ::::::::            Uptime: 7m
     ::::::::::::::::::. ::::::    ::::.    Packages: 470
    ::::::::::::::::::::: :::::.  .::::'    Shell: bash 5.2.21
           .....           ::::' :::::'     Disk: 1.1G / 20G (6%)
          :::::            '::' :::::'      CPU: Intel Core (Haswell, no TSX)
 ........:::::               ' :::::::::::. GPU: Cirrus Logic GD 5446
:::::::::::::                 ::::::::::::: RAM: 250MiB / 1935MiB
 ::::::::::: ..              :::::           
     .::::: .:::            :::::            
    .:::::  :::::          '''''    .....    
    :::::   ':::::.  ......:::::::::::::'    
     :::     ::::::. ':::::::::::::::::'     
            .:::::::: '::::::::::            
           .::::''::::.     '::::.           
          .::::'   ::::.     '::::.          
         .::::      ::::      '::::.         

{{</ highlight >}}

The Nixos VPS is ready ! Modify and reload configuration as you will ! ❄️
