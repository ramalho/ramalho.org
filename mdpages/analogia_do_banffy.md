# Ricardo Bánffy explica um Hash Criptográfico

Meu amigo [Ricardo Bánffy](https://twitter.com/rbanffy) trabalhou comigo
no **[Brasil Online](Brasil Online)**, o 1º portal da Abril na Web,
inaugurado em abril de 1996, um pouco antes do UOL (e uns 3 anos antes
do BOL, que reaproveitou o domínio `bol.com.br`).

Entre os primeiros usuários do **Brasil Online** havia uns tantos
\"VIPs\": executivos de mídia, publicitários etc. Um deles esqueceu a
senha e pediu para sua assistente ligar para a Abril para recuperá-la. O
recado chegou até o meu chefe, Antonio Machado, que pediu para eu
resolver. Passei a bola para o Bánffy. O usuário \"VIP\" estava irritado
porque o nosso suporte técnico havia dito que era impossível recuperar a
senha, e ele achava que era má vontade: como podíamos autenticar um
login sem saber a senha? Na verdade, a gente armazenava só um hash da
senha, na época um hash MD5 com sal era a [boa
prática](Boas práticas para lidar com hashes de senhas).

Para explicar a questão para a assistente do \"VIP\" irritado, o Bánffy
usou a seguinte analogia:

> Quando o usuário cria a conta, a gente anota a senha num pedaço de
> papel, queima o papel, e guarda as cinzas numa caixa de fósforos.
> Depois, quando o usuário digita o login e a senha, a gente pega essa
> senha, queima, e compara as cinzas com as que temos na caixa de
> fósforos. Assim a gente consegue dizer se as senhas eram iguais, mas
> não temos como reconstruir a senha a partir das cinzas.[^1]

Com essa estória o Bánffy convenceu a assistente que o patrão dela teria
que seguir o procedimento normal para criar uma nova senha.

![](tag>segurança senhas causo)

\~\~DISQUS\~\~

[^1]: Essa não é uma citação literal, mas é assim que eu lembro.
