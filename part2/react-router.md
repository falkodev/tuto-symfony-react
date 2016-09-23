## React-router

Comparé à Angular, framework JS complet, React apparaît comme un squelette autour duquel on peut ajouter des outils et librairies en fonction des besoins du projet.

Un des outils les plus essentiels à mon sens est un routeur, pour gérer les liens et l'historique de navigation des utilisateurs dans notre application.

De ce besoin, la communauté React a su proposer plusieurs solutions, dont une qui fait consensus : react-router. On verra dans un prochain article que si on intègre Redux pour gérer l'état des objets, react-router n'est pas forcément le plus adapté. Mais pour le moment, il fait très bien le job.

On commence par l'installer :  `npm i react-router --save`

On ajoute d'autres composants, qui seront comme des pages visuellement :

- components/Page1.js

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

- components/Page2.js



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

- components/NoMatch.js :

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

- layout/Header.js : Remplacer les lignes 15, 16 et 17 : 

```js
<NavItem href="home">Accueil</NavItem>
<NavItem href="page1">Menu 1</NavItem>
<NavItem href="page2">Menu 2</NavItem>
```

La différence avec la version précédente de ce fichier est qu'on a modifié les liens href du menu.

- Layout.js :  Supprimer la ligne 5 : 

`import Home from './components/Home';`

et remplacer la ligne `<Home />` par 

```js
{this.props.children}

```

`children` contient le composant qui sera appelé par react-router. En ce qui concerne les propriétés (`this.props`), un élément fondamental de la philosophie React, on verra dans un autre article comment les gérer. Pour en savoir plus dès maintenant : 

[https://facebook.github.io/react/docs/multiple-components.html](https://facebook.github.io/react/docs/multiple-components.html)

- app.js :

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

Le composant `app` est celui qui est le plus modifié puisque c'est lui qui intègre le router et donc les routes. 

Dans une application plus importante, les routes pourraient être importées depuis un autre fichier pour éviter de surcharger celui-ci.

Le router est connecté à l'API history du navigateur grâce à `browserHistory`. Il utilise plusieurs routes (`home`, `page1`, `page2`) héritant d'une route principale (`/`). On pourrait donc imaginer d'avoir des routes imbriquées telles que products/product1/details de cette manière : 

<Route path="products" component={Products}>
 <Route path="product1" component={Produc} />
 </Route>
