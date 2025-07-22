---
layout: tutorial_hands_on

title: Automating Antarctic Environmental Reports with Galaxy

questions:
- How can the production of environmental figures for Antarctica be automated?
- What Galaxy tools and FAIR practices are essential for reproducible environmental analyses?
- How can Jupyter notebooks be integrated into Galaxy workflows to generate reproducible reports?

objectives:
- Understand how FAIR principles apply to environmental data analysis
- Learn how to create and integrate a Jupyter notebook into a Galaxy workflow
- Be able to design a modular and automated workflow to generate scientific figures
- Learn how to use climatic and biological data sources within Galaxy
- Generate a structured Markdown report from a Galaxy workflow execution

time_estimation: 3H

key_points:
- Galaxy enables automation of reproducible analyses through its workflow system
- Integration of Jupyter notebooks within Galaxy enhances modularity and traceability
- The CCAMLR environmental report can be reproduced automatically in a FAIR-compliant way
- The “two-step” method allows rapid prototyping of tools using Jupyter before formal Galaxy integration
- Using open data sources and parameterized scripts improves reusability and standardization
license: MIT

contributors:
- Arthur BARREAU
- Yvan LE BRAS
- Marc ELEAUME
---

Ce tuto a pour but d'aider à l'utilisation d'un Workflow bie précis: [State environt Antarctic](https://ecology.usegalaxy.eu/u/arthurb/w/worflow-for-representig-state-of-the-environment-in-antarctic)
La fabrication du workflow est issus d'un stage de M2 sur l'automatisation d'un rapport type sur l'etat de l'nevironement et des ressoucreces marines dans la zone 48 en Antarctic, proposé par la delegation britanique à la CCAMLR, l'idée et d'automatiser la production des figures pour faciliter la production du rapport dans le temps. 

Le workflow est donc en cours de production est je vais expliquer comment ul'utiliser 

<!-- This is a comment. -->

> <agenda-title></agenda-title>
>
> In this tutorial, we will cover:
>
> 1. TOC
> {:toc}
>
{: .agenda}

# Pour les non initié

Pour tout personne n'ayant jamais touché Galaxy je vais presenté rapidement comment ca fonctionne est ce que tu dois faire pour faire le adeuxieme étapes 

> <hands-on-title> Create account </hands-on-title>
>
> Commencer par aller sur le site de Galaxy, je vous propose d'uiliser le serveur europe et le sous domaine ecology: (ecology.usegalaxy)[https://ecology.usegalaxy.eu/]
> Ensuite si vous n'avez pas un compte il faut en creer un, pour cela cliquer sur "Login or Registrer"
>![Image site](./Images/intergace.png)
> Cela ouvrira la page de login, sur celle ci vous prerai creer un compte en appyant sur le vbouton en bas à gauche 
>![Image site](./Images/register.png)

Lorsque les etapes suivantes sont faits, et que votre compte est bien créer nous pouvons passer à l'etape suivantes qui est un rapide cours de Galaxy 

![Image site](./Images/interface_explication.png)
La barre latérale, située à gauche, donne accès aux fonctions essentielles
de la plateforme, telles que les outils interactifs (« Interactive Tools », en violet), l’importation
de fichiers (« Upload », en rouge), la bibliothèque d’outils disponibles (« Tools », en bleu),
ou encore la section dédiée aux workflows (« Workflow », en vert). Cette dernière permet de
naviguer entre ses propres workflows et ceux partagés par la communauté.
À droite de l’écran, le panneau d’historique (« History »,en rose) permet de suivre l’en-
semble des éléments liés à l’analyse en cours : données importées, outils exécutés, résultats
générés. Il est possible de gérer plusieurs historiques en parallèle afin d’organiser distinctement
les différentes étapes ou versions d’un même projet.Au centre de l’interface se trouve l’espace
de visualisation du workflow. Chaque étape y apparaît de manière visuelle et modulaire. Un
soin particulier a été apporté à la clarté de cette représentation. Les options proposées à l’utili-
sateur, comme l’activation ou non de la génération d’une figure ou la définition de zones géo-
graphiques à analyser, ont été pensées pour être facilement compréhensibles et configurables
> <hands-on-title> Create account </hands-on-title>
>
>
>
>



# Hands-on Sections
Below are a series of hand-on boxes, one for each tool in your workflow file.
Often you may wish to combine several boxes into one or make other adjustments such
as breaking the tutorial into sections, we encourage you to make such changes as you
see fit, this is just a starting point :)

Anywhere you find the word "***TODO***", there is something that needs to be changed
depending on the specifics of your tutorial.

have fun!

## Get data

> <hands-on-title> Data Upload </hands-on-title>
>
> 1. Create a new history for this tutorial
> 2. Import the files from [Zenodo]({{ page.zenodo_link }}) or from
>    the shared data library (`GTN - Material` -> `{{ page.topic_name }}`
>     -> `{{ page.title }}`):
>
>    ```
>    
>    ```
>    ***TODO***: *Add the files by the ones on Zenodo here (if not added)*
>
>    ***TODO***: *Remove the useless files (if added)*
>
>    {% snippet faqs/galaxy/datasets_import_via_link.md %}
>
>    {% snippet faqs/galaxy/datasets_import_from_data_library.md %}
>
> 3. Rename the datasets
> 4. Check that the datatype
>
>    {% snippet faqs/galaxy/datasets_change_datatype.md datatype="datatypes" %}
>
> 5. Add to each database a tag corresponding to ...
>
>    {% snippet faqs/galaxy/datasets_add_tag.md %}
>
{: .hands_on}

# Title of the section usually corresponding to a big step in the analysis

It comes first a description of the step: some background and some theory.
Some image can be added there to support the theory explanation:

![Alternative text](../../images/image_name "Legend of the image")

The idea is to keep the theory description before quite simple to focus more on the practical part.

***TODO***: *Consider adding a detail box to expand the theory*

> <details-title> More details about the theory </details-title>
>
> But to describe more details, it is possible to use the detail boxes which are expandable
>
{: .details}

A big step can have several subsections or sub steps:

## Sub-step with **Interactive JupyterLab Notebook**

> <hands-on-title> Task description </hands-on-title>
>
> 1. {% tool [Interactive JupyterLab Notebook](interactive_tool_jupyter_notebook) %} with the following parameters:
>    - *"Do you already have a notebook?"*: `Load a previous notebook`
>        - {% icon param-file %} *"IPython Notebook"*: `output` (output of **Collapse Collection** {% icon tool %})
>        - *"Execute notebook and return a new one."*: `Yes`
>    - In *"User inputs"*:
>        - {% icon param-repeat %} *"Insert User inputs"*
>            - *"Name for parameter"*: `subarea`
>            - *"Choose the input type"*: `Text`
>                - *"Select value"*: `{'id': 3, 'output_name': 'output'}`
>        - {% icon param-repeat %} *"Insert User inputs"*
>            - *"Name for parameter"*: `multiplier`
>            - *"Choose the input type"*: `Integer`
>                - *"Select value"*: `{'id': 4, 'output_name': 'output'}`
>        - {% icon param-repeat %} *"Insert User inputs"*
>            - *"Name for parameter"*: `ZoneXASD`
>            - *"Choose the input type"*: `Boolean`
>                - *"Select value"*: `Yes`
>        - {% icon param-repeat %} *"Insert User inputs"*
>            - *"Name for parameter"*: `LabelXASD`
>            - *"Choose the input type"*: `Boolean`
>                - *"Select value"*: `Yes`
>        - {% icon param-repeat %} *"Insert User inputs"*
>            - *"Name for parameter"*: `ZoneXCEMP`
>            - *"Choose the input type"*: `Boolean`
>                - *"Select value"*: `Yes`
>        - {% icon param-repeat %} *"Insert User inputs"*
>            - *"Name for parameter"*: `bbox`
>            - *"Choose the input type"*: `Boolean`
>                - *"Select value"*: `Yes`
>
>    ***TODO***: *Check parameter descriptions*
>
>    ***TODO***: *Consider adding a comment or tip box*
>
>    > <comment-title> short description </comment-title>
>    >
>    > A comment about the tool or something else. This box can also be in the main text
>    {: .comment}
>
{: .hands_on}

***TODO***: *Consider adding a question to test the learners understanding of the previous exercise*

> <question-title></question-title>
>
> 1. Question1?
> 2. Question2?
>
> > <solution-title></solution-title>
> >
> > 1. Answer for question1
> > 2. Answer for question2
> >
> {: .solution}
>
{: .question}

## Sub-step with **Interactive JupyterLab Notebook**

> <hands-on-title> Task description </hands-on-title>
>
> 1. {% tool [Interactive JupyterLab Notebook](interactive_tool_jupyter_notebook) %} with the following parameters:
>    - *"Do you already have a notebook?"*: `Load a previous notebook`
>        - {% icon param-file %} *"IPython Notebook"*: `output` (output of **Collapse Collection** {% icon tool %})
>        - *"Execute notebook and return a new one."*: `Yes`
>    - In *"User inputs"*:
>        - {% icon param-repeat %} *"Insert User inputs"*
>            - *"Name for parameter"*: `year`
>            - *"Additional optional description"*: `Enter the year (e.g.,'1978' to '2024')`
>            - *"Choose the input type"*: `Text`
>                - *"Select value"*: `{'id': 11, 'output_name': 'output'}`
>        - {% icon param-repeat %} *"Insert User inputs"*
>            - *"Name for parameter"*: `color`
>            - *"Choose the input type"*: `Optional Text`
>                - *"Select value"*: `{'id': 12, 'output_name': 'output'}`
>        - {% icon param-repeat %} *"Insert User inputs"*
>            - *"Name for parameter"*: `median`
>            - *"Choose the input type"*: `Text`
>                - *"Select value"*: `{'id': 13, 'output_name': 'output'}`
>        - {% icon param-repeat %} *"Insert User inputs"*
>            - *"Name for parameter"*: `pngWidth`
>            - *"Choose the input type"*: `Optional Integer`
>                - *"Select value"*: `{'id': 14, 'output_name': 'output'}`
>        - {% icon param-repeat %} *"Insert User inputs"*
>            - *"Name for parameter"*: `pngHeight`
>            - *"Choose the input type"*: `Optional Integer`
>                - *"Select value"*: `{'id': 15, 'output_name': 'output'}`
>        - {% icon param-repeat %} *"Insert User inputs"*
>            - *"Name for parameter"*: `pngResolution`
>            - *"Choose the input type"*: `Optional Integer`
>                - *"Select value"*: `{'id': 16, 'output_name': 'output'}`
>
>    ***TODO***: *Check parameter descriptions*
>
>    ***TODO***: *Consider adding a comment or tip box*
>
>    > <comment-title> short description </comment-title>
>    >
>    > A comment about the tool or something else. This box can also be in the main text
>    {: .comment}
>
{: .hands_on}

***TODO***: *Consider adding a question to test the learners understanding of the previous exercise*

> <question-title></question-title>
>
> 1. Question1?
> 2. Question2?
>
> > <solution-title></solution-title>
> >
> > 1. Answer for question1
> > 2. Answer for question2
> >
> {: .solution}
>
{: .question}

## Sub-step with **Interactive JupyterLab Notebook**

> <hands-on-title> Task description </hands-on-title>
>
> 1. {% tool [Interactive JupyterLab Notebook](interactive_tool_jupyter_notebook) %} with the following parameters:
>    - *"Do you already have a notebook?"*: `Load a previous notebook`
>        - {% icon param-file %} *"IPython Notebook"*: `output` (output of **Collapse Collection** {% icon tool %})
>        - *"Execute notebook and return a new one."*: `Yes`
>    - In *"User inputs"*:
>        - {% icon param-repeat %} *"Insert User inputs"*
>            - *"Name for parameter"*: `subarea`
>            - *"Choose the input type"*: `Text`
>                - *"Select value"*: `{'id': 19, 'output_name': 'output'}`
>        - {% icon param-repeat %} *"Insert User inputs"*
>            - *"Name for parameter"*: `year`
>            - *"Additional optional description"*: `Enter the year (e.g.,'1978' to '2024')`
>            - *"Choose the input type"*: `Text`
>                - *"Select value"*: `{'id': 20, 'output_name': 'output'}`
>        - {% icon param-repeat %} *"Insert User inputs"*
>            - *"Name for parameter"*: `month`
>            - *"Choose the input type"*: `Text`
>                - *"Select value"*: `{'id': 21, 'output_name': 'output'}`
>        - {% icon param-repeat %} *"Insert User inputs"*
>            - *"Name for parameter"*: `year_anomaly`
>            - *"Choose the input type"*: `Optional Text`
>                - *"Select value"*: `{'id': 22, 'output_name': 'output'}`
>        - {% icon param-repeat %} *"Insert User inputs"*
>            - *"Name for parameter"*: `Concentration and/or Anomalies`
>            - *"Choose the input type"*: `Text`
>                - *"Select value"*: `{'id': 23, 'output_name': 'output'}`
>        - {% icon param-repeat %} *"Insert User inputs"*
>            - *"Name for parameter"*: `multiplier`
>            - *"Choose the input type"*: `Integer`
>                - *"Select value"*: `{'id': 24, 'output_name': 'output'}`
>        - {% icon param-repeat %} *"Insert User inputs"*
>            - *"Name for parameter"*: `title`
>            - *"Choose the input type"*: `Text`
>                - *"Select value"*: `{'id': 25, 'output_name': 'output'}`
>        - {% icon param-repeat %} *"Insert User inputs"*
>            - *"Name for parameter"*: `legend_chose`
>            - *"Choose the input type"*: `Text`
>                - *"Select value"*: `{'id': 26, 'output_name': 'output'}`
>        - {% icon param-repeat %} *"Insert User inputs"*
>            - *"Name for parameter"*: `ASD`
>            - *"Choose the input type"*: `Boolean`
>                - *"Select value"*: `Yes`
>        - {% icon param-repeat %} *"Insert User inputs"*
>            - *"Name for parameter"*: `bbox`
>            - *"Choose the input type"*: `Boolean`
>                - *"Select value"*: `Yes`
>
>    ***TODO***: *Check parameter descriptions*
>
>    ***TODO***: *Consider adding a comment or tip box*
>
>    > <comment-title> short description </comment-title>
>    >
>    > A comment about the tool or something else. This box can also be in the main text
>    {: .comment}
>
{: .hands_on}

***TODO***: *Consider adding a question to test the learners understanding of the previous exercise*

> <question-title></question-title>
>
> 1. Question1?
> 2. Question2?
>
> > <solution-title></solution-title>
> >
> > 1. Answer for question1
> > 2. Answer for question2
> >
> {: .solution}
>
{: .question}

## Sub-step with **Interactive JupyterLab Notebook**

> <hands-on-title> Task description </hands-on-title>
>
> 1. {% tool [Interactive JupyterLab Notebook](interactive_tool_jupyter_notebook) %} with the following parameters:
>    - *"Do you already have a notebook?"*: `Load a previous notebook`
>        - {% icon param-file %} *"IPython Notebook"*: `output` (output of **Collapse Collection** {% icon tool %})
>        - *"Execute notebook and return a new one."*: `Yes`
>    - In *"User inputs"*:
>        - {% icon param-repeat %} *"Insert User inputs"*
>            - *"Name for parameter"*: `subarea`
>            - *"Choose the input type"*: `Optional Text`
>                - *"Select value"*: `{'id': 31, 'output_name': 'output'}`
>        - {% icon param-repeat %} *"Insert User inputs"*
>            - *"Name for parameter"*: `year`
>            - *"Additional optional description"*: `Enter the year (e.g.,'1978' to '2024')`
>            - *"Choose the input type"*: `Text`
>                - *"Select value"*: `{'id': 32, 'output_name': 'output'}`
>        - {% icon param-repeat %} *"Insert User inputs"*
>            - *"Name for parameter"*: `month`
>            - *"Choose the input type"*: `Text`
>                - *"Select value"*: `{'id': 33, 'output_name': 'output'}`
>        - {% icon param-repeat %} *"Insert User inputs"*
>            - *"Name for parameter"*: `nb_months`
>            - *"Choose the input type"*: `Integer`
>                - *"Select value"*: `{'id': 34, 'output_name': 'output'}`
>        - {% icon param-repeat %} *"Insert User inputs"*
>            - *"Name for parameter"*: `multiplier`
>            - *"Choose the input type"*: `Integer`
>                - *"Select value"*: `{'id': 35, 'output_name': 'output'}`
>        - {% icon param-repeat %} *"Insert User inputs"*
>            - *"Name for parameter"*: `title`
>            - *"Choose the input type"*: `Text`
>                - *"Select value"*: `{'id': 36, 'output_name': 'output'}`
>        - {% icon param-repeat %} *"Insert User inputs"*
>            - *"Name for parameter"*: `legend_chose`
>            - *"Choose the input type"*: `Text`
>                - *"Select value"*: `{'id': 37, 'output_name': 'output'}`
>        - {% icon param-repeat %} *"Insert User inputs"*
>            - *"Name for parameter"*: `Zone_ASD`
>            - *"Choose the input type"*: `Boolean`
>                - *"Select value"*: `Yes`
>        - {% icon param-repeat %} *"Insert User inputs"*
>            - *"Name for parameter"*: `Label_ASD`
>            - *"Choose the input type"*: `Boolean`
>                - *"Select value"*: `Yes`
>        - {% icon param-repeat %} *"Insert User inputs"*
>            - *"Name for parameter"*: `bbox`
>            - *"Choose the input type"*: `Boolean`
>                - *"Select value"*: `Yes`
>
>    ***TODO***: *Check parameter descriptions*
>
>    ***TODO***: *Consider adding a comment or tip box*
>
>    > <comment-title> short description </comment-title>
>    >
>    > A comment about the tool or something else. This box can also be in the main text
>    {: .comment}
>
{: .hands_on}

***TODO***: *Consider adding a question to test the learners understanding of the previous exercise*

> <question-title></question-title>
>
> 1. Question1?
> 2. Question2?
>
> > <solution-title></solution-title>
> >
> > 1. Answer for question1
> > 2. Answer for question2
> >
> {: .solution}
>
{: .question}

## Sub-step with **Interactive JupyterLab Notebook**

> <hands-on-title> Task description </hands-on-title>
>
> 1. {% tool [Interactive JupyterLab Notebook](interactive_tool_jupyter_notebook) %} with the following parameters:
>    - *"Do you already have a notebook?"*: `Load a previous notebook`
>        - {% icon param-file %} *"IPython Notebook"*: `output` (output of **Collapse Collection** {% icon tool %})
>        - *"Execute notebook and return a new one."*: `Yes`
>    - In *"User inputs"*:
>        - {% icon param-repeat %} *"Insert User inputs"*
>            - *"Name for parameter"*: `year`
>            - *"Additional optional description"*: `Enter the year (e.g.,'1978' to '2024')`
>            - *"Choose the input type"*: `Text`
>                - *"Select value"*: `{'id': 45, 'output_name': 'output'}`
>        - {% icon param-repeat %} *"Insert User inputs"*
>            - *"Name for parameter"*: `subarea`
>            - *"Choose the input type"*: `Optional Text`
>                - *"Select value"*: `{'id': 46, 'output_name': 'output'}`
>
>    ***TODO***: *Check parameter descriptions*
>
>    ***TODO***: *Consider adding a comment or tip box*
>
>    > <comment-title> short description </comment-title>
>    >
>    > A comment about the tool or something else. This box can also be in the main text
>    {: .comment}
>
{: .hands_on}

***TODO***: *Consider adding a question to test the learners understanding of the previous exercise*

> <question-title></question-title>
>
> 1. Question1?
> 2. Question2?
>
> > <solution-title></solution-title>
> >
> > 1. Answer for question1
> > 2. Answer for question2
> >
> {: .solution}
>
{: .question}

## Sub-step with **Interactive JupyterLab Notebook**

> <hands-on-title> Task description </hands-on-title>
>
> 1. {% tool [Interactive JupyterLab Notebook](interactive_tool_jupyter_notebook) %} with the following parameters:
>    - *"Do you already have a notebook?"*: `Load a previous notebook`
>        - {% icon param-file %} *"IPython Notebook"*: `output` (output of **Collapse Collection** {% icon tool %})
>        - *"Execute notebook and return a new one."*: `Yes`
>    - In *"User inputs"*:
>        - {% icon param-repeat %} *"Insert User inputs"*
>            - *"Name for parameter"*: `year`
>            - *"Additional optional description"*: `Enter the year (e.g.,'1978' to '2024')`
>            - *"Choose the input type"*: `Text`
>                - *"Select value"*: `{'id': 51, 'output_name': 'output'}`
>        - {% icon param-repeat %} *"Insert User inputs"*
>            - *"Name for parameter"*: `subarea`
>            - *"Choose the input type"*: `Optional Text`
>                - *"Select value"*: `{'id': 52, 'output_name': 'output'}`
>
>    ***TODO***: *Check parameter descriptions*
>
>    ***TODO***: *Consider adding a comment or tip box*
>
>    > <comment-title> short description </comment-title>
>    >
>    > A comment about the tool or something else. This box can also be in the main text
>    {: .comment}
>
{: .hands_on}

***TODO***: *Consider adding a question to test the learners understanding of the previous exercise*

> <question-title></question-title>
>
> 1. Question1?
> 2. Question2?
>
> > <solution-title></solution-title>
> >
> > 1. Answer for question1
> > 2. Answer for question2
> >
> {: .solution}
>
{: .question}

## Sub-step with **Interactive JupyterLab Notebook**

> <hands-on-title> Task description </hands-on-title>
>
> 1. {% tool [Interactive JupyterLab Notebook](interactive_tool_jupyter_notebook) %} with the following parameters:
>    - *"Do you already have a notebook?"*: `Load a previous notebook`
>        - {% icon param-file %} *"IPython Notebook"*: `output` (output of **Collapse Collection** {% icon tool %})
>        - *"Execute notebook and return a new one."*: `Yes`
>    - In *"User inputs"*:
>        - {% icon param-repeat %} *"Insert User inputs"*
>            - *"Name for parameter"*: `Copernicus Data User`
>            - *"Choose the input type"*: `Optional Dataset`
>                - {% icon param-file %} *"Select value"*: `output` (Input dataset)
>        - {% icon param-repeat %} *"Insert User inputs"*
>            - *"Name for parameter"*: `subarea`
>            - *"Choose the input type"*: `Text`
>                - *"Select value"*: `{'id': 56, 'output_name': 'output'}`
>        - {% icon param-repeat %} *"Insert User inputs"*
>            - *"Name for parameter"*: `MonthXmode`
>            - *"Choose the input type"*: `Text`
>                - *"Select value"*: `{'id': 57, 'output_name': 'output'}`
>        - {% icon param-repeat %} *"Insert User inputs"*
>            - *"Name for parameter"*: `multiplier`
>            - *"Choose the input type"*: `Integer`
>                - *"Select value"*: `{'id': 58, 'output_name': 'output'}`
>        - {% icon param-repeat %} *"Insert User inputs"*
>            - *"Name for parameter"*: `index_max`
>            - *"Choose the input type"*: `Floating point`
>                - *"Select value"*: `{'id': 59, 'output_name': 'output'}`
>        - {% icon param-repeat %} *"Insert User inputs"*
>            - *"Name for parameter"*: `legend_position`
>            - *"Choose the input type"*: `Text`
>                - *"Select value"*: `{'id': 60, 'output_name': 'output'}`
>        - {% icon param-repeat %} *"Insert User inputs"*
>            - *"Name for parameter"*: `nb_row`
>            - *"Choose the input type"*: `Optional Integer`
>                - *"Select value"*: `{'id': 61, 'output_name': 'output'}`
>        - {% icon param-repeat %} *"Insert User inputs"*
>            - *"Name for parameter"*: `title`
>            - *"Choose the input type"*: `Boolean`
>                - *"Select value"*: `Yes`
>        - {% icon param-repeat %} *"Insert User inputs"*
>            - *"Name for parameter"*: `ASD`
>            - *"Choose the input type"*: `Boolean`
>                - *"Select value"*: `Yes`
>        - {% icon param-repeat %} *"Insert User inputs"*
>            - *"Name for parameter"*: `bbox`
>            - *"Choose the input type"*: `Boolean`
>                - *"Select value"*: `Yes`
>
>    ***TODO***: *Check parameter descriptions*
>
>    ***TODO***: *Consider adding a comment or tip box*
>
>    > <comment-title> short description </comment-title>
>    >
>    > A comment about the tool or something else. This box can also be in the main text
>    {: .comment}
>
{: .hands_on}

***TODO***: *Consider adding a question to test the learners understanding of the previous exercise*

> <question-title></question-title>
>
> 1. Question1?
> 2. Question2?
>
> > <solution-title></solution-title>
> >
> > 1. Answer for question1
> > 2. Answer for question2
> >
> {: .solution}
>
{: .question}

## Sub-step with **Interactive JupyterLab Notebook**

> <hands-on-title> Task description </hands-on-title>
>
> 1. {% tool [Interactive JupyterLab Notebook](interactive_tool_jupyter_notebook) %} with the following parameters:
>    - *"Do you already have a notebook?"*: `Load a previous notebook`
>        - {% icon param-file %} *"IPython Notebook"*: `output` (output of **Collapse Collection** {% icon tool %})
>        - *"Execute notebook and return a new one."*: `Yes`
>    - In *"User inputs"*:
>        - {% icon param-repeat %} *"Insert User inputs"*
>            - *"Name for parameter"*: `data`
>            - *"Choose the input type"*: `Optional Dataset`
>                - {% icon param-file %} *"Select value"*: `output` (Input dataset)
>        - {% icon param-repeat %} *"Insert User inputs"*
>            - *"Name for parameter"*: `plotXmode`
>            - *"Choose the input type"*: `Text`
>                - *"Select value"*: `{'id': 68, 'output_name': 'output'}`
>        - {% icon param-repeat %} *"Insert User inputs"*
>            - *"Name for parameter"*: `col_chose`
>            - *"Choose the input type"*: `Optional Text`
>                - *"Select value"*: `{'id': 69, 'output_name': 'output'}`
>        - {% icon param-repeat %} *"Insert User inputs"*
>            - *"Name for parameter"*: `keyword`
>            - *"Choose the input type"*: `Optional Text`
>                - *"Select value"*: `{'id': 70, 'output_name': 'output'}`
>        - {% icon param-repeat %} *"Insert User inputs"*
>            - *"Name for parameter"*: `title`
>            - *"Choose the input type"*: `Optional Text`
>                - *"Select value"*: `{'id': 71, 'output_name': 'output'}`
>        - {% icon param-repeat %} *"Insert User inputs"*
>            - *"Name for parameter"*: `x`
>            - *"Choose the input type"*: `Optional Text`
>                - *"Select value"*: `{'id': 72, 'output_name': 'output'}`
>        - {% icon param-repeat %} *"Insert User inputs"*
>            - *"Name for parameter"*: `y`
>            - *"Choose the input type"*: `Optional Text`
>                - *"Select value"*: `{'id': 73, 'output_name': 'output'}`
>        - {% icon param-repeat %} *"Insert User inputs"*
>            - *"Name for parameter"*: `log`
>            - *"Choose the input type"*: `Boolean`
>                - *"Select value"*: `Yes`
>        - {% icon param-repeat %} *"Insert User inputs"*
>            - *"Name for parameter"*: `year`
>            - *"Choose the input type"*: `Optional Text`
>                - *"Select value"*: `{'id': 75, 'output_name': 'output'}`
>        - {% icon param-repeat %} *"Insert User inputs"*
>            - *"Name for parameter"*: `width`
>            - *"Choose the input type"*: `Integer`
>                - *"Select value"*: `{'id': 76, 'output_name': 'output'}`
>        - {% icon param-repeat %} *"Insert User inputs"*
>            - *"Name for parameter"*: `res`
>            - *"Choose the input type"*: `Integer`
>                - *"Select value"*: `{'id': 77, 'output_name': 'output'}`
>
>    ***TODO***: *Check parameter descriptions*
>
>    ***TODO***: *Consider adding a comment or tip box*
>
>    > <comment-title> short description </comment-title>
>    >
>    > A comment about the tool or something else. This box can also be in the main text
>    {: .comment}
>
{: .hands_on}

***TODO***: *Consider adding a question to test the learners understanding of the previous exercise*

> <question-title></question-title>
>
> 1. Question1?
> 2. Question2?
>
> > <solution-title></solution-title>
> >
> > 1. Answer for question1
> > 2. Answer for question2
> >
> {: .solution}
>
{: .question}

## Sub-step with **Interactive JupyterLab Notebook**

> <hands-on-title> Task description </hands-on-title>
>
> 1. {% tool [Interactive JupyterLab Notebook](interactive_tool_jupyter_notebook) %} with the following parameters:
>    - *"Do you already have a notebook?"*: `Load a previous notebook`
>        - {% icon param-file %} *"IPython Notebook"*: `output` (output of **Collapse Collection** {% icon tool %})
>        - *"Execute notebook and return a new one."*: `Yes`
>    - In *"User inputs"*:
>        - {% icon param-repeat %} *"Insert User inputs"*
>            - *"Name for parameter"*: `data`
>            - *"Choose the input type"*: `Optional Dataset`
>                - {% icon param-file %} *"Select value"*: `output` (Input dataset)
>        - {% icon param-repeat %} *"Insert User inputs"*
>            - *"Name for parameter"*: `col_Y`
>            - *"Choose the input type"*: `Text`
>                - *"Select value"*: `{'id': 81, 'output_name': 'output'}`
>        - {% icon param-repeat %} *"Insert User inputs"*
>            - *"Name for parameter"*: `ylim`
>            - *"Choose the input type"*: `Optional Text`
>                - *"Select value"*: `{'id': 82, 'output_name': 'output'}`
>        - {% icon param-repeat %} *"Insert User inputs"*
>            - *"Name for parameter"*: `col_groupe_line`
>            - *"Choose the input type"*: `Optional Text`
>                - *"Select value"*: `{'id': 83, 'output_name': 'output'}`
>        - {% icon param-repeat %} *"Insert User inputs"*
>            - *"Name for parameter"*: `col_groupe_point`
>            - *"Choose the input type"*: `Optional Text`
>                - *"Select value"*: `{'id': 84, 'output_name': 'output'}`
>        - {% icon param-repeat %} *"Insert User inputs"*
>            - *"Name for parameter"*: `title`
>            - *"Choose the input type"*: `Optional Text`
>                - *"Select value"*: `{'id': 85, 'output_name': 'output'}`
>        - {% icon param-repeat %} *"Insert User inputs"*
>            - *"Name for parameter"*: `x`
>            - *"Choose the input type"*: `Optional Text`
>                - *"Select value"*: `{'id': 86, 'output_name': 'output'}`
>        - {% icon param-repeat %} *"Insert User inputs"*
>            - *"Name for parameter"*: `y`
>            - *"Choose the input type"*: `Optional Text`
>                - *"Select value"*: `{'id': 87, 'output_name': 'output'}`
>        - {% icon param-repeat %} *"Insert User inputs"*
>            - *"Name for parameter"*: `width`
>            - *"Choose the input type"*: `Integer`
>                - *"Select value"*: `{'id': 88, 'output_name': 'output'}`
>        - {% icon param-repeat %} *"Insert User inputs"*
>            - *"Name for parameter"*: `res`
>            - *"Choose the input type"*: `Integer`
>                - *"Select value"*: `{'id': 89, 'output_name': 'output'}`
>
>    ***TODO***: *Check parameter descriptions*
>
>    ***TODO***: *Consider adding a comment or tip box*
>
>    > <comment-title> short description </comment-title>
>    >
>    > A comment about the tool or something else. This box can also be in the main text
>    {: .comment}
>
{: .hands_on}

***TODO***: *Consider adding a question to test the learners understanding of the previous exercise*

> <question-title></question-title>
>
> 1. Question1?
> 2. Question2?
>
> > <solution-title></solution-title>
> >
> > 1. Answer for question1
> > 2. Answer for question2
> >
> {: .solution}
>
{: .question}

## Sub-step with **Interactive JupyterLab Notebook**

> <hands-on-title> Task description </hands-on-title>
>
> 1. {% tool [Interactive JupyterLab Notebook](interactive_tool_jupyter_notebook) %} with the following parameters:
>    - *"Do you already have a notebook?"*: `Load a previous notebook`
>        - {% icon param-file %} *"IPython Notebook"*: `output` (output of **Collapse Collection** {% icon tool %})
>        - *"Execute notebook and return a new one."*: `Yes`
>    - In *"User inputs"*:
>        - {% icon param-repeat %} *"Insert User inputs"*
>            - *"Name for parameter"*: `graph_selection`
>            - *"Choose the input type"*: `Text`
>                - *"Select value"*: `{'id': 44, 'output_name': 'output'}`
>        - {% icon param-repeat %} *"Insert User inputs"*
>            - *"Name for parameter"*: `year`
>            - *"Additional optional description"*: `Enter the year (e.g.,'1978' to '2024')`
>            - *"Choose the input type"*: `Text`
>                - *"Select value"*: `{'id': 45, 'output_name': 'output'}`
>        - {% icon param-repeat %} *"Insert User inputs"*
>            - *"Name for parameter"*: `subarea`
>            - *"Choose the input type"*: `Optional Text`
>                - *"Select value"*: `{'id': 46, 'output_name': 'output'}`
>        - {% icon param-repeat %} *"Insert User inputs"*
>            - *"Name for parameter"*: `txt`
>            - *"Choose the input type"*: `Optional Dataset`
>                - {% icon param-file %} *"Select value"*: `request` (output of **Copernicus Climate Data Store** {% icon tool %})
>        - {% icon param-repeat %} *"Insert User inputs"*
>            - *"Name for parameter"*: `data`
>            - *"Choose the input type"*: `Optional Dataset`
>                - {% icon param-file %} *"Select value"*: `ofilename` (output of **Copernicus Climate Data Store** {% icon tool %})
>
>    ***TODO***: *Check parameter descriptions*
>
>    ***TODO***: *Consider adding a comment or tip box*
>
>    > <comment-title> short description </comment-title>
>    >
>    > A comment about the tool or something else. This box can also be in the main text
>    {: .comment}
>
{: .hands_on}

***TODO***: *Consider adding a question to test the learners understanding of the previous exercise*

> <question-title></question-title>
>
> 1. Question1?
> 2. Question2?
>
> > <solution-title></solution-title>
> >
> > 1. Answer for question1
> > 2. Answer for question2
> >
> {: .solution}
>
{: .question}

## Sub-step with **Interactive JupyterLab Notebook**

> <hands-on-title> Task description </hands-on-title>
>
> 1. {% tool [Interactive JupyterLab Notebook](interactive_tool_jupyter_notebook) %} with the following parameters:
>    - *"Do you already have a notebook?"*: `Load a previous notebook`
>        - {% icon param-file %} *"IPython Notebook"*: `output` (output of **Collapse Collection** {% icon tool %})
>        - *"Execute notebook and return a new one."*: `Yes`
>    - In *"User inputs"*:
>        - {% icon param-repeat %} *"Insert User inputs"*
>            - *"Name for parameter"*: `graph_selection`
>            - *"Choose the input type"*: `Text`
>                - *"Select value"*: `{'id': 50, 'output_name': 'output'}`
>        - {% icon param-repeat %} *"Insert User inputs"*
>            - *"Name for parameter"*: `year`
>            - *"Additional optional description"*: `Enter the year (e.g.,'1978' to '2024')`
>            - *"Choose the input type"*: `Text`
>                - *"Select value"*: `{'id': 51, 'output_name': 'output'}`
>        - {% icon param-repeat %} *"Insert User inputs"*
>            - *"Name for parameter"*: `subarea`
>            - *"Choose the input type"*: `Optional Text`
>                - *"Select value"*: `{'id': 52, 'output_name': 'output'}`
>        - {% icon param-repeat %} *"Insert User inputs"*
>            - *"Name for parameter"*: `txt`
>            - *"Choose the input type"*: `Optional Dataset`
>                - {% icon param-file %} *"Select value"*: `request` (output of **Copernicus Climate Data Store** {% icon tool %})
>        - {% icon param-repeat %} *"Insert User inputs"*
>            - *"Name for parameter"*: `data`
>            - *"Choose the input type"*: `Optional Dataset`
>                - {% icon param-file %} *"Select value"*: `ofilename` (output of **Copernicus Climate Data Store** {% icon tool %})
>
>    ***TODO***: *Check parameter descriptions*
>
>    ***TODO***: *Consider adding a comment or tip box*
>
>    > <comment-title> short description </comment-title>
>    >
>    > A comment about the tool or something else. This box can also be in the main text
>    {: .comment}
>
{: .hands_on}

***TODO***: *Consider adding a question to test the learners understanding of the previous exercise*

> <question-title></question-title>
>
> 1. Question1?
> 2. Question2?
>
> > <solution-title></solution-title>
> >
> > 1. Answer for question1
> > 2. Answer for question2
> >
> {: .solution}
>
{: .question}


## Re-arrange

To create the template, each step of the workflow had its own subsection.

***TODO***: *Re-arrange the generated subsections into sections or other subsections.
Consider merging some hands-on boxes to have a meaningful flow of the analyses*

# Conclusion

Sum up the tutorial and the key takeaways here. We encourage adding an overview image of the
pipeline used.