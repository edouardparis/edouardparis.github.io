+++ 
title = "Revault" 
date = "2020-09-07" 
topics = ["bitcoin"]
draft = true 
toc = true
+++

# 1. Bitcoin, petit rappel de fonctionnement

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
d'un problème mathématique donc la difficulté[^1] évolue dans le temps. 
Ce serveur d'horodatage permet à la fois de résoudre le problème de 
la [double dépense](https://fr.wikipedia.org/wiki/Double_d%C3%A9pense), 
celui des [généraux byzantins]() 
et l'émission de la monnaie. 
Une transaction une fois partagée dans le registre gagne à chaque bloc
une forme de sécurité, un adversaire souhaitant réecrire le registre
pour exclure la transaction devra fournir une preuve de travail
supérieur à la preuve de travail de l'ensemble des blocs succédants au
bloc de la transaction.

## 1.1 UNSPENT TRANSACTION OUTPUT

```ascii
                                            +──────────────────────+
                                            |       tx #2          |
                                            +──────────+───────────+
                                      +───▶ | input #2 | output #1 |
                                      |     +──────────────────────+
                                      |                | output #2 |
         +──────────────────────+     |                +───────────+
         |       tx #1          |     |
         +──────────+───────────+     |
   +───▶ | input #1 | output #1 +─────+
...      +──────────────────────+
   +───▶ | input #2 | output #2 +─────+     +──────────────────────+
         +──────────────────────+     |     |       tx #3          |
                    | output #3 |     |     +──────────+───────────+
                    +───────────+     └───▶ | input #1 | output #1 |
                                            +──────────────────────+
         utxos:  tx#1/output#3        +───▶ | input #2 |
                 tx#2/output#1        |     +──────────+
                 tx#2/output#2        |
                 tx#3/output#1    ... +

```

## 1.2 ScriptSig et ScriptKey


## 1.3 Pour aller plus loin

# 2. La Protection des bitcoins.

Posséder des bitcoins, c'est posséder

## 2.1 Portefeuille papier

## 2.2 Shamir secret

## 2.3 Hardware wallet

## 2.4 Multisig & Musig

# 3. REVAULT

## 3.1 Explication

Le premier document technique de Revault se trouve ici,
le protocole peut etre encore soumis a des changements, le problème 
touche a plusieurs facettes difficile de Bitcoin.

```ascii
+──▶    tx           tx           tx       deposit
|        +            +            +
|        |            |            |
|        |            |            |
|        ▼            ▼            ▼
|   +────────+   +────────+   +────────+
|   | utxo 1 |   | utxo 2 |   | utxo 3 |   vaults
|   +──+─+───+   +────+───+   +────+───+
|      | |            |            |
|      | |            |            |       +────────────+
|      | +────────────+────────────+─────▶ | emergency  |
|      |          emergency tx             | deep vault |
|      |                                   +────────────+
|      | unvault tx                               ^
|      |                                          |
|      +───────────────+                          |
|                      |                          |
|    cancel tx         ▼    unvault emergency tx  |
+──────────────────────+──────────────────────────+
                       |
                       |  spend tx
                       |
                       ▼
                      out
```


## 3.3 Tour de garde et politiques de dépenses

## 3.4 Mempool et frais de minage

## 3.2 Cosignataire et attaque par réplication

# 4. Covenants

## 4.1 BIP 119 - CHECKTEMPLATEVERIFY

## 4.2 TAPLEAF_UPDATE_VERIFY


[^1]: Attention le terme difficulté ne signifie pas dans ce contexte complexité
