
Question Recommender (qrecommender)
===================================

You are a visiting Professor in the first and only University of Mars, which admits exclusively
Martians (off-planet tuition would be too expensive for Earthlings anyway). You have joined the
Department of Astronomy and are in charge of teaching Astrometrics. Unfortunately the
university is quite overcrowded and each semester there are close to 13,000 martian students
that take Astrometrics.
Given the size of your class, and a disturbing lack of teaching assistants, you are forced to limit
the number of questions given to each student on the mid-term exam to only five multiple choice
questions. To further complicate matters the department head has mandated that all test
questions be pulled from a bank of approximately 400 “approved” test questions. To increase
the robustness and ease of creation of your midterm exam you decide to randomly and
uniformly choose 5 questions from the question bank for each student. This means that each
student’s exam consists of disjoint, intersecting, or identical sets of 5 questions.
To assist you with running such a large class, the university has provided you with last year’s
student performance data (where the professor created exams using a process similar to your
own). Additionally, the department head has admitted to you that she thinks that not all the
questions in the “approved” question bank are actually useful for evaluating each student’s
relative understanding of Astrometrics. She also warns you that while it is OK not to use all the
questions from the bank, you must, at minimum, use 50% of them in order to achieve sufficient
coverage of the curriculum.
With the mid-term exam only a few days away, which questions should you exclude from the
exam generation algorithm such that the exam results will provide the most meaningful ranking
of the students in the class? Why? Please explain both your reasoning and methodology; also,
please include any code you used to generate your results. Finally, please include an estimate
of the time you spent solving this problem. Note: when solving this problem you may not use
Matlab or Octave and we discourage the use of R.
The data set you are given by the department has one record per line, where each record
consists of:

    1. A unique identifier for a question
    2. A unique identifier for a student
    3. The correctness of a student's response. A correct answer is marked as a 1; an incorrect
       response is marked as a 0


Data
====
astudentData.csv


Solution
========
This problem can be formulated by a 'rating matrix' where rows correspond to students, 
columns correspond to questions, and each cell represents how a student responds to 
a given question. Similar to the missing-value scenarios in movie rating predictions, 
each student is unlikely to have answered all quesitions but only a small subset of them.  

Given a historical record on student's response to a set of questions, we will first 
infer from the data (see astduentData.csv) how students would respond to questions they 
have never encountered before; this will lead us to a full rating matrix (or more precisely, 
a question response matrix).

This demo uses two regression-based collaborative filtering (CF) algorithms to infer this 
question response matrix, which allows us to subsequently establish the notion of 
"question similarity" based on the response each question received from the students. The 
resulting simialrity matrix is then served as the basis for the (question-)cluster analysis 
and for deriving the optimal question assignment policy using genetic algorithm.

For further details, please refer to [this documentation](https://drive.google.com/file/d/1Tv7_fl-UpqEFgxXP0nA7y1g9Q3W7uQAt/view?usp=sharing).


Implementation
==============

I. System Requirements

    (a) The code was implemented in Python 2.7 on Mac OS X 10.7 platform.
    (b) Required packages: numpy, scipy, matplotlib, PIL (for visualizing similar questions and
users).

II. Related Modules

    1. cofi.py is the main entry of the code. Most of the modules include tests and a "main"
       function that demonstrate main features and their usage. 

    2. The file set {cluster1.py, cluster2.py, cluster_util.py, similarity.py, multidim_scaling.py} 
       deals with clustering algorithms and data visualization (for the purpose of identifying 
       similar questions). 

    3. The file set {evoluation.py, cofi.py} contain the code for running genetic algorithm 
       (fitness function is defined in cofi.py whereas the main genetic optimization algorithm 
       is implemented in evolution.py. 

Note that most of the key subroutines are organized in cofi.py for the purpose of this demo. 
The file set {`cofi.py`, `cofi_ref.py`} implements both types of collaborative filtering algorithms i.e.
i) weighted averaging with similarity matrix and ii) a latent-factored based model that can be solved via 
matrix factorization or via regression. Lastly, `minimizer.py` implements Polack-Ribiere flavour of 
conjugate gradients. The rest of the files mostly contain helper functions.



