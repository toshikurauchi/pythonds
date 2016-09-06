..  Copyright (C)  Brad Miller, David Ranum
    This work is licensed under the Creative Commons Attribution-NonCommercial-ShareAlike 4.0 International License. To view a copy of this license, visit http://creativecommons.org/licenses/by-nc-sa/4.0/.


A Busca Sequencial
~~~~~~~~~~~~~~~~~~

Quando itens de dados são armazenados numa coleção tal qual uma lista,
nós dizemos que eles têm uma relação linear ou sequencial. Cada item de dado é
armazenado numa posição relativa aos demais. Em listas do Python, essas
posições relativas são os índices dos itens individuais. Como esses
índices estão ordenados, é possível que nós visitemos os itens sequencialmente.
Esse processo dá origem à nossa primeira técnica de busca, a **busca sequencial**.

:ref:`Figura 1 <fig_seqsearch>` mostra como essa busca funciona. Começando
pelo primeiro item da lista, nós simplesmente andamos de item a item,
obedecendo à disposição sequencial até que encontremos o que nós estamos
procurando ou que não haja mais itens a serem buscados. Quando não há mais
itens, então chegamos à conclusão de que o item buscado não estava presente.


.. _fig_seqsearch:

.. figure:: Figures/seqsearch.png
   :align: center

   Figura 1: Busca sequencial de uma lista de inteiros


A implementação em Python para esse algoritmo é mostrada em
:ref:`CodeLens 1 <lst_seqsearchpython>`. A função precisa da lista e do item
que estamos procurando e retorna um valor booleano para indicar se está
presente ou não. A variável booleana ``found`` é inicializada com ``False`` e
muda de valor para ``True`` caso o item seja encontrado na lista.

.. _lst_seqsearchpython:

.. codelens:: search1
    :caption: Busca sequencial em uma lista não ordenada

    def sequentialSearch(alist, item):
        pos = 0
        found = False

        while pos < len(alist) and not found:
            if alist[pos] == item:
                found = True
            else:
                pos = pos+1

        return found

    testlist = [1, 2, 32, 8, 17, 19, 42, 13, 0]
    print(sequentialSearch(testlist, 3))
    print(sequentialSearch(testlist, 13))

Análise da Busca Sequencial
^^^^^^^^^^^^^^^^^^^^^^^^^^^

Para analisar algoritmos de busca, precisamos chegar a um consenso sobre
a unidade básica de computação. Lembre-se de que este é o típico passo
a ser dado repetidas vezes para que o problema seja solucionado. Na busca,
faz sentido contar o número realizado de comparações. Cada comparação pode ou
não encontrar o item que estamos procurando. Além disso, fazemos outra
suposição. A lista de itens não está ordenada de maneira alguma. Os itens
foram colocados aleatoriamente na lista. Em outras palavras, a probabilidade
de que o item que estamos procurando esteja numa posição em particular é
exatamente a mesma para cada posição da lista.

Se o elemento não está na lista, a única maneira de ter certeza é comparando-o
com cada outro item da lista. Se exsitem :math:`n` itens, então a busca
sequencial requer :math:`n` comparações para descobrir que tal elemento
não está lá. Caso o item esteja na lista, a análise já não é tão simples.
Existem, nesse caso, três diferentes cenários que podem ocorrer. No melhor
caso, iremos encontrar o item na primeira posição que procurarmos, isto é,
no começo da lista. Consequentemente, só precisaremos de uma comparação.
No pior caso, só conseguiremos encontrar o item na última posição, ou seja,
realizando a `n-ésima` comparação.

E quanto ao caso médio? Na média, nós iremos encontrar o item mais ou menos
na metade da lista, isto é, faremos :math:`\frac{n}{2}` comparações com outros
elementos. Lembre-se, contudo, de que conforme *n* se torna grande, os coeficientes,
independente de quais sejam, tornam-se insignificantes na nossa aproximação,
então a complexidade da busca sequencial é :math:`O(n)`. A
:ref:`Tabela 1 <tbl_seqsearchtable>` resume esses resultados.

.. _tbl_seqsearchtable:

.. table:: **Tabela 1: Comparações Realizadas numa Busca Sequencial de uma Lista Não Ordenada**

    ==================== ========================== ========================== ========================
    **Caso**                   **Melhor Caso**             **Pior Caso**            **Caso Médio**
    ==================== ========================== ========================== ========================
    item presente        :math:`1`                  :math:`n`                  :math:`\frac{n}{2}`
    item não presente    :math:`n`                  :math:`n`                  :math:`n`
    ==================== ========================== ========================== ========================





We assumed earlier that the items in our collection had been randomly
placed so that there is no relative order between the items. What would
happen to the sequential search if the items were ordered in some way?
Would we be able to gain any efficiency in our search technique?

Assume that the list of items was constructed so that the items were in
ascending order, from low to high. If the item we are looking for is
present in the list, the chance of it being in any one of the *n*
positions is still the same as before. We will still have the same
number of comparisons to find the item. However, if the item is not
present there is a slight advantage. :ref:`Figure 2 <fig_seqsearch2>` shows this
process as the algorithm looks for the item 50. Notice that items are
still compared in sequence until 54. At this point, however, we know
something extra. Not only is 54 not the item we are looking for, but no
other elements beyond 54 can work either since the list is sorted. In
this case, the algorithm does not have to continue looking through all
of the items to report that the item was not found. It can stop
immediately. :ref:`CodeLens 2 <lst_seqsearchpython2>` shows this variation of the
sequential search function.

.. _fig_seqsearch2:

.. figure:: Figures/seqsearch2.png
   :align: center

   Figure 2: Sequential Search of an Ordered List of Integers



.. _lst_seqsearchpython2:

.. codelens:: search2
    :caption: Sequential Search of an Ordered List

    def orderedSequentialSearch(alist, item):
        pos = 0
        found = False
        stop = False
        while pos < len(alist) and not found and not stop:
            if alist[pos] == item:
                found = True
            else:
                if alist[pos] > item:
                    stop = True
                else:
                    pos = pos+1

        return found

    testlist = [0, 1, 2, 8, 13, 17, 19, 32, 42,]
    print(orderedSequentialSearch(testlist, 3))
    print(orderedSequentialSearch(testlist, 13))

:ref:`Table 2 <tbl_seqsearchtable2>` summarizes these results. Note that in the best
case we might discover that the item is not in the list by looking at
only one item. On average, we will know after looking through only
:math:`\frac {n}{2}` items. However, this technique is still
:math:`O(n)`. In summary, a sequential search is improved by ordering
the list only in the case where we do not find the item.

.. _tbl_seqsearchtable2:

.. table:: **Table 2: Comparisons Used in Sequential Search of an Ordered List**


     ================ ============== ==============  ===================
                      **Best Case**  **Worst Case**  **Average Case**
     ================ ============== ==============  ===================
     item is present  :math:`1`        :math:`n`     :math:`\frac{n}{2}`
     item not present :math:`1`        :math:`n`     :math:`\frac{n}{2}`
     ================ ============== ==============  ===================


.. admonition:: Self Check

   .. mchoice:: question_SRCH_1
      :correct: d
      :answer_a: 5
      :answer_b: 10
      :answer_c: 4
      :answer_d: 2
      :feedback_a: Five comparisons would get the second 18 in the list.
      :feedback_b: You do not need to search the entire list, only until you find the key you are looking for.
      :feedback_c: No, remember in a sequential search you start at the beginning and check each key until you find what you are looking for or exhaust the list.
      :feedback_d: In this case only 2 comparisons were needed to find the key.

      Suppose you are doing a sequential search of the list [15, 18, 2, 19, 18, 0, 8, 14, 19, 14].  How many comparisons would you need to do in order to find the key 18?

   .. mchoice:: question_SRCH_2
      :correct: c
      :answer_a: 10
      :answer_b: 5
      :answer_c: 7
      :answer_d: 6
      :feedback_a:  You do not need to search the entire list, since it is ordered you can stop searching when you have compared with a value larger than the key.
      :feedback_b: Since 11 is less than the key value 13 you need to keep searching.
      :feedback_c: Since 14 is greater than the key value 13 you can stop.
      :feedback_d: Because 12 is less than the key value 13 you need to keep going.

      Suppose you are doing a sequential search of the ordered list [3, 5, 6, 8, 11, 12, 14, 15, 17, 18].  How many comparisons would you need to do in order to find the key 13?
