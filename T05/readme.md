# Informe Tècnic de Seguretat – Projecte Nexus

## Introducció

Aprofitant que ja esteu treballant amb la seva infraestructura web, des de Projecte Nexus se’ns sol·licita una nova petició d’ajuda.

A causa del gran volum de dades sensibles que gestionen (dades personals d'estudiants, exàmens oficials no publicats i certificats de notes), estan molt preocupats per la integritat i privacitat de la seva gestió acadèmica.

La direcció de Projecte Nexus ha demanat una demostració pràctica de com la nostra empresa pot garantir els tres pilars de la seguretat de la informació: **Confidencialitat, Integritat i Autenticitat**.

---

## Descripció de l'activitat

### Tasca 1: Protecció de dades en repòs (Xifratge Simètric)

Els caps de departament necessiten transportar els exàmens finals en memòries USB per imprimir-los a secretaria, però tenen por de perdre el dispositiu i que les preguntes es filtrin abans de la data de la prova.

S’ha de crear un contenidor xifrat (unitat virtual) dins d'un pendrive (simulat al disc dur) utilitzant el programari VeraCrypt (o similar).

#### Requisits

- Crear un volum de **100MB**.
- Utilitzar l'algorisme de xifratge **AES-256**.
- Establir una contrasenya robusta.
- Dins la unitat xifrada, copiar un fitxer de text anomenat:

EXAMEN_FINAL_SEGURETAT.txt

amb preguntes de prova.

- Demostrar que, sense muntar la unitat amb la contrasenya, el fitxer és inaccessible.

---

### Tasca 2: Verificació d'Integritat (Hashing)

Nexus distribueix material didàctic i software als alumnes a través del seu servidor web. Volen assegurar-se que els fitxers no han estat alterats per un atacant per incloure malware.

Utilitzant una eina com:

- CertUtil (Windows)
- md5sum / sha256sum (Linux)
- 7-Zip

#### Procediment

1. Crear un document de text anomenat:

nota_final_curs.txt

amb el contingut:

L'alumne ha aprovat amb un 5

2. Calcular el **Hash SHA-256** del fitxer original.
3. Modificar el fitxer canviant una sola xifra, per exemple:

L'alumne ha aprovat amb un 9

4. Tornar a calcular el Hash.
5. Comparar els resultats per demostrar com l’empremta digital canvia completament i revela la manipulació de la nota.

---

## Justificació Teòrica

El **xifratge** i les **funcions hash** són eines criptogràfiques amb objectius diferents.

El **xifratge** (com AES-256) serveix per protegir la confidencialitat de la informació, transformant les dades en un format il·legible que només es pot recuperar amb la clau o contrasenya correcta.

En canvi, una **funció hash** (com SHA-256) no amaga la informació, sinó que genera una empremta digital única del fitxer. Si el contingut canvia, encara que sigui mínimament, el hash resultant és completament diferent.

Per tant:
- El xifratge protegeix l'accés a la informació.
- El hash garanteix la seva integritat.

---

## Evidències de la Tasca 1 (Xifratge)

Incloure:

- Captura de pantalla de la configuració del volum (mostrant:
  - Mida: 100MB
  - Algorisme: AES-256
  - Sistema de fitxers seleccionat)

- Captura de la unitat muntada amb el fitxer:

EXAMEN_FINAL_SEGURETAT.txt

- Captures del procés d'accés al fitxer mitjançant la contrasenya.

- Evidència que, sense muntar la unitat, el fitxer no és accessible.

---

## Evidències de la Tasca 2 (Hashing)

Incloure:

- Captura del terminal o programa mostrant:
  - Hash SHA-256 del fitxer original.
  - Hash SHA-256 del fitxer modificat.

- Evidència clara que:
  - Els dos hashos són completament diferents.
  - Un canvi mínim en el contingut altera totalment el resultat.

---

## Conclusió

Es recomana a Projecte Nexus:

1. Utilitzar sempre **xifratge en dispositius portables** (USB, discs externs, portàtils).
2. Aplicar polítiques de **contrasenyes robustes**:
   - Mínim 12-16 caràcters.
   - Combinació de majúscules, minúscules, números i símbols.
   - Ús de gestors de contrasenyes per emmagatzemar-les de manera segura.
3. Implementar la verificació amb **funcions hash** per assegurar la integritat de:
   - Actes de notes
   - Contractes
   - Exàmens oficials
   - Documentació acadèmica sensible

La combinació de xifratge (confidencialitat) i hashing (integritat) és essencial per protegir correctament la informació crítica de l’organització.

---

## Material de suport

- Material de l’assignatura Seguretat Informàtica. RA3. Introducció a la criptografia (Moodle de l’assignatura).
- Tutorial oficial de VeraCrypt:
  https://veracrypt.io/en/Beginner's%20Tutorial.html
