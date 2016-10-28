..  Copyright (C)  Brad Miller, David Ranum
    This work is licensed under the Creative Commons Attribution-NonCommercial-ShareAlike 4.0 International License. To view a copy of this license, visit http://creativecommons.org/licenses/by-nc-sa/4.0/.


A Ordenação Por Inserção
~~~~~~~~~~~~~~~~~~~~~~~~

A **ordenação por inserção**, embora seja :math:`O(n^{2})`, funciona de uma forma
ligeiramente diferente. Ela sempre mantém uma sublista ordenada nas posições
inferiores da lista. Cada novo item é "inserido" na sublista anterior de modo
que a sublista ordenada fique com um item a mais. A :ref:`Figura 4 <fig_insertionsort>`
mostra o processo de ordenação por inserção. Os itens sombreados representam
as sublistas ordenadas conforme cada passagem feita pelo algoritmo.


.. _fig_insertionsort:

.. figure:: Figures/insertionsort.png
   :align: center

   Figure 4: ``insertionSort``

Começamos pressupondo que uma lista com um único item (posição :math:`0`) está
ordenada. A cada passagem, do item 1 a :math:`n-1`, o item atual é comparado
com os que já estão na sublista ordenada. Conforme vamos caminhando de trás
pra frente na sublista já ordenada, movemos os itens que são maiores para a
direita. Quando encontramos um item menor ou chegamos ao fim da sublista, o
item atual é inserido.

A :ref:`Figura 5 <fig_insertionpass>` mostra a quinta passagem em detalhe.
Nesse ponto do algoritmo, há uma sublista com cinco itens: 17, 26, 54, 77 e 93.
Queremos inserir 31 nesse conjunto já ordenado. A primeira comparação faz com
que o item 93 vá para a direita. Os itens 77 e 54 também são deslocados para
frente. Quando o item 26 é encontrado, o processo de deslocamento para e o 31
é colocado na lacuna aberta. Temos agora uma sublista ordenada com seis itens.


.. _fig_insertionpass:

.. figure:: Figures/insertionpass.png
   :align: center

   Figura 5: ``Ordenação por Inserção``: Quinta Passagem do Algoritmo


A implementação da ``ordenação por inserção`` (:ref:`ActiveCode 1 <lst_insertion>`)
mostra mais uma vez que há :math:`n-1` passagens para ordenar *n* itens. A
iteração começa na posição 1 e vai até a posição : math:`n-1`, uma vez que esses
são os itens que precisam ser inseridos nas sublistas ordenadas. A linha 8
realiza a operação de deslocamento que move um valor uma posição à direita na
lista, abrindo espaço para a inserção. Note que essa não é uma troca completa
da mesma maneira que foi mostrada em algoritmos anteriores.

O número máximo de comparações para uma ordenação por inserção é a soma dos
primeiros :math:`n-1` inteiros. De novo, isso é :math:`O(n^{2})`. Contudo,
no melhor caso, apenas uma comparação é necessária a cada passagem. Esse
seria o caso de uma lista já ordenada.

Uma observação importante sobre a diferença entre deslocamento e troca: em
geral, a operação de deslocamento requer aproximadamente um terço de
processamento de uma troca, já que apenas uma atribuição é feita. Em estudos
de desempenho, a ordenação por inserção costuma mostrar ótimos resultados.


.. activecode:: lst_insertion
    :caption: Ordenação por Inserção

    def insertionSort(alist):
       for index in range(1,len(alist)):

         currentvalue = alist[index]
         position = index

         while position>0 and alist[position-1]>currentvalue:
             alist[position]=alist[position-1]
             position = position-1

         alist[position]=currentvalue

    alist = [54,26,93,17,77,31,44,55,20]
    insertionSort(alist)
    print(alist)

.. animation:: insertion_anim
   :modelfile: sortmodels.js
   :viewerfile: sortviewers.js
   :model: InsertionSortModel
   :viewer: BarViewer


.. Para mais detalhes, o CodeLens 4 permite que você realize um passo por vez do algoritmo.
..
.. .. codelens:: insertionsortcodetrace
..     :caption: Tracing the Insertion Sort
..
..     def insertionSort(alist):
..        for index in range(1,len(alist)):
..
..          currentvalue = alist[index]
..          position = index
..
..          while position>0 and alist[position-1]>currentvalue:
..              alist[position]=alist[position-1]
..              position = position-1
..
..          alist[position]=currentvalue
..
..     alist = [54,26,93,17,77,31,44,55,20]
..     insertionSort(alist)
..     print(alist)

.. admonition:: Autoavaliação

   .. mchoice:: question_sort_3
      :correct: c
      :answer_a: [4, 5, 12, 15, 14, 10, 8, 18, 19, 20]
      :answer_b: [15, 5, 4, 10, 12, 8, 14, 18, 19, 20]
      :answer_c: [4, 5, 15, 18, 12, 19, 14, 10, 8, 20]
      :answer_d: [15, 5, 4, 18, 12, 19, 14, 8, 10, 20]
      :feedback_a: Este é o bubble sort.
      :feedback_b: Este é o resultado da ordenação por seleção.
      :feedback_c: A ordenação por inserção começa pelo início da lista. Cada passagem produz uma lista ordenada maior.
      :feedback_d: A ordenação por inserção não trabalha a partir do final da lista.

       Suponha que você tenha a seguinte lista de números para ordenar: <br>
       [15, 5, 4, 18, 12, 19, 14, 10, 8, 20] qual lista representa a lista parcialmente ordenada depois três passagens completas da ordenação por inserção?
