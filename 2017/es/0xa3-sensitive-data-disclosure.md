# A3:2017 Exposición de Datos Sensibles

| Agentes de amenaza/Vectores de ataque | Debilidad de seguridad           | Impactos               |
| -- | -- | -- |
| Nivel de Acceso  \| Explotabilidad 2 | Prevalencia 3 \| Detección 2 | Técnico 3 \| Negocio |
| Hasta los atacantes anónimos, típicamente no rompen directamente la criptografía. Ellos rompen algo más, tal como robar claves, hacer ataques del tipo man-in-the-middle, o robar datos en forma de texto directamente desde el servidor, mientras estos datos se encuentran en tránsito, o como cliente, por ejemplo desde el navegador del usuario. Ataques manuales son generalmente requeridos. | Desde los últimos años, este ha sido el ataque de gran impacto más común. La falla más común es simplemente no encriptar los datos sensibles. Cuando la criptografía es empleada, la generación de claves débiles, el débil manejo de las claves, el uso de algoritmos débiles es común, particularmente técnicas débiles de hashing de contraseñas. Para los datos en tránsito, las debilidades del lado del servidor son fáciles de detectar, pero difíciles para los datos en reposo. Ambas son de bastante y variada explotación. | El fracaso frecuentemente compromete todos los datos que debieron haber sido protegidos. Típicamente, esta información incluye información personal sensible como archivos historiales de salud, credenciales, datos personales, datos de tarjetas de crédito, los cual en ocasiones requieren protección según dictan las leyes y regulaciones, tales como la regulación de la Unión Europea, GDPR (Regulación General de Protección de Datos) o leyes locales de privacidad. |

## Soy Vulnerable a la Exposición de datos?

Lo primero a determinar son las necesidades de protección de los datos en tránsito y en reposo. Por ejemplo, contraseñas, números de tarjetas de crédito, historiales médicos y la información personal requieren protección extra, particularmente si estos datos caen bajo la regulación de la Unión Europea, GDPR (Regulación General de Protección de Datos), regulaciones o leyes locales de privacidad, regulaciones de protección de datos financieros, tales como el Acuerdo PCI DSS del Consejo sobre Normas de Seguridad de la PCI (PCI Security Standards Council, LLC), o leyes de historiales médicos, tales como la ley de Transparencia y Responsabilidad de Seguro Médico (HIPAA). Para todo tipo de datos:

* ¿Es algún dato de un sitio transmitido en texto plano, de forma interna o externa? El tráfico de Internet es especialmente peligroso, pero desde el que balancea las cargas hasta los servidores web o desde los servidores web hasta los sistemas de back end, puede ser problemático.
* ¿Son los datos sensibles guardados en texto plano, incluyendo los respaldos?
¿Hay algún algoritmo criptográfico débil o viejo siendo usado por defecto o en código más viejo? (ver A6:2017 Configuración de Seguridad Incorrecta)
* ¿Existen claves criptográficas por defecto en uso, claves de seguridad criptográficas débiles generadas o re-utilizadas, o falta de rotación o manejo apropiado de la clave?
* ¿La encriptación es no forzada, por ejemplo, existe alguna directiva de seguridad o cabezales faltantes de algún agente de usuario(navegador)?

See ASVS areas [Crypto (V7), Data Protection (V9) and SSL/TLS (V10)](https://www.owasp.org/index.php/ASVS)

## Como prevenirlo?

Do the following, at a minimum and consult the references:

* Classify data processed, stored or transmitted by a system. Apply controls as per the classification.
Review the privacy laws or regulations applicable to sensitive data, and protect as per regulatory requirements
* Don’t store sensitive data unnecessarily. Discard it as soon as possible or use PCI DSS compliant tokenization or even truncation. Data you don’t retain can’t be stolen.
* Make sure you encrypt all sensitive data at rest 
* Encrypt all data in transit, such as using TLS. Enforce this using directives like HTTP Strict Transport Security (HSTS).
* Ensure up-to-date and strong standard algorithms or ciphers, parameters, protocols and keys are used, and proper key management is in place. Consider using [crypto modules](http://csrc.nist.gov/groups/STM/cmvp/documents/140-1/140val-all.htm).
* Ensure passwords are stored with a strong adaptive algorithm appropriate for password protection, such as [Argon2](https://www.cryptolux.org/index.php/Argon2), [scrypt](http://en.wikipedia.org/wiki/Scrypt), [bcrypt](http://en.wikipedia.org/wiki/Bcrypt) and [PBKDF2](http://en.wikipedia.org/wiki/PBKDF2). Configure the work factor (delay factor) as high as you can tolerate.
* Disable caching for response that contain sensitive data.
Verify independently the effectiveness of your settings.

## Ejemplo de Escenarios de Ataque

**Escenario #1**:  An application encrypts credit card numbers in a database using automatic database encryption. However, this data is automatically decrypted when retrieved, allowing an SQL injection flaw to retrieve credit card numbers in clear text. 

**Escenario #2**: A site doesn't use or enforce TLS for all pages, or if it supports weak encryption. An attacker simply monitors network traffic, strips or intercepts the TLS (like an open wireless network), and steals the user's session cookie. The attacker then replays this cookie and hijacks the user's (authenticated) session, accessing or modifying the user's private data. Instead of the above he could alter all transported data, e.g. the recipient of a money transfer.

**Escenario #3**: The password database uses unsalted hashes to store everyone's passwords. A file upload flaw allows an attacker to retrieve the password database. All the unsalted hashes can be exposed with a rainbow table of pre-calculated hashes.

## Referencias


* [OWASP Controles Proactivos - Protección de Datos](https://www.owasp.org/index.php/OWASP_Proactive_Controls#7:_Protect_Data)
* [OWASP Standard de Verificación de Seguridad de Aplicaciones - V9, V10, V11](https://www.owasp.org/index.php/Category:OWASP_Application_Security_Verification_Standard_Project)
* [OWASP Cheat Sheet - Protección de Capa Transporte](https://www.owasp.org/index.php/Transport_Layer_Protection_Cheat_Sheet)
* [OWASP Cheat Sheet - Protección de Seguridad de Usuario](https://www.owasp.org/index.php/User_Privacy_Protection_Cheat_Sheet)
* [OWASP Cheat Sheet - Almacenamiento de Contraseña](https://www.owasp.org/index.php/Password_Storage_Cheat_Sheet)
* [OWASP Cheat Sheet - Almacenamiento Criptográfico](https://www.owasp.org/index.php/Cryptographic_Storage_Cheat_Sheet)
* [OWASP Proyecto de Cabezales de Seguridad](https://www.owasp.org/index.php/OWASP_Secure_Headers_Project)
* [OWASP Guía de Testing - Testing para la criptografía débil](https://www.owasp.org/index.php/Testing_for_weak_Cryptography)

### Externas

* [CWE-359 Exposición de Información Privada - Violación de Privacidad](https://cwe.mitre.org/data/definitions/359.html)
* [CWE-220 Exposición de información sensible a través de consultas de datos](https://cwe.mitre.org/data/definitions/220.html)
* [CWE-310 Problemas Criptográficos](https://cwe.mitre.org/data/definitions/310.html)
* [CWE-312 Almacenamiento en Texto plano de Información Sensible](https://cwe.mitre.org/data/definitions/312.html)
* [CWE-319 Transmisión en Texto plano de Información Sensible](https://cwe.mitre.org/data/definitions/319.html)
* [CWE-326 Encriptación Débil](https://cwe.mitre.org/data/definitions/326.html)
