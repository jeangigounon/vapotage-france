# Vapotage France — carte et observatoire communal

Deux modèles prédictifs communaux, sur une même carte interactive.

**1. Vapotage.** Taux de vapotage quotidien des 18-75 ans pour chaque commune
française (34 852 communes, 45 arrondissements municipaux, 16 086 quartiers IRIS),
avec profils de vapoteurs et fourchettes d'incertitude. Estimation sur petits
domaines : enquêtes nationales (EROPP 2023, Baromètres SpF 2021/2024) × recensement
INSEE, calibration régionale multi-vagues avec partial pooling.

**2. Propension à l'action protestataire.** Indice 0-100 de propension à signer une
pétition ou participer à une manifestation, aux mêmes échelles. Pondérations
dérivées de la méta-analyse Boulianne & Earl (*Science Advances*, 2026, 181 études),
ancrage français sur l'enquête TeO2 (INSEE-INED), et intégration du vote de gauche
observé (présidentielle 2022). Méthodologie et limites dans
`docs/indice_participation_politique.md`.

Méthodologie complète des deux modèles dans `docs/`.

## Utilisation

Les pages chargent des fonds de carte détaillés via `fetch()` : il faut les servir
par HTTP (pas d'ouverture directe en `file://`).

```bash
python -m http.server 8000
# puis http://localhost:8000  (carte)  et  http://localhost:8000/observatoire.html
```

- **`index.html`** — carte interactive à 4 niveaux : France → département →
  commune → quartiers IRIS (arrondissements pour Paris/Lyon/Marseille). Les
  contours haute définition (`geo/`, ~100 m communes, ~30 m IRIS) se chargent au
  clic, avec repli automatique sur les contours embarqués. Le sélecteur permet de
  colorer la carte par indicateur (taux, motifs d'usage, indice protestataire) ou
  de croiser deux indicateurs ; la fiche complète du territoire sélectionné
  s'affiche sous la carte.
- **`observatoire.html`** — fiche détaillée par commune : taux, fourchette à 90 %,
  profils des vapoteurs (sexe, âge, diplôme, situation, statut tabagique),
  classements. Lien direct vers une commune : `observatoire.html#c=35238`.
- **`data/`** — les estimations complètes en CSV (`;`, UTF-8 BOM).
- **`docs/`** — méthodologie, sources, validations, revue de littérature.

## Lecture des estimations

Les taux sont des espérances conditionnelles à la composition sociodémographique,
calibrées sur les prévalences régionales observées. Les écarts entre régions et
départements sont validés (enquêtes + signaux de marché) ; les écarts fins entre
communes voisines sont des profils de risque, à lire avec les fourchettes
`taux_p05`–`taux_p95` (largeur médiane ±0,6 point).

Sources : OFDT/Santé publique France (EROPP 2023, Baromètres 2021 et 2024),
INSEE (RP2022/2023, Filosofi), contours IGN/INSEE. Données d'enquête publiques ;
estimations produites par modélisation — septembre 2026.
