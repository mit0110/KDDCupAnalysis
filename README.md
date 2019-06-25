# Dataset exploration with Spark and Zeppelin

This work is a collection of Zeppelin notebooks used to explore and preprocess the KDDCup 2015 dataset for dropout prediction. The dataset includes records of student interactions with the Xuetang educational platform, containing 38 different Massive Open Online Courses (MOOCs). If students do not return to the platform 10 days after the course ends, they are labeled as *dropout*. The objective of the challenge is to predict this label based on the sequence of interactions.

It is part of a Machine Learning pipeline that will apply, in further steps, neural models to the classification task.

## Structure of the work

The first notebook, `01_cleaning.json`, contains the initial exploration and cleaning of data. We detail the general distribution of elements on all files, and replace hash ids for numerical ones using spark's `StringIndexer`. This task was not straight forward, as the `objects.csv` file encodes a tree structure of modules, where children nodes are stores as lists. The Indexer must be applied to these lists, and the original format must be restored before saving the clean file.

Once the data is clean, we visualize the distribution of enrollments and interactions per course, separating them according to their labels. We also perform outlier detection and removal.

The second notebook, `02_relations.json`, contains a deeper analysis on the type of interactions undertaken by students, focusing on the correlation with the student label. An interactive visualization of different courses using angular is included.

We perform an analysis by sessions, motivated by the hypothesis that aggregated data will reveal more interpretable patterns.

After the explorations, we reach the following conclusions:

* Students who dropout have less interactions. However, there is a significant number of examples of long sequences with dropout label.
* The type of interactions is related to the type of students. Dropout students have more `access` and `navigate` events, and students that don't dropout have more `problem` and `discussion` events.
* The number of sessions per student has a different distribution between the dropout and no dropout class.

On the third notebook `03_classifiers.json`, we train some very simple ML linear classifiers to check if the features selected effectively model the dropout phenomenon.

The first classifier evaluated was a Logistic Regression. Different models must be trained for different courses, as the distribution of features are not correlated. We obtained results per course and a general result concatenating the predictions of all courses, and analyze them through different visualizations.

The second classifier is a Random Forest, which obtains similar results. We add a visualization of the most important features used by the classifier, using the D3.js library.


## Decisions taken

* The output files were saved on csv format to ensure compatibility with other tools and further steps on the classification pipeline.
* Many visualizations are created with Seaborn for consistency of style and color with the rest of the work.

## Requirements

Zeppelin 8.1 with Spark and PySpark interpreters

To run the cells using PySpark, the seaborn library is necessary. It can be installed via:

    $ pip install seaborn

