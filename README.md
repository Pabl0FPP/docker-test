# 🐳 Taller Docker Test - Rick & Morty App

**Autor:** Pablo Fernando Pineda P. - A00395831

## 📋 Descripción del Proyecto

Aplicación web desarrollada en **React** que muestra personajes de Rick & Morty. El proyecto implementa **Docker** para contenerización y **GitHub Actions** para CI/CD automático, demostrando las mejores prácticas de desarrollo moderno.

## 🚀 Guía de Ejecución Paso a Paso

### Prerrequisitos

- Docker instalado
- Node.js 18+ (para desarrollo local)
- Cuenta en Docker Hub
- Repositorio en GitHub

---

### 📝 Paso 1: Configuración del Dockerfile

Crear el archivo `Dockerfile` en la raíz del proyecto:

```dockerfile
FROM node:20
WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . .
EXPOSE 3000
CMD ["npm", "run", "dev"]
```

**Explicación:**
- Utiliza Node.js 20 como imagen base
- Establece `/app` como directorio de trabajo
- Copia dependencias e instala packages
- Expone el puerto 3000 para la aplicación React

---

### ⚙️ Paso 2: Configuración de GitHub Actions

Crear el archivo `.github/workflows/build-and-push.yml`:

```yaml
name: Build and Push Docker Image

on:
  push:
    branches: [ main, master ]
  pull_request:
    branches: [ main, master ]

jobs:
  build:
    runs-on: ubuntu-latest
    
    steps:
    - name: Checkout code
      uses: actions/checkout@v4
    
    - name: Set up Docker Buildx
      uses: docker/setup-buildx-action@v3
    
    - name: Login to Docker Hub
      uses: docker/login-action@v3
      with:
        username: ${{ secrets.DOCKER_USERNAME }}
        password: ${{ secrets.DOCKER_PASSWORD }}
    
    - name: Build and push Docker image
      uses: docker/build-push-action@v5
      with:
        context: .
        push: true
        tags: ${{ secrets.DOCKER_USERNAME }}/rick-morty-app:latest
```

---

### 🔑 Paso 3: Configuración de Secretos en Docker Hub

1. **Crear Access Token en Docker Hub:**
   - Ir a Docker Hub → Account Settings → Security
   - Crear nuevo Access Token con permisos **Read, Write, Delete**
   - Copiar el token generado

![Docker Hub Token](https://github.com/user-attachments/assets/c034b62d-7288-4da8-a4d6-d259e5f16d1a)

2. **Configurar secretos en GitHub:**
   - Repositorio → Settings → Secrets and variables → Actions
   - Agregar `DOCKER_USERNAME`: tu usuario de Docker Hub
   - Agregar `DOCKER_PASSWORD`: el token generado

---

### ✅ Paso 4: Ejecución del Pipeline CI/CD

Al hacer push al repositorio, GitHub Actions ejecuta automáticamente:

1. **Checkout:** Descarga el código fuente
2. **Docker Buildx:** Configura herramientas de build avanzadas
3. **Docker Login:** Autentica con Docker Hub usando secretos
4. **Build & Push:** Construye la imagen y la sube al registro

![Successful GitHub Action](https://github.com/user-attachments/assets/d5defec6-2ec6-4604-a36c-3e63df5cd730)

---

### 📦 Paso 5: Verificación en Docker Hub

La imagen se publica exitosamente en Docker Hub y está disponible para descarga:

![Docker Hub Repository](https://github.com/user-attachments/assets/3729a739-27f1-4620-adf7-f602fe462018)

---

## 🖥️ Ejecución Local

### Ejecutar desde Docker Hub

```bash
# Descargar y ejecutar la imagen desde Docker Hub
docker run -p 3000:3000 tu-usuario/rick-morty-app:latest
```

![Terminal Execution](https://github.com/user-attachments/assets/1cfe0ea3-e03d-42f0-9932-789b1efcb3aa)

---

## 🌐 Acceso a la Aplicación

Una vez ejecutado el contenedor, la aplicación estará disponible en:

**URL:** http://localhost:3000

![Running Application](https://github.com/user-attachments/assets/ace5a026-f943-4d2b-b3c6-f8b5d5075461)

---
