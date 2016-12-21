..  Copyright (C)  Brad Miller, David Ranum
    This work is licensed under the Creative Commons Attribution-NonCommercial-ShareAlike 4.0 International License. To view a copy of this license, visit http://creativecommons.org/licenses/by-nc-sa/4.0/.


O Shell Sort
~~~~~~~~~~~~

O **shell sort**, às vezes chamaado de "ordenação por incrementos diminutos",
melhora a ordenação por inserção ao quebrar a lista original em um número
menor de sublistas, as quais são ordenadas usando a ordenação por inserção.
A forma única como essas sublistas são escolhidas é a chave para o shell sort.
Em vez de quebrar a lista em sublistas de itens contíguos, o shell sort
usa um incremento ``i``, às vezes chamado de **gap**, para criar uma sublista
escolhendo todos os itens que estão afastados ``i`` itens uns dos outros.

Isso pode ser visto na :ref:`Figura 6 <fig_incrementsA>`. Essa lista tem nove
itens. Se usarmos um incremento de três, serão três sublistas, cada uma
sendo submetida à ordenação por inserção. Depois de realizado esse processo,
ficamos com a lista mostrada em :ref:`Figura 7 <fig_incrementsB>`. Embora
essa lista não esteja completamente ordenada, algo muito interessante
aconteceu. Ao ordenarmos as sublistas, os itens ficaram mais próximos de
onde eles pertencem de fato.

.. _fig_incrementsA:


.. figure:: Figures/shellsortA.png
   :align: center

   Figura 6: Shell Sort com Incremento de Três


.. _fig_incrementsB:

.. figure:: Figures/shellsortB.png
   :align: center

   Figura 7: Shell Sort depois de Ordenar Cada Sublista


A :ref:`Figura 8 <fig_incrementsC>` mostra uma ordenação por inserção final
usando um incremento de um; em outras palavras, uma ordenação por inserção
convencional. Observe que ao realizar as ordenações de sublistas anteriores,
reduzimos agora o número total de operações de deslocalmento necessárias para
colocar a lista na sua ordem final. Nesse caso, precisamos de apenas mais
quatro deslocamentos para completar o processo.


.. _fig_incrementsC:

.. figure:: Figures/shellsortC.png
   :align: center

   Figura 8: ShellSort: Uma Inserção Final Com Incremento de 1


.. _fig_incrementsD:

.. figure:: Figures/shellsortD.png
   :align: center

   Figura 9: Sublistas Iniciais para um Shell Sort


Dissemos antes que a forma como os incrementos são escolhidos é uma
característica única do shell sort. A função mostrada em
:ref:`ActiveCode 1 <lst_shell>` usa um conjunto de diferentes incrementos. Nesse
caso, começamos com :math:`\frac {n}{2}` sublistas. No passo seguinte,
:math:`\frac {n}{4}` sublistas são ordenadas. Eventualmente, uma única lista
é ordenada com a ordenação por inserção básica. A :ref:`Figure 9 <fig_incrementsD>`
mostra as primeiras sublistas para o nosso exemplo usando esse incremento.

A seguinte chamada da função ``shellSort`` mostra as listas parcialmente
ordenadas depois de cada incremento, com a ordenação final sendo uma
ordenação por inserção com incremento de um.


.. _lst_shell:

.. activecode:: lst_shellSort
    :caption: Shell Sort

    def shellSort(alist):
        sublistcount = len(alist)//2
        while sublistcount > 0:

          for startposition in range(sublistcount):
            gapInsertionSort(alist,startposition,sublistcount)

          print("After increments of size",sublistcount,
                                       "The list is",alist)

          sublistcount = sublistcount // 2

    def gapInsertionSort(alist,start,gap):
        for i in range(start+gap,len(alist),gap):

            currentvalue = alist[i]
            position = i

            while position>=gap and alist[position-gap]>currentvalue:
                alist[position]=alist[position-gap]
                position = position-gap

            alist[position]=currentvalue

    alist = [54,26,93,17,77,31,44,55,20]
    shellSort(alist)
    print(alist)



.. animation:: shell_anim
   :modelfile: sortmodels.js
   :viewerfile: sortviewers.js
   :model: ShellSortModel
   :viewer: BarViewer



.. Para mais detalhes, o CodeLens 5 permite que você veja um passo por vez do algoritmo.
..
..
.. .. codelens:: shellSorttrace
..     :caption: Tracing the Shell Sort
..
..     def shellSort(alist):
..         sublistcount = len(alist)//2
..         while sublistcount > 0:
..
..           for startposition in range(sublistcount):
..             gapInsertionSort(alist,startposition,sublistcount)
..
..           print("After increments of size",sublistcount,
..                                        "The list is",alist)
..
..           sublistcount = sublistcount // 2
..
..     def gapInsertionSort(alist,start,gap):
..         for i in range(start+gap,len(alist),gap):
..
..             currentvalue = alist[i]
..             position = i
..
..             while position>=gap and alist[position-gap]>currentvalue:
..                 alist[position]=alist[position-gap]
..                 position = position-gap
..
..             alist[position]=currentvalue
..
..     alist = [54,26,93,17,77,31,44,55,20]
..     shellSort(alist)
..     print(alist)



À primeira vista, você pode pensar que o shell sort não tem como ser melhor
que a ordenação por inserção, já que ele a utiliza na lista toda como um
último passo. Acontece que essa ordenação por inserção final não precisa
realizar muitas comparações (ou deslocamentos), já que a lista foi pré-ordenada
por ordenações por inserção incrementais anteriores, como descrito acima.
Em outras palavras, cada passagem produz uma lista que está "mais ordenada"
que a anterior. Isso faz com que a passagem final seja muito eficiente.

Embora uma análise geral do shell sort esteja além do escopo deste texto,
podemos dizer que ele tende a ficar entre :math:`O(n)` e :math:`O(n^{2})`,
baseado no comportamento descrito acima. Para os incrementos mostrados em
:ref:`Listing 5 <lst_shell>`, o desempenho é :math:`O(n^{2})`. Alterando o
incremento, por exemplo, usando :math:`2^{k}-1` (1, 3, 7, 15, 31, e assim
por diante), o shell sort pode executar a :math:`O(n^{\frac {3}{2}})`.



.. admonition:: Autoavaliação

   .. mchoice:: question_sort_4
      :correct: a
      :answer_a: [5, 3, 8, 7, 16, 19, 9, 17, 20, 12]
      :answer_b: [3, 7, 5, 8, 9, 12, 19, 16, 20, 17]
      :answer_c: [3, 5, 7, 8, 9, 12, 16, 17, 19, 20]
      :answer_d: [5, 16, 20, 3, 8, 12, 9, 17, 20, 7]
      :feedback_a: Cada grupo de números representados pelas posições de índice de separação 3 são ordenados corretamente.
      :feedback_b: Essa solução é para um gap de tamanho dois.
      :feedback_c: Essa é uma lista completamente ordenada, você foi longe demais.
      :feedback_d: O tamanho de gap três indica que cada grupo representado por cada terceiro número (e.g., 0, 3, 6, 9; 1, 4, 7; e 2, 5, 8) está ordenado, e não grupos de 3.

      Dada a seguinte lista de números: [5, 16, 20, 12, 3, 8, 9, 17, 19, 7]
      Que resposta exibe o conteúdo correto da lista depois que todas as trocas são feitas para um gap de tamanho 3?
