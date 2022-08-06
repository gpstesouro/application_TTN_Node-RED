<a name="ancora"></a>
# application_TTN_Node-RED
Explorando APIs e monitorando gateways TTN (The Thing Network)
***
<a id="ancora1"></a>
## Antiga Versão - v0 (caso já conheça o contexto, sugiro saltar para [Nova Versão](#ancora2))

A boa dica desta vez será um fluxo para monitorar falhas de gateways (Gateway Management). Na verdade, nada foi inventado, pois este trabalho foi inspirado e adaptado da comunidade TTN de Barcelona (https://tinkerman.cat/post/monitor-your-ttn-gateways-with-node-red).  Mas afinal, o que é TTN? Por favor, acesse o [link](https://www.thethingsnetwork.org/).

Mas em uma linha: **TTN é um rede voltada para IoT que emprega a tecnologia LoRa&LoRaWAN para atender aos quesitos de conectividade à longa distância (5 km variável) e _low power_**.

A ideia se baseia em utilizar algumas APIs, meio que perdidas no “limbo”, para checar o _status_ (_online_ ou _offline_) dos gateways numa determinada região e, melhor ainda, ser alertado por meio do envio de mensagem - aplicativo Telegram - quando houver alguma alteração de _status_.

Bem, faz-se necessário saber lidar um pouco com o Node-RED, descobrir o ID dos gateways-alvos de interesse e configurar apropriadamente o Node "telegram sender". Simplesmente, importe o código [gw-management-v0.json](https://github.com/Mario-Camara/application_TTN_Node-RED/blob/main/gw-management-v0.json) (descontinuado e substituído pela nova versão no item abaixo) para o seu Node-RED e faça as adaptações necessárias para sua rede, checando cada etapa do fluxo com o Node "debug".

![tela Node-RED](https://github.com/Mario-Camara/application_TTN_Node-RED/blob/main/tela_gw-management.jpg)

***
<a id="ancora2"></a>
## Nova Versão (v1)
_Bene, bene, bene_ ... o mundo sempre girando! 

Houve na TTN uma significativa mudança, eu diria não só de versão da *_stack_* do servidor de rede LoRaWAN mas também conceitual sob vários aspectos. Uma destas mudanças impactou as antigas API correlatas ao _status_ dos gateways, que foram utilizadas na descrição acima. Eis um trecho retirado da fala do próprio [Johan Stokking](https://www.thethingsnetwork.org/forum/t/new-api-for-gateway-mapping-status-and-info/49778): 

>I’m happy to announce that we have a new API for fetching gateway locations, online status and other info for The Things Stack Community Edition, Cloud, Open Source and Enterprise: **Packet Broker Mapper API**. Since we wanted to make gateway information available in one place, we built a new service for this in Packet Broker. With Packet Broker being the backbone for LoRaWAN traffic, making gateway information available in a central place allows for better insight in the entire ecosystem and discovery of (private) networks to exchange traffic with.
>
>Backwards compatibility
>
>This replaces two existing APIs: the [https://www.thethingsnetwork.org/gateway-data](https://www.thethingsnetwork.org/gateway-data/country/br) endpoint and the NOC (part of V2). The former now uses the Packet Broker Mapper API as data source, so applications using the old API now receive up-to-date information (including our very own www.thethingsnetwork.org gateway map as we work on a redesign of the homepage). Please update your API clients to consume the new API, as we will be shutting down the "/gateway-data" endpoint by the end of 2021. The NOC is already gone.


Pois então, a ideia é recuperar a aplicação antiga (gerenciamento dos GW regionais) utilizando-se da nova API (_Packet Broker Mapper_) disponibilizada. 

Exemplos de emprego da API: 
- todos os gateways num raio de 50 Km de Caxias do Sul - RS: [https://mapper.packetbroker.net/api/v2/gateways?distanceWithin[latitude]=-29.167778&distanceWithin[longitude]=-51.178889&distanceWithin[distance]=50000&netID=000013&tenantID=ttn](https://mapper.packetbroker.net/api/v2/gateways?distanceWithin%5Blatitude%5D=-29.167778&distanceWithin%5Blongitude%5D=-51.178889&distanceWithin%5Bdistance%5D=50000&netID=000013&tenantID=ttn); 
- todos os gateways - globais - online e conectados a TTN V2 (rede em obsolescência): [https://mapper.packetbroker.net/api/v2/gateways?online=true&netID=000013&tenantID=ttnv2](https://mapper.packetbroker.net/api/v2/gateways?online=true&netID=000013&tenantID=ttnv2); e 
- consulta as características de um gateway individual "`id`=gw-gpstesouro": [https://mapper.packetbroker.net/api/v2/gateways/netID=000013,tenantID=ttn,id=gw-gpstesouro](https://mapper.packetbroker.net/api/v2/gateways/netID=000013,tenantID=ttn,id=gw-gpstesouro).

> __Observações: (i)__ TTN utiliza `netID` 000013 e `tenantID` (locatário): ttn (V3) and ttnv2 (V2); __(ii)__ como novidade, `rxRate` e `txRate` são taxas médias de tráfego dos últimos 6 minutos e, também, houve a inclusão da _timestamp_ `updatedAt`; __(iii)__ tenha paciência em aguardar as atualizações: o _status_ `online`=_false_ (_offline_), `rxRate` e `txRate` podem demorar um pouco mais de 10 minutos e `online`=_true_ leva cerca de 1 minuto para ser reportado pela API;  __(iv)__ na versão anterior o robô enviava a mensagem para uma conta pessoal, agora amplia o público ao direcionar a mensagem para um grupo Telegram; e __(v)__ ainda estou pensando nisto (talvez a futura v1.1), parece viável implementar um indicativo de SLA (Acordo de Nível de Serviço) por gateway. Depende da adesão à ideia.

![tela Node-RED](https://github.com/Mario-Camara/application_TTN_Node-RED/blob/main/tela_gw-management-v1.jpg?raw=true)

Agora sim ... eis o novo código [gw-management-v1.json](https://github.com/Mario-Camara/application_TTN_Node-RED/blob/main/gw-management-v1.json) a ser importado para o Node-RED, para aqueles interessados em implantar na sua respectiva região.

- Sistemática de funcionamento: um robô por intermédio de API verifica constantemente o _status_ (_online_ ou _offline_) de alguns dos gateways elencados para serem monitorados, havendo alteração dispara uma mensagem utilizando o aplicativo Telegram; e
- ID dos gateways monitorados: [gw-gpstesouro](https://mapper.packetbroker.net/api/v2/gateways/netID=000013,tenantID=ttn,id=gw-gpstesouro), [samuelcamara](https://mapper.packetbroker.net/api/v2/gateways/netID=000013,tenantID=ttn,id=samuelcamara), [samuel](https://mapper.packetbroker.net/api/v2/gateways/netID=000013,tenantID=ttn,id=samuel), [gwtrinopolo03](https://mapper.packetbroker.net/api/v2/gateways/netID=000013,tenantID=ttn,id=gwtrinopolo03), [gwtrinopolo02](https://mapper.packetbroker.net/api/v2/gateways/netID=000013,tenantID=ttn,id=gwtrinopolo02), [gwtrinopolo01](https://mapper.packetbroker.net/api/v2/gateways/netID=000013,tenantID=ttn,id=gwtrinopolo01), [gtw-vortyce-2](https://mapper.packetbroker.net/api/v2/gateways/netID=000013,tenantID=ttn,id=gtw-vortyce-2), [gtw-vortyce-main](https://mapper.packetbroker.net/api/v2/gateways/netID=000013,tenantID=ttn,id=gtw-vortyce-main), [5813d30c0370](https://mapper.packetbroker.net/api/v2/gateways/netID=000013,tenantID=ttn,id=5813d30c0370), [farroupilha](https://mapper.packetbroker.net/api/v2/gateways/netID=000013,tenantID=ttn,id=farroupilha), [gw-veranopolis](https://mapper.packetbroker.net/api/v2/gateways/netID=000013,tenantID=ttn,id=gw-veranopolis) e [iffar-gw1](https://mapper.packetbroker.net/api/v2/gateways/netID=000013,tenantID=ttn,id=iffar-gw1). 

😊 GOSTOU DA IDEIA? Pois então, caso queira **observar uma solução já implantada e em produção**, junte-se ao grupo [TTN_Gateways_CXS](https://t.me/ttn_gateways_cxs) no Telegram e seja notificado a cada mudança de estado dos gateways LoRaWAN que integram a rede TTN em Caxias do Sul - RS. Por favor, "a principio" interprete a quietude do robô como favorável, pois poucas mensagens emitidas quer dizer que a rede está estabilizada (sem alternância entre _offline_ <-> _online_). Além disso ... na modelagem das API há um filtro temporal de transiente (evitar reportar flutuações); sendo mais claro, quero dizer que há uma latência de 11 minutos, caso o GW tenha se tornado "_offline_". Já a informação de "_online_" é reportada mais de imediato (1 min). 😊

***
<a id="ancora3"></a>
## Outras possibilidades para verificar a rede TTN:

1. Modo básico e que todos conhecem: [TTN Community](https://www.thethingsnetwork.org/community/caxias-do-sul/); 
2. [Mapa de Gateways](https://www.thethingsnetwork.org/map) oficial da TTN, já via _Packet Brocker Mapper_. Vale a pena observar no mapa a aglomeração de gateways na Europa; e
3. Agora, caso deseje uma solução personalizada e beirando um _designer_ profissional - com o requinte de um *_dashboard_* - não deixe de visitar o seguinte [link](https://github.com/JohanScheepers/TTN_Gateway_Node). Olha que belezura! E já testei ... **FUNCIONA BEM!**

![TTN Gateway Radius and New Node](https://github.com/Mario-Camara/application_TTN_Node-RED/blob/main/monitor-gw_caxiasdosul.jpg?raw=true)

***
<a id="ancora4"></a>
## Para finalizar, um PRÊMIO para quem leu o artigo até o final:

1. Que tal fuçar no mundo _Docker-Conteiner_ em três linhas? É sério! Você não se arrependerá!:muscle: Primeiro acesse o link: [Play with Docker](https://labs.play-with-docker.com/). Você precisa se cadastrar: fazer um LOGIN (e-mail e criar uma senha). Depois START.
2. Clique em + ADD NEW INSTANCE. Agora a parte mais difícil, não tenha medo!:scream: Na linha de comando digite:  `docker container run -d -p 1880:1880 gpstesouro/ttn_gateways_rs` \<enter\>. Espere até a tela parar de "pipocar", seu _container_ está sendo carregado.
4. Quase lá! Clique __1880__ (ou acione OPEN PORT e digite __1880__ \<OK\>). Uma nova janela se apresentará. Simplesmente, feche algumas janelas _popup_ de aviso e voilá!:pray: Finja que não vê o Node-Red e só chame o dashboard (coloque __ui__ após a primeira barra solitária da URL: `http://ip...play-with-docker.com/ui` \<enter\>) e divirta-se... __GET__

