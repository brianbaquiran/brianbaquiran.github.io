.. title: Programming Exam
.. slug: programming-exam
.. date: 2015-11-25 13:06:34 UTC+08:00
.. tags: mathjax
.. category:
.. link:
.. description:
.. type: text

Equilibrium index of a sequence is an index such that the sum of elements at lower indexes is equal to the sum of elements at higher indexes. For example, in a sequence A:

.. code:: python

    A[0]=-7 A[1]=1 A[2]=5 A[3]=2 A[4]=-4 A[5]=3 A[6]=0

``3`` is an equilibrium index, because:

.. code:: python

    A[0]+A[1]+A[2]=A[4]+A[5]+A[6]

``6`` is also an equilibrium index, because:

.. code:: python

    A[0]+A[1]+A[2]+A[3]+A[4]+A[5]=0

(sum of zero elements is zero)

``7`` is not an equilibrium index, because it is not a valid index of sequence A.

If you still have doubts, this is a precise definition:
    the integer :math:`k` is an equilibrium index
    of a sequence :math:`A[0], A[1], ..., A[n-1]` if and only if
    :math:`0 \le k \lt n` and
    :math:`\sum_{i=0}^{k-1} A[i] = \sum_{i=k+1}^{n-1} A[i]`

Assume the sum of zero elements is equal zero. Write a function

.. code:: C++

    int equi(int[] A); /* C/C++ */

or

.. code:: php

    function equi($A); /* PHP, $A is an array */

or

.. code:: python

    def equi(A):
        # A is a list
        pass

that given a sequence, returns its equilibrium index (any) or -1 if no equilibrium indexes exist. Assume that the sequence may be very long.
