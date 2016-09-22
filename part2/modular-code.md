## Code modulaire

Un des forces de React, c'est de permettre d'imbriquer des modules. Bon, c'est le cas de tout framework moderne, pas forcément JS. Mais avec React, on réfléchit en composants.

Nous allons concrètement mettre en oeuvre cette philosophie en créant un assemblage de composants à travers un layout (disposition en bon français).

Pour cela, créons un composant Layout.js à la racine du projet :

```js
import React from 'react';
import { Grid, Row, Col } from 'react-bootstrap';
import Header from './layout/Header';
import Footer from './layout/Footer';
import Content from './components/Content';

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
                        <Content />
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

Plus propre, n'est-ce pas ? Il faut maintenant les fichiers Header, Footer et Content (importés au début du fichier). Après avoir créé les dossiers vides layout et components, on peut les remplir avec les fichier js suivants :

- layout/Header.js (la copie du menu dans le fichier component.js)

```js
import React from 'react';
import { Nav, Navbar, NavItem } from 'react-bootstrap';

const Header = () => {
    return (
        <Navbar>
            <Navbar.Header>
                <Navbar.Brand>
                    <a href="#">Site de tutoriel</a>
                </Navbar.Brand>
                <Navbar.Toggle />
            </Navbar.Header>
            <Navbar.Collapse>
                <Nav>
                    <NavItem href="#">Accueil</NavItem>
                    <NavItem href="#">Menu 1</NavItem>
                    <NavItem href="#">Menu 2</NavItem>
                </Nav>
            </Navbar.Collapse>
        </Navbar>
    );
};

export default Header;
```

- layout/Footer.js (avec, au passage, du style en mode inline, c'est-à-dire sans passer par un fichier css externe)

```js
import React from 'react';
import { Grid, Row, Col, Nav, Navbar, NavItem } from 'react-bootstrap';
const Footer = () => {
    return (
        <div style={{
            height: '60px',
            backgroundColor: '#f8f8f8',
            marginTop: '50px',
            padding: '20px',
            paddingBottom: '20px',
            borderRadius: '4px',
            border: '1px solid #e7e7e7'
        }}>Falkodev - 2016</div>
    );
};

export default Footer;
```

- components/Content.js 

```js
import React from 'react';
import { Grid, Row, Col, Nav, Navbar, NavItem } from 'react-bootstrap';

const Content = () => {
    return (
        <div className="content">
            <h1>Hello world!</h1>
        </div>
    );
};

export default Content;
```

Par convention, les composants ont une majuscule en début de nom, mais ce n'est pas une obligation technique. 

Le code est du JSX, l'extension syntaxique type XML développée par Facebook pour React. Les fichiers pourraient donc avoir l'extension .jsx, mais ça n'a pas d'importance pour Webpack au moment de la compilation.

Comme les composants créés sont stateless (on verra dans une autre partie c'est que l'état dans une application React), j'ai utilisé des fonctions plutôt que des classes dans chaque composant.

Cela 