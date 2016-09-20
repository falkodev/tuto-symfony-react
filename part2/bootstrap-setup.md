## Installation de Twitter Bootstrap

Twitter Bootstrap, bien que discutable pour certains, reste bien pratique pour démarrer une appli web en mode responsive.

J'ai donc choisi de l'utiliser sur ce tutoriel, l'inconvénient majeur étant qu'il utilise jQuery (et jQuery, c'est le mal, surtout avec du code JS moderne).

Pour parer cet inconvénient, on ne va pas installer Bootstrap depuis les dépôts officiels.

Bootstrap-sass est un paquet NPM qui étend et compile Twitter Bootstrap en SASS. Seule la partie SASS est utilisée (donc pas de JS depuis Bootstrap). On l'installe en ligne de commande comme ceci :

`npm i bootstrap-sass --save-dev`

Puis on l'utilise en ajoutant ces 2 lignes au début du fichier app/Resources/scss/style.scss :

```css
$icon-font-path: "~bootstrap-sass/assets/fonts/bootstrap/";
@import "~bootstrap-sass/assets/stylesheets/bootstrap";
```

React-bootstrap est un autre paquet NPM bien utile. Il remplace le JS (et donc jQuery) dans Bootstrap. Il a seulement besoin du CSS de Bootstrap (généré par le SASS du paquet précédemment cité). On a donc 2 packages complémentaires. Pour l'installation de celui-ci : 

`npm i react-bootstrap --save`


