# Comment fonctionne Internet 

Tu t'es déjà demandé comment fonctionne Internet ? Voici une explication simplifiée.

## Le texte n'est qu'une suite de nombres

Chaque lettre a un nombre associé. Découvrez l'encodage [ASCII](https://fr.wikipedia.org/wiki/American_Standard_Code_for_Information_Interchange), où un espace est 32 et A majuscule 65. Majuscule B est 66, C 67 etc.

Pourquoi A vaut 65 et pas 88 par exemple ? Les créateurs d'ASCII en ont choisi ainsi, tout simplement. ASCII est en fait une grille d'associations. Tu peux en créer une. Personne va l'utiliser, mais tu peux. La raison d'utiliser ASCII est que tout le monde l'utilise. Ainsi le nombre 65 est transformé en lettre A sur ton ordinateur et le mien. Si on ne se met pas d'accord quelle nombre représente quelle lettre, on ne va pas pouvoir s'échanger du texte. 

Par exemple __LOVE__ en ASCII c'est (76, 79, 86, 69).

Il y a d'autres encodages. utf-8, latin-1 etc. qui peuvent gérer les accents et autres alphabets.

Donc le texte n'est qu'une suite de nombres.
Tu sais probablement que les ordinateurs utilisent les nombres binaires en interne. Un ordinateur ne peut pas directement comprendre A (65).


### Les nombres sont binaires

La plupart des humains utilisent la base 10 pour leur système numérique, cela vient du fait qu'on a 10 doigts, et c'est facile de compter jusqu'à dix. Base 10, ça veut dire qu'il y a 10 symboles différents 0-6, en incluant le 0. Le binaire est un système numérique de base 2. Il y a 2 symboles: 0 et 1. Chaque décimal (base 10) peut être convertit en sa représentation binaire (base 2) Par exemple
 
 * décimal 1024 est binaire 10000000000
 * décimal 1025 est binaire 10000000001
 * décimal 1026 est binaire 10000000010
 * décimal 1027 est binaire 10000000011

Pour comprendre comment écrire un nombre en binaire, essayons une manière alternative d'écrire 1024 en décimal. (x puissance 0 est 1)

 * `1027 = (1000) + (0) + (20) + (7)`
 * `1027 = (1 * 10**3) + (0 * 10**2) + (2 * 10**1) + (7 * 10**0)`
 * `1027 = (1 * 10**3) + (2 * 10**1) + (7 * 10**0)`

N'importe quel nombre en décimal peut s'écrire en tant que somme comme ça. En utilisant la même méthode, mais cette fois-ci avec la base 2

 * `111 = (100) + (10) + (1)`
 * `111 = (1 * 2**2) + (1 * 2**1) + (1 * 2**0)`
 * `111 = (1 * 4) + (1 * 2) + (1 * 1)`
 * `111 = (4) + (2) + ( 1)`
 * `111 = 7`
 

Un autre exemple (1027):
 
 * `10000000011 = (10000000000) + (0) + (0) + (0) + (0) + (0) + (0) + (0) + (0) + (10) + (1)`
 * `10000000011 = (1 * 2**10) + (0 * 2**9) + (0 * 2**8) + (0 * 2**7) + (0 * 2**6) + (0 * 2**5) + (0 * 2**4) + (0 * 2**3) + (0 * 2**2) + (1 * 2**1) + (1 * 2**0)`
 * `10000000011 = (1 * 2**10) + (1 * 2**1) + (1 * 2**0)`
 * `10000000011 = (1024) + (2) + (1)`

On peut convertir une lettre en nombre  avec une grille d'associations. On peut convertir un nombre décimal en binaire. On peut donc aller de lettre à binaire. 1000001 est binaire pour la lettre A.

Un bit est un 0 ou 1 singulier.

Un octet est composé de 8 bits.

Mais comment envoyer un nombre ? C'est une abstraction. Ca n'existe pas dans le monde physique. Comment les ordinateurs s'échangent des nombres ?

![Abstractimage](https://letz.social/wollek/sac/d/fichiers_blog/moibcolor2of2.jpg)

## Le binaire c'est du voltage

Bonjour le monde physique, où l'on utilise l'électricité pour envoyer des nombres. Par exemple, un voltage élevé est 1 et un voltage bas est 0. Donc comment on envoie la lettre A (1000001) par exemple ?

 - 1 Haut
 - 0 Bas
 - 0 Bas
 - 0 Bas
 - 0 Bas
 - 0 Bas
 - 1 Haut

Pendant combien de temps doit-on garder le courant électrique à haut voltage ? La durée est la période, qui doit être déterminée. Exemple: 1 seconde. Il faudrait 7 secondes pour envoyer la lettre A. Trop long. Rien que cette phrase prendrait 40 fois 7 secondes au moins. Une période de 1ms serait 1000 fois plus rapide. Mais il y a un minimum pour la période. Plus la période est courte, plus il faut du matériel précis.

Que doit faire l'ordinateur quand rien n'a encore été envoyé ? Le voltage sera bas et l'ordinateur pourrait croire qu'il reçoit des zéros. C'est pourquoi il y a une phase de synchronisation: Comme 0101010101 suivi par 1111111111111111111111111 uns et ensuite le vrai message commence. Sans cela (111) pourrait être interprété comme (011, 100) décalage de 1.

Et, on envoie où les bits ?


## Protocole Internet (IP)

Comme dans la vrai vie, si tu veux envoyer un message, il faut déjà savoir à qui, et ensuite il faut son adresse.

Le protocole Internet défini une adresse comme une série de nombres. Chaque appareil connecté à Internet a une adresse IP. Un adresse IPv4 ressemble à ça: 180.160.299.88.

Les messages IP ont une taille maximale, donc une image de haute définition ou vidéo, doit être réparti dans plusieurs messages.

### IPv4

Vous avez remarqué quelque chose ? L'adresse n'a que 11 chiffres, mais il y a plus de 99999999999 appareils connecté à Internet, ce qui veut dire qu'on est à court d'adresses ! Les créateurs d'IP n'ont jamais imaginé la taille qu' Internet prendrait.

### IPv6

Les adresses IP 6 ont été crée pour résoudre les problèmes d IPv4 avec des ordres de grandeurs d'adresses en plus.

### Protocole d'envoie



Un message IP doit contenir

 - l'adresse cible
 - l'adresse source
 - le message
 - le temps de vie (ttl)
 - la longueur total

L'adresse source est requise pour que la cible puisse savoir où envoyer la réponse. Si j'ouvre https://fr.wikipedia.org les ordinateurs Wikipédia doivent connaître mon adresse pour me renvoyer la page d'accueil.

On sait ce qu'il faut mettre dans un message IP. Comment l'envoyer à la bonne adresse ?

## Topologie Internet

Entre l'ordinateur et les serveurs de Wikipédia, il y a le routeur (la box wifi), le fournisseur d'accès Internet, une poignée de "switchs" et un autre routeur.

L'algorithme pour que le message arrive bien à destination est:

```
Etape 0
Est-ce que l'adresse cible est dans le tableau du routeur ?

Si non alors
    Diminuer le temps de vie
    Est-ce que le temps de vie est égal à 0 ?
    Si oui alors
        Abandonner le message (SORTIE)
    Envoyer le message au prochain qui est plus proche (Etape 0)
Si oui alors
    Envoyer le message à la connection physique correspondante
```

Par exemple: Un switch a un tableau avec des intervalles pour les adresses IP de 000 à 250: Envoyer au switch au sud, 201 à 999 envoyer au switch au nord. Ensuite ce switch la sait que pour les adresses IP 200 à 500 envoyer plus au nord, et 501 à 999 envoyer à l'est.

Le message est abandonné quand le temps de vie est 0, pour éviter les situation ou un message tourne en boucle, en case de mauvaise configuration du tableau des switch. A l'origine le temps de vie était mesuré en secondes, mais maintenant il est mesuré en "sauts" et a été renommé hops limit dans IPv6.

Une fois le message reçu, le serveur peut répondre à l'adresse source.

## Protocole de contrôle de transmission

Aussi connu sous l'acronyme TCP, ce protocole repose sur IP. Les messages TCP sont à l'intérieur du corps du message IP.

Utiliser le TCP ajoute les bénéfices suivants:

 - Les messages perdus sont renvoyés à nouveau (fiabilité)
 - Les messages sont renvoyés si des octets ont été changés par accident
 - Les messages sont dans le bon ordre

Les deux participants maintiennent en mémoire l'état de la connection, ils stockent le numéro du dernier message reçu. Et quand un message est reçu ils renvoient ce nombre en tant que confirmation. Si par exemple vous avez envoyé messages numéros 10 et 11 et que vous recevez la confirmation pour le 8, cela veut dire que le 9 n'a jamais été reçu. Donc il est renvoyé encore.

Vu que les messages sont numérotés, il peuvent être remis dans le bon ordre. Le protocole IP ne garantit pas que le premier message envoyé est le premier message reçu.

Les messages TCP contiennent une somme de contrôle, qui peut être calculé par le recevant. Si cette somme ne correspond pas à la somme calculé, alors ça veut dire que des octets ont été changés. Le message n'est pas confirmé, et après un certain temps le message va être renvoyé.

TCP gère aussi la bande passante, l'émetteur va envoyer plus de messages par seconde si le receveur en perd 0, et moins de messages par secondes si trop de messages doivent être renvoyés car ils étaient abandonné car la bande passante était insuffisante


## Protocole de transfert de l'hyper-texte

Aussi connu en tant que HTTP, c'est un protocolé basé sur TCP. TCP nous a donné les messages fiables et dans le bon ordre. Mais TCP n'explique pas ce qu'est le message final. Est-ce que le message, après assemblage est une image, une vidéo, du texte ? Et que veut-on en réponse ? On veut une réponse audio ou texte ? Et d'ailleurs, pourquoi on a envoyé ce message ?

Les messages HTTP ajoutent les fonctionnalités suivantes:

 - Intention
 - Description du contenu
 - Négociation du contenu
 - Mise en cache
 - Message d'erreur
 - et plus encore

Par exemple en ouvrant la page d'accueil de Wikipédia depuis un navigateurs, les en-têtes HTTP suivant (parmi d'autres) sont envoyés:

 - GET / 
 - Accept: text/html
 - Accept-Language: lb,pt;q=0.8
 - Accept-Encoding: gzip, deflate, br

Ce qui, pour un ordinateur qui parle le HTTP veut dire, Hé ! Je veux __avoir__ (GET) __la page d'accueil__ (/ est la page d'accueil) et je voudrais que la réponse soit du __texte__ , plus précisément __text/html__, et si possible en __luxembourgeois__ (lb) si tu l'as, sinon en __portugais__ (pt) si tu l'as. Ah ! Et je peux décoder du __gzip__ et plus encore.

Et le serveur Wikipédia me réponds ce que je veux en HTTPP 

 - 200 OK
 - Content-Language: lb
 - Content-Type: text/html; charset=UTF-8
 - Content-Encoding: gzip
 - CONTENU TEXTUEL APRES LES EN-TETES

Et si on voulait de l'audio plutôt ? On aurais pu envoyer 

 - Accept: audio/mpeg

Et le serveur aurait pu répondre (S'il ne gère pas l'audio)

 - 406 Non acceptable

Ce 406 est un code de statut HTTP, les codes dans les 200 indiquent que tout va bien, et ceux dans les 400 sont des erreurs. Quelques exemples:

 - 404 Pas trouvé
 - 429 Trop de requêtes
 - 409 Conflit !

A voir la [liste des codes HTTP](https://fr.wikipedia.org/wiki/Liste_des_codes_HTTP)

La méthode indique le __pourquoi__ de la requête, en voici 3 :

 - GET vu ci-dessus indique que l'on veut voir quelque chose
 - POST veut dire qu'on veuille ajouter quelque chose, par exemple après avoir écrit un commentaire on fait une requête POST
 - DELETE veut dire qu'on veut effacer quelque chose

## Internet ou Web ?

Internet c'est l'infrastructure physique, y compris les routeurs les switchs, les serveurs etc. L'Internet existait avant le Web les gens utilisaient des protocoles plus primitifs comme le TCP pour communiquer.

Le Web est un ensemble de technologie, principalement HTTP, HTML, CSS, JS qui utilisent Internet.

Email aussi utilise Internet mais ne fait pas partie du Web, voir les emails dans un navigateur Web est une fonctionnalité optionnelle non requise pour le protocole email.


## Adresse IP ou URL ?

Prenez l'URL https://lb.wikipedia.org/wiki/Hypertext_Transfer_Protocol il est possible de trouver le nom de domaine : lb.wikipedia.org. Le système de nom de domaine (DNS) permet ensuite d'obtenir l'adresse IP à partir du nom de domaine.



