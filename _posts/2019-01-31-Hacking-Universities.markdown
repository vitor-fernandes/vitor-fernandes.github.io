---
layout: post
title:  "Hackeando Instituições de Ensino no Brasil"
date:   2019-01-31 16:10:00
categories: All
img: banner.png
---
<p align="justify">
Olá, me chamo Vitor Fernandes a.k.a rapt00r, sou graduando em sistemas de informação pela UFPB e atualmente estou no 6º período do curso (01/2019). Faço parte de um grupo de estudos que criei com mais 3 amigos dentro da universidade, o EthicalHackers UFPB, discutimos temas sobre Ethical Hacking (really?? :O), Linux, Programação, Segurança da Informação e afins. O link para a página do grupo está no rodapé do blog, curte lá que ajuda bastante :)
</p>

<p align="justify">
Neste post irei falar um pouco sobre uma pesquisa pessoal que tinha como objetivo realizar testes de intrusão e documentar as vulnerabilidades encontradas no Sistema Integrado de Gestão de Atividades Acadêmicas (Sigaa) que é bastante utilizado em universidades públicas brasileiras (inclusive na minha). Esta pesquisa é de minha autoria, ou seja, não fui obrigado e nem muito menos pago para realizar os testes e documentar os resultados, tudo foi feito por livre e espontânea vontade e a documentação foi escrita com a ajuda de um professor da UFPB. Este post tem como objetivo falar sobre as vulnerabilidades encontradas, sendo bem objetivo para tornar a leitura menos cansativa. Os detalhes completos você pode encontrar no relatório que está disponível para download no final do post.
</p>

<p align="justify">
Bom de início, posso falar que o Sigaa é um sistema utilizado para o controle de atividades acadêmicas (ver notas, se cadastrar em disciplinas, em projetos, bolsas etc) por várias universidades e instituições públicas brasileiras. Procurei por quantas instituições utilizavam o sistema e encontrei um total de 42 instituições, compostas por institutos e universidades (podem haver mais) utilizando a dork: inurl:"sigaa/public/" e estão listadas abaixo:
</p>

<p align="justify">
<b>UFC, UFRN, UFMA, UFERSA, UFPB, UFS, UNILA, CEFETMG, UFAL, UNIFAP, UFABC, UFRA, UNIR, UFOPA, IFC, UFPI, UFRR, UNIFEI, UEMA, UFPA, UFOB, IFPA, UFRB, UFG, IFRJ, UFLA, IFFARROUPILHA, UFSB, IFBAIANO, UNIFESSPA, UFRRJ, UFSJ, UNILAB, IFAM, UFCA, IFSC, IFSUDESTEMG, IFRS, ITVDS, IFPR, IFAC, IFAL</b>
</p>

<p align="justify">
Qual o motivo de estar tornando este relatório público? É simples, a segurança da informação ainda não é tão importante no nosso país, e tive que realizar essa atitude (infelizmente) para que houvesse uma maior preocupação por parte dos desenvolvedores do sistema.
</p>

<p align="justify">
Quero deixar claro que meu objetivo não é denegrir a imagem de alguma instituição ou dos desenvolvedores, mas sim poder contribuir com a segurança do sistema bem como também contribuir para o cenário de Segurança da Informação no país. Espero que este post seja útil e que não haja consequências futuras. 
</p>

<p align="justify">
Os desenvolvedores já foram avisados previamente das falhas e sobre a publicação do relatório. A seguir, uma breve timeline de como foi desde o início até agora:
</p>

<p align="justify">
<i>
05/2018 -> Comecei os testes no Sigaa da UFPB e encontrei boa parte das falhas. <br>
12/07/2018 -> Documentei as falhas e o professor enviou o relatório completo para o STI (Equipe de TI da UFPB).<br>
24/07/2018 -> Enviei um e-mail pedindo alguma atualização sobre a correção das falhas, mas não obtive nenhuma resposta.<br>
30/07/2018 -> Percebi que apenas a falha de SQL Injection tinha sido corrigida, então enviei outro e-mail perguntando sobre a correção das outras vulnerabilidades.<br>
30/07/2018 -> Recebi resposta do professor falando que as falhas fizeram muito ruído, e causou indisponibilidade do Sigaa. Também foi citada uma futura parceria com o STI e o Ethical Hackers.<br>
12/11/2018 -> Enviei o relatório para a empresa desenvolvedora do Sigaa. Mas não obtive nenhuma resposta.<br>
28/12/2018 -> Enviei um e-mail para o STI avisando sobre a notícia que saiu no G1 sobre o aluno que explorou uma das vulnerabilidades catalogadas no relatório.<br>
28/12/2018 -> Enviei novamente o relatório para a empresa desenvolvedora do Sigaa, dessa vez obtive a resposta de confirmação.<br>
04/01/2019 -> Enviei um e-mail para a empresa desenvolvedora do Sigaa e o professor para deixar claro os objetivos que foram passados ao entregar o relatório bem como um prazo de até o dia 31/01/2019 até que o relatório se tornasse público.<br>
14/01/2019 -> Enviei um e-mail pedindo a versão do sistema para requisitar CVE's de algumas falhas. Sem resposta. <br>
22/01/2019 -> Enviei um e-mail pedindo atualizações sobre a correção das vulnerabilidades. Sem resposta.<br>
30/01/2019 -> Enviei um e-mail avisando que o tempo que dei para corrigir as vulnerabilidades acabou e irei publicar o relatório.<br>
2019 -> Processos? Espero que não!
</i>
</p>

<p align="justify">
Agora é parte que interessa para todos (creio eu), quais as falhas encontradas, como consegui encontrar e etc. So, let's go!
</p>

<p align="justify">
As falhas encontradas no sigaa vão de nível médio ao crítico (me baseei na OWASP para catalogação do nível de criticidade das vulnerabilidades) e estão catalogadas da seguinte forma:
</p>

<img src="../images/vulnerabilidades_sigaa.png">

<p align="justify">
Como podem notar encontrei vulnerabilidades de SQL Injection, XSS (Self e Stored) e IDOR. Falarei agora um pouco mais sobre cada uma delas e quais as consequências que cada um trouxe dentro do sistema.
</p>

<p align="justify">
<h2>IDOR (Insecure Direct Object Reference)</h2>
</p>

<p align="justify">
Começarei pelas falhas de <b>IDOR</b>. Se você não conhece essa falha eu falarei um pouco mais sobre ela agora!
</p>

<p align="justify">
"Insecure Direct Object Reference (IDOR) ocorre quando um aplicativo expõe uma referência a um objeto de implementação interno. Usando esse caminho, ele revela o identificador real e o formato / padrão usado do elemento no lado de back-end de armazenamento."
</p>

<p align="justify">
Encontrei IDOR's em diferentes locais do sistema:
</p>

<p align="justify">
A primeira foi no sistema de email, onde eu conseguia ler todos os e-mails enviados no Sigaa. Percebi que ao fazer uma requisição para ler algum e-mail, era passado um parâmetro que continha uma referência para o e-mail. Ao fazer uma requisição alterando o valor desse parâmetro, o e-mail referente ao valor passado era aberto. O vídeo da PoC se encontra abaixo:
</p>
<iframe width="560" height="315" src="https://www.youtube.com/embed/8CZ4sQW7yec" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

<br>

<p align="justify">
A segunda foi nos arquivos enviados pelos professores na disciplina, com esta falha eu conseguia baixar todos os arquivos que estavam armazenados no servidor, desde fotos de perfil até documentos de outros usuários e da própria universidade. Ao fazer uma requisição, era passado um parâmetro com um valor que identificava o documento, ao mudar este valor e enviar ao servidor o documento referente ao valor era retornado e baixado. O vídeo da PoC se encontra abaixo:
</p>
<iframe width="560" height="315" src="https://www.youtube.com/embed/aS8-g1K_CLQ" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

<br>

<p align="justify">
A terceira foi no acesso das turmas, onde eu conseguia APENAS acessar as turmas, não era possível fazer muita coisa de relevante. Ao fazer a requisição, um parâmetro contendo o ID da turma era passado, ao alterar este valor, a turma referente ao valor passado era acessada. Não gravei vídeo pois não achei muito relevante :)
</p>

<p align="justify">
<h2>XSS (Cross-Site Scripting)</h2>
Passando agora para as falhas de <b>XSS (Cross-Site Scripting)</b>
</p>

<p align="justify">
Se você estuda Segurança da Informação, já deve ter ouvido nesse carinha :), mas se ainda não conhece deixarei uma pequena explicação sobre:
</p>

<p align="justify">
"Essa vulnerabilidade permite um usuário injetar códigos JS (JavaScript) maliciosos em determinado campo de texto de uma página e dependendo do seu tipo, esse código será mostrado para outros usuários."
</p>

<p align="justify">
Encontrei 3 Self-XSS e 2 Stored, cataloguei como reflected, mas foi errado, perdão pelo erro.
</p>

<p align="justify">
<h3>Self-XSS</h3>
Falando sobre os <b>Self-XSS</b>, eram basicamente em locais onde a entrada do usuário era refletida diretamente no código sem nenhum tipo de sanitização.
</p>

<p align="justify">
O primeiro local que encontrei o Self-XSS foi na página de recuperação de login. Nessa página era pedido o e-mail do usuário no qual era refletido no código da página. Aqui era basicamente inserir um payload javascript qualquer que ele seria executado :)
</p>

<p align="justify">
O segundo local que encontrei foi na página de recuperação do código de confirmação e funciona da mesma forma que expliquei anteriormente.
</p>

<p align="justify">
O terceiro local foi na página de consulta da situação de determinado bolsista, nessa parte a única coisa que muda entre os exemplos anteriores é o campo, pois agora o payload javascript deve ser injetado no campo de matrícula.
</p>

<p align="justify">
<h3>XSS Stored</h3>
</p>

<p align="justify">
Em relação aos XSS Stored, um deles (XSS stored no fórum do curso) me permitia fazer o hijacking de sessão, ou seja, eu podia capturar a sessão de outros usuários, era necessário apenas que ele acessasse a postagem no fórum do curso.
</p>

<p align="justify">
O primeiro XSS Stored se encontra no sistema de e-mail (ele ataca novamente). Ao enviar uma mensagem com conteúdo javascript ele é interpretado e executado no browser do usuário receptor da mensagem.
</p>

<p align="justify">
O segundo estava no fórum do curso. Ao cadastrar um novo tópico no fórum com um título contendo o payload javascript, ao abrir-lo, o browser do usuário executaria o código contido no título do tópico. Esse XSS me permitia realizar o roubo de sessões de outros usuários. O vídeo da PoC se encontra abaixo:
</p>
<iframe width="560" height="315" src="https://www.youtube.com/embed/TiH67yflOj8" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

<p align="justify">
<h2>SQL Injection</h2>
Agora pra fechar com chave de ouro, vamos falar sobre as falhas de <b>SQL Injection!!</b>
</p>

<p align="justify">
Encontrei 3 falhas de SQL Injection, todas via POST. 
</p>

<p align="justify">
Abordarei apenas sobre 1 delas para que o post não fique mais longo. As outras você encontra no relatório :)
</p>

<p align="justify">
Se você ainda não conhece o que é SQL Injection, aqui está um breve comentário sobre:
</p>

<p align="justify">
"Um ataque de injeção SQL consiste em inserção ou "injeção" de uma consulta SQL por meio dos dados de entrada do cliente para o aplicativo. Uma exploração de injeção SQL bem-sucedida pode ler dados confidenciais do banco de dados, modificar dados do banco de dados (Inserir / Atualizar / Excluir), executar operações de administração no banco de dados (como desligar o DBMS), recuperar o conteúdo de um determinado arquivo presente no arquivo DBMS sistema e, em alguns casos, emitir comandos para o sistema operacional. Ataques de injeção SQL são um tipo de ataque de injeção, no qual comandos SQL são injetados na entrada do plano de dados para efetuar a execução de comandos SQL predefinidos."
</p>

<p align="justify">
Para encontrar essa falha, eu naveguei até a página de busca por docentes e decidi injetar um simples <b><i>';</i></b> no campo dos docentes e recebi uma mensagem de erro interno com uma página personalizada. Decidi tentar vários ataques para vários DBMS, foi então que consegui que o servidor "dormisse" por um tempo utilizando o payload: <b><i>rapt00r’; SELECT PG_SLEEP(5)--+-</i></b>
</p>

<p align="justify">
Foi então que vi que realmente estava vulnerável, então decidi capturar a requisição e utilizei a ferramenta para testar se realmente estava vulnerável, e booommm!!!! Tive acesso ao banco de dados da UFPB. Infelizmente não consegui capturar imagens do banco de dados da UFPB, mas utilizei a UFC como alvo para o vídeo da PoC, que pode ser encontrado abaixo:
</p>
<iframe width="560" height="315" src="https://www.youtube.com/embed/j_kjhnk7CuA" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
<br>

<p align="justify">
Bom pessoal, é isso. Espero que este relatório possa ajudar todos aqueles que gostam da área de Segurança da Informação. Espero que possa servir como base para trabalhos futuros e que possa ser utilizado para aprendizado. Irei utilizar as falhas mais graves para tentar ganhar algumas CVE's e poder concretizar ainda mais minha pesquisa.
</p>

<p align="justify">
<b>Lembre-se seja ético, tenha princípios.</b>
</p>

<p align="justify">
Para finalizar quero deixar claro que nenhum dado foi vazado ou repassado para outras pessoas e o relatório está disponível apenas a partir da data de hoje (31/01/2019).
</p>

<p align="justify">
<b>Como prometido, o relatório poderá ser baixado através do link: <a href="https://drive.google.com/file/d/1ZpQX8tLkupvU4Q7m4qSvbTTjK2e00rAp/view?usp=sharing" target="_blank">Sigaa - Relatório Completo</a></b>
</p>