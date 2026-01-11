# Guía para Configurar Formspree

## 📋 Pasos para Configurar Formspree

### Paso 1: Crear una Cuenta en Formspree

1. Ve a **https://formspree.io**
2. Haz clic en **"Sign Up"** o **"Get Started"**
3. Crea una cuenta usando:
   - Tu email, o
   - Tu cuenta de Google/GitHub (más rápido)

### Paso 2: Crear un Nuevo Formulario

1. Una vez dentro de tu cuenta, haz clic en **"New Form"** o **"+"**
2. Completa la información del formulario:
   - **Form Name**: "Contacto AquaTEN" (o el nombre que prefieras)
   - **Email**: Ingresa el email donde quieres recibir los mensajes
   - Opcionalmente, puedes agregar más emails en **"Additional Emails"**

3. Haz clic en **"Create Form"**

### Paso 3: Obtener el Form ID

1. Después de crear el formulario, verás una página con tu **Form ID**
2. El Form ID se verá así: `xrgkqwzy` o `abc123def456`
3. También puedes verlo en la URL: `https://formspree.io/f/xrgkqwzy`
4. **Copia este ID** (sin el `/f/`)

### Paso 4: Configurar el Formulario en tu Sitio

1. Abre el archivo `index.html`
2. Busca la línea que dice:
   ```html
   <form id="contactForm" action="https://formspree.io/f/YOUR_FORM_ID" method="POST">
   ```
3. Reemplaza `YOUR_FORM_ID` con tu Form ID real:
   ```html
   <form id="contactForm" action="https://formspree.io/f/xrgkqwzy" method="POST">
   ```

### Paso 5: Actualizar el JavaScript (Opcional pero Recomendado)

El código JavaScript actual tiene un `preventDefault()` que evita que el formulario se envíe. Necesitas actualizarlo para que funcione con Formspree.

1. Busca la sección del script del formulario (alrededor de la línea 1318)
2. Reemplaza el código del evento `submit` con el código que se proporciona más abajo

### Paso 6: Probar el Formulario

1. Abre tu sitio web en el navegador
2. Completa el formulario de contacto
3. Haz clic en "Enviar Mensaje"
4. Deberías ver un mensaje de éxito
5. Verifica que recibiste el email en la dirección configurada en Formspree

---

## 🔧 Código Actualizado para el JavaScript

Reemplaza la sección del formulario en el script (alrededor de línea 1318-1337) con este código:

```javascript
// Formulario de contacto con Formspree
const contactForm = document.getElementById('contactForm');
if (contactForm) {
    contactForm.addEventListener('submit', async function(e) {
        e.preventDefault();
        
        const submitBtn = this.querySelector('.submit-button');
        const originalText = submitBtn.textContent;
        submitBtn.textContent = 'Enviando...';
        submitBtn.disabled = true;
        
        try {
            const formData = new FormData(this);
            const response = await fetch(this.action, {
                method: 'POST',
                body: formData,
                headers: {
                    'Accept': 'application/json'
                }
            });
            
            if (response.ok) {
                // Éxito
                alert('¡Gracias! Tu mensaje ha sido enviado. Te contactaremos pronto.');
                this.reset();
            } else {
                // Error
                const data = await response.json();
                if (data.errors) {
                    alert('Hubo un error al enviar el formulario: ' + JSON.stringify(data.errors));
                } else {
                    alert('Hubo un error al enviar el formulario. Por favor, intenta nuevamente.');
                }
            }
        } catch (error) {
            alert('Hubo un error de conexión. Por favor, intenta nuevamente.');
            console.error('Error:', error);
        } finally {
            submitBtn.textContent = originalText;
            submitBtn.disabled = false;
        }
    });
}
```

---

## 📧 Opciones Avanzadas de Formspree

### Configurar Redirección después del Envío

Si quieres redirigir a los usuarios a una página de agradecimiento después de enviar el formulario:

1. En Formspree, ve a la configuración de tu formulario
2. En **"Redirect"**, ingresa la URL: `https://tudominio.com/#contacto` (o crea una página de agradecimiento)
3. O agrega un campo oculto en el HTML:
   ```html
   <input type="hidden" name="_next" value="https://tudominio.com/gracias.html">
   ```

### Agregar Campo de Nombre del Formulario

Formspree puede recibir múltiples formularios. Para identificarlos mejor:

1. Agrega este campo oculto en tu formulario:
   ```html
   <input type="hidden" name="_subject" value="Nuevo contacto desde AquaTEN">
   ```

### Protección Anti-Spam

Formspree tiene protección anti-spam incorporada. Para una protección adicional:

1. Ve a la configuración de tu formulario en Formspree
2. Habilita **"Honeypot"** (opción recomendada)
3. O configura **reCAPTCHA** si lo prefieres

### Personalizar el Email Recibido

1. En Formspree, ve a **"Settings"** > **"Notifications"**
2. Puedes personalizar el asunto y el contenido del email
3. Puedes usar variables como: `{{nombre}}`, `{{email}}`, `{{mensaje}}`

---

## 💡 Planes de Formspree

- **Plan Gratuito**: Hasta 50 envíos/mes, perfecto para empezar
- **Plan Starter** ($10/mes): 1,000 envíos/mes + más características
- **Plan Professional** ($40/mes): 5,000 envíos/mes + características avanzadas

Para la mayoría de sitios, el plan gratuito es suficiente.

---

## ❓ Troubleshooting

### El formulario no envía

1. Verifica que el Form ID esté correcto en el `action` del formulario
2. Asegúrate de que el JavaScript no tenga errores (abre la consola del navegador con F12)
3. Verifica que el formulario tenga el método `POST`
4. Revisa que todos los campos requeridos estén completos

### No recibo los emails

1. Verifica que el email esté correcto en la configuración de Formspree
2. Revisa tu carpeta de spam
3. Verifica el dashboard de Formspree para ver si hay errores
4. Asegúrate de que tu plan de Formspree tenga envíos disponibles

### Error 422 (Unprocessable Entity)

Este error generalmente significa que:
- El Form ID es incorrecto
- El formulario no está activado en Formspree
- Faltan campos requeridos

---

## 📝 Resumen Rápido

1. ✅ Crear cuenta en https://formspree.io
2. ✅ Crear nuevo formulario
3. ✅ Copiar el Form ID
4. ✅ Reemplazar `YOUR_FORM_ID` en `index.html` línea 1165
5. ✅ Actualizar el JavaScript del formulario (código arriba)
6. ✅ Probar el formulario

---

¿Necesitas ayuda? Consulta la documentación oficial de Formspree:
https://help.formspree.io/hc/en-us
