Resources for Researchers
=========================

Systematic review
-----------------

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


Datasets
--------

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


Tensorflow
^^^^^^^^^^

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



