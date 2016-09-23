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

Rien de bien extraordinaire, mais là où ça va devenir intéressant c'est quand on va modifier la barre de menus et ajouter le routeur dans app.

- layout/Header.js : Remplacer les lignes 15, 16 et 17 : 

```js
<NavItem href="home">Accueil</NavItem>
<NavItem href="page1">Menu 1</NavItem>
<NavItem href="page2">Menu 2</NavItem>
```

La différence avec la version précédente de ce fichier est qu'on a modifié les liens href du menu.

- Layout.js :  Supprimer la ligne 5 : 



```js

import React from 'react';

import { Grid, Row, Col } from 'react-bootstrap';

import Header from './layout/Header';

import Footer from './layout/Footer';

import Home from './components/Home';



class Layout extends React.Component {

 render () {

 return (

 <Grid fluid>

 <Row>

 <Col sm={12}>

 <Header />

 </Col>

 </Row>

 <Row>

 <Col sm={12}>

 <Home />

 </Col>

 </Row>

 <Row>

 <Col sm={12}>

 <Footer />

 </Col>

 </Row>

 </Grid>

 );

 }

};



export default Layout;

```


- app.js :

