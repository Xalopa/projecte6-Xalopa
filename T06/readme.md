# Guia de l'activitat

## Fase 1: Preparació de l'entorn de laboratori

Instal·lar **Ubuntu Server** i **Windows 11** a les màquines virtuals (VMs).  
Configurar els **adaptadors de xarxa en mode pont (Bridge)** per tenir accés directe a la xarxa.

Inicialment, **al client no se li assignarà cap IP**, fins que prèviament **pausem les actualitzacions**.

Un cop tot estigui instal·lat, configurarem una **IP estàtica al servidor i al client** amb el següent esquema:

| Grup-classe | IP | Mascara | Gateway | DNS |
|--------------|------|---------------|---------------|---------|
| A | 192.168.2.y | 255.255.255.0 | 192.168.2.254 | 8.8.8.8 |
| B | 192.168.4.y | 255.255.255.0 | 192.168.4.254 | 8.8.8.8 |

On **y** correspon al vostre **número de llista**.

Recordeu que:
- Un membre instal·la **Ubuntu Server**
- L'altre instal·la **Windows 11**

### Canviar el nom del servidor

Canviar el nom del servidor a:

```
ca.nexusX.test
```

On **X és el número del vostre grup**.

### Configurar el fitxer hosts al client

Al client, configurar el fitxer **hosts** per resoldre el nom del servei web (`ca.nexusX.test`) a la seva IP corresponent.

### Important

És molt aconsellable **crear instantànies (snapshots)** de les dues màquines abans d'iniciar la pràctica.  
Això permet **restaurar l'estat dels sistemes un cop finalitzada l'activitat**.

---

# Fase 2: Creació de l'Entitat de Certificació (CA)

Editar l'arxiu de configuració de **OpenSSL**:

```
/etc/ssl/openssl.cnf
```

Afegir una secció específica per a la **CA corporativa**:

```
[ca]
default_ca = CA_default

[CA_default]
dir               = /etc/ssl/CA
certs             = $dir/certs
crl_dir           = $dir/crl
database          = $dir/index.txt
```

### Crear l'estructura de directoris

Crear l'estructura de la CA i inicialitzar els fitxers necessaris:

```bash
sudo mkdir -p /etc/ssl/CA/{certs,crl,newcerts,private}
sudo touch /etc/ssl/CA/index.txt
sudo echo 001 > /etc/ssl/CA/serial
```

### Generar la clau privada i certificat de la CA

```bash
sudo openssl req -new -x509 -keyout demoCA/private/cakey.pem -out demoCA/cacert.pem
```

Per donar identitat a la CA:

- **Organization Name** → Nom de l'organització (ex: Nexus 1, Nexus 2)
- **Common Name** → Nom del servidor (ex: `ca.nexusX.test`)

---

# Fase 3: Generació de la clau i certificat d'usuari

Simular l'emissió del certificat d'usuari directament des del servidor.

### Generar clau privada d'usuari

```bash
openssl req -new -keyout userkey.pem -out userreq.csr
```

Es pot assignar un **PIN**, per exemple:

```
123456
```

### Signar la sol·licitud amb la CA

```bash
openssl ca -in userreq.csr -out usercert.pem
```

### Convertir el certificat a format PKCS#12

Convertir el certificat a format **.pfx**, el format estàndard per a Windows:

```bash
openssl pkcs12 -export -out CertUser.pfx -inkey userkey.pem -in usercert.pem
```

Assignar una **contrasenya d'exportació**, que serà necessària perquè l'usuari la introdueixi al seu equip.

---

# Fase 4: Distribució de Certificats (Servidor - Client)

L'usuari ha de rebre:

- Certificat de la **CA** (`cacert.pem`)
- Certificat **personal** (`CertUser.pfx`)

## Mètode 1 (Bàsic): Ús del protocol SCP

Per facilitar la transferència:

1. Copiar els certificats al directori accessible al client.
2. Configurar els permisos adequats als fitxers.

Exemple:

```bash
chmod 777 CertUser.pfx
```

### Instal·lar el servei SSH al servidor

```bash
apt install ssh
```

### Descarregar els certificats des de Windows

Obrir **PowerShell** i executar:

```bash
scp usuari@IP_SERVIDOR:/ruta/cacert.pem .
scp usuari@IP_SERVIDOR:/ruta/CertUser.pfx .
```

---

## Mètode 2 (Avançat - Portal d'empleat)

Instal·lar un servidor web com **Apache** o **Nginx** a Ubuntu.

Crear una pàgina HTML corporativa **"Portal de Certificats"** amb enllaços de descàrrega:

```html
<h1>Portal de Certificats</h1>
<a href="cacert.pem">Descarregar certificat CA</a><br>
<a href="CertUser.pfx">Descarregar certificat d'usuari</a>
```

L'usuari només haurà d'entrar a la **IP del servidor des del navegador** i descarregar els certificats.

---

# Fase 5: Instal·lació de Certificats al Client

Obrir **PowerShell amb privilegis d'administrador** i instal·lar el lector de PDF mitjançant **Winget**:

```bash
winget install Adobe.Acrobat.Reader.64-bit --accept-source-agreements --accept-package-agreements
```

### Obrir el gestor de certificats

Executar:

```
certmgr.msc
```

### Importar el certificat de la CA

A la branca:

```
Entitats de confiança arrel
```

Importar:

```
cacert.pem
```

Això permet que el sistema operatiu **reconegui la vostra CA com a segura**.

### Importar el certificat d'usuari

A la secció:

```
Personal
```

Importar:

```
CertUser.pfx
```

Introduir la **contrasenya d'exportació**.

---

# Fase 6: Signatura Digital d'un Document PDF

Crear qualsevol **document PDF**, per exemple una **factura simulada de l'empresa cap al client**.

Obrir-lo amb **Adobe Acrobat Reader**.

### Procediment

1. Anar a **Totes les eines**
2. Seleccionar **Usar un Certificat**
3. Prémer **Signar**

Dibuixar l'àrea on s'aplicarà la signatura i seleccionar el **certificat instal·lat**.

Finalment:

- Guardar el document
- (Opcional) Bloquejar el document

Reobrir el PDF per verificar que:

- La **signatura és vàlida**
- El **panell de signatures confirma l'autoria**
- Tot el procés criptogràfic funciona correctament

## Material de suport

- Material de l’assignatura Seguretat Informàtica. RA3. Signatura electrònica i Certificats Digitals (Moodle de l’assignatura).
- Guia de l’activitat [enllaç]
