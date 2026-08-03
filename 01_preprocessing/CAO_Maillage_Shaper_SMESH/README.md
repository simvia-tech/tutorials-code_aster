# CAO et maillage dans salome_meca : SHAPER & SMESH — Simvia web edition

A Modified Version of **"CAO et maillage dans salome_meca : SHAPER & SMESH"**,
*code_aster / salome_meca* course material authored and published by **EDF S.A.**
under the GNU Free Documentation License. The content and the figures are the
original course, unchanged; only the layout is new.

**Authors:** EDF S.A. — Gérald NICOLAS, Soizic PERON, EDF R&D PERICLES (the
original course material); [Simvia](https://simvia.tech/fr) (the modifications).

**Publisher of this Modified Version:** Simvia.

The original course material carries the notice `Copyright 2021 EDF`, preserved
here as the license requires. Copyright © 2026 Simvia, for the modifications.

Permission is granted to copy, distribute and/or modify this document under the
terms of the GNU Free Documentation License, Version 1.3 or any later version
published by the Free Software Foundation; with no Invariant Sections, no
Front-Cover Texts, and no Back-Cover Texts. A copy of the license is included in
the section entitled "GNU Free Documentation License".

---

The course runs in five parts, in the order the original deck presents them:

| | Part | What it covers |
|---|---|---|
| 1 | [Comment faire un maillage ?](#part-1--comment-faire-un-maillage-) | the three steps, and the module that carries out each one |
| 2 | [SHAPER](#part-2--shaper) | the CAD module: principles, conformity, reuse, tests, documentation |
| 3 | [SMESH](#part-3--smesh) | the meshing module: what it does, what it needs, which mesher runs |
| 4 | [Exercice n°1](#part-4--exercice-n1) | a quarter of a perforated plate, meshed in quadratic triangles |
| 5 | [Exercice n°2](#part-5--exercice-n2) | an elbowed pipe, meshed in linear hexahedra |

---

## Part 1 · Comment faire un maillage ?

> **In this part**
>
> *Original deck: slide 2.*

- Créer la CAO du domaine de calcul avec le module SHAPER
- La transférer au module SMESH
- Appliquer les différents algorithmes possibles en 1D, 2D, 3D

<p align="center">
  <img src="FIGURES/salome_modules_toolbar.png"
       alt="Screenshot of the SALOME 9.4.0 window with its Fichier, Edition, Affichage, Outils, Fenêtre and Aide menus and a module selector reading SALOME. Four module buttons in the toolbar are circled and labelled by arrows: Module de CAO Shaper and Module de maillage SMESH in dark red boxes, Module de géométrie GEOM and Module de visualisation ParaViS in blue boxes."
       width="840"/>
  <br>
  <em>Slide 2 — the four modules the course moves between, in the toolbar.</em>
</p>

<sub>[▲ back to the five parts](#cao-et-maillage-dans-salome_meca--shaper--smesh--simvia-web-edition)</sub>

---

## Part 2 · SHAPER

> **In this part**
> [Les grands principes](#les-grands-principes) ·
> [Une CAO pour la simulation numérique](#une-cao-pour-la-simulation-numrique) ·
> [Redessiner une CAO existante](#redessiner-une-cao-existante) ·
> [Modifier une CAO existante](#modifier-une-cao-existante) ·
> [Les tests](#les-tests) ·
> [Documentation](#documentation)
>
> *Original deck: slides 3–8.*

### Les grands principes

- Paramétrique et variationnel
  - Assemblage de pièces en 3D
- Dessin à l'IHM privilégié, ce qui n'empêche pas le scripting python
- Dédié à la CAO pour la simulation numérique

**Variationnel :**

1. Dessin interactif sur des esquisses 2D :
   - Similaire aux pratiques de dessin industriel
   - Application de contraintes : parallélisme, distance, coïncidence…

**Paramétrique :**

2. Mise en volume :
   - par extrusion, révolution, balayage
   - puis opérations booléennes…
   - Chaque paramètre peut être édité et modifié
   - La forme est automatiquement mise à jour

<p align="center">
  <img src="FIGURES/shaper_parametric_sketch.png"
       alt="A SHAPER sketch on a pale blue background: a tall narrow closed contour drawn in green with a broken corner at the top right, covered in blue dimension arrows and values such as 1.03, 0.284, 0.6, 2.705, 2.243, 0.0075, 15 and 0.225, with H and V letters marking the horizontal and vertical constraints and yellow dots marking coincidences."
       width="420"/>
  <br>
  <em>Slide 3 — a fully constrained sketch, every dimension a parameter.</em>
</p>

### Une CAO pour la simulation numérique

- Groupes
  - points, arêtes, faces, solides
- Conformité des formes :
  - géométrie non manifold (ex : arête partagée par plus de 2 faces)
  - partitionnement
  - multi-dimensionnel connecté
  - géométrie maillable « conforme »
- Scripting python

<p align="center">
  <img src="FIGURES/shaper_nonmanifold.png"
       alt="Two views of non-manifold geometry. On the left, four dark green quadrilateral faces meet along a single edge drawn in magenta, with a small circle at each end of that edge. On the right, on a dark grey background, four pale grey triangular faces meet at a single point where the coloured axes cross."
       width="620"/>
  <br>
  <em>Slide 4 — non manifold geometry: an edge shared by more than two faces.</em>
</p>

<p align="center">
  <img src="FIGURES/shaper_conformity.png"
       alt="Four views in a row. First, on black, a blue triangular mesh over a flat plate with a dome of fine white mesh rising from it. Second, a grey and lilac structural model of a three-storey corner building on red footings. Third, on black, three pairs of grey cubes: the first pair touching face to face with a green tick, the second and third pairs meeting only partially, each with a red cross. Fourth, on black, an orange wireframe box inside a grey wireframe box."
       width="850"/>
  <br>
  <em>Slide 4 — partitioning, connected multi-dimensional shapes, and what makes
  a geometry conformally meshable.</em>
</p>

### Redessiner une CAO existante

- Copier/adapter le script python
- Ou re-dessiner :
  - Relever les cotes
  - Macro « sketch drawer » → projection des contours d'une face plane
  - Macro « import de points 3D »

<p align="center">
  <img src="FIGURES/shaper_sketch_drawer.png"
       alt="Screenshot of the SHAPER Sketch panel on the left, with checkboxes for Show geometrical constraints, Show dimensional constraints, Show existing expressions, Show free points and Automatic constraints, a Change sketch plane button, a line reading DoF degrees of freedom equals 44, and an Object browser tree below. To its right, an OCC viewer shows a blade root profile projected onto a pale plane, its contour traced in red and covered in blue dimension values."
       width="560"/>
  <img src="FIGURES/shaper_measurement.png"
       alt="Screenshot of the SHAPER Measurement panel with a row of measuring tool buttons, three selected revolution faces listed, and a line reading Angle equals 135. To its right, an OCC viewer shows several pale grey cylindrical blades seen close up, with an arc annotated 135 degrees between two of them."
       width="480"/>
  <br>
  <em>Slide 5 — projecting the contour of a plane face, and reading a dimension
  off an existing model.</em>
</p>

### Modifier une CAO existante

- Exemple sur l'attache d'ailette ci-dessous :
  - Import de modèle GEOM dans SHAPER (formats BREP, XAO).
  - Ajouts/modifications possibles :
    - enlèvement de matière
    - translation, rotation, symétrie, mise à l'échelle, copie (linéaire, circulaire)

<p align="center">
  <img src="FIGURES/shaper_blade_root_edits.png"
       alt="Four views of the same blade-root part on a pale blue background. First, three upright fir-tree shaped teeth seen from the front. Second, the part shown as a curved arc of teeth seen from above, with a red line running along it and one tooth highlighted in pale yellow. Third, the arc of teeth with four short green faces marking removed material. Fourth, the finished part, an arc of five teeth each pierced by a small hole."
       width="850"/>
  <br>
  <em>Slide 6 — material removed, then the part translated, rotated, mirrored,
  scaled and copied.</em>
</p>

### Les tests

- Plus de 1000 modèles de tests généraux
- Quelques dizaines de modèles EDF réels :

<p align="center">
  <img src="FIGURES/shaper_edf_test_models.png"
       alt="A collage of nine CAD models on pale blue backgrounds, each captioned on an orange label: Colis déchets MFEE, a grey open box holding a bed of small coloured rods; Té fluide paramétré, a pipe junction partitioned into many coloured blocks; Bol GV DT, a large grey dished head with a nozzle; Attache ailette DT, a blade-root bracket; Cloison internes cuve MFEE, a slender internal partition; Barrage CIH, a curved dam wall with buttresses; Stator ERMES, a dark cylindrical stator with slots; Cuve DT, a grey vessel with three nozzles; Barrage de Roselend CIH, an arch dam seen from downstream."
       width="850"/>
  <br>
  <em>Slide 7 — a few dozen of the real EDF models used as tests.</em>
</p>

### Documentation

- Le mode d'emploi est consultable pour chaque fonction.
- Des exemples de programmation python sont téléchargeables.
- Recherche possible.

<p align="center">
  <img src="FIGURES/shaper_documentation.png"
       alt="Screenshot of the SHAPER 9.5.0 documentation website. A blue sidebar holds a Search docs box and a contents list: Introduction to SHAPER, Tutorial, Part plug-in, Sketch plug-in, Construction plug-in, Build plug-in, Primitives plug-in, Features plug-in, Collection plug-in, Exchange plug-in, Python addons, Connector plug-in, Parameters plug-in, Filters plug-in, Examples of TUI scripts. The page body reads Welcome to SHAPER documentation and repeats the contents as links, with Point, Line, Rectangle, Circle, Arc, Ellipse, Elliptic Arc, B-spline and periodic B-spline, Distance constraint, Horizontal distance constraint and Vertical distance constraint nested under Sketch plug-in."
       width="840"/>
  <br>
  <em>Slide 8 — the manual for every function, with downloadable python
  examples and a search box.</em>
</p>

<sub>[▲ back to the five parts](#cao-et-maillage-dans-salome_meca--shaper--smesh--simvia-web-edition)</sub>

---

## Part 3 · SMESH

> **In this part**
> [Objectif du module](#objectif-du-module) ·
> [Fonctionnement du module](#fonctionnement-du-module)
>
> *Original deck: slides 9–13.*

### Objectif du module

- Le module SMESH :
  - Permet de mailler un domaine 1D, 2D ou 3D
  - Traduit les groupes définis dans la CAO en groupes de mailles et de nœuds
    pour le calcul
  - Gère des manipulations sur des maillages existant (symétrie, translation, etc.)
  - Propose des fonctions de contrôle de maillage
  - Contient des utilitaires spécifiques (couches limites, fissure…)

<p align="center">
  <img src="FIGURES/smesh_modules_toolbar.png"
       alt="Screenshot of the SALOME 9.4.0 window with its Fichier, Edition, Affichage, Outils, Fenêtre and Aide menus. Four module buttons are circled and labelled by arrows: Module de CAO Shaper, Module de géométrie GEOM and Module de visualisation ParaViS in blue boxes, and Module de maillage SMESH in a red box with a red arrow, marking the module this part is about."
       width="840"/>
  <br>
  <em>Slide 9 — SMESH, the meshing module, picked out in red.</em>
</p>

### Fonctionnement du module

#### Un point de départ et des directives

- Un point de départ :
  - Une géométrie au format SALOME
  - Créée par GEOM ou SHAPER
- Des directives :
  - Choix d'un algorithme
  - Définition d'hypothèses
- IHM ou python

<p align="center">
  <img src="FIGURES/smesh_bolt_geometry.png"
       alt="A dark grey CAD model of a socket-head bolt on a pale blue background, seen from above the head, showing the hexagonal recess and the plain cylindrical shank."
       width="200"/>
  <img src="FIGURES/smesh_create_mesh.png"
       alt="Screenshot of the SALOME study tree listing the group names of a screw geometry — APPUI_B, APPUI_H, BAS, BORD, BORD_APPUI, BORD_FUT, BORD_TETE, CERCLE_1 to CERCLE_9, CONGE_B_A, CONGE_B_B, CONGE_H, CRACK, CYL_APPUI, CYL_CONGE_H_1 to CYL_CONGE_H_6 and CYL_FUT — beside the Créer un maillage dialog, which has fields for Nom set to Maillage_1, Géométrie set to VIS and Type de maillage set to Tout type, a ticked box reading Créer tous les groupes définis dans la géométrie, tabs 3D, 2D, 1D and 0D, and empty Algorithme and Hypothèse fields."
       width="640"/>
  <br>
  <em>Slide 10 — a SALOME geometry and its groups go in, and a mesh object comes
  out to be qualified.</em>
</p>

#### Des fonctions de visualisation

<p align="center">
  <img src="FIGURES/smesh_bolt_mesh.png"
       alt="The same socket-head bolt, now meshed: a bright blue surface covered in dark green triangle edges, the mesh visibly finer around the fillet between head and shank."
       width="720"/>
  <br>
  <em>Slide 11 — the computed mesh, displayed in the viewer.</em>
</p>

#### Deux types de mailleurs

- Schématiquement, SMESH propose deux types de mailleurs :
  - Des logiciels externes pour les principales fonctions ; il sont intégrés dans
    SALOME par EDF et le CEA.
  - Des compléments internes à SALOME pour des fonctions spécifiques : extrusion,
    couches limites, fissure, etc.
- Logiciel externe sous licence : MeshGems
  - La suite MeshGems est issue de travaux de l'INRIA.
  - Elle est distribuée par la société Dassault Systèmes :
    [www.spatial.com/products/3d-precise-mesh](https://www.spatial.com/products/3d-precise-mesh)
  - Triangles, quadrangles libres, tétraèdres.
  - Hexaèdre libre sous forme de prototype.
  - *Remarque : accessible sans frais à tout utilisateur EDF.*
- Logiciel externe libre : NETGEN
  - La suite NETGEN : [ngsolve.org](https://ngsolve.org)
  - Triangles, quadrangles libres, tétraèdres

#### Commentaires

- Commentaires
  - La suite MeshGems est recommandée pour tous les maillages en triangles et
    tétraèdres. La plupart du temps, les mailles obtenues sont de meilleure qualité.
  - De nombreuses options existent pour affiner le maillage.
- Maillage en hexaèdre
  - Le maillage automatique en hexaèdre est disponible à titre expérimental.
  - C'est un sujet de recherche dans la communauté internationale du maillage.
  - Le maillage en hexaèdre reste donc toujours une opération difficile.
- Documentation
  - La documentation est accessible par le menu général d'aide.

<p align="center">
  <img src="FIGURES/smesh_help_menu.png"
       alt="Screenshot of the SMESH toolbars, with the menus Fichier, Edition, Affichage, Maillage, Contrôles, Modification, Outils de mesure, Outils, Fenêtre and Aide, the last of which is circled in blue, and two rows of mesh tool buttons below."
       width="780"/>
  <br>
  <em>Slide 13 — the documentation is reached from the general help menu.</em>
</p>

<sub>[▲ back to the five parts](#cao-et-maillage-dans-salome_meca--shaper--smesh--simvia-web-edition)</sub>

---

## Part 4 · Exercice n°1

> **In this part**
> [Objectif](#objectif) ·
> [Données](#donnes) ·
> [L'esquisse de la plaque](#lesquisse-de-la-plaque) ·
> [Les contraintes](#les-contraintes) ·
> [La face et les groupes](#la-face-et-les-groupes) ·
> [Le maillage : algorithme et hypothèses](#le-maillage--algorithme-et-hypothses) ·
> [Calcul et exportation](#calcul-et-exportation)
>
> *Original deck: slides 14–42.*

### Objectif

- Un quart de plaque trouée
- Objectif :
  - Maillage en triangles
  - Plus fin au bord du trou
  - Quadratique

<p align="center">
  <img src="FIGURES/ex1_target_mesh.png"
       alt="A tall rectangular plate filled with a bright blue triangular mesh outlined in black, with a quarter-circle notch cut out of its bottom left corner where the triangles become very much finer."
       width="380"/>
  <br>
  <em>Slide 14 — the mesh the exercise builds: triangles, refined at the edge of
  the hole.</em>
</p>

### Données

- Groupes d'arêtes sur les bords :
  - `gauche`, `haut`, `droite`, `bas`, `arc`
- Groupes de nœuds :
  - `A`, `B`
- Tailles de maille :
  - 15 en général
  - 1,5 sur l'arc

<p align="center">
  <img src="FIGURES/ex1_data.png"
       alt="A dimensioned drawing of the quarter plate. A green contour rises from the X axis, turns up the right-hand side, runs across the top and comes down the left-hand side to a quarter-circle arc that closes it onto the X axis. Magenta dimensions read L equals 100 across the top, H equals 150 down the right and a equals 10 for the arc radius. Blue labels name the edges haut, droite, bas, gauche and arc, and the two nodes A, at the top of the arc, and B, at its foot."
       width="500"/>
  <br>
  <em>Slide 15 — the dimensions, the edge groups and the two node groups.</em>
</p>

### L'esquisse de la plaque

#### Activer le module SHAPER, créer une nouvelle pièce

- Activer le module SHAPER
- Créer une nouvelle pièce (part)
- Elle apparaît dans le browser, avec ces rubriques, vides au démarrage :
  - `Paramètres` : ceux de la future pièce.
  - `Constructions` : les différentes esquisses, plans, axes, etc. nécessaires au
    fur et à mesure.
  - `Résultats` : le ou les dernières parties créées pour constituer la pièce.
    Cette rubrique se remplit et se vide au fur et à mesure des opérations.

<p align="center">
  <img src="FIGURES/ex1_new_part.png"
       alt="Screenshot of the SHAPER menu bar reading File, Edit, View, Part, Sketch, Construction, Build and Primitives, with Part circled in blue, three rows of toolbar buttons below it, and an Object browser panel listing Part set with Parameters (0), Constructions (7), Parts (1) and, under Part_1, Parameters (0), Constructions (0) and Results (0)."
       width="620"/>
  <br>
  <em>Slide 16 — the new part appears in the browser with its three sections
  still empty.</em>
</p>

#### Créer une esquisse

- Créer une esquisse (sketch)
- Choisir successivement :
  - La taille ; c'est une estimation pour cadrer la vue. En gros c'est le côté
    d'un carré qui contient le futur dessin.
    - *Se tromper n'est pas grave mais simplement désagréable car il faudra faire
      des recadrages en visualisation.*
  - Le plan : celui sur lequel on va dessiner. La feuille de papier en quelque
    sorte. On choisit l'un des 3 plans XOY, YOZ ou XOZ, soit en le désignant à la
    souris dans la fenêtre graphique, soit en le sélectionnant dans la liste du
    browser. Pour cet exercice, on choisit XOY.
  - On valide.
- *Remarque :*
  - *Pour des CAO 3D compliquées, on pourra choisir un plan qui est une face de
    l'objet en cours de création ou un plan que l'on aura créé spécialement. Cela
    permet de positionner l'esquisse que l'on dessine directement à sa place dans
    l'espace 3D*

<p align="center">
  <img src="FIGURES/ex1_sketch_menu.png"
       alt="Screenshot of the SHAPER menu bar with Sketch circled in blue, the toolbars below it including a row of sketch tools, and an Object browser panel listing Part set with Parameters (0), Constructions (7), Parts (1) and, under Part_1, Parameters (0), Constructions (0) and Results (0)."
       width="500"/>
  <img src="FIGURES/ex1_sketch_plane.png"
       alt="Screenshot of the SHAPER Sketch panel showing a field Size of the view set to 200 and the prompt Select a plane on which to create a sketch, beside a viewer displaying the three coordinate planes edge on as three interlocking rectangles outlined in red, green and blue."
       width="440"/>
  <br>
  <em>Slide 17 — the size of the view, then the plane the sketch is drawn on.</em>
</p>

#### Dessiner à main levée les contours rectilignes

- Dessiner à main levée les contours rectilignes
  - Respecter *grosso modo* les formes et les dimensions.
  - Se placer du bon côté des axes.
  - Pour arrêter : touche `"Echap"`
- Inutile de se forcer à être exact :
  - Les dimensions réelles seront appliquées plus tard.
  - On peut bouger ou déformer le dessin après coup pour l'ajuster, par exemple
    pour se positionner du bon côté des axes.
- Bilan
  - Il y a 10 degrés de liberté : 5 sommets x 2 coordonnées

<p align="center">
  <img src="FIGURES/ex1_freehand_outline.png"
       alt="Screenshot of the SHAPER sketch toolbar with the line tool circled in blue, above a viewer on a pale yellow ground where five red segments trace a tall rectangle with its bottom left corner left open, and below them the Sketch panel with its Reversed checkbox, Set plane view button, the checkboxes Show geometrical constraints, Show dimensional constraints and Show existing expressions, and a line reading DoF degrees of freedom equals 10."
       width="820"/>
  <br>
  <em>Slide 18 — the straight contours drawn freehand, ten degrees of freedom
  left.</em>
</p>

#### Les pièges, ou les astuces

- Les pièges (ou les astuces…)
  - Si on trace un trait « presque » verticalement, SHAPER le contraint à l'être.
  - Evidemment même chose horizontalement !
- Bilan
  - Si on laisse SHAPER fixer verticales et horizontales, il n'y a plus que
    6 degrés de liberté :
    - 2 sommets au sud-ouest x 2 coordonnées
    - Le sommet au nord-est x 2 coordonnées
  - Cela peut s'avérer très pratique. Mais cela ne correspond pas toujours à ce
    qu'on cherche au final, par exemple si on veut paramétrer l'angle de la ligne
    avec un axe.
  - On peut donc inactiver cette fonction.

<p align="center">
  <img src="FIGURES/ex1_auto_constraints.png"
       alt="Screenshot of the SHAPER Sketch panel, with an arrow pointing to a ticked Automatic constraints checkbox and a line reading DoF degrees of freedom equals 6, beside a viewer where the red contour now carries the letters V on its two vertical segments and H on its two horizontal ones."
       width="640"/>
  <br>
  <em>Slide 19 — SHAPER has fixed the verticals and horizontals by itself, and
  the count drops to six.</em>
</p>

#### Un autre piège

- Un autre piège (ou une autre astuce…)
  - Si on est observateur ;=) on remarque que la figure finale est un rectangle
    avec un coin cassé.
  - On a la tentation de le dessiner ainsi
  - Alors il n'y a plus que 4 degrés de liberté :
    - Le sommet au sud-ouest x 2 coordonnées
    - Le sommet au nord-est x 2 coordonnées
- Problème :
  - Le sommet au sud-ouest est verrouillé.
  - On peut le débloquer mais l'opération est plus longue que d'avoir dessiné en
    tenant compte de l'ouverture dès le début.

<p align="center">
  <img src="FIGURES/ex1_broken_corner.png"
       alt="Screenshot of the SHAPER Sketch panel reading DoF degrees of freedom equals 4, beside a viewer where the sketch is now a plain closed red rectangle with V on its vertical sides and H on its horizontal ones, and no corner left open."
       width="640"/>
  <br>
  <em>Slide 20 — drawn as a plain rectangle, only four degrees of freedom remain,
  but the south-west vertex is locked.</em>
</p>

#### Insérer l'arc de cercle

- Insérer l'arc de cercle, grosso modo
  - On pique le centre (clic gauche), puis la 1ère extrémité, puis la seconde,
    à peu près où ils doivent être.
  - Le sens est horaire.
  - Se placer du bon côté des axes.
- Cela ajoute 5 degrés de liberté :
  - 2 extrémités x 2 coordonnées
  - Le rayon

<p align="center">
  <img src="FIGURES/ex1_insert_arc.png"
       alt="Screenshot of the SHAPER sketch toolbar with the arc tool circled in blue, the Arc panel below it offering three arc construction modes and empty Center point, Start point and End point coordinate fields, and to the left the Sketch panel reading DoF degrees of freedom equals 15 beside a viewer where a red arc has been added near the open bottom left corner of the contour."
       width="840"/>
  <br>
  <em>Slide 21 — the arc picked centre first, then its two ends, clockwise.</em>
</p>

### Les contraintes

#### La palette de contraintes

- Les contraintes
  - Pour figer l'esquisse, SHAPER propose une palette d'actions :
  - Distance entre un point et un autre point ou une ligne
  - Distance horizontale/verticale entre un point et un autre point ou une ligne
  - Longueur
  - Angle entre deux lignes
  - Rayon
  - Rendre horizontal/vertical
  - Figer tel quel
  - Rendre parallèle/perpendiculaire/tangent
  - Faire coïncider
  - Mettre un point au milieu d'une ligne
  - Attribuer la même longueur
  - Rendre colinéaire

<p align="center">
  <img src="FIGURES/ex1_constraint_palette.png"
       alt="The SHAPER constraint palette: a pale strip holding sixteen small red and grey icons in a row, for distance, horizontal and vertical distance, length, angle, radius, make horizontal, make vertical, fix as is, parallel, perpendicular, tangent, coincident, midpoint, equal length and collinear."
       width="820"/>
  <br>
  <em>Slide 22 — the palette of constraints available to fix the sketch.</em>
</p>

#### Degrés de liberté en théorie et en pratique

- On peut comprendre théoriquement qu'il existe des degrés de liberté sous une
  forme et dans la pratique les annihiler sous une autre forme.
- Exemple :
  - Une ligne isolée a 4 degrés de liberté qui sont les coordonnées de ses
    extrémités.
    - En imposant à la ligne d'être horizontale, il n'y a plus que 3 ddl.
    - En imposant à la 1ère extrémité d'être confondue avec l'origine du repère,
      il n'y a plus qu'un seul ddl.
    - En imposant une longueur à cette ligne, il n'y a plus aucun degré de liberté.
  - → On aura ainsi figé la ligne sans jamais donner les valeurs des coordonnées
    des extrémités.

<p align="center">
  <img src="FIGURES/ex1_dof_sequence.png"
       alt="Four stacked screenshots of the same SHAPER panel and viewer, each reading Show free points and a ticked Automatic constraints above a Change sketch plane button. In the first a sloping red line gives DoF degrees of freedom equals 4. In the second the line is horizontal and marked H, giving 3. In the third its left end sits on the origin, giving 1. In the fourth it is dimensioned 100, is drawn green, and the panel reads Sketch is fully fixed, DoF equals 0."
       width="440"/>
  <br>
  <em>Slide 23 — four degrees of freedom removed one constraint at a time, the
  coordinates never given.</em>
</p>

#### Appliquer les contraintes

- Appliquer les contraintes
  - Le centre du cercle est placé sur l'origine du repère (-2 ddl).
  - Les extrémités de l'arc sont au bout des segments (-2 ddl).
  - Les extrémités de l'arc sont sur les axes (-2 ddl).
  - Les 4 segments sont horizontaux ou verticaux (-6 ddl).
- Noter :
  - Les notations H et V sur les segments bloqués.
  - Les symboles de coïncidence.
  - En bougeant le dessin, on visualise ce qui manque.
- Bilan :
  - Il reste 3 degrés de liberté : les dimensions.

<p align="center">
  <img src="FIGURES/ex1_apply_constraints.png"
       alt="Screenshot of the SHAPER Sketch panel reading DoF degrees of freedom equals 3, beside a viewer where the red contour is now closed by an arc at the bottom left, carries H and V letters on its straight segments, and shows yellow dots at the coincidences between the arc ends and the segments and the axes."
       width="700"/>
  <br>
  <em>Slide 24 — twelve degrees of freedom removed; the three dimensions
  remain.</em>
</p>

#### Les contraintes de dimension

- Les contraintes de dimension
  - La valeur actuelle s'affiche en bleu
  - La valeur voulue peut être définie de 3 façons :
    - Brute : non modifiable ensuite.
    - Avec un paramètre défini auparavant, hors tracé d'esquisse.
    - En définissant un nouveau paramètre avec son nom et sa valeur ; ce paramètre
      sera modifiable et utilisable à nouveau si besoin.

<p align="center">
  <img src="FIGURES/ex1_dimension_constraint.png"
       alt="Screenshot of the SHAPER Length panel, with the prompt Select a line on which to calculate length, a Line field reading SketchLine_2, a Value field reading 93.56699838 and three Text location buttons. In the viewer beside it the current length 93.567 is shown in blue and circled, and an edit box circled beside it reads L equals 100."
       width="760"/>
  <br>
  <em>Slide 25 — the current value in blue, and the wanted value entered as a
  new named parameter.</em>
</p>

#### Fin des contraintes

- Fin des contraintes :
  - L'esquisse passe au vert (0 degrés de liberté)
  - Affichage des dimensions sous deux aspects :
    - La valeur prescrite.
    - La nom du paramètre, le cas échéant.

<p align="center">
  <img src="FIGURES/ex1_constraints_done.png"
       alt="Two screenshots side by side of the same fully fixed sketch, drawn in green with the panel reading Sketch is fully fixed, DoF equals 0. In the left one the dimensions are shown as the prescribed values 100, 150 and 10. In the right one, with Show existing expressions ticked, the same dimensions are shown as the parameter names L, H and a."
       width="850"/>
  <br>
  <em>Slide 26 — the sketch turns green, and the dimensions read either as values
  or as parameter names.</em>
</p>

#### Dans l'arbre d'études

- Dans l'arbre d'études
  - Dans la zone `Parameters`, on retrouve les paramètres qui ont été créés au
    cours du dimensionnement : nom et valeur. On peut les modifier en partant
    d'ici et le dessin s'ajuste automatiquement.
  - Dans la zone `Construction`, s'inscrit le produit du sketch. C'est d'ici que
    l'on pilote sa visualisation ou son utilisation ultérieure dans l'élaboration
    de la pièce.
  - Dans la zone courante, en bas, ce sont les opérations. C'est d'ici qu'on
    pourra modifier l'esquisse si besoin.

<p align="center">
  <img src="FIGURES/ex1_study_tree.png"
       alt="Screenshot of the SHAPER Object browser tree: Part set holding Parameters (0), Constructions (7), Parts (1) and Part_1, which holds Parameters (3) listing H equals 150, L equals 100 and a equals 10, Constructions (1) holding Sketch_1, and Results (0), with a further Sketch_1 entry below as the current operation."
       width="420"/>
  <br>
  <em>Slide 27 — the parameters, the sketch as a construction, and the operation
  itself.</em>
</p>

#### Commentaire sur les contraintes

- Commentaire sur les contraintes
  - A chaque contrainte correspond un symbole dans la partie graphique.
    - Point jaune pour les coïncidences.
    - Lettre H ou V pour horizontalité ou verticalité.
    - Flèches avec les cotes.
    - Etc.
  - On peut supprimer une contrainte en détruisant ce symbole.
  - On peut affecter les contraintes à n'importe quelle étape de l'élaboration de
    l'esquisse. Il est conseillé de les appliquer au fur et à mesure pour
    optimiser le traitement.

<p align="center">
  <img src="FIGURES/ex1_constraint_symbols.png"
       alt="The finished sketch drawn in green on a pale yellow ground, with blue dimension arrows labelled L across the top, H down the right and a at the arc, the letters H and V on the straight segments, and yellow dots at the coincidences. Blue callout arrows point in from the left at a yellow coincidence dot, at a V letter and at the arc dimension."
       width="560"/>
  <br>
  <em>Slide 28 — every constraint has its symbol in the graphics area, and can be
  deleted through it.</em>
</p>

### La face et les groupes

#### Création de la face

- Création de la face
  - Menu `"Build/Face"` avec la construction issue de l'esquisse.
    - On clique la face dans la vue graphique.
    - Elle apparaît sous un codage propre à Shaper dans la liste des objets.
- Dans l'arbre d'études :
  - La face est insérée dans la rubrique des résultats.
  - Elle peut être renommée *(comme tous les autres objets identifiés dans
    l'arbre d'études d'ailleurs)*.

<p align="center">
  <img src="FIGURES/ex1_build_face.png"
       alt="Screenshot of the SHAPER Face panel, whose Objects list holds one entry reading Sketch_1 slash Face-SketchArc_1_2r-S, beside a viewer where the plate face is outlined in cyan. Below, two boxed views of the study tree: in the first, Results (1) holds an entry circled and named Face_1_1; in the second, the same entry has been renamed Plaque, with a blue arrow pointing to it."
       width="850"/>
  <br>
  <em>Slide 29 — the face lands in the results, under a Shaper code, and is
  renamed Plaque.</em>
</p>

#### Les groupes

- Les groupes
  - Menu `"Features/Group"`
  - Création par un nom, le type et la désignation dans la vue graphique.
- Chaque groupe est identifié dans l'arbre d'études et est visualisable séparément

<p align="center">
  <img src="FIGURES/ex1_group_dialog.png"
       alt="Screenshot of the SHAPER Group panel: a Name field circled and reading gauche, a Type row of four buttons for vertex, edge, face and solid with the edge button pressed, and a selection list reading Plaque slash Modified_Edge and Sketch_1 slash SketchLine_1. In the viewer beside it the left-hand edge of the plate is highlighted in green."
       width="560"/>
  <img src="FIGURES/ex1_group_tree.png"
       alt="Screenshot of the SHAPER Object browser after the groups are made: Part_1 holds Parameters (3), Constructions (1), Results (1) with Plaque, and Groups (8) listing Face, gauche, haut, droite, bas, arc, A and B, each with an eye icon, with the same eight names repeated below as separate entries."
       width="300"/>
  <br>
  <em>Slide 30 — a group is a name, a type and a selection; each one is listed
  separately in the study tree.</em>
</p>

### Le maillage : algorithme et hypothèses

#### Création du maillage, activation du module SMESH

- Création du maillage : activation du module SMESH
  - La « création » du maillage crée en fait un objet de type « maillage » qu'il
    faudra ensuite qualifier.
  - On désigne l'objet géométrique voulu, puis deux méthodes :
    - Avec le menu déroulant `"Mesh"`,
    - Ou graphiquement avec l'icone ad-hoc.
  - *Remarque : dans la fenêtre qui apparaît, SMESH a reconnu qu'il n'y avait que
    des surfaces dans la géométrie et a inactivé le choix 3D.*
- Le maillage peut être renommé.

<p align="center">
  <img src="FIGURES/ex1_create_mesh_dialog.png"
       alt="Screenshot of the Create mesh dialog: a Name field circled and reading Mesh_1, a Geometry field reading Plaque, a Mesh type field reading Any, tabs 3D, 2D, 1D and 0D with 3D greyed out and 2D selected, empty Algorithm, Hypothesis and Add. Hypothesis fields, and the buttons Assign a set of hypotheses, Apply and Close, Apply, Close and Help. A blue callout box beside the Name field reads Le maillage peut être renommé."
       width="820"/>
  <img src="FIGURES/ex1_mesh_toolbar_tree.png"
       alt="Screenshot of the SMESH toolbars with the Maillage menu circled and the create-mesh button circled in the button row, above a study tree listing Shaper, ShaperResults and, under Plaque, the entries Plaque in red, Face, gauche, haut, droite, bas, arc, A and B."
       width="320"/>
  <br>
  <em>Slide 31 — SMESH has seen only surfaces in the geometry and greyed out the
  3D choice.</em>
</p>

#### Choix de l'algorithme

- Choix de l'algorithme
  - On choisit MG-CADSurf dans la liste des algorithmes possibles.
  - Constat :
    - On aurait pu choisir `"Triangular"` dans le type de maille, mais c'est
      inutile ici car le choix aura lieu dans la définition ultérieure des
      hypothèses.
    - Le choix 1D est inactivé, car cet algorithme *(comme NETGEN 1D_2D)* maille
      automatiquement les bords du domaine.

<p align="center">
  <img src="FIGURES/ex1_algorithm_choice.png"
       alt="Screenshot of the Créer un maillage dialog in French: Nom reading Maillage_1, Géométrie reading Plaque, Type de maillage reading Tout type, a ticked box reading Créer tous les groupes définis dans la géométrie, the 2D tab selected with 1D and 0D greyed out, Algorithme reading MG-CADSurf, Hypothèse reading Défaut with its edit button circled in blue, and Ajouter l'hypothèse reading None."
       width="760"/>
  <br>
  <em>Slide 32 — MG-CADSurf chosen; the 1D choice is greyed out because the
  algorithm meshes the boundaries itself.</em>
</p>

#### Les hypothèses

- Les hypothèses
  - Par défaut, MG-CADSurf propose des caractéristiques plausibles. Par exemple la
    taille de maille moyenne, `"User size"`, vaut le 10ème de la diagonale.

<p align="center">
  <img src="FIGURES/ex1_default_hypotheses.png"
       alt="Screenshot of the Hypothesis Construction dialog for MG-CADSurf, on the Arguments tab beside Advanced, Local size, Enforced vertices, Periodicity and Hyper-patch. Name reads MG-CADSurf Parameters_1. Physical Size gives Type Global size, User Size 18.0278, Min Size 0.180278 and Max Size 36.0555. Geometrical size gives Type Global size, Mesh angle 8 and Mesh distance 9.01388. Main parameters shows Quadratic mesh unticked, Gradation 1.3 and Mesh optimisation ticked. Elements type has Triangles selected. Other parameters lists Anisotropic, Optimize tiny edges, Remove tiny edges and Remove bad elements unticked, Correct surface intersections ticked at 15, and Volume Gradation unticked."
       width="840"/>
  <br>
  <em>Slide 33 — the defaults MG-CADSurf proposes, the average size a tenth of
  the diagonal.</em>
</p>

#### La taille des mailles

- La taille des mailles « physique »
  - `"User size"` est la taille moyenne que l'on veut
  - `"Min size"` et `"Max size"` sont à ajuster, par exemple /100 et *3.
- La taille des mailles « géométrique »
  - L'angle et la distance gèrent l'écart entre le segment créé et la tangente à
    la frontière.
  - *Inutile de définir ces valeurs si la géométrie ne comporte que des lignes
    droites.*
- Paramètres principaux
  - On choisit quadratique ou non.
  - Gradation : le coefficient de variation d'une maille à sa voisine ; ne pas
    toucher en général.
- Type de maille voulu

<p align="center">
  <img src="FIGURES/ex1_mesh_size_params.png"
       alt="Four panels of the MG-CADSurf hypothesis dialog stacked in a column. Physical Size gives Type Global size, User Size 15, Min Size 0.15 and Max Size 45. Geometrical size gives Type Global size, Mesh angle 8 and Mesh distance 0.25. Main parameters shows Quadratic mesh ticked, Gradation 1.3 and Mesh optimisation ticked. Elements type offers Triangles, selected, Quadrangle dominant and Quadrangles."
       width="440"/>
  <br>
  <em>Slide 34 — the physical sizes, the geometrical sizes, the main parameters
  and the element type.</em>
</p>

- Les hypothèses
  - Les autres paramètres restent par défaut.
  - Valider avec `"OK"`

<p align="center">
  <img src="FIGURES/ex1_hypotheses_final.png"
       alt="Screenshot of the same Hypothesis Construction dialog for MG-CADSurf, now with User Size 15, Min Size 0.15, Max Size 45, Mesh angle 8, Mesh distance 0.25, Quadratic mesh ticked, Gradation 1.3, Mesh optimisation ticked, Triangles selected as the element type, and the OK, Cancel and Help buttons at the bottom."
       width="840"/>
  <br>
  <em>Slide 35 — the other parameters left at their defaults, and OK.</em>
</p>

#### Le raffinement local sur l'arc

- Le raffinement local sur l'arc
  - Onglet `"Local size"`.

<p align="center">
  <img src="FIGURES/ex1_local_size_tab.png"
       alt="Screenshot of the Hypothesis Construction dialog for MG-CADSurf with the Local size tab selected. An empty table headed Name and Local size fills the left of the tab; on the right are a Select a shape field, a Local Size field reading 0, Simple map and Advanced buttons, and the buttons Add, Remove and Modify, with Modify greyed out."
       width="800"/>
  <br>
  <em>Slide 36 — the Local size tab, still empty.</em>
</p>

- Le raffinement local sur l'arc
  - Sélectionner le groupe géométrique correspondant à l'arc.
  - Entrer la taille de maille souhaitée
  - Après ajout, la donnée est enregistrée. Elle pourra être modifiée plus tard.

<p align="center">
  <img src="FIGURES/ex1_local_size_arc.png"
       alt="Screenshot of the study tree with the group arc selected and highlighted in blue among Face, gauche, haut, droite, bas, A and B, beside the French Construction d'une hypothèse dialog for MG-CADSurf 2D on its Taille locale tab, where a selection field circled in blue reads arc and a field reading Taille Locale contains 0.15. Below, a small view of the English Local size tab shows the table now holding one row, arc with the value 0.15."
       width="850"/>
  <br>
  <em>Slide 37 — the arc group picked and given its own mesh size, then
  recorded.</em>
</p>

#### Les groupes de mailles et de nœuds

- Les groupes, si l'option automatique n'a pas été cochée
  - Les groupes définis dans Shaper sont transférés entre l'objet géométrique et
    l'objet de maillage.
  - Dans la fenêtre `"Create Groups from Geometry"` de l'onglet `"Mesh"`, on
    sélectionne les groupes de mailles et de nœuds depuis l'arbre d'études.
  - *Remarque : à ce moment, cette opération est simplement une mise en relation
    pour les futures mailles et nœuds qui seront créés. En cas d'oubli, on peut
    aussi la faire après calcul du maillage.*

<p align="center">
  <img src="FIGURES/ex1_groups_from_geometry.png"
       alt="Screenshot of the study tree with the node groups A and B selected and highlighted in blue, beside the French Créer des groupes à partir de la géométrie dialog. A Maillage field reads Maillage_1. Under Eléments, a Géométrie list holds Face, gauche, haut, droite, bas and arc. Under Nœuds, a second Géométrie list holds A and B. The buttons Appliquer et fermer, Appliquer, Fermer and Aide run along the bottom."
       width="840"/>
  <br>
  <em>Slide 38 — the Shaper groups related to the mesh object, before any mesh
  exists.</em>
</p>

### Calcul et exportation

#### Après application et fermeture

- Après application et fermeture :
  - L'arbre d'études s'est enrichi de l'algorithme et des hypothèses créés.
  - On peut éditer les hypothèses pour les modifier.
  - Le symbole indique que le maillage est défini mais n'a pas encore été calculé.
- Le calcul effectif du maillage se fait en sélectionnant l'objet de maillage dans
  l'arbre d'études et :
  - Par le choix `"Compute"` de l'onglet `"Mesh"`,
  - Ou par le choix `"Compute"` avec la patte droite de la souris
  - Ou par l'icône

<p align="center">
  <img src="FIGURES/ex1_mesh_tree.png"
       alt="Screenshot of the SMESH study tree: Shaper, ShaperResults and Mesh, which holds Hypotheses with MG-CADSurf Parameters_1, Algorithms with MG-CADSurf, and Maillage_1, itself holding Plaque in red, Applied algorithms, Groups of Nodes with A and B, Groups of Edges with gauche, haut, droite, bas and arc, and Groups of Faces with Face."
       width="420"/>
  <img src="FIGURES/ex1_compute_icon.png"
       alt="Screenshot of the SMESH toolbar with the File, Edit, View, Mesh and Controls menus above two rows of buttons, the compute-mesh gear button at the right circled in blue."
       width="400"/>
  <br>
  <em>Slide 39 — the algorithm and hypotheses now in the tree, and the Compute
  button that runs the mesher.</em>
</p>

#### Après calcul

- Après calcul :
  - Affichage d'un tableau récapitulatif des caractéristiques du maillage.
  - *Remarque : ces algorithmes de maillage en triangles incluent une fonction de
    tirage au hasard pour l'insertion de nouveaux nœuds. Il n'est donc pas anormal
    que les nombres de nœuds et de mailles varient légèrement d'une fois sur
    l'autre.*

<p align="center">
  <img src="FIGURES/ex1_mesh_info.png"
       alt="Screenshot of the Mesh computation succeed dialog. Under Name it reads Mesh_1. A Mesh Infos table with columns Total, Linear, Quadratic and Bi-Quadratic gives Nodes 2556, 0D Elements 0, Balls 0, Edges 165 of which 165 quadratic, Faces 1195 all quadratic, Triangles 1195 all quadratic, Quadrangles 0, Polygons 0, and all volume rows — Volumes, Tetrahedrons, Hexahedrons, Pyramids, Prisms, Hexagonal prisms and Polyhedrons — at zero."
       width="780"/>
  <br>
  <em>Slide 40 — the summary table: 2556 nodes and 1195 quadratic triangles.</em>
</p>

- Après calcul :
  - Affichage du maillage.

<p align="center">
  <img src="FIGURES/ex1_computed_mesh.png"
       alt="The computed mesh of the quarter plate: a bright blue rectangle covered in dark green triangle edges, with a quarter-circle hole at the bottom left where the triangles become very much finer, a green axis triad at the top left and a dark blue marker on the node at the foot of the arc."
       width="380"/>
  <br>
  <em>Slide 41 — the mesh on screen, refined at the edge of the hole.</em>
</p>

#### Exportation du maillage

- Exportation du maillage dans un fichier au format MED
  - Par le choix `"Export/MED file"` avec la patte droite de la souris.

<p align="center">
  <img src="FIGURES/ex1_export_med.png"
       alt="Screenshot of the Export mesh file dialog. Look in reads slash home slash D68518 slash Salome, and the file list shows the folders al, DISTENE, doc, Exemples, Formation, HYDRO, MMC, MODIF, Outils and resu with their dates. The File name field holds Mesh_1, selected, Files of type reads MED 4.0 files with the extension med, and the Save and Cancel buttons sit at the right."
       width="800"/>
  <br>
  <em>Slide 42 — the mesh exported to a MED file.</em>
</p>

<sub>[▲ back to the five parts](#cao-et-maillage-dans-salome_meca--shaper--smesh--simvia-web-edition)</sub>

---

## Part 5 · Exercice n°2

> **In this part**
> [Objectif du tube coudé](#objectif-du-tube-coud) ·
> [Données du tube coudé](#donnes-du-tube-coud) ·
> [Principe général](#principe-gnral) ·
> [Analyse](#analyse) ·
> [Les esquisses du tuyau](#les-esquisses-du-tuyau) ·
> [Le chemin et le tuyau](#le-chemin-et-le-tuyau) ·
> [Les groupes du tuyau](#les-groupes-du-tuyau) ·
> [Le maillage : algorithmes et sous-maillages](#le-maillage--algorithmes-et-sous-maillages) ·
> [Calcul et exportation du maillage](#calcul-et-exportation-du-maillage)
>
> *Original deck: slides 43–71.*

### Objectif du tube coudé

- Un tube coudé
- Objectif :
  - Maillage en hexaèdres
  - Linéaire
  - Groupes sur les faces de bord
  - Groupe pour un nœud pour le post-traitement

<p align="center">
  <img src="FIGURES/ex2_target_mesh.png"
       alt="A bent tube running up from the bottom left, turning through a right angle at the top and continuing down to the right, drawn as a bright blue surface covered in a regular grid of dark green quadrilateral edges."
       width="820"/>
  <br>
  <em>Slide 43 — the mesh the exercise builds: linear hexahedra on an elbowed
  pipe.</em>
</p>

### Données du tube coudé

- Groupes de faces :
  - `Base`, `Efond` : section du tuyau
  - `Surfint`, `Surfext` : surfaces intérieure et extérieure du tuyau
- Groupe de nœud :
  - `B` : sur la section `Efond` et sur la surface interne du tuyau
- Tailles de maille :
  - 15 segments sur chacun des tronçons le long du tuyau
  - 20 segments sur la périphérie
  - 2 segments dans l'épaisseur

<p align="center">
  <img src="FIGURES/ex2_data.png"
       alt="A dimensioned drawing of the elbowed pipe. A green centre line rises vertically, turns through a magenta-annotated angle theta equals 90 with a bend radius Rc equals 0.6, and runs horizontally to the right. Magenta dimensions read Lg equals 3 for each straight run. The lower end is labelled Base in blue and the far end B and Efond. To the right, a green annulus seen end on is dimensioned Re equals 0.2 and e equals 0.02, its outer and inner surfaces labelled Surfext and Surfint."
       width="800"/>
  <br>
  <em>Slide 44 — the dimensions, the four face groups and the node group B.</em>
</p>

### Principe général

- Méthode à employer :
  - Créer la surface de base
  - Créer l'axe moyen du tuyau
  - Propager la surface de base le long de l'axe moyen

<p align="center">
  <img src="FIGURES/ex2_general_principle.png"
       alt="A solid green silhouette of the elbowed pipe seen from the side: a vertical leg rising from the bottom, a rounded corner, and a horizontal leg running to the right, with a small white annulus at the foot of the vertical leg standing for the base surface."
       width="340"/>
  <br>
  <em>Slide 45 — the base surface swept along the centre line of the pipe.</em>
</p>

### Analyse

#### Réduire chaque surface à un carré

- Mailler les volumes en hexaèdres suppose de mailler les surfaces en quadrangles,
  donc :
  - Chaque surface est réductible à un carré.
  - Les discrétisations sont les mêmes pour deux arêtes en vis-à-vis pour ce carré.
- Il faut donc anticiper cela au moment de la création de la CAO
  - Par un dessin spécifique
  - Par des partitions
  - …

<p align="center">
  <img src="FIGURES/ex2_quad_reduction.png"
       alt="Four views in a row, separated by an orange rule. First, a blue quadrangle mesh on a face with a notched bottom left corner, with two blue ellipses ringing the places where the mesh degenerates. Second, the same face drawn plain in pale orange, captioned 5 côtés, problème. Third, the same face in pale yellow but split by a diagonal into two four-sided zones, captioned Séparation en zones à 4 côtés, réglé. Fourth, the resulting clean blue quadrangle mesh."
       width="850"/>
  <br>
  <em>Slide 46 — a five-sided face is a problem; split into four-sided zones it
  is solved.</em>
</p>

#### La ligne de couture

- Subtilité de la ligne de couture…
  - Un cylindre, et plus généralement une forme extrudée à partir d'une courbe
    fermée, n'a pas sa surface « lisse ».
  - Il existe toujours une ligne de « couture » :
- Positionnement de cette ligne
  - Pour les plans de départ canoniques, c'est du côté du premier axe dans la
    permutation circulaire (X,Y,Z) :
    - XOY et extrusion selon Z → X+
    - YOZ et extrusion selon X → Y+
    - XOZ et extrusion selon Y → Z+
  - Pour un autre plan, je ne sais pas !
- Conséquence pour le maillage
  - Une arête sera automatiquement créée sur cette ligne de couture. Elle
    délimitera ainsi une surface à 3 côtés.

<p align="center">
  <img src="FIGURES/ex2_seam_line.png"
       alt="A short pale lilac cylinder seen at an angle, with a single dark vertical line running down the visible side of its curved surface, marking the seam."
       width="440"/>
  <br>
  <em>Slide 47 — the seam line an extruded closed curve always carries.</em>
</p>

#### Dans notre cas

- Dans notre cas :
  - Il faut gérer le découpage en surfaces à 4 côtés…
    - Donc découper le disque de base
  - en ayant en tête la future ligne de couture,
    - Le cercle de base est dans le plan XOZ et on extrudera selon Y → couture en Z+
  - et le nœud pour le groupe `B`.
    - Dans le cercle, de base, il doit y avoir une coupure en X+ du côté intérieur.
- Conséquences
  - La coupure pour le futur nœud est à l'intérieur du cylindre. Mais pour garder
    la correspondance entre l'intérieur et l'extérieur, il faut aussi insérer un
    point à l'extérieur, en vis-à-vis.
  - On ajoutera donc un découpage en créant un segment qui les joint.

<p align="center">
  <img src="FIGURES/ex2_pipe_end_mesh.png"
       alt="The open end of a meshed tube seen at an angle, the annular end face outlined in bright cyan and the dark blue inner surface behind it covered in a grid of cyan quadrilateral edges."
       width="380"/>
  <img src="FIGURES/ex2_base_cut_one.png"
       alt="A thick green annulus seen face on, with orange Z and X axis arrows crossing at its centre and labelled Z, Y and X. A short dark blue segment cuts across the ring where the X axis meets it, and a blue callout arrow points at that cut."
       width="330"/>
  <br>
  <em>Slides 48 and 49 — the base disc cut at X plus, on the inside, for the
  future node of group B.</em>
</p>

#### La coupure de l'extrusion

- Mais la coupure de l'extrusion…
  - Ne s'applique qu'aux surfaces nouvellement créées. La base est inchangée est
    reste une surface à 3 côtés.
  - On insère donc une autre coupure de la base et on la place en regard de ce qui
    sera la couture de l'extrusion.
- C'est correct, mais…
  - La ligne périphérique sera coupée en deux tronçons inégaux et les hypothèses
    de maillage seront moins simples à appliquer.
  - On coupe donc en 4 :
- Il faudra prévoir des groupes pour définir le nombre de segments au bord du disque
  - `d1` : un des segments de coupure
  - `L1` : la périphérie

<p align="center">
  <img src="FIGURES/ex2_base_cut_two.png"
       alt="A thick green annulus seen face on with orange Y and X axis arrows at its centre, now carrying two short dark blue cuts across the ring: one where the X axis meets it and one at the top, where the Z axis meets it."
       width="300"/>
  <img src="FIGURES/ex2_base_cut_four.png"
       alt="The same thick green annulus with orange Z, Y and X axis labels, now carrying four short dark blue cuts across the ring, evenly spaced at the top, bottom, left and right, dividing the periphery into four equal arcs."
       width="300"/>
  <br>
  <em>Slide 49 — a second cut facing the future seam, then four equal cuts so the
  meshing hypotheses stay simple.</em>
</p>

### Les esquisses du tuyau

#### Activer le module SHAPER, créer une nouvelle pièce

- Activer le module SHAPER
- Créer une nouvelle pièce (part)
- Elle apparaît dans le browser, avec ces rubriques, vides au démarrage :
  - `Paramètres` : ceux de la future pièce.
  - `Constructions` : les différentes esquisses, plans, axes, etc. nécessaires au
    fur et à mesure.
  - `Résultats` : le ou les dernières parties créées pour constituer la pièce.
    Cette rubrique se remplit et se vide au fur et à mesure des opérations.

<p align="center">
  <img src="FIGURES/ex2_new_part.png"
       alt="Screenshot of the SHAPER menu bar reading File, Edit, View, Part, Sketch, Construction, Build and Primitives, with Part circled in blue, the toolbars below it, and an Object browser listing Part set with Parameters (0), Constructions (7), Parts (1) and, under Part_1, Parameters (0), Constructions (0) and Results (0)."
       width="620"/>
  <br>
  <em>Slide 50 — a new part for the second exercise, its three sections still
  empty.</em>
</p>

#### Créer une esquisse dans le plan XOZ

- Créer une esquisse (sketch)
- Choisir successivement :
  - La taille ; c'est une estimation pour cadrer la vue. En gros c'est le côté
    d'un carré qui contient le futur dessin.
    - *Se tromper n'est pas grave mais simplement désagréable car il faudra faire
      des recadrages en visualisation.*
  - Le plan : celui sur lequel on va dessiner. La feuille de papier en quelque
    sorte. On choisit l'un des 3 plans XOY, YOZ ou XOZ, soit en le désignant à la
    souris dans la fenêtre graphique, soit en le sélectionnant dans la liste du
    browser. Pour cet exercice, on choisit XOZ.
  - On valide.
- *Remarque :*
  - *Pour des CAO 3D compliquées, on pourra choisir un plan qui est une face de
    l'objet en cours de création ou un plan que l'on aura créé spécialement. Cela
    permet de positionner l'esquisse que l'on dessine directement à sa place dans
    l'espace 3D*

<p align="center">
  <img src="FIGURES/ex2_sketch_menu.png"
       alt="Screenshot of the SHAPER menu bar with Sketch circled in blue, the toolbars below it including a row of sketch tools, and an Object browser panel listing Part set with Parameters (0), Constructions (7), Parts (1) and, under Part_1, Parameters (0), Constructions (0) and Results (0)."
       width="500"/>
  <img src="FIGURES/ex2_sketch_plane.png"
       alt="Screenshot of the SHAPER Sketch panel showing a field Size of the view set to 200 and the prompt Select a plane on which to create a sketch, beside a viewer displaying the three coordinate planes edge on as three interlocking rectangles outlined in red, green and blue."
       width="440"/>
  <br>
  <em>Slide 51 — the same two steps, this time choosing the XOZ plane.</em>
</p>

#### Dessiner les deux cercles

- Dessiner à main levée les deux cercles
  - On pique le centre (clic gauche), on écarte la souris, clic gauche pour finir.
  - Respecter grosso modo la position et les dimensions.
  - Placer le centre du bon côté des axes.
- Bilan
  - Il y a 6 degrés de liberté, 3 pour chaque cercle :
    - les 2 coordonnées du centre
    - le rayon.

<p align="center">
  <img src="FIGURES/ex2_two_circles.png"
       alt="Screenshot of the SHAPER sketch toolbar with the circle tool circled in blue, a viewer where two concentric red circles have been drawn freehand on a pale yellow ground with the coordinate axes crossing near their centre, and below them the Sketch panel with its Reversed checkbox, Set plane view button, the checkboxes Show geometrical constraints, Show dimensional constraints and Show existing expressions, and a line reading DoF degrees of freedom equals 6."
       width="700"/>
  <br>
  <em>Slide 52 — the two circles drawn freehand, six degrees of freedom.</em>
</p>

#### Contraintes de positionnement et de dimension

- Contraintes de positionnement
  - Le centre des cercles sont sur l'origine (-4 ddl)
- Contraintes de dimension
  - La valeur actuelle s'affiche en bleu
  - La valeur voulue peut être définie de 3 façons :
    - Brute : non modifiable ensuite.
    - Avec un paramètre défini auparavant.
    - En définissant un nouveau paramètre avec son nom et sa valeur ; ce paramètre
      sera utilisable à nouveau si besoin.
- Bilan :
  - L'esquisse passe au vert (0 ddl)

<p align="center">
  <img src="FIGURES/ex2_radius_constraint.png"
       alt="Screenshot of the SHAPER Radius panel, with the prompt Select a circle or an arc on which to calculate radius, a Circle or Arc field reading SketchCircle_2_2, a Value field reading 92.1317772712 and three Text location buttons. In the viewer beside it the current radius 92.1318 is shown in blue and an edit box circled beside it reads Re equals 0.2."
       width="820"/>
  <br>
  <em>Slide 53 — the current radius in blue, the wanted one entered as the
  parameter Re.</em>
</p>

<p align="center">
  <img src="FIGURES/ex2_circles_fixed.png"
       alt="Screenshot of the SHAPER Sketch panel reading Sketch is fully fixed, DoF equals 0, beside a viewer where the two concentric circles are now drawn in green, dimensioned 0.2 and 0.18 by blue leader arrows, with a black dot marking their common centre on the origin."
       width="720"/>
  <br>
  <em>Slide 53 — both centres on the origin and both radii set: the sketch turns
  green.</em>
</p>

#### Tracer les coupures

- Tracer la première coupure
  - Piquer un point sur le cercle extérieur pas très loin de l'objectif ; le
    cercle passe en bleu.
  - Puis piquer un point sur le cercle intérieur, grosso modo où on veut arriver ;
    le cercle passera au bleu.
  - Touche `"Echap"` pour arrêter le tracé.
  - La ligne existe avec ses deux extrémités en jaune pour signifier qu'elles sont
    attachées au cercle.
- Bilan
  - Il y a 2 degrés de liberté : position de chaque extrémité sur leur cercle
    respectif.
- Deux possibilités
  - On contraint chaque point à appartenir à l'axe Ox.
  - Ou on déclare que l'origine appartient à la droite et que la droite est
    horizontale.

<p align="center">
  <img src="FIGURES/ex2_line_tool_strip.png"
       alt="Screenshot of the SHAPER sketch toolbar strip, showing a greyed Sketch label, a green tick and a red cross, then the point, line and rectangle tools, with the line tool circled in blue."
       width="420"/>
  <br>
  <em>Slide 54 — the line tool, used to draw the cut.</em>
</p>

<p align="center">
  <img src="FIGURES/ex2_first_cut.png"
       alt="Screenshot of a SHAPER viewer showing the two concentric circles in red on a pale ground, crossed by faint grey construction spokes, dimensioned 0.2 and 0.18 by blue leader arrows, with two yellow dots on the right where the ends of the new cut segment attach to the circles. A small panel below reads DoF degrees of freedom equals 2."
       width="560"/>
  <br>
  <em>Slide 54 — the first cut drawn between the two circles, its ends yellow
  because they are attached.</em>
</p>

- Procéder de la même façon pour les autres coupures
- Fin des contraintes :
  - L'esquisse passe au vert (0 ddl)
  - Affichage des dimensions sous deux aspects :
    - La valeur prescrite.
    - La nom du paramètre, le cas échéant.

<p align="center">
  <img src="FIGURES/ex2_cuts_done.png"
       alt="Two screenshots side by side of the same finished base sketch, each with its panel reading Sketch is fully fixed, DoF equals 0. Two concentric circles are drawn in green with yellow dots at four evenly spaced pairs of points where the cuts attach. In the left view the radii are dimensioned 0.2 and 0.18; in the right one, with Show existing expressions ticked, the same radii read Re and Re minus e."
       width="850"/>
  <br>
  <em>Slide 55 — all four cuts constrained, the dimensions shown as values then
  as parameter names.</em>
</p>

#### Le tracé du chemin

- Créer une esquisse (sketch) dans le plan XOY
- Dessiner à main levée le tracé du chemin
  - Respecter grosso modo les formes et les dimensions.
  - Se placer du bon côté des axes.
  - Pour arrêter : touche `"Echap"`
- Ajouter l'arrondi par un congé
  - Piquer l'angle
- 7 degrés de liberté au final
  - Position des extrémités (2x2ddl)
  - Longueur des segments droits (+2ddl)
  - Angle entre les segments (+1ddl)

<p align="center">
  <img src="FIGURES/ex2_path_sketch.png"
       alt="Screenshot of two SHAPER sketch toolbar strips, the first with the line tool circled and the second with the fillet tool circled. Between and below them, two viewers show the path: first a red vertical segment meeting a red horizontal segment at a sharp corner, then the same path with the corner rounded by a fillet, beside a Sketch panel reading DoF degrees of freedom equals 7."
       width="820"/>
  <br>
  <em>Slide 56 — the path drawn freehand, then the corner rounded by a
  fillet.</em>
</p>

- Contraintes pour éliminer les 7 degrés de liberté
  - L'origine est confondue avec la 1ère extrémité (-2ddl)
  - Le 1er segment est vertical (-1ddl)
  - Longueur du 1er segment (-1ddl)
  - Rayon du congé (-1ddl)
  - Longueur du 2nd segment identique au 1er (-1ddl)
  - Angle entre les segments (-1ddl)
- Remarque
  - Les degrés de liberté que l'on constate ne sont pas forcément ceux qui sont
    directement éliminés.
  - Par exemple ici, contraindre le 1er segment à être vertical revient à imposer
    la coordonnée x de sa seconde extrémité.

<p align="center">
  <img src="FIGURES/ex2_path_constraints.png"
       alt="Screenshot of the SHAPER Sketch panel reading Sketch is fully fixed, DoF equals 0, beside a viewer where the path is now drawn in green: a vertical segment marked V and dimensioned Lg, a fillet dimensioned Rc, a horizontal segment marked with an equals sign, and an angle dimension labelled Theta swept between them."
       width="760"/>
  <br>
  <em>Slide 57 — the seven degrees of freedom removed, the path fixed by Lg, Rc
  and Theta.</em>
</p>

### Le chemin et le tuyau

#### Créer le chemin

- Créer le chemin
  - Menu `"Build/Wire"`
  - Sélectionner les 3 lignes
- Dans l'arbre d'études :
  - Le chemin est inséré dans la rubrique des résultats.
  - Il peut être renommé *(comme tous les autres objets identifiés dans l'arbre
    d'études d'ailleurs)*.

<p align="center">
  <img src="FIGURES/ex2_build_wire.png"
       alt="Screenshot of the SHAPER Wire panel, whose Segments and wires list holds Sketch_2 slash SketchLine_7, Sketch_2 slash SketchArc_1_2 and Sketch_2 slash SketchLine_8, beside a viewer where the path is drawn in green. Below, two boxed views of the study tree: in the first, Results (1) holds an entry circled and named Wire_1_1; in the second the same entry has been renamed Chemin, with a blue arrow pointing to it."
       width="820"/>
  <br>
  <em>Slide 58 — the three lines built into a wire, then renamed Chemin.</em>
</p>

#### Créer le tuyau

- Créer le tuyau
  - Menu `"Features/pipe"`
  - Les objets de la base sont les 4 surfaces du disque.
  - *Attention à ne pas sélectionner toute l'esquisse sinon le disque central sera
    pris également.*
  - Le chemin est le « wire » créé précédemment.

<p align="center">
  <img src="FIGURES/ex2_create_pipe.png"
       alt="Screenshot of the SHAPER Pipe panel, whose Base objects list holds four selected entries reading Sketch_1 slash Face-SketchLine_1f, 5r, 2f and 6r, an empty Path object field and three sweep-mode buttons, beside a viewer showing the annular base sketch in dark red seen at an angle with the coordinate axes at its centre. Below, a second view of the Path object field, now circled and reading Chemin."
       width="820"/>
  <br>
  <em>Slide 59 — the four base surfaces swept along the wire named Chemin.</em>
</p>

### Les groupes du tuyau

- Les groupes
  - Menu `"Features/Group"`
  - Création par un nom, le type et la désignation dans le résultat
- Chaque groupe est identifié dans l'arbre d'études et est visualisable séparément
- Quels groupes ?
  - Pour le calcul : `Base`, `Efond`, `Surfext`, etc.

<p align="center">
  <img src="FIGURES/ex2_group_dialog.png"
       alt="Screenshot of the SHAPER Group panel: a Name field circled and reading L1, a Type row of four buttons with the edge button pressed, and a selection list holding four entries beginning Pipe_1_1 slash From_Face_1 and Pipe_1_1 slash Generated. In the viewer beside it the open end of the pale grey pipe is seen at an angle with its outer edge highlighted in pale green and yellow."
       width="700"/>
  <img src="FIGURES/ex2_group_tree.png"
       alt="Screenshot of the SHAPER Object browser: Part_1 holds Parameters (5), Constructions (2), Results (2) with Chemin and Tuyau, and Groups (7) listing Base, EFon, Surfext, Surfint, B, d1 and L1, with the same seven names repeated below as separate entries alongside Sketch_1, Sketch_2, Wire_1 and Pipe_1."
       width="330"/>
  <br>
  <em>Slide 60 — a group named, typed and picked on the result; each one listed
  separately in the tree.</em>
</p>

### Le maillage : algorithmes et sous-maillages

#### Création du maillage, activation du module SMESH

- Création du maillage : activation du module SMESH
  - La « création » du maillage crée en fait un objet de type « maillage » qu'il
    faudra ensuite qualifier.
  - On désigne l'objet GEOM voulu, puis deux méthodes :
    - Avec le menu déroulant `"Mesh"`,
    - Ou graphiquement avec l'icone ad-hoc.
- Le maillage peut être renommé.

<p align="center">
  <img src="FIGURES/ex2_create_mesh_dialog.png"
       alt="Screenshot of the French Créer un maillage dialog: Nom circled and reading Maillage_1, Géométrie reading Tuyau, Type de maillage reading Tout type, a ticked box reading Créer tous les groupes définis dans la géométrie, tabs 3D, 2D, 1D and 0D with 3D selected, empty Algorithme and Hypothèse fields, and the buttons Définir des hypothèses automatiques, Appliquer et fermer, Appliquer, Fermer and Aide. A blue callout box reads Le maillage peut être renommé."
       width="820"/>
  <img src="FIGURES/ex2_mesh_toolbar_tree.png"
       alt="Screenshot of the SMESH toolbars with the Maillage menu circled and the create-mesh button circled in the button row, above a study tree listing Shaper, ShaperResults and, under Tuyau, the entries Tuyau in red, Base, EFond, Surfext, Surfint, B, d1 and L1."
       width="300"/>
  <br>
  <em>Slide 61 — the pipe designated, and the mesh object created and renamed.</em>
</p>

#### Choix des algorithmes 3D et 2D

- Choix des algorithmes 3D et 2D
  - On choisit Hexahedron (i,j,k) dans la liste des algorithmes possibles pour le 3D.
    - Pas d'hypothèses particulière : les hexaèdres sont bâtis à partir des
      quadrangles créés sur les faces.
  - Puis « Quadrangle: Mapping » pour les surfaces.
    - Pas d'hypothèse particulière : le quadrillage est fait à partir des
      discrétisations des bords 1D.

<p align="center">
  <img src="FIGURES/ex2_algo_3d.png"
       alt="Screenshot of the French Créer un maillage dialog on its 3D tab: Nom reading Maillage_1, Géométrie reading Tuyau, Type de maillage reading Tout type, a ticked box reading Créer tous les groupes définis dans la géométrie, Algorithme reading Hexahedron with the indices i, j, k, Hypothèse reading Défaut and Ajouter l'hypothèse reading None."
       width="700"/>
  <img src="FIGURES/ex2_algo_2d.png"
       alt="Screenshot of the same dialog on its 2D tab, with Algorithm reading Quadrangle colon Mapping, Hypothesis reading Default and Add. Hypothesis reading None."
       width="620"/>
  <br>
  <em>Slide 62 — Hexahedron for the volumes, Quadrangle Mapping for the surfaces,
  neither needing a hypothesis.</em>
</p>

#### Choix de l'algorithme 1D

- Choix de l'algorithme 1D
  - On a 3 modes de discrétisation :
    - Le long du tuyau.
    - La périphérie du tuyau.
    - L'épaisseur du tuyau.
  - On choisit de définir globalement la discrétisation le long du tuyau, voulue à
    15 segments par arête : le 1er tronçon, le virage, le 2nd tronçon.
  - Les autres sont gérés par des sous-maillages.
- Algorithme 1D global
  - Discrétisation des arêtes en 15 segments équirépartis

<p align="center">
  <img src="FIGURES/ex2_algo_1d.png"
       alt="Screenshot of the 1D tab of the mesh dialog, with Algorithm reading Wire Discretisation and Hypothesis reading None, its edit button circled in blue. An arrow leads to a Number of Segments panel whose Arguments give Name Number of Segments_1, Number of Segments 15 and Type of distribution Equidistant distribution. A second view of the 1D tab below now shows Hypothesis reading Number of Segments_1."
       width="850"/>
  <br>
  <em>Slide 63 — the global 1D rule: fifteen equally spaced segments per edge
  along the pipe.</em>
</p>

#### Sous-maillage dans l'épaisseur

- Sous-maillage dans l'épaisseur
  - La création d'un sous-maillage se fait en sélectionnant l'objet de maillage
    dans l'arbre d'études et :
    - Par le choix `"Create Sub-mesh"` de l'onglet `"Mesh"`,
    - Ou par le choix `"Create Sub-mesh"` avec la patte droite de la souris
    - Ou par l'icône
- Support du sous-maillage : le groupe géométrique

<p align="center">
  <img src="FIGURES/ex2_submesh_toolbar.png"
       alt="Screenshot of the SMESH File, Edit, View and Mesh menus above a row of buttons, the create-sub-mesh button circled in blue, and an Object Browser below listing Shaper, Geometry with Wire_1_1 and Tuyau, whose children are Base, EFon, Surfext, Surfint, B, d1 and L1, then Mesh with Hypotheses, Algorithms and Mesh_1."
       width="300"/>
  <img src="FIGURES/ex2_submesh_dialog.png"
       alt="Screenshot of the Object Browser with the group d1 selected and highlighted in blue, beside the Create sub-mesh dialog whose Name field reads Sub-mesh_1, Mesh reads Mesh_1, Geometry reads d1 and Mesh type reads Any, with the 1D tab selected, empty Algorithm, Hypothesis and Add. Hypothesis fields, and the buttons Apply and Close, Apply, Close and Help."
       width="700"/>
  <br>
  <em>Slide 64 — a sub-mesh created on the geometrical group d1.</em>
</p>

- Sous-maillage 1D dans l'épaisseur
  - Discrétisation de l'arête liée au groupe `d1` en 2 segments.
  - Pour la cohérence du maillage, il faut propager cette hypothèse sur toutes les
    arêtes qui lui sont parallèles.

<p align="center">
  <img src="FIGURES/ex2_segments_2.png"
       alt="Screenshot of a Number of Segments panel whose Arguments give Name Number of Segments_2, Number of Segments 2 and Type of distribution Equidistant distribution."
       width="620"/>
  <br>
  <em>Slide 65 — two segments across the thickness.</em>
</p>

<p align="center">
  <img src="FIGURES/ex2_propagation.png"
       alt="Two screenshots of the 1D tab of the sub-mesh dialog. In the first, Algorithm reads Wire Discretisation, Hypothesis reads Number of Segments_2 and Add. Hypothesis reads None, with its edit button circled in blue. An arrow leads to the second, where Add. Hypothesis now reads Propagation of 1D Hyp. on Opposite Edges_1."
       width="850"/>
  <br>
  <em>Slide 65 — the hypothesis propagated to every parallel edge.</em>
</p>

#### Sous-maillage dans la périphérie

- Sous-maillage 1D dans la périphérie
  - Discrétisation du bord lié au groupe `L1` en 20 segments.
  - Même technique que pour `d1` mais attention : on veut 20 segments sur la
    périphérie. Le groupe `L1` est formé de 4 arêtes de même longueur donc on aura
    5 segments sur chacune d'elles.
  - *Remarque : c'est ici que l'on apprécie d'avoir coupé la face de base en
    4 parties égales…*

<p align="center">
  <img src="FIGURES/ex2_segments_5.png"
       alt="Screenshot of a Number of Segments panel whose Arguments give Name Number of Segments_3, Number of Segments 5 and Type of distribution Equidistant distribution."
       width="620"/>
  <br>
  <em>Slide 66 — five segments on each of the four equal edges of L1, twenty
  around the periphery.</em>
</p>

#### Les groupes de mailles et de nœuds

- Les groupes, si l'option automatique n'a pas été cochée
  - Les groupes définis dans Shaper sont transférés entre l'objet géométrique et
    l'objet de maillage.
  - Dans la fenêtre `"Create Groups from Geometry"` de l'onglet `"Mesh"`, on
    sélectionne les groupes de mailles et de nœuds depuis l'arbre d'études.
  - *Remarque : ici, cette opération est simplement une mise en relation pour les
    futures mailles et nœuds qui seront créés. En cas d'oubli, on peut aussi la
    faire après le calcul du maillage.*

<p align="center">
  <img src="FIGURES/ex2_groups_from_geometry.png"
       alt="Screenshot of the study tree with the node group B selected and highlighted in blue, and Maillage_1 expanded to show SubMeshes on Compound holding Sous-maillage_1 on d1 and Sous-maillage_2 on L1. Beside it the French Créer des groupes à partir de la géométrie dialog has a Maillage field reading Maillage_1, an Eléments list holding Base, EFond, Surfext, Surfint, d1 and L1, and a Nœuds list holding B."
       width="840"/>
  <br>
  <em>Slide 67 — the pipe's groups related to the mesh object, nodes and elements
  apart.</em>
</p>

### Calcul et exportation du maillage

#### Après application et fermeture

- Après application et fermeture :
  - L'arbre d'études s'est enrichi de l'algorithme et des hypothèses créés.
  - On peut éditer les hypothèses pour les modifier.
  - Les symboles indiquent que le maillage est défini mais n'a pas encore été calculé.
- Le calcul effectif du maillage se fait en sélectionnant l'objet de maillage dans
  l'arbre d'études et :
  - Par le choix `"Compute"` de l'onglet `"Mesh"`,
  - Ou par le choix `"Compute"` avec la patte droite de la souris
  - Ou par l'icône

<p align="center">
  <img src="FIGURES/ex2_mesh_tree.png"
       alt="Screenshot of the SMESH study tree: Shaper, ShaperResults and Mesh, which holds Hypotheses, Algorithms and Maillage_1. Maillage_1 holds Tuyau in red, Applied hypotheses, Applied algorithms, SubMeshes on Compound with Sous-maillage_1 on d1 and Sous-maillage_2 on L1, Groups of Nodes with B, Groups of Edges with d1 and L1, and Groups of Faces with Base, EFond, Surfext and Surfint."
       width="420"/>
  <img src="FIGURES/ex2_compute_icon.png"
       alt="Screenshot of the SMESH toolbar with the File, Edit, View, Mesh and Controls menus above two rows of buttons, the compute-mesh gear button at the right circled in blue."
       width="400"/>
  <br>
  <em>Slide 68 — the two sub-meshes and every group in the tree, and the Compute
  button.</em>
</p>

#### Après calcul

- Après calcul :
  - Affichage d'un tableau récapitulatif des caractéristiques du maillage.

<p align="center">
  <img src="FIGURES/ex2_mesh_info.png"
       alt="Screenshot of the Mesh computation succeed dialog. Under Name it reads Mesh_1. A Mesh Infos table with columns Total, Linear, Quadratic and Bi-Quadratic gives Nodes 2760, 0D Elements 0, Balls 0, Edges 552 all linear, Faces 2240 all linear, Triangles 0, Quadrangles 2240 all linear, Polygons 0, Volumes 1800 all linear, Tetrahedrons 0, Hexahedrons 1800 all linear, Pyramids 0, Prisms 0, Hexagonal prisms 0 and Polyhedrons 0."
       width="780"/>
  <br>
  <em>Slide 69 — the summary table: 2760 nodes, 2240 quadrangles and 1800 linear
  hexahedra.</em>
</p>

- Après calcul :
  - Affichage du maillage.

<p align="center">
  <img src="FIGURES/ex2_computed_mesh.png"
       alt="The computed mesh of the elbowed pipe: a bright blue tube rising from the bottom left, turning through a right angle and running down to the right, covered in a regular grid of dark green quadrilateral edges, with the annular section visible at the open far end."
       width="820"/>
  <br>
  <em>Slide 70 — the hexahedral mesh on screen.</em>
</p>

#### Exportation du maillage

- Exportation du maillage dans un fichier au format MED
  - Par le choix `"Export/MED file"` avec la patte droite de la souris.

<p align="center">
  <img src="FIGURES/ex2_export_med.png"
       alt="Screenshot of the Export mesh file dialog. Look in reads slash home slash D68518 slash Salome, and the file list shows the folders al, DISTENE, doc, Exemples, Formation, HYDRO, MMC, MODIF, Outils and resu with their dates. The File name field holds Mesh_1, selected, Files of type reads MED 4.0 files with the extension med, and the Save and Cancel buttons sit at the right."
       width="800"/>
  <br>
  <em>Slide 71 — the pipe mesh exported to a MED file.</em>
</p>

<sub>[▲ back to the five parts](#cao-et-maillage-dans-salome_meca--shaper--smesh--simvia-web-edition)</sub>

---

Merci !

---

## History

- **CAO et maillage dans salome_meca : SHAPER & SMESH** — *code_aster /
  salome_meca* course material by Gérald NICOLAS and Soizic PERON, EDF R&D
  PERICLES, authored and published by EDF S.A. under the GNU Free Documentation
  License, distributed as a slide deck. Its Title Page states no year; the deck
  carries the notice `Copyright 2021 EDF`.
- **CAO et maillage dans salome_meca : SHAPER & SMESH — Simvia web edition**,
  2026, modified and published by Simvia. Converted from the slide deck into a
  web page: text and figures unchanged, layout new.
  <https://simvia-tech.github.io/tutorials-code_aster/>

## GNU Free Documentation License

The full text of the license is in [LICENSE](../../LICENSE) at the root of this
repository.
