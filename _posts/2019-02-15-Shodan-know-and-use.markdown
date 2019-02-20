---
layout: post
title:  "Shodan - Conhecendo e Utilizando - PARTE 01"
date:   2019-02-15 11:08:00
categories: All
img: shodan.jpg
---
<p align="justify">
Nesse post irei falar um pouco sobre o motor de busca Shodan para te ajudar a entender um pouco mais sobre ele e também para que você possa utilizar futuramente nos seus pentests, em programas de Bug Bounty etc.
<br>
Irei escrever mais artigos sobre minhas pesquisas com o Shodan no futuro. Então conheça, aprenda e use o Shodan a partir de hoje!
</p>

<p align="justify">
<h3>Primeiramente, o que é ou quem é o Shodan???</h3>
"Shodan is the world's first search engine for Internet-connected devices." - <a href="https://www.shodan.io/" target="_blank">Shodan</a>.
<br>
</p>
<p align="justify">
Ou seja, o Shodan é um motor de busca para encontrar dispositivos conectados à internet, como dispositivos IoT, servidores web, organizações e até mesmo usinas. 
Tempos atrás eu acreditava que o Shodan era apenas um motor de busca qualquer como os demais, e não conhecia o seu verdadeiro poder!!! Eu usava apenas "for Lulz" ou seja, para diversão, como ver webcams, câmeras de segurança de empresas etc.
Mas agora, estou conhecendo o poder real do Shodan e quero que você conheça também.
</p>

<p align="justify">
<h3>Como o Shodan Funciona?</h3>
Você pode controlar e filtar os resultados no Shodan, procurar por produtos específicos, países, portas, IPs, organizações e etc.
<br>
</p>
<p align="justify">
O Shodan funciona por meio de alguns comandos chamados de "Dorks". Iremos ver alguns exemplos agora:
</p>
<ul>
    <p align="justify">
    <li><b>product:</b> -> Retorna apenas resultados que sejam do produto especificado. Exemplo: product:MongoDB</li>
    </p>
    <p align="justify">
    <li><b>country:</b> -> Retorna apenas resultados que façam parte do país que você definiu. Exemplo: country:br</li>
    </p>
    <p align="justify">
    <li><b>net:</b> -> Retorna apenas resultados que façam parte de uma rede. Exemplo: net:192.168.0.1/24 </li>
    </p>
    <p align="justify">
    <li><b>port:</b> -> Retorna apenas resultados que possuam serviços rodando na porta definida. Exemplo: port:27017</li>
    </p>
    <p align="justify">
    <li><b>org:</b> -> Retorna apenas resultados que façam parte de determinada organização. Exemplo: org:Google</li>
    </p>
    <p align="justify">
    <li><b>city:</b> -> Retorna apenas resultado que façam parte de determinada cidade. Exemplo: city:</li>
    </p>
    <p align="justify">
    <li><b>-</b> -> Este é um operador lógico que remove resultados que contenham determinada palavra. Exemplo: -authentication</li>
    </p>
    <p align="justify">
    <li><b>NOT</b> -> Este operador lógico possui a mesma função do operador anterior. Example: NOT authentication</li>
    </p>
    <p align="justify">
    <li>etc etc</li>
    </p>
</ul>
</p>

<p align="justify">
<h3>Como usar o Shodan?</h3>
<b>!!! Primeiro, você deve criar uma conta !!!</b>
<br>
<br>
Depois de ter criado a conta e ter efetuado o login, vamos usar o Shodan para que você entenda sua interface, seu funcionamento e os resultados mostrados.
<br>
<br>
A Dork que iremos utilizar será a <b><i>product</i></b>:
<br>
<img src="/images/shodan/shodan00.png"/>
<br>
</p>
<p align="justify">
Como você pode ver, o Shodan nos trás uma página com vários resultados e campos que serão estudados no decorrer deste artigo.
<br>
<br>
O campo <b>Total Results</b> mostra o número total de resultados que foram encontrados pelo Shodan, como você pode notar, foram encontrados 15,196,574 resultados relacionados ao item pesquisado, neste caso <i>product: Nginx</i>:
<br>
<img src="/images/shodan/shodan01.png"/>
<br>
<br>
O campo <b>Top Countries</b> mostra o ranking dos países com mais resultados presentes da busca:
<br>
<img src="/images/shodan/shodan02.png"/>
<br>
<br>
O campo <b>Top Services</b> mostra o ranking dos serviços mais utilizados de acordo com a busca:
<br>
<img src="/images/shodan/shodan03.png"/>
<br>
<br>
O campo <b>Top Organization</b> mostra o ranking das organizações que mais utilizam o produto pesquisado:
<br>
<img src="/images/shodan/shodan04.png"/>
<br>
<br>
O campo <b>Top Operation Systems</b> mostra o ranking com os sistemas mais utilizados de acordo com a busca:
<br>
<img src="/images/shodan/shodan05.png"/>
<br>
<br>
O campo <b>Top Versions</b> mostra o ranking das versões mais utilizadas do produto buscado:
<br>
<img src="/images/shodan/shodan06.png"/>
<br>
<br>
Podemos observar várias informações a respeito dos Hosts retornados pelo Shodan.
<br>
<img src="/images/shodan/shodan07.png"/>
<br>
</p>
<p align="justify">
Começando pelo Título, é possível notar que o título do Host está definido como <b><i>302 Found</i></b>
<br>
Logo abaixo, é possível identificar qual o IP do Server: <b><i>128.90.143.82</i></b>
<br>
Identificamos também qual o hostname: <b><i>undefined.hostname.localhost</i></b>
<br>
A organização que o host pertence (Pode ser tanto o dono do host, como um serviço de hospedagem): <b><i>Powerhouse Management</i></b>
<br>
A data que o server foi adicionado ao Shodan: <b><i>2019-02-20 03:07:48 GMT</i></b>
<br>
A cidade e país onde o host está hospedado: <b><i>Paris - França</i></b>
<br>
E por fim, o corpo da resposta retornada pelo host.
</p>

