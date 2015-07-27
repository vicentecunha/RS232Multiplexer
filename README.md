# RS232 Multiplexer
Vicente Cunha, julho de 2015

## Descrição Geral
Este multiplexador foi projetado a fim de que determinado datalogger com apenas uma porta de comunicação RS232 seja capaz adquirir dados de múltiplos sensores RS232.
A multiplexação e a demultiplexação dos canais são realizadas em nível lógico TTL,
sendo utilizados drivers duais EIA-232 (MAX232) para a conversão dos níveis de tensão.

### Conexões Externas
Alimentação: 5V - GND
Canais a serem multiplexados: 22 canais RS232 (0 - 21) de três conexões cada (RX, TX e GND)
Canal de "destino": 1 canal RS232 de três conexões (RX, TX e GND)
Seleção de canais: 5 sinais para seleção de canal (SEL4-SEL0)

#### Detalhamento
O multiplexador pode ser separado em três partes funcionais principais:
- Conversão de níveis de tensão EIA-232/TTL, utilizando MAX232;
- Multiplexação de TX dos canais a serem multiplexados para o RX do canal destino, utilizando 74251;
- Demultiplexação de TX do canal destino aos canais a serem multiplexados utilizando 74156.

Segue o detalhamento para cada uma destas partes, em respectiva ordem.

##### MAX232, driver dual EIA-232
Responsável pela conversão dos níveis de tensão do padrão EIA-232 para o padrão TTL.

As especificações de tensão para EIA-232 são superiores e mais "tolerantes" que as especificações para TTL,
sendo comum encontrar dispositivos com TX do RS232 em 'HIGH' em torno de -7 Volts.
O MAX232 é alimentado por somente 5V e GND;
a fim de fornecer tensões maiores que a alimentação e de polaridade reversa,
MAX232 utiliza "charge pumps" internos e quatro capacitores de 1uF externos.

Cada MAX232 utilizado é capaz converter níveis de tensão RX e TX para dois canais.
Este projeto utiliza portanto 12 MAX232 ao todo,
[22 canais]/2 para os canais a serem multiplexados e 1 para o canal de destino.

##### 74251, multiplexador 8:1
Realiza a multiplexação dos sinais de TX dos canais para o RX do canal de destino.

Cada 74251 é capaz de multiplexar apenas 8 sinais;
a fim de multiplexar 22 canais, conectam-se os sinais TX às entradas de três 74251,
estando os três com sinais de seleção menos significativos (SEL2-SEL0).
Um quarto 74251 então multiplexa a saída destes três com sinais de seleção mais significativos (SEL4-SEL3).
Desta forma, uma espécie de "árvore" de multiplexadores é formada,
e cada canal pode ser selecionado individualmente por sua respectiva sequência binária (SEL4-SEL0).

##### 74156, demultiplexador 1:8
Realiza a demultiplexação dos sinais de TX do canal de destino para o RX dos demais canais.
O 74156 é na verdade composto por um par de demultiplexadores 1:4,
mas unindo entre si as conexões de entrada (G1 e G2) e os sinais de seleção mais significativos (C1 e C2),
obtém-se um demultiplexador 1:8.

A exemplo da parte multiplexadora, a parte demultiplexadora também é conectada em "árvore" para atender 22 canais.
Para que os sinais multiplexados e demultiplexados sejam correspondentes a um mesmo canal,
os sinais de seleção (SEL4 - SEL0) são os mesmo utilizados em ambas as partes.
