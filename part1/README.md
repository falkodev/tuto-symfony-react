# Tutoriel Symfony / React / Webpack / Babel {#tutoriel-symfony-react-webpack-babel}

Dans ce tutoriel, nous allons voir comment mettre en place un éco-système moderne et performant avec :

* Symfony, le framework PHP MVC de référence,
* React.js, la solution front dominante,
* Webpack, l'empaqueteur et gestionnaire de ressources qui se marie bien avec React,
* Babel, pour écrire de l'ES6, ce qui est plus pratique et sexy que de l'ES5 quand même.

Les sources finales sont disponibles à cette adresse : https://github.com/falkodev/tuto-symfony-react-webpack-babel



Nous allons partir du postulat que vous êtes développeur et donc pas complètement débutant sur :

* Symfony, son intérêt et son fonctionnement, sans forcément être expert,
* React \(le V de MVC comme clame Facebook, son promoteur\) et l'ES6,
* la ligne de commande et les paquets NPM.

Le but de cet article est plus de montrer comment avoir une stack complète et faire travailler ensemble ces outils plutôt que d'enseigner un langage ou un framework.

Si vous avez le choix, autant partir sur un environnement full JS avec Node en back et une solution front \(React, Angular2, ...\), et faites-vous plaisir sur la base de données \(MongoDB, Neo4J\). Meteor permet d'ailleurs de combiner les deux, ce sera le sujet d'un autre article.

Mais si on \(votre client, votre entreprise ou votre budget\) vous impose un hébergement mutualisé, avec PHP et MySQL comme souvent, le mieux reste de partir avec un bon vieux framework comme Symfony 2 et l'ORM Doctrine. Côté front, on pourrait aussi se contenter de la solution classique de templating Twig. Mais pourquoi se priver de pages encore plus performantes, réactives et éviter des rechargements de page si, en plus, on a un code modulaire comme le permet React ?

