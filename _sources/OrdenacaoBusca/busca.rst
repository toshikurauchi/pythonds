..  Copyright (C)  Brad Miller, David Ranum
    This work is licensed under the Creative Commons Attribution-NonCommercial-ShareAlike 4.0 International License. To view a copy of this license, visit http://creativecommons.org/licenses/by-nc-sa/4.0/.


Busca
-----

Vamos dirigir nossa atenção para alguns dos problemas mais recorrentes
em computação: busca e ordenação. Nesta seção, iremos estudar o problema
da busca. Mais tarde no capítulo voltaremos ao assunto da ordenação.
Busca é o processo algorítmico de encontrar um item específico numa
coleção de itens. Uma busca tipicamente devolve ``True`` ou ``False``,
indicando se o item está presente ou não. Ela também pode ser modificada
em algumas situações para retornar o elemento encontrado. Para os nossos
propósitos, iremos apenas nos preocupar com a questão da pertinência do item.

Em Python, existe um jeito fácil de verificar se um item está presente
numa lista de itens. Basta usar o operador ``in``.

::

    >>> 15 in [3,5,2,4,1]
    False
    >>> 3 in [3,5,2,4,1]
    True
    >>>

Embora isso seja fácil de escrever, um processo subjacente precisa ocorrer
para responder essa requisição. O fato é que existem muitas formas diferentes
de buscar um item. Estamos interessados aqui em como esses algoritmos
funcionam e como eles se portam comparativamente.
