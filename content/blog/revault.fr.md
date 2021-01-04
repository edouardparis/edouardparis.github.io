+++ 
title = "Revault" 
date = "2020-09-07" 
topics = ["bitcoin"]
draft = true 
toc = true
+++

Novembre 2020, je quitte mon travail actuel de développeur chez
[Ulule](https://ulule.com). J'ai adoré cette expérience  et c'est avec un petit pincement
au cœur que se finit pour moi mon premier CDI. Ulule m'a recruté
directement après mon stage de fin d'étude chez eux. J'étais un étudiant
qui par curiosité allait à des meetups Go sur Paris et Ulule m'a fait
découvrir et aimer le développement web.

Je pars vivre une aventure entrepreneuriale encore plus intense.
Mon nouvel employeur est une petite compagnie portugaise fondé par deux
Francais, qui possede le doux nom de: 

```ascii
     *                          O    .           ____
            *       /\            .   o        .'* *.'            *
       *           /*.\            o        __/_*_*(_
                  /----\     WizardSardine   /o )   \ ,';     *
             ;`, /   ( o\        O .         \ ,'    ;  /
       *     \  ;    `, /       .    o        "-.__.'"\_;       *
             ;_/"`.__.-"       _________                   *        *
                             c(`       ')o
    *             *            \.     ,/             *         *
                              _//^---^\\_   
```

Kévin Loaec et Antoine Poinsot sont deux Bitcoiners endurcis avec
lesquels je partage la meme vision de l'open source et dont j'admire la
connaissance technique et le scepticisme vis a vis des réseaux pairs 
a pairs et de la sécurité en informatique en général.

Le principal produit de WizardSardine est Revault, un nouveau concept de
protection de fonds pour des protocoles monétaires utilisant le format
de transaction des UTXOs notamment Bitcoin. 

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
