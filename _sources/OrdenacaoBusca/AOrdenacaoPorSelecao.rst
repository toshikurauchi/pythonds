..  Copyright (C)  Brad Miller, David Ranum
    This work is licensed under the Creative Commons Attribution-NonCommercial-ShareAlike 4.0 International License. To view a copy of this license, visit http://creativecommons.org/licenses/by-nc-sa/4.0/.


A Ordenação Por Seleção
~~~~~~~~~~~~~~~~~~~~~~~

A **ordenação por seleção** melhora o bubble sort ao realizar apenas uma troca
a cada passagem pela lista. Para conseguir isso, uma ordenação por seleção
procura pelo valor mais alto enquanto faz uma passagem e, depois de
completá-la, coloca-o na posição correta. Assim como o bubble sort, depois da
primeira passagem, o maior item sempre está na posição correta. Depois da
segunda passagem, o próximo maior item também vai para a posição adequada.
O processo continua dessa forma e demanda :math:`n-1` passagens para ordenar
*n* itens, já que o item final deverá estar no lugar certo depois da
:math:`(n-1)`-ésima passagem.

A :ref:`Figura 3 <fig_selectionsort>` mostra o processo de ordenação completo.
A cada passagem, o maior item remanescente é selecionado e colocado no lugar
apropriado. A primeira passagem posiciona o 93, a segunda, o 77, a terceira, o
55, e assim por diante. A função é mostrada no
:ref:`ActiveCode 1 <lst_selectionsortcode>`.

.. _fig_selectionsort:

.. figure:: Figures/selectionsortnew.png
   :align: center


   Figura 3: ``Ordenação Por Seleção``



.. activecode:: lst_selectionsortcode
    :caption: Ordenação por Seleção

    def selectionSort(alist):
       for fillslot in range(len(alist)-1,0,-1):
           positionOfMax=0
           for location in range(1,fillslot+1):
               if alist[location]>alist[positionOfMax]:
                   positionOfMax = location

           temp = alist[fillslot]
           alist[fillslot] = alist[positionOfMax]
           alist[positionOfMax] = temp

    alist = [54,26,93,17,77,31,44,55,20]
    selectionSort(alist)
    print(alist)

.. animation:: selection_anim
   :modelfile: sortmodels.js
   :viewerfile: sortviewers.js
   :model: SelectionSortModel
   :viewer: BarViewer


.. Para mais detalhes, o CodeLens 3 permite que você veja cada passo do algoritmo.
..
..
.. .. codelens:: selectionsortcodetrace
..     :caption: Rastreando a Ordenação por Seleção
..
..     def selectionSort(alist):
..        for fillslot in range(len(alist)-1,0,-1):
..            positionOfMax=0
..            for location in range(1,fillslot+1):
..                if alist[location]>alist[positionOfMax]:
..                    positionOfMax = location
..
..            temp = alist[fillslot]
..            alist[fillslot] = alist[positionOfMax]
..            alist[positionOfMax] = temp
..
..     alist = [54,26,93,17,77,31,44,55,20]
..     selectionSort(alist)
..     print(alist)

Você pode ver que a ordenação por seleção faz o mesmo número de comparações
que o bubble sort e também é :math:`O(n^{2})`. Contudo, devido à redução do
número de trocas, a ordenação por seleção tipicamente executa mais rápido
em análises de desempenho. Na verdade, para a nossa lista, o bubble sort
realiza 20 trocas, enquanto a ordenação por seleção faz apenas 8.



.. admonition:: Autoavaliação

   .. mchoice:: question_sort_2
      :correct: d
      :answer_a: [7, 11, 12, 1, 6, 14, 8, 18, 19, 20]
      :answer_b: [7, 11, 12, 14, 19, 1, 6, 18, 8, 20]
      :answer_c: [11, 7, 12, 14, 1, 6, 8, 18, 19, 20]
      :answer_d: [11, 7, 12, 14, 8, 1, 6, 18, 19, 20]
      :feedback_a: A ordenação por seleção é semelhante ao bubble sort (aparentemente, o que você fez), mas realiza menos trocas.
      :feedback_b: Isso parece com a ordenação por inserção.
      :feedback_c: Parece correto, mas em vez de terem sido feitas trocas, os números foram deslocados para a esquerda para a inclusão dos valores corretos.
      :feedback_d: A ordenação por seleção melhora o bubble sort ao realizar menos trocas.

      Suponha que você tenha a seguinte lista de números para ordenar:
      [11, 7, 12, 14, 19, 1, 6, 18, 8, 20] qual lista representa a lista parcialmente ordenada depois de três passagens completas da ordenação por seleção?
