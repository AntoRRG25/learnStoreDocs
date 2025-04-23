# Verificacion de correo

[Volver a Readme](../README.md)

Entiendo, para verificar la dirección de correo electrónico de un usuario en una API REST con Express, generalmente se sigue un proceso similar al de enviar un correo electrónico de confirmación con un enlace único para que el usuario haga clic y confirme su dirección de correo electrónico.

Aquí hay un enfoque general sobre cómo podrías implementarlo:

Generación de un token único: Cuando un usuario se registra en tu aplicación, generas un token único que se asocia con su dirección de correo electrónico y se almacena en tu base de datos.

Envío del correo electrónico de verificación: Envía un correo electrónico al usuario con un enlace que contiene este token único. El enlace debe apuntar a una ruta en tu servidor que maneje la verificación del correo electrónico.

Manejo de la solicitud de verificación: Cuando el usuario hace clic en el enlace de verificación en el correo electrónico, tu servidor recibe la solicitud en la ruta correspondiente y verifica el token. Si el token es válido, marcas la dirección de correo electrónico del usuario como verificada en tu base de datos.

Protección de rutas restringidas: Puedes tener ciertas rutas en tu API que solo sean accesibles para usuarios con direcciones de correo electrónico verificadas.

Aquí hay un ejemplo básico de cómo podrías implementar esto en Express:

```javascript
import jwt from 'jsonwebtoken';
import nodemailer from 'nodemailer';
import User from '../models/User'; // Importa tu modelo de usuario o donde tengas almacenada la información del usuario

// Función para generar un token único
const generateVerificationToken = (email) => {
  return jwt.sign({ email }, 'tu_clave_secreta', { expiresIn: '1d' }); // Cambia 'tu_clave_secreta' por una clave secreta real
};

// Función para enviar el correo electrónico de verificación
const sendVerificationEmail = async (email, token) => {
  // Configura el transporte de correo
  const transporter = nodemailer.createTransport({
    service: 'gmail',
    auth: {
      user: 'tu_correo@gmail.com',
      pass: 'tu_contraseña'
    }
  });

  // Cuerpo del correo electrónico
  const mailOptions = {
    from: 'tu_correo@gmail.com',
    to: email,
    subject: 'Verificación de correo electrónico',
    text: `Para verificar tu correo electrónico, haz clic en el siguiente enlace: http://localhost:3000/verify-email/${token}`
  };

  // Envía el correo electrónico
  await transporter.sendMail(mailOptions);
};

// Ruta para la verificación de correo electrónico
router.get('/verify-email/:token', async (req, res) => {
  const { token } = req.params;

  try {
    // Verifica el token
    const decoded = jwt.verify(token, 'tu_clave_secreta'); // Cambia 'tu_clave_secreta' por la misma clave secreta utilizada en la generación del token

    // Actualiza la dirección de correo electrónico del usuario como verificada en la base de datos
    await User.updateOne({ email: decoded.email }, { verified: true });

    // Redirige al usuario a una página de éxito o muestra un mensaje de éxito
    res.send('Correo electrónico verificado correctamente');
  } catch (error) {
    // En caso de error, redirige al usuario a una página de error o muestra un mensaje de error
    res.send('Error al verificar el correo electrónico');
  }
});

// Ruta para el registro de usuarios
router.post('/register', async (req, res) => {
  const { email } = req.body;

  try {
    // Genera un token de verificación
    const verificationToken = generateVerificationToken(email);

    // Envía el correo electrónico de verificación
    await sendVerificationEmail(email, verificationToken);

    // Registra al usuario en la base de datos (sin marcar la dirección de correo electrónico como verificada por ahora)
    await User.create({ email, verified: false });

    // Envía una respuesta exitosa al cliente
    res.send('Usuario registrado correctamente. Verifica tu correo electrónico para completar el registro.');
  } catch (error) {
    // En caso de error, envía una respuesta de error al cliente
    res.status(500).json({ error: 'Error al registrar al usuario' });
  }
});

```
Este es solo un ejemplo básico para darte una idea de cómo podrías implementar la verificación de correo electrónico en tu API REST con Express. Dependerá de ti adaptarlo y extenderlo según las necesidades específicas de tu aplicación.