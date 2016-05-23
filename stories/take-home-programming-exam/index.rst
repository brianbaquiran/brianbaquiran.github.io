.. title: Take Home Programming Exam
.. slug: take-home-programming-exam
.. date: 2015-11-25 14:06:08 UTC+08:00
.. tags: mathjax, programming, risingtide, work
.. category:
.. link:
.. description:
.. type: text

Instructions
------------
You are free to use any programming language for these problems, but you must use the same language for all three problems.
To submit your work, you will need an account on `GitLab`_. We assume you already know how to use git_.

.. _`GitLab`: https://gitlab.com
.. _git: http://www.git-scm.com/

You should follow the following steps before beginning work on the problems.

#. Create a new project repository in GitLab with the name **rtprogexam-<firstname>-<lastname>** (using your own first/last name, of course).
#. Set the visibility level to **Private (Project access must be granted explicitly for each user)**
#. Add Brian Baquiran (brian_risingtide) to the project members, with **Reporter** access permissions.

You can begin working on the problems locally.

- ``git clone`` the project to your local machine.
- Be sure to commit and push often.
- You are strongly encouraged to provide unit tests and documentation for your work.
- You are given 48 hours to complete all three problems. Most applicants finish within 12 hours.

Problem 1: Translate a block of text into an array structure
------------------------------------------------------------

Given a text file that looks like this:

.. code::

    XXOOXOXOXO
    XOOXOXOXOX
    OXX XOOXOX
    OXXXOOX XO
    XOOXOXOXXO
    XOX XXO OX
    XXOOXOXOXO
    XO OXOOXOO
    OXXXOO OXO
    OOOOOOOXOX

Each character represents a space in a rectangular grid. The space may be occupied by either an agent, or it may be an empty space. There are two types of agents: X and O.

**Write a function** ``loadgrid`` **that reads a text file and returns an object or data structure array representing the grid, populated with the contents of the text file.**

- The function should take a single parameter, the filename, and return a single object or data structure.
- You may implement the data structure using existing types, define a new class for the data structure, or use a library that makes handling 2-d arrays convenient, e.g. numpy arrays.
- The function should not make any assumptions regarding the dimensions of the input grid, but should determine them from the contents of the file.
- You may assume the text file exists and will be clean. No characters other than X, O and whitespace (space or CRLF) will be in the file.
- You can use the example data, available `here`_. Feel free to generate your own input files for testing.

.. _`here`: https://raw.githubusercontent.com/brianbaquiran/brianbaquiran.github.io/source/files/schelling10x10.txt

Problem 2: Compute the Index of Dissimilarity for a subregion
-------------------------------------------------------------

Now that you have your function and data structure from Task 1, write a function to compute the *Index of Dissimilarity* of a rectangular subregion within the array. Simply put, the Index of Dissimilarity is a way of measuring how segregated a set of spaces is. The index has a range of 0 to 1, where 0 indicates no segregation and 1 indicates total segregation.

For a region containing a total of N spaces, the general formula for the Index of Dissimilarity is

    :math:`\text{Index of Dissimilarity} = 0.5 \sum_{i=1}^N\big|\frac{x_i}{X} - \frac{o_i}{O}\big|`

    Where:

    - :math:`x_i` = number of X's in the subregion
    - :math:`X` = number of X's in the entire region
    - :math:`o_i` = number of O's in the subregion
    - :math:`O` = number of O's in the entire region

For our purposes, it is sufficient to consider :math:`x_i` and :math:`o_i` to be the number of X's and O's in the subregion we're interested in. The formula simplifies to:

    :math:`\text{Index of Dissimilarity} = 0.5 \big|\frac{x_i}{X} - \frac{o_i}{O}\big|`


For example, if we take the upper rightmost 4x4 region from the example grid above:

.. code::

    XOXO
    OXOX
    OXOX
    X XO

that contains 8 X's and 7 O's, we would compute the Index of Dissimilarity as

    Index of Dissimilarity = :math:`0.5 \big|\frac{8}{44} - \frac{7}{50}\big|` (there are 44 X's and 50 O's in the entire region)

    Index of Dissimilarity = :math:`0.02090909090909 \approx 0.021`

**Write a function** ``dissimilarity`` **that computes the index of dissimilarity for a subregion in a grid**

- The function should take five parameters: the grid data structure or object returned by ``loadgrid``, the row and column indexes of one corner of the subregion, the row and column indexes of the diagonally opposite corner of the subregion, e.g. ``dissimilarity(grid, row1, col1, row2, col2)``
- The function should return a numeric value, the index of dissimilarity

Problem 3: Simulate Schelling's Segregation Model
-------------------------------------------------
Read this summary of `Schelling's Model of Segregation <http://nifty.stanford.edu/2014/mccown-schelling-model-segregation/>`_. You will implement a *simplified* version of the model, where non-satisfied agents just leave the grid rather than relocate to an unoccupied space.

In Schelling's model, a **satisfied** agent is one that is surrounded by at least *t* percent of agents that are like itself. This threshold *t* is one that will apply to all agents in the model, even though in reality everyone might have a different threshold they are satisfied with. Note that the higher the threshold, the higher the likelihood the agents will not be satisfied with their current location.

For example, if t = 0.30, agent X is satisfied if at least 30% of its neighbors are also X. If fewer than 30% are X, then the agent is not satisfied, and it will want to leave its space in the grid.

All dissatisfied agents leave the grid at the same time, and leave behind an empty space.

**Write a function** ``schelling`` **that removes dissatisfied agents from the grid.**

- The function should take two parameters, the grid data structure or object returned by ``loadgrid``, and the threshold ``t``.
- The threshold ``t`` should be a numeric value between 0 and 1. Check for invalid values.
- The function should return a grid data structure, the input grid with dissatisfied agents replaced by an empty space.




