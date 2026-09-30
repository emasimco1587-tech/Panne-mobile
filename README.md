# Carte des pannes réseaux mobiles (Orange · SFR · Free · Bouygues)

Page web qui affiche sur une carte de France tous les sites mobiles déclarés
hors service par les 4 opérateurs, à partir de leurs fichiers CSV officiels.

## Mise en place (GitHub Pages) — 5 minutes

1. Crée un dépôt GitHub (ex. `pannes-mobiles`) et envoie-y tout le contenu de ce dossier,
   **y compris le dossier caché `.github/`**.
2. Dans le dépôt : **Settings → Pages** → Source : *Deploy from a branch* → branche `main`, dossier `/ (root)`.
3. **Actions → "Mise à jour des pannes opérateurs" → Run workflow** pour faire le premier téléchargement
   (ensuite il tourne tout seul toutes les 3 heures).
4. Ouvre `https://<ton-compte>.github.io/pannes-mobiles/`.

## Comment ça marche

- `.github/workflows/maj-donnees.yml` télécharge les 4 CSV dans `data/` et les enregistre dans le dépôt
  seulement s'ils ont changé. Si un opérateur ne répond pas, l'ancien fichier est conservé.
- `index.html` lit `data/*.csv`, place chaque site sur la carte et se recharge toutes les 30 min.
  S'il ne trouve pas de copie locale, il tente le lien direct de l'opérateur (souvent bloqué par le navigateur).
- Bouton **Importer CSV…** : permet aussi de charger un fichier téléchargé à la main (utile pour tester en local).

## Lecture de la carte

- Couleur = opérateur · point plein = voix **et** data coupées · anneau = coupure partielle ou dégradée
  · carré = maintenance programmée.
- Si un fichier n'a pas de coordonnées GPS, le site est placé au centre de sa commune (code INSEE).

## Réglages

En haut du script de `index.html` : liste `SOURCES` (liens des fichiers) et `AUTO_REFRESH_MIN`.
Fréquence de téléchargement : ligne `cron` du workflow (heure UTC).
