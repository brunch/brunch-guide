# Des plugins pour tous les besoins de build

Ceci fait partie du [Guide de Brunch.io](README.md).

Dans Brunch, la répartition des tâches est assez différente de celle qu’on trouve dans Grunt, Gulp, etc.  Beaucoup de fonctionnalités et comportements sont **inclus de base** (*pipeline* de build, *watcher* incrémental, *source maps*, etc.), mais le reste appartient en effet à des **plugins**, y compris la prise en charge de chaque **langage source**.

On utilise en général au moins un plugin pour les fichiers de scripts, un pour ceux de styles, et un minifieur pour chaque format.

Le site officiel en [recense pas mal](http://brunch.io/plugins.html), selon les *pull requests* de leurs auteurs, mais il y en a en fait [beaucoup plus](https://www.npmjs.com/search?q=brunch) ; nous allons parcourir les principaux ci-dessous.

**Note :** dans tout ce chapitre, les noms de plugins sont des **liens** vers leur descriptif sur npm.

## Activation d'un plugin

Pour qu’un plugin soit actif et utilisé, **il suffit qu’il soit installé**, donc qu'il figure au `package.json` et soit présent dans `node_modules` également.  La manière la plus simple d'aboutir à ça pour une première installation est `npm install --save-dev`, et la plus simple si `package.json` connaît déjà le module est tout bêtement `npm install`.

Brunch inspecte en fait les modules ainsi installés à la recherche de tous ceux dont le nom contient « brunch » (généralement à la fin, après un tiret, mais ça peut être n'importe où), et qui exportent un constructeur dont le prototype a une propriété `brunchPlugin` à `true`.

Il instancie alors automatiquement le plugin avec la configuration générale en argument, et l’enregistre en fonction de son type de fichier pris en charge, s’il en indique un (via ses propriétés `type` et `extension`).

Bref, oubliez les `loadNpmTasks` et autres blagues du genre.  Brunch la joue simple.

## Affinage éventuel par configuration

Chaque plugin est normalement conçu pour être **opérationnel et utile sans configuration spécifique** ; mais il reste possible d’affiner leur comportement via une configuration dédiée.  Celle-ci est dans le `brunch-config.coffee`, sous la clé `plugins` et la sous-clé définie par le plugin.

Par exemple, le
plugin [`appcache-brunch`](https://www.npmjs.com/package/appcache-brunch) lit `plugins.appcache`.  Le plus souvent, les noms
de clé sont triviaux à deviner, mais ça peut varier, utiliser la casse Camel ou non…  Par exemple, [`browser-sync-brunch`](https://www.npmjs.com/package/browser-sync-brunch) lit `plugins.browserSync`.  Consultez la doc du plugin pour être sûr-e !

## Brunch et les CSS

Les plugins orientés CSS ont sur leur prototype le type `"stylesheet"`, et généralement une valeur spécifique pour `extension`.  Ce sont principalement des *transpilers*, ce que Brunch appelle génériquement des *compilers*.  À l'heure où j'écris ces lignes, on trouve notamment :

  * [`css-brunch`](https://www.npmjs.com/package/css-brunch), pour la gestion des CSS « nues » (celles du W3C) ;
  * [`cssnext-brunch`](https://www.npmjs.com/package/cssnext-brunch), qui vise les évolutions en cours (CSS4+, etc.) grâce à [cssnext](https://cssnext.github.io/) ;
  * [`less-brunch`](https://www.npmjs.com/package/less-brunch) et [`sass-brunch`](https://www.npmjs.com/package/sass-brunch), évidemment, ainsi que [`compass-brunch`](https://www.npmjs.com/package/compass-brunch) qui délègue à Compass pour ceux qui aiment les trucs un peu… euh… lourds ?!
  * [`stylus-brunch`](https://www.npmjs.com/package/stylus-brunch) pour [Stylus](http://learnboost.github.com/stylus/), mon p’tit chéri ;
  * Divers outils essaient de jouer les couteaux suisses à CSS ; on trouve des plugins pour eux, par exemple [`rework-brunch`](https://github.com/bolasblack/rework-brunch) pour [rework](https://github.com/reworkcss/rework), [`pleeease-brunch`](https://www.npmjs.com/package/pleeease-brunch) pour [pleeease](http://pleeease.io/) ou encore [`postcss-brunch`](https://www.npmjs.com/package/postcss-brunch)pour [PostCSS](https://github.com/postcss/postcss).
  * [`autoprefixer-brunch`](https://www.npmjs.com/package/autoprefixer-brunch) se concentre sur l'auto-préfixage des règles à l'aide du bien-nommé [autoprefixer](https://github.com/postcss/autoprefixer), qui fait justement partie de PostCSS.
  * Côté *coding style*, [CSSComb](http://csscomb.com/) est sympa, et dispose évidemment de plugins pour les builders, dont [`csscomb-brunch`](https://www.npmjs.com/package/csscomb-brunch).

## Brunch et JavaScript

Paysage similaire aux CSS, mais pour un `type` valant `"javascript"`.  Je vous parlerai des *linters* plus loin, mais côté *transpilers*, on est déjà grave servis :

  * [`javascript-brunch`](https://www.npmjs.com/package/javascript-brunch) pour du JS « nu » (ES3/5, standards ECMA-262) ;
  * [`coffee-script-brunch`](https://www.npmjs.com/package/coffee-script-brunch), évidemment, et [`iced-coffee-script-brunch`](https://www.npmjs.com/package/iced-coffee-script-brunch) pour les têtes brulées ;
  * [`json-brunch`](https://www.npmjs.com/package/json-brunch), pour utiliser directement des fichiers JSON comme modules ;
  * dans le même univers, mais infiniment moins usité, on trouvera [`LiveScript-brunch`](https://www.npmjs.com/package/LiveScript-brunch) pour [LiveScript](http://gkz.github.io/LiveScript/), [`ember-script-brunch`](https://www.npmjs.com/package/ember-script-brunch) pour le plutôt niche [EmberScript](https://github.com/ghempton/ember-script), [`roy-brunch`](https://www.npmjs.com/package/roy-brunch) pour le *encore plus niche* [Roy](http://roy.brianmckenna.org/) et [`typescript-brunch`](https://www.npmjs.com/package/typescript-brunch) pour le déjà nettement plus populaire [TypeScript](http://www.typescriptlang.org/) (David & David, c’est vous que je regarde !).
  * Plus subtils : [`wisp-brunch`](https://www.npmjs.com/package/wisp-brunch) qui gère [Wisp](https://github.com/Gozala/wisp), une sorte de ClojureScript, et [`sweet-js-brunch`](https://www.npmjs.com/package/sweet-js-brunch) qui donne accès aux merveilles des « macros hygiéniques » de [sweet.js](http://sweetjs.org/).

Et parce qu'on est quand même en 2015, hein, on trouve aussi une gestion automatique des JSX de React et plusieurs voies pour ES6 :

  * [`react-brunch`](https://www.npmjs.com/package/react-brunch) qui auto-compile les `.jsx` via React ;
  * [`es6-module-transpiler-brunch`](https://www.npmjs.com/package/es6-module-transpiler-brunch) pour gérer seulement la syntaxe de modules d'ES6 (c'est l'avenir !) ;
  * [`traceur-brunch`](https://www.npmjs.com/package/traceur-brunch) et [`babel-brunch`](https://www.npmjs.com/package/babel-brunch) pour accéder à un maximum d’ES6.  Actuellement, [Babel](https://babeljs.io/) (anciennement 6to5 + CoreJS) a [nettement l'avantage](http://kangax.github.io/compat-table/es6/#babel) côté taux de prise en charge du standard, et en plus, il intègre directement JSX !

## Brunch et les templates

Après les scripts et les styles, la troisième catégorie de fichiers qui bénéfice d'une gestion spécifique par Brunch, ce sont les templates.

Pour rappel, lorsque Brunch a un plugin pour ça, il s'agit d'un *compiler* qui va **précompiler le template** pour produire un **module exportant une unique fonction** prête à l'emploi : on ne paie donc pas une pénalité d'analyse à l'exécution.  Cette fonction reçoit un objet dont les propriétés sont exploitables dans le template comme des variables : ce qu'on appelle traditionnellement un *presenter* ou *view model*.  La fonction retourne le HTML résultat.

Et côté langages de templates, on a vraiment **l'embarras du choix** :

  * Les **grands classiques** :
    * [`handlebars-brunch`](https://www.npmjs.com/package/handlebars-brunch) et une spécialisation [`ember-handlebars-brunch`](https://www.npmjs.com/package/ember-handlebars-brunch), évidemment ;
    * [`hoganjs-brunch`](https://www.npmjs.com/package/hoganjs-brunch), si vous préférez Hogan, une alternative courante ;
    * [`jade-brunch`](https://www.npmjs.com/package/jade-brunch) pour mon chéri [Jade](http://jade-lang.com/) et même une spécialisation [`jade-angularjs-brunch`](https://www.npmjs.com/package/jade-angularjs-brunch), qui produit un module AngularJS plutôt.
  * **L'ultra-puissant** [Dust](http://akdubya.github.io/dustjs/) est également bien représenté via [`dustjs-linkedin-brunch`](https://www.npmjs.com/package/dustjs-linkedin-brunch) pour [l'extension Dust](http://linkedin.github.io/dustjs/) maintenue par LinkedIn, qui est notamment utilisée par PayPal, aussi…
  * Un type a sorti [`jade-react-brunch`](https://www.npmjs.com/package/jade-react-brunch), qui me permet **d'éviter JSX et de continuer à utiliser la syntaxe Jade dans un fichier distinct**, et ça me pond un module exportant une fonction qui utilise le builder `React.DOM`…  Ça me fait saliver, ça !
  * Ensuite, toute la série des syntaxes **un peu plus niches**, en tout cas pour du templating :
    * [`eco-brunch`](https://www.npmjs.com/package/eco-brunch) pour [Eco](https://github.com/sstephenson/eco), une syntaxe de type ERB basée sur CoffeeScript ;
    * [`emblem-brunch`](https://www.npmjs.com/package/emblem-brunch) pour [Emblem](http://emblemjs.com/), similaire à Jade mais avec beaucoup de syntaxes confortables pour Ember et les habitués d’Handlebars ;
    * [`markdown-brunch`](https://www.npmjs.com/package/markdown-brunch) et [`yaml-front-matter-brunch`](https://www.npmjs.com/package/yaml-front-matter-brunch), qui du coup ressemble à du Jekyll ;
    * [`swig-brunch`](https://www.npmjs.com/package/swig-brunch) pour [Swig](http://paularmstrong.github.io/swig/), à l'attention des fans de Django ;
    * [`ractive-brunch`](https://www.npmjs.com/package/ractive-brunch) pour [RactiveJS](http://www.ractivejs.org/) ;
    * [`nunjucks-brunch`](https://www.npmjs.com/package/nunjucks-brunch) pour [Nunjucks](http://mozilla.github.io/nunjucks/), et enfin
      * [`html2js-brunch`](https://www.npmjs.com/package/html2js-brunch) pour [HTML2JS](https://github.com/aberman/html2js-brunch).
  * Il y a également des plugins qui compilent « statiquement » : c'est juste quand, au lieu de faire un *asset* statique HTML, vous préférez utiliser une syntaxe alternative, quitte à y injecter un *presenter* issu d’un fichier JSON, par exemple :
      * [`static-jade-brunch`](https://www.npmjs.com/package/static-jade-brunch) ;
      * [`static-underscore-brunch`](https://www.npmjs.com/package/static-underscore-brunch) (pour le micro-templating d’Underscore.js).

## Brunch et le workflow de développement

Aujourd’hui, **le dev front, c'est compliqué**.  On utilise plein de technos, on veut continuer à pouvoir déboguer rapidement, avoir un feedback facile dans le navigateur, faire attention aux performances, etc.

Il existe plein d'outils pour nous aider, mais c'est la plaie de devoir les gérer chacun de son côté, les lancer à part, etc.  **Brunch peut nous aider**, grâce à ses plugins d'intégration.

Les **linters** d'abord :

  * [`jshint-brunch`](https://www.npmjs.com/package/jshint-brunch) évidemment, qui va exécuter [JSHint](http://jshint.com/) avec nos réglages en vigueur (`.jshintrc` notamment) sur tout notre code applicatif (par défaut, `app` donc).  Peut fonctionner en mode avertissement (dans le log, mais n'empêche pas le build) ou erreur (empêche le build).  Exécuté en incrémental lors du *watcher*, aussi.
  * [`coffeelint-brunch`](https://www.npmjs.com/package/coffeelint-brunch) pour [CoffeeLint](http://www.coffeelint.org/), si vous faites du CoffeeScript.
  * [`jsxhint-brunch`](https://www.npmjs.com/package/jsxhint-brunch) pour [JSXHint](https://github.com/STRML/JSXHint/), capable d'exécuter JSHint sur du JSX sans se prendre les pieds dans le tapis.
  * Hélas pas d'intégration [ESLint](http://eslint.org/docs/integrations/) pour le moment, mais **pourquoi ne pas la contribuer** vous-même ?
  * Pas d'intégration JSLint non plus, mais là je ne risque pas de chouiner…

Une **boucle de feedback rapide** est indispensable quand on fait du front, avec la capacité notamment à voir tout de suite le résultat de nos travaux CSS et JS dans le navigateur, voire les navigateurs, ouvert(s).  On trouve quelques plugins pour ça, tous conçus pour fonctionner en mode *watcher* :

  * [`auto-reload-brunch`](https://www.npmjs.com/package/auto-reload-brunch) utilise une approche assez lourde : il recharge automatiquement la page dès qu'un changement a eu lieu.  Ça peut être un peu bourrin ; en plus, ça utilise les Web Sockets uniquement, donc pour IE, il faut au moins la version 10.
  * [`browser-sync-brunch`](https://www.npmjs.com/package/browser-sync-brunch) enrobe l'excellent [BrowserSync](http://www.browsersync.io/), qui permet d'injecter la CSS en direct (sans rechargement), mais aussi de synchroniser tout un tas de manips entre divers navigateurs ouverts sur la page : remplissage de formulaire, défilement, clics, etc.  Très pratique pour tester du *responsive* simplement.  *(full disclaimer : je fais partie des mainteneurs du plugin).*
  * [`fb-flo-brunch`](https://github.com/deliciousinsights/fb-flo-brunch), par votre serviteur, fournit une intégration de premier plan pour le génial [fb-flo](https://facebook.github.io/fb-flo/), jetez-vous dessus !

La **documentation du code** n’est pas oubliée : des intégrations regénèrent la doc au moment du build, pour vous épargner une ligne de commande.

  * [`jsdoc-brunch`](https://www.npmjs.com/package/jsdoc-brunch) évidemment, mais aussi…
  * [`docco-brunch`](https://www.npmjs.com/package/docco-brunch), pour [Docco](http://jashkenas.github.io/docco/), en mode **code source annoté** donc.
  * J'adorerais voir quelqu'un contribuer `groc-brunch`, car [Groc](http://nevir.github.io/groc/) enterre Docco !

On trouve aussi toute une série de plugins conçus pour **remplacer** des mots-clés, marqueurs ou chaînes de traduction pendant le build :

  * [`process-env-brunch`](https://www.npmjs.com/package/process-env-brunch) se base sur les variables d’environnement ;
  * [`keyword-brunch`](https://www.npmjs.com/package/keyword-brunch) (deux variantes) utilise la configuration générale pour déterminer sa table de correspondance et son comportement ;
  * [`jspreprocess-brunch`](https://www.npmjs.com/package/jspreprocess-brunch) ajoute un préprocesseur « façon C » (avec des directives `#BRUNCH_IF`, etc. dans des commentaires) qui permet de faire varier le code obtenu en fonction de la cible de compilation ;
  * [`constangular-brunch`](https://www.npmjs.com/package/constangular-brunch), un peu dans le même esprit, injecte des configurations au format YAML dans votre application AngularJS sous forme d'un module dédié, de façon sensible à l'environnement (développement, production) ;
  * [`yaml-i18n-brunch`](https://www.npmjs.com/package/yaml-i18n-brunch), plus spécialisé, convertit des fichiers YAML en fichiers JSON, en prenant soin de remplir les trous dans les *locales* dérivés à partir du *locale* par défaut.

Quelques autres plugins méritent d'être signalés :

  * [`dependency-brunch`](https://www.npmjs.com/package/dependency-brunch) permet d'exprimer des dépendances entre fichiers sources que Brunch n'a pas vu, de façon à ce qu'il recompile le nécessaire.  Par exemple, si des vues Jade étendent un layout ou incluent des mixins, on peut exprimer ces dépendances pour que toucher au layout (ou aux mixins) recompile automatiquement les vues qui les utilisent.
  * [`groundskeeper-brunch`](https://www.npmjs.com/package/groundskeeper-brunch) retire de vos fichiers JS tout ce qui peut gêner en production : appels `console`, instructions `debugger`, blocs spécifiques… (à définir impérativement *avant* les minifieurs).
  * [`after-brunch`](https://www.npmjs.com/package/after-brunch) fournit une manière simple d'enregistrer des commandes (lignes de commande) à exécuter après un build, ce qui ouvre la porte à beaucoup de choses !

## Brunch et la performance web

Brunch se soucie évidemment de vos perfs, et donc de produire des **assets aussi optimisés que possible**, tout en favorisant des technologies complémentaires.  La plupart de ces plugins n'ont pas d'intérêt au watch, plus pour les builds one-shot de production.

Ça commence avec divers plugins autour des **images** :

  * [`retina-brunch`](https://www.npmjs.com/package/retina-brunch) dérive la version simple de toute image « déclarée Retina » (dotée d'un suffixe de mantisse `@2x`) ;
  * [`sprite-brunch`](https://www.npmjs.com/package/sprite-brunch) utilise [Spritesmith](https://github.com/Ensighten/spritesmith) pour produire une grosse image *sprited* et la feuille de style associée (SASS, LESS ou Stylus) à partir de vos images d'origine.  Pas aussi puissant que Glue, mais costaud quand même.
  * [`imageoptmizer-brunch`](https://www.npmjs.com/package/imageoptmizer-brunch) (notez l'absence du `i` central…) se déclenche en mode production / optimisation pour appliquer à vos images dans le dossier public, suivant ce que vous avez d'installé, [JPEGTran](http://desgeeksetdeslettres.com/programmation-java/jpegtran-un-outil-permettant-doptimiser-les-images-jpeg), [OptiPNG](http://optipng.sourceforge.net/) et [SmushIt](http://imgopt.com/).  Histoire de bien réduire les tailles.

On a bien sûr des **minifieurs** haut de gamme pour JS et CSS :

  * [`uglify-js-brunch`](https://www.npmjs.com/package/uglify-js-brunch) utilise [UglifyJS 2](https://github.com/mishoo/UglifyJS2) pour compacter à mort les fichiers JS produits ;
  * [`clean-css-brunch`](https://www.npmjs.com/package/clean-css-brunch) se base sur [CleanCSS](https://github.com/jakubpawlowicz/clean-css), un des meilleurs compacteurs CSS du marché (et si vous voulez intégrer le tout nouveau tout beau more-css, n'hésitez pas à contribuer un plugin !).
  * On trouve aussi [`csso-brunch`](https://www.npmjs.com/package/csso-brunch).
  * [`uncss-brunch`](https://www.npmjs.com/package/uncss-brunch) exploite le génial [UnCSS](https://github.com/giakki/uncss) pour détecter les **parties « mortes » de nos feuilles de style** ; si vous voulez combiner ça avec clean-css, vous avez soit les deux modules séparément, soit la combo [`clean-css-uncss-brunch`](https://www.npmjs.com/package/clean-css-uncss-brunch).

On trouve également beaucoup de plugins visant à apposer une « empreinte » dans les noms de fichiers, afin de permettre des **en-têtes de cache agressifs** (expirations très lointaines).

  * [`digest-brunch`](https://www.npmjs.com/package/digest-brunch) calcule l'empreinte en se basant sur le contenu ;
  * [`git-digest-brunch`](https://www.npmjs.com/package/git-digest-brunch) et [`hg-digest-brunch`](https://www.npmjs.com/package/hg-digest-brunch) préfèrent utiliser le SHA du commit courant (ce qui suppose que vous committez de façon appropriée).
  * [`gzip-brunch`](https://www.npmjs.com/package/gzip-brunch) compresse les assets CSS/JS finalisés, en copie ou en remplacement des fichiers d’origine.

Si vous utilisez AppCache (et tant que vous n'avez pas `ServiceWorker` sous la main, vous devriez !), vous trouverez des trucs utiles aussi :

  * [`appcache-brunch`](https://www.npmjs.com/package/appcache-brunch) maintient tout seul le **manifeste à jour**, avec les noms des fichiers à cacher mais aussi un *digest* unique, auto-maintenu, qui fait que **si un des fichiers cachés changent, le manifeste aussi !**  Sans ça, invalider l'appcache en dev est vite pénible…
  * Dans le même esprit, [`brunch-signature`](https://www.npmjs.com/package/brunch-signature) calcule un *digest* à partir des fichiers produits et le pose dans un fichier statique de votre choix ; ça vous permet par exemple de faire un **poll Ajax à faible fréquence** dessus pour détecter automatiquement, en cours de vie de la page, que l'appli a changé (et donc proposer un rechargement).
  * Toujours dans ce domaine-là, [`version-brunch`](https://www.npmjs.com/package/version-brunch) auto-maintient un numéro de version pour votre appli, basé sur celui de votre `package.json` mais avec un numéro de build en plus ; il le met aussi à dispo dans un fichier statique (que vous pouvez donc *poller*), et auto-remplace certains marqueurs de version dans tous vos fichiers produits.

Enfin, [`cloudfront-brunch`](https://www.npmjs.com/package/cloudfront-brunch) est un des plugins capables d'auto-uploader vos **assets dans un *bucket* S3**, et d'envoyer la requête d'invalidation CloudFront pour vous.  Sympa.

## Réaliser un plugin Brunch

Tous ces plugins sont bien sympa, mais je suis sûr qu'à ce stade vous êtes déjà en train de réfléchir à ce nouveau plugin tout frais que vous aimeriez créer, pas vrai ?  Pas d'inquiétude, je vais vous montrer comment faire **vos propres plugins** Brunch…

Brunch découpe ses plugins en plusieurs catégories : compilateurs, linters, optimiseurs…  Suivant leur catégoriée détectée, ils sont consultés à divers moments du cycle de build, dans différents environnements, etc.

Une doc « API » est [disponible en ligne](https://github.com/brunch/brunch/blob/stable/docs/plugins.md), qui aide pas mal, et puis bien sûr on est libre de consulter le source des plugins existants.  Mais pour vous mettre le pied à l'étrier, on va faire un plugin de type compilateur, qui sera générique quant aux extensions, toutefois.

Je vous ai parlé tout à l'heure du plugin `git-digest-brunch`, qui injecte dans les fichiers produits le SHA du HEAD à la place du marqueur `?DIGEST` (il vise les URLs des assets).  L’idée est de proposer une sorte de *cache busting*.  Ce plugin n'intervient d'ailleurs qu'en mode production (ou plus exactement que lorsque le réglage `optimize` est actif).

Nous allons faire une variation de ça : un plugin qui remplace à la volée, au fil des compilations, un marqueur libre.  Notre **spécification fonctionnelle** serait la suivante :

  * Le **marqueur** est `!GIT-SHA!` par défaut, mais la partie entre les points d'exclamation doit pouvoir être **configurée** via `plugins.gitSHA.marker`.
  * La transformation se fait **à tout moment, à la volée** (builds one-shot ou *watcher*, production ou non).
  * Tous les fichiers des répertoires surveillés (quels qu'ils soient) sont concernés, sauf les « purs statiques » (ceux qui sont dans un sous-dossier `assets`).
  * Le marqueur comme les noms de dossiers surveillés doivent **pouvoir contenir des caractères spéciaux** d'expression rationnelle sans que cela pose problème.

Sur cette base, comment procéder ?  Un plugin Brunch est avant tout **un module Node**, alors commençons par créer un dossier `git-sha-plugin` et déposons-y un `package.json` approprié :

```json
{
  "name": "git-sha-brunch",
  "version": "1.7.0",
  "private": true,
  "peerDependencies": {
    "brunch": "~1.7"
  }
}
```

La partie `peerDependencies` n'est pas obligatoire (elle est même en phase de dépréciation), mais bon…  En revanche, il est communément admis que les plugins Brunch suivent les numéros de versions majeur et mineur du Brunch à partir duquel ils sont compatibles.  Donc si vous ne testez pas en-dessous de 1.7 par exemple, assurez-vous que votre version à vous démarre bien par 1.7.

Comme on n'a pas précisé de champ `main`, Node supposera que notre point d'entrée est un fichier `index.js`.  On sait qu'un plugin Brunch est un constructeur dont le `prototype` est doté de certaines propriétés, mentionnées plus haut dans cet article :

  * `brunchPlugin`, qui doit valoir `true` ;
  * `type`, `extension` ou `pattern` pour pouvoir être consulté au fil de la compilation ;
  * `compile(…)`, `lint(…)` ou `optimize(…)`, suivant le rôle ;
  * `onCompile(…)` si on veut n'être notifié qu'en fin de build (même si c'est du *watcher*)
  * `teardown(…)` si on doit faire du nettoyage lorsque Brunch s'arrête (par exemple arrêter un serveur qu'on aurait lancé dans le constructeur).

(En réalité, à part `brunchPlugin`, toutes les autres propriétés peuvent être définies dynamiquement sur l'instance produite par le constructeur, mais c'est rarement le cas, sauf pour `pattern`.)

Nous allons donc partir du squelette suivant dans `index.js` :

```javascript
"use strict";

// Marqueur par défaut.  Peut être configuré via `plugins.gitSHA.marker`.
var DEFAULT_MARKER = 'GIT-SHA';

function GitShaPlugin(config) {
  // 1. Construire le `pattern` en fonction de la config

  // 2. Définir la regexp du marqueur en fonction de la config
}

// Indique à Brunch qu’on est bien un plugin
GitShaPlugin.prototype.brunchPlugin = true;

// Callback de compilation à la volée (fichier par fichier) ; suppose
// que Brunch a fait la correspondance avec notre `type`, `extension` ou
// `pattern`.
GitShaPlugin.prototype.compile = function processMarkers(params, callback) {
  // Zéro transfo pour le moment
  callback(null, params);
};

// Utilitaire : échappe tout caractère spécial de regex
function escapeRegex(str) {
  return String(str).replace(/[-[\]{}()*+?.,\\^$|#\s]/g, '\\$&');
}

// Le plugin est l’export par défaut du module
module.exports = GitShaPlugin;
```

OK, commençons par le constructeur.  On n'est pas spécialisés sur un type de fichier (scripts, styles ou templates), donc pas de propriété `type` sur notre prototype.  Et on n'est pas limités à une extension, donc pas de propriété `extension` non plus.  Il va nous falloir un `pattern`, qui est une [expression rationnelle](http://www.js-attitude.fr/2012/08/13/enfin-maitriser-les-expressions-rationnelles/).

Comme celui-ci dépend des chemins, pas des extensions, il a besoin de la configuration, et sera créé dynamiquement à partir de ça.  Le code sera celui-ci, au début du constructeur :

```javascript
var pattern = config.paths.watched.map(escapeRegex).join('|');
pattern = '^(?:' + pattern + ')/(?!assets/).+';
this.pattern = new RegExp(pattern, 'i');
```

Ainsi, les chemins par défaut (`['app', 'vendor', 'test']`) donneront l'expression suivante : `/^(?:app|vendor|test)\/(?!assets\/).+/i`.

À présent le marqueur.  Le code sera un poil plus simple :

```javascript
var marker = (config.plugins.gitSHA || {}).marker || DEFAULT_MARKER;
this.marker = new RegExp('!' + escapeRegex(marker) + '!', 'g');
```

On est sûrs que `config.plugins` existe, même s’il est un objet vide.  Du coup sa propriété `gitSHA` pourrait être `undefined` d'où le `|| {}` pour obtenir un objet vide dans ce cas.  On y choppe `marker`, là aussi potentiellement `undefined`, ce qui nous amènerait de toutes façons à `DEFAULT_MARKER`.  Mais si la clé de configuration est là, on prend.

Et on construit une bonne fois pour toutes la regexp correspondante.

À présent, chaque fois que `compile(…)` sera appelée (ce qui sous-entend qu'on est bien sur un fichier qui nous concerne, d'après notre `pattern`), il nous va falloir récupérer le SHA du HEAD Git en vigueur, et procéder au remplacement dans le contenu en mémoire pour le fichier.

On ne récupère pas le SHA une seule fois au démarrage, car il est fréquent qu'on committe au fil du dev, sans arrêter le *watcher* Brunch pour autant, et du coup la valeur deviendrait obsolète au fil du temps.

Cette récupération se fait en exécutant un `git rev-parse --short HEAD` en ligne de commande, ce qui, pour être propre et fidèle à l’esprit Node, est fait en asynchrone.  On utilisera donc une fonction de rappel, en prenant soin de transmettre l'erreur éventuelle (genre, tu n'es pas dans un dépôt Git).

Voici notre petite fonction utilitaire :

```javascript
function getSHA(callback) {
  exec('git rev-parse --short HEAD', function(err, stdout) {
    callback(err, err ? null : stdout.trim());
  });
}
```

Et maintenant, à nous la transformation à proprement parler :

```javascript
GitShaPlugin.prototype.compile = function processMarkers(params, callback) {
  var self = this;
  getSHA(function(err, sha) {
    if (!err) {
      params.data = params.data.replace(self.marker, sha);
    }
    callback(err, params);
  });
};
```

Et hop !

Pour **tester notre plugin sans pourrir npm**, nous allons faire ce qu'on appelle un `npm link` : l'installation en local d'un module en cours de développement.

Si vous avez récupéré le [dépôt d'exemple](https://github.com/deliciousinsights/brunch-article-demos), vous avez parmi les dossiers :

  * `6-templates`, le dernier où on ne jouait pas avec un serveur custom, et
  * `8-git-sha-plugin`, qui contient le code ci-dessus, dûment commenté.

Voici comment faire :

  1. Allez dans `8-git-sha-plugin` depuis la ligne de commande ;
  2. Faites un `npm link` : ça va enregistrer le dossier courant comme source des futurs `npm link git-sha-plugin` ;
  3. Allez dans `6-templates` depuis la ligne de commande ;
  4. Faites un `npm link git-sha-plugin` : ça va vous l’installer (si on veut) en se basant sur le dossier source ;
  5. Ajoutez tout de même (`npm link` ne le fait pas) votre nouveau module local dans le `package.json`, sans oublier la virgule à la fin de la ligne précédente :

```json
{
  "name": "simple-brunch",
  "version": "0.1.0",
  "private": true,
  "devDependencies": {
    "brunch": "^1.7.20",
    "jade-brunch": "^1.8.1",
    "javascript-brunch": "^1.7.1",
    "sass-brunch": "^1.8.9",
    "git-sha-brunch": "^1.7.0"
  }
}
```

Sans ça, Brunch ne le verra pas (il itère sur `package.json`, par sur le contenu de `node_modules`).

Si vous n'avez pas récupéré le dépôt par un `git clone`, vous n'êtes pas dans un dépôt Git.  Si vous avez Git d'installé, voici comment obtenir un HEAD vite fait :

```sh
$ git init
Initialized empty Git repository in …/6-templates/.git/
$ git commit --allow-empty -m "Initial commit"
[master (root-commit) 8dfa8d9] Initial commit
```

(Bien évidemment, vous n'aurez pas le même SHA.)

Une fois prêts, ouvrez par exemple, dans ce même dossier, `app/application.js`, et rajoutez à un ou deux endroits un commentaire du style `// Version: !GIT-SHA!`.  Sauvez.  Lancez le build.  Puis regardez le contenu du module en bas de `public/app.js` : le SHA a remplacé le marqueur.  Vous pouvez essayez pendant que le *watcher* tourne, aussi : ça marche ! ᕙ(⇀‸↼‶)ᕗ

----

« Précédent : [Serveur web, intégré ou personnalisé](chapter10-web-server.md) • Suivant : [Conclusion](chapter12-conclusion.md) »
