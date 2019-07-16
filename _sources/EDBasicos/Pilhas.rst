..  Copyright (C)  Brad Miller, David Ranum
    This work is licensed under the Creative Commons Attribution-NonCommercial-ShareAlike 4.0 International License. To view a copy of this license, visit http://creativecommons.org/licenses/by-nc-sa/4.0/.


O que é uma Pilha?
~~~~~~~~~~~~~~~~~~

Uma **pilha** (=*stack*) é uma coleção ordenada de itens
onde a inserção de novos itens e a remoção de
os itens existentes sempre ocorrem na mesma extremidade.
Este extremidade é comumente chamada de **topo**.
A extremidade oposta ao topo é conhecida como **base**.

Os itens armazenados na pilha que estão mais perto da base são aqueles
que estão na pilha a mais tempo.
O último item inserido é aquele que está na posição para ser removido primeiro.
Este princípio de ordenação é chamado às vezes
**LIFO**, **último a entrar primeiro a sair** (=*last in first out*).
Os itens estão armazenados de acordo com o tempo de permanência da estrutura.
Itens recentemente inseridos na estrutura estão mais próximos do topo,
enquanto os itens mais antigos na estrutura estão perto da base.

Muitos exemplos de pilhas ocorrem em situações cotidianas.
Quase qualquer cafeteria tem uma pilha de bandejas ou pratos onde
você pega ao item que está mais em cima,
descobrindo uma nova bandeja ou prato para o próximo cliente na fila.
Imagine uma pilha de livros em uma mesa (:ref:`Figure 1 <fig_bookstack>`).
O único livro cuja capa é visível é o que está no topo.
Para acessar algum livro na pilha,
precisamos remover os que estão sobre ele.
:ref:`Figure 2 <fig_objectstack>` mostra outra pilha que
contém alguns objetos nativos de Python.

.. _fig_bookstack:

.. figure:: Figures/bookstack2.png
   :align: center
   :scale: 50 %

   Figure 1: Uma pilha de livros

.. _fig_objectstack:

.. figure:: Figures/primitive.png
   :align: center
   :scale: 50 %

   Figure 2: Uma pilha de objetos de Python

Uma das idéias mais úteis relacionadas às pilhas vem da simples
observação de itens à medida que são inseridos e depois removidos.
Suponha que você começa com uma mesa vazia.
Agora, coloque um livro de cada vez na mesa, um sobre o outro.
Você está construindo uma pilha.
Considere o que acontece quando você começar a remover livros.
A ordem que eles são removidos é exatamente a inversa da ordem em que
foram colocados. As pilhas são fundamentalmente importantes,
pois elas podem ser usadas para reverter a ordem dos itens.
A ordem de inserção é a inversa da ordem de remoção.
:ref:`Figura 3 <fig_reversal>` mostra a pilha de objetos de Python
como ela foi criada e, novamente, como os itens são removidos.
Preste atenção a ordem do objetos.
 
.. _fig_reversal:

.. figure:: Figures/simplereversal.png
   :align: center

   Figure 3: The Reversal Property of Stacks


Considerando essa propriedade de reversão,
talvez você possa pensar em exemplos de
pilhas que ocorrem quando você usa seu computador.
Por exemplo, toda  navegador tem um botão *Voltar*.
À medida que você navega de uma página para outra página,
essas páginas são colocadas em uma pilha (na verdade,
são as URLs que vão para a pilha).
A página atual que você está visualizando está no topo
e a primeira página que você olhou está na base. Se você clicar no
botão *Voltar*, você começa a se mover em ordem inversa pelas páginas.

