---
name: fr-zonage-plu
description: "Analyze NYC lot zoning, FAR, height, setbacks, uses, and buildable envelope from PLUTO and the Zoning Resolution. Use for \"what can I build\"; use fr-derogations-urbanisme for relief and fr-enveloppe-constructible for 3D visualization."
allowed-tools:
  - WebFetch
  - Write
  - Read
  - Bash
  - Glob
  - Grep
---

# /as:fr-zonage-plu — Analyse du zonage français (PLU, PLUi, RNU)

Before acting, read the [host contract](../../docs/host-harness-contract.md) and this component's [declaration](host-contract.json) (`skill:fr-zonage-plu`). Charger seulement les modes applicables depuis le [catalogue partagé](../../corpus/host-contracts.json). Les déclarations sont des exigences, pas une preuve d'accès.

Use the actual host's [delivery route](../../docs/host-adapters.md).

Read the [shared applicability contract](../../docs/geographic-applicability.md) before applying any rule.

## Exécution native et sorties

Suivre cette procédure avec les outils disponibles. Pour un rapport enregistré, appliquer la [séquence de mutation native](../../docs/workspace-model.md#native-mutation-sequence) et le [contrat de complétion](../../docs/completion-reporting.md). Une réponse dans le chat ne nécessite aucun fichier.

## Applicabilité géographique

Résoudre la parcelle française visée, la date d'analyse, l'usage projeté et le périmètre des travaux avant d'appliquer une règle. Ne jamais appliquer les règles d'une commune à une autre, ni les règles nationales à la place d'un document local.

## Flux de travail

### 1. Identifier la parcelle et la version du document

Accepter une adresse complète ou un identifiant cadastral (INSEE, section, numéro). Retrouver la parcelle via l'API Carto du cadastre. Vérifier ensuite le document d'urbanisme applicable sur le Géoportail de l'urbanisme (GPU) : PLU, PLUi, carte communale, PSMV ou RNU (si la commune n'a pas de document). Noter la date d'approbation et la date d'application. Ne pas utiliser une copie datée sans vérifier qu'elle est la version en vigueur.

### 2. Récupérer le zonage et le règlement applicable

Obtenir la zone (U, AU, A, N ou leurs sous-secteurs) et les prescriptions surfaciques, linéaires ou ponctuelles via l'API Carto urbanisme. Le règlement écrit reste opposable : un PDF publié sur le GPU ou disponible en mairie ou en préfecture. Une donnée SIG n'a pas de valeur réglementaire et doit le dire.

### 3. Interpréter et calculer à partir de données vérifiées

Séparer les règles extraites du règlement, les hypothèses du projet et les résultats calculés. Ne calculer que si la règle et les données du site sont établies (hauteur, emprise, retraits, COS hors champ). Ne jamais cumuler des règles de zones ou de servitudes sans que le règlement le permette. Une servitude d'utilité publique (SUP) doit être listée séparément.

### 4. Présenter le résultat

Présenter la parcelle, la commune, la version du document, la zone, les contrôles et leur source, les calculs et les points non résolus. Un tableau type : `Contrôle | Valeur ou statut | Article ou section du règlement | Donnée du site ou calcul`.

Pour une demande de visualisation, transmettre le polygone, les unités (mètres), les contrôles vérifiés et les entrées non résolues à `/as:fr-enveloppe-constructible`. Ne jamais compléter une valeur manquante par zéro.

## Disclaimer et marqueur final (obligatoire)

Chaque rapport de ce skill, affiché ou enregistré, doit se terminer par le bloc de disclaimer canonique de `rules/professional-disclaimer.md`, suivi d'une ligne vide puis du marqueur :

```markdown
> **Disclaimer:** This is an AI-generated analysis for preliminary planning purposes. All findings must be verified by a licensed professional before use in design, permitting, or regulatory submissions.

<!-- architecture-studio:requires-disclaimer -->
```

Le marqueur n'apparaît qu'une fois, en dernière ligne. Vérifier le texte final avant de terminer.
