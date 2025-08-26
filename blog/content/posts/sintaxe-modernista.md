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
Outras linguagens são cheias de `$`, `&`, `->`, `{…}`, `;`, `@`.

Python já foi mais simples.
O símbolo `@` não fazia parte da sintaxe até o Python 2.4,
quando apareceu a ideia do `@decorator`, e agora também serve para multiplicar matrizes
no NumPy: `a @ b` (ou qualquer classe que implemente `__matmul__`).

A sintaxe de Rust me assusta um pouco: `struct Foo<'a> { bar: &'a Bar }`.
C++ é uma catedral gótica, [Raku](https://raku.org/) é barroca,
Lisp é modernista raiz Bauhaus, a sintaxe mais simples possível,
`(f x y)`, tão simples que chega a ser incômoda.
Como uma casa modernista feita só de retângulos de concreto e vidro:
gélida no inverno, tórrida no verão.

## A sintaxe de Lisp e Scheme

A sintaxe de Lisp, denominada *S-expression*, é famosa pelos parentesis por toda parte.

O problema é que o uso generalizado de `(…)` não comunica
visualmente a diferença entre semânticas diferentes.
O ideal é que diferenças semânticas sejam representadas
por sintaxes diferentes.

Veja a definição de uma função para calcular o MDC (Máximo Divisor Comum)
em Scheme, uma variante de Lisp que também usa _S-expressions_:

```
(define (mdc m n)
    (if (= n 0)
        m
        (mdc n (modulo m n))))
```

Para chamar essa função:

```
(mdc 18 45)
```

Em Python, esse algoritmo de MDC recursivo fica assim:[^1]

```
def mdc(m, n):
    if n == 0:
        return m
    else:
        return mdc(n, m % n)
```

E a chamada:

```
mdc(18, 45)
```

[^1]: Essa função funciona mas não é eficiente em Python.
Escrevi assim para ficar parecida com a função `mdc` em Scheme.
No Python, a `mdc` recursiva pode usar muita memória na pilha de execução
se os argumentos forem inteiros grandes.
Scheme tem _recursão de cauda_ otimizada:
quando a chamada recursiva está no fim do corpo da função,
o interpretador salta para a próxima iteração reaproveitando
o frame atual, sem criar novo frame na pilha.

Gosto que Python não usa `;` no final das instruções,
e também gosto que Scheme não usa `,` para separar os argumentos,
só espaços.

O que me incomoda mais nos parentesis de Scheme é
que eles delimitam construções muito diferentes:
instruções como `(define …)` ou `(if …)`, chamadas de função
`(modulo m n)`, e até listas `(0 1 2 3)`.

Em algum código legado você pode encontrar isso:

```
(concurrent (fetch (urls)) (file out))
```

Será que `concurrent` é uma função ou uma instrução especial?
Se for uma função, você sabe que `(fetch (urls))` e `(file out)`
serão invocadas antes e seus resultados serão passados para `concurrent`.

Mas se `concurrent` for uma _forma especial_[^2] ou uma _macro_ em Scheme,
daí tudo é possível.

[^2]: Em Python uma _forma especial_ tem uma sintaxe especial com uma palavra reservada,
como `with` ou `await`.

## Porque if não pode ser uma função

Exemplo de "tudo é possível": o `if` do Scheme pode até parecer uma chamada
de função:

```
(if a (b) (c))
```

Mas o que acontece é que o valor de `a` determina
se `(b)` ou `(c)` é invocada, nunca as duas.

Agora, se o `if` fosse uma função, poderia causar o fim da humanidade.

Considere esta linha de código num sistema de lançamento de mísseis nucleares:

```
(if (checar senha) (lançar mísseis) (desativar mísseis))
```

Se o `if` fosse uma função, as três expressões seriam avaliadas
para então serem passadas para o `if`.
A senha é checada, os mísseis são lançados, e também desativados,
independente do resultado de `(checar senha)`.
Toda vez assim, incondicionalmente.
A função `if` é invocada só depois de executadas essas três chamadas,
para calcular os valores dos três argumentos necessários.
Será tarde demais: os mísseis já estão voando.

Avaliação condicional deve ter uma sintaxe especial.

Em Python a sintaxe do `if` é
bem diferente de chamada de função:

```
if validar(senha): salvar(senha)
```

Na sintaxe de S-expression, isso seria

```
(if (validar senha) (salvar senha))
```

## Inventando uma sintaxe modernista melhor

Se eu fosse inventar uma linguagem experimental simples,
buscaria uma sintaxe mais enxuta que Python,
mas um pouco mais enfeitada que Scheme,
com mais sinais gráficos para ajudar na leitura.

Estou criando a *sintaxe L*. O exemplo dos mísseis ficaria assim:

```
if{validar(senha) lançar(mísseis) desativar(mísseis)}
```

Os princípios básicos da *sintaxe L* são:

* Colocar o primeiro identificador fora dos delimitadores: `f(x y)` em vez de `(f x y)`.
Facilita a leitura porque o primeiro elemento da expressão define o que ela faz.
* Usar delimitadores diferentes para expressões de naturezas diferentes.

A sintaxe geral é `ident「 … 」` onde `ident` é um identificador,
seguido de zero ou mais expressões entre delimitadores que podem ser:

`()` para delimitar os argumentos em uma chamada de função;

`{}` para delimitar o corpo de uma instrução especial;

`[]` para delimitar uma coleção de itens de dados (estou sendo vago de propósito, logo mais explico).

Voltando ao exemplo da *S-expression* misteriosa:

```
(concurrent (fetch (urls)) (file out))
```

Na *sintaxe L*, poderia ser:

```
concurrent(fetch(urls()) file(out))
```

Isso seria uma chamada de função `concurrent` com os argumentos `fetch(urls())` e `file(out)`.

Outra semântica teria outra sintaxe:

```
concurrent{fetch [urls] file(out)}
```

Nesse caso as `{}` indicam que `concurrent` é uma forma especial,
que pode implementar uma semântica diferente
no tratamento de `fetch`, `[urls]` e `file(out)`.
Por exemplo (inventando agora) o comportamento poderia ser:
`fetch` seria invocada de modo concorrente em
diferentes threads, uma cada para cada item de `[urls]`,
e `file(out)` seria invocada no final para capturar os resultados.

Uma terceria variação seria:

```
concurrent{fetch [urls] [file out]}
```

Aqui `[file out]` seria uma tupla com dois identificadores dentro,
equivalente a `(file, out)` em Python.

Antes eu falei que a sintaxe geral é `ident「 … 」`.
A sintaxe ganha muita flexibilidade
com um identificador prefixo, que funciona como um 
[sigil](https://en.wikipedia.org/wiki/Sigil_(computer_programming)).
Por exemplo, `v[…]` é um vetor (uma lista unidimensional),
`c[…]` é um conjunto, `t[…]` é uma tupla.

Uma tupla é uma construção tão útil que merece um atalho, então
`t[1 2 3]` pode ser escrita sem prefixo: `[1 2 3]`.[^3]

[^3]: Essa ideia surgiu depois, quando pensei na sintaxe
para declarar uma função, onde precisei de
uma tupla de identificadores para nomear os parâmetros.

E um dicionário?

Um dicionário na sintaxe de Python:

```
{a:1, b:2, c:3}
```

Poderia ser assim na *sintaxe L*:

```
d[a:1 b:2 c:3]
```

Isso me forçou a reservar outro símbolo, `:` mas por enquanto 
a sintaxe continua enxuta, temos só `:(){}[]` como símbolos especiais.

Para atribuição, podemos usar `=` com o delimitador de instrução especial:

```
={pi 3.1416}
```

Porque a atribuição não pode ser uma função?
Porque o identificador `pi` pode estar sendo criado agora,
não tem como ser avaliado como argumento para uma função.

Veja outra atribuinção em *sintaxe L*:

```
={amostra [10 20 30]}
```

Python[^4]:

```
amostra = 10, 20, 30
```

[^4]: Esse é um atalho válido para escrever uma tupla em Python:
itens separados por vírgula, sem `()` delimitando.
Funciona em alguns contextos.
Em outros, é preciso delimitar com `()`.

Assim como em Scheme, na minha linguagem `=` seria um identificador,
não um símbolo especial.
O mesmo identificador serve para o operador de comparação,
com os delimitadores de uma chamada de função:

A expressão `=(x y)` na *sintaxe L* equivale a `x == y` em Python

Segui o exemplo de Lisp e evitei usar uma notação infixa só para operações aritméticas.
Isso não é tão fácil de ler, mas simplifica muito o parser.
É uma troca válida em uma linguagem experimental que seja fácil de implementar.

O exemplo MDC na *sintaxe L*:

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
(escrita como `m % n` em Python).

Vamos ver agora outra forma de computar MDC,
com laço em vez de recursão.

Python idiomático, usando atribuição de tuplas
para atualizar duas variáveis em parelo:

```
def mdc(m, n):
    while n != 0:
        m, n = n, m % n
    return m
```

Python sem usar atribuição paralela:

```
def mdc(m, n):
    while n != 0:
        temp = n
        n = m % n
        m = temp
    return m
```

*Sintaxe M:*

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

A semântica de `while{}` controla um corpo com N expressões.
A cada volta do laço, a primeira expressão é avaliada.
Enquanto ela é verdadeira,
as demais expressões são executadas.
Quando a primeira expressão é falsa, o laço termina.

Para o operador *diferente*, escolhi `<>` (como em Pascal)
para reservar o `!` para algum uso posterior.
Também acho mais bonita a simetria de `<>` em contraste com `!=`.
Acredito no valor da estética para tomar decisões de sintaxe.

A versão com atribuição paralela de tupla pode ser assim:

```
def{mdc [m n]
    while{ <>(n 0)
        ={[m n] [n modulo(m n)]}
    }
    m
}
```

Isso é viável porque `={…}` denota uma instrução especial,
então temos a liberdade de implementar variações sintáticas dentro dela.
Quando a primeira expressão de `={…}` é uma tupla de identificadores,
a expressão seguinte será avaliada e seus itens atribuídos aos
identificadores, respectivamente.

## Delimitadores diferentes indicam semânticas diferentes

Assim como temos `=(…)` e `={…}` com semânticas diferentes,
podemos usar a *sintaxe L* para expressar os dois operadores de
conjunção em Python: `and` e `&`.

O `and` de Python é um operador especial que não pode ser
sobrecarregado, porque ele tem uma regra de avaliação
diferente dos operadores comuns: na expressão `a() and b()`,
a função `b()` só será executada se `a()` devolver um
valor verdadeiro. Isso se chama "avaliação em curto circuito":
quando o primeiro operando é falso, o segundo não é computado.

Mas em `a() & b()` as duas funções
são executadas sempre, para fornecer os argumentos
do método `__and__` que faz o AND bit-a-bit entre inteiros.

O ponto central é que diferentes regras de avaliação devem
ter sintaxes diferentes, por isso Python tem `and` e `&`.

Na *sintaxe L*, os delimitadores resolvem esse problema.

* `&(a() b())` avalia `a()` e `b()`, para então aplicar `&()` aos dois resultados.

* `&{a() b()}` avalia `a()`; se for verdade, avalia `b()`. A regra de avaliação é diferente.

A notação infixa `x & y` tem a limitação de só acomodar dois operandos, um de cada lado.
Na notação prefixa não temos essa limitação. A instrução `&{…}` pode ter mais argumentos:

```
def{validar [senha]
    &{  >=(len(senha) 8)
        contem(senha '!@#$%&*-')
        contem(senha '0123456789')
        contem_maiúscula(senha)
        contem_minúscula(senha)
    }
}
```

A função `validar` aplica diferentes funções a uma senha.
Se todas retornarem verdadeiro, o resultado é verdadeiro.
Se uma retornar falso, o resultado será falso e as
funções seguintes não serão chamadas.

Por hoje, é só.

Amanhã escrevo outro post com mais exemplos da *sintaxe L*.

> Fiado só amanhã.<br>
> —*aviso comum em botecos no Brasil*
