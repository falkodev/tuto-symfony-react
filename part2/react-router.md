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

