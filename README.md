# HoneypotSeguroenVPS

Quizá pueda sonar un poco contradictorio un "Honeypot Seguro", al mencionar "Seguro" me refiero a perfectamente aislado, por lo que usaremos **Docker** para esta tarea, en un punto mientras el honeypot estaba operando se detectaron solicitudes automatizadas desde la misma IP por lo que se procedió a instalar y ejecutar **Fail2ban**, tambien se implementó un script en Python3 para que por medio de un bot de **Telegram** recibir logs y reportes en la misma aplicación, y por ultimo se instaló y ejecutó **Wazuh** (SIEM) para tener mas control y generar reportes.

## Fecha de implementación: Noviembre 2025
## Proyecto: Sistema de detección y análisis de accesos SSH maliciosos
## Entorno: Servidor VPS (Ubuntu Server 22.04 LTS)
## Herramienta principal: Cowrie Honeypot
## Modo de despliegue: Contenedor Docker

# Objetivos del Proyecto:
Implementar un honeypot SSH funcional, estable y totalmente aislado.
Capturar y registrar intentos de conexión, credenciales utilizadas y comandos ejecutados por atacantes.
Analizar patrones de comportamiento malicioso en tiempo real.
Integrar la infraestructura con Wazuh SIEM para la correlación y visualización de alertas.
Garantizar que la ejecución del honeypot no afecte la seguridad ni estabilidad del VPS principal.

## Recursos
Servidor VPS: Ubuntu Server 22.04 LTS (1 vCPU, 2 GB RAM, 20 GB SSD).
Docker Engine: Para aislar la instancia del honeypot y facilitar su gestión.
Cowrie: Honeypot SSH diseñado para registrar sesiones falsas, actividad de comandos y movimiento lateral simulado.

# Instalación y Configuración
## Preparación del Servidor VPS

## Actualizamos los repositorios y paquetes del sistema:

sudo apt update && sudo apt upgrade -y

## Configuramos el firewall UFW para proteger el sistema:
sudo ufw default deny incoming
sudo ufw default allow outgoing
sudo ufw allow 22/tcp      # SSH VPS
sudo ufw allow 2222/tcp    # Honeypot SSH
sudo ufw enable

## Instalamos Docker y Docker Compose:
sudo apt install docker.io docker-compose -y

## Habilita y arranca el servicio:
sudo systemctl enable docker
sudo systemctl start docker

# Despliegue del Honeypot Cowrie

## Creamos un directorio de trabajo para el proyecto:
mkdir /opt/cowrie && cd /opt/cowrie

## Clonamos el repositorio oficial de Cowrie:
git clone https://github.com/cowrie/cowrie.git

## docker run -d \
--name cowrie \
-p 22:2222 \
cowrie/cowrie:latest

## Verificamos que el contenedor esté activo:
docker ps

# Validación del Honeypot

## Comprobamos que el servicio está escuchando correctamente
netstat -tulnp | grep 22

## Pruebamos una conexión SSH (desde otra computadora)
ssh root@<IP_del_VPS>

### El honeypot responderá con un shell falso, registrando el intento y cualquier comando introducido.
### Los registros se almacenan automáticamente en formato JSON para su posterior análisis.

# Consultamos los logs generados por Cowrie:
## Nota: Según foros y documentación sobre cowrie, para consultar logs seria de la siguiente manera: docker logs cowrie, Pero no fue mi caso, aunque indagando un poco la ruta donde estaban alojados los logs era: docker logs honeypot-cowrie

##




