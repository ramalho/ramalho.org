+++
date = '2025-09-01T18:45:24-03:00'
title = 'Python Fluente: mutirão para revisar o vol. 1'
toc = true
draft = false
tags = ['tecnologia', 'Python Fluente', 'livro']
+++

Agora vai!

Estou finalizando a pré-produção do _Python Fluente Segunda Edição_ em PT-BR **IMPRESSO**.

O livro será lançado em 3 volumes, 8 capítulos cada,
cerca de 400 páginas por volume.

Agora estou trabalhando no **volume 1**, sub-título _Dados + Funções_.

<!--more--> 

O _preprint_ **pre2** está aqui:

https://pythonfluente.com/pyfl-vol1-pre2.pdf

## Como ajudar

Convido a comunidade a ajudar na revisão do _preprint_ em PDF.

Defeitos a reportar:

* palavras erradas ou grudadas;
* linhas quebradas em lugares errados nos códigos Python
* links quebrados;
* problemas de formatação;
* qualquer coisa que estiver estranha.

### Prazo

Preciso fechar esse volume até dia 8/set.

### Sorteio do capítulo

Para evitar que todo mundo revise o capítulo 1 e ninguém revise o capítulo 8, vamos fazer assim.

Abra o console do Python e faça essa conta com o ano do seu nascimento:

```python
>>> ano = 1963
>>> ano % 8 + 1
4
```

O resultado é o número do capítulo que eu gostaria que você revisasse. 🙏

Há 3 maneiras de mandar correções:

### Mande e-mail

Facilita se você criar um _issue_, mas precisa ter conta no
Github.

Toda ajuda é bem vinda, então se quiser pode mandar e-mail para `<primeiro_nome_do_LR>@<ultimo-nome_do_LR>.org`.

Por gentileza, coloque a palavra `[ERRATA]` no assunto e não se esqueça de:

* Identificar a versão do pré-print. Isso aparece na página 2 do PDF. Ex. se for "2a pré-impressão", coloque `pre2` no assunto da mensagem, e uma breve descrição.
* Identificar o número página lógica onde está o problema. O número da página lógica é o que aparece no rodapé (o PDF pode ter páginas antes de 1, mas o texto principal começa na página lógica 1).
* Descrever o problema incluindo texto ao redor para facilitar a busca.


### Cadastre um issue no GitHub

A aba de _issues_ do repositório está aqui:
https://github.com/pythonfluente/pythonfluente2e/issues

Clique em `[New issue]` e use o _template_ chamado "Revisão pre-print". Pode cadastrar um ou mais defeitos simples no mesmo issue.
Por gentileza, mantenha a palavra `[ERRATA]` no título e o tag `(impresso)`.


### Faça um PR

Se preferir, pode fazer um _pull-request_.
Use a prefixo `[ERRATA]` no título do PR.

Os arquivos para impressão do volume 1 estão no diretório `/vol1`.

https://github.com/pythonfluente/pythonfluente2e/tree/main/vol1

Não precisa cadastrar um _issue_ antes de mandar PR,
mas se puder, facilita um pouco o meu lado.

**Agradeço sua ajuda!**

🐍 🧡 🦎
