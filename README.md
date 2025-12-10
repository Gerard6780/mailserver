# Servidor de Correo Docker - gerard.test

Servidor de correo completo usando Docker con soporte para SMTP e IMAP para el dominio `gerard.test`.

## 📋 Requisitos Previos

- Docker Desktop instalado y ejecutándose
- Docker Compose
- Puertos disponibles: 25, 587, 465, 143, 993

## 🚀 Instalación

### 1. Iniciar el servidor

```powershell
docker-compose up -d
```

### 2. Verificar que el contenedor está ejecutándose

```powershell
docker-compose ps
```

### 3. Crear una cuenta de correo

Para crear un usuario de correo (por ejemplo: `usuario@gerard.test`):

```powershell
docker exec -it mailserver setup email add usuario@gerard.test
```

Se te pedirá que ingreses una contraseña para el usuario.

### 4. Listar cuentas de correo

```powershell
docker exec -it mailserver setup email list
```

## 📧 Configuración del Cliente de Correo

### Configuración IMAP (Recibir correos)
- **Servidor**: localhost o mail.gerard.test
- **Puerto**: 993 (SSL) o 143 (sin SSL)
- **Usuario**: tu-usuario@gerard.test
- **Contraseña**: la que configuraste

### Configuración SMTP (Enviar correos)
- **Servidor**: localhost o mail.gerard.test
- **Puerto**: 587 (STARTTLS) o 465 (SSL) o 25
- **Usuario**: tu-usuario@gerard.test
- **Contraseña**: la que configuraste
- **Autenticación**: Requerida

## 🧪 Pruebas

### Probar conexión SMTP con telnet

```powershell
telnet localhost 25
```

Luego escribe:
```
EHLO gerard.test
QUIT
```

### Enviar correo de prueba

Puedes usar cualquier cliente de correo (Thunderbird, Outlook, etc.) con la configuración anterior.

## 📝 Comandos Útiles

### Ver logs del servidor
```powershell
docker-compose logs -f mailserver
```

### Detener el servidor
```powershell
docker-compose down
```

### Reiniciar el servidor
```powershell
docker-compose restart
```

### Eliminar una cuenta de correo
```powershell
docker exec -it mailserver setup email del usuario@gerard.test
```

### Crear un alias de correo
```powershell
docker exec -it mailserver setup alias add alias@gerard.test usuario@gerard.test
```

### Acceder al contenedor
```powershell
docker exec -it mailserver bash
```

## 🔧 Configuración Avanzada

### Habilitar Anti-spam y Antivirus

Edita el archivo `.env` y cambia:
```
ENABLE_SPAMASSASSIN=1
ENABLE_CLAMAV=1
```

Luego reinicia:
```powershell
docker-compose down
docker-compose up -d
```

**Nota**: Esto aumentará el uso de recursos (RAM y CPU).

### Configurar DNS Local (Opcional)

Para que `mail.gerard.test` resuelva localmente, añade a tu archivo hosts:

**Windows**: `C:\Windows\System32\drivers\etc\hosts`
```
127.0.0.1 mail.gerard.test gerard.test
```

## 🐛 Solución de Problemas

### El contenedor no inicia
- Verifica que los puertos no estén en uso: `netstat -ano | findstr ":25 :587 :993"`
- Revisa los logs: `docker-compose logs mailserver`

### No puedo conectarme al servidor
- Verifica que el firewall de Windows permita las conexiones
- Asegúrate de que el contenedor está ejecutándose: `docker-compose ps`

### Olvidé la contraseña de un usuario
Elimina el usuario y créalo nuevamente:
```powershell
docker exec -it mailserver setup email del usuario@gerard.test
docker exec -it mailserver setup email add usuario@gerard.test
```

## 📂 Estructura de Archivos

```
mailserver/
├── docker-compose.yml          # Configuración de Docker
├── .env                        # Variables de entorno
├── README.md                   # Este archivo
└── docker-data/               # Datos persistentes (se crea automáticamente)
    └── dms/
        ├── mail-data/         # Buzones de correo
        ├── mail-state/        # Estado del servidor
        ├── mail-logs/         # Logs
        └── config/            # Configuración
```

## 🔒 Seguridad

- Este servidor usa certificados SSL auto-firmados por defecto
- Para producción, configura certificados válidos (Let's Encrypt)
- El dominio `.test` es solo para desarrollo/pruebas locales
- No expongas este servidor directamente a Internet sin configuración adicional de seguridad

## 📚 Recursos

- [Documentación docker-mailserver](https://docker-mailserver.github.io/docker-mailserver/latest/)
- [Configuración avanzada](https://docker-mailserver.github.io/docker-mailserver/latest/config/advanced/)
