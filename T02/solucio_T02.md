## T02: Missió Apache: Desplegament Multidomini i segur

Primer de tot farem la configuració inicial

![captura1](img/1.png)

Seguidament instalarem el servei apache2

![captura1](img/2.png)

Seguidament haurem de verificar el funcionament del servei utilitzant la comanda "apachectl" per comprovar l'estat.
Ens hem d'assegurar que el usuaria www-data s'ha creat correctament i també hem de verificar els permisos de la carpeta /var/www.

![captura1](img/3.png)

Ara hem de crear dues carpetes per cada domini dins de la carpeta /var/www que ja ha creat Apache a l'instalarse.

![captura1](img/4.png)

Per configurar els virtual host crearem els arxius de configuració a /etc/apache2/sites-aviable un cop estem a dins haurem de ficaraquestes dues comandes:
sudo cp 000-default.conf xavi1.conf
sudo cp 000-default.conf xav2.conf

![captura1](img/6.png)

Ara anem a editar l'arxiu sudo nano xavi1.conf i sudo nano xavi2.conf
I tindrem que ficar la linea que falta que és la seguent: ServerName xavi.test i a l'arxiu de xavi2.conf tambe haurem de ficar aquesta altre configuració. ServerName xavi1.test

![captura1](img/7.png)

![captura1](img/8.png)

Un cop hem editat la configuració haurem d'actualitzar els canvis. De la següent manera hem de fer un sudo a2ensite xavi1.conf i ens demanara que reiniciem el servei apache fent un sudo systemctl reload apache2, haurem de fer el mateix amb la altre.

![captura1](img/9.png)

Com treballem en un entorn sense DNS, modificarem directament l'arxiu de /etc/hosts al zorin (client).
Modificació arxiu hosts
Cal modificar l'arxiu de hosts de la maquina del client afegint els dominis creats i la IP del servidor. Obrim un terminal a la maquina zorin (client) i entrem a l'arxiu /etc/hosts i fiquem la seguent configuració.

![captura1](img/10.png)

![captura1](img/11.png)

Ara anem al navegador i busquem http://xavi1.test, http://xavi2.test i ens surt el missatge que hem configurat previament.

![captura1](img/12.png)

![captura1](img/13.png)

Crear pàgina d'error: Crearem un fitxer html professional dins del directori de la web. Primer de tot hem de crear la ruta del arxiu.

![captura1](img/14.png)

Un cop l'hem creat l'obrim amb sudo nano /var/www/xavi1/404.html i configurarem el arxiu de la seguent manera.

![captura1](img/15.png)

Un cop hem configurat l'arxiu haurem de enllaçar l'error, editarem el fitxer de virtualhost i afagirem: ``ErrorDocument 404 /404.html.

![captura1](img/16.png)

Al accedir a una pagina que no existeix en saltara l'error.

![captura1](img/17.png)

Generar el certificat autosignat, crearem les carpetes de seguretat: sudo mkdir private i sudo mkdir cert dins del directori de xavi1 i xavi2.

![captura1](img/18.png)

Executarem la comanda openssl per generar la clau i el certificat de 365 dies.
Amb la comanda: sudo openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout /var/www/xavi1/private/xavi1.key -out
/var/www/xavi1/cert/xavi1.crt

![captura1](img/19.png)

Xavi2

![captura1](img/20.png)

Habilitarem el ssl a Apache: sudo a2enmod ssl.

![captura1](img/21.png)

Xavi2

![captura1](img/21.png)

Crearem el fitxer xavi1-ssl.conf copiant el default-ssl.conf

![captura1](img/23.png)

Configurarem el Server Name, document root i les rutes SSL CertificateFilei SSLCertificateKeyFile apuntant als fitxers creats: sudo nano /etc/apache2/sites-available/xavi1-ssl.conf

![captura1](img/24.png)

Xavi2

![captura1](img/25.png)

Per habilitar el protocol https en apache cal fer:
a2enmod ssl
a2ensite site1-ssl.conf
systemctl restart apache2

![captura1](img/26.png)

Xavi2

![captura1](img/27.png)

Com podem veure la web no es segura, ara ho cambiarem.

![captura1](img/28.png)

Ara forçarem el HTTPS: al fitxer de configuració afagirem la línea Redirect / https: //xavi1.test.

![captura1](img/29.png)

Xavi2

![captura1](img/30.png)

Restringir accés a claus: Un problema de seguretat es que es visible tot el nostre directori, inclòs les claus i coses privades.

![captura1](img/31.png)

Xavi 2

![captura1](img/32.png)

Per solucionar-ho: Dins de la configuració, afegeix una directiva per a la carpeta /private amb Require all denied per seguretat.

![captura1](img/33.png)

Xavi2

![captura1](img/34.png)

Comprovació:

![captura1](img/35.png)

Xavi2

![captura1](img/36.png)

Activar el mòdul: Executarem sudo a2enmod http2

![captura1](img/37.png)

I finalment, configurar el protocol: Dins dels VirtualHosts de la pàgina segura (port 443), afegeix la línia Protocols h2 http/1.1

![captura1](img/38.png)

Xavi2

![captura1](img/39.png)







































