COMMENT J'AI FAIT
=================

Foutre Wordpress sur mon hébergeur
__________________________________

Ça c'est comme d'hab, j'ai juste modifié `wp_config.php` et j'ai envoyé le tout via FTP.

Choper Roots
____________

Y'a un bouton "Download" à droite. J'ai cliqué dessus, et j'ai mis le contenu du Zip dans un dossier que j'ai intelligement nommé "Thème".

Le truc avec Grunt là
_____________________

Ben j'ai ouvert un terminal dans ce dossier et j'ai fait `npm install` puis `grunt dev`, comme expliqué sur le site officiel, quoi.

FTPer mon thème dans le dossier des thèmes de Wordpress
________________________________________________________

C'est là qu'il faut pas se faire niquer : ne _pas_ copier le dossier `/node_modules` (crée par la commande `npm install`). Il pèse dix tonnes et je suis quasiment sûr qu'il ne sert à rien sur le serveur : c'est certainement pour bosser en local uniquement.

Taper dans les CSS !
____________________

Ou plutôt "taper dans les Less", puisqu'ici on travaille avec des fichiers less de Bootstrap (tout est dans `/assets/less`) qu'on transforme ensuite en CSS avec Grunt. Faut que je consulte [la doc de Bootstrap](http://roots.io/modifying-bootstrap-in-roots/) (à ce stade, j'imagine que la doc de Boilerplate sera plutôt à étudier au moment de changer le code HTML).

### variables.less

L'emphase est de moi :

> Take some time to review all these variables and understand how they work. *Before writing any additional CSS* for your site, make sure that the effect you are looking for cannot be easily achieved by editing the Bootstrap variables.

J'ai prévu des effets à la con pour mon site, donc ça va barder.

	@brand-primary:         #ffa500; // orange !
	@body-bg:               #428bca; // bleu
	@text-color:            @gray-lighter;
	@headings-color:          #fff;

### Polices

J'ai modifié *app.less* en rajoutant des @import pour chaque police Google que j'utlise (à savoir Roboto, Archivo Black et Chivo). Ça me permet ensuite de mettre ces polices dans *variables.less* :

	@font-family-sans-serif:  "Chivo", sans-serif;
	@headings-font-family:    "Archivo Black", sans-serif;

Pour le logo, j'ai dû changer les couleurs dans *variables.less*

À présent je vais dans *type.less* et là on devrait commencer à rigoler :

	h1, .h1,
	h2, .h2, // pour la page d'accueil
	h3, .h3 { text-transform: uppercase; } // j'imagine que c'est bien pour les intertitres aussi

J'ajoute les bordules bien dégueu

### Virer le superflu

J'ai commenté des trucs dans *bootstrap.less*. Reste à savoir qu'est-ce qui est inutile et qu'est-ce qui ne l'est pas.

### MISC

- je crois que je vais devoir virer le h1 comme sur Large Prime Numbers

