+++
date = '2026-01-17T22:14:07-03:00'
title = 'A Rede Social Local'
toc = true
draft = false
tags = ['tecnologia', ]
+++

Muita gente passa dias ou até semanas sem encontrar amigos
num bar, sem ir ao cinema, sem dar um rolê na praça.
Principalmente a partir da pandemia.

O isolamento leva à polarização.
A polarização destrói nossa vontade de colaborar
para resolver os problemas que são de todos.

Como construir uma nova rede social que não agrave estes problemas?

## O Bonde

O [Jeferson Fernando](https://linuxtips.io/instrutor/jeferson-fernando/)
da LinuxTIPS lançou a ideia de construir uma rede social no Brasil:
[O Bonde](https://obonde.linuxtips.io/).

Fiquei feliz por ser convidado a colaborar com aquele time de feras,
para desenvolver um projeto que reduza nossa dependência
de plataformas controladas por bilionários ou governos,
e ainda proporcione uma oportunidade de aprendizagem real.

Até 17/jan ainda não tivemos uma reunião, só o anúncio naquela
[live em 15/jan](https://www.youtube.com/watch?v=wqCQ6R_B9uM).

A live me pilhou para compartilhar uma ideia:
uma abordagem diferente para uma rede social.

## O local

As redes sociais e o streaming
contribuem para o isolamento, preenchendo o
tempo livre com atividades que não satisfazem nossa
necessidade de contato humano para se divertir e aprender juntos.

Cresci numa época em que não existia celular, muito menos
Web ou redes sociais. Mesmo assim, não faltava balada
toda noite, para quem tivesse a fim.
A gente se encontrava em algum local previamente combinado,
e daí as coisas aconteciam. Simples assim.

A troca de ideias cara a cara, em um lugar físico, tem
outra qualidade. Tem mais afeto, tem mais graça.

Você não pode ser um troll, porque
é muito fácil ser banido de um lugar físico e de
uma turma que se encontra regularmente.

## A RSL

Imagine uma rede social federada, no modelo do Mastodon[^1],
onde existem N "instâncias" que se conectam para trocar mensagens pelo mesmo
protocolo. Você tem sua conta na *Ciberlandia.pt* mas
pode trocar ideia com pessoas da *Fosstodon.org* e da *Bolha.us*.
Sua timeline pode ter mensagens de várias instâncias.

[^1]: A ideia da rede social local que apresento aqui é
independente de qualquer protocolo específico. O Mastodon
usa ActivityPub, o Bluesky usa AT Proto.
Num movimento de rede social soberana, além da arquitetura
federada, seria muito bom ter um esforço de integração através
de pontes entre protocolos.

Diferentes instâncias podem ter diferentes regras para se inscrever:
algumas só aceitam convidados, outras aceitam qualquer pessoa, mas
a pessoa tem que informar um e-mail ou celular, que será
validado.

A diferença fundamental: numa rede social local (RSL)
você só pode validar sua inscrição em uma instância
depois de se encontrar presencialmente com 3 pessoas daquela instância.
Algumas instâncias podem até exigir encontros presenciais periódicos
para revalidar a conta.[^2]

[^2]: Estes são dois parâmetros que podem variar de instância
para instância. Numa instância, a regra é 6 contatos presenciais
na inscrição, e anualmente a pessoa precisa refazer 6 contatos
presenciais. Outra instância pode definir que só precisa 2 contatos
presenciais na inscrição, mas eles precisam ser renovados todo mês.
Isso estimula as pessoas a manterem o contato no mundo real!

Isso limitaria o alcance das instâncias? Sim.
Nenhuma empresa faria isso.
Mas não estamos aqui para criar uma nova big tech.
Estamos aqui para melhorar a vida social das pessoas,
com uma rede social onde o convívio no mundo real
contribui para um convívio virtual mais
humano, solidário, colaborativo e saudável.

## Como construir

A ideia do [Bonde](https://obonde.linuxtips.io/) é
oferecer uma oportunidade de aprendizagem para um
grande número de pessoas que vão colaborar na
construção e operação de uma rede social.

O difícil é organizar uma galera em torno de um
único projeto.
As dificuldades de comunicação e alinhamento
crescem geometricamente com o número de colaboradores.

Mas queremos ir além de construir uma plataforma
de rede social.

Queremos uma rede social federada, então desde o início
o objetivo é ter várias instâncias que troquem mensagens.

Algumas instâncias podem usar o software do mastodon,
que [está lá no Github](https://github.com/mastodon/mastodon),
apenas mudando o processo de validação da inscrição
para exigir a validação presencial.

Outros grupos podem criar do zero novos servidores compatíveis em Go,
ou Rust, ou Python, ou Node.js, ou PHP...

A solução de validação presencial é um projeto em si.
Pode ser um *plug-in* para os servidores.

Novos apps para Android, iOS, Linux, Windows, MacOS, podem ser criados.
Mais de um por SO até. Como já acontece no Mastodon: há vários clientes
disponíveis para alguns SO.

E quem optar pelo software do Mastodon, não vai aprender nada?
Vai sim. Muito antes do pessoal que fizer do zero,
este pessoal estará praticando a operação de uma instância.
E também aprendendo o mais difícil:
como fazer a moderação de uma rede social.

Mas o trabalho deles não será tão pesado quanto é nas grandes
redes sociais centralizadas.

Porque em cada instância, as regras de moderação vão
se aplicar a uma comunidade local, que se encontra ao vivo.
Não existe melhor estímulo para o bom comportamento social que
o convívio contínuo com um grupo de seres humanos que pode
te dar feedback na hora se você não souber respeitar os outros.

Então, taí minha primeira contribuição para **O Bonde**.
Vamos focar desde o começo em federação, múltiplas implementações,
governança e moderação com aquele toque humano que só existe em
comunidades locais.[^3]

[^3]: O desenvolvimento e operação das instâncias
pode ser feito remotamente, por times espalhados mundo afora.
Mas a participação dentro de uma instância local só pode iniciar
com um encontro presencial. Daí nada melhor que 
uma escola, um local de trabalho, um baile funk, ou
uma conferência comunitária como uma PythonBrasil, 
para reunir a galera ao vivo e estreitar os laços na rede.
