# OdooTests
# **Implementación de Odoo en Render**

## **1. Requisitos Previos**
### **1.1 GitHub**
Antes de comenzar, debemos tener preparado nuestro repositorio en GitHub con la siguiente estructura de directorios:
Dockerfile
extra-addons/
├  ├── .gitkeep
├  └── dummy_module/
├      ├── __init__.py
├      └── __manifest__.py
└── README.md
> 🔴 **Nota:**  
> Los unicos archivos con contenido son: "Dockerfile" y "__manifest__.py", el resto no tienen contenido.
> El contenido de estos archivos se encuentra al final de esta documentación.

---

### **1.2 Render**

Para poder implementar Odoo en Render, será necesario:
- Crear una cuenta en [Render](https://render.com/).
- Iniciar sesión en la plataforma antes de proceder con la configuración del servicio.

---

## **2. Pasos en Render para Crear el Servicio de Odoo**

### **2.1 Crear Web Service en Render**

En la parte superior derecha de la página de Render, haremos clic en **"New"** y seleccionaremos la opción **"Web Service"**.

A continuación:

1. Enlazaremos el servicio con el repositorio de **GitHub** que el usuario haya creado previamente, el cual contiene el árbol de directorios mostrado en el apartado anterior.  
2. Configuraremos el servicio según las preferencias del usuario (nombre, región, etc.).  
3. Es **muy importante** seleccionar el tipo de lenguaje (**Language**) como **Docker**.  
4. Finalmente, haremos clic en **"Deploy Web Service"**.

Render comenzará el proceso de implementación, analizando el contenido del repositorio y preparando el entorno.  
Este proceso puede tardar unos minutos.  
Si todo es correcto, Render mostrará un mensaje informando del **estado del despliegue**.  
En caso contrario, se mostrarán mensajes de error indicando el motivo del fallo.

---

### **2.2 Crear la Base de Datos Postgres**

De nuevo, en la parte superior derecha de Render, haremos clic en **"New"** y esta vez seleccionaremos la opción **"Postgres"**.

Después:

1. Configuraremos la base de datos según las preferencias del usuario (nombre, región, plan gratuito o de pago, etc.).  
2. Cuando todo esté listo, pulsaremos el botón **"Create Database"**.

Esto creará nuestra base de datos **PostgreSQL**, que será la que Odoo utilizará para almacenar toda la información del sistema.

---
