# TrueNAS Uploader

Pagina web simple para subir archivos al dataset `/mnt/NVME/upload` en TrueNAS.

## Portainer

1. Subi este proyecto a GitHub.
2. En Portainer, entra a **Stacks**.
3. Crea un stack nuevo desde **Repository**.
4. Usa el repositorio de GitHub y el archivo `docker-compose.yml`.
5. Deploy.

Por defecto publica la pagina en el puerto `8085`. El stack usa la imagen oficial de Nginx, sin `build`, para evitar errores de permisos en Portainer.

```yaml
services:
  truenas-uploader:
    image: nginx:1.27-alpine
    container_name: truenas-uploader
    restart: unless-stopped
    ports:
      - "8085:80"
    volumes:
      - ./:/usr/share/nginx/html:ro
    command: >
      sh -c "cp /usr/share/nginx/html/nginx.conf /etc/nginx/conf.d/default.conf
      && nginx -g 'daemon off;'"
```

## Uso

- Abrir `http://IP_DEL_HOST:8085`
- Dejar el campo **Servidor TrueNAS o proxy** como `/truenas`
- Usar como destino `/mnt/NVME/upload`
- Ingresar usuario y clave de TrueNAS
- Seleccionar o arrastrar archivos

El contenedor incluye un proxy Nginx hacia `http://192.168.1.120/_upload/` para evitar problemas de CORS.
