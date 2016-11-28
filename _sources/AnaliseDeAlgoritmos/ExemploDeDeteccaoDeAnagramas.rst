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
        aindaOK = True

        while pos1 < len(s1) and aindaOK:
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
                aindaOK = False

            pos1 = pos1 + 1

        return aindaOK

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

Outra solução para o problema do anagrama utilizará o fato de que mesmo
que ``s1`` e ``s2`` sejam diferentes, eles são anagramas somente se 
consistem exatamente dos mesmos caracteres. Então, se começarmos ordenando
cada string alfabeticamente, de a a z, concluiremos com a mesma string
se as duas strings originais forem anagramas. :ref:`ActiveCode 2 <lst_ana2>`
mostra essa solução. Novamente, em Python podemos utilizar o método ``sort``
em listas simplesmente convertendo cada string em uma lista no início.

.. _lst_ana2:

.. activecode:: active6
    :caption: Ordenar e Comparar

    def solucaoAnagrama2(s1,s2):
        umalista1 = list(s1)
        umalista2 = list(s2)

        umalista1.sort()
        umalista2.sort()

        pos = 0
        iguais = True

        while pos < len(s1) and iguais:
            if umalista1[pos]==umalista2[pos]:
                pos = pos + 1
            else:
                iguais = False

        return iguais

    print(solucaoAnagrama2('abcde','edcba'))

À primeira vista você pode se sentir tentado a pensar que este algoritmo
é :math:`O(n)`, já que há somente uma simples iteração para comparar os
*n* caracteres depois do processo de ordenação. Entretanto, as duas 
chamadas ao método ``sort`` do Python também têm seus custos. Como 
veremos no último capítulo, ordenações são tipicamente :math:`O(n^{2})` 
ou :math:`O(n\log n)`, então as operações de ordenação dominam sobre a
iteração. No fim, este algoritmo terá a mesma ordem de magnitude do 
processo de ordenação.

Solução 3: Força Bruta
^^^^^^^^^^^^^^^^^^^^^^

Uma técnica de **força bruta** para resolver um problema tipicamente
tenta todas as possibilidades à exaustão. Para o problema de detecção
de anagramas, podemos simplesmente gerar uma lista com todas as strings
possíveis usando os caracteres de ``s1`` e verificar se ``s2`` ocorre.
Entretanto, há uma dificuldade com esta abordagem. Ao gerar todas as
strings possíveis a partir de ``s1``, há *n* possíveis primeiros
caracteres, :math:`n-1` possíveis caracteres para a segunda posição,
:math:`n-2` para a terceira e assim por diante. O número total de 
strings candidatas é :math:`n*(n-1)*(n-2)*...*3*2*1`, ou seja, :math:`n!`.
Embora algumas das strings sejam duplicadas, o programa não pode saber
disso de antemão e então ele ainda gerará :math:`n!` strings diferentes.

Acontece que :math:`n!` cresce mais rápido ainda do que :math:`2^{n}`
conforme *n* cresce. De fato, se ``s1`` contivesse 20 caracteres, haveria
:math:`20!=2.432.902.008.176.640.000` possíveis strings candidatas. Se
processássemos uma possibilidade a cada segundo, ainda seriam necessários
77.146.816.596 anos para percorrer toda a lista. Esta provavelmente não
será uma boa solução.

Solução 4: Contar e Comparar
^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Nossa solução final para o problema do anagrama se aproveita do fato de
que quaisquer dois anagramas terão o mesmo número de a's, o mesmo número
de b's, o mesmo número de c's e assim por diante. Para decidir se duas
strings são anagramas, vamos inicialmente contar o número de vezes que
cada caractere ocorre. Como há 26 possibilidades de caracteres, podemos
utilizar uma lista de 26 contadores, um para cada caractere possível. 
Cada vez que encontramos um caractere em particular, incrementamos o 
seu contador naquela posição. Ao final, se as duas listas de contadores
são idênticas, as strings são anagramas. :ref:`ActiveCode 3 <lst_ana4>`
mostra esta solução.

.. _lst_ana4:

.. activecode:: active7
    :caption: Contar e Comparar

    def solucaoAnagrama4(s1,s2):
        c1 = [0]*26
        c2 = [0]*26

        for i in range(len(s1)):
            pos = ord(s1[i])-ord('a')
            c1[pos] = c1[pos] + 1

        for i in range(len(s2)):
            pos = ord(s2[i])-ord('a')
            c2[pos] = c2[pos] + 1

        j = 0
        aindaOK = True
        while j<26 and aindaOK:
            if c1[j]==c2[j]:
                j = j + 1
            else:
                aindaOK = False

        return aindaOK

    print(solucaoAnagrama4('marrocos','socorram'))


Novamente, a solução tem algumas iterações. Entretanto, diferentemente 
da primeira solução, nenhuma delas é aninhada. As duas primeiras
iterações utilizadas para contar os caracteres são ambas baseadas em
*n*. A terceira iteração, comparando as duas listas de contadores, sempre
demoram 26 passos, já que há somente 26 possíveis caracteres nas strings.
Adicionando tudo temos :math:`T(n)=2n+26` passos. Ou seja, :math:`O(n)`.
Encontramos um algoritmo com ordem de magnitude linear para resolver
este problema.

Antes de deixarmos este exemplo, devemos dizer algo sobre requisitos de
espaço. Embora a última solução tenha sido capaz de ser executada em tempo
linear, ela só pôde fazer isso utilizando memória adicional para armazenar
as duas listas de contadores de caracteres. Em outras palavra, este 
algoritmo sacrificou espaço para ganhar tempo.

Isso é bastante comum. Em diversas ocasiões você deverá tomar decisões
entre tempo e espaço. Neste caso, a quantidade de espaço adicional não
é significativa. Entretanto, se o alfabeto utilizado tivesse milhões de
caracteres, seria uma preocupação maior. Como um cientista da computação,
quando dada a escolha de algoritmos, caberá a você determinar o melhor
uso dos recursos computacionais dado um problema em particular.

.. admonition:: Auto Avaliação

   .. mchoice:: analysis_1
       :answer_a: O(n)
       :answer_b: O(n^2)
       :answer_c: O(log n)
       :answer_d: O(n^3)
       :correct: b
       :feedback_a: Em um exemplo como este você deve contar o número de laços aninhados. Especialmente os laços que dependem da mesma variável, neste caso, n.
       :feedback_b: Um único laço aninhado como este é O(n^2).
       :feedback_c: log n tipicamente é indicado por um problema que é feito iterativamente menor.
       :feedback_d: Em um exemplo como este você deve contar o número de laços aninhados. Especialmente os laços que dependem da mesma variável, neste caso, n. 

       Dado o seguinte trecho de código, qual é o seu tempo de execução em Notação O?

       .. code-block:: python

         teste = 0
         for i in range(n):
            for j in range(n):
               teste = teste + i * j

   .. mchoice:: analysis_2
       :answer_a: O(n)
       :answer_b: O(n^2)
       :answer_c: O(log n)
       :answer_d: O(n^3)
       :correct: a
       :feedback_b: Cuidado, ao contar laços verifique que eles são aninhados.
       :feedback_d: Cuidado, ao contar laços verifique que eles são aninhados.
       :feedback_c: log n tipicamente é indicado por um problema que é feito iterativamente menor.
       :feedback_a: Apesar de haver dois laços, eles não são aninhados. Você pode entender este caso como O(2n), mas podemos ignorar a constante 2.

       Dado o seguinte trecho de código, qual é o seu tempo de execução em Notação O?

       .. code-block:: python

         teste = 0
         for i in range(n):
            teste = teste + 1

         for j in range(n):
            teste = teste - 1

   .. mchoice:: analysis_3
       :answer_a: O(n)
       :answer_b: O(n^2)
       :answer_c: O(log n)
       :answer_d: O(n^3)
       :correct: c
       :feedback_a: Olhe atentamente para a variável i do laço. Note que o valor de i é cortado pela metade cada a cada iteração. Esse é um grande indício de que a performance é melhor do que O(n).
       :feedback_b: Verifique novamente, esse é um laço aninhado?
       :feedback_d: Verifique novamente, esse é um laço aninhado?       
       :feedback_c: O valor de i é cortado pela metade a cada iteração do laço, então serão necessárias somente log n iterações.

       Dado o seguinte trecho de código, qual é o seu tempo de execução em Notação O?

       .. code-block:: python

         i = n
         while i > 0:
            k = 2 + 2
            i = i // 2
