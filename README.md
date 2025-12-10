# BillionMail - Servidor de Correo en Red Local

Servidor de correo completo para despliegue en red local (LAN) usando Docker. 
Diseñado para ser accedido fácilmente desde cualquier dispositivo en la misma red mediante IP, manteniendo el dominio interno `gerard.test`.

## 📋 Requisitos Previos

- **Docker** y **Docker Compose** instalados
- **Puertos disponibles**: 25, 80, 110, 143, 443, 465, 587, 993, 995
- **IP Local Fija recomendada** en la máquina servidor

## 🚀 Pasos de Despliegue

### 1. Configuración Inicial

Copia la plantilla de configuración:

```powershell
# Windows
Copy-Item env_init .env
```
```bash
# Linux/macOS
cp env_init .env
```

El archivo `.env` ya viene preconfigurado con:
- **Red Interna Segura**: `172.22.1.0/24` (para evitar conflictos)
- **Dominio de Correo**: `mail.gerard.test` (usado internamente por el sistema de correo)

### 2. Levantar el Servidor

```powershell
docker-compose up -d
```

Espera unos minutos a que todos los servicios (Base de datos, Antispam, ClamAV, etc.) inicien correctamente.

## 🌐 Acceso desde la Red Local

A diferencia de un despliegue público, aquí accederemos usando la **IP Local** del ordenador donde está corriendo Docker.

### 1. Descubrir tu IP Local del Servidor

En la máquina donde corre Docker:
- **Windows**: `ipconfig` -> Busca "Dirección IPv4" (ej: `192.168.1.50`)
- **Linux/Mac**: `ip addr` o `ifconfig`

### 2. Acceder al Panel de Administración

Desde cualquier PC/Móvil en la misma red WiFi/Cable:
```
http://<TU_IP_LOCAL>/billion
```
*Ejemplo: http://192.168.1.50/billion*

Usuario: `admin`
Contraseña: `pirineus`

### 3. Acceder al Webmail (Roundcube)

```
http://<TU_IP_LOCAL>/roundcube
```
*Ejemplo: http://192.168.1.50/roundcube*

## 📧 Configuración de Clientes (Outlook, Thunderbird, Móvil)

Para conectar tu gestor de correo sin configurar DNS ni archivo hosts.

### Datos de la Cuenta
- **Email**: `usuario@gerard.test` (El dominio es cosmético pero necesario)
- **Contraseña**: La que hayas creado en el panel.

### Servidor Entrante (IMAP)
- **Servidor**: `<TU_IP_LOCAL>` (ej: `192.168.1.50`)
- **Puerto**: `143` (STARTTLS/Sin seguridad) o `993` (SSL/TLS - *Acepta el certificado inseguro*)

### Servidor Saliente (SMTP)
- **Servidor**: `<TU_IP_LOCAL>` (ej: `192.168.1.50`)
- **Puerto**: `587` (STARTTLS) o `465` (SSL/TLS)
- **Autenticación**: Sí, misma que entrada.

> [!NOTE]
> Al usar IPs y certificados autofirmados, tus clientes de correo mostrarán avisos de seguridad. Debes **aceptar/confiar** en el certificado para continuar.

## 🛠️ Comandos de Mantenimiento

```powershell
# Ver estado
docker-compose ps

# Ver logs en tiempo real
docker-compose logs -f

# Parar el servidor
docker-compose down
```

## ⚠️ Nota sobre el Dominio
Aunque accedas por IP (`192.168.x.x`), el servidor necesita un nombre de dominio interno para gestionar los correos (el `@gerard.test`). 
- No necesitas comprar este dominio.
- No necesitas configurar DNS si sigues esta guía de acceso por IP.
- Los correos solo funcionarán **dentro de tu red local** o entre usuarios de este servidor.
