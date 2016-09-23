## L'application côté back

Par défaut, dans le dossier app/Resources/views, nous trouvons le fichier base.html.twig. Etant donné que ce fichier sera celui utilisé par tous les autres templates, nous devons le personnaliser comme ceci :

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

Ce qui est intéressant ici, c'est le bloc `{% block javascripts %}`.

Comme nous sommes en environnement de développement, on insert le fichier servi par Webpack depuis le port 3000 du localhost \(voir la configuration définie précédemment dans "output" de webpack.config.js\). On remarque que le body est vide, ce qui nous fera une belle page blanche. Notez aussi que le fichier css \(`style.css`\) est servi de la même manière que bundle.js.

Le contrôleur par défaut affiche la page `index.html.twig` \(qui elle-même étend base.html.twig, que nous venons de modifier\). Voir le fichier `src/AppBundle/Controller/DefautController.php`.

Personnalisons ce fameux fichier index dans `app/Resources/views/default/index.html.twig` :

```php
{% extends 'base.html.twig' %}
{% block body %}
    <div id="app"></div>
{% endblock %}
```

Voilà pour la partie twig. 

Modifions légèrement la route utilisée dans le contrôleur `src/AppBundle/Controller/DefautController.php` aussi : 

```php
<?php

namespace AppBundle\Controller;

use Sensio\Bundle\FrameworkExtraBundle\Configuration\Route;
use Symfony\Bundle\FrameworkBundle\Controller\Controller;
use Symfony\Component\HttpFoundation\Request;

class DefaultController extends Controller
{
    /**
     * @Route("{slug}", name="homepage", requirements={"slug"=".+"})
     */
    public function indexAction(Request $request, $slug = "default")
    {
        return $this->render('default/index.html.twig');
    }
}
```

Ainsi, toutes les urls seront gérées par ce contrôleur
et React va prendre la suite en gérant le html et l'état des objets de l'application front.

Dans le cadre d'une application avec d'autres contrôleurs, il faudrait juste veiller à placer cette route en dernier dans le routing (voir le fichier `app/config/routing.yml` où il n'y a qu'un lien vers ce contrôleur pour l'instant), pour ne pas écraser la gestion des autres routes.

En résumé de cette partie, nous avons créé les conditions pour que React s'exécute au sein de notre application Symfony en ajoutant une simple balise div avec un id qui vaut "app".

Passons au plus intéressant : l'application React en elle-même.
