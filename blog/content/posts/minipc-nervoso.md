+++
date = '2025-07-27T17:32:45-03:00'
title = 'Minipc Nervoso'
toc = true
draft = true
tags = ['tecnologia', ]
+++

Eu chamo de "minipc" um computador pequeno que roda GNU/Linux.
Todos os Raspberry Pi entram nessa categoria,
exceto os novos
[Raspberry Pi Pico](https://www.raspberrypi.com/documentation/microcontrollers/pico-series.html)
que são microcontroladores.[^1]

brew uninstall elixir
brew uninstall erlang

brew update
brew install fwup squashfs coreutils xz pkg-config

brew install wxwidgets

https://elixirforum.com/t/wifi-problems-with-nerves-livebook-on-raspberry-pi-zero-2w/61957
https://elixirforum.com/t/nerves-on-raspberry-pi-zero-2-w-does-not-boot/59885
https://github.com/elixir-circuits/circuits_quickstart/blob/main/README.md

[^1]: A placa microcontroladora mais famosa na cena hacker é o Arduino R3.
A diferença essencial entre um microcontrolador e um minipc é que
o primeiro não tem memória suficiente para rodar o GNU/Linux.
O *firmware* que você instala num microcontrolador contém a sua aplicação e dependências,
mas não um SO.