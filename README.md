# CinemaBooking Deployment System

## 1. Giới thiệu

CinemaBooking là hệ thống quản lý đặt vé xem phim được triển khai theo mô hình container hóa sử dụng Docker Compose.

Hệ thống bao gồm:

- ASP.NET Core Web Application
- MySQL Database
- phpMyAdmin Database Management
- Nginx Reverse Proxy
- Prometheus Monitoring
- Grafana Dashboard
- cAdvisor Container Metrics
- Loki Centralized Logging
- Promtail Log Collector


## 2. Kiến trúc hệ thống

```
                    Client Browser

                          |
                          v

                    Nginx :80

                          |
                          v

                ASP.NET Core Application

                          |
                          v

                    MySQL Database



Monitoring:

        cAdvisor
            |
            v
       Prometheus
            |
            v
        Grafana



Logging:

      Docker Logs
            |
            v
        Promtail
            |
            v
          Loki
            |
            v
        Grafana Explore
```


## 3. Công nghệ sử dụng

| Thành phần | Công nghệ |
|---|---|
| Backend | ASP.NET Core |
| Database | MySQL 8.4 |
| Container | Docker |
| Orchestration | Docker Compose |
| Reverse Proxy | Nginx |
| Monitoring | Prometheus + Grafana |
| Metrics Exporter | cAdvisor |
| Logging | Loki + Promtail |
| Source Control | GitHub |


## 4. Cấu trúc thư mục

```
CinemaBooking_Deploy

├── CinemaBooking
│   └── ASP.NET Core Source Code
│
├── nginx
│   └── nginx.conf
│
├── prometheus
│   └── prometheus.yml
│
├── loki
│   └── loki-config.yml
│
├── promtail
│   └── promtail-config.yml
│
├── grafana
│
├── docker-compose.yml
│
└── .env.example
```


## 5. Yêu cầu môi trường

Cần cài đặt:

- Docker Desktop
- Docker Compose


Kiểm tra:

```bash
docker --version

docker compose version
```


## 6. Cấu hình môi trường

Tạo file `.env`

Ví dụ:

```env
MYSQL_ROOT_PASSWORD=your_password

MYSQL_DATABASE=CinemaDB

MYSQL_USER=cinema_app

MYSQL_PASSWORD=your_password

PHPMYADMIN_PORT=8081
```


## 7. Khởi chạy hệ thống


Build và chạy toàn bộ container:

```bash
docker compose up -d
```


Kiểm tra trạng thái:

```bash
docker compose ps
```


Kết quả mong muốn:

```
cinema-app          Up

cinema-mysql        Healthy

cinema-nginx        Up

cinema-prometheus   Up

cinema-grafana      Up

cinema-loki         Up

cinema-promtail     Up

cinema-cadvisor     Up
```


## 8. Danh sách dịch vụ

| Service | Port |
|-|-|
| Website | 80 |
| ASP.NET | 8080 |
| phpMyAdmin | 8081 |
| cAdvisor | 8082 |
| Prometheus | 9090 |
| Grafana | 3000 |
| Loki | 3100 |


## 9. Reverse Proxy Nginx

Người dùng truy cập:

```
http://localhost
```


Luồng xử lý:

```
Client

 ↓

Nginx

 ↓

ASP.NET Container
```


Security headers được cấu hình:

```
X-Content-Type-Options

X-Frame-Options

X-XSS-Protection
```


## 10. Monitoring System


### Prometheus

Truy cập:

```
http://localhost:9090
```


Kiểm tra Targets:

```
Status
→ Targets
```


Các target:

```
prometheus     UP

cadvisor       UP
```


### Grafana

Truy cập:

```
http://localhost:3000
```


Datasource:

```
Prometheus

URL:
http://prometheus:9090
```


Dashboard giám sát:

- CPU Usage
- Memory Usage
- Container Status
- Network Usage


## 11. Centralized Logging


Hệ thống log:

```
Docker Container

        |

    Promtail

        |

      Loki

        |

    Grafana Explore
```


Datasource:

```
Loki

URL:

http://loki:3100
```


## 12. LogQL Examples


### Application Log

```logql
{container="cinema-app"}
```


### Nginx Access Log

```logql
{container="cinema-nginx"}
```


### MySQL Log

```logql
{container="cinema-mysql"}
```


## 13. Stop System


Dừng toàn bộ:

```bash
docker compose down
```


Khởi động lại:

```bash
docker compose up -d
```


## 14. Git Commit History


```
feat: add Nginx reverse proxy and security headers

feat: add Prometheus Grafana monitoring

feat: add Loki Promtail centralized logging

docs: update deployment documentation
```
