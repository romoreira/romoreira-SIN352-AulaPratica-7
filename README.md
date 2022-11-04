# SIN352 Aula Pratica 7

## Fragmentação na Camada IP

Nesta seção, veremos o envio de um segmento UDP grande (3.000 bytes) sendo enviado pelo programa `traceroute` que é fragmentado em vários datagramas IP.

Inicialmente, faça as consigurações no ambiente para utilizarmos o `traceroute` (Linux) ou `tracert` (Windows). :computer:

..* Linux/MacOS. Com o comando traceroute do Linux/MacOS, o tamanho do datagrama UDP enviado para o destino final pode ser definido explicitamente indicando o número de bytes no datagrama; esse valor é inserido na linha de comando traceroute imediatamente após o nome ou endereço do destino. Por exemplo, para enviar datagramas traceroute de 3000 bytes para gaia.cs.umass.edu, o comando seria:
```
~$ traceroute gaia.cs.umass.edu 3000
```
..* Windows. O programa tracert fornecido com o Windows não permite alterar o tamanho da mensagem ICMP enviada pelo tracert. Portanto, não será possível usar uma máquina Windows para gerar mensagens ICMP grandes o suficiente para forçar a fragmentação de IP. Se você quiser fazer a segunda parte deste laboratório, você pode baixar um arquivo de rastreamento de pacote que foi capturado previamente. (Arquivo está no Github).

Eventualmente o tracert pode não estar instalado nativametne no seu Linux, por isso faça:

* `sudo apt-get update`
* `sudo apt-get install inetutils-traceroute`

Faça o seguinte:

* Inicie o Wireshark e comece a captura de pacotes. (Capturar->Iniciar ou clicar no botão de barbatana de tubarão azul no canto superior esquerdo da janela do Wireshark).
* Digite digite o comando `traceroute gaia.cs.umass.edu 3000`.

Responda: :notebook:

1. Encontre o primeiro datagrama IP contendo a primeira parte do segmento enviado para 128.119.245.12 enviado pelo seu computador através do comando traceroute para gaia.cs.umass.edu (aplique um filtro no wireshark com `ip.dst == 128.119.245.12`), depois de especificar que o comprimento do pacote traceroute deve ser 3000. (Dica : Este é o pacote 179 no arquivo de rastreamento [ip-wireshark-trace1-1.pcapng]([other_file.md](https://github.com/romoreira/romoreira-SIN352-AulaPratica-7/blob/main/ip-wireshark-trace1-1.pcapng)). Os pacotes 179, 180 e 181 são três datagramas IP criados pela fragmentação do primeiro segmento UDP de 3.000 bytes enviado para 128.119.145.12). Esse segmento foi fragmentado em mais de um datagrama IP? (Dica: a resposta é sim!)
2. Quais informações no cabeçalho IP indicam que este datagrama foi fragmentado?
3. Quais informações no cabeçalho IP deste pacote indicam se este é o primeiro fragmento versus o último fragmento?
4. Quantos bytes existem neste datagrama IP (cabeçalho mais carga útil)?
5. Agora inspecione o datagrama que contém o segundo fragmento do segmento UDP fragmentado. Quais informações no cabeçalho IP indicam que este não é o primeiro fragmento de datagrama?
6. Quais campos mudam no cabeçalho IP entre o primeiro e o segundo fragmento?
7. Agora encontre o datagrama IP contendo o terceiro fragmento do segmento UDP original. Quais informações no cabeçalho IP indicam que este é o último fragmento desse segmento?

***

## Dynamic Host Configuration Protocol (DHCP)

Nesta parte, veremos o protocolo de configuração de host dinâmico, DHCP. Lembre-se de que o DHCP é usado extensivamente em LANs corporativas, universitárias e domésticas com fio e sem fio para atribuir dinamicamente endereços IP a hosts, bem como para configurar outras informações de configuração de rede.

As duas primeiras etapas do protocolo DHCP na Figura 4.24 (usando as mensagens Discover e Offer) são opcionais (no sentido de que nem sempre precisam ser usadas quando, por exemplo, um novo endereço IP é necessário ou um endereço DHCP existente é ser renovado); as mensagens Request e ACK não são. Para coletar uma captura Wireshark que conterá todos os quatro tipos de mensagem DHCP, precisaremos realizar algumas ações de linha de comando em no Linux.


Em uma máquina Linux:
..* Em uma janela/shell de terminal, digite os seguintes comandos:
```
sudo ip addr flush eth0
sudo dhclient -r
```
onde eth0 (neste exemplo) é a interface na qual você deseja capturar pacotes usando o Wireshark. Você pode encontrar facilmente a lista de nomes de interface no Wireshark escolhendo Capture -> Options. Este comando removerá o endereço IP existente da interface e liberará quaisquer concessões de endereço DHCP existentes.
2. Inicie o Wireshark, capturando pacotes na interface que você desconfigura na Etapa 1.
3. Na janela/shell do terminal, digite o seguinte comando:
```
sudo dhclient eth0
```
onde, como acima, eth0 é a interface na qual você está atualmente capturando pacotes. Isso fará com que o protocolo DHCP solicite e receba um endereço IP e outras informações do servidor DHCP.
4. Após aguardar alguns segundos, pare a captura do Wireshark.


Se você não conseguir executar o Wireshark em uma conexão de rede ativa, não conseguir capturar todas as quatro mensagens DHCP ou for designado para isso, você pode usar o arquivo de rastreamento do Wireshark, [dhcp-wireshark-trace1-1.pcapng](https://github.com/romoreira/romoreira-SIN352-AulaPratica-7/blob/main/dhcp-wireshark-trace1-1.pcapng) (anexada à este repositório). Você pode também baixar essa captura, mesmo se tiver capturado seu próprio rastreamento e usá-lo, bem como seu próprio rastreamento, enquanto explora as perguntas abaixo.

Vamos começar examinando a mensagem DHCP Discover. Localize o datagrama IP que contém a primeira mensagem Discover em seu rastreamento.

1. Esta mensagem DHCP Discover é enviada usando UDP ou TCP como protocolo de transporte subjacente?
2. Qual é o endereço IP de origem usado no datagrama IP que contém a mensagem Discover? Há algo de especial neste endereço? Explique.
3. Qual é o endereço IP de destino usado no datagrama que contém a mensagem Discover. Há algo de especial neste endereço? Explique.
4. Qual é o valor no campo ID da transação desta mensagem DHCP Discover?
5. Agora inspecione o campo de opções na mensagem DHCP Discover. Quais são as cinco informações (além de um endereço IP) que o cliente está sugerindo ou solicitando para receber do servidor DHCP como parte dessa transação DHCP?

Agora vamos ver a mensagem DHCP Offer. Localize o datagrama IP que contém a mensagem DHCP Offer em seu rastreamento que foi enviado por um servidor DHCP na resposta à mensagem DHCP Discover que você estudou nas perguntas 1-5 acima.

6. Como você sabe que esta mensagem de Oferta está sendo enviada em resposta à mensagem DHCP Discover que você estudou nas questões 1-5 acima?
7. Qual é o endereço IP de origem usado no datagrama IP que contém a mensagem de Oferta? Há algo de especial neste endereço? Explique.
8. Qual é o endereço IP de destino utilizado no datagrama que contém a mensagem de Oferta? Há algo de especial neste endereço? Explique. 
9. Agora inspecione o campo de opções na mensagem DHCP Offer. Quais são as cinco informações que o servidor DHCP está fornecendo ao cliente DHCP na mensagem DHCP Offer?

Parece que, uma vez recebida a mensagem de oferta de DHCP, o cliente pode ter todas as informações necessárias para prosseguir. No entanto, o cliente pode ter recebido OFERTAS de vários servidores DHCP e, portanto, é necessária uma segunda fase, com mais duas mensagens obrigatórias – a mensagem DHCP Request cliente-servidor e a mensagem DHCP ACK servidor-cliente é necessária. Mas pelo menos o cliente sabe que existe pelo menos um servidor DHCP por aí! 

Vamos dar uma olhada na mensagem DHCP Request, lembrando que embora já tenhamos visto uma mensagem Discover em nossa captura, nem sempre é o caso quando uma mensagem de solicitação DHCP é enviada.

Localize o datagrama IP que contém a primeira mensagem de solicitação de DHCP em seu rastreamento e responda às perguntas a seguir.

10. Qual é o número da porta de origem UDP no datagrama IP que contém a primeira mensagem de solicitação de DHCP em seu rastreamento? Qual é o número da porta de destino UDP que está sendo usado?
11. Qual é o endereço IP de origem no datagrama IP que contém esta mensagem de solicitação? Há algo de especial neste endereço? Explique.
12. Qual é o endereço IP de destino usado no datagrama que contém esta mensagem de solicitação. Há algo de especial neste endereço? Explique.
13. Qual é o valor no campo ID da transação desta mensagem de solicitação de DHCP? Ele corresponde aos IDs de transação das mensagens anteriores de descoberta e oferta?
14. Agora inspecione o campo de opções na mensagem DHCP Discover e dê uma olhada na “Lista de Solicitações de Parâmetros”. O DHCP RFC observa que
“O cliente pode informar ao servidor quais parâmetros de configuração o cliente está interessado incluindo a opção 'lista de solicitações de parâmetros'. A parte de dados desta opção lista explicitamente as opções solicitadas pelo número da etiqueta.”
Que diferenças você vê entre as entradas na opção 'lista de solicitações de parâmetros' nesta mensagem de solicitação e a mesma opção de lista na mensagem anterior de descoberta?

Localize o datagrama IP que contém a primeira mensagem DHCP ACK em seu rastreamento e responda às seguintes perguntas.

15. Qual é o endereço IP de origem no datagrama IP que contém esta mensagem ACK? Há algo de especial neste endereço? Explique.
16. Qual é o endereço IP de destino usado no datagrama que contém esta mensagem ACK. Há algo de especial neste endereço? Explique.
17. Qual é o nome do campo na mensagem DHCP ACK (como indicado na janela Wireshark) que contém o endereço IP do cliente atribuído?
18. Há quanto tempo (o chamado “lease time”) o servidor DHPC atribui esse endereço IP ao cliente?
19. Qual é o endereço IP (retornado pelo servidor DHCP ao cliente DHCP nesta mensagem DHCP ACK) do roteador de primeiro salto no caminho padrão do cliente para o resto da Internet?

