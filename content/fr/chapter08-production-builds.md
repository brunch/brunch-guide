# Builds de développement et de production

Ceci fait partie du [Guide de Brunch.io](README.md).

Par défaut, Brunch s'exécute en environnement de **développement**.  Cela signifie notamment que les plugins de minification (JS ou CSS) ne sont pas appliqués (le réglage `optimize` vaut par défaut `false` en environnement de développement ; on peut bien sûr le changer).

L’environnement est précisé en ligne de commande, par l’option `-e` ou `--env`, à laquelle on passe le nom de l’environnement.  On peut avoir tous les environnements qu’on veut, sachant que `production` est une valeur prédéfinie (avec son option dédiée, `-P` ou `--production`, qui équivaut à `--env production`), notamment parce qu’elle recale par défaut `optimize` à `true`.

Mais surtout, on peut **modifier les réglages pour des environnements précis**, au moyen de la clé racine `overrides`.  On précise en-dessous le nom de l'environnement, et à l'intérieur de celui-ci, des remplacements pour tout réglage où on le juge nécessaire.  La doc officielle donne un exemple pas mauvais :

```coffeescript
overrides:
  production:
    optimize: true
    sourceMaps: false
    plugins: autoReload: enabled: false
```

Dans cet exemple, en production on active les minifieurs, on vire les source maps (moi je les garde, mais bon…), et on désactive le plugin `auto-reload` (fonction du style LiveReload) si on *watche* en mode production.

----

« Précédent : [Adapter Brunch à un projet existant](chapter07-using-brunch-on-legacy-code.md) • Suivant : [*Watcher*](chapter09-watcher.md) »
