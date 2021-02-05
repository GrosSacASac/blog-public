# Comment l'Internet fonctionne

Tu t'es déjà demandé comment fonctionne l'Internet ? Voici une explication simplifié.

## Le texte n'est qu'une suite de nombres

Chaque lettre a un nombe associé. Bonjour l' [ASCII](https://fr.wikipedia.org/wiki/American_Standard_Code_for_Information_Interchange), où un éspace est 32 et A majuscule 65. Majuscule B est 66, C 67 etc.

Pourquoi A vaut 65 et pas 88 par exemple ? Les créateurs d'ASCII en ont choisi ainsi, tout simplement. ASCII est en fait une grille d'associations. Tu peux en créer une. Personne va l'utiliser, mais tu peux. La raison d'utiliser ASCII est que tout le monde l'utilise. Ainsi le nombre 65 est transformé en lettre A sur ton ordinateur et le mien  mine. Si on ne se met pas d'accord quelle nombre resprésente quelle lettre, on ne va pas pouvoir s'échanger du texte. 

ASCII LOVE c'est (76, 79, 86, 69).

Il y a d'autres encodages. utf-8, latin-1 etc qui peuvent gérer les accents et autres alphabets.

Donc le texte n'est qu'une suite de nombres.
Tu sais problament que les ordinateurs utilisent les nombres binaires en interne. Un ordinateur ne peut pas directement comprendre A (65).


### Les nombres sont binaires

La plupart des humains utilisent la base 10 pour leur systême numérique, cela vient du fait qu'on a 10 doigts, et c'est facile de compter jusqu'à dix. Base 10, ça veut dire qu'il y a 10 symboles différents 0-6, en incluant le 0. Le binaire est un systême numérique de base 2. Il y a 2 symboles: 0 et 1. Chaque décimal (base 10) peut être convértit en sa représentation binaire (base 2) Par exemple
 
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

On peut convertir une lettre en nombre  avec une grille d'associations. On peut convertire un nombre décimal en binaire. On peut donc aller de lettre à binaire. 1000001 est binaire pour la lettre A.

Un bit est un 0 ou 1 singulier.

Un octet est composé de 8 bits.

Mais comment envoyer un nombre ? C'est une abstraction. Ca n'existe pas dans le monde physique. Comment les ordinateurs s'échangent des nombres ?

## Le binaire c'est du voltage

Bonjour le monde physique, où l'on utilise l'éléctricité pour envoyer des nombres. Par exemple, un voltage élevé est 1 et un voltage bas est 0. Donc comment on envoie la lettre A (1000001) par exemple ?

 - 1 Haut
 - 0 Bas
 - 0 Bas
 - 0 Bas
 - 0 Bas
 - 0 Bas
 - 1 Haut

Pendant comien de temps doit'on garder le courant éléctrique à haut voltage ? La durée est la période, qui doit être déterminé. Exemple: 1 seconde. Il faudrait 7 secondes pour envoyer la lettre A. Trop long. Rien que cette phrase prendrait 40 fois 7 secondes au moins. Une période de 1ms serait 1000 fois plus rapide. Mais il y a un minimum pour la période. Plus la période est courte, plus il faut du matériel précis.

Que doit faire l'ordinateur quand rien n'a encore été envoyé ? Le voltage sera bas et l'ordinateur pourrait croire qu'il recoit des zéros. C'est pourquoi il y a une phase de synchronisation: Comme 0101010101 suivi par 25 uns et ensuite le vrai message commence. Sans cela (111) pourrait être interprété comme (011, 100) décalage de 1.

Et, on envoie où les bits ?


## Internet Protocol

Like in the real world, if you want to send a message you need to know to whom. And once you know that you need to have his address.

The Internet protocol defines addresses as a series of numbers. Every device connected to the Internet has an IP address. An IPv4 address looks like this 180.160.299.88.

IP messages have a maximum size, so a big image file or video, needs to be split in multiple messages.

### IPv4

Have you noticed something ? The address only has 11 digits. But there are more than 99999999999 devices connected to the Internet, which means we ran out of addresses ! The designers of the IP never imagined how big the Internet would become.

### IPv6

IP 6 addresses fixes the issues of IPv4 with orders of magnitude more addresses.

### Send protocol

In addition to defining what an address is, IP also defines how to send a message. An IP message should contain

 - target address
 - source address
 - message
 - time to live (ttl)
 - total length
 - and a few others

Source address is required so that the target can know where to send the response. If I open https://en.wikipedia.org the wikipedia computers need to know my address in order to send me the homepage back.

We now know what to put inside an IP message. How do we send it to correct address ?

## Internet topology

In between the device and the servers of wikipedia, there is the router (aka wifi box) the Internet service provider, and a handfull of switches, and another router.

The algorithm to make sure the message arrives at destination is as follows:

```
Step 0
Is the target address in the routing table ?

If not then
    Decrease the time to live
    Is time to live equal to 0 ?
    If yes then
        Drop the message (EXIT)
    Send it to the next one who is closer (Go to Step 0)
If yes then
    Send it to the matching physical connection
```

For example: A switch has range tables that for ips that start with 000 to 200: send to the next switch located south, 201 to 999 send it to the switch located north. Then that switch knows that for ips from 200 - 500 send it further north, and for 501 to 999 send it east.

The reason the message is dropped when time to live is 0, is to prevent situation where a message moves around in circles forever, in case of routing table missconfiguration. Historically time to live was measured in seconds, nut now it is measured in "hops", it was renamed to hop limit with IPv6.

Once a message is received, The server can send a response back to the source address.

## Transmission Control Protocol

Also know as TCP for short, this protocol is built on top of IP. TCP messages are inside the IP message body.

Using TCP brings the following benefits:

 - Messages are resent if lost (reliable)
 - Messages are resent if some bytes were changed accidentally changed
 - Messages are ordered

The way it works is that the both ends of the connection maintain a connection state. Both computer store the information about the number of the last message that is received. And when a message is received they send that number back as an acknowledgement. If for example you have sent message number 10 and 11 and receive acknowledgements about 8, it means that 9 was never received. So it is sent again.

Since message are numbered, they can be ordered. Remember that the IP protocol does not guarantee that first message sent is first message received.

TCP messages contain a checksum, which can be calculated by receiving party. If the checksum in the message does not match the calculated checksum, then it means that some bytes were changed. The message is not acknowledged and eventually the message will be resent

TCP aslo handles transmission speed, the sender will send more messages per second if the receiver drops 0 messages, and less messages per second if too many messages need to be resent because they were dropped because the bandwidth was insufficient.

## Hypertext Transfer Protocol

Hypertext Transfer Protocol also known as HTTP, is a protocol built on top of TCP. TCP gave as reliable and ordered messages. But TCP does not explain what the final message is. Is the message after assembly, an image, a video, text ? And what do we want as an answer ? Do we want a text answer ? Or an audio file as an answer ? And why did we send the message in the first place ?

HTTP messages adds the following features:

 - Intent
 - Content description
 - Content negotiation
 - Caching
 - Error messages
 - and more

For example when opening the wikipedia homepage from a browser, the following HTTP headers (amongst others) are sent:

 - GET / 
 - Accept: text/html
 - Accept-Language: lb,pt;q=0.8
 - Accept-Encoding: gzip, deflate, br

Which, for a computer that speaks HTTP means, Hey ! I want to __get__ the __homepage__ ( / is the homepage) and I would like the answer to be __text__, more specifically __text/html__ , and if possible in __Luxemburgish__ (lb) if you have it, othewise in __Portuguese__ (pt) if you have it. Oh and I can decode __gzip__ and more.


And the wikipedia server sends me back what I want in HTTP 

 - 200 OK
 - Content-Language: lb
 - Content-Type: text/html; charset=UTF-8
 - Content-Encoding: gzip
 - TEXT CONTENT AFTER THE HEADERS

If we wanted audio instead ? We could have sent

 - Accept: audio/mpeg

And the server could have responded (if it does not handle audio)

 - 406 Not Acceptable

That 406 is a HTTP status code, those in the 200s inidicate that everything is Ok, and those in the 400s are usually errors. Some examples:

 - 404 Not Found
 - 429 Too Many Requests
 - 409 Conflict

See [list of HTTP status codes](https://en.wikipedia.org/wiki/List_of_HTTP_status_codes)

The method indicates __why__ we are making the request, 3 of them :

 - GET as seen above means we want to view something
 - POST means we want to add something. For example after writing a comment we make a POST request
 - DELETE means we want to delete something


## Internet or Web ?

Internet is the physical infrastructure, including routers, switches, servers etc. The Internet existed before the Web and people used more primitive protocols like TCP to communicate.

The Web is a set of technology, mainly HTTP, HTML, CSS, JS that uses the Internet.

Email also uses the Internet but is not part of the Web, viewing emails in a Web browser is an optional feature, but is not required for the email protocol.


## IP address or URL ?

Given an URL https://lb.wikipedia.org/wiki/Hypertext_Transfer_Protocol it is possible to find the domain name lb.wikipedia.org . The Domain name system (DNS) allows us to get the IP address from a domain name.



