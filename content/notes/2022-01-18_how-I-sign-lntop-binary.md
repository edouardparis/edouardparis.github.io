+++
title = "How I sign a LNTOP binary"
date = "2022-01-18"
draft = false
slug = "how-i-sign-lntop-binary"
+++

# How I sign a LNTOP binary

I am always amazed that someone, somewhere, is running my code or a
binary built from it. Since I released a little software named
[`lntop`](https://github.com/edouardparis/lntop) a few years ago, I
receive sometimes notifications from people asking for help or
new features and new release tags.
Because `lntop` is native to the terminal, it was added to the
[RaspiBolt](https://github.com/raspibolt/raspibolt/) guide as a little
optional tool for the cli-lover node operator. RaspiBolt developers
[asked](https://github.com/edouardparis/lntop/issues/49) me
about binaries release especially for ARM64 for their raspberry pi
users and I felt happily obliged to answer their request.

But, releasing binary is introducing a sort of responsibility from me
toward the people who will download it. Releasing code only is
convenient, people have to read it and then run it before making you
responsible for their loss. With a binary, they trust the platform
hosting it and the developer who compiled it.
For safety and for their (and my) peace of mind, I gpg-key signed
the last `lntop` releases.

I compiled the lntop binary for arm64 and paste the command, the tag
version of `lntop` and the [`go`](https://go.dev/) version I used
in a RELEASE.md file.

{{< code lang="shell" >}}
git checkout tags/v0.3.0
go version go1.16.4 linux/amd64
GOOS=linux GOARCH=arm64 go build ./cmd/lntop
{{< / code >}}

I created a directory with the release file, the binary and the LICENSE.

{{< code lang="shell" >}}
release-v0.3.0
├── LICENSE
├── lntop
└── RELEASE.md
{{< / code >}}

gzip and tar it:

{{< code lang="shell" >}}
tar -czf lntop-v0.3.0-Linux-arm64.tar.gz release-v0.3.0
{{< / code >}}

got the sha256 checksum in a file:

{{< code lang="shell" >}}
git checkout tags/v0.3.0
sha256sum -b lntop-v0.3.0-Linux-arm64.tar.gz > checksums-lntop-v0.3.0.txt
{{< / code >}}

gpg-key signed the checksum file:

{{< code lang="shell" >}}
gpg --detach-sig --sign --default-key a8ba... checksums-lntop-v0.3.0.txt
{{< / code >}}

I attached to the github release page of `lntop@v0.3.0` the files:
`lntop-v0.3.0-Linux-arm64.tar.gz`, `checksums-lntop-v0.3.0.txt`
and `checksums-lntop-v0.3.0.txt.sig`.

If someone wants to download and verify that the archive is the one I
created and signed, he/she can run:

{{< code lang="shell" >}}
sha256sum --check checksums-lntop-v0.3.0.txt
gpg --verify checksums-lntop-v0.3.0.txt.sig checksums-lntop-v0.3.0.txt
{{< / code >}}

My gpg key fingerprint is in my [twitter
bio](https://twitter.com/edouardparis).
Finally the binary and release files are extracted with:

{{< code lang="shell" >}}
tar -ztvf lntop-v0.3.0-Linux-arm64.tar.gz
{{< / code >}}
