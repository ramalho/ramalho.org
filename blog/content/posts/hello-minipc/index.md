+++
date = '2025-07-27T17:32:45-03:00'
title = 'Hello, minipc!'
toc = true
draft = false
tags = ['tecnologia', 'Elixir', 'IoT']
+++

<img src="/posts/minipc-nervoso/nerves-blink3.gif" width="{{< width >}}"
alt="Um Raspberry Pi Zero fazendo um LED vermelho piscar.">

No mundo da eletrônica digital,
"Hello, world!" é fazer um LED piscar.

Hoje fiz um LED piscar com Elixir rodando num 
[Raspberry Pi Zero W](https://www.raspberrypi.com/products/raspberry-pi-zero-w/).

<!--more-->

Eu chamo de "minipc" um computador pequeno que roda GNU/Linux.
Todos os Raspberry Pi entram nessa categoria,
exceto os
[Raspberry Pi Pico](https://www.raspberrypi.com/documentation/microcontrollers/pico-series.html)
que são microcontroladores.[^1]

Para hackeragem eletrônica com Elixir, é preciso um minipc,
onde a gente instala um firmware com Nerves—o framework de IoT.
Um dos minipc mais baratos que rodam Nerves é o Raspberry Pi Zero W.
Agora tem um projeto muito bacana chamado
[Elixir Circuits](https://github.com/elixir-circuits),
que facilita muito os primeiros passos.

O único software que você precisa instalar no seu computador é o
[fwup](https://github.com/fwup-home/fwup),
que grava imagens em um cartão microSD para instalar no Raspberry Pi.

No meu caso, baixei a imagem 
[circuits_quickstart_rpi0.fw](https://github.com/elixir-circuits/circuits_quickstart/releases)
e usei este comando para gravar no microSD já configurando a senha do WiFi:

```sh
% NERVES_WIFI_SSID='FJR_2G' NERVES_WIFI_PASSPHRASE='???' fwup circuits_quickstart_rpi0.fw
```

Coloquei o microSD no Raspberry Pi, liguei e fiz:

```sh
% ping nerves.local
PING nerves.local (192.168.25.176): 56 data bytes
64 bytes from 192.168.25.176: icmp_seq=0 ttl=64 time=136.334 ms
64 bytes from 192.168.25.176: icmp_seq=1 ttl=64 time=10.165 ms
^C
--- nerves.local ping statistics ---
2 packets transmitted, 2 packets received, 0.0% packet loss
round-trip min/avg/max/stddev = 10.165/73.249/136.334/63.085 ms
```

Sucesso! 🎉

Daí fiz SSH para lá, usuário `circuits`, senha `circuits`.

Daí aparece esse textão todo:

```sh
% ssh circuits@nerves.local
Elixir Circuits Quickstart
https://github.com/elixir-circuits/circuits_quickstart

ssh circuits@nerves-eb49.local # Use password "circuits"

(circuits@nerves.local) Password: 
Interactive Elixir (1.18.4) - press Ctrl+C to exit (type h() ENTER for help)

                ;kX'
              ,0XXXl
             xNXNNXX.
           'KNNXXXXX0.
          ;XNNNNNNNNN0.
         ;XNNNNNNNNNNNX:
        .XNNNXXXXXXXXXXXO.
        kNNNXNNNNXXNXXNNXNo
       .NNNXNNNNNXXNNXNNXXXO
       cXXXNNNNNNXXNNNNNNNNNd
       lNNNNNNNNNXXNXXXNNNNNK
       'NNXNXNXXXXXXXXXNNNNNk
        oNXNXXXXXNXXXNNNNXXX.
         :KXXXXXXNXXXNNNNXk.
           ;xXNXXXXNXXX0o.
              .',::;,.

      Elixir Circuits Quickstart

All of the Elixir Circuits projects are available in this firmware
image. See https://github.com/elixir-circuits/circuits_quickstart for
more details.

circuits_quickstart 0.13.1 (a8ed94b5-987f-5492-4456-fcec68c50e7c) arm rpi0
  Serial       : 000000005bbdeb49
  Uptime       : 36 minutes and 0 seconds
  Clock        : 2025-07-28 01:19:10 UTC
  Temperature  : 38.5°C

  Firmware     : Valid (A)               Applications : 43 started
  Memory usage : 56 MB (18%)             Part usage   : 0 MB (0%)
  Hostname     : nerves-eb49             Load average : 0.00 0.00 0.00

  wlan0        : 192.168.25.176/24, fe80::ba27:ebff:fee8:be1c/64
  usb0         : 172.31.205.245/30

Nerves CLI help: https://hexdocs.pm/nerves/iex-with-nerves.html

Toolshed imported. Run h(Toolshed) for more info.
iex(1)> 
```

Sucesso!! 🎉🎉

No prompt do `iex` você pode escrever o que quiser.

Essa sequência liga e desliga um led que eu conectei ao pino 18 do RPi:

```elixir
iex(1)> alias Circuits.GPIO
Circuits.GPIO
iex(2)> GPIO.open(18, :output)
{:ok, %Circuits.GPIO.CDev{ref: #Reference<0.4162599041.148242441.43479>}}
iex(3)> {:ok, led} = v()
{:ok, %Circuits.GPIO.CDev{ref: #Reference<0.4162599041.148242441.43479>}}
iex(4)> GPIO.write(led, 1)
:ok
iex(5)> GPIO.write(led, 0)
:ok
```

No passo 2, configurei o pino 18 para saída.
Em seguida, usei a função `v()` para recuperar o resultado anterior
e vincular a variável `led` ao segundo valor da tupla
devolvida por `GPIO.open(18, :output).
Os passos 4 e 5 são ligar e desligar o led.

Sucesso!!! 🎉🎉🎉

E finalmente, escrevi uma função anônima para piscar o led:

```elixir
blink = fn led ->
  Stream.cycle([1, 0]) |> 
  Enum.each(fn state ->
    GPIO.write(led, state)
    Process.sleep(500)  # 500ms delay between state changes
  end)
end
```

Invocando ela assim, o LED passou a piscar!

```
iex(7)> blink.(led)
```

Sucesso!!!! 🎉🎉🎉🎉

Agora é arrumar tempo para continuar experimentando
com Elixir e Nerves no Raspberry Pi!

Até mais!

[^1]: A placa microcontroladora mais famosa na cena hacker é o Arduino R3.
A diferença essencial entre um microcontrolador e um minipc é que
o primeiro não tem memória suficiente para rodar o GNU/Linux.
O *firmware* que você instala num microcontrolador contém a sua aplicação e dependências,
mas não um SO.