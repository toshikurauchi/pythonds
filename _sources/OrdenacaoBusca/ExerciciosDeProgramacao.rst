..  Copyright (C)  Brad Miller, David Ranum
    This work is licensed under the Creative Commons Attribution-NonCommercial-ShareAlike 4.0 International License. To view a copy of this license, visit http://creativecommons.org/licenses/by-nc-sa/4.0/.


Exercícios de programação
-------------------------

#. Crie um experimento aleatório para testar a diferença entre a busca
   sequencial e binária em uma lista de inteiros.

#. Use as funções de busca binária do texto (as versões recursiva e
   iterativa). Gere aleatoriamente uma lista ordenada de inteiros e
   faça uma análise de desempenho para cada uma. Quais os resultados?
   Você pode explicá-los?

#. Implemente a busca binária usando recursão sem o operador de "slice".
   Lembre-se de que você irá precisar passar a lista junto com os índices
   de início e fim para as sublistas. Gere aleatoriamente uma lista ordenada
   de inteiros e faça uma análise de desempenho.

#. Implemente o método ``len`` (\_\_len\_\_) para a implementação da tabela
   de dispersão Map ADT.

#. Implemente o método ``in`` (\_\_contains\_\_) para a implementação da
   tabela de dispersão Map ADT.

#. Como você consengue eliminar itens de uma tabela de dispersão que usa
   encadeamento na resolução de colisões? E se o endereçamento aberto fosse
   usado? Quais as circunstâncias particulares que precisam ser levadas em
   conta? Implemente o método ``del`` para a classe ``HashTable``.

#. Na implementação da tabela de dispersão estilo mapa, o tamanaho escolhido
   para a tabela de dispersão foi 101. Se a tabela ficar cheia, esse valor
   precisa ser incrementado. Reimplemente o método ``put`` de modo que a
   tabela ajuste automaticamente de tamanho quando o fator de carga atingir
   um valor pré-determinado (você pode decidir qual valor baseado no seu
   conhecimento sobre carga versus desempenho).

#. Implemente a sondagem quadrática como uma técnica de rehash.

#. Usando um gerador de números aleatórios, crie uma lista com 500 inteiros.
   Faça uma análise de desempenho usando alguns dos algoritmos de ordenação
   deste capítulo. Qual é a diferença na velocidade de execução?

#. Implemente o bubble sort usando a atribuição simultânea.

#. O bubble sort pode ser modificado para "borbulhar" em ambas as direções.
   A primeira passagem move-se para "cima" da lista, enquanto a segunda
   move-se para "baixo". Esse padrão de alternância continua até que não
   haja mais passagens necessárias. Implemente essa variação e explique sob
   quais circunstâncias ela pode ser apropriada.

#. Implemente a ordenação por seleção usando a atribuição simultânea.

#. Faça uma análise de desempenho para o shell sort, usando diferentes
   conjuntos de incrementos em uma mesma lista.

#. Implemente a função ``mergeSort`` sem usar o operador "slice".

#. Uma forma de melhorar o quick sort é usar a ordenação por inserção em
   listas que têm um tamanho pequeno (podemos chamá-las de "limites de partição").
   Por que isso faz sentido? Reimplemente o quick sort e use-o para ordenar
   uma lista aleatória de inteiros. Faça uma análise usando diferentes
   tamanhos de lista para o limite de partição.

#. Implemente, como uma modificação do ``quickSort``, o método
   "mediana melhor-de-três" para selecionar o valor do pivô. Faça um experimento
   e compare as duas técnicas.
