🚀 Paso 1: Estructura del Repositorio (Los Archivos a Subir)Antes de escribir el README, necesitas organizar los archivos "inteligentes" que has creado. Crea carpetas dentro de tu repositorio para que todo esté ordenado.Carpeta / ArchivoQué debe contenerNota de Interés / Seguridad📂 docker/Esta carpeta centraliza tu infraestructura.├── docker-compose.ymlEl archivo maestro con los servicios de FileBrowser, Plex, Transmission, etc.⚠️ CRÍTICO: Reemplaza todas tus contraseñas reales por changeme o usa variables de entorno (${MY_PASSWORD}). No subas secretos.└── pihole/Subcarpeta opcional si tienes configs específicos.📂 scripts/Demuestra que sabes automatizar.└── backup.shUn script genérico basado en Restic (inspirado en la image_2.png) que muestre cómo haces el respaldo de /DATA al disco externo.Usa rutas genéricas como /mnt/backup_drive.📂 docs/Documentación extendida o esquemas.└── network-diagram.mdEl esquema ASCII o Mermaid.js del tráfico de tu red que definimos.Muy útil para visualizar tu arquitectura local.📂 assets/La parte visual que lo hace interesante.├── hero-technician.pngLa imagen estilo Ghibli del técnico montando el equipo (image_0.png).Úsala como portada del README.├── restic-monitor.pngLa imagen que muestra el monitoreo de Restic en progreso (image_2.png).Úsala en la sección de Backups.├── terminal-boot.jpgLa foto real que tomaste de la pantalla de arranque con compaqserver login.Autenticidad pura: "Bare Metal real".├── plex-dash.pngUna captura de pantalla de tu Plex funcionando (con IPs censuradas).Demuestra el resultado final.LICENSEUna licencia MIT o Apache 2.0.Demuestra que entiendes el open-source.Paso 2: El README.md Definitivo (Sección por Sección)Copia y pega este bloque y rellena las partes indicadas. He estructurado esto con un enfoque de marketing de ingeniería: Problema -> Solución Técnica -> Impacto.Markdown# 🚀 Home Lab: Reviviendo un Compaq CQ2000 (Bare Metal Private Cloud)

![Hero Image](./assets/hero-technician.png)

> "De hardware obsoleto a infraestructura crítica: un ejercicio de self-hosting, optimización y redes Zero-Trust."

Este proyecto documenta la transformación completa de un **HP Compaq CQ2000** (procesador de 2 núcleos, 12GB RAM DDR3), un equipo considerado e-waste, en un servidor doméstico de alta disponibilidad 24/7. El enfoque principal es la soberanía de datos, la privacidad y la maximización del rendimiento de hardware limitado mediante arquitectura **Bare Metal**.

---

## 📊 Especificaciones del Hardware (The Base)

* **Equipo:** HP Compaq CQ2000 (Mini-PC).
* **CPU:** AMD E-Series de 2 núcleos (Optimizado para bajo consumo).
* **RAM:** 12GB DDR3 (Suficiente para múltiples contenedores Docker).
* **Almacenamiento Local:** HDD Interno asignado a `/DATA` (Microservicios y Medios).
* **Almacenamiento de Respaldo:** Disco Externo Ruggedizado (Backup Dedicado).
* **OS:** Ubuntu Server 24.04 LTS (Bare Metal).

---

## 🛠️ Stack Tecnológico (Servicios Activos)

Utilizo **CasaOS** como panel de gestión visual sobre Docker para orquestar los siguientes servicios:

| Icono | Servicio | Función Crítica |
| :---: | :--- | :--- |
| 🛡️ | **Pi-hole** | DNS Sinkhole LAN-wide, Bloqueo de Publicidad y Servidor DHCP Maestro. |
| 🌐 | **Tailscale** | Red Mesh Zero-Trust (WireGuard) para acceso remoto seguro sin Port Forwarding. |
| 📁 | **FileBrowser** | Mi nube privada autogestionada con acceso WAN cifrado. |
| 🎬 | **Plex** | Media Server para transmisión de contenido local y remoto. |
| 📥 | **Transmission** | Cliente BitTorrent 24/7 autónomo para descargas desatendidas (ISOs, Medios). |
| 💾 | **Restic** | Backups incrementales, deduplicados y encriptados de `/DATA`. |

![Plex Dashboard](./assets/plex-dash.png)

---

## 🧠 Arquitectura de Red y Desafíos Técnicos (Troubleshooting)

Esta es la sección más importante. No todo fue fácil, y así lo resolvimos:

### 1. Superando las Restricciones del ISP (DHCP Autogestionado)
**Desafío:** El router Sagemcom 5670 de mi ISP bloqueaba la modificación de DNS a nivel de red, impidiendo que Pi-hole filtrara el tráfico.
**Solución:** Desactivé el DHCP del router ISP y delegué la autoridad de red al contenedor Pi-hole en el Compaq. Esto requirió configurar **UFW** (Firewall) para abrir los sockets críticos:
```bash
sudo ufw allow 67/udp
sudo ufw allow 68/udp
sudo ufw reload
2. El Conflicto del Puerto 53 (systemd-resolved)Desafío: Pi-hole no podía arrancar porque el puerto 53 (DNS) estaba ocupado por el servicio nativo systemd-resolved.Solución: Desactivé el servicio persistente en Ubuntu y forcé una configuración de DNS estática temporal para permitir el despliegue del contenedor.Bashsudo systemctl stop systemd-resolved
sudo systemctl disable systemd-resolved
3. Recuperación de Desastres: Bypass de CredencialesDesafío: Un bloqueo de login en CasaOS tras un cambio de red. Las herramientas de la CLI habituales fallaban por incompatibilidad de flags.Solución (Engineering Approach): Manipulación directa del sistema de archivos. Se procedió a la eliminación manual de la base de datos de usuarios (user.db) y reinicio del daemon, forzando un estado de Uninitialized que permitió la re-creación administrativa conservando la persistencia de datos de los contenedores Docker.4. Estrategia de Backups AutomáticosImplementé Restic para respaldar la carpeta crítica /DATA hacia un disco externo dedicado. El sistema realiza copias incrementales y deduplicadas, minimizando el impacto en el almacenamiento y en los 2 núcleos del procesador.
