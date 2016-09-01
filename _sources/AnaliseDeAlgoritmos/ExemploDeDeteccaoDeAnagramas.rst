..  Copyright (C)  Brad Miller, David Ranum
    This work is licensed under the Creative Commons Attribution-NonCommercial-ShareAlike 4.0 International License. To view a copy of this license, visit http://creativecommons.org/licenses/by-nc-sa/4.0/.


Exemplo: Detecção de Anagramas
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Um bom exemplo de problema para mostrar algoritmos com diferentes ordens
de magnitude é o problema clássico de detecção de anagramas para strings.
Uma string é um anagrama de outra se a segunda é simplesmente um rearranjo
da primeira. Por exemplo, ``'amor'`` e ``'roma'`` são anagramas. As strings
``'marrocos'`` e ``'socorram'`` também são anagramas. Por uma questão de 
simplicidade, vamos assumir que as duas strings em questão são de 
comprimentos iguais e que são compostas por símbolos do conjunto dos 26
caracteres alfabéticos minúsculos. Nosso objetivo é escrever uma função
booleana que receberá duas strings e devolverá se elas são anagramas.

Solução 1: Marcação
^^^^^^^^^^^^^^^^^^^

Nossa primeira solução para o problema dos anagramas verificará se cada
caractere na primeira string realmente ocorre na segunda. Se for possível
"marcar" cada caractere, então as duas strings são anagramas. A marcação
de um caractere será realizada substituindo-o pelo valor especial em 
Python ``None``. Entretanto, como strings em Python são imutáveis, o 
primeiro passo do processo será converter a segunda string em uma lista.
Cada caractere da primeira string será buscado entre os caracteres da
segunda lista e, caso encontrado, será marcado por substituição.
:ref:`ActiveCode 1 <lst_anagramSolution>` mostra essa função.

.. _lst_anagramSolution:

.. activecode:: active5
    :caption: Marcação

    def solucaoAnagrama1(s1,s2):
        umalista = list(s2)

        pos1 = 0
        aindalOK = True

        while pos1 < len(s1) and aindalOK:
            pos2 = 0
            encontrado = False
            while pos2 < len(umalista) and not encontrado:
                if s1[pos1] == umalista[pos2]:
                    encontrado = True
                else:
                    pos2 = pos2 + 1

            if encontrado:
                umalista[pos2] = None
            else:
                aindalOK = False

            pos1 = pos1 + 1

        return aindalOK

    print(solucaoAnagrama1('abcd','dcba'))

Para analisar este algoritmo, devemos notar que cada um dos *n*
caracteres em ``s1`` causará uma iteração por até *n* caracteres
na lista de ``s2``. Cada uma das *n* posições na lista será visitada
uma vez para marcar um caractere de ``s1``. O número de visitas 
então é dado pela soma dos inteiros entre 1 e *n*. Vimos 
anteriormente que isso pode ser escrito como

.. math::

   \sum_{i=1}^{n} i &= \frac {n(n+1)}{2} \\
                    &= \frac {1}{2}n^{2} + \frac {1}{2}n

Conforme :math:`n` aumenta, o termo :math:`n^{2}` dominará sobre
o termo :math:`n` e o :math:`\frac {1}{2}` pode ser ignorado.
Assim, a solução é :math:`O(n^{2})`.

Solução 2: Ordenar e Comparar
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Another solution to the anagram problem will make use of the fact that
even though ``s1`` and ``s2`` are different, they are anagrams only if
they consist of exactly the same characters. So, if we begin by sorting
each string alphabetically, from a to z, we will end up with the same
string if the original two strings are anagrams. :ref:`ActiveCode 2 <lst_ana2>` shows
this solution. Again, in Python we can use the built-in ``sort`` method
on lists by simply converting each string to a list at the start.

.. _lst_ana2:

.. activecode:: active6
    :caption: Sort and Compare

    def anagramSolution2(s1,s2):
        alist1 = list(s1)
        alist2 = list(s2)

        alist1.sort()
        alist2.sort()

        pos = 0
        matches = True

        while pos < len(s1) and matches:
            if alist1[pos]==alist2[pos]:
                pos = pos + 1
            else:
                matches = False

        return matches

    print(anagramSolution2('abcde','edcba'))

At first glance you may be tempted to think that this algorithm is
:math:`O(n)`, since there is one simple iteration to compare the *n*
characters after the sorting process. However, the two calls to the
Python ``sort`` method are not without their own cost. As we will see in
a later chapter, sorting is typically either :math:`O(n^{2})` or
:math:`O(n\log n)`, so the sorting operations dominate the iteration.
In the end, this algorithm will have the same order of magnitude as that
of the sorting process.

Solution 3: Brute Force
^^^^^^^^^^^^^^^^^^^^^^^

A **brute force** technique for solving a problem typically tries to
exhaust all possibilities. For the anagram detection problem, we can
simply generate a list of all possible strings using the characters from
``s1`` and then see if ``s2`` occurs. However, there is a difficulty
with this approach. When generating all possible strings from ``s1``,
there are *n* possible first characters, :math:`n-1` possible
characters for the second position, :math:`n-2` for the third, and so
on. The total number of candidate strings is
:math:`n*(n-1)*(n-2)*...*3*2*1`, which is :math:`n!`. Although some
of the strings may be duplicates, the program cannot know this ahead of
time and so it will still generate :math:`n!` different strings.

It turns out that :math:`n!` grows even faster than :math:`2^{n}` as
*n* gets large. In fact, if ``s1`` were 20 characters long, there would
be :math:`20!=2,432,902,008,176,640,000` possible candidate strings.
If we processed one possibility every second, it would still take us
77,146,816,596 years to go through the entire list. This is probably not
going to be a good solution.

Solution 4: Count and Compare
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Our final solution to the anagram problem takes advantage of the fact
that any two anagrams will have the same number of a’s, the same number
of b’s, the same number of c’s, and so on. In order to decide whether
two strings are anagrams, we will first count the number of times each
character occurs. Since there are 26 possible characters, we can use a
list of 26 counters, one for each possible character. Each time we see a
particular character, we will increment the counter at that position. In
the end, if the two lists of counters are identical, the strings must be
anagrams. :ref:`ActiveCode 3 <lst_ana4>` shows this solution.

.. _lst_ana4:

.. activecode:: active7
    :caption: Count and Compare

    def anagramSolution4(s1,s2):
        c1 = [0]*26
        c2 = [0]*26

        for i in range(len(s1)):
            pos = ord(s1[i])-ord('a')
            c1[pos] = c1[pos] + 1

        for i in range(len(s2)):
            pos = ord(s2[i])-ord('a')
            c2[pos] = c2[pos] + 1

        j = 0
        stillOK = True
        while j<26 and stillOK:
            if c1[j]==c2[j]:
                j = j + 1
            else:
                stillOK = False

        return stillOK

    print(anagramSolution4('apple','pleap'))



Again, the solution has a number of iterations. However, unlike the
first solution, none of them are nested. The first two iterations used
to count the characters are both based on *n*. The third iteration,
comparing the two lists of counts, always takes 26 steps since there are
26 possible characters in the strings. Adding it all up gives us
:math:`T(n)=2n+26` steps. That is :math:`O(n)`. We have found a
linear order of magnitude algorithm for solving this problem.

Before leaving this example, we need to say something about space
requirements. Although the last solution was able to run in linear time,
it could only do so by using additional storage to keep the two lists of
character counts. In other words, this algorithm sacrificed space in
order to gain time.

This is a common occurrence. On many occasions you will need to make
decisions between time and space trade-offs. In this case, the amount of
extra space is not significant. However, if the underlying alphabet had
millions of characters, there would be more concern. As a computer
scientist, when given a choice of algorithms, it will be up to you to
determine the best use of computing resources given a particular
problem.

.. admonition:: Self Check

   .. mchoice:: analysis_1
       :answer_a: O(n)
       :answer_b: O(n^2)
       :answer_c: O(log n)
       :answer_d: O(n^3)
       :correct: b
       :feedback_a: In an example like this you want to count the nested loops. especially the loops that are dependent on the same variable, in this case, n.
       :feedback_b: A singly nested loop like this is O(n^2)
       :feedback_c: log n typically is indicated when the problem is iteratvely made smaller
       :feedback_d: In an example like this you want to count the nested loops. especially the loops that are dependent on the same variable, in this case, n.

       Given the following code fragment, what is its Big-O running time?

       .. code-block:: python

         test = 0
         for i in range(n):
            for j in range(n):
               test = test + i * j

   .. mchoice:: analysis_2
       :answer_a: O(n)
       :answer_b: O(n^2)
       :answer_c: O(log n)
       :answer_d: O(n^3)
       :correct: a
       :feedback_b: Be careful, in counting loops you want to make sure the loops are nested.
       :feedback_d: Be careful, in counting loops you want to make sure the loops are nested.
       :feedback_c: log n typically is indicated when the problem is iteratvely made smaller
       :feedback_a: Even though there are two loops they are not nested.  You might think of this as O(2n) but we can ignore the constant 2.

       Given the following code fragment what is its Big-O running time?

       .. code-block:: python

         test = 0
         for i in range(n):
            test = test + 1

         for j in range(n):
            test = test - 1

   .. mchoice:: analysis_3
       :answer_a: O(n)
       :answer_b: O(n^2)
       :answer_c: O(log n)
       :answer_d: O(n^3)
       :correct: c
       :feedback_a: Look carefully at the loop variable i.  Notice that the value of i is cut in half each time through the loop.  This is a big hint that the performance is better than O(n)
       :feedback_b: Check again, is this a nested loop?
       :feedback_d: Check again, is this a nested loop?       
       :feedback_c: The value of i is cut in half each time through the loop so it will only take log n iterations.

       Given the following code fragment what is its Big-O running time?

       .. code-block:: python

         i = n
         while i > 0:
            k = 2 + 2
            i = i // 2
