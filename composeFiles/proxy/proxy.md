# Proxy

This container sis created by (Jamie Curnow)[https://github.com/jc21]. The (project)[https://nginxproxymanager.com] can be used to use nginx as a proxy. This is ofcourse default value for nginx, 
however this project adds an UI to it. 

`version: '3.8'
services:
  proxy:
    image: jc21/nginx-proxy-manager:latest
    container_name: proxy
    hostname: proxy
    restart: always
    environment:
      - TZ=Europe/Amsterdam
    ports:
      - 80:80
      - 81:81
      - 443:443
    volumes:
      - ./data:/data
      - ./letsencrypt:/etc/letsencrypt
`
