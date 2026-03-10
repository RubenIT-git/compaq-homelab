# 🚀 Home Lab: Compaq CQ2000 Bare Metal Server

![Terminal de Ubuntu](./assets64dcce9b-47d0-4858-a5a5-a051cc265922.jpeg)

Este proyecto documenta la transformación de un **HP Compaq CQ2000** (2 núcleos, 12GB RAM DDR3) de hardware en desuso a un servidor doméstico de alta disponibilidad. "Todo está para el futuro".

## 📊 Especificaciones del Hardware
* **Modelo:** HP Compaq CQ2000.
* **CPU:** 2 Núcleos (Arquitectura x86_64).
* **RAM:** 12GB DDR3 (Optimizado para microservicios).
* **OS:** Ubuntu Server 24.04.4 LTS (Bare Metal).

## 🛠️ Stack de Software
Gestiono el servidor mediante **CasaOS** y los siguientes servicios en **Docker**:
* **🛡️ Pi-hole:** DNS Sinkhole y Servidor DHCP para toda la red local.
* **🌐 Tailscale:** Red Mesh VPN para acceso remoto seguro.
* **📁 FileBrowser:** Gestión de archivos privados con acceso desde el móvil.
* **🎬 Plex:** Servidor multimedia para transmisión en Smart TVs.
* **📥 Transmission:** Cliente de descargas autónomo 24/7.

---

## 🧠 Bitácora de Troubleshooting (Solución de Problemas)

### 1. Conflicto del Puerto 53 (DNS)
**Problema:** `systemd-resolved` ocupaba el puerto 53, impidiendo el arranque de Pi-hole.
**Solución:**
```bash
sudo systemctl stop systemd-resolved
sudo systemctl disable systemd-resolved

2. Bypass de Credenciales en CasaOS
Problema: Bloqueo de login por cambio de IP y error en flags de reseteo.
Solución: Eliminación manual de la base de datos de identidad para forzar un nuevo inicio:

Bash
sudo rm /var/lib/casaos/db/user.db
sudo systemctl restart casaos-user-service

3. Red Local (DHCP Maestro)
Para saltar las restricciones del router ISP (Sagemcom 5670), delegué la autoridad DHCP al servidor:

Puertos abiertos: 67/udp y 68/udp en el firewall (ufw).

📂 Conectividad
Acceso remoto garantizado mediante la IP fija de Tailscale (100.x.x.x), permitiendo gestionar archivos y descargas desde cualquier red móvil 5G.

Documentación creada por RubenIT-git para su portafolio profesional.


## 🕰️ Análisis de Vida Útil: Maximizando el Retorno de Inversión (ROI) Tecnológico

Este proyecto constituye un caso de estudio sobre la **extensión radical del ciclo de vida del hardware**. Al rescatar el Compaq CQ2000 (lanzado originalmente alrededor de 2011-2012) y reimplementarlo como un servidor *Bare Metal* en 2024, hemos logrado los siguientes hitos de ingeniería y sostenibilidad:

### 📊 Métricas de Extensión de Vida

| Métrica | Estado Original (Estimado) | Estado Actual (Home Lab) | Impacto de Ingeniería |
| :--- | :--- | :--- | :--- |
| **Ciclo de Vida Útil** | ~5-7 años (como PC de escritorio) | **~12+ años (y contando)** | **Duplicación de la vida operativa** mediante cambio de rol a servidor de bajo consumo. |
| **Rol de Hardware** | Estación de trabajo Windows (E-waste potencial) | **Nodo de Infraestructura Crítica Local** | Maximización de la utilidad marginal del hardware *legacy*. |
| **Eficiencia de Software** | OS con GUI pesada (obsoleto para este HW) | **Ubuntu Server Bare Metal (Optimizado)** | Eliminación de *overhead* para dedicar recursos al 100% a microservicios Docker. |

### 🧠 Lecciones Aprendidas y Estrategia de Futuro

1.  **Sostenibilidad Real:** La forma más efectiva de *Green IT* no es comprar hardware "eficiente" nuevo, sino **no fabricar hardware nuevo**. Al extender la vida de este equipo, hemos evitado la huella de carbono asociada a la producción y transporte de un nuevo servidor.
2.  **Ingeniería de Limitación:** Enfrentarse a las limitaciones de un procesador de 2 núcleos y memoria DDR3 obliga a realizar una **optimización de software rigurosa**, una habilidad crítica para cualquier administrador de sistemas.
3.  **El Mantra:** Como solemos decir en este proyecto: **"Todo está para el futuro"**. Esta máquina es la prueba de que, con la administración correcta, el pasado técnico puede ser la base del futuro digital.

---
*Este análisis documenta el compromiso de RubenIT-git con la ingeniería eficiente y sostenible.*

## 🔧 Guía de Mantenimiento Preventivo (SOP)

Para asegurar que el Compaq CQ2000 siga operativo "para el futuro", he definido un **Standard Operating Procedure (SOP)** mensual. Estos comandos garantizan que el almacenamiento no se llene de basura y que los servicios estén protegidos.

### 1. Actualización del Sistema y Seguridad
Mantiene el kernel de Ubuntu y los paquetes de seguridad al día:
```bash
sudo apt update && sudo apt upgrade -y
2. Higiene de Docker (Limpieza de Capas)
Docker acumula imágenes antiguas y volúmenes huérfanos que pueden agotar el disco duro. Una vez al mes ejecuto:

Bash
docker system prune -a --volumes
3. Salud de la Red (Pi-hole)
Actualizar la lista de bloqueo (Gravity) y el software del contenedor:

Bash
pihole -g
# O si usas Docker:
docker pull pihole/pihole && docker-compose up -d
4. Verificación de Backups (Restic)
Comprobar que los datos en el disco externo no están corruptos:

Bash
restic -r /mnt/backup_drive check

Nota de Ingeniería: La consistencia en el mantenimiento es lo que diferencia a un "entusiasta" de un "administrador de sistemas". Este servidor está diseñado para durar.




