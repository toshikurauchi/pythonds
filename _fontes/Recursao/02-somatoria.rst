..  Copyright (C)  Brad Miller, David Ranum
    This work is licensed under the Creative Commons Attribution-NonCommercial-ShareAlike 4.0 International License. To view a copy of this license, visit http://creativecommons.org/licenses/by-nc-sa/4.0/.


..  Calculating the Sum of a List of Numbers
    ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Calculando a Soma de Uma Lista de Números
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. We will begin our investigation with a simple problem that you already
   know how to solve without using recursion. Suppose that you want to
   calculate the sum of a list of numbers such as:
   :math:`[1, 3, 5, 7, 9]`. An iterative function that computes the sum
   is shown in :ref:`ActiveCode 1 <lst_itsum>`. The function uses an accumulator variable
   (``theSum``) to compute a running total of all the numbers in the list
   by starting with :math:`0` and adding each number in the list.

Nós vamos iniciar nossa investigação com um problema simples que
você já sabe resolver sem usar recursão. Imagine que você queira
calcular a soma de uma lista de números tal como:
:math:`[1, 3, 5, 7, 9]`. Uma função iterativa que calcula
a soma é mostrada em :ref:`ActiveCode 1<lst_itsum>`.
A função usa uma variável acumuladora ("soma") que calcula
a somatória de todos os números da lista começando com
:math:`0` e adicionando cada número da lista. 

.. activecode:: lst_itsum
    :caption: Somatória Iterativa

    def somaLista(numeros):
        soma = 0
        for i in numeros:
            soma = soma + i
        return soma
        
    print(somaLista([1,3,5,7,9]))

.. Pretend for a minute that you do not have ``while`` loops or ``for``
   loops. How would you compute the sum of a list of numbers? If you were a
   mathematician you might start by recalling that addition is a function
   that is defined for two parameters, a pair of numbers. To redefine the
   problem from adding a list to adding pairs of numbers, we could rewrite
   the list as a fully parenthesized expression. Such an expression looks
   like this:

Imagine por um minuto que você não tem os comandos de repetição ``while``
e ``for``. Como você calcularia a soma de uma lista de números? Se você
fosse um matemático, você poderia começar lembrando que a adição é uma
função definida par dois parâmetros, um par de números. Para redefinir
o problema de adição de lista para adição de um par de números, podemos
reescrever a lista como uma expressão com parênteses. Essa expressão
poderia ser como essa:

.. math::
    ((((1 + 3) + 5) + 7) + 9)
    
.. We can also parenthesize
   the expression the other way around,

Nós podemos também formar uma expressão com parênteses em ordem contrária:

.. math::

     (1 + (3 + (5 + (7 + 9)))) 

.. Notice that the innermost set of
   parentheses, :math:`(7 + 9)`, is a problem that we can solve without a
   loop or any special constructs. In fact, we can use the following
   sequence of simplifications to compute a final sum.

Observe que o conjunto mais interno de parênteses, :math:`(7 + 9)`,
é um problema que podemos resolver sem comandos de repetição ou qualquer
construção especial. De fato, podemos usar a seguinte sequência de
simplificações para calcular a soma final.

.. math::

    total = \  (1 + (3 + (5 + (7 + 9)))) \\
    total = \  (1 + (3 + (5 + 16))) \\
    total = \  (1 + (3 + 21)) \\
    total = \  (1 + 24) \\
    total = \  25


.. How can we take this idea and turn it into a Python program? First,
   let’s restate the sum problem in terms of Python lists. We might say the
   the sum of the list ``numList`` is the sum of the first element of the
   list (``numList[0]``), and the sum of the numbers in the rest of the
   list (``numList[1:]``). To state it in a functional form:

Como podemos considerar essa ideia e aplicá-la em um programa em Python? Primeiro
vamos considerar o problema de soma em termos de listas em Python. Podemos dizer
que a soma da lista ``numeros`` é a soma do primeiro elemento da lista
(``numeros[0]``), e a soma dos números do resto da lista
(``numeros[1:]``). Para escrever isso em forma funcional:

.. math::

      somaLista(numeros) = primeiro(numeros) + somaLista(resto(numeros))
    \label{eqn:listsum}


.. In this equation :math:`first(numList)` returns the first element of
   the list and :math:`rest(numList)` returns a list of everything but
   the first element. This is easily expressed in Python as shown in
   :ref:`ActiveCode 2 <lst_recsum>`.

Nessa equação :math:`primeiro(numeros)` retorna o primeiro elemento da lista
e :math:`resto(numeros)` retorna a lista menos o primeiro elemento. Isso é
facilmente expresso em Python como mostrado em
:ref:`ActiveCode 2 <lst_recsum>`.
      
.. activecode:: lst_recsum
    :caption: Somatória Recursiva

    def somaLista(numeros):
       if len(numeros) == 1:
            return numeros[0]
       else:
            return numeros[0] + somaLista(numeros[1:])
            
    print(somaLista([1,3,5,7,9]))

.. There are a few key ideas in this listing to look at. First, on line 2 we are checking to see if the list is one element long. This
   check is crucial and is our escape clause from the function. The sum of
   a list of length 1 is trivial; it is just the number in the list.
   Second, on line 5 our function calls itself! This is the
   reason that we call the ``listsum`` algorithm recursive. A recursive
   function is a function that calls itself.

Há algumas ideias básicas que devem ser observadas nesse código. Primeiro,
na linha 2 nós verificamos se a lista possui apenas um elemento. Essa
verificação é crucial e é a forma de sair da função. A soma de uma lista
de comprimento 1 é trivial; é simplesmente o número que está na lista.
Segundo, na linha 5, nossa função chama a si mesma! Essa é a razão
pela qual chamamos o algoritmo ``somaLista`` de recursivo. Uma função
recursiva é uma função que chama a si mesma.

.. :ref:`Figure 1 <fig_recsumin>` shows the series of **recursive calls** that are
   needed to sum the list :math:`[1, 3, 5, 7, 9]`. You should think of
   this series of calls as a series of simplifications. Each time we make a
   recursive call we are solving a smaller problem, until we reach the
   point where the problem cannot get any smaller.

A :ref:`figura 1 <fig_recsumin>` mostra a série de **chamadas recursivas**
necessárias para somar a lista :math:`[1, 3, 5, 7, 9]`. Você deve considerar
essa série de chamadas como um série de simplificações. Cada vez que fazemos
uma chamada recursiva estamos resolvendo um problema menor, até chegar a um
ponto onde o problema não precisa ficar menor.

.. _fig_recsumin:

.. figure:: Figures/sumlistIn.png
   :align: center
   :alt: image


   Figura 1: Série de chamadas recursivas somando a lista de números

.. When we reach the point where the problem is as simple as it can get, we
   begin to piece together the solutions of each of the small problems
   until the initial problem is solved. :ref:`Figure 2 <fig_recsumout>` shows the
   additions that are performed as ``listsum`` works its way backward
   through the series of calls. When ``listsum`` returns from the topmost
   problem, we have the solution to the whole problem.

Quando chegamos ao ponto onde o problema se torna o mais simples possível, nós
passamos a montar as soluções de cada um dos problemas menores até resolver
o problema inicial. A :ref:`figura 2 <fig_recsumout>` mostra as adições
realizadas a medida que ``somaLista`` vai retornando pela série de chamadas.
Quando ``somaLista`` retorna da primeira chamada, nós obtemos a solução de todo o
problema.

.. _fig_recsumout:

.. figure:: Figures/sumlistOut.png
   :align: center
   :alt: image

   Figura 2: Série de returns recursivos da adição de uma lista de números

