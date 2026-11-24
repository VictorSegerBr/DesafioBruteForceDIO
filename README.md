# Desafio Brute Force DIO
Este repositório é referente ao desafio proposto de técnicas de Brute force utilizando a ferramenta **Medusa** do Kali Linux

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
COm isso descobrimos que o IP da máquina alvo na nossa rede interna é <code>192.168.56.101</code>Numa situação mais parecida com a realidade, apenas a fim de exemplificação, IPs de máquinas podem ser descobertos publicamente com a ferramenta <a href="https://www.shodan.io/">Shodan</a>.

## Força Bruta em Porta FTP 
Nosso primeiro objetivo do desafio será tentar acesso ao servidor por meio de alguma porta aberta da máquina.<br>
Com a finalidade de identificar tais vulnerabilidade no alvo, utilizamos um comando de outra ferramenta do Kali Linux, o **Nmap**. Tal ferramenta consegue identificar na máquina alvo se as portas de conexão externa estão abertas ou fechadas, a versão dos serviços utilizados e outras funções. Para isso utilizaremos o comando <code>nmap -sV -p 21,22,80,445,139 192.168.56.101</code>. Esse comando retorna detalhes sobre as portas relativas aos serviços de ftp, ssh, http e samba.<br>
<div align="center">Comando e resposta do Nmap<br>
<img width="500" height="250" alt="image" src="https://github.com/user-attachments/assets/873dba99-8a7c-4303-b1ba-1d7aa2dfb300" /></div><br>
Podemos identificar que várias portas estão abertas mas nessa demonstração nossa escolha será o serviço **FTP**.<br>
<br>
Para continuar nossa demonstração agora precisamos de duas listas. Uma com possíveis nomes de usuários e outra com senhas. Em situações hipotéticas tais listas podem ser geradas ou adquiridas de diferentes formas. Por meio de vazamentos de dados seria uma destas maneiras. No nosso desafio criaremos listas simples com 4 ou 5 itens cada. As listas foram geradas em arquivos <code>.txt</code> e cada tentativa deve estar em uma respectiva linha, você poderá acessar essa listas em [pass.txt](

