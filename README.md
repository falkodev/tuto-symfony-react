# Tutoriel Symfony \/ React \/ Webpack \/ Babel {#tutoriel-symfony-react-webpack-babel}

Dans ce tutoriel, nous allons voir comment mettre en place un éco-système moderne et performant. Nous allons partir du postulat que vous êtes développeur et donc pas complètement débutant sur :

* Symfony, son intérêt et son fonctionnement, sans forcément être expert,
* React \(le M de MVC comme clame Facebook, son promoteur\) et l'ES6,
* la ligne de commande et les paquets NPM.

Le but de cet article est plus de montrer comment avoir une stack complète et faire travailler ensemble ces outils plutôt que d'enseigner un langage ou un framework.

Si vous avez le choix, autant partir sur un environnement full JS avec Node en back et une solution front \(React, Angular2, ...\), et faites-vous plaisir sur la base de données \(MongoDB, Neo4J\). Meteor permet d'ailleurs de combiner les deux, ce sera le sujet d'un autre article.

Mais si on \(votre client, votre entreprise ou votre budget\) vous impose un hébergement mutualisé, avec PHP et MySQL comme souvent, le mieux reste de partir avec un bon vieux framework comme Symfony 2 et l'ORM Doctrine. Côté front, on pourrait aussi se contenter de la solution classique de templating Twig. Mais pourquoi se priver de pages encore plus performantes, réactives et éviter des rechargements de page si, en plus, on a un code modulaire comme le permet React ?

**Les outils**

Les frameworks JS modernes poussent à du code modulaire, et Webpack se marie bien à React. C'est un "bundler" ou un empaqueteur en bon français, ce qui va permettre de prendre tous les modules et leurs dépendances et d'en faire un fichier de ressource statique, un gros fichier js en somme.

Comme on est parti pour une appli web moderne, autant coder en ES6. Il faut donc utiliser un transpileur tel que Babel.

Pour démarrer facilement ce projet, nous allons télécharger Symfony et quelques paquets NPM. Bien entendu, Node et NPM doivent être installés sur votre machine de développement.

**Installation**

On commence par télécharger [l'installateur Symfony tel qu'expliqué ici](http://symfony.com/doc/current/book/installation.html) :

```
--- Linux/Mac ---
sudo curl -LsS https://symfony.com/installer -o /usr/local/bin/symfony
sudo chmod a+x /usr/local/bin/symfony
```

```
--- Windows ---
c:\> php -r "readfile('https://symfony.com/installer');" > symfony
c:\> move symfony c:\projects
c:\projects\> php symfony
```

Puis on installe l'application Symfony.

```
--- Linux/Mac --- 
symfony new nouveau_projet lts
```

```
--- Windows --- 
c:\> cd projects/
c:\projects\> php symfony new nouveau_projet
```

Avec cette commande, ce sera la version LTS \(long term support\) actuelle qui sera installée, ce qui garantit d'avoir un support durable. Mais on peut tout aussi bien choisir une version spécifique. Par exemple :

```
symfony new nouveau_projet 3.0.2
```

Pour ceux qui avaient l'habitude d'installer Symfony avec Composer, vous pouvez constater la rapidité accrue de téléchargement et d'installation.

On peut maintenant passer à l'étape suivante : l'installation des outils du front. J'ai nommé React, Babel et Webpack.

Dans le répertoire du projet, on initialise le fichier de configuration NPM qui servira à télécharger les packages nécessaires au projet :

```
npm init -f
```

Cela crée un fichier package.json quasiment vide. J'ai ajouté l'option "-f" pour ne pas avoir à répondre aux questions posées par NPM à l'initialisation.

On ajoute les packages du projet. Deux bonnes grosses lignes de commande pour ça :

```
npm i react react-dom --save
npm i react-bootstrap react-hot-loader babel-core babel-loader babel-preset-es2015 babel-preset-react bootstrap-sass css-loader extract-text-webpack-plugin file-loader node-sass sass-loader style-loader webpack webpack-dev-server --save-dev
```

Qu'est-ce que c'est que tous ces paquets ? De base, on aurait pu se contenter des packets babel et webpack mais les paquets concernant Sass et les loaders sont là pour intégrer Twitter Bootstrap que j'ai ajouté dans le projet.

En tout cas, ces commandes vont compléter le fichier package.json déjà créé. On va aussi ajouter un élément pour les scripts :

![](assets/package_json.png)

L'élément script est vide. Ajoutez le code tel que sur la capture d'écran ci-dessus :

```js
"scripts": {
 "start": "node webpack.dev-server.js",
 "build": "webpack"
 },
```

Puis, il faut un fichier de configuration webpack.config.js à la racine du projet avec ce contenu :

```js
var path = require('path');
var webpack = require('webpack');
var node_modules_dir = path.join(__dirname, 'node_modules');
var ExtractTextPlugin = require('extract-text-webpack-plugin');

var config = { 
   entry: [ 
       'webpack-dev-server/client?http://127.0.0.1:3000', 
       'webpack/hot/only-dev-server',
       './app/Resources/js/app.js', 
       './app/Resources/scss/style.scss' 
    ], 
    output: { 
        path: path.join(__dirname, 'web/dist'),
        filename: 'bundle.js', 
        publicPath: 'http://127.0.0.1:3000/static/'
    }, 
    plugins: [ 
        new webpack.HotModuleReplacementPlugin(), 
        new webpack.NoErrorsPlugin(), 
        new ExtractTextPlugin('style.css', { allChunks: true }),
    ],
    module: { 
        loaders: [ 
           { 
               test: /\.jsx?$/, 
               include: path.join(__dirname, 'app/Resources/js'), 
               exclude: node_modules_dir, 
               loaders: ['react-hot', 'babel?presets[]=es2015&presets[]=react'], 
               presets: ['es2015', 'react'] 
           },
           { 
               test: /\.scss$/, 
               loader: ExtractTextPlugin.extract('css!sass') 
           }, 
           { 
               test: /\.jpe?g$|\.gif$|\.png$|\.svg$|\.woff$|\.woff2?$|\.ttf$|\.eot$|\.svg$/, 
               loader: "file?name=[name].[ext]" 
           } 
        ] 
    }
};
module.exports = config;
```

Et enfin, un fichier webpack.dev-server.js :

```js
var webpack = require('webpack');
var WebpackDevServer = require('webpack-dev-server');
var config = require('./webpack.config');

new WebpackDevServer( 
    webpack(config), 
    {
        publicPath: config.output.publicPath, 
        hot: true, 
        quiet: false, 
        noInfo: false, 
        historyApiFallback: true     
    }
).listen( 
    3000, 
    '0.0.0.0', 
    function (err, result) { 
        if (err) { console.log(err); }
        console.log('Listening at 0.0.0.0:3000'); 
    }
);
```

Tout est prêt pour commencer à coder : un serveur Node webpack.dev-server \(basé sur Express en fait\) écoute les modifications sur le code, construit et sert le fichier bundle.js selon la configuration définie dans le fichier webpack.config.js.

Il reste donc à démarrer le serveur Symfony, en ligne de commande, à la racine du projet :

```
php app/console server:run
```

et la tâche \(dans un deuxième terminal\) qui sera utilisée pour le développement \(nous verrons plus tard pour la production\):

```
npm start
```

Cette commande lance, selon le fichier package.json, une commande Node qui démarre le serveur de développement de webpack. Aucun fichier n'est créé, il est en mémoire du serveur webpack et servi tel quel. Inutile donc de chercher le fichier bundle.js défini dans le "output" du fichier webpack.config.js. En revanche, on peut avoir un aperçu du fichier généré en se rendant à l'adresse http:\/\/127.0.0.1:3000\/static\/bundle.js. Ce fichier sera mis à jour à chaque changement dans nos fichiers js que nous créerons dans la sous-partie "L'application côté front".

Pour la production, il suffira de lancer la commande

```
npm build
```

qui lancera webpack en mode production, c'est-à-dire qui créera un fichier js minifié, utilisable sans avoir à démarrer le serveur webpack donc.

Pour l'instant, nous sommes en phase de développement, on se contente donc de "npm start". Vous devez obtenir le message suivant dans votre terminal :

```
webpack: bundle is now VALID.
```

Si c'est bien le cas, on va créer l'application React. Elle sera incorporée à l'application Symfony. On va donc commencer par mettre en place les conditions pour que React puisse s'épanouir dans l'environnement qui l'accueillera.

**L'application côté back**

Par défaut, dans le dossier app\/Resources\/views, nous trouvons le fichier base.html.twig. Etant donné que ce fichier sera celui utilisé par tous les autres templates, nous devons le personnaliser comme ceci :

```php
<!DOCTYPE html>
<html>
    <head>
        <meta charset="UTF-8" /> 
        <meta name="viewport" content="width=device-width, initial-scale=1"> 
        <title>{% block title %}Nom projet{% endblock %}</title> 
        {% block stylesheets %}{% endblock %} 
        {% if app.environment == 'dev' %} 
            <link rel="stylesheet" href="http://127.0.0.1:3000/static/style.css"></script> 
        {% else %} 
            <link rel="stylesheet" href="{{ asset('dist/style.css') }}" /> 
        {% endif %} 
        <link rel="icon" type="image/x-icon" href="{{ asset('favicon.ico') }}" /> 
    </head> 
    <body> 
        {% block body %}{% endblock %} 
        {% block javascripts %} 
            {% if app.environment == 'dev' %} 
                <script src="http://127.0.0.1:3000/static/bundle.js"></script> 
        {% else %} 
            <script src="{{ asset('dist/bundle.js') }}"></script> 
        {% endif %} 
    {% endblock %} 
    </body>
</html>
```

Ce qui est intéressant ici, c'est le block `{% block javascripts %}`.

Comme nous sommes en environnement de développement, on insert le fichier servi par Webpack depuis le port  3000 du localhost \(voir la configuration définie plus haut dans "output" de webpack.config.js\). On remarque que le body est vide, ce qui nous fera une belle page blanche. Notez aussi que le fichier css \(style.css\) est servi de la même manière que bundle.js.

Le contrôleur par défaut affiche la page index.html.twig \(qui elle-même étend base.html.twig, que nous venons de modifier\). Voir le fichier src\/AppBundle\/Controller\/DefautController.php.

Personnalisons ce fameux fichier index dans app\/Resources\/views\/default\/index.html.twig :

```php
{% extends 'base.html.twig' %}
{% block body %}
    <div id="app"></div>
{% endblock %}
```

Voilà pour la partie twig. C'est React qui va prendre la suite en gérant le html et l'état des objets de l'application front.

En résumé de cette partie, nous avons créé les conditions pour que React s'exécute au sein de notre application Symfony en ajoutant une simple balise div avec un id qui vaut "app".

Passons au plus intéressant : l'application React en elle-même.

**L'application côté front**

React fonctionne par composant. C'est-à-dire que des fichiers js vont inclure d'autres fichiers js et leur passer des objets.

Le composant de base s'appellera "app". Et sera rattaché au DOM par la balise "app" définie dans index.html.twig.

Créons donc ce premier composant dans app\/Resources\/js\/app\/js :

```js
import React from 'react';
import ReactDom from 'react-dom';
import Component from './component.js';
ReactDom.render(<Component/>, document.getElementById('app'));
```

Félicitations, votre application React est créée. Qu'a-t-on fait ? On a dit d'importer react, c'est le minimum. Puis react-dom, qui permettra de manipuler le DOM pour afficher nos composants. Enfin, on importe un composant nommé Component. Ce composant s'affichera dans la balise "app" du fichier HTML grâce à la fonction render de react-dom.

Bon si on regarde maintenant dans un navigateur l'adresse [http:\/\/localhost\/nom\_du\_projet\/web\/app\_dev.php](http://localhost/nom_du_projet/web/app_dev.php), on ne verra qu'une page blanche.

Mais en ouvrant la console du navigateur, vous devriez voir des informations de ce style :

![](assets/xhr.png)

Ceci montre que l'application React a démarré, même si elle n'affiche rien pour l'instant. Il y a d'ailleurs aussi une ligne en rouge dans cette console, à propos du composant Component. C'est normal, il n'est pas encore créé.

Pour commencer à afficher quelque chose, il faut créer le fichier component.js \(toujours dans app\/Resources\/js\/app\/\) avec ce contenu par exemple :

```js
import React from 'react';
const Component = React.createClass({
    render: function () {
        return (
            <h1>Hello world !</h1>
        );
    }
});
export default Component;
```

Ici, on déclare un composant qui s'appelle Component et qui est une classe React. Dans cette classe, la méthode render, lorsqu'elle est appelée, affiche la phrase "Hello World !". Rien de complexe donc.

Si on retourne sur notre navigateur, et qu'on rafraîchit, on doit voir apparaître cette phrase. Les prochaines modifications se verront sans avoir besoin de rafraîchir grâce au module Hot Reload de React installé au début. Mais après une erreur console, la page ne se rafraîchit pas.

Voilà, c'est tout pour cette partie. Dans une seconde partie, on va styliser un peu tout ça avec Twitter Bootstrap et voir des concepts plus avancés de React.

