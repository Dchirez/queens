# Queens

Puzzle logique jouable dans le navigateur, en JavaScript pur et sans dépendance.
Chaque grille est générée à la volée avec la **garantie d'une solution unique**.

▶ **[dchirez.fr/queens/](https://dchirez.fr/queens/)**

## Règle

Placer exactement une reine par **ligne**, par **colonne** et par **région colorée**.
Deux reines ne peuvent jamais se toucher, même en diagonale — en revanche, contrairement
aux échecs, elles peuvent partager une diagonale si elles ne sont pas côte à côte.

Trois difficultés : 8×8, 9×9, 10×10.

## Génération des grilles

Le cœur du projet. Une grille se construit en trois temps :

1. **Tirage d'une solution.** Backtracking aléatoire sur les colonnes, avec la
   contrainte d'écart `|colonne(l) − colonne(l−1)| ≥ 2` qui interdit deux reines
   adjacentes sur des lignes consécutives.
2. **Croissance des régions.** Chaque reine devient la graine d'une région, que l'on
   fait grandir case par case sur ses voisines libres. Les graines reçoivent des poids
   aléatoires : des régions de tailles très inégales contraignent davantage le plateau.
3. **Réparation jusqu'à l'unicité.** Un solveur énumère les solutions. Tant qu'il en
   existe une autre que la vraie, on prend l'une de ses cases-reines et on la donne à
   une région voisine : cette solution parasite devient invalide (deux de ses reines
   partagent désormais une région) sans que la vraie solution ne soit affectée. La case
   déplacée n'est jamais une case de la vraie solution, la région d'origine doit rester
   d'un seul tenant et conserver au moins deux cases.

Parmi les coups possibles, on retient celui qui laisse le moins de solutions parasites,
avec une courte liste tabou pour éviter les allers-retours. Si la réparation s'enlise,
la grille est jetée et on recommence.

Le calcul tourne dans un **Web Worker**, et la grille suivante est préparée pendant que
le joueur réfléchit : le clic sur « Nouvelle grille » est instantané, y compris en 10×10
(≈ 200 ms de calcul). Sans Worker disponible, le jeu retombe sur un calcul synchrone.

## Jouer

| Action | Effet |
| --- | --- |
| Clic | croix → reine → case vide |
| Clic droit | même cycle en sens inverse |
| Glisser | pose une série de croix |
| Flèches + Espace | navigation et pose au clavier |

Options : croix automatiques (barre les cases rendues impossibles par une reine),
indice, annulation illimitée, chronomètre et record par difficulté.

## Structure

```
index.html   jeu complet — balisage, styles et logique
ds.css       design system partagé avec dchirez.fr
```

Aucune étape de build : le dépôt est publié tel quel via GitHub Pages.
