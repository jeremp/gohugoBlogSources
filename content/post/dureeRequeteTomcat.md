+++
date = "2015-10-03T14:02:33+02:00"
title = "Tracer la durée de traitement des requêtes avec Tomcat"
draft = true
description = "comment produire son propre formulaire d'authentification avec le SSO OpenAM"
tags = [ "log", "tomcat" ]
categories = [
  "Development"
]
+++

Ça y est votre API REST est en place, vous avez rédigé une belle documentation et même mis en place un client swagger pour aider vos clients à l'utiliser.

Puis arrive le temps des premiers retours d'utilisation de vos clients, certains trouvent que votre API met trop de temps à répondre sur telle ou telle requête.

En général c'est à ce moment là qu'on commence à mettre en place un système pour logguer la durée de traitement de vos requêtes. En général vous utiliserez la notion de Filter ou d'Interceptor (si vous utilisez Spring MVC) ou tenterez d'utiliser des outils plus avancés tels que JavaMelody (il faut vraiment que j'essaye ce truc d'ailleurs).

En fait il y a beaucoup plus simple, il suffit de demander cela non pas à votre application mais à votre conteneur de servlets. Il existe déjà dans les fichiers de log de Tomcat les localhost_acces_XXX.log qui tracent chaque requête (la méthode HTTP, l'URL demandée, le code retour etc...).

Le format de ces logs se configure dans le server.xml, par défaut la "Valve" utilisée est AccessLogValve qui permet de tracer tout un tas d'informations mais pas le temps que Tomcat a pris pour fournir une réponse à la requête, pour cela il vous faut utiliser l'Extended Access Log Valve et faire apparaître dans le pattern de log le "time-taken".

Ci dessous un exemple de server.xml
