..  Copyright (C)  Brad Miller, David Ranum
    This work is licensed under the Creative Commons Attribution-NonCommercial-ShareAlike 4.0 International License. To view a copy of this license, visit http://creativecommons.org/licenses/by-nc-sa/4.0/.


O Bubble Sort
~~~~~~~~~~~~~
O **bubble sort** realiza múltiplas passagem por uma lista. Ele compara itens
adjacentes e troca aqueles que estão fora de ordem. Cada passagem pela lista
coloca o próximo maior valor na sua posição correta. Em essência, cada item
se desloca como uma "bolha" para a posição à qual pertence.

A :ref:`Figura 1 <fig_bubblepass>` mostra a primeira passagem de um bubble sort.
Os itens sombreados são aqueles que estão sendo comparados para verificar se estão
fora de ordem. Se existem *n* itens na lista, então existem :math:`n-1` pares de
itens que precisam ser comparados na primeira passagem. É importante observar
que o maior valor na lista esteja em alguma comparação, ele será continuamente
empurrado até o fim da passagem.


.. _fig_bubblepass:

.. figure:: Figures/bubblepass.png
   :align: center

   Figura 1: ``bubbleSort``: A Primeira Passagem

No começo da segunda passagem, o maior valor agora está ordenado. Ainda existem
:math:`n-1` itens para serem ordenados, o que significa que teremos
:math:`n-2` pares de comparação. Como cada passagem colocar o próximo maior
valor encontrado no lugar certo, o número total de passagens necessárias será
:math:`n-1`. Depois de completar :math:`n-1` passagens, o menor item estará
na posição correta e nenhum processamento adicional será necessário. O
:ref:`ActiveCode 1 <lst_bubble>` mostra a função ``bubbleSort`` completa.
Ela recebe uma lista como parâmetro e a modifica, trocando os itens conforme a
necessidade.

A operação de troca, às vezes chamada de "swap", é ligeiramente diferente
em Python, quando comparada a outras linguagens de programação. Tipicamente,
a troca de dois elementos numa lista requer o uso de uma variável para
armazenamento temporário. O trecho de código abaixo

::

    temp = alist[i]
    alist[i] = alist[j]
    alist[j] = temp

irá trocar os `i-ésimos` com os `j-ésimos` itens na lista. Sem a variável
de armazenamento temorário, um dos valores seria sobrescrito.

Em Python, porém, é possível realizar atribuições simultâneas. A expressão
``a,b=b,a`` irá resultar em duas atribuições sendo feitas ao mesmo tempo
(see :ref:`Figure 2 <fig_pythonswap>`). Usando atribuições simultâneas,
a troca pode ser feita em uma única linha.

As linhas 5-7 em :ref:`ActiveCode 1 <lst_bubble>` realizam a troca entre os
itens :math:`i` e :math:`(i+1)` utilizando o procedimento de três passos
descrito anteriormente. Note que poderíamos ter usado a atribuição
simultânea para trocar os itens.


.. _fig_pythonswap:

.. figure:: Figures/swap.png
   :align: center

   Figura 2: Trocando Dois Valores em Python

O exemplo de activecode a seguir mostra a função ``bubbleSort`` completa
utilizando a lista mostrada acima.



.. activecode:: lst_bubble
    :caption: The Bubble Sort

    def bubbleSort(alist):
        for passnum in range(len(alist)-1,0,-1):
            for i in range(passnum):
                if alist[i]>alist[i+1]:
                    temp = alist[i]
                    alist[i] = alist[i+1]
                    alist[i+1] = temp

    alist = [54,26,93,17,77,31,44,55,20]
    bubbleSort(alist)
    print(alist)

A animação abaixo mostra o ``bubbleSort`` em ação.

.. animation:: bubble_anim
   :modelfile: sortmodels.js
   :viewerfile: sortviewers.js
   :model: BubbleSortModel
   :viewer: BarViewer

.. Para mais detalhes, o CodeLens 1 irá lhe guiar passo a passo pelo algoritmo.
..
.. .. codelens:: bubbletrace
..     :caption: Passo a Passo do BubbleSort
..
..     def bubbleSort(alist):
..         for passnum in range(len(alist)-1,0,-1):
..             for i in range(passnum):
..                 if alist[i]>alist[i+1]:
..                     temp = alist[i]
..                     alist[i] = alist[i+1]
..                     alist[i+1] = temp
..
..     alist = [54,26,93,17,77,31,44,55,20]
..     bubbleSort(alist)
..     print(alist)

Para analisar o bubble sort, devemos perceber que independente de como os
itens estão dispostos na lista inicial, :math:`n-1` passagens serão feitas
para ordenar uma lista de tamanho *n*. A :ref:`Tabela 1 <tbl_bubbleanalysis>`
mostra o número de comparações para cada passagem. O número total de comparações
é a soma dos primeiros :math:`n-1` inteiros. Lembre-se de que a soma dos
primeiros *n* inteiros é :math:`\frac{1}{2}n^{2} + \frac{1}{2}n`. A soma dos
primeiros :math:`n-1` inteiros é :math:`\frac{1}{2}n^{2} + \frac{1}{2}n - n`,
que é :math:`\frac{1}{2}n^{2} - \frac{1}{2}n`. Ou seja, continuamos com
:math:`O(n^{2})` comparações. No melhor caso, se a lista já estiver ordenada,
nenhuma mudança será feita. Contudo, no pior caso, cada comparação resultará
numa troca. Na média, trocamos metade das vezes.

.. _tbl_bubbleanalysis:

.. table:: **Tabela 1: Comparações para Cada Passagem do Bubble Sort**

    ================= ==================
    **Passagem**      **Comparações**
    ================= ==================
             1         :math:`n-1`
             2         :math:`n-2`
             3         :math:`n-3`
             ...       ...
       :math:`n-1`     :math:`1`
    ================= ==================


O bubble sort com frequência é considerado o método de ordenação mais
ineficiente, já que ele precisa realizar a troca de itens sem saber qual será
sua posição final. Essas trocas "desnecessárias" são muito custosas. Contudo,
justamente por realizar passagens pela porção desordenada da lista, o
bubble sort consegue fazer o que a maioria dos outros algoritmos de ordenação
não consegue. Em particular, se durante uma passagem não houver trocas, então
sabemos que a lista está ordenada. O bubble sort pode ser modificado para
terminar antes se descobrir que a lista ficou ordenada. Isso significa que
para listas que requerem apenas algumas passagens, o bubble sort pode ter a
vantagem de reconhecer a lista ordenada e parar. O :ref:`ActiveCode 2 <lst_shortbubble>`
mostra essa modificação, a qual é comumente chamada de **short bubble**.


.. activecode:: lst_shortbubble
    :caption: O Short Bubble Sort

    def shortBubbleSort(alist):
        exchanges = True
        passnum = len(alist)-1
        while passnum > 0 and exchanges:
           exchanges = False
           for i in range(passnum):
               if alist[i]>alist[i+1]:
                   exchanges = True
                   temp = alist[i]
                   alist[i] = alist[i+1]
                   alist[i+1] = temp
           passnum = passnum-1

    alist=[20,30,40,90,50,60,70,80,100,110]
    shortBubbleSort(alist)
    print(alist)

.. Finalmente, aqui está o ``shortBubbleSort`` no CodeLens (CodeLens 2)..
..
.. .. codelens:: shortbubbletrace
..     :caption: Registrando o Short Bubble Sort
..
..     def shortBubbleSort(alist):
..         exchanges = True
..         passnum = len(alist)-1
..         while passnum > 0 and exchanges:
..            exchanges = False
..            for i in range(passnum):
..                if alist[i]>alist[i+1]:
..                    exchanges = True
..                    temp = alist[i]
..                    alist[i] = alist[i+1]
..                    alist[i+1] = temp
..            passnum = passnum-1
..
..     alist=[20,30,40,90,50,60,70,80,100,110]
..     shortBubbleSort(alist)
..     print(alist)

.. admonition:: Autoavaliação

   .. mchoice:: question_sort_1
       :correct: b
       :answer_a: [1, 9, 19, 7, 3, 10, 13, 15, 8, 12]
       :answer_b: [1, 3, 7, 9, 10, 8, 12, 13, 15, 19]
       :answer_c: [1, 7, 3, 9, 10, 13, 8, 12, 15, 19]
       :answer_d: [1, 9, 19, 7, 3, 10, 13, 15, 8, 12]
       :feedback_a:  Esta resposta representa três trocas. Uma passagem significa que você continua realizando trocas durante todo o tempo até o fim da lista.
       :feedback_b:  Muito bem.
       :feedback_c: O bubble sort continua a trocar os número até a posição do índice passnum. Mas lembre-se de que passnum começa com o tamanho da lista - 1.
       :feedback_d: Você fez a ordenação por inserção, não o bubble sort.

       Suponha que você tenha a seguinte lista de números para ordenar: <br>
       [19, 1, 9, 7, 3, 10, 13, 15, 8, 12] qual lista representa a lista parcialmente ordenada depois de três passagens completas do bubble sort?
