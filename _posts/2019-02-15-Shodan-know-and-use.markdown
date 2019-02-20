---
layout: post
title:  "Shodan - Conhecendo e Usando"
date:   2019-02-15 11:08:00
categories: All
img: shodan.jpg
---
<p align="justify">
Nesse post irei falar um pouco sobre o Shodan para te ajudar a entender um pouco mais sobre ele e também para que você possa utilizar futuramente nos seus pentests, em programas de Bug Bounty etc.
Irei escrever mais artigos sobre minhas pesquisas com o Shodan no futuro. Então conheça, aprenda e use o Shodan a partir de hoje!
</p>

<p align="justify">
<h3>Primeiramente, o que é ou quem é o Shodan???</h3>
"Shodan is the world's first search engine for Internet-connected devices." - <a href="https://www.shodan.io/" target="_blank">Shodan</a>.
Ou seja, o Shodan é um motor de busca para encontrar dispositivos conectados à internet, como dispositivos IoT, servidores web, organizações e até mesmo usinas. 
Tempos atrás eu acreditava que o Shodan era apenas um motor de busca qualquer como qualquer outro, e não conhecia o seu verdadeiro poder!!! Eu usava apenas "for Lulz" ou seja, para diversão, como ver webcams, câmeras de segurança de empresas etc.
Mas agora, estou conhecendo o poder real do Shodan e quero que você conheça também.
</p>

<p align="justify">
<h3>Como o Shodan Funciona?</h3>
O Shodan funciona por meio de alguns comandos chamados de "Dorks". Iremos ver alguns exemplos agora:
<ul>
    <li>product: -> Retorna apenas resultados que sejam do produto especificado. Exemplo: product:MongoDB</li>
    <li>country: -> Retorna apenas resultados que façam parte do país que você definiu. Exemplo: country:br</li>
    <li>net: -> Retorna apenas resultados que façam parte de uma rede. Exemplo: net:192.168.0.1/24 </li>
    <li>port: -> Retorna apenas resultados que possuam serviços rodando na porta definida. Exemplo: port:27017</li>
    <li>org: -> Retorna apenas resultados que façam parte de determinada organização. Exemplo: org:Google</li>
    <li>city: -> Retorna apenas resultado que façam parte de determinada cidade. Exemplo: city:</li>
    <li>- -> Este é um operador lógico que remove resultados que contenham determinada palavra. Exemplo: -authentication</li>
    <li>NOT -> Este operador lógico possui a mesma função do operador anterior. Example: NOT authentication</li>
    <li>etc etc</li>
</ul>
</br>
Então, você pode controlar e filtar os resultados no Shodan, procurar por produtos específicos, países, portas, IPs, organizações e etc.
</p>

<p align="justify">
<h3>Como usar o Shodan?</h3>
!!! Primeiro, você deve criar uma conta !!!

Depois de ter criado a conta e ter efetuado o login, vamos usar o Shodan para que você entenda sua interface, seu funcionamento e os resultados mostrados.
</br>
A primeira Dork que iremos utilizar será a <i>product</i>:
</p>
<img src="./shodan/shodan00.png"/>
