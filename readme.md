# 🐳 Odoo 16 con Docker sobre Ubuntu Server 24.04  
## Máquina Virtual Educativa (VirtualBox)

Este proyecto proporciona una **máquina virtual completamente funcional y lista para usar** que ejecuta **Odoo 16** mediante **Docker**, junto con **PostgreSQL**, sobre **Ubuntu Server 24.04**.

La solución está diseñada específicamente para **uso educativo**, prácticas de aula, formación en **DAM / DAW**, y para introducir al alumnado en **entornos reales de despliegue de software profesional**.

---

## 🎯 Objetivos del proyecto

- Disponer de un entorno Odoo funcional y estable
- Evitar instalaciones complejas en el sistema operativo
- Introducir conceptos reales de:
  - Virtualización
  - Contenedores (Docker)
  - Arquitectura cliente-servidor
  - Redes (NAT y puente)
- Facilitar un entorno común para todo el alumnado

---

## 📦 Descarga de la máquina virtual (OVA)

⚠️ GitHub **NO permite subir archivos grandes** como máquinas virtuales.

La máquina virtual se distribuye como archivo **`.ova`**, listo para importar en VirtualBox.

👉 **Descarga desde Google Drive**:  
🔗 https://drive.google.com/drive/folders/13PnemSHQJgSO7fTeZ6fMLF1mJNoaT4Te?usp=sharing

---

## 💻 Requisitos previos

### En el equipo HOST (Windows o Linux)

- **VirtualBox** instalado
- Mínimo recomendado:
  - 8 GB de RAM
  - 20 GB libres en disco
  - CPU con virtualización activada (VT-x / AMD-V)

---

## 📥 Instalación de VirtualBox

### 🪟 En Windows

1. Descargar VirtualBox desde:
   https://www.virtualbox.org/wiki/Downloads
2. Instalar:
   - VirtualBox
   - Extension Pack (misma versión)
3. Reiniciar el sistema si lo solicita

---

### 🐧 En Linux (Ubuntu / Debian)

```bash
sudo apt update
sudo apt install virtualbox -y
````

(O bien usar el repositorio oficial de VirtualBox para versiones más recientes)

---

## 📦 Importar la máquina virtual (OVA)

1. Abrir **VirtualBox**
2. Menú **Archivo → Importar servicio virtualizado**
3. Seleccionar el archivo `.ova`
4. Importar
5. Ajustar recursos si es necesario:

   * RAM: 2–4 GB
   * CPU: 2
6. Arrancar la VM

---

## 🐧 Sistema operativo de la VM: Ubuntu Server 24.04

**Ubuntu Server** es una distribución Linux orientada a servidores:

* No tiene entorno gráfico
* Es ligera y estable
* Muy usada en entornos profesionales
* Ideal para:

  * servidores web
  * bases de datos
  * contenedores
  * cloud y virtualización

---

## 🧱 Arquitectura general

- **Sistema base**: Ubuntu Server 24.04
- **Odoo**: versión 16 (imagen oficial Docker)
- **Base de datos**: PostgreSQL (contenedor independiente)
- **Gestión**: Docker Compose
- **Acceso**: navegador web (puerto 8069)

📌 Odoo **NO está instalado directamente** en el sistema operativo.

---

## 🌐 Configuración de red de la máquina virtual

La VM utiliza **DOS adaptadores de red**, con objetivos distintos.

---

### 🔹 Adaptador 1 — NAT

* Proporciona acceso a Internet a la VM
* Permite:

  * instalar paquetes
  * descargar imágenes Docker
* Permite acceso desde el host mediante **port forwarding**

#### Acceso web desde el host:

```
http://localhost:8069
```

#### Acceso SSH:

```bash
ssh usuario@localhost -p 2222
```

👉 **Modo recomendado en aulas**

---

### 🔹 Adaptador 2 — Adaptador puente

* Asigna una IP real de la red local
* Permite que la VM actúe como **servidor real**
* Accesible desde otros equipos de la red

#### Acceso web:

```
http://IP_DE_LA_VM:8069
```

#### Acceso SSH:

```bash
ssh usuario@IP_DE_LA_VM
```

⚠️ Puede desactivarse si la red del centro lo bloquea.

---

## 🧠 ¿Por qué usar dos redes?

* NAT → compatibilidad y facilidad
* Puente → realismo profesional

Permite explicar conceptos reales de **redes y servidores**.

---

## 🧩 ¿Qué es Odoo?

**Odoo** es un **ERP (Enterprise Resource Planning)** de código abierto que permite gestionar:

* Ventas
* CRM
* Almacén
* Facturación
* Contabilidad
* Recursos Humanos
* Proyectos

Se utiliza ampliamente en empresas reales.

En este proyecto se usa **Odoo Community 16**.

---

## 🐘 ¿Qué es PostgreSQL?

**PostgreSQL** es un sistema gestor de bases de datos relacional:

* Código abierto
* Muy robusto
* Escalable
* Recomendado oficialmente por Odoo

En esta arquitectura:

* PostgreSQL se ejecuta en un contenedor separado
* Odoo se conecta por red interna Docker

---

## 🐳 ¿Qué es Docker?

**Docker** permite ejecutar aplicaciones en **contenedores**, que son entornos aislados.

### Ventajas:

* Evita conflictos de dependencias
* Facilita la instalación
* Permite cambiar versiones fácilmente
* Es el estándar actual en despliegues profesionales

👉 Odoo **NO se instala directamente** en Ubuntu.

---

## 📦 Estructura instalada en la VM

```text
/home/usuario/
└── odoo-docker/
    ├── docker-compose.yml
    ├── config/
    │   └── odoo.conf
    ├── addons/
    │   └── (módulos personalizados)
    └── postgresql/
        └── (datos persistentes)
```

---

## ⚙️ Contenido de docker-compose.yml

```yaml
services:
  db:
    image: postgres:15
    environment:
      POSTGRES_DB: postgres
      POSTGRES_USER: odoo
      POSTGRES_PASSWORD: odoo
    volumes:
      - ./postgresql:/var/lib/postgresql/data

  odoo:
    image: odoo:16
    depends_on:
      - db
    ports:
      - "8069:8069"
    volumes:
      - ./config:/etc/odoo
      - ./addons:/mnt/extra-addons
    environment:
      - HOST=db
      - USER=odoo
      - PASSWORD=odoo
```

---

## 🧩 Configuración de Odoo

Archivo: `config/odoo.conf`

```ini
[options]
addons_path = /mnt/extra-addons
admin_passwd = admin123
```

---

## ▶️ Comandos de gestión (IMPRESCINDIBLES)

Desde `~/odoo-docker`:

```bash
docker compose up -d        # Arrancar
docker compose down         # Parar
docker compose restart      # Reiniciar
docker compose logs -f odoo # Ver logs
docker ps                  # Ver contenedores
```

---

## 🌍 Acceso a Odoo

Desde el navegador del host:

* NAT:

  ```
  http://localhost:8069
  ```
* Puente:

  ```
  http://IP_DE_LA_VM:8069
  ```

Pantalla de creación de base de datos → ✅

---

## 📦 Exportar la VM (OVA)

VirtualBox → Archivo → Exportar servicio virtualizado

* Formato: OVA
* Generar nuevas MAC
* RAM: 2–4 GB
* CPU: 2

---

## 🧠 Decisiones técnicas importantes

* ❌ No usar `apt install odoo`
* ❌ No forzar Python
* ❌ No parchear librerías
* ✅ Docker garantiza estabilidad
* ✅ Arquitectura profesional
* ✅ Entorno reproducible


---

## 🔐 Usuarios y credenciales de la máquina virtual

La máquina virtual incluye usuarios preconfigurados para facilitar el acceso en el entorno educativo.

### Usuario normal (uso habitual)
- **Usuario:** `usuario`
- **Contraseña:** `usuario`

Este usuario se utiliza para:
- Iniciar sesión en la VM
- Ejecutar comandos Docker
- Gestionar el entorno Odoo

### Usuario administrador
- **Usuario:** `root`
- **Contraseña:** `usuario`

⚠️ El usuario `root` debe utilizarse **solo para tareas de administración avanzada** del sistema.

---

## 🖥️ Acceso inicial a la máquina virtual

1. Arranca la máquina virtual desde VirtualBox.
2. Se mostrará una consola de texto (Ubuntu Server **no tiene entorno gráfico**).
3. Inicia sesión con el usuario:
```

usuario

```
4. Introduce la contraseña:
```

usuario

````
5. Una vez dentro, el entorno Odoo se gestiona mediante Docker.

---

## 📌 Qué incluye y qué NO incluye esta VM

### ✅ Incluye
- Ubuntu Server 24.04 LTS
- Docker y Docker Compose instalados y configurados
- Odoo 16 Community ejecutándose en contenedor Docker oficial
- PostgreSQL en contenedor independiente
- Persistencia de datos de la base de datos
- Persistencia de configuración y addons
- Configuración de red con NAT y adaptador puente

### ❌ NO incluye
- Entorno gráfico
- Instalación directa de Odoo en el sistema operativo
- Odoo Enterprise
- Certificados HTTPS
- Configuración de correo electrónico
- Servicios adicionales no relacionados con Odoo

📌 Estas exclusiones son **intencionadas** para centrar el aprendizaje en infraestructura y arquitectura.

---

## 🧪 Comprobación rápida del estado del sistema

Desde la carpeta `~/odoo-docker`:

```bash
docker ps
````

Debe mostrarse al menos:

* Un contenedor `odoo`
* Un contenedor `postgres`

Para ver los logs de Odoo:

```bash
docker compose logs -f odoo
```

Si aparece el mensaje:

```
HTTP service running on 0.0.0.0:8069
```

✔️ El sistema está funcionando correctamente.

---

## ❗ Problemas comunes y solución rápida

### Odoo no carga en el navegador

* Comprueba que los contenedores están activos:

  ```bash
  docker ps
  ```

### No responde `http://localhost:8069`

* Verifica el reenvío de puertos en VirtualBox (NAT → puerto 8069).
* Comprueba que el contenedor de Odoo está escuchando.

### Error al arrancar los contenedores

Ejecuta:

```bash
docker compose down
docker compose up -d
```

---

## 🔄 Reinicio completo del entorno

Si se requiere reiniciar todo el entorno Odoo:

```bash
cd ~/odoo-docker
docker compose down
docker compose up -d
```

---

## 📌 Nota sobre seguridad

Este entorno está diseñado para **uso educativo**:

* Las contraseñas son simples y conocidas
* No se recomienda su uso en producción
* No está expuesto a Internet

---

## 🎓 Uso recomendado en el aula

Esta máquina virtual está pensada para:

* Comprender cómo se despliega un ERP real
* Aprender la relación entre:

  * Sistema operativo
  * Contenedores
  * Base de datos
  * Red
* Evitar problemas de instalación en equipos personales

⚠️ El alumnado **no debe modificar**:

* El sistema base
* La instalación de Docker
* La configuración de red
  salvo indicación expresa del profesorado.

---

## 🗓️ Información final

* Última revisión: diciembre 2025
* Probado en Windows y Linux
* VirtualBox como hipervisor

---

## 🧠 Nota final

> “No es solo aprender Odoo.
> Es aprender cómo funciona la infraestructura real.”

Este proyecto enseña **software + sistemas**, como en el mundo profesional.

```