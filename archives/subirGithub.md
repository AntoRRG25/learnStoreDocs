# Subir nuevo repositorio a Github
[Volver a Readme](../README.md)

Claro, te explico cómo subir un repositorio a GitHub por primera vez usando token de acceso personal:

## Pasos para subir un repositorio a GitHub por primera vez

### 1. Crear el repositorio en GitHub
Primero, crea un repositorio vacío en tu cuenta de GitHub:
- Ve a GitHub y haz clic en el botón "+" en la esquina superior derecha
- Selecciona "New repository"
- Dale un nombre al repositorio
- Deja las opciones de inicialización sin marcar (sin README, .gitignore, etc.)
- Crea el repositorio

### 2. Inicializar tu repositorio local (si aún no lo has hecho)
```bash
cd tu-proyecto
git init
git add .
git commit -m "Commit inicial"
```

### 3. Configurar el remoto con tu token
```bash
git remote add origin https://[TOKEN]@github.com/[TU-USUARIO]/[NOMBRE-REPO].git
```

Reemplaza:
- `[TOKEN]` con tu token de acceso personal
- `[TU-USUARIO]` con tu nombre de usuario de GitHub
- `[NOMBRE-REPO]` con el nombre del repositorio que creaste

### 4. Subir tu código al repositorio remoto
```bash
git push -u origin main
```
(O usa `master` en lugar de `main` si tu rama principal se llama así)

### 5. Verificar
Visita tu repositorio en GitHub para confirmar que todo se subió correctamente.

Si necesitas hacer cambios en el futuro, simplemente:
```bash
git add .
git commit -m "Descripción de los cambios"
git push
```

¿Hay alguna parte del proceso sobre la que necesites más información?

<hr>

### Pregunta:
no hay problema con mi git? que tenga dos usuarios, no queda en memoria?
<hr>

Buena pregunta. Git maneja la configuración de usuario a diferentes niveles, lo que puede causar confusión cuando trabajas con múltiples cuentas.

Cuando usas tokens de acceso personal para autenticarte, Git no guarda en caché tu nombre de usuario de GitHub, sino las credenciales de autenticación. Esto puede crear algunas situaciones a tener en cuenta:

1. **Credenciales en caché**: Git puede almacenar temporalmente las credenciales usando un credential helper. Si usas un token para una cuenta y luego intentas realizar operaciones con otra cuenta, podrías tener conflictos.

2. **Configuración de usuario**: Git usa dos configuraciones importantes que afectan tus commits:
   - `user.name` - Tu nombre visible en los commits
   - `user.email` - Tu dirección de email asociada a los commits

Estas configuraciones pueden establecerse a nivel global (para todos los repositorios) o a nivel local (específico para un repositorio).

Para evitar problemas:

1. **Configura cada repositorio por separado**:
   ```bash
   # Dentro del repositorio de tu cuenta secundaria
   git config user.name "Tu Nombre Para Segunda Cuenta"
   git config user.email "tu_segundo_email@gmail.com"
   ```

2. **Limpia el almacenamiento de credenciales** si necesitas cambiar entre cuentas:
   ```bash
   git credential reject
   # O más específicamente:
   git credential reject host=github.com
   ```

3. **Verifica antes de hacer commit** qué configuración está activa:
   ```bash
   git config user.name
   git config user.email
   ```

De esta forma, te aseguras de que cada commit se asocie con la cuenta correcta, incluso si estás autenticándote con tokens diferentes.
<hr>

### Pregunta:
hay diferencia entre las credenciales globales o locales? como accedo a ellas desde git?

<hr>

Sí, hay diferencia entre las configuraciones de Git globales y locales. Las configuraciones globales se aplican a todos los repositorios en tu sistema, mientras que las locales solo afectan al repositorio específico en el que las configuras.

### Configuraciones globales vs. locales

#### Configuración global
- Se aplica a todos los repositorios de tu usuario
- Se almacena en `~/.gitconfig` (en Linux/Mac) o `C:\Users\TuUsuario\.gitconfig` (en Windows)

#### Configuración local
- Se aplica solo al repositorio actual
- Se almacena en `.git/config` dentro del directorio del repositorio

### Cómo acceder y modificar las configuraciones

#### Para ver tus configuraciones:
```bash
# Ver toda la configuración global
git config --global --list

# Ver toda la configuración local (del repositorio actual)
git config --local --list

# Ver una configuración específica global
git config --global user.name

# Ver una configuración específica local
git config user.name
```

#### Para modificar las configuraciones:
```bash
# Configurar globalmente
git config --global user.name "Tu Nombre"
git config --global user.email "tu_email@ejemplo.com"

# Configurar localmente (solo para el repositorio actual)
git config user.name "Tu Otro Nombre"
git config user.email "tu_otro_email@ejemplo.com"
```

#### Para eliminar una configuración:
```bash
# Eliminar configuración global
git config --global --unset user.name

# Eliminar configuración local
git config --unset user.name
```

Para tu caso con dos cuentas de GitHub, es mejor usar configuraciones locales en los repositorios de tu segunda cuenta. Así mantienes tu configuración global para tu cuenta principal, y solo configuras específicamente los repositorios que quieres asociar con tu segunda cuenta.

Esto asegura que cada commit sea creado con la identidad correcta, independientemente de qué token o método de autenticación estés usando para subir los cambios.