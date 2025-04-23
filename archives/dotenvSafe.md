# DotenvSafe

[Volver a Readme](../README.md)

Para configurar **dotenv-safe** en tu API de Express, necesitarás asegurarte de que todas las variables de entorno necesarias estén definidas en un archivo `.env.example`. Luego, utilizarás **dotenv-safe** para cargar esas variables en tu aplicación. A continuación, te muestro cómo puedes hacerlo paso a paso:

### Paso 1: Instalar dotenv-safe

Primero, asegúrate de tener `dotenv-safe` instalado en tu proyecto:

```bash
npm install dotenv-safe
```

### Paso 2: Crear archivos de configuración

1. **Archivo `.env.example`**: Este archivo debe contener todas las variables de entorno necesarias para tu aplicación. Aquí tienes un ejemplo basado en tu archivo `envConfig.js`:

   **.env.example**
   ```plaintext
   VITE_SECRET_KEY=your_secret_key
   VITE_DB_USER=your_db_user
   VITE_DB_PASSWORD=your_db_password
   VITE_DB_HOST=localhost
   VITE_DB_NAME=your_db_name
   PORT=3000
   VITE_USER_IMG=your_user_image
   VITE_GMAIL_USER=your_gmail_user
   VITE_GMAIL_APP_PASS=your_gmail_app_password
   ```

2. **Archivo `.env`**: Este archivo contendrá las variables de entorno que usarás en tu aplicación. Puedes crearlo como sigue, utilizando valores reales:

   **.env**
   ```plaintext
   VITE_SECRET_KEY=my_secret_key
   VITE_DB_USER=my_db_user
   VITE_DB_PASSWORD=my_db_password
   VITE_DB_HOST=localhost
   VITE_DB_NAME=my_database
   PORT=3000
   VITE_USER_IMG=/path/to/user/image
   VITE_GMAIL_USER=my_gmail@example.com
   VITE_GMAIL_APP_PASS=my_gmail_app_password
   ```

### Paso 3: Configurar dotenv-safe en tu API

Ahora puedes usar **dotenv-safe** para cargar las variables de entorno al inicio de tu aplicación. Modifica tu archivo `envConfig.js` de la siguiente manera:

```javascript
import dotenvSafe from 'dotenv-safe';

dotenvSafe.config({
    allowEmptyValues: false, // Cambiar a true si deseas permitir valores vacíos
});

const {
    VITE_SECRET_KEY,
    VITE_DB_USER,
    VITE_DB_PASSWORD,
    VITE_DB_HOST,
    VITE_DB_NAME,
    PORT,
    VITE_USER_IMG,
    VITE_GMAIL_USER,
    VITE_GMAIL_APP_PASS,
} = process.env;

const ConnectDb = `postgres://${VITE_DB_USER}:${VITE_DB_PASSWORD}@${VITE_DB_HOST}/${VITE_DB_NAME}`;

export default {
    Port: PORT,
    Status: process.env.NODE_ENV === 'development' ? 'in development' : 'in production',
    ConnectDb,
    userImg: VITE_USER_IMG,
    gmailUser: VITE_GMAIL_USER,
    gmailPass: VITE_GMAIL_APP_PASS,
};
```

### Paso 4: Validación de las Variables de Entorno

Si quieres validar las variables de entorno para asegurarte de que cumplan con ciertos criterios, puedes integrar Joi para hacerlo. Aquí hay un ejemplo simple:

```javascript
import Joi from 'joi';

// Definir el esquema de validación
const schema = Joi.object({
    VITE_SECRET_KEY: Joi.string().required(),
    VITE_DB_USER: Joi.string().required(),
    VITE_DB_PASSWORD: Joi.string().required(),
    VITE_DB_HOST: Joi.string().default('localhost'),
    VITE_DB_NAME: Joi.string().required(),
    PORT: Joi.number().default(3000),
    VITE_USER_IMG: Joi.string().required(),
    VITE_GMAIL_USER: Joi.string().email().required(),
    VITE_GMAIL_APP_PASS: Joi.string().required(),
});

// Validar las variables de entorno
const { error } = schema.validate(process.env);
if (error) {
    console.error('Error de validación:', error.details);
    process.exit(1); // Terminar la aplicación si hay un error
}
```

### Conclusión

Con esta configuración, tu aplicación ahora se asegurará de que todas las variables de entorno necesarias estén presentes y se validen correctamente antes de que la aplicación comience a ejecutarse. Esto proporciona una capa adicional de seguridad y confiabilidad, ayudando a evitar problemas en tiempo de ejecución relacionados con configuraciones faltantes o incorrectas.