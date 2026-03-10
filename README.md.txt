# 🚀 Home Lab: Reviviendo un Compaq CQ2000 (Bare Metal Private Cloud)

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