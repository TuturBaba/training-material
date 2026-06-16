---
layout: tutorial_hands_on

title: Pipeline d'annotation d'espèces marines par IA (Project Moorev - Marine)
zenodo_link: https://zenodo.org/records/20639741/files/Lieujaune-PorzBreign-GOPR5167_Edited.mp4?download=1
questions:
- Comment utiliser l'intelligence artificielle pour annoter des images d'espèces marines ?
- Comment vérifier et corriger ces annotations ?
objectives:
- Utiliser SAM3 pour détecter automatiquement des espèces marines à partir d'un simple mot-clé
- Corriger les annotations générées avec Edit COCO et AnyLabeling
time_estimation: 3H
key_points:
- L'IA fait le gros du travail d'annotation, mais un œil humain reste indispensable pour vérifier et corriger.
- Une fois mis en place, ce pipeline est réutilisable pour d'autres espèces ou d'autres projets.
tags:
  - Marine ecosystems
  - biodiversity
  - AI segmentation
  - YOLO
  - deep learning
  - object detection
contributions:
  authorship:
    - TuturBaba
    - yvanlebras
    - NadineLeBris
  funding:
    - moorev
    - sorbonneuniv
    - ISYEB
    - mnhn
    - pndb
lang: fr
translations:
    - en
---

Dans ce tutoriel, nous allons explorer un pipeline complet pour l'annotation d'images et de vidéos d'espèces marines. Nous utiliserons comme exemple une vidéo issue du projet [Moorev](https://moorev.fr/), découpée et disponible sur Zenodo.

Ce pipeline se décompose en deux grandes étapes :
1. **Annotation automatique** par prompt textuel avec {% tool [SAM3 Semantic Segmentation](toolshed.g2.bx.psu.edu/repos/ecology/sam3_semantic_segmentation/sam3_semantic_segmentation/1.0.1+galaxy4) %}
2. **Correction et validation** des annotations avec :
   - {% tool [Edit COCO Annotation](edit_coco_annotation) %}
   - {% tool [AnyLabeling Interactive](interactive_tool_anylabeling) %}

> <details-title>En savoir plus sur le projet MOOREV</details-title>
>
> **MOOREV** : Microclimats et outils d'observation des réponses du vivant sur les fonds marins 
> 
> **Observer les interactions entre espèces marines face aux gradients microclimatiques sur l'estran**
>
> Le projet MOOREV, porté par Nadine Le Bris (Sorbonne Université, Station Marine de Concarneau), 
> avec le soutien du Muséum National d'Histoire Naturelle, du CNRS et de la Fondation de France, 
> a été lancé en 2022. Son objectif : mieux comprendre et faire comprendre les effets des 
> perturbations climatiques sur la biodiversité du littoral.
>
> L'approche repose sur des méthodes d'imagerie sous-marine pour observer les espèces benthiques 
> à l'échelle individuelle, sur différents habitats de l'estran. Des groupes de scolaires 
> répètent des acquisitions de données sur leurs sites d'étude, labellisés par le programme 
> Aires Marines Éducatives de l'Office Français de la Biodiversité, au fil des cycles de marée, 
> des saisons et des années.
>
> Le projet associe chercheurs et professionnels de l'éducation à l'environnement, et est 
> co-construit avec des classes et leurs enseignants, en utilisant le littoral comme laboratoire 
> naturel. À terme, il vise à soutenir la mise en œuvre de mesures de protection et de 
> conservation, en tenant compte des interactions entre changement climatique et socio-écosystèmes 
> marins.
>
{: .details}

> <agenda-title>Dans ce tutoriel, nous allons couvrir :</agenda-title>
>
> 1. TOC
> {:toc}
>
{: .agenda}

# Chargement des données

La vidéo utilisée dans ce tutoriel est un extrait issu directement du projet Moorev, disponible sur Zenodo. Cet extrait a été sélectionné car il permet d'illustrer la majorité des fonctionnalités des outils présentés.

> <hands-on-title>Charger la vidéo dans Galaxy</hands-on-title>
>
> 1. Créez un nouvel historique pour ce tutoriel (par exemple : "Annotation Moorev")
> 
>    {% snippet faqs/galaxy/histories_create_new.md %}
>  
>    {% snippet faqs/galaxy/histories_rename.md %}
> 
> 2. Importez le fichier depuis [Zenodo]({{ page.zenodo_link }}) :
>
>    ```
>    https://zenodo.org/records/20639741/files/Lieujaune-PorzBreign-GOPR5167_Edited.mp4?download=1
>    ```
>
>    {% snippet faqs/galaxy/datasets_import_via_link.md %}
>
> 3. Vérifiez que le fichier apparaît en **vert** dans l'historique avant de continuer
>
> 4. Vérifiez le type de données
>
>    > <tip-title>Vérifier le type de données</tip-title>
>    >
>    > Le type de données est attribué automatiquement par Galaxy. Comme la majorité des outils l'utilisent pour filtrer leurs entrées, il est important de s'assurer qu'il est correct. Dans notre cas, vérifiez que le type est bien `mp4`. Si ce n'est pas le cas, l'import a peut-être rencontré une erreur, vous pouvez modifier le type manuellement.
>    >
>    > ![Vérifier le type de données](../../images/Annotation_AI_Pipeline/Check_data.png){: style="width:75%; display:block; margin:auto;"}
>    >
>    > {% snippet faqs/galaxy/datasets_change_datatype.md datatype="mp4" %}
>    >
>    {: .tip}
{: .hands_on}

# Annotation automatique avec SAM3

SAM3 est un modèle de segmentation guidé par texte. Il détecte et segmente automatiquement les objets correspondant à votre prompt, sans nécessiter d'annotations préalables.

> <tip-title>Un tutoriel dédié à SAM3 existe</tip-title>
>
> Si vous souhaitez en savoir plus sur SAM3 et ses paramètres, consultez le tutoriel dédié :
> [Tutoriel SAM3](https://training.galaxyproject.org/training-material/topics/ecology/tutorials/SAM3/tutorial.html)
>
{: .tip}

> <hands-on-title>Paramétrage de SAM3</hands-on-title>
>
> 1. {% tool [SAM3 Semantic Segmentation](toolshed.g2.bx.psu.edu/repos/ecology/sam3_semantic_segmentation/sam3_semantic_segmentation/1.0.1+galaxy4) %} avec ces paramètres :
>
>    - {% icon param-file %} *"Model data"* : `Segment Anything Model 3 (SAM 3)` (par défaut)
>    - {% icon param-select %} *"Input type"* : `One video`
>    - {% icon param-file %} *"Input video file"* : `Lieujaune-PorzBreign-GOPR5167_Edited.mp4`
>    - {% icon param-select %} *"Video quality"* : `4000k - Good (1080p)`
>    - {% icon param-select %} *"COCO output mode"* : `Annotate extracted frames — saves JPGs and one COCO entry per frame image`
>    - {% icon param-select %} *"Additional output formats"* : `Empty` (par défaut)
>    - {% icon param-text %} *"Text prompt"* : `fish`
>    - {% icon version %} *"Confidence threshold"* : `0.5`
>    - {% icon version %} *"Video frame stride"* : `5`
>    - {% icon param-toggle %} *"Show bounding boxes on annotated output"* : `Yes`
>    - {% icon param-toggle %} *"Normalize outputs?"* : `No` (par défaut)
>
> 2. Cliquez sur **Execute**
>
>    > <comment-title>Temps de traitement</comment-title>
>    >
>    > Le traitement peut prendre plusieurs minutes selon la longueur de la vidéo et les ressources du serveur. Patientez jusqu'à ce que les sorties apparaissent en vert dans l'historique.
>    >
>    {: .comment}
>
> 3. Une fois terminé, vous obtenez dans votre historique :
>    - **32: Annotation COCO** : `annotations.json` — les masques de segmentation au format COCO
>    - **31: Annotated Outputs** : la vidéo annotée pour vérification visuelle
>    - **30: COCO Extracted Frames** : la collection d'images extraites (non annotées)
>
> 4. Vérifiez visuellement les résultats en cliquant sur {% icon galaxy-eye %} sur **Annotated Outputs**
>
>    ![Résultat de segmentation SAM3 sur la vidéo](../../images/Annotation_AI_Pipeline/Visualize_output_SAM3-ezgif.com-video-to-gif-converter.gif){: style="width:75%; display:block; margin:auto;"}
>
>    > <comment-title>Limites de SAM3</comment-title>
>    >
>    > Comme vous pouvez le constater, toutes les annotations ne sont pas parfaites, les faux positifs sont le problème le plus fréquent. C'est pourquoi l'étape suivante de correction et validation est indispensable.
>    >
>    {: .comment}
>
{: .hands_on}

# Correction et validation des annotations

Nous allons voir 2 outils complémentaires pour corriger les annotations. Ils ne sont pas opposés : vous pouvez les utiliser l'un après l'autre, ou n'en utiliser qu'un seul selon vos besoins.

## Corriger avec Edit COCO Annotation

L'outil **Edit COCO Annotation** permet de modifier les annotations COCO sans avoir à ouvrir les images. Il propose trois modes :

- **Keep** : garder uniquement les IDs listés (supprimer tous les autres) — utile quand peu d'IDs sont à conserver, permet aussi de les renommer
- **Remove** : supprimer uniquement les IDs listés
- **Rename** : renommer les IDs listés sans en supprimer d'autres

> <hands-on-title>Garder, supprimer ou renommer des annotations</hands-on-title>
>
> 1. {% tool [Edit COCO Annotation](edit_coco_annotation) %} avec ces paramètres :
>
>    - {% icon param-select %} *"Mode"* : `Keep - keep only listed IDs (remove all others)`
>    - {% icon param-repeat %} *"1: Track to keep"*
>        - {% icon param-text %} *"Track IDs"* : `0,4-6`
>        - {% icon param-text %} *"Rename"* : `Gobiusculus flavescens`
>        - {% icon param-text %} *"Frame min"* : `Empty` (par défaut)
>        - {% icon param-text %} *"Frame max"* : `Empty` (par défaut)
>    - {% icon param-repeat %} *"2: Track to keep"*
>        - {% icon param-text %} *"Track IDs"* : `2`
>        - {% icon param-text %} *"Rename"* : `Crenilabre`
>        - {% icon param-text %} *"Frame min"* : `Empty` (par défaut)
>        - {% icon param-text %} *"Frame max"* : `Empty` (par défaut)
>
>    > <comment-title>Résultat équivalent en deux étapes</comment-title>
>    >
>    > Il est aussi possible d'obtenir le même résultat en deux étapes : utiliser **Remove** pour supprimer les IDs 1 et 3, puis **Rename** pour renommer les groupes restants.
>    >
>    > À noter : SAM3 peut parfois attribuer le même Track ID à deux objets différents à des moments distincts de la vidéo. C'est pour cette raison que les paramètres **Frame min** et **Frame max** ont été ajoutés.
>    >
>    {: .comment}
>
> 2. Une fois terminé, vous obtenez dans votre historique :
>    - **58: Edited COCO annotations** : Le JSON format COCO modifier d'après les entrées
>
{: .hands_on}

### Visualiser les annotations avec COCO Annotation Visualizer

Cet outil permet de visualiser facilement un fichier JSON au format COCO superposé sur une vidéo ou des images. Nous l'utilisons ici pour vérifier nos annotations après correction.

> <hands-on-title>Visualiser les annotations COCO</hands-on-title>
>
> 1. {% tool [COCO Annotation Visualizer](coco_annotation_visualizer) %} avec ces paramètres :
>
>    - {% icon param-select %} *"Input Type"* : `One Video`
>    - {% icon param-file %} *"Input video file"* : `Lieujaune-PorzBreign-GOPR5167_Edited.mp4`
>    - {% icon version %} *"Frame stride"* : `5`
>    - {% icon param-file %} *"COCO annotation file"* : `Edited COCO annotations`
>    - {% icon param-text %} *"Filter categories"* : `Empty` (par défaut)
>    - Dans *"Display options"* :
>        - {% icon param-toggle %} *"Show bounding boxes"* : `No`
>        - {% icon param-toggle %} *"Show segmentation masks"* : `Yes` (par défaut)
>        - {% icon param-toggle %} *"Show category labels"* : `Yes` (par défaut)
>        - {% icon param-toggle %} *"Show annotation count"* : `No` (par défaut)
>        - {% icon version %} *"Mask opacity"* : `0.4` (par défaut)
>        - {% icon version %} *"Bounding box thickness"* : `2` (par défaut)
>        - {% icon version %} *"Label font scale"* : `0.6` (par défaut)
>        - {% icon param-select %} *"Color mode"* : `Per instance (different color for each annotation)`
>        - {% icon param-select %} *"Output image format"* : `PNG (lossless)` (par défaut)
>    - {% icon param-select %} *"Output mode"* : `Both (frames and video)`
>    - {% icon version %} *"Video frame rate (FPS)"* : `5.0`
>    - {% icon param-toggle %} *"Annotated frames only"* : `Yes` (par défaut)
>
{: .hands_on}

## Corriger manuellement avec AnyLabeling

AnyLabeling est un outil interactif qui permet de corriger les annotations directement sur les images, incluant la modification des masques de segmentation. Il offre plus de liberté qu'Edit COCO, mais demande plus de temps.

> <comment-title>Limites d'AnyLabeling pour les vidéos</comment-title>
>
> AnyLabeling présente deux limitations importantes dans ce contexte :
> - La conversion du format COCO vers LabelMe entraîne la **perte des Track IDs**
> - AnyLabeling n'est pas conçu pour les vidéos : sans tracking, vous devrez corriger les annotations **frame par frame**
>
{: .comment}

### Convertir les annotations COCO en format LabelMe

Le format COCO est un unique fichier JSON contenant toutes les annotations. Le format LabelMe, lui, requiert **un fichier JSON par frame**, nommé exactement comme l'image correspondante (à l'extension près).

> <hands-on-title>Conversion COCO vers LabelMe</hands-on-title>
>
> 1. {% tool [COCO to LabelMe JSON Converter](coco2labelme) %} avec ces paramètres :
>
>    - {% icon param-file %} *"COCO annotation file"* : `Annotation COCO`
>    - {% icon param-select %} *"Image path mode"* : `Custom - user defined base path`
>    - {% icon param-text %} *"Custom base path"* : `Empty` (laisser vide)
>
>    > <comment-title>Pourquoi laisser le chemin vide ?</comment-title>
>    >
>    > AnyLabeling cherche les images dans le même dossier que les annotations. En laissant le chemin vide, le nom de la frame est utilisé directement, ce qui correspond à l'organisation des fichiers dans l'outil interactif.
>    >
>    {: .comment}
>
{: .hands_on}

### Corriger les annotations avec AnyLabeling

AnyLabeling est un outil interactif : Galaxy lance une application dans un environnement virtuel que vous contrôlez depuis votre navigateur. Une fois vos corrections terminées, vous fermez l'application et les données sont renvoyées dans votre historique Galaxy.

> <hands-on-title>Annotation manuelle via AnyLabeling</hands-on-title>
>
> 1. {% tool [AnyLabeling Interactive](interactive_tool_anylabeling) %} avec ces paramètres :
>
>    - {% icon param-file %} *"Input images"* : `COCO Extracted Frames`
>    - {% icon param-file %} *"Pre-existing annotations"* : `LabelMe JSON annotations`
>    - {% icon param-file %} *"Classes file (one class per line)"* : `Empty` (par défaut)
>    - {% icon param-file %} *"A tarball containing custom model files and yaml files"* : `Empty` (par défaut)
>
{: .hands_on}

# Conclusion

Vous savez maintenant utiliser ce pipeline complet pour annoter des vidéos d'espèces marines :

- **SAM3** génère automatiquement des annotations à partir d'un simple prompt textuel
- **Edit COCO** permet de nettoyer rapidement les annotations en gardant, supprimant ou renommant des IDs de tracks
- **COCO Annotation Visualizer** permet de vérifier visuellement les résultats à chaque étape
- **AnyLabeling** offre une correction manuelle fine, frame par frame, pour les cas complexes

Ce pipeline est réutilisable pour d'autres espèces ou d'autres projets d'imagerie marine — il suffit d'adapter le prompt et les paramètres de confiance de SAM3.