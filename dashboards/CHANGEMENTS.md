# Dashboard Maison : récapitulatif des changements

## 🐛 Bugs corrigés

| Où | Problème | Correction |
|---|---|---|
| Pop-up Mollye | Les chips batterie et GPS lisaient le téléphone de **Cyril** (`device_tracker.cdupret_…`), à cause d'un copier-coller | Elles lisent maintenant `device_tracker.cph2415` |
| Boutons Cyril/Mollye | `.bubble-icon-container` était déclaré 2 fois : le 2e bloc écrasait le 1er, donc la pastille verte/bleue (maison/absent) ne s'affichait jamais | Les deux blocs sont fusionnés en un seul style, partagé via une ancre YAML |
| En-tête Accueil | Le `#` était seul sur sa ligne : on obtenait un H1 vide et la salutation en texte normal, donc le style `h1` n'était jamais appliqué | La salutation est sur une seule ligne `# …`, avec la date en français en dessous |
| Plafonniers (Salon) | La condition de visibilité n'avait pas d'`entity` : elle était invalide et, si elle fonctionnait, on ne pourrait plus rallumer la lampe depuis la carte | La condition est retirée |
| Vue Voiture | La vue ne contenait que le pop-up `#peugeot` : la page restait vide | Ajout d'une carte « Peugeot 208 » (carburant + état du verrouillage) qui ouvre le pop-up |
| Voiture, conditions | Ancien format `entity/state` utilisé à l'intérieur d'un `condition: or` (pas fiable) | Passage au format moderne `condition: state` ; `state_not` en liste (`unknown`/`unavailable`) |
| Chambre | Une `mushroom-chips-card` vide laissait un trou dans la grille | Supprimée |

## ✨ Améliorations

- **Navbar** : l'onglet « Voiture » est masqué pour les utilisateurs qui n'ont pas accès à la vue. Le menu « ⋮ » a maintenant des libellés, et la confirmation de redémarrage est en français.
- **Salon** : ajout de badges explicites **TV** (télécommande) et **Clim** (le ventilo n'avait pas de texte). L'ouverture de la télécommande n'est plus réservée au tap « caché » sur le titre.
- **Hash** `#ventilafion` remplacé par `#ventilation`.
- **Pop-ups personnes** : Mollye a maintenant les mêmes chips que Cyril (vitesse et dernière mise à jour). L'icône et la couleur changent selon que la personne est à la maison ou non. La vitesse gère `velocity` (iOS) et `speed` (Android). On affiche `?` au lieu de `None` si un attribut manque.
- **Couleur rose** pour Mollye sur la carte et dans la fiche de position.
- **Salle de bain** : le badge du titre affiche le temps restant, et seulement quand la machine tourne.
- **Imprimante 3D** : bouton d'alimentation ajouté dans le pop-up, puisque le tap sur la carte ouvre le pop-up et qu'il fallait un appui long pour l'allumer.
- **To-do** : l'en-tête affiche le nombre de tâches en attente.
- **Templates Jinja** simplifiés : ternaires `'a' if … else 'b'`, `expand('binary_sensor.group_…')` direct.
- **Ancres YAML** (`&nom` / `*nom`) pour ne plus dupliquer :
  - le style des boutons personnes
  - le style des tuiles de la voiture (une quinzaine de copies)
  - la carte du ventilo et la télécommande TV, réutilisées dans « Labo »
  - les options de carte figées
- **Vue « Calendrier » renommée « Labo »** (`/dashboard-maison/labo`). Elle ne contenait aucun calendrier, seulement des brouillons. Elle est maintenant visible uniquement par l'utilisateur `de1fc08b…`.

## ⚠️ À vérifier de ton côté

1. **`hidden` sur la route Voiture (navbar-card)** : cette option utilise un template JS avec `user.id`. Je ne suis pas sûr à 100 % que ta version de navbar-card l'accepte. Si l'onglet disparaît pour tout le monde, supprime simplement le bloc `hidden:`.
2. **Chip Lumières → `/lovelace/lumieres`** : ce lien pointe vers un **autre** dashboard. Si la vue n'existe pas, le lien mène à une page vide. Je n'y ai pas touché.
3. **Vue Labo** : si `de1fc08b…` n'est pas ton compte, tu ne la verras plus. Corrige l'ID ou retire `visible:`.
4. **Ancres YAML** : HA les accepte dans l'éditeur YAML brut, mais les « déplie » à l'enregistrement. Garde le fichier du repo comme source de vérité.
