# Auditoria y Analisis de Vulnerabilidades en Infraestructura Cloud (OpenVAS / Metasploitable)

## 1. Resumen Ejecutivo y Alcance Operativo
Este repositorio documenta el proceso de auditoría y análisis de vulnerabilidades realizado sobre la infraestructura tecnológica simulada de la entidad Soporte Experto Educación Superior (SEES). Considerando la designación operativa de SEES como Operador de Importancia Vital (OIV) bajo el marco normativo de la Ley Marco de Ciberseguridad (Ley 21.663), se ejecutó una evaluación práctica utilizando la plataforma OpenVAS desplegada sobre un entorno Kali Linux, evaluando un servidor objetivo vulnerable (Metasploitable).

El objetivo principal de este proyecto es evidenciar competencias técnicas en la identificación de brechas perimetrales, validación de controles defensivos, análisis de riesgos bajo métricas CVSS y la propuesta de contramedidas y mitigaciones orientadas a asegurar la continuidad operativa y la protección de la información.

---

## 2. Especificaciones del Entorno de Evaluación
La auditoría se llevó a cabo en un entorno virtualizado aislado, compuesto por las siguientes características técnicas:

* **Estación de Auditoría (Kali Linux / OpenVAS):**
  - Sistema Operativo: Debian (64-bit) / OpenVAS Scanner
  - Dirección IP: `192.168.56.131` (Interfaz Host-only)
  - Subred: `255.255.255.0`

* **Servidor Objetivo (Metasploitable):**
  - Sistema Operativo: Ubuntu 14.04 (Simulación de cargas vulnerables)
  - Dirección IP: `192.168.56.103`
  - Subred: `255.255.255.0`

---

## 3. Definición y Justificación de Controles de Seguridad
Para cumplir con los estándares normativos exigidos a un Operador de Importancia Vital (OIV), se estructuraron controles preventivos y detectivos:
* **Protección de Datos (IE1.1.1):** Implementación de cifrado robusto en reposo y en tránsito mediante AES-256 y TLS 1.3, asegurando la confidencialidad de la información institucional. Aplicación estricta del Control de Acceso Basado en Roles (RBAC) bajo el principio de mínimo privilegio.
* **Protección de la Infraestructura (IE1.1.2):** Despliegue conceptual de un Firewall de Próxima Generación (NGFW) para inspección de tráfico a nivel de aplicación y un Sistema de Detección/Prevención de Intrusos (IDS/IPS) enfocado en el monitoreo perimetral transparente.

---

## 4. Hallazgos Críticos y Análisis de Vulnerabilidades
El análisis automatizado mediante OpenVAS sobre el objetivo (`192.168.56.103`) permitió identificar vectores de riesgo relevantes:
1. **SSL/TLS: Report Vulnerable Cipher Suites for HTTPS (CVSS 7.5):** Uso de cifrados de bloques débiles (3DES) vulnerables al ataque SWEET32, permitiendo la posible interceptación de tráfico cifrado.
2. **SSH Weak Key Exchange (KEX) Algorithms Supported (CVSS 5.3):** Presencia de algoritmos de intercambio de claves obsoletos (como Diffie-Hellman con SHA-1 en 1024 bits).
3. **SSH Weak Host Key Algorithm(s) (CVSS 5.3):** Configuración permitiendo el uso del algoritmo de firma digital obsoleto (ssh-dss).
4. **FTP Unencrypted Cleartext Login (CVSS 4.8):** Autenticación permitida en texto plano sin exigir comandos de cifrado explícitos (`AUTH TLS`).
5. **Cleartext Transmission of Sensitive Information via HTTP (CVSS 4.8):** Transmisión de credenciales y datos de sesión sin forzar redirección automática a canales seguros HTTPS.

---

## 5. Propuestas de Mitigación y Remediación Técnica
* **Para HTTPS:** Deshabilitar formalmente las suites de cifrado débiles (3DES y RC4) y restringir los protocolos de comunicación exclusivamente a TLS 1.2 y TLS 1.3.
* **Para SSH:** Actualizar la configuración del servicio (`sshd_config`) eliminando algoritmos KEX y claves de host obsoletas (desactivando `ssh-dss`), priorizando curvas elípticas modernas.
* **Para FTP:** Forzar el uso obligatorio de FTP seguro sobre TLS (FTPS) o migrar completamente la administración a canales cifrados como SFTP.
* **Para Aplicaciones Web:** Implementar redirecciones automáticas perimetrales desde HTTP hacia HTTPS para impedir la exposición de credenciales en texto claro.

---

## Evidencias Documentales y Reportes
* `reports/screenshots.pdf`: Registro gráfico anonimizado de la configuración de objetivos, tareas de escaneo en Greenbone Security Assistant (OpenVAS) y el detalle de los reportes de vulnerabilidades detectadas.

*Nota: Este repositorio representa un estándar metodológico y técnico replicable para auditorías defensivas en ciberseguridad y cumplimiento normativo cloud.*
