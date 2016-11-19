..  Copyright (C)  Brad Miller, David Ranum
    This work is licensed under the Creative Commons Attribution-NonCommercial-ShareAlike 4.0 International License. To view a copy of this license, visit http://creativecommons.org/licenses/by-nc-sa/4.0/.


Hashing
~~~~~~~

Nas seções anteriores conseguimos uma melhora nos nossos algoritmos
de busca por meio da informação de como cada item de uma coleção
é armazenado em relação ao outro. Por exemplo, sabendo que uma lista
estava ordenada, nós pudemos realizar uma busca em tempo logarítmico
fazendo uma busca binária. Nesta seção tentaremos dar um passo além,
construindo uma estrutura de dados cuja busca tem tempo :math:`O(1)`.
Esse conceito é conhecido como **hashing**.

Para fazer isso, precisaremos saber mais sobre onde os itens poderão
estar quando procurarmos por eles na coleção. Se cada item está onde
deveria, então a busca pode ser feita usando uma única comparação para
descobrir sua presença. Iremos perceber, contudo, que isso não é o que
acontece na prática.

Uma **tabela de dispersão** (ou hash table) é uma coleção de itens que são
armazenados de maneira a serem encontrados com facilidade mais tarde. Cada
posição da tabela de dispersão, geralmente denominada **índice** (ou slot), pode
guardar um item e possui um rótulo inteiro começando a partir de 0. Por exemplo,
teremos um índice com rótulo 0, um índice com rótulo 1, outro com rótulo 2 e assim
por diante. Inicialmente, a tabela de dispersão não contém nenhum item, então
todos os índices estão vazios. Nós podemos implementar uma tabela de dispersão
usando uma lista com cada elemento inicializado pelo valor especial
``None`` do Python. A :ref:`Figura 4 <fig_hashtable1>` mostra uma tabela de
dispersão de tamanho :math:`m=11`. Em outras palavras, existem *m* índices na
tabela, rotulados de 0 a 10.

.. _fig_hashtable1:

.. figure:: Figures/hashtable.png
   :align: center

   Figura 4: Tabela de Dispersão com 11 Índices Vazios

O mapeamento entre a chave e o índice ao qual ela pertence na tabela é
conhecido como a **função de espalhamento** (ou hash function). A função de
espalhamento irá receber qualquer item na coleção e irá retornar um inteiro
dentro do intervalo dos índices, isto é, entre 0 e *m*-1. Suponha, por exemplo,
o conjunto de inteiros formado por 54, 26, 93, 17, 77 e 31. Nossa primeira
função de hash, às vezes chamada de "método do resto", simplesmente pega um item
e o divide pelo tamanho da tabela, retornando o resto da divisão e o seu valor
de espalhamento (:math:`h(item)=item \% 11`). A :ref:`Tabela 4 <tbl_hashvalues1>`
mostra todos os valores de espalhamento para os itens do nosso conjunto. Observe
que o método do resto (artimética modular) tipicamente estará presente de algum
modo em todas as funções de espalhamento, já que o resultado deve estar dentro do
intervalo dos índices.

.. _tbl_hashvalues1:

.. table:: **Tabela 4: Função Simples de Espalhamento Usando o Resto da Divisão**


    ================= ===========================
             **Item**   **Valor de Espalhamento**
    ================= ===========================
                   54                          10
                   26                           4
                   93                           5
                   17                           6
                   77                           0
                   31                           9
    ================= ===========================


Uma vez que os valores de espalhamento tenham sido computados, podemos inserir
cada item na tabela de dispersão na posição designada, como mostrado na
:ref:`Figura 5 <fig_hashtable2>`. Note que 6 dos 11 índices estão agora ocupados.
Essa razão é conhecida como **fator de carga** e é comumente denotada por
:math:`\lambda = \frac {numerodeitens}{tamanhodatabela}`. Para o nosso exemplo,
o fator de carga é :math:`\lambda = \frac {6}{11}`.

.. _fig_hashtable2:

.. figure:: Figures/hashtable2.png
   :align: center

   Figura 5: Tabela de Dispersão com Seis Itens

Agora, quando queremos procurar um item, simplesmente utilizamos a função de
espalhamento para calcular o índice correspondente da chave e acessamos a
tabela de dispersão para verificar se ela está presente. Essa operação de
busca é :math:`O(1)`, já que é necessário apenas um tempo constante para
computar o valor de espalhamento e acessar a posição apontada pelo índice
correspondente na tabela. Se tudo estiver onde deveria, encontramos um
algoritmo de busca de tempo constante.

Você provavelmente já deve ter notado que essa técnica só irá funcionar se cada
item for mapeado uma localização única na tabela de dispersão. Por exemplo,
se o item 44 fosse a próxima chave na nossa coleção, ele teria um valor de
espalhamento de 0 (:math:`44 \% 11 == 0`). Como 77 também possui 0 como valor
de espalhamento, teríamos um problema. Segundo a função de espalhamento,
dois ou mais itens deveriam estar no mesmo índice. Isso é chamado de **colisão**.
Claramente, colisões são um problema nesta técnica e iremos discuti-las mais
tarde.


Funções de Espalhamento
^^^^^^^^^^^^^^^^^^^^^^^

Dada uma coleção de itens, uma função de espalhamento capaz de mapear cada item
para um único índice é chamada de **função de espalhamento perfeita**. Se
soubermos que os itens e a coleção nunca irá mudar, então é possível construir
uma função de espalhamento perfeita (confira os exercícios para saber mais
sobre funções de espalhamento perfeitas). Infelizmente, dada uma coleção
arbitrária de itens, não existe uma maneira sistemática de contruir uma função
de espalhamento perfeita. Felizmente, não precisamos que a função de
espalhamento seja pefeita para ainda termos ganhos de desempenho.

Uma forma de sempre termos uma função de espalhamento perfeita é aumentar o
tamanho da tabela de dispersão, de modo que cada valor possível no intervalo
de itens possa ser acomodado. Isso garante que cada item terá um único índice.
Embora isso seja algo prático para um número pequeno de itens, tal estratégia
é inviável quando o número de itens é muito grande. Por exemplo, se os itens
fossem os números de nove dígitos do INSS, esse método iria requerer quase
um bilhão de índices. Se quiséssemos apenas armazenar os dados de uma sala de
aula com 25 estudantes, estaríamos desperdiçando uma quantidade enorme
de memória.

Nosso objetivo é criar uma função de espalhamento que minimize o número de
colisões, seja fácil de computar e distribua os itens uniformemente na
tabela de dispersão. Há algumas maneiras conhecidas de estender o método do
resto da divisão. Iremos considerar algumas delas agora.

O **método de folding** para construir funções de espalhamento começa
dividindo o item em pedaços de tamanhos iguais (o último pedaço pode não ser
de tamanho igual). Esses pedaços são somados para então gerar o valor de
espalhamento resultante. Por exemplo, se o nosso item fosse o número de
telefone 436-555-4601, iríamos extrair os dígitos e dividi-los em grupos de 2
(43, 65, 55, 46, 01). Depois de somar tudo, :math:`43+65+55+46+01`, teríamos
210. Se a nossa tabela de dispersão tiver 11 índices, então precisaríamos
realizar o passo adicional de dividir o resultado por 11 e pegar o resto
da divisão. Nesse caso, :math:`210\ \%\ 11` é 1, então o número de telefone
436-555-4601 seria mapeado para o índice 1. Alguns métodos de folding vão
além e trocam a ordem dos pedaços antes de somá-los. Para o exemplo acima,
teríamos algo como :math:`43+56+55+64+01 = 219`, o que resulta em
:math:`219\ \%\ 11 = 10`.

Outra técnica numérica para construir a função de espalhamento é conhecida como
**método do quadrado do meio**. Nela, elevamos primeiro o item ao quadrado e
depois extraímos uma parte dos dígitos resultantes. Por exemplo, se o item
fosse 44, primeiro calculamos :math:`44 ^{2} = 1.936`. Extraindo os dois
dígitos do meio, 93, e realizando o passo de pegar o resto da divisão, ficamos
com 5 (:math:`93\ \%\ 11`). A :ref:`Tabela 5 <tbl_hashvalues2>` mostra valores
de espalhamento calculados tanto para o método do resto da divisão quanto para
o do quadrado do meio. Verifique se você entendeu como esses valores foram
calculados.


.. _tbl_hashvalues2:

.. table:: **Tabela 5: Comparação dos Métodos de Resto e do Quadrado do Meio**


    ================= =============== ======================
             **Item**       **Resto**   **Quadrado do Meio**
    ================= =============== ======================
                   54              10                      3
                   26               4                      7
                   93               5                      9
                   17               6                      8
                   77               0                      4
                   31               9                      6
    ================= =============== ======================

Nós podemos criar também funções de espalhamento para itens baseados em
caracteres, como strings. A palavra "gato" pode ser entendida como uma sequência
de valores ordinais.

::

    >>> ord('g')
    103
    >>> ord('a')
    97
    >>> ord('t')
    116
    >>> ord('o')
    111

Podemos pegar esses quatro valores, somá-los e usar o método do resto da divisão
para extrair um valor de espalhamento (veja a :ref:`Figura 6 <fig_stringhash>`).
O :ref:`Código 1 <lst_hashfunction1>` mostra uma função chamada ``hash`` que
recebe uma string e o tamanho de uma tabela e retorna o valor de espalhamento
dentro do intervalo de 0 a ``tablesize``-1.


.. _fig_stringhash:

.. figure:: Figures/stringhash.png
   :align: center

   Figura 6: Hashing de uma String Usando Valores Ordinais


.. _lst_hashfunction1:

**Código 1**

::

    def hash(astring, tablesize):
        sum = 0
        for pos in range(len(astring)):
            sum = sum + ord(astring[pos])

        return sum%tablesize


É interessante notar que quando usamos essa função de espalhamento, anagramas
sempre terão o mesmo valor de dispersão. Para contornar isso, poderíamos usar
a posição do caractere como um peso. A :ref:`Figura 7 <fig_stringhash2>` mostrar
uma maneira possível de usar o valor posicional como um fator de peso. A
modificação na função de espalhamento é deixada como exercício.


.. _fig_stringhash2:

.. figure:: Figures/stringhash2.png
   :align: center

   Figura 7: Hashing de uma String Usando Valores Ordinais com Peso

Você pode pensar em diversas outras formas de computar valores de
espalhamento para os itens de uma coleção. O importante é lembrar que a
função de espalhamento precisa ser eficiente, de modo que ela não se torne
a parte dominante no processo de armazenamento e busca. Se a função de
espalhamento for muito complexa, então computar posições na tabela se torna
mais trabalhoso do que simplesmente realizar uma busca sequencial ou
binária, já abordadas anteriormente. Isso iria aniquilar o propósito
do hashing.


Resolução de Colisões
^^^^^^^^^^^^^^^^^^^^^

Retornamos agora ao problema das colisões. Quando dois itens são levados
à mesma posição pela função de espalhamento, precisamos ter uma forma
sistemática de colocar o segundo item na tabela de dispersão. Esse processo
é chamado de **resolução de colisões**. Como dito anteriormente, se a função
de espalhamento for perfeita, colisões nunca ocorrerão. Contudo, como isso
não é muito realista, a resolução de colisões acaba sendo uma parte
muito importante do hashing.

Um método para resolução de colisões olha para a tabela de dispersão e tenta
encontrar outra posição aberta que possa armazenar o item que causou a colisão.
Uma forma simples de fazer isso é começar pela posição do valor de espalhamento
original e mover de forma sequencial pelas entradas até encontrar a primeira
que esteja vazia. Note que poderemos ter que voltar para a primeira entrada
(circularmente) para cobrir a tabela de dispersão inteira. Esse processo de
resolução de colisões é conhecido como **endereçamento aberto**, já que ele
procura encontrar a próxima entrada aberta na tabela de dispersão. Ao visitar
sistematicamente uma posição por vez, estamos realizando uma técnica de
endereçamento aberto chamada **sondagem linear**.

A :ref:`Figura 8 <fig_linearprobing>` mostra o conjunto ampliado de inteiros
(54,26,93,17,77,31,44,55,20) submetidos à função de espalhamento simples
baseada no resto da divisão. A :ref:`Tabela 4 <tbl_hashvalues1>` acima mostra
os valores de espalhamento para os itens originais. A
:ref:`Figura 5 <fig_hashtable2>` mostra seu elementos originais. Quando
tentamos colocar o 44 na entrada 0, ocorre uma colisão. Usando a sondagem
linear, procuramos sequencialmente, entrada por entrada, até encontrarmos uma
posição aberta. Nesse caso, achamos o slot 1.

Novamente, o 55 deveria cair no slot 0, mas acaba sendo colocado no 2, já que
é a próxima entrada disponível. O valor final de 20 leva à posição 9. Como
ela já está ocupada, começamos a fazer a sondagem linear. Visitamos então
as entradas 10, 0, 1 e 2, até finalmente encontrarmos uma entrada vazia na
posição 3 da tabela.

.. _fig_linearprobing:

.. figure:: Figures/linearprobing1.png
   :align: center

   Figura 8: Resolução de Colisões com Sondagem Linear


Uma vez construída uma tabela de dispersão usando endereçamento aberto e
sondagem linear, é essencial que utilizemos os mesmos métodos para procurar
os itens. Suponha que estamos em busca do item 93. Quando computamos seu
valor de espalhamento, temos 5 como resultado. Ao procurar na posição 5,
encontramos o 93, então podemos retornar ``True``. Mas o que acontece se
procurarmos pelo 20? Nesse caso, o valor de espalhamento é 9 e o slot 9
está sendo ocupado pelo 31. Nós não podemos simplesmente retornar ``False``
porque sabemos que pode ter ocorrido colisões. Somos obrigados então a fazer
uma busca sequencial, começando pela posição 10 e procurando até encontrarmos
o item 20 ou uma entrada vazia.

Uma desvantagem da sondagem linear é a tendência para **aglutinação**, isto é,
os itens tendem a ficar agrupados na tabela. Isso significa que se ocorrerem
muitas colisões em um mesmo valor de espalhamento, as entradas vizinhas
ficarão ocupadas por causa da sondagem linear. Isso terá um impacto sobre os
outros itens que forem inseridos, como vimos quando tentamos adicionar o item
20 acima. Uma aglutinação de valores sendo levados a 0 teve que ser
vencida para que finalmente encontrássemos uma posição vazia. Essa aglutinação
é mostrada na :ref:`Figura 9 <fig_clustering>`.


.. _fig_clustering:

.. figure:: Figures/clustering.png
   :align: center

   Figura 9: Uma Aglutinação de Items para a Entrada 0

Uma forma de lidar com a aglutinação é estender a técnica de sondagem linear
para que em vez de procurar sequencialmente para a próxima entrada aberta,
alguns slots sejam pulados, distribuindo assim os itens que causaram colisão
de maneira mais uniforme. Isso potencialmente irá reduzir a aglutinação.
A :ref:`Figura 10 <fig_linearprobing2>` mostra como os itens ficam dispostos
quando uma resolução por colisão é feita com uma sondagem "mais 3". Isso
significa que uma vez ocorrida a colisão, iremos procurar de três em três
entradas até encontrar uma vazia.


.. _fig_linearprobing2:

.. figure:: Figures/linearprobing2.png
   :align: center

   Figura 10: Resolução de Colisão Usando o "Mais 3"


The general name for this process of looking for another slot after a
collision is **rehashing**. With simple linear probing, the rehash
function is :math:`newhashvalue = rehash(oldhashvalue)` where
:math:`rehash(pos) = (pos + 1) \% sizeoftable`. The “plus 3” rehash
can be defined as :math:`rehash(pos) = (pos+3) \% sizeoftable`. In
general, :math:`rehash(pos) = (pos + skip) \% sizeoftable`. It is
important to note that the size of the “skip” must be such that all the
slots in the table will eventually be visited. Otherwise, part of the
table will be unused. To ensure this, it is often suggested that the
table size be a prime number. This is the reason we have been using 11
in our examples.

A variation of the linear probing idea is called **quadratic probing**.
Instead of using a constant “skip” value, we use a rehash function that
increments the hash value by 1, 3, 5, 7, 9, and so on. This means that
if the first hash value is *h*, the successive values are :math:`h+1`,
:math:`h+4`, :math:`h+9`, :math:`h+16`, and so on. In other words,
quadratic probing uses a skip consisting of successive perfect squares.
:ref:`Figure 11 <fig_quadratic>` shows our example values after they are placed using
this technique.

.. _fig_quadratic:

.. figure:: Figures/quadratic.png
   :align: center

   Figure 11: Collision Resolution with Quadratic Probing


An alternative method for handling the collision problem is to allow
each slot to hold a reference to a collection (or chain) of items.
**Chaining** allows many items to exist at the same location in the hash
table. When collisions happen, the item is still placed in the proper
slot of the hash table. As more and more items hash to the same
location, the difficulty of searching for the item in the collection
increases. :ref:`Figure 12 <fig_chaining>` shows the items as they are added to a hash
table that uses chaining to resolve collisions.

.. _fig_chaining:

.. figure:: Figures/chaining.png
   :align: center

   Figure 12: Collision Resolution with Chaining


When we want to search for an item, we use the hash function to generate
the slot where it should reside. Since each slot holds a collection, we
use a searching technique to decide whether the item is present. The
advantage is that on the average there are likely to be many fewer items
in each slot, so the search is perhaps more efficient. We will look at
the analysis for hashing at the end of this section.

.. admonition:: Self Check

   .. mchoice:: HASH_1
      :correct: c
      :answer_a: 1, 10
      :answer_b: 13, 0
      :answer_c: 1, 0
      :answer_d: 2, 3
      :feedback_a:  Be careful to use modulo not integer division
      :feedback_b:  Don't divide by two, use the modulo operator.
      :feedback_c: 27 % 13 == 1 and 130 % 13 == 0
      :feedback_d: Use the modulo operator

      In a hash table of size 13 which index positions would the following two keys map to?  27,  130

   .. mchoice:: HASH_2
      :correct: b
      :answer_a: 100, __, __, 113, 114, 105, 116, 117, 97, 108, 99
      :answer_b: 99, 100, __, 113, 114, __, 116, 117, 105, 97, 108
      :answer_c: 100, 113, 117, 97, 14, 108, 116, 105, 99, __, __
      :answer_d: 117, 114, 108, 116, 105, 99, __, __, 97, 100, 113
      :feedback_a:  It looks like you may have been doing modulo 2 arithmentic.  You need to use the hash table size as the modulo value.
      :feedback_b:  Using modulo 11 arithmetic and linear probing gives these values
      :feedback_c: It looks like you are using modulo 10 arithmetic, use the table size.
      :feedback_d: Be careful to use modulo not integer division.

      Suppose you are given the following set of keys to insert into a hash table that holds exactly 11 values:  113 , 117 , 97 , 100 , 114 , 108 , 116 , 105 , 99 Which of the following best demonstrates the contents of the has table after all the keys have been inserted using linear probing?

Implementing the ``Map`` Abstract Data Type
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

One of the most useful Python collections is the dictionary. Recall that
a dictionary is an associative data type where you can store key–data
pairs. The key is used to look up the associated data value. We often
refer to this idea as a **map**.

The map abstract data type is defined as follows. The structure is an
unordered collection of associations between a key and a data value. The
keys in a map are all unique so that there is a one-to-one relationship
between a key and a value. The operations are given below.

-  ``Map()`` Create a new, empty map. It returns an empty map
   collection.

-  ``put(key,val)`` Add a new key-value pair to the map. If the key is
   already in the map then replace the old value with the new value.

-  ``get(key)`` Given a key, return the value stored in the map or
   ``None`` otherwise.

-  ``del`` Delete the key-value pair from the map using a statement of
   the form ``del map[key]``.

-  ``len()`` Return the number of key-value pairs stored in the map.

-  ``in`` Return ``True`` for a statement of the form ``key in map``, if
   the given key is in the map, ``False`` otherwise.

One of the great benefits of a dictionary is the fact that given a key,
we can look up the associated data value very quickly. In order to
provide this fast look up capability, we need an implementation that
supports an efficient search. We could use a list with sequential or
binary search but it would be even better to use a hash table as
described above since looking up an item in a hash table can approach
:math:`O(1)` performance.

In :ref:`Listing 2 <lst_hashtablecodeconstructor>` we use two lists to create a
``HashTable`` class that implements the Map abstract data type. One
list, called ``slots``, will hold the key items and a parallel list,
called ``data``, will hold the data values. When we look up a key, the
corresponding position in the data list will hold the associated data
value. We will treat the key list as a hash table using the ideas
presented earlier. Note that the initial size for the hash table has
been chosen to be 11. Although this is arbitrary, it is important that
the size be a prime number so that the collision resolution algorithm
can be as efficient as possible.

.. _lst_hashtablecodeconstructor:

**Listing 2**

::

    class HashTable:
        def __init__(self):
            self.size = 11
            self.slots = [None] * self.size
            self.data = [None] * self.size


``hashfunction`` implements the simple remainder method. The collision
resolution technique is linear probing with a “plus 1” rehash function.
The ``put`` function (see :ref:`Listing 3 <lst_hashtablecodestore>`) assumes that
there will eventually be an empty slot unless the key is already present
in the ``self.slots``. It computes the original hash value and if that
slot is not empty, iterates the ``rehash`` function until an empty slot
occurs. If a nonempty slot already contains the key, the old data value
is replaced with the new data value.  Dealing with the situation where there are
no empty slots left is an exercise.

.. _lst_hashtablecodestore:

**Listing 3**

::

    def put(self,key,data):
      hashvalue = self.hashfunction(key,len(self.slots))

      if self.slots[hashvalue] == None:
        self.slots[hashvalue] = key
        self.data[hashvalue] = data
      else:
        if self.slots[hashvalue] == key:
          self.data[hashvalue] = data  #replace
        else:
          nextslot = self.rehash(hashvalue,len(self.slots))
          while self.slots[nextslot] != None and \
                          self.slots[nextslot] != key:
            nextslot = self.rehash(nextslot,len(self.slots))

          if self.slots[nextslot] == None:
            self.slots[nextslot]=key
            self.data[nextslot]=data
          else:
            self.data[nextslot] = data #replace

    def hashfunction(self,key,size):
         return key%size

    def rehash(self,oldhash,size):
        return (oldhash+1)%size


Likewise, the ``get`` function (see :ref:`Listing 4 <lst_hashtablecodesearch>`)
begins by computing the initial hash value. If the value is not in the
initial slot, ``rehash`` is used to locate the next possible position.
Notice that line 15 guarantees that the search will terminate by
checking to make sure that we have not returned to the initial slot. If
that happens, we have exhausted all possible slots and the item must not
be present.

The final methods of the ``HashTable`` class provide additional
dictionary functionality. We overload the __getitem__ and
__setitem__ methods to allow access using``[]``. This means that
once a ``HashTable`` has been created, the familiar index operator will
be available. We leave the remaining methods as exercises.

.. _lst_hashtablecodesearch:

**Listing 4**

.. highlight:: python
    :linenothreshold: 5

::

    def get(self,key):
      startslot = self.hashfunction(key,len(self.slots))

      data = None
      stop = False
      found = False
      position = startslot
      while self.slots[position] != None and  \
                           not found and not stop:
         if self.slots[position] == key:
           found = True
           data = self.data[position]
         else:
           position=self.rehash(position,len(self.slots))
           if position == startslot:
               stop = True
      return data

    def __getitem__(self,key):
        return self.get(key)

    def __setitem__(self,key,data):
        self.put(key,data)



.. highlight:: python
    :linenothreshold: 500



The following session shows the ``HashTable`` class in action. First we
will create a hash table and store some items with integer keys and
string data values.

::

    >>> H=HashTable()
    >>> H[54]="cat"
    >>> H[26]="dog"
    >>> H[93]="lion"
    >>> H[17]="tiger"
    >>> H[77]="bird"
    >>> H[31]="cow"
    >>> H[44]="goat"
    >>> H[55]="pig"
    >>> H[20]="chicken"
    >>> H.slots
    [77, 44, 55, 20, 26, 93, 17, None, None, 31, 54]
    >>> H.data
    ['bird', 'goat', 'pig', 'chicken', 'dog', 'lion',
           'tiger', None, None, 'cow', 'cat']

Next we will access and modify some items in the hash table. Note that
the value for the key 20 is being replaced.

::

    >>> H[20]
    'chicken'
    >>> H[17]
    'tiger'
    >>> H[20]='duck'
    >>> H[20]
    'duck'
    >>> H.data
    ['bird', 'goat', 'pig', 'duck', 'dog', 'lion',
           'tiger', None, None, 'cow', 'cat']
    >> print(H[99])
    None


The complete hash table example can be found in ActiveCode 1.

.. activecode:: hashtablecomplete
   :caption: Complete Hash Table Example
   :hidecode:

   class HashTable:
       def __init__(self):
           self.size = 11
           self.slots = [None] * self.size
           self.data = [None] * self.size

       def put(self,key,data):
         hashvalue = self.hashfunction(key,len(self.slots))

         if self.slots[hashvalue] == None:
           self.slots[hashvalue] = key
           self.data[hashvalue] = data
         else:
           if self.slots[hashvalue] == key:
             self.data[hashvalue] = data  #replace
           else:
             nextslot = self.rehash(hashvalue,len(self.slots))
             while self.slots[nextslot] != None and \
                             self.slots[nextslot] != key:
               nextslot = self.rehash(nextslot,len(self.slots))

             if self.slots[nextslot] == None:
               self.slots[nextslot]=key
               self.data[nextslot]=data
             else:
               self.data[nextslot] = data #replace

       def hashfunction(self,key,size):
            return key%size

       def rehash(self,oldhash,size):
           return (oldhash+1)%size

       def get(self,key):
         startslot = self.hashfunction(key,len(self.slots))

         data = None
         stop = False
         found = False
         position = startslot
         while self.slots[position] != None and  \
                              not found and not stop:
            if self.slots[position] == key:
              found = True
              data = self.data[position]
            else:
              position=self.rehash(position,len(self.slots))
              if position == startslot:
                  stop = True
         return data

       def __getitem__(self,key):
           return self.get(key)

       def __setitem__(self,key,data):
           self.put(key,data)

   H=HashTable()
   H[54]="cat"
   H[26]="dog"
   H[93]="lion"
   H[17]="tiger"
   H[77]="bird"
   H[31]="cow"
   H[44]="goat"
   H[55]="pig"
   H[20]="chicken"
   print(H.slots)
   print(H.data)

   print(H[20])

   print(H[17])
   H[20]='duck'
   print(H[20])
   print(H[99])



Analysis of Hashing
^^^^^^^^^^^^^^^^^^^

We stated earlier that in the best case hashing would provide a
:math:`O(1)`, constant time search technique. However, due to
collisions, the number of comparisons is typically not so simple. Even
though a complete analysis of hashing is beyond the scope of this text,
we can state some well-known results that approximate the number of
comparisons necessary to search for an item.

The most important piece of information we need to analyze the use of a
hash table is the load factor, :math:`\lambda`. Conceptually, if
:math:`\lambda` is small, then there is a lower chance of collisions,
meaning that items are more likely to be in the slots where they belong.
If :math:`\lambda` is large, meaning that the table is filling up,
then there are more and more collisions. This means that collision
resolution is more difficult, requiring more comparisons to find an
empty slot. With chaining, increased collisions means an increased
number of items on each chain.

As before, we will have a result for both a successful and an
unsuccessful search. For a successful search using open addressing with
linear probing, the average number of comparisons is approximately
:math:`\frac{1}{2}\left(1+\frac{1}{1-\lambda}\right)` and an
unsuccessful search gives
:math:`\frac{1}{2}\left(1+\left(\frac{1}{1-\lambda}\right)^2\right)`
If we are using chaining, the average number of comparisons is
:math:`1 + \frac {\lambda}{2}` for the successful case, and simply
:math:`\lambda` comparisons if the search is unsuccessful.
