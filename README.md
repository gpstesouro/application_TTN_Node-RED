<a name="ancora"></a>
# application_TTN_Node-RED
Explorando APIs e nodes TTN
***
<a id="ancora1"></a>
## Antiga Versão - v0 (caso já conheça o contexto, sugiro saltar para [Nova Versão](#ancora2))

A boa dica desta vez será um fluxo para monitorar falhas de gateways (Gateway Management). Na verdade, nada foi inventado, pois este trabalho foi inspirado e adaptado da comunidade TTN de Barcelona (https://tinkerman.cat/post/monitor-your-ttn-gateways-with-node-red). 

A ideia se baseia em utilizar algumas APIs, meio que perdidas no “limbo”, para checar o status (on-line ou off-line) dos gateways numa determinada região e, melhor ainda, ser alertado por meio do envio de mensagem - aplicativo Telegram - quando houver alguma alteração de status.

Bem, faz-se necessário saber mexer um pouco com o Node-RED, descobrir o ID dos gateways-alvos de interesse e configurar apropriadamente o Node "telegram sender". Simplesmente, importe o código [gw-management.json](https://github.com/Mario-Camara/application_TTN_Node-RED/blob/main/gw-management.json) para o seu Node-RED e faça as adaptações necessárias para sua rede, checando cada etapa do fluxo com o Node "debug".

![tela Node-RED](https://github.com/Mario-Camara/application_TTN_Node-RED/blob/main/tela_gw-management.jpg)

***
<a id="ancora2"></a>
## Nova Versão (v1)
Bene, bene, bene ... o mundo sempre girando! 

Houve na TTN uma significativa mudança, eu diria não só de versão da *_stack_* do servidor de rede LoRaWAN mas também conceitual sob vários aspectos. Uma destas mudanças impactou as antigas API correlatas ao status dos gateways, que foram utilizadas na descrição acima. Eis um trecho retirado da fala do próprio [Johan Stokking](https://www.thethingsnetwork.org/forum/t/new-api-for-gateway-mapping-status-and-info/49778): 

>I’m happy to announce that we have a new API for fetching gateway locations, online status and other info for The Things Stack Community Edition, Cloud, Open Source and Enterprise: **Packet Broker Mapper**. Since we wanted to make gateway information available in one place, we built a new service for this in Packet Broker. With Packet Broker being the backbone for LoRaWAN traffic, making gateway information available in a central place allows for better insight in the entire ecosystem and discovery of (private) networks to exchange traffic with.
>
>Backwards compatibility
>
>This replaces two existing APIs: the https://www.thethingsnetwork.org/gateway-data/ endpoint and the NOC (part of V2). The former now uses the Packet Broker Mapper API as data source, so applications using the old API now receive up-to-date information (including our very own www.thethingsnetwork.org gateway map as we work on a redesign of the homepage). Please update your API clients to consume the new API, as we will be shutting down the "/gateway-data" endpoint by the end of 2021. The NOC is already gone.

...
Pois então, a ideia é recuperar a aplicação acima (gerenciamento dos GW regionais) utilizando-se da nova API disponibilizada. Exemplos: (i) [todos os gateways num raio de 50 Km de Caxias do Sul](https://mapper.packetbroker.net/api/v2/gateways?distanceWithin%5Blatitude%5D=-29.167778&distanceWithin%5Blongitude%5D=-51.178889&distanceWithin%5Bdistance%5D=50000&netID=000013&tenantID=ttn); (ii) [todos os gateways - globais - online e conectados a TTN V2 (rede em obsolescência)](https://mapper.packetbroker.net/api/v2/gateways?online=true&netID=000013&tenantID=ttnv2); e (iii) [consulta as características de um gateway individual "id=gw-gpstesouro"](https://mapper.packetbroker.net/api/v2/gateways/netID=000013,tenantID=ttn,id=gw-gpstesouro).

👷‍♂️
...

***
<a id="ancora3"></a>
## Outra possibilidade
Agora, caso deseje uma solução mais profissional - com o requinte de um *_dashboard_* - não deixe de visitar o seguinte [LINK](https://github.com/JohanScheepers/TTN_Gateway_Node).

Olha que belezura! E já testei ... FUNCIONA!

![TTN Gateway Radius and New Node](https://www.thethingsnetwork.org/forum/uploads/default/original/3X/f/b/fb37473479b621cf7092e58ed1209e8338f325c6.jpeg)
