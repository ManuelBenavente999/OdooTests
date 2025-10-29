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








## **3. Contenido de los Archivos del Proyecto**
A continuación se muestra el contenido de los archivos principales utilizados en la implementación del proyecto.

### **3.1 Archivo `Dockerfile`**
```
# Imagen base Odoo 17
FROM odoo:17

# (Opcional) módulos propios
COPY ./extra-addons /mnt/extra-addons

# Puerto HTTP de Odoo
EXPOSE 8069

# Puerto por defecto de PostgreSQL
ENV PGPORT=5432

# 1) Inicializa la BD indicada en $PGDATABASE si está vacía (stop-after-init)
# 2) Después arranca el servidor normalmente
#
# NOTA: usamos $PGDATABASE para que la inicialización vaya contra esa BD
# y --db-filter la fije para evitar que Odoo “coja” otra por error.
CMD ["bash","-lc", "\
  echo '==> Checking/initializing DB $PGDATABASE' && \
  odoo -d $PGDATABASE -i base --without-demo=all \
       --db_host=$PGHOST --db_port=$PGPORT \
       --db_user=$PGUSER --db_password=$PGPASSWORD \
       --addons-path=/usr/lib/python3/dist-packages/odoo/addons,/mnt/extra-addons \
       --stop-after-init || true; \
  echo '==> Starting Odoo server' && \
  odoo --db_host=$PGHOST --db_port=$PGPORT \
       --db_user=$PGUSER --db_password=$PGPASSWORD \
       --addons-path=/usr/lib/python3/dist-packages/odoo/addons,/mnt/extra-addons \
       --db-filter=$PGDATABASE \
       --dev=all"]
```
### **3.1 Archivo `__manifest__.py`**
```
{
    "name": "Dummy Module",
    "version": "1.0",
    "summary": "Módulo vacío de prueba",
    "installable": True,
}
```





