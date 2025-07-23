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


> <hands-on-title> Create account </hands-on-title>
>
> Commencer par aller sur le site de Galaxy, je vous propose d'uiliser le serveur europe et le sous domaine ecology: (ecology.usegalaxy)[https://ecology.usegalaxy.eu/]
> Ensuite si vous n'avez pas un compte il faut en creer un, pour cela cliquer sur "Login or Registrer"
>![Image site](./Images/interface.png)
> Cela ouvrira la page de login, sur celle ci vous prerai creer un compte en appyant sur le vbouton en bas à gauche 
>![Image site](./Images/register.png)
{: .hands_on}

Lorsque les etapes suivantes sont faits, et que votre compte est bien créer nous pouvons passer à l'etape suivantes qui est un rapide cours de Galaxy 

![Image site](./Images/interface_explication.png)
Petite présentation des grands fonctionnalités de Galaxy 

> <details-title> Upload (red) </details-title>
>
> Permet l'importation des differents fichier 
>![Image site](./Images/upload.png)
> Pou télécharger un fichier depuis votre machine, il vous suffit de cliquer sur "Choose local file", de choisir votre dossier ensuite lorsque qu'il apparait sur le board vous pouvert lancer le téléchargelment en appyant sur "Start" et enfin lorsque que c'est vert, cliquer sur "Close" pour fermer
> Il y a d'autre méthode, plus specifique comme "paste/Fectch data" qui permet de télécharger via un URL, ou d'uatre pour des fichier specifique comme les shapefiles.
{: .details}


> <details-title> Tools (green) </details-title>
>
> La bibliothèque d’outils disponibles
>![Image site](./Images/Tools.png)
> Il existe de nombreux outils disponible sur Galaxy, 
> Il y a d'autre méthode, plus specifique comme "paste/Fectch data" qui permet de télécharger via un URL, ou d'uatre pour des fichier specifique comme les shapefiles.
> Vous pouvez chercher un outil via la barre de recherche, un outil a un formulaire a repondre, souvent assez clair a pprendre en mains, mais pour plus de faciliter, il y a souvent des tutorials, comme celui ci dans le basde la page de l'outil
> ![Image site](./Images/tools_jupyter.png)
{: .details}


> <details-title> Workflow (blue) </details-title>
>
> la section dédiée aux workflows
>![Image site](./Images/workflow.png)
> Cette section permet de creer 
> Cette dernière permet de naviguer entre ses propres workflows et ceux partagés par la communauté
{: .details}


> <details-title> History (pink) </details-title>
>
> À droite de l’écran, le panneau d’historique
>![Image site](./Images/workflow.png)
> permet de suivre l’ensemble des éléments liés à l’analyse en cours : données importées, outils exécutés, résultats générés. Il est possible de gérer plusieurs historiques en parallèle afin d’organiser distinctement les différentes étapes ou versions d’un même projet.Au centre de l’interface se trouve l’espace de visualisation du workflow. Chaque étape y apparaît de manière visuelle et modulaire. Un soin particulier a été apporté à la clarté de cette représentation. Les options proposées à l’utilisateur, comme l’activation ou non de la génération d’une figure ou la définition de zones géographiques à analyser, ont été pensées pour être facilement compréhensibles et configurables
{: .details}


---------------------------------------------------------------------------
> **In this tutorial, you will learn how to:**
>
> 1. Understand what Galaxy is and how it works
> 2. Access the workflow and run it
> 3. Choose inputs or upload your own data
> 4. Generate automated figures


# Introduction

This tutorial will guide you through using a specific Galaxy workflow:

{% icon galaxy-link %} **[State of the Environment – Antarctic](https://ecology.usegalaxy.eu/u/arthurb/w/worflow-for-representig-state-of-the-environment-in-antarctic)**

This workflow was created during a Master’s internship focused on automating the production of standard reports about the environmental and marine ecosystem status in Antarctica. The project is based on a proposal by the British delegation to the **CCAMLR** (Commission for the Conservation of Antarctic Marine Living Resources).

The idea is to **automate the creation of visual figures** used in annual or periodic reports, making it easier to update and reproduce them over time. The workflow is still under development, but this tutorial will show you how to use it step by step.

<!-- This is a comment. -->

> <agenda-title></agenda-title>
>
> In this tutorial, we will cover:
>
> 1. TOC
> {:toc}
>
{: .agenda}


## For First-Time Users: What is Galaxy?

**Galaxy** is an open-source software platform designed to simplify scientific data analysis. Originally developed for biology, it is now widely used in many other research fields.

Galaxy’s core mission is to **make complex data analysis accessible** and to promote **reproducible and transparent science**. To do this, Galaxy allows researchers to run advanced analyses through an {% icon galaxy-interface %} **easy-to-use web interface**, without needing any programming skills or complex installations.

### How does Galaxy work?

There are many Galaxy instances around the world — each one is a server hosting the platform. The two largest public instances are:

- **[UseGalaxy.org](https://usegalaxy.org)** — for North America  
- **[UseGalaxy.eu](https://usegalaxy.eu)** — for Europe

Other regional or project-specific instances exist too, such as {% icon galaxy-link %} [UseGalaxy.fr](https://usegalaxy.fr) for France.

When using Galaxy, you can:

- {% icon upload %} **Upload and store datasets** directly on the platform
- {% icon tool %} **Run analysis tools and workflows** on remote servers — your computer doesn’t handle heavy processing

Each step of your analysis is recorded in a personal {% icon history %} **history**, making your work:

- {% icon history-select %} **Reproducible** — Re-run the same workflow with the same settings anytime
- {% icon galaxy-history %} **Reusable** — Adapt existing workflows for new datasets or use cases
- {% icon cofest %} **Shareable** — Collaborate easily by sharing your workflows and histories with others

Galaxy is designed for scientists — whether or not they code. In this tutorial, you'll learn how to use Galaxy to generate automated figures for Antarctic environmental reports with just a few clicks.

> <hands-on-title>Create a Galaxy Account</hands-on-title>
>
> First, go to the Galaxy server we'll be using for this tutorial:  
> [ecology.usegalaxy.eu](https://ecology.usegalaxy.eu) {% icon galaxy-link %}
>
> If you don’t have an account yet, you’ll need to create one. Click on **"Login or Register"** in the top-right corner of the homepage.
>
> ![Login Button](./Images/interface.png)
>
> On the login page, you can register a new account by clicking the button at the bottom-left:
>
> ![Register Page](./Images/register.png)
>
> Once your account is created and you are logged in, you are ready to begin working with Galaxy!
{: .hands_on}

---

## A quick tour of the Galaxy interface

Here is a general overview of Galaxy’s main components. You’ll become familiar with these as we go through the tutorial:

![Interface overview](./Images/interface_explication.png)

---
{% tool [Upload] %}
> <details-title>{% icon upload %} Upload (Red Panel)</details-title>
>
> {% icon upload %} This is where you upload your datasets.
>
> ![Upload Panel](./Images/upload.png)
>
> - Click **“Choose local file”** to select files from your computer  
> - After the file appears in the panel, click **“Start”** to begin the upload  
> - Once the upload is complete (green bar), click **“Close”**
>
> You can also use other options like:
> - **“Paste/Fetch data”**: paste URLs to fetch data from the web  
> - Upload specific formats like **shapefiles** (e.g. `.shp`, `.dbf`, etc.)
{: .details}

---

> <details-title>Tools (Green Panel)</details-title>
>
> {% icon tool %} This is Galaxy’s library of all available tools.
>
> ![Tools Panel](./Images/Tools.png)
>
> - Use the **search bar** to find tools by name or keyword  
> - Each tool has a **form** with input parameters. These are usually well-documented  
> - At the bottom of each tool page, you may find links to tutorials like this one:
>
> ![Jupyter Tool Example](./Images/tools_jupyter.png)
>
> Tools range from data conversion, filtering, and plotting, to more complex statistical or geospatial analysis.
{: .details}

---

> <details-title>Workflow (Blue Panel)</details-title>
>
> {% icon workflow %} This is where you manage and run workflows.
>
> ![Workflow Panel](./Images/workflow.png)
>
> - Access your saved workflows  
> - Browse shared workflows from the Galaxy community  
> - Launch, edit, or create workflows using Galaxy’s visual editor
{: .details}

---

> <details-title>History (Pink Panel)</details-title>
>
> {% icon history %} This panel tracks everything you do in Galaxy.
>
> ![History Panel](./Images/workflow.png)
>
> - It shows uploaded files, tool runs, outputs, and parameters used  
> - Each step is logged, making your work reproducible  
> - You can manage multiple histories in parallel (e.g., for different projects)
>
> This panel also allows you to **name, annotate, and export** your analysis history.
{: .details}




















# Hands-on Sections
Below are a series of hand-on boxes, one for each tool in your workflow file.
Often you may wish to combine several boxes into one or make other adjustments such
as breaking the tutorial into sections, we encourage you to make such changes as you
see fit, this is just a starting point :

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