+++
date = '2025-08-12T00:33:44-03:00'
title = 'Uma sintaxe modernista'
toc = true
draft = false
tags = ['programming languages']
+++

A sintaxe é a interface de usuário de uma linguagem de programação.

Gosto muito da sintaxe do Python.
Tem um estilo modernista: linhas retas, poucos adornos.
Outras linguagens são cheias de `$`, `&`, `->`, `{...}`, `;`, `@`.

Python já foi mais simples.
O símbolo `@` não fazia parte da sintaxe até o Python 2.4,
quando apareceu a ideia do `@decorator`, e agora também serve para multiplicar matrizes
no NumPy: `a @ b` (ou qualquer classe que implemente `__matmul__`).

A sintaxe de Rust me assusta um pouco: `struct Foo<'a> { bar: &'a Bar }`.
C++ é uma catedral gótica, [Raku](https://raku.org/) é barroca,
Lisp é modernista raiz Bauhaus, a sintaxe mais simples possível,
`(f x y)`, tão simples que chega a ser incômoda.
Como uma casa modernista feita só de retângulos de concreto e vidro:
frígida no inverno, tórrida no verão.

Veja a definição de uma função para calcular o MDC (Máximo Divisor Comum)
em Scheme, dialeto de Lisp com a mesmo tipo de sintaxe
(conhecida como _S-expression_):

```scheme
(define (mdc m n)
    (if (= n 0)
        m
        (mdc n (modulo m n))))
```

Para chamar essa função:

```scheme
(mdc 18 45)
```

Em Python, esse algoritmo de MDC recursivo fica assim:[^1]

```python
def mdc(m, n):
    if n == 0:
        return m
    else:
        return mdc(n, m % n)
```

E a chamada:

```python
mdc(18, 45)
```

[^1]: A solução funciona mas consome muita memória em Python,
se os argumentos forem inteiros grandes.
Scheme tem recursão de cauda otimizada:
como a chamada recursiva está no fim do corpo da função,
o interpretador salta para a próxima iteração reaproveitando
o frame atual, sem criar novo frame no stack.

Assim como gosto que Python não usa `;` no final das instrções,
também gosto que em Scheme não há `,` separando os argumentos,
só espaços.

O que me incomoda mais no Scheme não é o excesso de parentesis,
mas o fato de que eles delimitam construções muito diferentes:
instuções como `(define ...)` ou `(if ...)`, chamadas de função
`(modulo m n)`, e até listas `(0 1 2 3)`.

Em algum código legado você pode encontrar isso:

```scheme
(concurrent (fetch urls) (file out))
```

Será que `concurrent` é uma função ou uma instrução especial?
Se for uma função, você sabe que `(fetch urls)` e `(file out)`
serão invocadas antes e seus resultados serão passados para `concurrent`.

Mas se `concurrent` for uma _forma especial_[^2] ou uma _macro_ em Scheme,
daí tudo é possível.

[^2]: algo que teria uma sintaxe especial com uma palavra reservada,
como `with` ou `await` em Python.

Exemplo de "tudo é possível": o `if` do Scheme pode até parecer uma chamada
de função `(if a (b) (c))`, mas o que acontece é que o valor de `a` determina
se `(b)` ou `(c)` é invocada, nunca as duas.

Agora, se o `if` fosse uma função, poderia ser o fim da humanidade.

Considere esta linha de código num sistema de lançamento de mísseis nucleares:

```scheme
(if (checar senha) (lançar mísseis) (desativar mísseis))
```

Se o `if` fosse uma função, as três expressões seriam avaliadas
para então serem passadas para o `if`.
A senha seria checada, os mísseis seriam lançados, e também desativados,
independente do resultado de `(checar senha)`.
Toda vez seria assim.
A função `if` seria invocada só depois de executadas essas três chamadas,
recebendo seus três valores como argumentos.

O problema é que a sintaxe `(select y z)` pode ser uma chamada de função,
a aplicação de uma instrução especial `select`, ou até uma
lista contendo três identificadores, dependendo do contexto.

Em Python a sintaxe de uma instrução condicional é
bem diferente de chamada de função: `if senha: salvar(senha)`.

Na sintaxe de S-expression, isso seria `(if senha (salvar senha))`.

Como eu disse, Lisp tem uma sintaxe simples demais,
tão simples que não há diferença visual entre instruções especiais,
chamadas de função, e listas.

Se eu fosse inventar uma linguagem, buscaria uma sintaxe mais enxuta que Python,
mas um pouco mais enfeitada que Scheme,
com mais sinais gráficos para ajudar na leitura.

Vamos chamar de sintaxe L. O exemplo dos mísseis ficaria assim:

```
if {validar(senha) lançar(mísseis) desativar(mísseis)}
```

A sintaxe geral é `ident「 . . . 」`
onde `ident` é um identificador, seguido de zero ou mais expressões
entre delimitadores que podem ser:

`()` delimitando os argumentos em uma chamada de função;

`{}` delimitando o corpo de uma instrução especial;

`[]` delimitando uma sequência de itens de dados (estou sendo vago de propósito, logo mais explico).

Voltando a esse exemplo de S-expression misteriosa:

```scheme
(concurrent (fetch urls) (file out))
```

Na sintaxe L, poderia ser:

```
concurrent(fetch(urls) file(out))
```

Isso seria uma chamada de função `concurrent` com os argumentos `fetch(urls)` e `file(out)`.

Outra semântica teria outra sintaxe:

```
concurrent{fetch(urls) file(out)}
```

Nesse caso `concurrent` seria uma forma especial, com uma semântica diferente
no tratamento de `fetch(urls)` e `file(out)`.
Por exemplo (inventando agora) o comportamento poderia ser:
`fetch` seria invocada de modo concorrente em
diferentes threads, uma cada para cada item de `urls` (que poderia ser uma lista),
e `file(out)` seria invocada no final para armazenar os resultados.

Uma terceria variação seria:

```
concurrent{fetch(urls) [file out]}
```

Nesse caso, `[file out]` seria uma lista com dois identificadores dentro,
equivalente a `[file, out]` em Python.

Mas antes eu falei que a sintaxe geral é `ident「 . . . 」`,
então em vez de usar apenas `[]`, a sintaxe ganha muita flexibilidade
com um identificador prefixo, que funciona como um 
[sigil](https://en.wikipedia.org/wiki/Sigil_(computer_programming)).
Por exemplo, `v[...]` é um vetor (uma lista unidimensional),
`c[...]` é um conjunto, `t[...]` é uma tupla.

Uma tupla é uma construção tão útil que merece um atalho, então
`t[1 2 3]` pode ser escrita sem prefixo: `[1 2 3]`.[^3]

[^3]: Essa ideia surgiu quando escrevi alguns parágrafos depois,
pensando na tupla de identificadores dos parâmetros de uma função.

E um dicionário?

Um dicionário na sintaxe de Python:

```
{a:1, b:2, c:3}
```

Poderia ser assim na sintaxe L:

```
d[a:1 b:2 c:3]
```

Isso me forçou a reservar outro símbolo, `:` mas por enquanto temos
apenas `(){}[]:` então está bom.

Para atribuição, podemos usar `=` com a sintaxe de instrução especial:

```
={pi 3.1416}
```

Outra atribuinção em Sintaxe L:

```
={amostra [10 20 30]}
```

Python[^4]:

[^4]: Esse é um atalho válido para escrever uma tupla em Python:
itens separados por vírgula, sem `()` delimitando.
Funciona em alguns contextos.

```python
amostra = 10, 20, 30
```

Assim como em Scheme, na minha linguagem `=` é um identificador válido.
O mesmo identificador serve para o operador de comparação,
com os delimitadores de uma chamada de função:

A expressão L `=(x y)` equivale a `x == y` em Python

Note que acabei de descartar a necessidade de uma notação infixa
especial só para operações aritméticas.

O exemplo MDC na sintaxe L:

```
def{mdc [m n]
    if{ =(n 0)
        m
        mdc(n modulo(m n))
    }
}
```

Pensei em escrever `%(m n)` em vez de `modulo(m n)`,
mas achei melhor reservar o `%` para alguma outra ideia,
já que não usamos tanto assim a operação módulo
(no caso, o resto da divisão `m / n`, escrito como `m % n` em Python).

Vamos ver agora outra forma de computar MDC,
com laço em vez de recursão.

Python idiomático:

```python

def mdc(m, n):
    while n != 0:
        m, n = n, m % n
    return m
```

Python sem usar atribuição paralela de tuplas:

```python
def mdc(m, n):
    while n != 0:
        temp = n
        n = m % n
        m = temp
    return m
```

Sintaxe L:

```
def{mdc [m n]
    while{ <>(n 0)
        ={temp n}
        ={n modulo(m n)}
        ={m temp}
    }
    m
}
```

Para o operador _diferente de_, escolhi `<>` (como em Pascal)
para reservar o `!` para algum uso posterior.
Também acho mais bonito `<>` do que `!=`.

A versão com atribuição paralela de tupla pode ser assim:

```
def{mdc [m n]
    while{ <>(n 0)
        ={[m n] [n modulo(m n)]}
    }
    m
}
```

Isso é viável porque `={...}` denota uma instrução especial,
então temos a liberdade de implementar variações sintáticas dentro dela.

and:

&{...}  # short circuit
&(...)  # bitwise



Por hoje, é só.

Amanhã escrevo outro post com mais exemplos da sintaxe L.

> Fiado só amanhã.<br>
> —*aviso comum em botecos no Brasil*
