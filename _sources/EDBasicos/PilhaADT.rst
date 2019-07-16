..  Copyright (C)  Brad Miller, David Ranum
    This work is licensed under the Creative Commons Attribution-NonCommercial-ShareAlike 4.0 International License. To view a copy of this license, visit http://creativecommons.org/licenses/by-nc-sa/4.0/.


O Tipo Abstrato de Dados Pilha
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

O tipo abstrato de dados  pilha é definido pela seguinte estrutura e
operações. Uma pilha é estruturada, conforme descrito anteriormente,
como uma coleção ordenada de itens onde itens são inseridoos
e removidos da extremidade chamada de "topo".
As operações sobre pilha são apresentadas a seguir.

-  ``Stack()`` cria uma nova pilha vazia.
   Não necessita parâmetros e retorna uma pilha vazia.


-  ``push(item)`` insere um novo item na pilha.
   A operação necessita o item e não retorna coisa alguma.

-  ``pop()`` remove o item que está no topo da pilha.
   Não necessita parametros e retorna o item removido. A pilha é modificada.

-  ``peek()`` retorna o item no topo da pilha mas não o remove da pilha.
   Não necessita de parâmetros. A pilha não é modificada.

-  ``isEmpty()`` testa se a pilha está vazia. Não necessita parâmetros e retorna    um valor booleano; valor do tipo ``bool``, ``True`` or ``False``.

-  ``size()`` retorna o número de item na pilha.
   Não necessita parâmetros e retorna um inteiro; valor do tipo `int`.
   
Po exemplo, se  ``s`` é uma pilha que criada inicialmente vazia,
então :ref:`Table 1 <tbl_stackops>` mostra o resultado de uma sequência de
operações sobre ``s``. Em "conteúdo", o item no topo está listado mais a direita.

.. _tbl_stackops:

.. table:: **Tabela 1: Operações sobre pilha**

    ============================ ======================== ====================
             **Operações**       **Conteúdo**             **Valor retornado**
    ============================ ======================== ====================
                 ``s.isEmpty()``                   ``[]``           ``True``
                   ``s.push(4)``                  ``[4]``
               ``s.push('dog')``            ``[4,'dog']``
                    ``s.peek()``            ``[4,'dog']``          ``'dog'``
                ``s.push(True)``       ``[4,'dog',True]``
                    ``s.size()``       ``[4,'dog',True]``              ``3``
                 ``s.isEmpty()``       ``[4,'dog',True]``          ``False``
                 ``s.push(8.4)``   ``[4,'dog',True,8.4]``
                     ``s.pop()``       ``[4,'dog',True]``            ``8.4``
                     ``s.pop()``            ``[4,'dog']``           ``True``
                    ``s.size()``            ``[4,'dog']``              ``2``
    ============================ ======================== ==================


