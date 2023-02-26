# **Como Instalar Elasticsearch, Logstash e Kibana no Ubuntu**

Este tutorial mostra como fazer a instalação e configuração do ELK Stack em uma máquina virtual Ubuntu Server.

O **ELK Stack** é uma ferramenta gratuita da Elastic com softwares para pesquisar, analisar e visualizar logs ajudando no monitoramento de equipamentos.

Elasticsearch – Busca e analise de várias fontes.

Logstash – Processador de dados com pipelines que recebe, transforma e envia dados simultâneos.

Kibana – Visualizar dados do Elasticsearch através de gráficos e dashboards.

Beats – Agentes que envia dados coletados para o Logstash ou Elasticsearch.

Para saber mais sobre o ELK Stack, acesse o [Elastic](https://www.elastic.co/pt/what-is/elasticsearch).

# Pré - Requisitos

Para utilizar o ELK Stack, vamos configurar uma máquina virtual com as seguintes configurações:
- [Ubuntu Server (v. 22.04)](https://ubuntu.com/download/server)
- RAM: 6MB
- 1 CPU
> Para este passo é necessário que tenha um software para criação de máquinas virtuais instalado. No tutorial, utilizamos o VirtualBox. Para realizar a instalação do virtual box e para a criação da máquina virtual acesse os links abaixo:  
[VirtualBox](https://www.virtualbox.org/wiki/Downloads)  
[Ubuntu Server]()  

## Instalação Elasticsearch

Para fazer a instalação do Elasticsearch no Ubuntu Server:

1 - execute o comando para importar a chave GPG pública do Elasticsearch para o APT:

```
wget -qO - https://artifacts.elastic.co/GPG-KEY-elasticsearch | sudo apt-key add -
```

2 - Atualize os pacotes e execute o comando para instalar o transport-https:

```
sudo apt-get install apt-transport-https
```

3 - Adicione a lista de origens da Elastic ao diretório sources.list.d, onde o APT irá procurar por novas origens:

```
echo "deb https://artifacts.elastic.co/packages/8.x/apt stable main" | sudo tee -a /etc/apt/sources.list.d/elastic-8.x.list
```

4 – Atualize a lista de pacotes:

```
sudo apt update
```

5 – Instale o Elasticsearch:

```
sudo apt install elasticsearch
```

6 – Faça algumas alterações no arquivo abaixo, descomentando as seguintes linhas:

```
nano /etc/elasticsearch/elasticsearch.yml
```

-node.name: node-1	

-network.host: 0.0.0.0

-discovery.seeds_hosts: ["127.0.0.1"]

-cluster.initial_master_nodes: ["node-1"]


7 – Carregue o Elaticsearch no servidor:


```
sudo /bin/systemctl daemon-reload
```
```
sudo /bin/systemctl start elasticsearch.service
```
```
sudo systemctl enable elasticsearch.service
```
```
sudo systemctl status elasticsearch
```
```
curl -X GET "http:localhost:9200"
```


## Instalação do Kibana:

1 – Atualize os pacotes:

```
sudo apt update
```

2 – Instale o kibana

```
sudo apt install kibana
```

3 – Realize as alterações no arquivo abaixo:

```
nano /etc/kibana/kibana.yml
```

-server.port: 5601

-server.host: "0.0.0.0"

-elasticsearch.hosts["http://127.0.0.1:9200"]

4 – Inicie o serviço no servidor:

```
sudo /bin/systemctl daemon-reload
```
```
sudo /bin/systemctl start kibana.service
```
```
sudo systemctl enable kibana.service
```
```
sudo systemctl status kibana
```
## Instalação do Logstash

1 – Instale o Logstash:

```
sudo apt install logstash
```

2 – Entre na pasta e inicie o serviço:

```
cd /etc/logstash/
```
```
sudo /bin/systemctl start logstash
```
```
sudo systemctl enable logstash
```
```
sudo systemctl status logstash
```

## Para iniciar um script
> Para que o ELK Stack funcione corretamente é necessário que o Java esteja instalado. Se houver algum erro relacionado ao Java, execute o comando para verificar a versão:
```
java -version
```
> Se houver uma mensagem semelhante a esta, será necessário instalar o JDK.   
⚠️ Command 'java' not found, but can be installed  
Execute o comando abaixo:  
```
sudo apt install default-jre
```
```
sudo apt install default-jdk
```
```
sudo apt update
```
Entre na pasta do Logstash:
```
cd /usr/share/logstash
```
```
sudo /bin/logstash -f /etc/logstash/nome_do_arquivo.conf
```
