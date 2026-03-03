# Prova de Concepte (PoC) – Infraestructura de Certificació Digital – Projecte Nexus

## Introducció

Un cop resolt el problema de la confidencialitat, Projecte Nexus ha detectat una necessitat crítica: garantir la **integritat, autenticitat i el no repudi** dels seus documents interns i contractes amb proveïdors.  

Fins ara signaven en paper, però volen modernitzar-se.

Han sol·licitat una **Prova de Concepte (PoC)** per demostrar que es pot desplegar una infraestructura pròpia on els empleats puguin obtenir **certificats digitals corporatius** i signar documents PDF oficialment, sense necessitat de comprar certificats a tercers per a ús intern.

---

## Descripció de l'activitat

L’activitat es divideix en tres fases principals.

Es treballarà en parelles:

- **Administrador de Nexus** → Gestionarà el servidor (Ubuntu Server).
- **Treballador de Nexus** → Gestionarà la màquina client.

Ambdós col·laboraran durant tot el procés.

### Fase 1: Desplegament de la CA a Ubuntu Server

- Instal·lació i configuració d’una Autoritat de Certificació (CA).
- Generació del certificat arrel.
- Configuració de l’entorn de signatura.

### Fase 2: Sol·licitud i Emissió de Certificats pel client

- Creació de la sol·licitud de certificat (CSR).
- Enviament al servidor.
- Signatura per part de la CA.
- Emissió del certificat digital del treballador.

### Fase 3: Signatura Digital i Verificació

- Instal·lació del certificat al client.
- Signatura digital d’un document PDF oficial.
- Verificació de la signatura mitjançant lector de PDF (ex: Acrobat Reader).
- Comprovació d’integritat i autenticitat.

---

## Què cal lliurar

Dins del repositori del projecte, a la carpeta corresponent a la tasca, cal lliurar:

---

### 1️⃣ Memòria tècnica

- Format: **MarkDown**
- Nom del fitxer:  

memoria.md

Ha d’incloure:

- Captures de pantalla comentades del procés d’instal·lació de l’Autoritat de Certificació (CA) a Ubuntu Server.
- Documentació del procediment de sol·licitud del certificat client.
- Procediment de creació del certificat client.
- Instal·lació de la clau pública de la CA al client.
- Instal·lació del certificat client.
- Procediment de signatura d’un document PDF i comprovació de la signatura.
- Breu explicació de les diferències entre **Clau Pública** i **Clau Privada** en aquest procés.

---

### 2️⃣ Evidència de la signatura

- Fitxer PDF de prova de Nexus signat digitalment per un dels membres del grup.
- El fitxer s’haurà d’adjuntar al repositori.

---

### 3️⃣ Certificat arrel

- Fitxer `.cer` corresponent a la clau pública de la vostra Autoritat de Certificació.
- També s’haurà d’incloure al repositori per poder ser descarregat i verificar signatures.

---

## Explicació tècnica: Clau Pública vs Clau Privada

En una infraestructura de certificació digital:

- **Clau Privada**
  - Només la coneix el propietari.
  - Serveix per signar digitalment documents.
  - Ha d’estar protegida i mai compartida.

- **Clau Pública**
  - Es distribueix lliurement.
  - Serveix per verificar signatures digitals.
  - Forma part del certificat digital.

La seguretat del sistema es basa en el fet que la clau privada no es pot deduir a partir de la clau pública.

---

## Objectius de Seguretat Assolits

Amb aquesta infraestructura s’aconsegueix:

- **Integritat** → El document no ha estat modificat.
- **Autenticitat** → Es pot verificar la identitat del signant.
- **No repudi** → El signant no pot negar haver signat el document.

---

## Material de suport

- Material de l’assignatura Seguretat Informàtica. RA3. Signatura electrònica i Certificats Digitals (Moodle de l’assignatura).
- Guia de l’activitat [enllaç]
