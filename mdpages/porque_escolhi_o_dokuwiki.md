# Porquê escolhi o dokuwiki

Existem vários motores de wiki [^1]. O mais famoso é o
[MediaWiki](https://www.mediawiki.org/wiki/MediaWiki), usado na
Wikipédia. O **MediaWiki** tem muitos recursos, alto desempenho, e uma
grande comunidade. Porém a administração técnica de um MediaWiki é
complicada: o conteúdo fica em dezenas de tabelas diferentes em um banco
de dados MySQL. Isso significa que fazer e recuperar um backup não é
trivial. E as atualizações do MediaWiki às vezes exigem procedimentos
complicados.

Em contraste, o [docuwiki](https://www.dokuwiki.org/) não precisa de um
banco de dados para funcionar. O conteúdo das páginas fica em arquivos
de texto simples, em diretórios. Assim é muito fácil fazer backup \--
posso usar até o `git` \-- e também criar scripts para automatizar
tarefas de edição de páginas.

Existem outros motores de wiki que armazenam as páginas em arquivos de
texto simples. Porém, entre esses wikis sem banco de dados, eu concluí
que **docuwiki** é o mais amplamente utilizado, o melhor documentado, e
aquele suportado pela maior comunidade internacional \-- uma indicação
disso é o número de traduções disponíveis. Depois que eu já havia feito
a escolha, o Rasmus Lerdorf (criador do PHP) lacrou a decisão: encontrei
um tuite dele contando que [wiki.php.net](https://wiki.php.net/) usa
**docuwiki**, e ele foi escolhido porque é fácil de administrar,
atualizar e becapear:

<blockquote class="twitter-tweet" data-lang="en"><p lang="en" dir="ltr">We use <a href="https://twitter.com/dokuwiki?ref_src=twsrc%5Etfw">@DokuWiki</a> for <a href="https://t.co/SeGL1Bi7Fp">https://t.co/SeGL1Bi7Fp</a> - easy to manage, upgrade and backup</p>&mdash; Rasmus Lerdorf (@rasmus) <a href="https://twitter.com/rasmus/status/913676141558468609?ref_src=twsrc%5Etfw">September 29, 2017</a></blockquote>
<script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>

![](tag>colaboração)

\~\~DISQUS\~\~

[^1]: Um *wiki engine* ou motor de wiki é um programa, geralmente
    instalado em um servidor Web, que organiza e apresenta as páginas de
    um wiki
