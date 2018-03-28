## React-router

Comparé à Angular, framework JS complet, React apparaît comme un squelette autour duquel on peut ajouter des outils et librairies en fonction des besoins du projet.

Un des outils les plus essentiels à mon sens est un routeur, pour gérer les liens et l'historique de navigation des utilisateurs dans notre application.

De ce besoin, la communauté React a su proposer plusieurs solutions, dont une qui fait consensus : react-router. On verra dans un prochain article que si on intègre Redux pour gérer l'état des objets, react-router n'est pas forcément le plus adapté. Mais pour le moment, il fait très bien le job.

On commence par l'installer :  `npm i react-router --save`

On ajoute d'autres composants, qui seront comme des pages visuellement :

* components/Page1.js

```js
import React from 'react';

const Page1 = () => {
    return (
        <div>
            <h1>Titre page 1</h1>
            <div>Contenu page 1</div>
        </div>
    );
};

export default Page1;
```

* components/Page2.js

```js
import React from 'react';

const Page2 = () => {
    return (
        <div>
            <h1>Titre page 2</h1>
            <div>Contenu page 2</div>
         </div>
    );
};

export default Page2;
```

* components/NoMatch.js :

```js
import React from 'react';

const NoMatch = () => {
    return (
        <div>
            <h1>404</h1>
            <div>Page non trouvée</div>
        </div>
    );
};

export default NoMatch;
```

Rien de bien extraordinaire, mais là où ça va devenir intéressant c'est quand on va modifier la barre de menus et ajouter le routeur dans app.

* layout/Header.js : 

Ajouter ceci en ligne 3 :

```js
import { LinkContainer } from 'react-router-bootstrap';
```

Remplacer les liens NavItem par ceci :

```js
<LinkContainer to="home"><NavItem>Accueil</NavItem></LinkContainer>
<LinkContainer to="page1"><NavItem>Menu 1</NavItem></LinkContainer>
<LinkContainer to="page2"><NavItem>Menu 2</NavItem></LinkContainer>
```

La différence avec la version précédente de ce fichier est qu'on a modifié les liens href du menu.

* Layout.js :  Supprimer la ligne 5 : 

`import Home from './components/Home';`

et remplacer la ligne `<Home />` par

```js
{this.props.children}
```

`children` contient le composant qui sera appelé par react-router. En ce qui concerne les propriétés \(`this.props`\), un élément fondamental de la philosophie React, on verra dans un autre article comment les gérer. Pour en savoir plus dès maintenant :

[https://facebook.github.io/react/docs/multiple-components.html](https://facebook.github.io/react/docs/multiple-components.html)

* app.js :

```js
import React from 'react';
import ReactDom from 'react-dom';
import { Router, Route, IndexRoute, browserHistory } from 'react-router';

import Layout from './Layout';
import Home from './components/Home';
import Page1 from './components/Page1';
import Page2 from './components/Page2';
import NoMatch from './components/NoMatch';

ReactDom.render((
    <Router history={browserHistory}>
        <Route path="/" component={Layout}>
            <IndexRoute component={Home} />
            <Route path="/home" component={Home} />
            <Route path="/page1" component={Page1} />        
            <Route path="/page2" component={Page2} />
            <Route path="*" component={NoMatch} />    
        </Route>
    </Router>
), document.getElementById('app'));
```

Le composant `app` est celui qui est le plus modifié puisque c'est lui qui intègre le routeur et donc les routes.

Dans une application plus importante, les routes pourraient être importées depuis un autre fichier pour éviter de surcharger celui-ci.

Le router est connecté à l'API history du navigateur grâce à `browserHistory`. Il utilise plusieurs routes \(`home`, `page1`, `page2`\) héritant d'une route principale \(`/`\).

On pourrait donc imaginer d'avoir des routes imbriquées telles que page1/souspage1/details de cette manière :

```js
<Route path="page1" component={Page1}>
    <Route path="souspage1" component={SousPage1}>
        <Route path="details" component={Details} />
    </Route>
</Route>
```

Un très bon article sur react-router : [https://css-tricks.com/learning-react-router/](https://css-tricks.com/learning-react-router/)

On remarque dans app.js une ligne avec `<IndexRoute>`. C'est la manière de déclarer une route par défaut.

Pour résumer, par défaut \(en tapant localhost:8000 donc\), le routeur va chercher la route `/`, c'est-à-dire le layout, mais comme `IndexRoute` est précisé, il charge dans ce layout, le composant `Home`.

Ensuite, si on navigue vers localhost:8000/page1 par exemple, le contenu de `Page1` s'affichera dans le layout.

Si on navigue vers une url qui n'est pas prise en charge \(par exemple localhost:8000/zzzz\), le routeur affichera le composant `NoMatch`.

![](/assets/Capture d’écran 2016-09-23 à 14.35.33.png)

