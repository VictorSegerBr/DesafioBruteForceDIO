# Desafio Brute Force DIO
Este repositório é referente ao desafio proposto de técnicas de Brute force utilizando a ferramenta **Medusa** no Kali Linux

## Configurações Iniciais
O primeiro passo foi criar duas máquinas virtuais, uma com o sistema operacional **Metasploitable2** e outra com **Kali Linux**.<br>
O sistema Metasploitable2 é um servidor linux propositalmente defeituoso e vulnerável e nosso objetivo será invadi-lo com diferentes técnicas de Brute Force.<br>
<div align="center">Configuração da máquina Metasploitable2<br>
<img width="500" height="500" alt="image" src="https://github.com/user-attachments/assets/4bd4d084-33e4-4a98-96a0-2abb288b1e68" /></div><br>
O Kali Linux é uma distribuição Linux focada em cibersegurança e já vem com várias ferramentas úteis integradas, incluindo o Medusa.<br><br>
<div align="center">Configuração da máquina Kali Linux<br>
<img width="500" height="500" alt="image" src="https://github.com/user-attachments/assets/972f59e8-0952-46b9-b131-0cae9f78e84b" /></div><br>
Ambas máquinas estão configuradas na mesma rede privada, com o adaptador de rede do VirtualBox configurado para Host-Only.<br>
Para fins desta demonstração descobrimos o IP da máquina a ser invadida acessando ela nós mesmos com as credenciais padrão e utilizando o comando linux <code>ip a</code><br>
<div align="center">Tela do servidor Metasploitable<br>
<img width="500" height="250" alt="image" src="https://github.com/user-attachments/assets/dfe666da-b83b-48b0-8331-48699da34cbd" /></div><br>
Com isso descobrimos que o IP da máquina alvo na nossa rede interna é <code>192.168.56.101</code>. Esse endereço pode mudar dependendo da configuração de hardware e software de cada um. Numa situação mais parecida com a realidade, apenas a fim de exemplificação, IPs de máquinas podem ser descobertos publicamente com a ferramenta <a href="https://www.shodan.io/">Shodan</a>.

## Força Bruta em Porta FTP 
Nosso primeiro objetivo do desafio será tentar acesso ao servidor por meio de alguma porta aberta da máquina.<br>
Com a finalidade de identificar tais vulnerabilidade no alvo, utilizamos um comando de outra ferramenta do Kali Linux, o **Nmap**. Tal ferramenta consegue identificar na máquina alvo se as portas de conexão externa estão abertas ou fechadas, a versão dos serviços utilizados e outras funções. Para isso utilizaremos o comando <code>nmap -sV -p 21,22,80,445,139 192.168.56.101</code>. Esse comando retorna detalhes sobre as portas relativas aos serviços de ftp, ssh, http e samba.<br>
<div align="center">Comando e resposta do Nmap<br>
<img width="500" height="250" alt="image" src="https://github.com/user-attachments/assets/873dba99-8a7c-4303-b1ba-1d7aa2dfb300" /></div><br>
Podemos identificar que várias portas estão abertas mas nessa demonstração nossa escolha será o serviço **FTP**.<br>
<br>
Para continuar nossa demonstração agora precisamos de duas listas. Uma com possíveis nomes de usuários e outra com senhas. Em situações hipotéticas tais listas podem ser geradas ou adquiridas de diferentes formas. Por meio de vazamentos de dados seria uma destas maneiras. No nosso desafio criaremos listas simples com 4 ou 5 itens cada. As listas foram geradas em arquivos <code>.txt</code> e cada tentativa deve estar em uma respectiva linha. Você poderá acessar a lista de <a href="/pass.txt">Senhas<a> e de <a href="/users.txt">Usuários<a> clicando nesses links ou na pasta raiz deste diretório.<br>
<br>
Agora temos todas as informações que precisamos para executar o comando usando a ferramenta <strong>Medusa</strong>. Com isso podemos usar o seguinte comando no terminal do Kali Linux:<br>
<br>
<div align="center"><code>medusa -h 192.168.56.101 -U users.txt -P pass.txt -M ftp -t 6
</code></div><br>
Os argumentos desse comando são os seguintes:<br>
<div align="center">-h # indica o IP alvo<br>
	-U # arquivo de usuários<br>
	-P # arquivo de senhas<br>
	-M # tipo de ataque<br>
	-t # quantidade de threads simultâneas
</div><br>
Ao rodar o comando podemos observar o seguinte resultado:<br>
<br>
<div align="center">
  Resultado do comando Medusa<br>
  <img width="689" height="717" alt="image" src="https://github.com/user-attachments/assets/b42b269e-eb2f-4160-8f95-9e5769b96e58" />
</div><br>
Destacado na imagem podemos ver que o software identificou que um conjunto de credenciais nome de usuário e senha válidos. Essas credenciais estavam como opção nos nossos arquivos e correspondem ao login e senha padrão do Masploitable2 (msfadmin). Podemos também, no terminal, usar o comando <code>ftp</code> no IP alvo e comprovar a validade das credenciais.<br>
<br>
<div align="center">Aceso remoto à maquina alvo usando ftp<br>
<img width="309" height="184" alt="image" src="https://github.com/user-attachments/assets/31ce0fe9-7148-42aa-beb2-136e05700d9b" /></div><br>
E com isso completamos o procedimento de Força Bruta de senhas utilizando o software Medusa e explorando vulnerabilidade em porta FTP.

## Força Bruta em formulário WEB
Agora que executamos um ataque do tipo força bruta em um servidor, vamos ver como ele pode ser aplicado em formulários de páginas da web. Para isso vamos precisar de uma página de teste. Por sorte o Metasploitable2 possui uma funcionalidade para nos ajudar. Para isso é necessário rodar as mesmas maquinas virtuais configuradas no <a href="#força-bruta-em-porta-ftp">passo anterior</a>. Utilizando o navegador Mozilla do Kali Linux vamos acessar o seguinte endereço: <code>http://192.168.56.101/dvwa/login.php</code>. Neste endereço é possível observar a página <strong>Damn Vulnerable Web Application (DVWA)</strong>. Essa página foi propositalmente desenvolvida para apresentar vulnerabilidades que podem ser exploradas para aprendizado na área de cibersegurança.<br>
<div align="center">Página Inicial do DVWA<br>
<img width="1275" height="793" alt="image" src="https://github.com/user-attachments/assets/d09e5358-d589-4399-b499-791819703608" /></div><br>
Precisamos entender como os dados informados no formulário são enviados, para isso usamos o Devtools. Colocamos quaisquer dados nos campos Username e Password e enviamos, apenas para termos certeza de como os dados são comunicados na requisição.<br>
<br>
<div align="center">Inspecionando a página<br>
<img width="1275" height="790" alt="image" src="https://github.com/user-attachments/assets/7e2f3f47-f7fc-4aef-adab-606b4176ac81" /></div><br>
Podemos observar que são enviados três parametros na requisição POST, isso será importante para o comando medusa posteriormente.



