# How does the Internet work

Ever wondered how the Internet works ? Here's a simplified explanation.

## Text is a series of numbers

Every letter has an associated number. Discover the [ASCII](https://en.Wikipedia.org/wiki/ASCII) encoding, where a space is 32 and capital A is 65. Capital B is 66, C 67 and so forth.

Why is A 65 and not 88 for example ? The people who created ASCII chose it that way, that's it. ASCII is in fact an association table. You can create your own. No one will use it, but you can. The reason to use ASCII is that everyone else is using it. So the number 65 will be transformed into the letter A on your computer and on mine. If we don't agree which number represents what letter, we will not be able to send each other text. 

For example __LOVE__ in ASCII is (76, 79, 86, 69).

There are other encodings. utf-8, latin-1 etc that can handle accents and other set of letters.

So text is just a series of numbers.
You probably know that computers internally use binary numbers. A computer cannot directly understand A (65).


### Numbers are binary

Most humans use base 10 for their numerical system, it comes from the fact that we have 10 fingers and makes counting easy. Base 10 means there are 10 different symbols 0-9, 0 included. Binary is a number system with base 2. There are 2 different symbols: 0 and 1. Every decimal (base 10) number can be converted to a binary (base 2) number representation. For example 
 
 * decimal 1024 is binary 10000000000
 * decimal 1025 is binary 10000000001
 * decimal 1026 is binary 10000000010
 * decimal 1027 is binary 10000000011

To understand how to write a number in binary, let us consider an alternative way to write 1024 in decimal. (x power of 0 is 1.)

 * `1027 = (1000) + (0) + (20) + (7)`
 * `1027 = (1 * 10**3) + (0 * 10**2) + (2 * 10**1) + (7 * 10**0)`
 * `1027 = (1 * 10**3) + (2 * 10**1) + (7 * 10**0)`

Any number in decimal can be written as a sum like this. Now using the same method, but we replace base 10 with base 2

 * `111 = (100) + (10) + (1)`
 * `111 = (1 * 2**2) + (1 * 2**1) + (1 * 2**0)`
 * `111 = (1 * 4) + (1 * 2) + (1 * 1)`
 * `111 = (4) + (2) + ( 1)`
 * `111 = 7`
 

Another example (1027):
 
 * `10000000011 = (10000000000) + (0) + (0) + (0) + (0) + (0) + (0) + (0) + (0) + (10) + (1)`
 * `10000000011 = (1 * 2**10) + (0 * 2**9) + (0 * 2**8) + (0 * 2**7) + (0 * 2**6) + (0 * 2**5) + (0 * 2**4) + (0 * 2**3) + (0 * 2**2) + (1 * 2**1) + (1 * 2**0)`
 * `10000000011 = (1 * 2**10) + (1 * 2**1) + (1 * 2**0)`
 * `10000000011 = (1024) + (2) + (1)`

We can convert a letter into a number with an association table. We can convert a decimal number into binary number. Which means we can go from letter to binary. 1000001 is Binary for letter A.

A bit is a single 0 or 1.

A byte is 8 bits.

But how to send a number ? It is an abstraction. They do not exist in the physical world. How do computers send each other numbers ?


## Binary are voltages

Enter the physical world, where we use electricity to send numbers. For example high voltage is 1 and low voltage is 0. So how do we send the letter A (1000001) for example ?

 - 1 High
 - 0 Low
 - 0 Low
 - 0 Low
 - 0 Low
 - 0 Low
 - 1 High

How long should we keep the electric current on high voltage ? This duration is the period. It needs to be agreed upon. Example: 1 second.
It would take 7 seconds to send the letter A. That would be a long time. This sentence alone would take 40 * 7 seconds at least. A period of 1ms would be 1000 times faster. But there is a limit on how low the period can be. The smaller it is, the more precise the hardware has to be.

What should the computer do when there is nothing sent ? The voltage will be low and the computer could think it receives 0s. That is why there is a synchronization phase: Like 0101010101 followed by 1111111111111111111111111 and then the real message begins. Without this (111) could be interpreted as (011, 100) shift by 1.

Where do we sent the bits ?


## Internet Protocol

Like in the real world, if you want to send a message you need to know to whom. And once you know that you need to have his address.

The Internet protocol defines addresses as a series of numbers. Every device connected to the Internet has an IP address. An IPv4 address looks like this 180.160.299.88.

IP messages have a maximum size, so a high definition image or video, need to be split in multiple messages.

### IPv4

Have you noticed something ? The address only has 11 digits. But there are more than 99999999999 devices connected to the Internet, which means we ran out of addresses ! The designers of the IP never imagined how big the Internet would become.

### IPv6

IP 6 addresses fixes the issues of IPv4 with orders of magnitude more addresses.

### Send protocol

An IP message must contain

 - target address
 - source address
 - message
 - time to live (ttl)
 - total length

Source address is required so that the target can know where to send the response. If I open https://en.wikipedia.org the Wikipedia computers need to know my address in order to send me the homepage back.

We now know what to put inside an IP message. How do we send it to correct address ?

## Internet topology

In between the device and the servers of Wikipedia, there is the router (aka WIFI box) the Internet service provider, and a handful of switches, and another router.

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

For example: A switch has range tables that for IP addresses that start with 000 to 200: send to the next switch located south, 201 to 999 send it to the switch located north. Then that switch knows that for IP addresses from 200 - 500 send it further north, and for 501 to 999 send it east.

The reason the message is dropped when time to live is 0, is to prevent situation where a message moves around in circles forever, in case of routing table misconfiguration. Historically time to live was measured in seconds, but now it is measured in "hops", it was renamed to hop limit in IPv6.

Once a message is received, the server can send a response back to the source address.

## Transmission Control Protocol

Also know as TCP for short, this protocol is built on top of IP. TCP messages are inside the IP message body.

Using TCP brings the following benefits:

 - Messages are resent if lost (reliable)
 - Messages are resent if some bytes were changed accidentally changed
 - Messages are ordered

The way it works is that the both ends of the connection maintain a connection state. Both computer store the information about the number of the last message that is received. And when a message is received they send that number back as an acknowledgement. If for example you have sent message number 10 and 11 and receive acknowledgements about 8, it means that 9 was never received. So it is sent again.

Since messages are numbered, they can be ordered. The IP protocol does not guarantee that first message sent is first message received.

TCP messages contain a checksum, which can be calculated by receiving party. If the checksum in the message does not match the calculated checksum, then it means that some bytes were changed. The message is not acknowledged and eventually the message will be resent

TCP also handles transmission speed, the sender will send more messages per second if the receiver drops 0 messages, and less messages per second if too many messages need to be resent because they were dropped because the bandwidth was insufficient.

## Hypertext Transfer Protocol

Hypertext Transfer Protocol also known as HTTP, is a protocol built on top of TCP. TCP gave as reliable and ordered messages. But TCP does not explain what the final message is. Is the message after assembly, an image, a video, text ? And what do we want as an answer ? Do we want a text answer ? Or an audio file as an answer ? And why did we send the message in the first place ?

HTTP messages adds the following features:

 - Intent
 - Content description
 - Content negotiation
 - Caching
 - Error messages
 - and more

For example when opening the Wikipedia homepage from a browser, the following HTTP headers (amongst others) are sent:

 - GET / 
 - Accept: text/html
 - Accept-Language: lb,pt;q=0.8
 - Accept-Encoding: gzip, deflate, br

Which, for a computer that speaks HTTP means, Hey ! I want to __get__ the __homepage__ ( / is the homepage) and I would like the answer to be __text__, more specifically __text/html__ , and if possible in __Luxemburgish__ (lb) if you have it, otherwise in __Portuguese__ (pt) if you have it. Oh and I can decode __gzip__ and more.


And the Wikipedia server sends me back what I want in HTTP 

 - 200 OK
 - Content-Language: lb
 - Content-Type: text/html; charset=UTF-8
 - Content-Encoding: gzip
 - TEXT CONTENT AFTER THE HEADERS

If we wanted audio instead ? We could have sent

 - Accept: audio/mpeg

And the server could have responded (if it does not handle audio)

 - 406 Not Acceptable

That 406 is a HTTP status code, those in the 200s indicate that everything is Ok, and those in the 400s are usually errors. Some examples:

 - 404 Not Found
 - 429 Too Many Requests
 - 409 Conflict

See [list of HTTP status codes](https://en.wikipedia.org/wiki/List_of_HTTP_status_codes)

The method indicates __why__ we are making the request, 3 of them :

 - GET as seen above means we want to view something
 - POST means we want to add something, for example after writing a comment we make a POST request
 - DELETE means we want to delete something


## Internet or Web ?

Internet is the physical infrastructure, including routers, switches, servers etc. The Internet existed before the Web and people used more primitive protocols like TCP to communicate.

The Web is a set of technology, mainly HTTP, HTML, CSS, JS that uses the Internet.

Email also uses the Internet but is not part of the Web, viewing emails in a Web browser is an optional feature, but is not required for the email protocol.


## IP address or URL ?

Given an URL https://lb.wikipedia.org/wiki/Hypertext_Transfer_Protocol it is possible to find the domain name lb.wikipedia.org . The Domain name system (DNS) allows us to get the IP address from a domain name.



