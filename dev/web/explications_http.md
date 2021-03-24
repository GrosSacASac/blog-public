# Explications sur quelques en têtes HTTP

## Strict-Transport-Security

Historiquement parlant quand on est écrit example.com dans la barre d'adresse le navigateur tente d'abord une connection http, puis l'augmente en https. Avec cette en-tête vous indiquez que vous voulez que votre site soit directement accédé en https la prochaine fois. Moins important de nos jours car les navigateurs evergreen (du moins Firefox et Chrome) vont ^étre en https

Exemple Nginx

```nginx
add_header Strict-Transport-Security "max-age=31536000; includeSubdomains; preload";
```
