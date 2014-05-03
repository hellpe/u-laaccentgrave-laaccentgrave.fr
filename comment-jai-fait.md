COMMENT J'AI FAIT
=================

##Foutre Wordpress sur mon hébergeur

Ça c'est comme d'hab, j'ai juste modifié `wp_config.php` et j'ai envoyé le tout via FTP.

##Choper Roots

~~Y'a un bouton "Download" à droite. J'ai cliqué dessus, et j'ai mis le contenu du Zip dans un dossier que j'ai intelligement nommé "Thème".~~ ouais et après j'ai fait `git clone` et tout et tout chuis un boss

##Le truc avec Grunt là

Ben j'ai ouvert un terminal dans ce dossier et j'ai fait `npm install` puis `grunt dev`, comme expliqué sur le site officiel, quoi. Et naturellement, je fais gaffe à relancer Grunt à chaque fois que je reprend le taf.

##FTPer mon thème dans le dossier des thèmes de Wordpress

C'est là qu'il faut pas se faire niquer : ne _pas_ copier le dossier `/node_modules` (crée par la commande `npm install`). Il pèse dix tonnes et il ne sert à rien sur le serveur : c'est certainement pour bosser en local uniquement.

### Mettre le petit nom du thème dans `style.css` et `screenshot.png` 

Ça sert juste à "baptiser" le thème.

##Taper dans les CSS !

Ou plutôt "taper dans les Less", puisqu'ici on travaille avec des fichiers *.less de Bootstrap (tout est dans `/assets/less`) qu'on transforme ensuite en CSS avec Grunt. Faut que je consulte [la doc de Bootstrap](http://roots.io/modifying-bootstrap-in-roots/) (à ce stade, j'imagine que la doc de Boilerplate sera plutôt à étudier au moment de changer le code HTML).

### variables.less

L'emphase est de moi :

> Take some time to review all these variables and understand how they work. *Before writing any additional CSS* for your site, make sure that the effect you are looking for cannot be easily achieved by editing the Bootstrap variables.

J'ai prévu des effets à la con pour mon site, donc ça va barder.

```less
	@brand-primary:         #ffa500; // orange !
	@page-header-bg:        #428bca; // nouvelle variable pour la bannière et l'accueil - bleu
	@body-bg:               lighten(@header-bg); // bleu
	@page-header-text-color:	@gray-lighter; //nouvelle variable pour le texte de la bannière
	@text-color:            @gray-darker;
	@link-color:            darken(@brand-primary, 15%);
	@headings-color:        #fff;
	
	@border-radius-extreme:     35px; // pour les vignettes d'article
	
	@thumbnail-bg:          @page-header-bg;
```

Pour les couleurs `@page-header*`, j'ai précisé ça dans `scaffolding.less` :

```less
body.home {
  color: @gray-lighter;
  background-color: @page-header-bg;
}
```


### Polices

J'ai modifié `app.less` en rajoutant des `@import` pour chaque police Google que j'utlise (à savoir Roboto, Archivo Black et Chivo). Ça me permet ensuite de mettre ces polices dans `variables.less` :

```less
	@font-family-sans-serif:  "Chivo", sans-serif;
	@headings-font-family:    "Archivo Black", sans-serif;
```

Pour le logo, j'ai dû changer la typo dans `navbar.less` et les couleurs dans `variables.less` ("*navbar brand label*").

À présent je vais dans `type.less` et là on devrait commencer à rigoler :

```less
	h1, .h1,
	h2, .h2, // pour la page d'accueil
	h3, .h3 { text-transform: uppercase; } // j'imagine que c'est bien pour les intertitres aussi
```

J'ajoute les bordures bien dégueu sur les titres :

```less
h1, .h1 { text-shadow:	2px 0 0 @brand-primary,
                   	-2px 0 0 @brand-primary,
                  	0 2px 0 @brand-primary,
                    	0 -2px 0 @brand-primary,  
        
       			2px 2px 0 @brand-primary,
      			-2px -2px 0 @brand-primary,
                    	2px -2px 0 @brand-primary,
                    	-2px 2px 0 @brand-primary, /*contour*/

                    	0 5px 0 @brand-primary, /*ombre*/
                    	2px 5px 0 @brand-primary,
                    	-2px 5px 0 @brand-primary; /*contour de l'ombre*/
  		  	-webkit-text-stroke: 1px @brand-primary; /*contour intérieur*/ }
```

Pour les `h2` j'ai fait pareil en réduisant un peu les tailles (1,5 au lieu de 2, 4 au lieu de 5), puis j'ai ajouté l'inversion des couleurs en `:hover` et en `:focus`.

### Vignettes

Pour l'effet d'*ombre* sur les vignettes, j'ai mis au point 2 méthodes :

1. avec deux `<div>`, un `.parallelogrammeGros` (l'ombre) et un `.parallelogramme` tout court (l'image), ce qui me permet du texte qui déborde pas hors de l'ombre (marquee U-LÀLÀ)
2. avec un seul `<div class="parallelogramme">`sur lequel je mets une `box-shadow`, ce qui m'oblige à utiliser `position: relative` (y'a ptet une méthode plus élégante) pour pouvoir mettre du texte sur l'ombre (dates sur la home)

Pour les articles de la home, j'ai mis le CSS correspondant dans `thumbnails.less` (en utilisant la méthode n°2 : celle à base de `box-shadow`) :

```css
.parallelogramme {
  width:200px; 
  height:200px; 
  border-radius: 35px;
  overflow: hidden;
  transform: skew(-11deg); 
    -webkit-transform: skew(-11deg);
  border-radius: @border-radius-extreme;
  box-shadow: 0 -30px 0 @brand-primary;
}

.parallelogramme img{
  position: relative;
  left: -20px;
  top: -60px;
  transform: skew(11deg); 
    -webkit-transform: skew(11deg);
}
```

Pour centrer les dates, je les ai passées en `display: block`.
Reste à régler le padding des vignettes, pour les rendre moins large (peut-être étudier le Grid System de Bootstrap au préalable)

### pager.less

~~détail à la con : j'ai incliné les boutons de bas de page~~ laisse tomber, y'a pas moyen d'incliner le cartouche sans le texte, ça fait trop moche

### Virer le superflu

J'ai commenté des trucs dans `bootstrap.less`. Reste à savoir qu'est-ce qui est inutile et qu'est-ce qui ne l'est pas.

## Taper dans le PHP ! (alias les templates)

Si j'ai bien compris c'est :
- `home.php` pour la page d'accueil (liste des articles les plus récents)
- `single-critique` pour les critiques (il va falloir créer un "custom post type")
- `single.php` pour les articles (càd tout le reste)
- `category-critique.php` ça peut servir ça
- `category.php`
- `search.php` pour les recherches (bof)
- `author.php` pour les auteurs (on peut distinguer par auteur mais osef)
- `page-about.php` mais `page.php` peut suffire
- `404.php` faudra bien y songer

### Home

> Copy `index.php` to `home.php` for customizing the Home page if you’re showing the latest posts (under Reading Settings) instead of a static front page

Ça tombe bien c'est exactement ce qu'il me faut.
Par contre à mon avis, il ne faut pas faire ce qui suit : copier `templates/content.php` vers `content-home.php`
J'ai ensuite fusionné `home.php` avec [ça](http://getbootstrap.com/components/#thumbnails), et pour aller à la ligne j'utilise [ce code](http://wordpress.org/support/topic/adding-a-clearing-div-to-every-third-post-in-the-loop) trouvé au pif sur Google, mais en virant la première ligne (càd en préservant la syntaxte normale de WP, vu que le `if (have_posts())` est déjà plus haut)
> (à voir également : http://www.wpbeginner.com/wp-themes/how-to-create-a-grid-display-of-post-thumbnails-in-wordpress-themes/)

Et pour faire les vignettes, j'ai rajouté mon `<div class="parallelogramme">` en suivant la méthode n°2 (cf. plus haut).
La norme à adopter semble être 230x200 (on rajoute 15px à gauche et à droite) ; reste à voir comment gérer la vignette plus grosse en header.

Pour les dates, il y a [le problème des posts du 1er](http://www.wordpress-fr.org/support/viewtopic.php?id=19609) (solution : éviter de poster le 1er du mois héhéhé)

### Header

même méthode bizarre : je copie `templates/page-header.php` vers `page-header-home.php`
(faut-il plutôt créer un `base-home.php` ? on verra)

la méthode n°1 de vignettes me fait créer des classes en plus, c'est moche mais ça marche (faut que j'apprenne à me servir du Less)

## MISC

- ~~je crois que je vais devoir virer le `<h1>` de la home ("Derniers articles") comme sur Large Prime Numbers~~ non, je peux être plus malin que ça et mettre U-LÀLÀ en `<h1>`, même si ce qui suit reste vrai
- sémantiquement, le titre de l'article "featured" doit avoir les mêmes balises que les autres, donc je ferai probablement `<h2 class="grostitre">`.
- "[You don't normally replace the foundations of a house once it has been built.](https://github.com/h5bp/html5-boilerplate/blob/v4.2.0/doc/faq.md#do-i-need-to-upgrade-my-sites-each-time-a-new-version-of-html5-boilerplate-is-released)"

