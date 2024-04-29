# Configuração de Servidor Web com NGINX e Docker Compose

Este repositório contém a configuração de um servidor web utilizando NGINX, empacotado em imagens Docker e orquestrado com Docker Compose.

# Dockerfile

O Dockerfile utilizado neste projeto é o seguinte:
```Dockerfile
# Use uma imagem base leve do NGINX
FROM nginx:latest

# Copie o seu arquivo HTML para o diretório raiz do servidor web no contêiner
COPY index.html /usr/share/nginx/html/index.html

# Exponha a porta 80 para o tráfego da web
EXPOSE 80

# Comando para iniciar o NGINX quando o contêiner for iniciado
CMD ["nginx", "-g", "daemon off;"]
```
Esse Dockerfile:
- Utiliza a imagem base nginx:latest do NGINX;
- Copia o arquivo index.html para o diretório raiz do servidor web no contêiner;
- Expõe a porta 80 para o tráfego da web;
- Define o comando para iniciar o NGINX quando o contêiner for iniciado.

# index.html
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body style="color: chocolate;">
    <h2>Dockerfile</h2>
</body>
</html>
```
# Docker Compose
A configuração do Docker Compose utilizada neste projeto é a seguinte:

```yaml
services:
  load_balancer:
    image: nginx:latest
    ports:
      - "80:80"
    volumes:
      - "./load-balancer/nginx.conf:/etc/nginx/nginx.conf"
    networks:
      - rede-compose
    depends_on:
      - site1
      - site2
      - site3

  site1:
    image: matheuslessa/nginx:cor1
    networks:
      - rede-compose

  site2:
    image: matheuslessa/nginx:cor2
    networks:
      - rede-compose

  site3:
    image: matheuslessa/nginx:cor3
    networks:
      - rede-compose

networks:
  rede-compose:
    driver: bridge
```
Essa configuração:
- Cria um serviço de balanceador de carga (load_balancer) usando a imagem nginx:latest;
- Mapeia a porta 80 do host para a porta 80 do contêiner do balanceador de carga;
- Monta um arquivo de configuração do NGINX (nginx.conf) no balanceador de carga;
- Cria três serviços de aplicação (site1, site2 e site3) usando imagens personalizadas (matheuslessa/nginx:cor1, matheuslessa/nginx:cor2 e matheuslessa/nginx:cor3);
- Conecta todos os serviços à mesma rede Docker (rede-compose).

# nginx.conf
```nginx
worker_processes  1;

events {
    worker_connections  1024;
}

http {
    upstream sites {
        server site1:80;
        server site2:80;
        server site3:80;
    }

    server {
        listen 80;

        location / {
            proxy_pass http://sites;
        }
    }
}
```
Essa configuração:
- Define um grupo de servidores upstream chamado sites, que inclui os três serviços de aplicação (site1, site2 e site3);
- Configura um servidor HTTP que escuta na porta 80 e encaminha todas as solicitações para o grupo de servidores upstream sites.

# Uso
Para usar essa configuração, siga as seguintes etapas:
1. Construa a imagem Docker usando o Dockerfile:
```bash
docker build -t matheuslessa/nginx .
```
2. Execute o Docker Compose para iniciar os serviços:
```bash
docker-compose up -d
```
3. Acesse o balanceador de carga em http://localhost no seu navegador.
