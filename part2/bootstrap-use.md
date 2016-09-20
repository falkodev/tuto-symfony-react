## Utilisation de Twitter Bootstrap

Reprenons le composant créé dans la première partie du tutoriel : component.js, qui n'affiche qu'un "Hello world!".

Voici à quoi il ressemblait :

```js
import React from 'react';

const Component = () => {
 return (<h1>Hello world!</h1>);
};

export default Component;
```

Nous allons maintenant l'améliorer un peu : 

```js
import React from 'react';
import { Grid, Row, Col, Nav, Navbar, NavItem } from 'react-bootstrap';

const Component = () => {
    return (
        <Grid fluid>
            <Row>
                <Col sm={12}>
                    <Navbar>
                        <Navbar.Header>
                            <Navbar.Brand>
                                <a href="#">Site de tutoriel</a>
                            </Navbar.Brand>
                            <Navbar.Toggle/>
                        </Navbar.Header>
                        <Navbar.Collapse>
                            <Nav>
                                <NavItem href="#">Accueil</NavItem>
                                <NavItem href="#">Menu 1</NavItem>
                                <NavItem href="#">Menu 2</NavItem>
                            </Nav>
                        </Navbar.Collapse>
                    </Navbar>
                </Col>
                <Col sm={12}>
                    <div className="content">
                        <h1>Hello world!</h1>
                    </div>
                </Col>
            </Row>
        </Grid>
    );
};

export default Component;
```

Qu'a-t-on fait ? Rien de bien extraordinaire : on a ajouté une barre de menu et rendu le tout responsive. On peut d'ailleurs redimensionner l'écran pour s'en rendre compte.

En mode desktop :
![](/assets/bootstrap_large.png)

En mode mobile :
![](/assets/bootstrap_small.png)

Pour rendre le compte plus propre et modulaire, ce code contenant la barre de menu sera à mettre dans un composant de layout, lui-même incluant un composant header.