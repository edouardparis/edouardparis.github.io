+++ 
title = "nixos-anywhere and ovh" 
date = "2024-01-12"
slug = "nixos-anywhere-and-ovh"
topics = ["nixos"]
draft = true
+++

[nixos-anywhere](https://github.com/nix-community/nixos-anywhere) is 
an interesting flake.

## 1. Create vps and get credentials

- Create an ovh account
- Order vps with debian 12
- Ovh send to you email with id, ip, and link to create a password 

you can connect with `ssh debian@<vps-ip>`.

## 2. Change password with `passwd` and add ssh key to authorized keys.

{{< highlight shell >}}
sudo vim /root/.ssh/authorized_keys
sudo systemctl restart ssh
{{</ highlight >}}

## 3. Install nix

{{< highlight shell >}}
sh <(curl -L https://nixos.org/nix/install) --daemon
{{</ highlight >}}

Nix won't work in active shell sessions until you restart them.
exit and reconnect in a new root ssh session.

{{< highlight shell >}}
nix-env -iE "_: with import <nixpkgs/nixos> { configuration = {}; }; \
  with config.system.build; [ nixos-generate-config ]"

nixos-generate-config --no-filesystems --root /mnt
{{</ highlight >}}

Copy from `/mnt` the `hardware-configuration.nix` file. 

The rest of the commands should be done on your local machine and not on the target host.

## 4. Prepare flake

You can find an example of the flake [here](https://github.com/edouardparis/nixos-ovh-vps-example).
Change the `hardware-configuration.nix` file with the one you copied and change `disk-config.nix`

## 5. Test flake

{{< highlight shell >}}
nix run github:nix-community/nixos-anywhere -- --flake .#ovh-vps --vm-test
{{</ highlight >}}

## 6. Load nixos-anywhere

{{< highlight shell >}}
nix run github:nix-community/nixos-anywhere -- --flake .#ovh-vps root@<vps-ip>
{{</ highlight >}}

## 7. Reload configuration

Add `screenfetch` to the flake configuration.nix `environment.systemPackages`

{{< highlight shell >}}
nixos-rebuild switch --flake .#ovh-vps --target-host "root@<vps-ip>"
{{</ highlight >}}

Connect to the host

{{< highlight shell >}}
ssh root@<vps-ip>
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
