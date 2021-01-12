+++ 
title = "Revault" 
date = "2020-09-07" 
topics = ["bitcoin"]
draft = true 
toc = true
+++

# Bitcoin, petit rappel de fonctionnement

Bitcoin, qu'on l'aime ou le déteste difficile de l'ignorer si on s'intéresse
aux politiques monétaire et encore moins aux alternatives économique 
dans notre société de l'information.   
Je suis convaincue que ce protocole va prendre de l'importance pendant les
prochaines années pour de multiple raisons qui méritent un article à part.

Cependant pour comprendre le fonctionnement de Revault, il faut expliquer certaines
propriétés de Bitcoin.

Bitcoin est une monnaie numérique reposant sur le partage par un réseau de pairs à
pairs d'un registre de transaction. Les transactions sont groupées et
ordonnées en blocs par un serveur d'horodatage décentralisé.
Ce serveur fonctionne par la libre compétition de pairs à la résolution 
d'un problème mathématique donc la difficulté[¹](#1) évolue dans le temps. 
Ce serveur d'horodatage permet à la fois de résoudre le problème de 
la [double dépense](https://fr.wikipedia.org/wiki/Double_d%C3%A9pense), 
celui des [généraux byzantins]() 
et l'émission de la monnaie. 
Une transaction une fois partagé dans le registre gagne à chaque bloc
une forme de sécurité.

## UNSPENT TRANSACTION OUTPUT

```ascii
                                            +----------------------+
                                            |       tx #2          |
                                            +----------+-----------+
                                      +---> | input #2 | output #1 |
                                      |     +----------------------+
                                      |                | output #2 |
         +----------------------+     |                +-----------+
         |       tx #1          |     |
         +----------+-----------+     |
   +---> | input #1 | output #1 +-----+
...      +----------------------+
   +---> | input #2 | output #2 +-----+     +----------------------+
         +----------------------+     |     |       tx #3          |
                    | output #3 |     |     +----------+-----------+
                    +-----------+     +---> | input #1 | output #1 |
                                            +----------------------+
         utxos:  tx#1/output#3        +---> | input #2 |
                 tx#2/output#1        |     +----------+
                 tx#2/output#2        |
                 tx#3/output#1    ... +

```

## ScriptSig et ScriptKey


## Pour aller plus loin

                 
# La Protection des bitcoins.


# REVAULT

Le premier document technique de Revault se trouve ici,
le protocole peut etre encore soumis a des changements, le problème 
touche a plusieurs facettes difficile de Bitcoin.

```ascii
+-->    tx           tx           tx       deposit
|        +            +            +
|        |            |            |
|        |            |            |
|        v            v            v
|
|   +--------+   +--------+   +--------+
|   | utxo 1 |   | utxo 2 |   | utxo 3 |   vaults
|   +--+-+---+   +----+---+   +----+---+
|      | |            |            |
|      | |            |            |       +------------+
|      | +------------+------------+-----> | emergency  |
|      |          emergency tx             | deep vault |
|      |                                   +------------+
|      | unvault tx                               ^
|      |                                          |
|      +---------------+                          |
|                      |                          |
|    cancel tx         v    unvault emergency tx  |
+-------------------------------------------------+
                       |
                       |  spend tx
                       |
                       v
                      out
```

## Les transactions presignés

## La dépense d'un vault

## L effrayante Mempool

#1: Attention le terme difficulté ne signifie pas dans ce contexte complexité

Ascii art from:
fish: https://ascii.co.uk/art/fish (I added the wizard hat)
