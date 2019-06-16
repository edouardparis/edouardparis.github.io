+++
title = "Meetup Go 2019-06-18"
outputs = ["Reveal"]
[reveal_hugo]
highlight_theme = "vs"
custom_theme = "css/slides.css"
+++

#  Connect yourself to the lightning network with Go

An example with LND and lntop

<small>Edouard Paris - https://edouard.paris/slides/2019-06-18-meetup-go-paris</small>

---

## Bitcoin

* 1976 Asymetric encryption - Diffie-Hellman
* 1992 ECDSA - Scott Vanstone
* 1992 Cypherpunk: E. Hughes - T. May - J. Gilmore
* 1997 Hashcash - A. Black
* 1998 b-money - Wei Dai  | BitGold - N. Szabo
* 2008 Bitcoin whitepaper - S. Nakamoto
* 2009 first block mined


secp256k1: y² = x³ + 7

---

## Lightning Network

* 2013 Payment channels - Nakamoto & Core devs
* 2015 Feb *Lightning Network* - J.Poon - T.Dryja
* 2015 Nov *Reaching the ground with LN* - R. Russell
* 2017 Dec Specs V1.0 Rc | first tx on mainnet
* 2028 Avr Majors implementations are on mainnet

---

![example transactions bitcoin](transac.svg)

---

explication sigkey and bitcoin script

---

![bidirectional-channel](bidirectional-channel.svg)

---

explication HTLC

---

## The Lightning Daemon LND

- src: https://github.com/LightningNetwork/lnd
- doc: https://api.lightning.community/
- version: v0.7.0
- support: https://lightningcommunity.slack.com/

---

```go
package lnd

import (
	"io/ioutil"
	"net/url"

	"github.com/pkg/errors"
	"google.golang.org/grpc"
	"google.golang.org/grpc/credentials"
	macaroon "gopkg.in/macaroon.v2"

	"github.com/lightningnetwork/lnd/lncfg"
	"github.com/lightningnetwork/lnd/macaroons"
)

type Config struct {
	Address         string `toml:"address"`
	Cert            string `toml:"cert"`
	Macaroon        string `toml:"macaroon"`
	MacaroonTimeOut int64  `toml:"macaroon_timeout"`
	MacaroonIP      string `toml:"macaroon_ip"`
	MaxMsgRecvSize  int    `toml:"max_msg_recv_size"`
	ConnTimeout     int    `toml:"conn_timeout"`
}

func NewConn(c *Config) (*grpc.ClientConn, error) {
	macaroonBytes, err := ioutil.ReadFile(c.Macaroon)
	if err != nil {
		return nil, err
	}

	mac := &macaroon.Macaroon{}
	err = mac.UnmarshalBinary(macaroonBytes)
	if err != nil {
		return nil, errors.WithStack(err)
	}

	macConstraints := []macaroons.Constraint{
		macaroons.TimeoutConstraint(c.MacaroonTimeOut),
		macaroons.IPLockConstraint(c.MacaroonIP),
	}

	constrainedMac, err := macaroons.AddConstraints(mac, macConstraints...)
	if err != nil {
		return nil, errors.WithStack(err)
	}

	cred, err := credentials.NewClientTLSFromFile(c.Cert, "")
	if err != nil {
		return nil, err
	}

	u, err := url.Parse(c.Address)
	if err != nil {
		return nil, err
	}

	opts := []grpc.DialOption{
		grpc.WithTransportCredentials(cred),
		grpc.WithPerRPCCredentials(macaroons.NewMacaroonCredential(constrainedMac)),
		grpc.WithDialer(lncfg.ClientAddressDialer(u.Port())),
		grpc.WithDefaultCallOptions(grpc.MaxCallRecvMsgSize(c.MaxMsgRecvSize)),
	}

	conn, err := grpc.Dial(u.Hostname(), opts...)
	if err != nil {
		return nil, errors.WithStack(err)
	}

	return conn, nil
}
```



---

Macaroon

---

## LNTOP

https://github.com/edouardparis/lntop
![lntop-v0.1.0](lntop-v0.1.0.png)

---

architecture


---

### grpc connexion pool


---

### pubsub

---

### Ui - MVC

Schema MVC

---

Code MVC
