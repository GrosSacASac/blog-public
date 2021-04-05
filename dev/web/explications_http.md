# Explications sur quelques en têtes HTTP

## Strict-Transport-Security

Historiquement parlant quand on est écrit example.com dans la barre d'adresse le navigateur tente d'abord une connection HTTP, puis l'augmente en HTTPS. Avec cette en-tête vous indiquez que vous voulez que votre site soit directement accédé en HTTPS la prochaine fois. Moins important de nos jours car les navigateurs evergreen (du moins Firefox et Chrome) vont être en HTTPS dés la première connexion.

Exemple Nginx

```nginx
add_header Strict-Transport-Security "max-age=31536000; includeSubdomains; preload";
```
## Access-Control-Allow-Origin

Utilisé pour permettre à d'autres sites de faire de requêtes CORS (Cross origin resource sharing) sur le vôtre. Pour comprendre comment ça marche il faut déjà distinguer votre serveur, un autre serveur avec un autre nom de domaine et la page rendu de cet autre serveur.

Si la page rendu d'un autre serveur essaye de faire une requête (coté client donc) sur votre serveur, il y aura une erreur sauf si votre serveur autorise cette requête via les bons en-têtes HTTP.

Sauf pour les requêtes simples qui elles sont autorisés par défaut. Les requêtes simples sont les GET, HEAD, POST avec aucun en-tête HTTP custom et si POST alors le content type peut être seulement `application/x-www-form-urlencoded` ou `multipart/form-data` ou `text/plain` (ce que `<form>` envoie par défaut)

Par exemple si vous voulez que le coté client de example.com puisse faire des requêtes à votre serveur alors ajoutez 

 * `Access-Control-Allow-Origin: https://example.com`
 * `Access-Control-Allow-Methods: POST, GET, DELETE, PUT`
 * `Access-Control-Allow-Headers: Content-Type, Range`

Access-Control-Allow-Methods et Access-Control-Allow-Headers étant optionnels et vous permet d'autoriser plus. Et il faut répondre aux requêtes `OPTIONS` avec ces en-têtes sur la même URL que les URL que vous autorisez.

## Content-Security-Policy

est en quelque sorte l'inverse de Access-Control-Allow-Origin. Content-Security-Policy vous permet d'indiquer pour chaque type de ressource, de quelle origine la requêtes peut être faite. Par défaut tout est autorisé.

`Content-Security-Policy "default-src 'self' 'unsafe-inline' 'unsafe-eval'" always;` bloque toutes les requêtes vers un autre nom de domaine pour tout type de resource.

C'est en faite un mechanise de défense en profondeur. Si quelqu'un réussi à faire une attaque XSS sur votre site, les données volés ne peuvent pas être envoyé sur un serveur qui n'est pas listé dans Content-Security-Policy. Les requêtes seront bloqués.

## X-Frame-Options

Cette en-tête vous permet de déclarer comment votre site peut être mis dans un `<iframe>`. Pour bloquer cela utilisez:

`X-Frame-Options "DENY"`