Resources for Researchers
=========================

Systematic literature review
----------------------------

`Parsifal <https://parsif.al/>`_ is an online tool designed to support researchers to perform systematic literature reviews within the context of Software Engineering. Geographically distributed researchers can work together within a shared workspace, designing the protocol and conducting the research.

As well as providing a way to document the whole process, the tool will help you remind what is important during a systematic literature review. During the planning phase, `Parsifal <https://parsif.al/>`_  will help you with the objectives, PICOC, research questions, search string, keywords and synonyms, selecting the sources, the inclusion and exclusion criterias. Will also provide mechanisms to build a quality assessment checklist and data extraction forms.

During the conducting phase, you will be able to import bibtex files and select the studies, find duplicates among all the different sources, execute the quality assessment and extract data from the papers.

Advanced Information Research Skills (AIRS)
-------------------------------------------

`Advanced Information Research Skills (AIRS) <https://airs.library.qut.edu.au/>`_ is a coursework for Higher Degree Research (HDR) students enrolled in a Doctor of Philosophy (PhD) or Master of Philosophy (MPhil) at Queensland University of Technology (QUT), Australia. The curriculum content is openly accessible, creative commons licensed and available for use by all.

The curriculum includes:

* formulating a good research question
* advanced search strategies
* sourcing and evaluating quality literature
* bibliographic and data management
* note-taking strategies
* citation analysis and research impact
* collaboration tools
* authorship and academic integrity
* publishing and pathways.

.. youtube:: Z1mBTHw3xVo

Researcher Academy
------------------

*Learn*
`Researcher Academy <https://researcheracademy.elsevier.com/>`_ provides free access to countless e-learning resources designed to support researchers on every step of their research journey. Browse our extensive module catalogue to uncover a world of knowledge, and earn certificates and rewards as you progress.

RESEARCH PREPARATION

* Funding
* Research data management
* Research collaborations

WRITING FOR RESEARCH

* Fundamentals of manuscript preparation
* Writing skills
* Technical writing skills
* Book writing

PUBLICATION PROCESS

* Fundamentals of publishing
* Finding the right journal
* Ethics
* Open science
* How to publish in premium journals
* Publishing in the Chemical Sciences

NAVIGATING PEER REVIEW

* Certified Peer Reviewer Course
* Fundamentals of peer review
* Becoming a peer reviewer
* Going through peer review

COMMUNICATING YOUR RESEARCH

* Social impact
* Ensuring visibility
* Inclusion and Diversity for Researchers


*Career path*
A career in research can take many twists and turns. Whether you decide to stay in academia or move onto industry, `Researcher Academy’s <https://researcheracademy.elsevier.com/>`_ career resources will help you plan accordingly. Browse the different career sections for sound practical advice to tackle your future.

Career planning
^^^^^^^^^^^^^^^
Being an effective researcher requires method and order, and those strong organization skills can prove just as useful when it comes to mapping out your career.

In these career planning modules, we learn about the importance of investigating and considering all options before deciding on your next step. We discover why knowing and understanding yourself can be the key to making good choices. We explore whether a PhD is valued outside academia, and we hear why changing career, or even your field of study, can bring big benefits and opportunities for fresh and novel thinking.

Job search
^^^^^^^^^^
You’ve decided to explore career options in industry, but are there any special points to consider when job hunting? The answer is yes, and in these modules, we highlight what they are and how you can prepare for them.

You’ll find out how to take the experiences you’ve gained in the lab and translate them into practical skills any employer can relate to. You’ll also learn how to write a CV that will appeal to a company boss. Finally, we run you through a typical interview process and some of the questions you are likely to be asked.

Career guidance
^^^^^^^^^^^^^^^
As a researcher, your career path will be littered with crossroads and side tracks. Faced with all those choices, how do you decide on the right route for you? In our career guidance resources, you will find some helpful words of wisdom from seasoned colleagues.

We examine the ingredients required to make the ideal supervisor who can support and advise you on your journey. We explore how you can maintain a healthy balance between work and family life. We discover why it’s so important to be bold and nurture fresh thinking. And, we run through some of the differences you will encounter if you move from academia to industry.


Reference managers
------------------

Reference managers help collect, organize and share references and create citations in various formats. `Mendeley <https://www.mendeley.com>`_ and `Zotero <https://www.zotero.org/>`_ are free reference managers.

Mendeley
^^^^^^^^
`Mendeley <https://www.mendeley.com>`_ simplifies your workflow, so you can focus on achieving your goals.

Zotero
^^^^^^

`Zotero <https://www.zotero.org/>`_ is a free, easy-to-use tool to help you collect, organize, cite, and share research.

Datasets
--------

Mendeley
^^^^^^^^

`Mendeley Data <https://data.mendeley.com/>`_ is a secure cloud-based repository where you can store your data, ensuring it is easy to share, access and cite, wherever you are.

Search 28.1 million datasets from domain-specific and cross-domain repositories.


OpenML
^^^^^^

The `Open Machine Learning <https://openml.org/>`_ is a public repository for machine learning data and experiments, that allows everybody to upload open datasets. It integrates with scikit-learn.

.. youtube:: 1N3qATxXrpE

Example::

	from sklearn import ensemble
	from openml import tasks, flows, Runs

	task = tasks.get_task(3954)
	clf = ensemble.RandomForestClassifier()
	flow = flows.sklearn_to_flow(clf)
	run = runs.run_flow_on_task(task, flow)
	result = run.publish()

Key features:

* Query and download OpenML datasets and use them however you like
* Build any sklearn estimator or pipeline and convert to OpenML flows
* Run any flow on any task and save the experiment as run objects
* Upload your runs for collaboration or publishing
* Query, download and reuse all shared runs


Tensorflow Datasets
^^^^^^^^^^^^^^^^^^^

Tensorflow Datasets (TFDS) provides a collection of ready-to-use datasets for use with TensorFlow, Jax, and other Machine Learning frameworks.


https://www.tensorflow.org/datasets/catalog/overview

.. youtube:: -nTe44WT0ZI

Google Research
^^^^^^^^^^^^^^^

Google periodically releases data of interest to researchers in a wide range of computer science disciplines.

https://research.google/tools/datasets/

Google dataset search
^^^^^^^^^^^^^^^^^^^^^

Google provides a search engine for datasets. Discover datasets hosted in thousands repositories.

https://datasetsearch.research.google.com/



PyTorch
^^^^^^^^

Torch Audio: https://pytorch.org/audio/stable/datasets.html

Torchvision: https://pytorch.org/vision/stable/datasets.html

Torch text: https://pytorch.org/text/stable/datasets.html



