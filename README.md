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
