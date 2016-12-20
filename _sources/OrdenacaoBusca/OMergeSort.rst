..  Copyright (C)  Brad Miller, David Ranum
    This work is licensed under the Creative Commons Attribution-NonCommercial-ShareAlike 4.0 International License. To view a copy of this license, visit http://creativecommons.org/licenses/by-nc-sa/4.0/.


O Merge Sort
~~~~~~~~~~~~

Agora nós voltamos nossa atenção para usar a estratégia de "dividir para
conquistar" como uma forma de melhorar o desempenho dos algoritmos de ordenação.
O primeiro algoritmo que iremos estudar é o **merge sort**, um algoritmo
recursivo que divide uma lista continuamente pela metade. Se a lista estiver
vazia ou tiver um único item, ela está ordenada por definição (o caso base).
Se a lista tiver mais de um item, dividimos a lista e invocamos recursivamente
um merge sort em ambas as metades. Assim que as metades estiverem ordenadas,
a operação fundamental, chamada de **intercalação**, é realizada. Intercalar
é o processo de pegar duas listas menores ordenadas e combiná-las de modo
a formar uma lista nova, única e ordenada. A :ref:`Figura 10 <fig_mergesortA>`
mostra nossa lista familiar sendo dividida pelo ``mergeSort``. A
:ref:`Figura 11 <fig_mergesortB>` mostra como as listas mais simples,
agora ordenadas, são intercaladas.


.. _fig_mergesortA:

.. figure:: Figures/mergesortA.png
   :align: center

   Figura 10: Dividindo a Lista no Merge Sort


.. _fig_mergesortB:

.. figure:: Figures/mergesortB.png
   :align: center

   Figura 11: Intercalação das Listas



A função ``mergeSort`` mostrada em :ref:`ActiveCode 1 <lst_merge>` começa
perguntando pelo caso base. Se o tamanho da lista form menor ou igual a um,
então já temos uma lista ordenada e nenhum processamento adicional é necessário.
Se, por outro lado, o tamanho da lista for maior do que um, então usamos
a operação de ``slice`` do Python para extrair a metades esquerda e direita.
É importante observar que a lista pode não ter um número par de elementos.
Isso, contudo, não importa, já que a diferença de tamanho entre as listas
será de apenas um elemento.


.. _lst_merge:

.. activecode:: lst_mergeSort
    :caption: Merge Sort

    def mergeSort(alist):
        print("Splitting ",alist)
        if len(alist)>1:
            mid = len(alist)//2
            lefthalf = alist[:mid]
            righthalf = alist[mid:]

            mergeSort(lefthalf)
            mergeSort(righthalf)

            i=0
            j=0
            k=0
            while i < len(lefthalf) and j < len(righthalf):
                if lefthalf[i] < righthalf[j]:
                    alist[k]=lefthalf[i]
                    i=i+1
                else:
                    alist[k]=righthalf[j]
                    j=j+1
                k=k+1

            while i < len(lefthalf):
                alist[k]=lefthalf[i]
                i=i+1
                k=k+1

            while j < len(righthalf):
                alist[k]=righthalf[j]
                j=j+1
                k=k+1
        print("Merging ",alist)

    alist = [54,26,93,17,77,31,44,55,20]
    mergeSort(alist)
    print(alist)



Quando a função ``mergeSort`` é chamada nas metades esquerda e direita
(linhas 8-9), pressupõe-se que elas já estão ordenadas. O resto da função
(linhas 11-31) é responsável por intercalar as duas listas ordenadas
menores em uma lista ordenada maior. Note que a operação de intercalação
coloca um item por vez de volta na lista original (``alist``) ao tomar
repetidamente o menor item das listas ordenadas.

A função ``mergeSort`` foi aumentada com uma chamada de ``print`` (linha 2)
para mostrar o conteúdo da lista sendo ordenada no começo de cada chamada.
Também há uma declaração de ``print`` (linha 32) para mostrar o processo
de intercalação. O transcrito abaixo mostra o resultado de executar a
função na nossa lista de exemplo. Observe que a lista com 44, 55 e 20 não
irá ser dividida igualmente. A primeira metade resulta em [44] enquanto a
segunda em [55,20]. É fácil perceber que o procesos de divisão irá gerar
em algum momento uma lista que poderá ser imediatamente intercalada com
outras listas ordenadas.


.. animation:: merge_anim
   :modelfile: sortmodels.js
   :viewerfile: sortviewers.js
   :model: MergeSortModel
   :viewer: BarViewer


.. Para mais detalhes, o CodeLens 6 permite que você veja cada passo do algoritmo.
..
..
.. .. codelens:: mergetrace
..     :caption: Rastreando o Merge Sort
..
..     def mergeSort(alist):
..         print("Splitting ",alist)
..         if len(alist)>1:
..             mid = len(alist)//2
..             lefthalf = alist[:mid]
..             righthalf = alist[mid:]
..
..             mergeSort(lefthalf)
..             mergeSort(righthalf)
..
..             i=0
..             j=0
..             k=0
..             while i<len(lefthalf) and j<len(righthalf):
..                 if lefthalf[i]<righthalf[j]:
..                     alist[k]=lefthalf[i]
..                     i=i+1
..                 else:
..                     alist[k]=righthalf[j]
..                     j=j+1
..                 k=k+1
..
..             while i<len(lefthalf):
..                 alist[k]=lefthalf[i]
..                 i=i+1
..                 k=k+1
..
..             while j<len(righthalf):
..                 alist[k]=righthalf[j]
..                 j=j+1
..                 k=k+1
..         print("Merging ",alist)
..
..     alist = [54,26,93,17,77,31,44,55,20]
..     mergeSort(alist)
..     print(alist)


Para analisar a função ``mergeSort``, precisamos considerar os dois processos
distintos que compõem sua implementação. Primeiro, a lista é dividida em
duas metades. Nós já computamos (na busca binária) que podemos dividir uma
lista ao meio :math:`\log n` vezes, onde *n* é o tamanho da lista. O segundo
processo é a intercalação. Cada item na lista irá ser processado em algum
momento e colocado na lista ordenada. Então a operação de intercalação que
resulta em uma lista de tamanho *n* requer *n* operações. O resultado
desta análise é: :math:`\log n` divisões, cada qual custando :math:`n`,
totalizando :math:`n\log n` operações. Logo, o merge sort é um algoritmo
:math:`O(n\log n)`.

Lembre-se de que o operador "slice" é :math:`O(k)`, onde k é o tamanho
do corte. Para garantir que a função ``mergeSort`` seja :math:`O(n\log n)`,
precisamos remover o operador "slice". Isso é possível se simplesmente
passarmos os índices de início e fim junto com a lista quando fazemos a
chamada recursiva. Deixamos essa implementação como um exercício.

É importante notar que a função ``mergeSort`` requer espaço extra para
armazenar as duas metades conforme elas são extraídas no processo de divisão.
Esse espaço adicional pode ser um fator crítico se a lista for grande e
pode tornar a ordenação problemática em conjuntos grandes de dados.



.. admonition:: Autoavaliação

   .. mchoice:: question_sort_5
      :correct: b
      :answer_a: [16, 49, 39, 27, 43, 34, 46, 40]
      :answer_b: [21,1]
      :answer_c: [21, 1, 26, 45]
      :answer_d: [21]
      :feedback_a: Esta é a segunda metade da lista.
      :feedback_b: Sim, o merge sort irá continuar recursivamente em direção ao começo da lista até chegar ao caso base.
      :feedback_c: Lembre-de de que o merge sort não atua na metade direita da lista até que a esquerda esteja ordenada.
      :feedback_d: Esta é a lista depois de 4 chamadas recursivas.

      Dada a seguinte lista de números: <br> [21, 1, 26, 45, 29, 28, 2, 9, 16, 49, 39, 27, 43, 34, 46, 40] <br> qual resposta mostra a lista que deveria ser ordenada depois de 3 chamadas recursivas do merge sort?

   .. mchoice:: question_sort_6
      :correct: c
      :answer_a: [21, 1] e [26, 45]
      :answer_b: [[1, 2, 9, 21, 26, 28, 29, 45] e [16, 27, 34, 39, 40, 43, 46, 49]
      :answer_c: [21] e [1]
      :answer_d: [9] e [16]
      :feedback_a: As primeiras duas listas intercaladas irão formar as listas do caso base, mas não chegamos ainda ao caso base.
      :feedback_b: Estas serão as últimas duas listas intercaladas.
      :feedback_c: As listas [21] e [1] são os dois primeiros exemplos de caso base encontrados pelo merge sort e serão as duas primeiras listas a serem intercaladas.
      :feedback_d: Embora o 9 e o 16 estejam próximos um do outro, eles estão em diferentes metades da lista que sofre a primeira divisão.

      Dada a seguinte lista de números: <br> [21, 1, 26, 45, 29, 28, 2, 9, 16, 49, 39, 27, 43, 34, 46, 40] <br> qual resposta exibe as duas primeiras listas a serem intercaladas.
