+++
date = "2015-10-03T13:32:06+02:00"
title = "Personnaliser la page de login d’OpenAM"
description = "comment produire son propre formulaire d'authentification avec le SSO OpenAM"
draft = false
tags = [ "sso", "openam" ]
categories = [
  "Development"
]
slug = "personnaliser-la-page-de-login-dopenam"
+++

![Logo d'OpenAm](../../img/openam_logo.png "Logo d'OpenAm")


[OpenAm](https://forgerock.org/openam/) est un véritable couteau suisse du SSO, à première vue son côté “usine à gaz” peut en rebuter certains (qui lui préféreront sans doute CAS) mais je vous invite à effectuer le tutoriel [Getting Started With OpenAM](http://docs.forgerock.org/en/openam/12.0.0/getting-started/index/index.html#getting-started) pour vous faire un avis. Le système extensible d’authentication module d’OpenAM vous permettra d’adapter la mécanique d’authentification si le mode username + password n’est pas suffisant. 
Le seul point de difficulté que j’ai personnellement rencontré est la personnalisation de la page de login. Depuis la version 12.0.0, OpenAM propose un système appelé XUI (basé sur des templates javascript et le framework CSS “Less”) qui me paraît plutôt compliqué à prendre en main. Concrètement, si votre projet nécessite un mode d’authentification “exotique” comme par exemple :

* un premier écran avec un nom d’utilisateur et un password
* un second écran avec un second password ou code à saisir

Le système d’authentication module vous permettra de décrire et d’implémenter cette logique d’authentification, en revanche vous serez assez limité en ce qui concerne la personnalisation de la partie présentation de vos écrans d’identification sans parler des modes de saisies que vous auriez peut être envie de modifier comme par exemple les claviers virtuels (à la mode sur les sites bancaires) où l’on saisie son code à la souris. 

Heureusement OpenAM expose ses services d’authentification (et d’autres) au travers de [web services REST](https://backstage.forgerock.com/#!/docs/openam/12.0.0/dev-guide/chap-rest]. Il vous suffit dès lors de réaliser dans votre application les pages d’authentification (aussi complexes soient-elles),et d’appeler avec les données saisies par votre utilisateur le web service d’authentification. Ci dessous l’exemple avec la commande cUrl fourni dans la doc  :
```
curl \
 --request POST \
 --header "X-OpenAM-Username: demo" \
 --header "X-OpenAM-Password: changeit" \
 --header "Content-Type: application/json" \
 --data "{}" \
 https://openam.example.com:8443/openam/json/authenticate
{ "tokenId": "AQIC5w...NTcy*", "successUrl": "/openam/console" }
```

Si les identifiants sont valides, le service retourne le fameux jeton d’identification qu’il vous suffit d’attacher en tant que cookie à la session de votre utilisateur. Ceci nécessitera bien sûr que votre instance d’OpenAM tourne sur le même domaine que vos applicatifs (si ce n’est pas le cas sur l’environnement de développement, pensez à faire une correspondance dans votre fichier Host de manière à ce que cela soit le cas) : Si votre OpenAM tourne sur *openam.mycompany.com* il suffit de modifier votre hosts :
```
127.0.0.1    localhost mypc.mycompany.com
```
Ainsi votre application locale tournera sur le même domaine (*mycompany.com*) que votre SSO et le partage du cookie sera possible.



