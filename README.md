# SÑEAM — tienda de videojuegos

## Instalación

1. Instala las dependencias: `pip install -r requirements.txt`
2. Configura la contraseña administrativa y una clave secreta antes de iniciar la aplicación:

   ```powershell
   $env:SNEAM_ADMIN_PASSWORD = "elige-una-contraseña-segura"
   $env:SNEAM_SECRET_KEY = (python -c "import secrets; print(secrets.token_hex(32))")
   ```

3. Ejecuta `python app.py` y abre <http://localhost:5000>.
4. El panel está en <http://localhost:5000/admin>.

La clave secreta debe mantenerse privada y ser estable entre reinicios. En un servidor público, configúrala mediante el gestor de secretos del proveedor, activa HTTPS y establece `SNEAM_HTTPS=1`. La contraseña de administración no tiene un valor predeterminado: sin `SNEAM_ADMIN_PASSWORD`, el inicio de sesión permanece desactivado.

## Funciones

- Catálogo con búsqueda, categorías, ofertas destacadas y carrito.
- Checkout de demostración con nombre, correo y forma de pago.
- Registro e inicio/cierre de sesión de clientes; su biblioteca e historial quedan asociados a su cuenta (compras de invitado se pueden vincular al registrarse).
- Reseñas públicas por producto para usuarios con cuenta.
- Panel administrativo protegido para crear, editar y eliminar productos (incluidas sus imágenes), cambiar descuentos y consultar pedidos.
- API de catálogo en `/api/juegos`.
- Base de datos SQLite local en `sneam.db`. Se puede cambiar su ubicación con `SNEAM_DATABASE`.

**Pagos:** las opciones de tarjeta, PayPal y SPEI solo registran una compra simulada. No se realizan cobros y no se solicitan ni almacenan números de tarjeta o códigos de seguridad. Para cobrar de verdad se debe integrar una pasarela de pagos.

## Despliegue en Render

1. Crea un servicio web con el repositorio de GitHub.
2. Usa la opción que ejecuta `gunicorn app:app` (también está incluido en `render.yaml` y `Procfile`).
3. Configura las variables de entorno de Render:
   - `SNEAM_ADMIN_PASSWORD`: contraseña del panel administrativo; no se usa un valor por defecto.
   - `SNEAM_SECRET_KEY`: clave secreta estable para sesiones y CSRF.
   - `SNEAM_HTTPS=1` para habilitar cookies seguras en producción.
4. Haz el primer deploy y accede a la URL pública generada por Render.

## Estructura

```text
SÑEAM/
├── app.py
├── templates/
│   ├── index.html
│   ├── library.html
│   └── admin.html
├── static/css/style.css
└── requirements.txt
```
