Consul:
  Tem agente em modo server ou modo client.

  - O consul implementa um servidor DNS por padrão
Levantar images:
docker-compose up -d

Entrar dentro do container:
docker exec -it consul01 sh 

Cirando um agente dentro do container:
consul  agent -dev

consul members - Mostra os membros consul, OU SEJA, todo mundo que faz parte do 
cluster/server

Para ver os nodes:
curl localhost:8500/v1/catalog/nodes

Intalando um pacote para conultar o dns local(dig):
apk -U add bind-tools

buscando  oservidor de dns local:
dig @localhost -p 8600

buscando todos os endereços e nodes atrelados ao nosso server dns:
dig @localhost -p 8600 consul01.node.consul


ifconfig
mkdir /etc/consul.d
mkdir /var/lib/consul
consul agent -server -bootstrap-expect=3 -node=consulserver03 -bind=192.168.0.4 -data-dir=/var/lib/consul agent -config-dir=/etc/consul.d

Join de máquinas consul:
consul join 192.168.0.2:8301

Client:
ifconfig
mkdir /etc/consul.d
mkdir /var/lib/consul
consul agent -bind=192.168.0.5 -data-dir=/var/lib/Consul -config-dir=/etc/consul.d

// Atualizar todos os Serviços
consul reload

### Sincronizando servers via Arquivo(Facilitando a vida):
- Dentro do diretório servers/server01, apenas configurar o ip dos demais
  servidores. Após rodar e entrar no container:
  docker exec -it consul01 sh 

  Executar este comando para configurar o consul, com o json:
    consul agent -config-dir=/etc/consul.d

  Para listar:
    consul members

### Criptografia:
  Após criar os servers consul, entrar em um e gerar uma chave:
  
  consul keygen

  Esta chave deve ser colocada na propriedade encrypt, dos arquivos server.json