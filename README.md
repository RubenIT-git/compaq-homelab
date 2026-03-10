# 🚀 Home Lab: Compaq CQ2000 Bare Metal Server

![Terminal de Ubuntu](./assets/ee25ccd8-95b3-475a-800c-423089b5ef1f.jpg)

Este proyecto documenta la transformación de un **HP Compaq CQ2000** (2 núcleos, 12GB RAM DDR3) de hardware en desuso a un servidor doméstico de alta disponibilidad. "Todo está para el futuro".

---

## 📊 Especificaciones del Hardware
* **Modelo:** HP Compaq CQ2000.
* **CPU:** 2 Núcleos (Arquitectura x86_64).
* **RAM:** 12GB DDR3 (Optimizado para microservicios).
* **OS:** Ubuntu Server 24.04.4 LTS (Bare Metal).

---

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
```


2. Bypass de Credenciales en CasaOS
Problema: Bloqueo de login por cambio de IP y error en flags de reseteo.
Solución: Eliminación manual de la base de datos de identidad para forzar un nuevo inicio:
```
sudo rm /var/lib/casaos/db/user.db
sudo systemctl restart casaos-user-service
```


3. Red Local (DHCP Maestro)
Para saltar las restricciones del router ISP (Sagemcom 5670), delegué la autoridad DHCP al servidor:

Configuración: Puertos abiertos 67/udp y 68/udp en el firewall (ufw).


📂 Conectividad
Acceso remoto garantizado mediante la IP fija de Tailscale (100.x.x.x), permitiendo gestionar archivos y descargas desde cualquier red móvil 5G.


🕰️ Análisis de Vida Útil: Maximizando el ROI Tecnológico
Este proyecto constituye un caso de estudio sobre la extensión radical del ciclo de vida del hardware. Al rescatar el Compaq CQ2000 y reimplementarlo como un servidor Bare Metal, hemos logrado duplicar su vida útil operativa.


### 📊 Métricas de Extensión de Vida (Análisis de Impacto)

| Métrica | Estado Original (Estimado) | Estado Actual (Home Lab) | Impacto de Ingeniería |
| :--- | :--- | :--- | :--- |
| **Ciclo de Vida Útil** | ~5-7 años | **~12+ años (y contando)** | **Duplicación de vida operativa** mediante cambio de rol. |
| **Rol de Hardware** | Estación Windows | **Nodo de Infraestructura Crítica** | Maximización de la utilidad marginal del hardware. |
| **Eficiencia de SW** | OS con GUI pesada | **Ubuntu Server Bare Metal** | Eliminación completa de *overhead* del sistema. |


🔧 Guía de Mantenimiento Preventivo (SOP)
Para asegurar que el Compaq CQ2000 siga operativo, he definido un SOP mensual:

1. Actualización del Sistema
  ```
 sudo apt update && sudo apt upgrade -y

```
2. Higiene de Docker
```
Bash
docker system prune -a --volumes
```

3. Salud de la Red (Pi-hole)
```
Bash
pihole -g
```


4. Verificación de Backups (Restic)
```
Bash
restic -r /mnt/backup_drive check
```


Nota de Ingeniería: La consistencia en el mantenimiento es lo que diferencia a un "entusiasta" de un "administrador de sistemas". Documentación creada por RubenIT-git para su portafolio profesional.
