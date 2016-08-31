..  Copyright (C)  Brad Miller, David Ranum
    This work is licensed under the Creative Commons Attribution-NonCommercial-ShareAlike 4.0 International License. To view a copy of this license, visit http://creativecommons.org/licenses/by-nc-sa/4.0/.

O Que é Análise de Algoritmos?
------------------------------

É muito comum estudantes de ciência da computação compararem seus programas
com os colegas, principalmente durante o início do curso. Você também deve 
ter notado que é comum que os programas de computador sejam muito parecidos, 
especialmente os mais simples. Uma questão interessante sempre surge. Quando 
dois programas resolvem o mesmo problema, mas parecem diferentes, será que um 
é melhor do que o outro?

Para responder a essa questão, precisamos lembrar que existe uma diferença
importante entre um programa e o algoritmo que este programa está 
representando. Como dissemos no Capítulo 1, um algoritmo é uma lista passo
a passo genérica de instruções para resolver um problema. É um método para
resolver qualquer instância do problema tal que, dada uma entrada particular,
o algoritmo produz o resultado desejado. Um programa, por sua vez, é um
algoritmo que foi codificado em alguma linguagem de programação. Pode haver
diversos programas para o mesmo algoritmo, dependendo do programador e da
linguagem de programação utilizada.

Para explorar essa diferença mais a fundo, considere a função mostrada
em :ref:`ActiveCode 1 <lst_sum1>`. Essa função resolve um problema familiar,
calcular a some dos *n* primeiros inteiros. O algoritmo usa a ideia de
uma variável acumuladora que é inicializada com 0. A solução então
itera pelos *n* inteiros, adicionando cada um ao acumulador.

.. _lst_sum1:

.. activecode:: active1
    :caption: Soma dos n Primeiros Inteiros

    def somaDeN(n):
       aSoma = 0
       for i in range(1,n+1):
           aSoma = aSoma + i

       return aSoma

    print(somaDeN(10))

Agora veja a função em :ref:`ActiveCode 2 <lst_sum2>`. À primeira vista ela
pode parecer estranha, mas após uma inspeção mais cautelosa você pode ver
que essa função está fazendo essencialmente a mesma coisa que a anterior. A
razão pela qual isso não é óbvio é a má codificação. Nós não utilizamos bons
nomes de variáveis para auxiliar a legibilidade e usamos uma atribuição 
extra durante o passo de acumulação que não era necessária.

.. _lst_sum2:

.. activecode:: active2
    :caption: Outra Soma dos n Primeiros Inteiros

    def foo(tom):
        fred = 0
        for bill in range(1,tom+1):
           barney = bill
           fred = fred + barney

        return fred

    print(foo(10))

Anteriormente nos questionamos se uma função é melhor do que a outra.
A resposta depende dos seus critérios. A função ``somaDeN`` é certamente
melhor do que a função ``foo`` se você está preocupado com a legibilidade.
Você já deve ter visto diversos exemplos disso em seu curso introdutório
de programação, já que um dos objetivos nestes cursos é te ajudar a 
escrever programas que são fáceis de ler e fáceis de entender. Neste 
curso, entretanto, também estamos interessados em caracterizar o 
próprio algoritmo. (Nós certamente esperamos que você continuará se
empenhando em escrever código legível e fácil de entender).

A análise de algoritmos se preocupa com a comparação de algoritmos
baseada na quantidade de recursos computacionais que cada algoritmo
usa. Queremos ser capazes de considerar dois algoritmos e dizer que
um é melhor que o outro porque é mais eficiente no uso desses recursos
ou talvez porque ele simplesmente os usa menos. Por essa perspectiva,
as duas funções acima parecem bastante similares. Ambas usam 
essencialmente o mesmo algoritmo para resolver o problema da soma.

Neste ponto é importante pensarmos mais sobre o que realmente queremos
dizer com recursos computacionais. Existem duas maneiras de olhar para
esta questão. Uma maneira é considerar a quantidade de espaço ou memória
que um algoritmo necessita para resolver o problema. A quantidade de 
espaço necessária para a resolução de um problema é tipicamente ditada
pela instância do problema. De vez em quando, entretanto, há algoritmos
que possuem requisitos de espaço bastante específicos e nesses casos
teremos que ser muito cuidadosos ao explicar as variações.

Como uma alternativa aos requisitos de espaço, podemos analisar e
comparar algoritmos baseado na quantidade de tempo que necessitam para
ser executados. Nos referimos a essa medida como "tempo de execução"
do algoritmo. Uma forma de medirmos o tempo de execução da função
``somaDeN`` é realizar uma análise de aferição. Isso significa que vamos
contar o tempo necessário para o programa calcular o resultado. Em 
Python, podemos aferir uma função anotando o tempo de início e fim com
relação ao sistema que estamos usando. No módulo ``time`` há uma função
chamada ``time`` que devolve o tempo atual do sistema em segundos desde
algum ponto inicial arbitrário. Ao chamar esta função duas vezes, uma 
no início e outra no fim, e então calculando a diferença, podemos 
obter o número exato de segundos (frações na maioria dos casos) de
execução.

.. _lst_sum11:

**Listing 1**

.. sourcecode:: python

    import time

    def somaDeN2(n):
       inicio = time.time()

       aSoma = 0
       for i in range(1,n+1):
          aSoma = aSoma + i

       fim = time.time()

       return aSoma,fim-inicio

:ref:`Listing 1 <lst_sum11>` mostra a função original ``somaDeN`` com
as chamadas de cronômetro inseridas antes e depois do somatório. A 
função devolve uma tupla contendo o resultado e a quantidade de tempo
(em segundos) necessária para o cálculo. Se realizarmos 5 invocações
da função, cada vez calculando a soma dos primeiros 10.000 inteiros,
obtemos o seguinte:




::

    >>>for i in range(5):
           print("A soma é %d demorou %10.7f segundos"%somaDeN2(10000))
    A soma é 50005000 demorou  0.0018950 segundos
    A soma é 50005000 demorou  0.0018620 segundos
    A soma é 50005000 demorou  0.0019171 segundos
    A soma é 50005000 demorou  0.0019162 segundos
    A soma é 50005000 demorou  0.0019360 segundos

Descobrimos que o tempo é razoavelmente consistente e que o código demora
em média 0.0019 segundos para ser executado. E se executarmos a função
somando os primeiros 100.000 inteiros?

::

    >>>for i in range(5):
           print("A soma é %d demorou %10.7f segundos"%somaDeN2(100000))
    A soma é 5000050000 demorou  0.0199420 segundos
    A soma é 5000050000 demorou  0.0180972 segundos
    A soma é 5000050000 demorou  0.0194821 segundos
    A soma é 5000050000 demorou  0.0178988 segundos
    A soma é 5000050000 demorou  0.0188949 segundos
    >>>

Mais uma vez, o tempo necessário para cada execução, embora mais longo,
é bastante consistente, durando em média 10 vezes mais segundos. Para
``n`` igual a 1.000.000 obtemos:

::

    >>>for i in range(5):
           print("A soma é %d demorou %10.7f segundos"%somaDeN2(1000000))
    A soma é 500000500000 demorou  0.1948988 segundos
    A soma é 500000500000 demorou  0.1850290 segundos
    A soma é 500000500000 demorou  0.1809771 segundos
    A soma é 500000500000 demorou  0.1729250 segundos
    A soma é 500000500000 demorou  0.1646299 segundos
    >>>

Neste caso, a média foi mais uma vez aproximadamente 10 vezes a anterior.

Agora considere :ref:`ActiveCode 3 <lst_sum3>`, que mostra outra forma
de resolver o problema da soma. Essa função, ``somaDeN3``, se aproveita
de uma equação fechada :math:`\sum_{i=1}^{n} i = \frac {(n)(n+1)}{2}` 
para calcular a soma dos ``n`` primeiros inteiros sem iterar.

.. _lst_sum3:

.. activecode:: active3
    :caption: Somatório Sem Iteração

    def somaDeN3(n):
       return (n*(n+1))/2

    print(somaDeN3(10))

Se realizarmos a mesma medida de aferição para ``somaDeN3``, usando 
cinco valores diferentes de ``n`` (10.000, 100.000, 1.000.000, 
10.000.000 e 100.000.000), obtemos os seguintes resultados:

::

    A soma é 50005000 demorou 0.00000095 segundos
    A soma é 5000050000 demorou 0.00000191 segundos
    A soma é 500000500000 demorou 0.00000095 segundos
    A soma é 50000005000000 demorou 0.00000095 segundos
    A soma é 5000000050000000 demorou 0.00000119 segundos

Há duas coisas importantes para notar neste resultado. Primeiro, os
tempos obtidos acima são mais curtos do que qualquer um dos exemplos
anteriores. Segundo, eles são bastante consistentes, independente do
valor de ``n``. Parece que ``somaDeN3`` mal é impactada pelo número 
de inteiros sendo somados.

Mas o que esta aferição realmente nos diz? Intuitivamente, podemos 
ver que as soluções iterativas parecem fazer mais trabalho, uma vez
que alguns passos do programa são repetidos. Essa é provavelmente
a razão pela qual demoram mais. Além disso, o tempo necessário para
a solução iterativa parece aumentar conforme aumentamos o valor de
``n``. Entretanto, há um problema. Se executarmos a mesma função em
computadores diferentes ou usarmos uma linguagem de programação
diferente, provavelmente obteríamos resultados diferentes. A função
``somaDeN3`` poderia demorar mais se o computador fosse mais antigo.

Precisamos de uma forma melhor de caracterizar esses algoritmos com
relação ao tempo de execução. A técnica de aferição calcula o tempo
real de execução. Ela não nos fornece uma medida necessariamente
útil, pois ela depende de uma máquina, programa, hora do dia, compilador
e linguagem de programação particulares. Ao invés disso, gostaríamos
de ter uma caracterização mais independente do programa ou computador
sendo utilizados. Essa medida seria então útil para julgar o algoritmo
por si só (sem comparação) e poderia ser usada para comparar algoritmos
com diferentes implementações.
