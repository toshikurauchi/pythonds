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




Nós presumimos anteriormente que os itens em nossa coleção foram
posicionados aleatoriamente de modo que não houvesse ordem relativa
entre eles. O que aconteceria à busca sequencial se os itens estivessem
ordenados de alguma forma? Conseguiríamos obter alguma eficiência
na nossa técnica de busca?

Suponha que a lista de itens tenha sido construída de tal forma que eles
tenham sido posicionados em ordem crescente, do menor para o maior. Se
o item que estamos buscando estiver na lista, a probabilidade de ele estar
em uma das *n* posições continua a mesma. Nós ainda temos que fazer o mesmo
número de comparações para encontrá-lo. Contudo, se o item não estiver
presente, há uma pequena vantagem. A :ref:`Figura 2 <fig_seqsearch2>`
exemplifica isso conforme o algoritmo procura pelo item 50. Observe que os
elementos continuam sendo comparados sequencialmente até o 54. Mas nesse
ponto, porém, temos uma informação extra. Além de o item 54 não ser aquele
que estamos procurando, não é possível que o nosso item esteja em alguma
posição superior a 54, pois a lista está ordenada. Nesse caso, o algoritmo
não precisa continuar procurando no restante da lista para informar que o
item não foi encontrado. Ele pode parar imediatamente. O
:ref:`CodeLens 2 <lst_seqsearchpython2>` mostra essa variação da
função de busca sequencial.


.. _fig_seqsearch2:

.. figure:: Figures/seqsearch2.png
   :align: center

   Figura 2: Busca Sequencial em uma Lista Ordenada de Inteiros



.. _lst_seqsearchpython2:

.. codelens:: search2
    :caption: Busca Sequencial em uma Lista Ordenada

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

A :ref:`Tabela 2 <tbl_seqsearchtable2>` resume esses resultados. Observe que
no melhor caso nós temos condições de saber se o item não está na lista
olhando apenas para um elemento. Na média, iremos descobrir isso depois de
procurar por apenas :math:`\frac {n}{2}` itens. No entanto, essa técnica
é ainda :math:`O(n)`. Em suma, a busca sequencial é melhorada quando
sabemos que a lista está ordenada somente no caso em que não encontramos
o item.


.. _tbl_seqsearchtable2:

.. table:: **Tabela 2: Comparações Usadas na Busca Sequencial de uma Lista Ordenada**


     ================= =============== ==============  ===================
                       **Melhor Caso**  **Pior Caso**  **Caso Médio**
     ================= =============== ==============  ===================
     item presente     :math:`1`        :math:`n`     :math:`\frac{n}{2}`
     item não presente :math:`1`        :math:`n`     :math:`\frac{n}{2}`
     ================= =============== ==============  ===================


.. admonition:: Auto-avaliação

   .. mchoice:: question_SRCH_1
      :correct: d
      :answer_a: 5
      :answer_b: 10
      :answer_c: 4
      :answer_d: 2
      :feedback_a: Cinco comparações iriam devolver o segundo 18 na lista.
      :feedback_b: Você não precisa buscar na lista toda, apenas até encontrar a chave que você está procurando.
      :feedback_c: Não, lembre-se de que numa busca sequencial você inicia pelo começo e checa cada chave até encontrar o que você está procurando ou a lista terminar.
      :feedback_d: Neste caso, apenas 2 comparações foram necessárias para encontrar a chave.

      Suponha que você esteja fazendo uma busca sequencial na lista [15, 18, 2, 19, 18, 0, 8, 14, 19, 14]. Quantas comparações você precisaria fazer para encontrar a chave 18?

   .. mchoice:: question_SRCH_2
      :correct: c
      :answer_a: 10
      :answer_b: 5
      :answer_c: 7
      :answer_d: 6
      :feedback_a: Você não precisa buscar na lista inteira. Como ela está ordenada, você pode parar de procurar quando você fizer uma comparação com um valor maior do que a chave.
      :feedback_b: Como 11 é menor do que 13, você precisa continuar procurando.
      :feedback_c: Como 14 é maior do que 13, você pode parar.
      :feedback_d: Como 12 é menor do que 13, você precisa continuar procurando.

      Suponha que você esteja fazendo uma busca sequencial na lista ordenada [3, 5, 6, 8, 11, 12, 14, 15, 17, 18]. Quantas comparações você precisaria fazer para encontrar a chave 13?
