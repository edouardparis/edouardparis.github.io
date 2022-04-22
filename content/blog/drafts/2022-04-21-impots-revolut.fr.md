+++ 
title = "Revolut et les impôts"
date = "2022-04-21"
slug = "impots-revolut"
topics = ["bitcoin"]
draft = true
toc = false
+++

# Revolut et les impôts

En 2017, j'ai commis plusieurs erreurs. L'une d'entre elle est d'avoir
par curiosité, boursicoté sur l'application Revolut.
Revolut venait de ajouter une fonctionnnalité de revente et d'achats de
cryptomonnaie pour se démarquer de ses concurrents.

Selon le code des impôts, les plus values réalisées à l'occasion de la revente de crypto-monnaies sont imposables. 

---

**notice du formulaire n°2086**

**Fractions de capital initial**
Il s’agit de la fraction de capital contenue dans la valeur ou le prix de chacune des différentes cessions d'actifs numériques à
titre gratuit ou onéreux réalisées antérieurement, hors opérations d’échange ayant bénéficié du sursis d’imposition sans soulte.
Ces fractions de capital initial réduisent le prix total d’acquisition.

**Exemple :**

En janvier N, un contribuable fiscalement domicilié en France acquiert pour 1 000 € d'actifs numériques (il ne détient jusqu’alors aucun actif numérique).
En mars N, la valeur globale de son portefeuille est de 1 200 €. Il réalise alors une cession de 450 €.
La plus-value afférente à cette cession est déterminée comme suit : 450 – (1000 x 450 / 1200) = 450 – 375 € = 75 €.

En août N, la valeur globale de son portefeuille est de 1 300 €. Il décide alors de céder l'ensemble des actifs numériques qu'il détient.
Pour déterminer la plus-value afférente à cette nouvelle cession, il convient de minorer le prix total d'acquisition de la fraction de capital initial (375 €) déduite lors de la précédente cession. 
Soit : 1300 – [(1000 – 375) x 1300 / 1300] = 1300 – 625 = 675 € de plus-value
 
---

Heureusement, Revolut a ajouté une

| Status   | Method   | Domain          | File                                                                                      | ... | Type      |
| -------- | -------- | --------------- | ----------------------------------------------------------------------------------------- | --- | --------- |
| 200      | GET      | app.revolut.com | /api/retail/user/current/transactions/last?to=1593598035404&count=50&internalPocketId=... | ... | json      |

Clique droit: `Copy` > `Copy as cURL`

```pink
curl --get
  --data-urlencode "to=1593598035404"
  --data-urlencode "count=50"
  --data-urlencode "InternalPocketId=<id>
  'https://app.revolut.com/api/retail/user/current/transactions/last'
  -H 'User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:91.0) Gecko/20100101 Firefox/91.0' 
  -H 'Accept: application/json, text/plain, */*' 
  -H 'Accept-Language: en-US,en;q=0.5' --compressed 
  -H 'Referer: https://app.revolut.com/crypto/BTC' 
  -H 'x-browser-application: WEB_CLIENT' 
  -H 'x-client-version: 100.0' 
  -H 'x-client-geo-location: <location>' 
  -H 'x-device-id: <device-id>' 
  -H 'Alt-Used: app.revolut.com' 
  -H 'Connection: keep-alive' 
  -H 'Cookie: <cookie>' 
  -H 'Sec-Fetch-Dest: empty' 
  -H 'Sec-Fetch-Mode: cors' 
  -H 'Sec-Fetch-Site: same-origin' 
  -H 'TE: trailers'
```

La réponse à la requête est un fichier de format json comprenant une
liste d'objet qui chacun caractèrise un événement dans une monnaie
donnée.

Un acte d'achat ou de vente est représenté par deux
événements avec le même champs `id`, un `sell` et un `buy` qui
donnent l'information respectivement du point de vue des portefeuilles
des deux monnaies échangées.

Example pour la vente de euros (`direction: "sell"`):

```green
{
    "id": "...",
    "legId": "...",
    "type": "EXCHANGE",
    "state": "COMPLETED",
    "startedDate": 1512982152612,
    "updatedDate": 1512982152881,
    "completedDate": 1512982152881,
    "createdDate": 1512982152612,
    "currency": "EUR",
    "amount": -40000,
    "fee": 0,
    "feeBreakdownId": "...",
    "balance": 0,
    "description": "Exchanged to BTC",
    "tag": "general",
    "category": "general",
    "account": {
      "id": "...",
      "type": "CURRENT"
    },
    "suggestions": [],
    "transparentPricing": true,
    "rate": 7.08609e-05,
    "direction": "sell",
    "counterpart": {
      "amount": 2834436,
      "currency": "BTC",
      "account": {
        "id": "...",
        "type": "CURRENT"
      }
    },
    "localisedDescription": {
      "key": "transaction.description.internal.exchange.buy.currency",
      "params": [
        {
          "key": "to_currency",
          "value": "BTC"
        }
      ]
    }
  },
```

l'achat de BTC (`direction: "buy"`):

```green
  {
    "id": "...",
    "legId": "...",
    "type": "EXCHANGE",
    "state": "COMPLETED",
    "startedDate": 1512982152612,
    "updatedDate": 1512982152881,
    "completedDate": 1512982152881,
    "createdDate": 1512982152612,
    "currency": "BTC",
    "amount": 2834436,
    "fee": 0,
    "feeBreakdownId": "...",
    "balance": 2834436,
    "description": "Exchanged from EUR",
    "tag": "general",
    "category": "general",
    "account": {
      "id": "...",
      "type": "CURRENT"
    },
    "suggestions": [],
    "transparentPricing": true,
    "rate": 14112.1549401715,
    "direction": "buy",
    "counterpart": {
      "amount": -40000,
      "currency": "EUR",
      "account": {
        "id": "...",
        "type": "CURRENT"
      }
    },
    "localisedDescription": {
      "key": "transaction.description.internal.exchange.buy.currency",
      "params": [
        {
          "key": "to_currency",
          "value": "BTC"
        }
      ]
    }
  }
```
