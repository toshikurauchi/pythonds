..  Copyright (C)  Brad Miller, David Ranum
    This work is licensed under the Creative Commons Attribution-NonCommercial-ShareAlike 4.0 International License. To view a copy of this license, visit http://creativecommons.org/licenses/by-nc-sa/4.0/.


A Busca Binária
~~~~~~~~~~~~~~~

É possível obter uma grande vantagem do fato de uma lista estar ordenada se
formos espertos o bastante com as nossas comparações. Na busca sequencial,
quando iniciamos a comparação a partir do primeiro item, há no máximo mais
:math:`n-1` elementos a serem buscados se o primeiro item não era o que
estávamos procurando. Em vez de procurar o item sequencialmente, uma
**busca binária** irá começar examinando o item do meio. Se esse elemento
é o que estamos buscando, a procura terminou. Se não for o item correto,
podemos utilizar o fato da lista estar ordenada para eliminar metade dela.
Se o item que estamos procurando for maior que o elemento do meio, sabemos
que a metade inferior (contando com o item do meio) não precisa mais ser
levada em consideração. O item, se estiver na lista, necessariamente está
na metade superior.

Podemos então repetir o processo com a parte superior. Começamos pelo item
do meio e o comparamos com o que estamos buscando. Novamente, ou o item do
meio é igual ao que buscamos, ou divimos a lista no meio, eliminando então
outra parte considerável do nosso espaço de busca. A
:ref:`Figura 3 <fig_binsearch>` mostra como esse algoritmo consegue encontrar
rapidamente o valor 54. A função completa é exibida em
:ref:`CodeLens 3 <lst_binarysearchpy>`.


.. _fig_binsearch:

.. figure:: Figures/binsearch.png
   :align: center

   Figura 3: Busca Binária em Uma Lista Ordenada de Inteiros


.. _lst_binarysearchpy:

.. codelens:: search3
    :caption: Busca Binária em uma Lista Ordenada

    def binarySearch(alist, item):
        first = 0
        last = len(alist)-1
        found = False

        while first<=last and not found:
            midpoint = (first + last)//2
            if alist[midpoint] == item:
                found = True
            else:
                if item < alist[midpoint]:
                    last = midpoint-1
                else:
                    first = midpoint+1

        return found

    testlist = [0, 1, 2, 8, 13, 17, 19, 32, 42,]
    print(binarySearch(testlist, 3))
    print(binarySearch(testlist, 13))

Before we move on to the analysis, we should note that this algorithm is
a great example of a divide and conquer strategy. Divide and conquer
means that we divide the problem into smaller pieces, solve the smaller
pieces in some way, and then reassemble the whole problem to get the
result. When we perform a binary search of a list, we first check the
middle item. If the item we are searching for is less than the middle
item, we can simply perform a binary search of the left half of the
original list. Likewise, if the item is greater, we can perform a binary
search of the right half. Either way, this is a recursive call to the
binary search function passing a smaller list. :ref:`CodeLens 4 <lst_recbinarysearch>`
shows this recursive version.

.. _lst_recbinarysearch:

.. codelens:: search4
    :caption: A Binary Search--Recursive Version

    def binarySearch(alist, item):
        if len(alist) == 0:
            return False
        else:
            midpoint = len(alist)//2
            if alist[midpoint]==item:
              return True
            else:
              if item<alist[midpoint]:
                return binarySearch(alist[:midpoint],item)
              else:
                return binarySearch(alist[midpoint+1:],item)

    testlist = [0, 1, 2, 8, 13, 17, 19, 32, 42,]
    print(binarySearch(testlist, 3))
    print(binarySearch(testlist, 13))



Analysis of Binary Search
^^^^^^^^^^^^^^^^^^^^^^^^^

To analyze the binary search algorithm, we need to recall that each
comparison eliminates about half of the remaining items from
consideration. What is the maximum number of comparisons this algorithm
will require to check the entire list? If we start with *n* items, about
:math:`\frac{n}{2}` items will be left after the first comparison.
After the second comparison, there will be about :math:`\frac{n}{4}`.
Then :math:`\frac{n}{8}`, :math:`\frac{n}{16}`, and so on. How many
times can we split the list? :ref:`Table 3 <tbl_binaryanalysis>` helps us to see the
answer.

.. _tbl_binaryanalysis:

.. table:: **Table 3: Tabular Analysis for a Binary Search**

    ======================== ======================================
             **Comparisons**   **Approximate Number of Items Left**
    ======================== ======================================
                           1                   :math:`\frac {n}{2}`
                           2                   :math:`\frac {n}{4}`
                           3                   :math:`\frac {n}{8}`
                         ...
                           i                 :math:`\frac {n}{2^i}`
    ======================== ======================================


When we split the list enough times, we end up with a list that has just
one item. Either that is the item we are looking for or it is not.
Either way, we are done. The number of comparisons necessary to get to
this point is *i* where :math:`\frac {n}{2^i} =1`. Solving for *i*
gives us :math:`i=\log n`. The maximum number of comparisons is
logarithmic with respect to the number of items in the list. Therefore,
the binary search is :math:`O(\log n)`.

One additional analysis issue needs to be addressed. In the recursive
solution shown above, the recursive call,

``binarySearch(alist[:midpoint],item)``

uses the slice operator to create the left half of the list that is then
passed to the next invocation (similarly for the right half as well).
The analysis that we did above assumed that the slice operator takes
constant time. However, we know that the slice operator in Python is
actually O(k). This means that the binary search using slice will not
perform in strict logarithmic time. Luckily this can be remedied by
passing the list along with the starting and ending indices. The indices
can be calculated as we did in :ref:`Listing 3 <lst_binarysearchpy>`. We leave this
implementation as an exercise.

Even though a binary search is generally better than a sequential
search, it is important to note that for small values of *n*, the
additional cost of sorting is probably not worth it. In fact, we should
always consider whether it is cost effective to take on the extra work
of sorting to gain searching benefits. If we can sort once and then
search many times, the cost of the sort is not so significant. However,
for large lists, sorting even once can be so expensive that simply
performing a sequential search from the start may be the best choice.

.. admonition:: Self Check

   .. mchoice:: BSRCH_1
      :correct: b
      :answer_a: 11, 5, 6, 8
      :answer_b: 12, 6, 11, 8
      :answer_c: 3, 5, 6, 8
      :answer_d: 18, 12, 6, 8
      :feedback_a:  Looks like you might be guilty of an off-by-one error.  Remember the first position is index 0.
      :feedback_b:  Binary search starts at the midpoint and halves the list each time.
      :feedback_c: Binary search does not start at the beginning and search sequentially, its starts in the middle and halves the list after each compare.
      :feedback_d: It appears that you are starting from the end and halving the list each time.

      Suppose you have the following sorted list [3, 5, 6, 8, 11, 12, 14, 15, 17, 18] and are using the recursive binary search algorithm.  Which group of numbers correctly shows the sequence of comparisons used to find the key 8.

   .. mchoice:: BSRCH_2
      :correct: d
      :answer_a: 11, 14, 17
      :answer_b: 18, 17, 15
      :answer_c: 14, 17, 15
      :answer_d: 12, 17, 15
      :feedback_a:  Looks like you might be guilty of an off-by-one error.  Remember the first position is index 0.
      :feedback_b:  Remember binary search starts in the middle and halves the list.
      :feedback_c:  Looks like you might be off by one, be careful that you are calculating the midpont using integer arithmetic.
      :feedback_d: Binary search starts at the midpoint and halves the list each time. It is done when the list is empty.

      Suppose you have the following sorted list [3, 5, 6, 8, 11, 12, 14, 15, 17, 18] and are using the recursive binary search algorithm.  Which group of numbers correctly shows the sequence of comparisons used to search for the key 16?
