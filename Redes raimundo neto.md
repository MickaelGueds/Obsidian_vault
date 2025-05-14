Camada de rede transporte e aplicação vai cair:

### camada de Redes
O que devo saber da camada de redes do protocolos tcp ip e osi?:

Roteamento de pacote, e encaminhamento e endereçamento logico, ipv4 e ipv6 resolução de endereço mac e etc com protocolos de roteamento,

 tem roteamento e como funciona, dentro do roteador ele recebe pacotes e verifica o enderço de destino, consulta a tabela e encaminha o pacote,no caso seria roteamento estatico e dinamico

Claro temos o cidr a mascara o endereçamento da rede e o broadcast aqui tambem 

### Camada de transporte

Camada depois da camada de rede e antes da aplicação, a função é controlar o erro e o fluxo, segmentação e remontagem de dados;


Existem as portas logicas, e os protocolos de transporte que seriam o:
Tcp: confivavel orentavel a conexão, usados com http.https,ftp,smtp

Udp: Não é confiavel, sem conexão porem amis rapido que o tcp, é usado em dns voip streaming games e etc...nao tem controle de erro aplicações em tempo real onde velocidade é mais importante, pacotes podem se perder e etc

multiplexação, muitos aplicativo usam a rede nas portas, aqui eles enviam os pedidos pela mesma rede

Demultiplexação dados sao entregues a apliacação correta com base na porta de destino, ou seja aqui recebm os pedidos

### Camadas de aplicação
Camada mais alta, iteração entre programas e serviços, interação entre software e rede, define formatos de dados e codificações, autenticação e segurança 

http e https navegação web onde o https temo tls que é para criptografar conexões

Dns que traduz nomes de dominio para o endereço ip 

smtp: envio de emails

pop3 e imap: recebe emails

ftp e sftp é orotocolo de tranferencia de arquivo

dhcp é endereçamento dinamico de ip 

aqui suportam modelos de aplicação que seriam a cliente servidor e as p2p

cliente servidor é: cliente solicita os dados servidor responde 

p2p é que o dispositivo é ao mesmo tempo cliente e ao mesmo tempo servidor como BitTorrente blockchain e etc

em relaçao a segurança temos o https e sftp que protege contra ataques xxss injeções sql e phishing