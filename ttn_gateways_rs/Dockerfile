#Fonte: https://github.com/JohanScheepers/TTN_Gateway_Node
#Inspiração: https://www.youtube.com/watch?v=fdSaJR4dGmQ
#Consulta: https://nodered.org/docs/getting-started/docker
#Comando Docker: "docker build -t gpstesouro/ttn_gateways_rs:v1 ." 

FROM nodered/node-red
COPY package*.json /data/
RUN npm install --unsafe-perm --no-update-notifier --no-fund --only=production

COPY settings.js /data/settings.js
COPY flows_cred.json /data/flows_cred.json
COPY flows.json /data/flows.json 

WORKDIR /usr/src/node-red
RUN npm install node-red-dashboard
RUN npm install node-red-contrib-web-worldmap
RUN npm install node-red-node-ui-table

EXPOSE 1880

#CMD ["node", "server.js"] (não usado)
