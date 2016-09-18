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

Antes de irmos para a análise, note que esse algoritmo é um ótimo exemplo
da estratégia de dividir para conquistar, isto é, nós dividimos o problema
em partes menores, resolvemos as menores partes de algum modo e então
remontamos tudo para chegar ao resultado. Quando nós realizamos uma
busca binária de uma lista, primeiro checamos o item do meio. Se o
item que estamos procurando é menor que o item intermediário, nós
simplesmente fazer uma busca binária na metade esquerda da lista original.
Do mesmo modo, se o item for maior, nós realizamos uma
binária na metade direita. De qualquer forma, trata-se de uma chamada
recursiva da função de busca binária passando uma lista menor como
parâmetro. :ref:`CodeLens 4 <lst_recbinarysearch>` mostra essa versão
recursiva.


.. _lst_recbinarysearch:

.. codelens:: search4
    :caption: Uma Busca Binária--Versão Recursiva

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



Análise da Busca Binária
^^^^^^^^^^^^^^^^^^^^^^^^

Para analisar o algoritmo de busca binária, precisamos ter em mente que cada
comparação elimina cerca de metade dos itens restantes a serem considerados.
Qual é o número máximo de comparações que esse algoritmo irá requerer para
checar a lista inteira? Se começarmos com *n* itens, em torno de :math:`\frac{n}{2}`
itens sobrarão após a primeira comparação. Depois da segunda comparação,
haverá cerca de :math:`\frac{n}{4}`, depois :math:`\frac{n}{8}`,
:math:`\frac{n}{16}` e assim por diante. Mas quantas vezes podemos dividir
essa lista? A :ref:`Tabela 3 <tbl_binaryanalysis>` nos ajuda a responder
essa pergunta.


.. _tbl_binaryanalysis:

.. table:: **Tabela 3: Análise da Busca Binária**

    ======================== ========================================
             **Comparações** **Número aproximado de itens restantes**
    ======================== ========================================
                           1                   :math:`\frac {n}{2}`
                           2                   :math:`\frac {n}{4}`
                           3                   :math:`\frac {n}{8}`
                         ...
                           i                 :math:`\frac {n}{2^i}`
    ======================== ========================================

Quando nós quebramos a lista um número suficiente de vezes, acabamos
ficando com uma lista composta por um único item. Logo, ou esse item é
aquele que estamos buscando ou não é. De qualquer forma, o processo
termina. O número necessário de comparações para chegar a esse ponto é
*i*, onde :math:`\frac {n}{2^i} = 1`. Isolando *i*, ficamos com
:math:`i=\log n`. Assim, o número máximo de comparações é o logaritmo
do número de itens na lista. Portanto, a busca binária é :math:`O(\log n)`.

Uma questão adicional de análise ainda precisa ser tratada. Na solução
recursiva mostrada acima, a chamada recursiva

``binarySearch(alist[:midpoint],item)``

usa o operador de fatiamento para criar a metade esquerda da lista que é
passada para a próxima chamada (e, da mesma maneira, para a metade direita).
A análise que fizemos acima pressupõe que o operador de fatiamento tem
tempo constante. Contudo, nós sabemos que na verdade o operador de
fatiamento em Python é O(k). Isso significa que a busca binária usando esse
operador não irá ter um desempenho estritamente logarítmico. Felizmente isso
pode ser remediado passando a lista junto com os índices de começo e fim.
Os índices podem ser calculados como fizemos em :ref:`Listing 3` <lst_binarysearchpy>`.
Deixamos essa implementação como um exercício.

Embora uma busca binária seja geralmente melhor que uma sequencial, é importante
notar que para valores pequenos de *n*, o custo adicional de ordenar a lista
provavelmente não vale a pena. Na verdade, sempre deveríamos considerar
o custo-benefício do trabalho adicional de ordenação para obter benefícios
na busca. Se nós podemos ordenar uma vez e então fazer inúmeras buscas,
o custo da ordenação não é tão significativo. Entretanto, para listas muito
grandes, a ordenação, mesmo que feita uma única vez, pode ser tão cara que
simplesmente fazer uma busca sequencial desde o começo pode se mostrar
uma escolha melhor.


.. admonition:: Auto-avaliação

   .. mchoice:: BSRCH_1
      :correct: b
      :answer_a: 11, 5, 6, 8
      :answer_b: 12, 6, 11, 8
      :answer_c: 3, 5, 6, 8
      :answer_d: 18, 12, 6, 8
      :feedback_a:  Parece que você errou por um. Lembre-se de que a primeira posição tem índice 0.
      :feedback_b:  A busca binária começa pelo meio e divide a lista um turno por vez.
      :feedback_c: A busca binária não começa pelo início e busca sequencialmente, ela começa pelo meio e divide a lista após cada comparação.
      :feedback_d: Parece que você está começando pelo fim e dividindo a lista um turno por vez.

      Suponha que você tenha a lista ordenada [3, 5, 6, 8, 11, 12, 14, 15, 17, 18] e que você esteja usando o algoritmo recursivo da busca binária. Qual grupo de números mostra corretamente a sequência de comparações usadas para encontrar a chave 8?

   .. mchoice:: BSRCH_2
      :correct: d
      :answer_a: 11, 14, 17
      :answer_b: 18, 17, 15
      :answer_c: 14, 17, 15
      :answer_d: 12, 17, 15
      :feedback_a:  Parece que você errou por um. Lembre-se de que a primeira posição tem índice 0.
      :feedback_b:  Lembre-se: a busca binária começa pelo meio e depois divide a lista.
      :feedback_c:  Parece que você errou por um. Cuidado quando estiver calculando o ponto central usando aritmética de inteiros.
      :feedback_d: A busca binária começa pelo elemento do meio e divide a lista a cada turno. Ela termina quando a lista está vazia.

      Suponha que você tenha a lista ordenada [3, 5, 6, 8, 11, 12, 14, 15, 17, 18] e que você esteja usando o algoritmo recursivo da busca binária. Qual grupo de números mostra corretamente a sequência de comparações usadas para encontrar a chave 16?
