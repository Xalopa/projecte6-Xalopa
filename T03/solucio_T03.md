# T03: Missió Nginx: Migració d'Alt Rendiment i Arquitectura Lleugera

# 1. Preparació de l’Entorn i Instal·lació
## 1.1 Aturada del servei Apache

Per evitar conflictes de ports (80 i 443), es va procedir a aturar i deshabilitar el servei Apache:

sudo systemctl stop apache2
sudo systemctl disable apache2

![captura1](img/1.png)

![captura1](img/2.png)

## 1.2 Instal·lació de Nginx

Ara, instal·larem el servidor web Nginx mitjançant el gestor de paquets:

sudo apt update
sudo apt install nginx -y

![captura1](img/3.png)

![captura1](img/4.png)

## 1.3 Verificació del servei

Comprovarem que el servei està actiu:

sudo systemctl status nginx

![captura1](img/5.png)

Finalment, es va validar l’accés des del navegador, mostrant correctament la pàgina de benvinguda de Nginx.

![captura1](img/6.png)

# 2. Configuració de Server Blocks (Multidomini)
## 2.1 Estructura de directoris

Reutilitzarem les carpetes existents:

/var/www/nexus

/var/www/academia

Ajustarem els permisos:

sudo chown -R www-data:www-data /var/www/nexus
sudo chown -R www-data:www-data /var/www/academia

![captura1](img/7.png)

## 2.2 Creació de Server Blocks

Crearem dos fitxers de configuració a:

/etc/nginx/sites-available/nexus
/etc/nginx/sites-available/academia

![captura1](img/8.png)

![captura1](img/9.png)

I haurem de fer una petita configuració:

![captura1](img/10.png)

![captura1](img/11.png)

## 2.3 Activació dels llocs

Crearem els enllaços simbòlics:

sudo ln -s /etc/nginx/sites-available/nexus /etc/nginx/sites-enabled/
sudo ln -s /etc/nginx/sites-available/academia /etc/nginx/sites-enabled/

![captura1](img/12.png)

Verificació de la sintaxi, reinici del servei i comprobacio en el client:

![captura1](img/13.png)

![captura1](img/14.png)

![captura1](img/15.png)

![captura1](img/16.png)

# 3. Personalització d’Errors

Configurararem la gestió d’errors 404 amb la directiva:

error_page 404 /404.html;

![captura1](img/17.png)

![captura1](img/18.png)

Quan un usuari accedeix a un recurs inexistent, el servidor retorna la pàgina personalitzada prèviament creada, millorant l’experiència d’usuari.

![captura1](img/19.png)

![captura1](img/20.png)

# 4. Seguretat i Certificats (HTTPS)
## 4.1 Generació del certificat SSL

Per habilitar connexions segures, es va generar un certificat SSL autofirmat mitjançant la següent comanda:

sudo openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout /etc/ssl/private/projectenexus.key -out /etc/ssl/certs/projectenexus.crt

sudo openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout /etc/ssl/private/academia.key -out /etc/ssl/certs/academia.crt

![captura1](img/21.png)

![captura1](img/22.png)

## 4.2 Configuració SSL

Reutilitzarem els certificats SSL existents i es va configurar el servidor per escoltar al port 443:

![captura1](img/23.png)

![captura1](img/24.png)

Comprovarem que tot estigui correctament el que hem creat previament:

![captura1](img/25.png)

I finalment comprovem que ara funciona via HTTP/2:

![captura1](img/26.png)


































