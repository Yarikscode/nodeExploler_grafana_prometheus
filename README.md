# Node Exporter

## Установка

Установка Node Exporter будет осуществляться напрямую на виртуальную машину, т.к. мы хотим собирать метрики с неё, а не с контейнера.

Скачиваем архив:  
```bash
wget https://github.com/prometheus/node_exporter/releases/download/v1.9.0/node_exporter-1.9.0.linux-amd64.tar.gz
```

Обязательно проверьте актуальный релиз:  
https://github.com/prometheus/node_exporter/releases

Распаковка архива:  
```bash
tar xvf node_exporter-1.9.0.linux-amd64.tar.gz
```

Переходим в папку с распакованным архивом:  
```bash
cd node_exporter-1.9.0.linux-amd64
```

Копируем бинарник в `/usr/local/bin`:  
```bash
sudo cp node_exporter /usr/local/bin
```

Удаляем папку:  
```bash
rm -rf ./node_exporter-1.9.0.linux-amd64
```

Создаём сервисного пользователя:  
```bash
sudo useradd --no-create-home --shell /bin/false node_exporter
```

Выдаём необходимые права:  
```bash
sudo chown node_exporter:node_exporter /usr/local/bin/node_exporter
```

Создаём файл демона:  
```bash
sudo vim /etc/systemd/system/node_exporter.service
```

Содержимое взять отсюда:  
https://github.com/Yarikscode/nodeExporter_grafana_prometheus/blob/main/node-exploler/node_exporter.service

Перезагрузим конфигурацию systemd:  
```bash
sudo systemctl daemon-reload
```

Автоматический старт демона при запуске системы:  
```bash
sudo systemctl enable node_exporter
```

Запуск демона:  
```bash
sudo systemctl start node_exporter
```

Просмотр статуса демона:  
```bash
sudo systemctl status node_exporter.service
```

На этом этапе `node_exporter` запущен и собирает метрики.

---

# Prometheus

## Установка

1. Необходимо разместить на вашей ВМ файлы `docker-compose.yml` и `prometheus.yml` аналогично репозиторию.
2. Перейти в папку `prometheus` и выполнить команду:
   ```bash
   docker compose up -d
   ```
3. Проверить работоспособность Prometheus можно по ссылке:  
   `http://<ваш ip>:9090`

> Обратите внимание, что в конфиге используется IP-адрес вашего сервера.

---

# Grafana

## Установка

1. Сохрани конфигурацию Docker Compose в файл `docker-compose.yml`.
2. Запусти Grafana:
   ```bash
   docker compose up -d
   ```
3. Перейди в Grafana по адресу:  
   `http://<host-ip>:3000`

4. В разделе **источники данных (Data Sources)** выбери в качестве источника **Prometheus**.

5. Настрой дашборд в разделе **Dashboards**, для этого импортируй доску и вставь соответствующий ID. В качестве примера — **Node Exporter Full** с ID `1860`.

