..  Copyright (C)  Brad Miller, David Ranum
    This work is licensed under the Creative Commons Attribution-NonCommercial-ShareAlike 4.0 International License. To view a copy of this license, visit http://creativecommons.org/licenses/by-nc-sa/4.0/.


Dicionários
~~~~~~~~~~~

O dicionário é outra das estruturas de dados principais do Python. Como você
provavelmente se lembra, a diferença entre dicionários e listas é que você
pode acessar itens em um dicionário através de uma chave ao invés de uma posição. 
Em capítulos futuros neste livro você verá que há diversas maneiras de implementar
um dicionário. O mais importante agora é perceber que os operadores de acesso e 
atribuição a um item de um dicionário são :math:`O(1)`. Outro operador importante
em dicionários é o operador contém (*in*). Verificar se uma chave está no
dicionário ou não também é :math:`O(1)`. A eficiência de todas as operações de
dicionário está resumida em :ref:`Table 3 <tbl_dictbigo>`. É importante notar que
as eficiências apresentadas na tabela são performances médias. Em alguns casos
raros as operações contém, acessar e atribuir item se tornam :math:`O(n)`, mas
discutiremos isso em capítulos futuros quando falamos sobre as diferentes maneiras
como um dicionário poderia ser implementado.

.. _tbl_dictbigo:

.. table:: **Tabela 3: Eficiência em Notação O das Operações de Dicionário em Python**

    ======================= =======================
                   operador Eficiência em Notação O
    ======================= =======================
            copiar (*copy*)                    O(n)
               acessar item                    O(1)
              atribuir item                    O(1)
               remover item                    O(1)
              contém (*in*)                    O(1)
                   iteração                    O(n)
    ======================= =======================



Para o nosso último experimento vamos comparar a performance do
operador contém entre listas e dicionários. Durante o processo vamos confirmar
que o operador contém para listas é :math:`O(n)` e o operador contém para 
dicionários é :math:`O(1)`. O experimento que realizaremos para comparar as
duas operações é simples. Vamos criar uma lista com uma determinada quantidade
de números. Então vamos escolher números aleatórios e verificar se esses
números estão na lista. Se nossa tabela de performances está correta,
quanto maior a lista, mais tempo deve ser necessário para determinar se qualquer
número está contido nela.

Vamos repetir o mesmo experimento para um dicionário que contém números como 
chaves. Nesse experimento deveríamos ver que determinar se um número está ou
não no dicionário é, não somente muito mais rápido, mas também o tempo 
necessário para a verificação deve permanecer constante mesmo se o dicionário
aumentar em tamanho.

:ref:`Listing 6 <lst_listvdict>` implementa essa comparação. Note que estamos
realizando exatamente a mesma operação, ``número in x``. A diferença
é que na linha 7 ``x`` é uma lista e na linha 9 ``x`` é um dicionário.

.. _lst_listvdict:

**Listing 6**


.. sourcecode:: python
    :linenos:

    import timeit
    import random

    for i in range(10000,1000001,20000):
        t = timeit.Timer("random.randrange(%d) in x"%i,
                         "from __main__ import random,x")
        x = list(range(i))
        tempo_lista = t.timeit(number=1000)
        x = {j:None for j in range(i)}
        tempo_dic = t.timeit(number=1000)
        print("%d,%10.3f,%10.3f" % (i, tempo_lista, tempo_dic))
        
        


A :ref:`Figura 4 <fig_listvdict>` resume os resultados da execução de
:ref:`Listing 6 <lst_listvdict>`. Você pode ver que o dicionário é consistentemente
mais rápido. Para a menor lista, com 10.000 elementos, o dicionário é
89,4 vezes mais rápido do que a lista. Para o maior tamanho, de 990.000 elementos,
o dicionário é 11.603 vezes mais rápido! Você também pode ver que o
tempo necessário para o operador contém na lista cresce linearmente 
com o tamanho da lista. Isso verifica a afirmação de que o operador contém
em uma lista é :math:`O(n)`. Também é possível ver que o tempo para
o operador contém no dicionário é constante, mesmo conforme o dicionário
cresce. De fato para um dicionário de tamanho 10.000 o operador contém
levou 0.004 milissegundos e para o dicionário de tamanho 990.000 ele também
levou 0.004 milissegundos.

.. _fig_listvdict:

.. figure:: Figures/listvdict.png

    Figura 4: Comparando o Operador contém (``in``) para Listas e Dicionários em Python

Como Python é uma linguagem em evolução, sempre há mudanças ocorrendo por
trás dos panos. A informação atualizada sobre a performance de estruturas de
dados do Python pode ser encontrada na página do Python. No momento da
escrita deste texto a wiki do Python possui uma página bastante interessante
sobre a complexidade de tempo, que pode ser encontrada em
`Time Complexity Wiki <http://wiki.python.org/moin/TimeComplexity>`_.



.. admonition:: Auto Avaliação

    .. mchoice:: mcpyperform
       :answer_a: list.pop(0)
       :answer_b: list.pop()
       :answer_c: list.append()
       :answer_d: list[10]
       :answer_e: todas as opções são O(1)
       :correct: a
       :feedback_a: Quando removemos o primeiro elemento de uma lista, todos os outros elementos da lista precisam ser movidos para a esquerda.
       :feedback_b: Remover um elemento do final de uma lista é uma operação constante.
       :feedback_c: Adicionar um elemento ao final de uma lista é uma operação constante.
       :feedback_d: Acessar um elemento em uma determinada posição de uma lista é uma operação constante.
       :feedback_e: Há uma operação que necessita que todos os outros elementos da lista sejam movidos.

       Qual das operações sobre listas mostradas a seguir não é O(1)?

    .. mchoice:: mcpydictperf
      :answer_a: 'x' in mydict
      :answer_b: del mydict['x']
      :answer_c: mydict['x'] == 10
      :answer_d: mydict['x'] = mydict['x'] + 1
      :answer_e: todas as opções são O(1)
      :correct: e
      :feedback_a: contém (``in``) é uma operação constante para dicionários, pois você não precisa iterar, mas há uma resposta melhor.
      :feedback_b: remover um elemento de um dicionário é uma operação constante, mas há uma resposta melhor.
      :feedback_c: A atribuição a uma chave de um dicionário é uma operação constante, mas há uma resposta melhor.
      :feedback_d: Re-atribuição a uma chave de um dicionário é uma operação constante, mas há uma resposta melhor.
      :feedback_e: As únicas operações sobre dicionários que não são O(1) são aquelas que necessitam de iteração.

      Qual das operações sobre dicionários mostradas a seguir não é O(1)?

.. video::  pythonopsperf
   :controls:
   :thumb: ../_static/function_intro.png

   http://media.interactivepython.org/pythondsVideos/pythonops.mov
   http://media.interactivepython.org/pythondsVideos/pythonops.webm
