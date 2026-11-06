# Site Réel MK (reelmk.com)

Site statique publié par Netlify à chaque push sur `main`. Contenu dans `content/site.json`, médias publics dans `uploads/`, admin Decap CMS dans `admin/` (reelmk.com/admin).

## Traiter les « Envois à Claude »

Le propriétaire dépose ses enregistrements depuis l'admin (collection « 📤 Envois à Claude »). Chaque envoi = un fichier `envois/<date>-<slug>.json` (title, date, fichiers[], destination, note, statut, retour) ; les fichiers bruts sont dans `envois/fichiers/` (bloqués en public par `netlify.toml`).

Quand il demande de « traiter ses envois » :
1. `git pull`, puis lire les envois dont `statut` = « À traiter ».
2. Pour chaque fichier, suivre `note` et `destination`. Vidéos : ffmpeg en H.264 MP4, `-preset veryfast -crf 23 -movflags +faststart`, sans son si c'est une vidéo de fond, 1080 px max sur le petit côté, viser < 15 Mo. Écrire le résultat dans `uploads/` (nom court, minuscules, sans espaces).
3. Mettre à jour `content/site.json` (chemins en `/uploads/…`). Garder la structure existante (8 projets, 4 services, 4 étapes) sauf demande contraire.
4. Dans l'envoi : `statut` → « En ligne », `retour` → une phrase sur ce qui a été fait. Ne pas supprimer les fichiers bruts sans demande.
5. Un commit clair, push sur `main`, puis vérifier que reelmk.com affiche la nouvelle version.

## Zip « Exporter pour Netlify » (version Claude du site)

Le zip contient `content/site.json` + seulement les nouveaux médias dans `uploads/` (les fichiers déjà en ligne sont référencés directement). Copier ces deux dossiers dans le dépôt (compresser les vidéos comme ci-dessus), pousser. Si `MANQUANTS.txt` existe, garder l'ancienne valeur pour ces champs. Après chaque mise en ligne, mettre à jour la table `online` (id d'asset → `/uploads/…`) dans la page de l'artefact Réel MK, pour que l'export suivant ne retélécharge pas ces fichiers.
