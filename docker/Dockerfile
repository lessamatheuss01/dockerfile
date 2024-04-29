# Use uma imagem base leve do NGINX
FROM nginx:latest

# Copie o seu arquivo HTML para o diretório raiz do servidor web no contêiner
COPY index.html /usr/share/nginx/html/index.html

# Exponha a porta 80 para o tráfego da web
EXPOSE 80

# Comando para iniciar o NGINX quando o contêiner for iniciado
CMD ["nginx", "-g", "daemon off;"]
