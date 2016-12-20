..  Copyright (C)  Brad Miller, David Ranum
    This work is licensed under the Creative Commons Attribution-NonCommercial-ShareAlike 4.0 International License. To view a copy of this license, visit http://creativecommons.org/licenses/by-nc-sa/4.0/.


O Quick Sort
~~~~~~~~~~~~

O **quick sort** utiliza a estratégia de dividir para conquistar para obter
as mesmas vantagens do merge sort, mas sem usar espaço adicional. Em
compensação, é possível que a lista possa não ser dividida ao meio. Quando isso
ocorre, veremos que seu desempenho é reduzido.

O quick sort primeiro seleciona um valor, chamado de **pivô**. Embora existam
muitas maneiras de selecionar o pivô, iremos utilizar simplesmente o primeiro
item na lista. O papel do pivô é ajudar na divisão da lista. A posição à qual
o pivô pertence de fato na lista ordenada, conhecida como **ponto de divisão**,
será usada para quebrar a lista em chamadas subsequentes do quick sort.

A :ref:`Figura 12 <fig_splitvalue>` mostra que o 54 funcionará como nosso
primeiro pivô. Como já olhamos para esse exemplo algumas vezes, sabemos que o
54 acabará uma hora na posição em que está o 31. O processo de **partição**
ocorrerá em seguida. Ele irá encontrar o ponto de divisão e, ao mesmo tempo,
moverá os itens para o lado apropriado da lista, isto é, maior ou menor que
o valor do pivô.


.. _fig_splitvalue:


.. figure:: Figures/firstsplit.png
   :align: center

   Figura 12: O Primeiro Pivô do Quick Sort




O particionamento começa localizando dois marcadores de posição — vamos
chamá-los de ``leftmark`` e ``rightmark`` — no começo e no fim dos demais
itens da lista (posições 1 e 8 em :ref:`Figura 13 <fig_partitionA>`).
O objetivo do processo de particionamento é mover os itens que estão do lado
errado do pivô para o lado certo, convergindo para o ponto de divisão.
A :ref:`Figura 13 <fig_partitionA>` mostra esse processo conforme nós
localizamos a posição do número 54.


.. _fig_partitionA:

.. figure:: Figures/partitionA.png
   :align: center

   Figura 13: Encontrando o ponto de divisão para o 54

Começamos incrementando o ``leftmark`` até encontrarmos um valor que seja
maior que o valor do pivô. Nós então decrementamos o ``rightmark`` até que
encontremos um valor que seja menor que o valor do pivô. Nesse momento,
descobrimos dois itens que estão fora de ordem em relação ao eventual ponto
de divisão. Para o nosso exemplo, isso ocorre com o 93 e o 20. Agora podemos
trocar esses dois itens e repetir o processo novamente.

No momento em que o ``rightmark`` se tornar menor que o ``leftmark``, paramos.
A posição do ``rightmark`` vira agora o ponto de divisão. O valor do pivô pode
ser trocado com o que estiver no ponto de divisão e o valor do pivô estará
agora em seu devido lugar (:ref:`Figura 14 <fig_partitionB>`). Além disso,
todos os itens à esquerda do ponto de divisão são menores do que o valor do pivô
e todos os itens à direita do ponto de divisão são maiores do que o valor do
pivô. A lista pode agora ser dividida no ponto de divisão e o quic ksort
pode ser chamado recursivamente sobre as duas metades.


.. _fig_partitionB:

.. figure:: Figures/partitionB.png
   :align: center

   Figura 14: Completando o Particionamento Para Encontrar o Ponto de Divisão do 54


A função ``quickSort`` mostrada em :ref:`ActiveCode 1 <lst_quick>` chama uma
função recursiva ``quickSortHelper``, que começa com o mesmo caso base que
o merge sort. Se o tamanho da lista for menor ou igual a um, ela está
ordenada. Se for maior, então ela pode ser particionada e ordenada
recursivamente. A função ``partition`` implementa o processo descrito anteriormente.


.. activecode:: lst_quick
    :caption: Quick Sort

    def quickSort(alist):
       quickSortHelper(alist,0,len(alist)-1)

    def quickSortHelper(alist,first,last):
       if first<last:

           splitpoint = partition(alist,first,last)

           quickSortHelper(alist,first,splitpoint-1)
           quickSortHelper(alist,splitpoint+1,last)


    def partition(alist,first,last):
       pivotvalue = alist[first]

       leftmark = first+1
       rightmark = last

       done = False
       while not done:

           while leftmark <= rightmark and alist[leftmark] <= pivotvalue:
               leftmark = leftmark + 1

           while alist[rightmark] >= pivotvalue and rightmark >= leftmark:
               rightmark = rightmark -1

           if rightmark < leftmark:
               done = True
           else:
               temp = alist[leftmark]
               alist[leftmark] = alist[rightmark]
               alist[rightmark] = temp

       temp = alist[first]
       alist[first] = alist[rightmark]
       alist[rightmark] = temp


       return rightmark

    alist = [54,26,93,17,77,31,44,55,20]
    quickSort(alist)
    print(alist)



.. animation:: quick_anim
   :modelfile: sortmodels.js
   :viewerfile: sortviewers.js
   :model: QuickSortModel
   :viewer: BarViewer


.. Para mais detalhes, o CodeLens 7 irá guiá-lo passo a passo pelo algoritmo.
..
.. .. codelens:: quicktrace
..     :caption: Tracing the Quick Sort
..
..     def quickSort(alist):
..        quickSortHelper(alist,0,len(alist)-1)
..
..     def quickSortHelper(alist,first,last):
..        if first<last:
..
..            splitpoint = partition(alist,first,last)
..
..            quickSortHelper(alist,first,splitpoint-1)
..            quickSortHelper(alist,splitpoint+1,last)
..
..
..     def partition(alist,first,last):
..        pivotvalue = alist[first]
..
..        leftmark = first+1
..        rightmark = last
..
..        done = False
..        while not done:
..
..            while leftmark <= rightmark and \
..                    alist[leftmark] <= pivotvalue:
..                leftmark = leftmark + 1
..
..            while alist[rightmark] >= pivotvalue and \
..                    rightmark >= leftmark:
..                rightmark = rightmark -1
..
..            if rightmark < leftmark:
..                done = True
..            else:
..                temp = alist[leftmark]
..                alist[leftmark] = alist[rightmark]
..                alist[rightmark] = temp
..
..        temp = alist[first]
..        alist[first] = alist[rightmark]
..        alist[rightmark] = temp
..
..
..        return rightmark
..
..     alist = [54,26,93,17,77,31,44,55,20]
..     quickSort(alist)
..     print(alist)

Para anlisar a função ``quickSort``, note que para uma lista de tamanho *n*,
se a partição sempre ocorre no meio da lista, haverá de novo :math:`\log n`
divisões. Para encontrar o ponto de divisão, cada um dos *n* itens precisa
ser comparado com o valor do pivô. O resultado é :math:`n\log n`. Contudo,
não há necessidade de memória adicional como no caso do merge sort.

Infelizmente, no pior caso, os pontos de divisão podem não estar no meio.
Na verdade, eles podem tender muito à esquerda ou à direita,
criando uma separação desigual. Nesse caso, ordenar uma lista de *n* itens
poderia dividir a lista em 0 itens de um lado :math:`n-1` do outro.
Ordenar uma lista de *n-1*, por sua vez, resultaria na divisão de uma lista
de tamanho 0 de um lado e :math:`n-2` do outro, e assim por diante.
O resultado disso é uma ordenação da ordem :math:`O(n^{2})` com o custo
tipicamente requerido por chamadas recursivas.

Nós mencionamos anteriormente que há diferentes formas de escolher o valor
do pivô. Em particular, podemos tentar aliviar parte da tendência de divisões
desiguais usando uma técnica chamada **mediana melhor-de-três*. Para escolher
o valor do pivô, iremos considerar o primeiro, o intermediário e o último
elemento na lista. No nosso exemplo, eles são o 54, o 77 e o 20. Agora
pegamos o valor da mediana, que é o 54, e o utilizamos como pivô (no caso,
o pivô que já havíamos usado originalmente). A ideia é que no caso em que o
primeiro item na lista não pertencer ao meio da lista, a mediana irá
resultar em um melhor "valor intermediário". Isso será particularmente útil
quando a lista original já estiver razoavelmente ordenada. Deixamos a
implementação dessa técnica de seleção de pivô como um exercício.


.. admonition:: Autoavaliação

   .. mchoice:: question_sort_7
      :correct: d
      :answer_a: [9, 3, 10, 13, 12]
      :answer_b: [9, 3, 10, 13, 12, 14]
      :answer_c: [9, 3, 10, 13, 12, 14, 17, 16, 15, 19]
      :answer_d: [9, 3, 10, 13, 12, 14, 19, 16, 15, 17]
      :feedback_a: É importante lembrar que o quicksort atua na lista toda e não requer memória adicional.
      :feedback_b: Lembre-se: o quicksort atua na lista toda e não requer memória adicional.
      :feedback_c: O primeiro particionamento atua na lista toda e o segundo apenas na esquerda, não na direita
      :feedback_d: O primeiro particionamento atua na lista toda e o segundo na partição esquerda.

      Dada a seguinte lista de números [14, 17, 13, 15, 19, 10, 3, 16, 9, 12], qual resposta mostra a lista correta após o segundo particionamento, de acordo com o algoritmo do quicksort?

   .. mchoice:: question_sort_8
       :correct: b
       :answer_a: 1
       :answer_b: 9
       :answer_c: 16
       :answer_d: 19
       :feedback_a: Os três números usados para selecionar o pivô são 1, 9, 19.  O 1 não é o valor médio e seria uma má escolha para o pivô, já que é o menor número na lista.
       :feedback_b: Bom trabalho.
       :feedback_c: embora 16 seja o valor média de 1, 16 e 19, o meio da lista está em len(lista) // 2.
       :feedback_d: os três números usados na seleção do pivô são 1, 9 e 19. O 19 seria uma má escolha já que é quase o maior da lista.

       Dada a seguinte lista de números [1, 20, 11, 5, 2, 9, 16, 14, 13, 19], qual seria o primeiro valor do pivô usando o método da mediana melhor-de-três?


   .. mchoice:: question_sort_9
       :multiple_answers:
       :answer_a: Shell Sort
       :answer_b: Quick Sort
       :answer_c: Merge Sort
       :answer_d: Insertion Sort
       :correct: c
       :feedback_a: O shell sort é da ordem de ``n^1.5``.
       :feedback_b: O quick sort pode ser O(n log n), mas se os pontos do pivô não forem bem escolhidos e dependendo da lista, ele pode ser O(n^2).
       :feedback_c: O merge sort é O(n log n) mesmo no pior caso. Em compensação, o merge sort exige mais memória.
       :feedback_d: O insertion sort é ``O(n^2)``.

       Qual dos algoritmos de ordenação a seguir são sempre O(n log n), mesmo no pior caso?
