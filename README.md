# application_TTN_Node-RED
Explorando APIs e nodes TTN

A boa dica de hoje será um fluxo de monitoramento de falhas de gateways (Gateway Management). Na verdade, não inventei coisa alguma, pois me inspirei no trabalho da comunidade TTN de Barcelona (https://tinkerman.cat/post/monitor-your-ttn-gateways-with-node-red). 

A ideia é utilizar algumas API, meio que perdidas no “limbo”, para checar o status (online ou offline) de gateways numa determinada região e, melhor ainda, alertar por meio do envio de mensagem pelo Telegram quando houver alguma alteração de status.

Bem, se faz necessário saber mexer um pouco com o Node-RED, descobri o ID dos gateways de interesse e configurar apropriadamente o Node Telegram. Importe o código e faça as adaptações necessárias para sua rede.
