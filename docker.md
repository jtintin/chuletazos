# --- DOCKER DE 0 A PRO ---

# --- INSTALACIÓN Y VERIFICACIÓN ---
docker --version                  # verificar versión instalada
docker info                       # información del sistema Docker
docker ps                         # ver contenedores en ejecución
docker ps -a                      # ver todos los contenedores

# --- IMÁGENES ---
docker pull ubuntu:20.04          # descargar imagen específica
docker images                     # listar imágenes locales
docker rmi nombre-imagen          # borrar imagen
docker rmi $(docker images -q)    # borrar todas las imágenes

# --- CONTENEDORES ---
docker run -it ubuntu bash        # crear y entrar a contenedor interactivo
docker run -d -p 8080:80 nginx    # correr contenedor en segundo plano con puerto
docker stop nombre-contenedor     # detener contenedor
docker start nombre-contenedor    # iniciar contenedor detenido
docker rm nombre-contenedor       # borrar contenedor
docker rm $(docker ps -aq)        # borrar todos los contenedores

# --- VOLUMENES Y PERSISTENCIA ---
docker volume create mi_volumen   # crear volumen
docker volume ls                  # listar volúmenes
docker run -v mi_volumen:/data ubuntu  # montar volumen en contenedor
docker volume rm mi_volumen       # borrar volumen

# --- REDES ---
docker network ls                 # listar redes
docker network create mi_red      # crear red
docker run -d --network mi_red nginx   # correr contenedor en red personalizada

# --- DOCKERFILE ---
# Ejemplo básico
FROM ubuntu:20.04
RUN apt-get update && apt-get install -y curl
CMD ["bash"]

# Construir imagen desde Dockerfile
docker build -t mi_imagen .

# --- DOCKER COMPOSE ---
# docker-compose.yml ejemplo
version: "3"
services:
  web:
    image: nginx
    ports:
      - "8080:80"
  db:
    image: postgres
    environment:
      POSTGRES_PASSWORD: ejemplo

# Comandos básicos
docker compose up -d              # levantar servicios
docker compose down               # bajar servicios
docker compose ps                 # ver estado de servicios

# --- LIMPIEZA ---
docker system prune               # limpiar contenedores, imágenes y redes no usadas
docker system df                  # ver uso de espacio
docker builder prune              # limpiar caché de construcción

# --- PRO TIPS ---
docker exec -it nombre-contenedor bash   # entrar a contenedor en ejecución
docker logs nombre-contenedor            # ver logs
docker inspect nombre-contenedor         # ver detalles
docker tag mi_imagen repo/mi_imagen:v1   # etiquetar imagen
docker push repo/mi_imagen:v1            # subir imagen a Docker Hub

