## Установка ElasticSearch 7.17
Установка из официального репозитория 

`curl -fsSL https://artifacts.elastic.co/GPG-KEY-elasticsearch | sudo apt-key add -`

`echo "deb https://artifacts.elastic.co/packages/7.x/apt stable main" | sudo tee -a /etc/apt/sources.list.d/elastic-7.x.list`

`sudo apt update`

`sudo apt install elasticsearch`

или из deb пакета

```
wget https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-7.17.13-linux-x86_64.tar.gz
tar -xzf elasticsearch-7.17.13-linux-x86_64.tar.gz
cd elasticsearch-7.17.13/ 
dpkg -i elasticsearch-7.17.3-amd64.deb
```

## Настройка

Настройка сетевого доступа

`nano /etc/elasticsearch/elasticsearch.yml`

```
network.host: [ localhost, _global_ ] # _global_ - доступ из внешки
discovery.seed_hosts: [ localhost, _global_ ]
```
Ограничиваем максимальное потребление памяти

`nano /etc/elasticsearch/jvm.options `

```
-Xms2g
-Xmx2g
```
