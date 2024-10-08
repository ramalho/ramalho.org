## Boas práticas para lidar com hashes de senhas

Desde 1995 aprendi que não se armazena senhas de usuários para efeito
de login, mas apenas um *hash* da senha, que é uma espécie de assinatura
digital. A primeira versão do **passdrill** usava o algoritmo SHA-256
para gerar o hash antes de praticar.

O hash de uma senha não é uma senha encriptada. Matematicamente, não há
como inverter o efeito de uma função de hash para obter o dado original
\-- a [analogia do Bánffy](analogia do Bánffy) transmite muito bem a
ideia. Mas é possível aplicar força bruta: podemos tentar todas as
senhas possíveis até um tamanho razoável, aplicando a função de hash e
comparando o resultado com o hash que eu tenho.

Para tornar um ataque de força bruta mais difícil, há duas coisas a
fazer:

1.  Concatenar um **sal** à senha antes de gerar o hash.
2.  Usar um algoritmo computacionalmente mais caro.

O **sal** (*salt*, em inglês) é uma sequência de bytes aleatórios que
deve ser concatenada à senha do usuário antes de ser computado o hash. O
ideal é que o sal seja diferente para cada senha, de modo que duas
senhas iguais tenham hashes diferentes. O sal pode ser escondido do
atacante, mas mesmo que ele seja armazenado junto com o hash, já ajuda
muito, porque impede o atacante de usar um banco de hashes
pré-computados a partir de um dicionário de senhas comuns.

Quando mostrei o [passdrill](passdrill) para colegas da ThoughtWorks
especialistas em segurança, me explicaram que a melhor prática em 2017 é
usar uma [função de derivação de
senha](https://en.wikipedia.org/wiki/Key_derivation_function) que aplica
algum algoritmo de hash milhares de vezes sobre a senha, tornando bem
mais custosos os ataques de força bruta. As funções de derivação de
senha mais indicadas hoje são **argon2**, **scrypt** e **bcrypt**.

Usei o **scrypt** porque ele está na biblioteca padrão de Go e de Python
(na versão 3.6). Porém, mesmo usando Python 3.6 em alguns ambientes a
função `hashlib.scrypt` não está disponível porque ela depende do
OpenSSL 1.1, que ainda não está empacotado em todas as distribuições.
Então quando o programa `passdrill.py` usa a função
`hashlib.pkdbf2_hmac` quando ele não consegue importar a função
`hashing.scrypt`. A versão `passdrill.go` suporta ler tanto **scrypt**
quanto **pbkdf2** em todos os ambientes, mas dá preferência ao
**scrypt** na hora de criar um novo arquivo de hash para praticar.

![](tag>segurança senhas)

\~\~DISQUS\~\~
