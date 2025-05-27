# Earthquake Event Dispatcher Monitor

一個基於 Docker Compose 的系統監控和日誌記錄基礎設施，用於監控[主要地震處理系統](https://github.com/Peipeixuan/Earthquake-Event-Dispatcher)。

### 系統架構

本監控基礎設施實現雙管道架構：
- **指標收集管道**：使用 Prometheus 和 Grafana
- **日誌處理管道**：使用 ELK （Elasticsearch、Logstash、Kibana）

### 核心組件

| 組件 | 容器名稱 | port | 用途 |
|------|----------|------|------|
| Prometheus | `prometheus` | 9090 | 指標收集和儲存 |
| Grafana | `grafana` | 3000 | 指標視覺化和儀表板 |
| Elasticsearch | `elasticsearch` | 9200 | 日誌存儲和索引 |
| Logstash | `logstash` | 5044 | 日誌處理和轉發 |
| Kibana | `kibana` | 5601 | 日誌分析和視覺化 |

### 服務配置

#### Prometheus
- 映像：`prom/prometheus:v2.26.0`
- 配置文件：`./prometheus.yml` 掛載到容器內
- 持久化存儲：使用 `prometheus`

#### Grafana
- 映像：`grafana/grafana-oss` 
- 依賴：Prometheus 服務 
- 配置：通過 `.env` 文件和 provisioning 目錄
- 持久化存儲：使用 `grafana-storage` 

#### Elasticsearch
- 映像：`docker.elastic.co/elasticsearch/elasticsearch:7.10.0`  
- 配置：單節點模式 

#### Logstash
- 映像：`docker.elastic.co/logstash/logstash:7.10.0`
- 依賴：Elasticsearch 服務 
- 配置：`./logstash.conf`

#### Kibana
- 映像：`docker.elastic.co/kibana/kibana:7.10.0` 
- 依賴：Elasticsearch 服務 
- Elasticsearch 連接：`http://elasticsearch:9200`

### Quick Start

1. 確保已安裝 Docker 和 Docker Compose
2. Clone 此儲存庫
3. 創建必要的配置文件：
   - `prometheus.yml`：Prometheus 配置
   - `logstash.conf`：Logstash 管道配置
   - `.env`：Grafana 環境變數
4. 啟動所有服務：
   ```bash
   docker-compose up -d
   ```

### 訪問服務

- **Grafana 儀表板**：http://localhost:3000
- **Prometheus Web UI**：http://localhost:9090
- **Kibana 介面**：http://localhost:5601
- **Elasticsearch API**：http://localhost:9200

### 系統架構圖
![infra_overview](https://github.com/user-attachments/assets/6eaa7141-88d7-476c-b84c-f8bfb4ac62af)
![infra_detail](https://github.com/user-attachments/assets/b5a69da8-bba3-47e7-b46e-5285ec0ad89a)
