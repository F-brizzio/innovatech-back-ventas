# 🚀 Innovatech Back-Ventas — Spring Boot

API REST de gestión de ventas para **Innovatech Chile**, desarrollada con Spring Boot (Java 17), contenedorizada con Docker y desplegada en AWS EC2.

## 🛠️ Tecnologías
- Java 17 + Spring Boot 3.4
- Spring Data JPA + MySQL
- Maven
- Docker multi-stage build

## 📋 Requisitos
- Docker Desktop instalado
- Java 17+ (solo desarrollo local)

## 🐳 Ejecutar con Docker

### Build de la imagen
```bash
docker build -t innovatech-back-ventas ./Springboot-API-REST
```

### Correr el contenedor
```bash
docker run -d -p 8082:8081 \
  --name innovatech-back-ventas \
  -e DB_ENDPOINT=localhost \
  -e DB_PORT=3306 \
  -e DB_NAME=innovatech_db \
  -e DB_USERNAME=innovatech \
  -e DB_PASSWORD=innova1234 \
  innovatech-back-ventas
```

## 🔧 Variables de entorno
| Variable | Descripción |
|----------|-------------|
| `DB_ENDPOINT` | Host de la base de datos MySQL |
| `DB_PORT` | Puerto MySQL (3306) |
| `DB_NAME` | Nombre de la base de datos |
| `DB_USERNAME` | Usuario MySQL |
| `DB_PASSWORD` | Contraseña MySQL |

## 📦 Estructura del Dockerfile
El Dockerfile usa **multi-stage build**:
- **Stage 1 (builder):** Maven 3.9 + JDK 17 Alpine — compila y empaqueta el JAR
- **Stage 2 (production):** Eclipse Temurin JRE 17 Alpine — ejecuta el JAR

Buenas prácticas aplicadas:
- ✅ Multi-stage build (imagen final sin Maven)
- ✅ Usuario no root
- ✅ Cache de dependencias con dependency:go-offline
- ✅ Imagen mínima Alpine

## 🔄 Pipeline CI/CD
El pipeline se activa automáticamente con cada push a la rama `deploy`:
1. Construye la imagen Docker
2. Publica en Docker Hub (`lucianodelgado/innovatech-back-ventas`)
3. Despliega en EC2 vía SSH

### Secrets requeridos en GitHub
| Secret | Descripción |
|--------|-------------|
| `DOCKER_USERNAME` | Usuario Docker Hub |
| `DOCKER_TOKEN` | Token Docker Hub |
| `EC2_BACKEND_HOST` | IP pública EC2 Backend |
| `EC2_SSH_KEY` | Clave privada SSH (.pem) |
| `DB_ENDPOINT` | Host MySQL |
| `DB_NAME` | Nombre BD |
| `DB_USERNAME` | Usuario BD |
| `DB_PASSWORD` | Contraseña BD |

## ☁️ Despliegue en AWS
- **Instancia:** EC2 Ubuntu 24.04 LTS (t2.micro)
- **IP:** 98.82.222.182
- **Puerto:** 8082
- **API Docs:** http://98.82.222.182:8082/swagger-ui.html
