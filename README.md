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

1. Encontre o primeiro datagrama IP contendo a primeira parte do segmento enviado para 128.119.245.12 enviado pelo seu computador através do comando traceroute para gaia.cs.umass.edu (aplique um filtro no wireshark com `ip.dst == 128.119.245.12`), depois de especificar que o comprimento do pacote traceroute deve ser 3000. (Dica : Este é o pacote 179 no arquivo de rastreamento ip-wireshark-trace1-1.pcapng. Os pacotes 179, 180 e 181 são três datagramas IP criados pela fragmentação do primeiro segmento UDP de 3.000 bytes enviado para 128.119.145.12). Esse segmento foi fragmentado em mais de um datagrama IP? (Dica: a resposta é sim!)
2. Quais informações no cabeçalho IP indicam que este datagrama foi fragmentado?
3. Quais informações no cabeçalho IP deste pacote indicam se este é o primeiro fragmento versus o último fragmento?
4. Quantos bytes existem neste datagrama IP (cabeçalho mais carga útil)?
5. Agora inspecione o datagrama que contém o segundo fragmento do segmento UDP fragmentado. Quais informações no cabeçalho IP indicam que este não é o primeiro fragmento de datagrama?
6. Quais campos mudam no cabeçalho IP entre o primeiro e o segundo fragmento?
7. Agora encontre o datagrama IP contendo o terceiro fragmento do segmento UDP original. Quais informações no cabeçalho IP indicam que este é o último fragmento desse segmento?

***

## Dynamic Host Configuration Protocol
