..  Copyright (C)  Brad Miller, David Ranum
    This work is licensed under the Creative Commons Attribution-NonCommercial-ShareAlike 4.0 International License. To view a copy of this license, visit http://creativecommons.org/licenses/by-nc-sa/4.0/.


Ordenação
---------

Ordenação é o processo de colocar elementos de uma coleção em uma determinada
ordem. Por exemplo, uma lista de palavras poderia estar ordenada
alfabeticamente ou por comprimento. Uma lista de cidades poderia estar
ordenada por população, área ou CEP. Nós já vimos vários algoritmos
que se beneficiam de uma lista ordenada (lembre-se do último exemplo
do anagrama e da busca binária).

Existem muitos, muitos algoritmos de ordenação já desenvolvidos e analisados.
Isso sugere que a ordenação é uma área de estudo importante na ciência da
computação. Ordenar um grande número de itens pode demandar uma quantidade
substancial de recursos computacionais. Assim como a busca, a eficiência
de um algoritmo de ordenação está relacionada ao número de itens que estão
sendo processados. Para coleções pequenas, um método complexo de ordenação
pode dar muito trabalho e não valer a pena. Por outro lado, para coleções
grandes, queremos tirar vantagem do maior número possível de melhorias.
Nesta seção, nós discutiremos várias técnicas de ordenação e as compararemos
de acordo com o seu tempo de execução.

Mas antes de entrar nos algoritmos, devemos pensar antes nas operações
que podem ser usadas para analisar o processo de ordenação. Primeiro,
será necessário comparar dois valores para saber qual é menor (ou maior).
Para ordenar uma coleção, será necessário ter algum meio sistemático
de comparar valores para verificar se eles estão fora de ordem. O número
total de comparações será a maneira mais comum de medir um procedimento
de ordenação. Segundo, quando os valores não estiverem na posição correta
em relação aos demais, será necessário trocá-los. Essa troca é uma operação
custosa e o número total de trocas também será importante para avaliar a
eficiência total do algoritmo.
