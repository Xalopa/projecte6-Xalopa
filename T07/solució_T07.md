# T07: TransLògic: Administració Avançada i Seguretat

## 1. Configuració de Seguretat de Contrasenyes

Primer de tot, establir una base de seguretat per a tots els usuaris del domini, modifica la Default Domain Policy i una longitud mínima de la contrasenya: 8 caràcters.

![captura1](img/1.png)

Ara, haurem de fer doble clic sobre Minimum password length i el posem al valor 8, per arribar a aqui haurem de fer el següent:

![captura2](img/2.png)

![captura1](img/3.png)

![captura1](img/4.png)

![captura1](img/5.png)

![captura1](img/6.png)

![captura1](img/7.png)

![captura1](img/8.png)

![captura1](img/9.png)

Seguidament, haurem de fer com que és específica per a una Unitat Organitzativa (OU), hem de crear una GPO nova, crear una GPO en aquest domini i vincular-la aquí.

![captura1](img/10.png)

![captura1](img/11.png)

L'haurem d'editar la nova GPO.

![captura1](img/12.png)

Haurem d'anar a la meteixa ruta que hem fet per arribar a on abans i configurarem el Realx minimum password length limits per a que ens deixi posar-la de 18 caràcters.

![captura1](img/13.png)

![captura1](img/14.png)

La contrasenya minim 18 caràcters

![captura1](img/15.png)

I de (Maximum password age), 28 dies

![captura1](img/16.png)

També, la contrasenya ha de complir els requisits de complexitat: Marca-ho com a Desactivada.

![captura1](img/17.png)

Després de tot això, haurem de crea una carpeta al disc C:\soft1, on posarem els fitxers .msi de 7zip i Firefox, i compartir-ho perquè tothom la pugui llegir.

![captura1](img/18.png)

![captura1](img/19.png)

Seguidament, haurem de crear una nova GPO anomenada “Instal·lar_7zip” dins la OU Gestió i editar-la.
Anar a Configuració d’usuari → Instal·lació de software i afegir un nou paquet per instal·lar 7zip.

![captura1](img/20.png)

![captura1](img/21.png)

![captura1](img/22.png)























