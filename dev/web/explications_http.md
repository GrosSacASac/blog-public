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

Pour les requêtes simples il suffit d'ajouter un en-tête HTTP dans la réponse. Les requêtes simples sont les GET, HEAD, POST avec aucun en-tête HTTP custom et si POST alors le content type peut être seulement `application/x-www-form-urlencoded` ou `multipart/form-data` ou `text/plain` (ce que `<form>` envoie par défaut)

Par exemple si vous voulez que le coté client de example.com puisse faire des requêtes à votre serveur alors ajoutez

* `Access-Control-Allow-Origin: https://example.com`


danse la réponse à ces requêtes.

Par exemple en Node.js (si vous testez localhost:3000): 

```js
response.setHeader(`Access-Control-Allow-Origin`, `http://localhost:3000`);
```


Pour les requêtes non simples, le navigateur fera automatiquement une requête OPTION en PREFLIGHT sur la même URL que la resource demandé.  Et il faut répondre aux requêtes `OPTIONS` avec ces en-têtes sur la même URL que l' URL que vous autorisez.

Il faudra alors répondre à cette requête qui utilise une méthode HTTP OPTION avec 

* `Access-Control-Allow-Origin: https://example.com`
* `Access-Control-Allow-Methods: POST, GET, DELETE, PUT`
* `Access-Control-Allow-Headers: x-custom`

Ne mettez que les méthodes effectivement attendus dans Access-Control-Allow-Methods, Access-Control-Allow-Headers est optionnel et vous permet d'autoriser plus.

## Content-Security-Policy

est en quelque sorte l'inverse de Access-Control-Allow-Origin. Content-Security-Policy vous permet d'indiquer pour chaque type de ressource (image, script, etc.), de quelle origine une requête peut être faite. Par défaut, tout est autorisé.

`Content-Security-Policy "default-src 'self' 'unsafe-inline' 'unsafe-eval'" always;` bloque toutes les requêtes vers un autre nom de domaine pour tout type de resource.

Par example si votre site utilise des images hébergés sur imgur alors il faut rajouter une exception pour imgur.com

C'est en faite un mechanise de défense en profondeur. Si quelqu'un réussi à faire une attaque XSS sur votre site, les données volés ne peuvent pas être envoyé sur un serveur qui n'est pas listé dans Content-Security-Policy. Les requêtes seront bloqués.

## X-Frame-Options

Cette en-tête vous permet de déclarer comment votre site peut être mis dans un `<iframe>`. Pour bloquer cela utilisez:

`X-Frame-Options "DENY"`
