# application_TTN_Node-RED
Explorando APIs e nodes TTN

A boa dica desta vez será um fluxo de monitoramento de falhas de gateways (Gateway Management). Na verdade, nada foi inventado, pois me inspirei e adaptei o trabalho da comunidade TTN de Barcelona (https://tinkerman.cat/post/monitor-your-ttn-gateways-with-node-red). 

A ideia é utilizar algumas API, meio que perdidas no “limbo”, para checar o status (online ou offline) de gateways numa determinada região e, melhor ainda, alertar por meio do envio de mensagem pelo Telegram quando houver alguma alteração de status.

Bem, se faz necessário saber mexer um pouco com o Node-RED, descobrir o ID dos gateways alvos de interesse e configurar apropriadamente o Node Telegram. Importe o código (gw-management.json) e faça as adaptações necessárias para sua rede.
