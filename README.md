# Mes outils

https://darielsc28.github.io/toolKitTL/

Une page d'accueil personnelle, en un seul fichier HTML, pour regrouper et lancer ses outils et projets hébergés sur GitHub Pages (ou ailleurs). Aucune dépendance, fonctionne hors-ligne, et tout se configure depuis un panneau de paramètres intégré.

✦ Vibe codé par DSC ✦

## Aperçu

`index.html` affiche tes liens sous forme de cartes regroupées par sections, avec un thème clair ou sombre. Un panneau de paramètres (icône ✦ en haut à droite) permet de tout personnaliser visuellement : titre, couleurs, sections, et liste d'outils — sans toucher au code.

La page n'utilise **aucun stockage de navigateur** (pas de `localStorage`). Les réglages que tu fais dans le panneau servent à générer un bloc de configuration JSON que tu colles ensuite dans le fichier pour les rendre permanents. C'est ce qui garde l'outil portable et versionnable proprement sur Git.

## Fonctionnalités

- Page unique, autonome, sans dépendance externe ni connexion réseau requise.
- Deux thèmes prédéfinis (**Jour** clair, **Nuit** sombre) plus un mode **Personnalisé** où sept couleurs prioritaires sont réglables ; les couleurs secondaires (fond, ombres) sont dérivées automatiquement.
- Sections d'outils créables, renommables, réordonnables.
- Réorganisation des outils par glisser-déposer, y compris d'une section à l'autre.
- « Filet de sécurité » : supprimer une section ne détruit pas ses outils, ils basculent dans une zone **Sans section**.
- Base d'URL globale, surchargeable par outil, ou lien externe complet (voir plus bas).
- Configuration entièrement éditable depuis l'interface, exportée en JSON à coller dans le fichier.

## Installation sur GitHub Pages

1. Crée un dépôt (par exemple `mon-portail`) et dépose `index.html` dedans.
2. Dans les **Settings** du dépôt → **Pages**, choisis la branche à publier (souvent `main`) et le dossier racine.
3. Après quelques instants, ta page est en ligne à une adresse du type `https://TON_PSEUDO.github.io/mon-portail/`.
4. Ouvre la page, clique sur ✦ et configure-la.

## Configuration

Toute la configuration vit dans un bloc en haut du fichier :

```html
<script id="config" type="application/json">
{ ... }
</script>
```

Tu n'as pas besoin de l'éditer à la main : le panneau de paramètres le fait pour toi. Mais en voici la structure pour référence.

### Structure du JSON

| Champ | Type | Rôle |
|-------|------|------|
| `titre` | texte | Titre affiché en haut de page et dans l'onglet. |
| `sous_titre` | texte | Ligne secondaire sous le titre. |
| `theme` | `"jour"` \| `"nuit"` \| `"custom"` | Thème actif au chargement. |
| `couleurs` | objet | Couleurs personnalisées (utilisé quand `theme` vaut `custom`). Vide sinon. |
| `base` | texte | Préfixe d'URL appliqué par défaut à chaque lien. |
| `sections` | liste | Sections, chacune avec un `nom` et une liste d'`outils`. |
| `sans_section` | liste | Outils sans section (filet de sécurité). |

Chaque outil est un objet :

```json
{ "nom": "Outil 1", "desc": "Description courte", "url": "mon-repo", "base": "" }
```

| Champ | Rôle |
|-------|------|
| `nom` | Titre de la carte. |
| `desc` | Description courte affichée sous le nom. |
| `url` | Nom du repo (ou chemin) ajouté après la base. |
| `base` | Optionnel. Username/préfixe spécifique à cet outil. Absent ou vide = base globale. |

### Comment l'URL d'un lien est construite

Le champ **base** de chaque outil décide de la destination :

- **Vide** → le lien est `base globale` + `url`.
  Exemple : base globale `https://moncompte.github.io/` + url `mon-repo` → `https://moncompte.github.io/mon-repo`.
- **Un username / préfixe** → ce préfixe remplace la base globale, puis `url` est ajouté.
  Pratique pour pointer vers le dépôt d'un autre compte sans changer ta base par défaut.
- **`-` (un simple tiret)** → le champ `url` est traité comme une **URL complète** et utilisé tel quel.
  Exemple : base `-` + url `https://www.youtube.com/` → ouvre directement YouTube.
  Quand tu tapes `-`, le champ suivant t'invite d'ailleurs à coller une URL complète.

## Utilisation du panneau de paramètres

Clique sur l'icône **✦** en haut à droite. Le panneau s'ouvre en superposition.

- **En-tête** : titre, sous-titre, et l'URL de base globale.
- **Sections & outils** : ajoute/renomme/réordonne les sections, ajoute des outils, glisse-dépose les outils (y compris entre sections). Supprimer une section renvoie ses outils dans « Sans section ».
- **Thème** (en bas) : choisis Jour, Nuit ou Personnalisé. Un **double-clic** sur Jour ou Nuit copie ses couleurs dans Personnalisé, prêtes à être retouchées.

En bas du panneau :

- **Appliquer et fermer** — applique les changements en aperçu et referme.
- **Appliquer** — applique en aperçu sans fermer.
- **Copier le JSON** — copie la configuration complète dans le presse-papier.
- **Annuler** — annule les changements non appliqués.

Le panneau se ferme uniquement via la croix ✕ ou la touche **Échap** (pas en cliquant à côté, pour éviter les sorties accidentelles).

### Rendre les changements permanents

Les réglages faits dans le panneau ne survivent pas au rechargement tant qu'ils ne sont pas écrits dans le fichier. Pour les conserver :

1. Clique sur **Copier le JSON**.
2. Ouvre `mes_outils.html` dans un éditeur de texte.
3. Remplace le contenu du bloc `<script id="config" type="application/json"> ... </script>` par ce que tu as copié.
4. Sauvegarde, puis commit/push sur ton dépôt.

## Personnalisation des couleurs

En mode **Personnalisé**, sept couleurs sont réglables (fond, cartes, bordures, texte, texte atténué, accent du titre/liens, accent des sections), via sélecteur visuel ou code hexadécimal. Les couleurs secondaires sont calculées automatiquement à partir de celles-ci, et le clair/sombre est détecté selon la luminosité du fond.

## Compatibilité

Fonctionne dans tout navigateur moderne. Le glisser-déposer des outils repose sur l'API native de drag & drop : pleinement opérationnel à la souris sur ordinateur. Sur tactile pur, le glissement peut ne pas être pris en charge selon le navigateur.

## Licence

Projet personnel. Réutilise et adapte librement.
