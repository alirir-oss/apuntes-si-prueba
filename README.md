# **Inicializar el sitio Hugo con la plantilla Book**
Ejecuta el siguiente comando para crear un nuevo sitio Hugo y añadir la plantilla **Hugo Book** como submódulo de Git:
```bash
hugo new site doc-sistemas-informaticos --force
cd doc-sistemas-informaticos
git init -b main
git submodule add https://github.com/alex-shpak/hugo-book.git themes/hugo-book
# Crea un archivo .gitignore para excluir archivos innecesarios
echo "public/" >> .gitignore
echo "node_modules/" >> .gitignore
```

# **Configurar el archivo `config.toml`**
Edita el archivo `config.toml` para configurar el sitio:
```toml
baseURL = "https://user.gitlab.io/doc-sistemas-informaticos/"  # Cambia por tu URL de GitLab Pages
locale = "es-es"
title = "Documentación Sistemas Informáticos"
theme = "hugo-book"

# Configuración multilingüe
defaultContentLanguage = "es"

# Configuración para multilingüe
[languages]
  [languages.es]
    languageName = "Castellano"
    weight = 1
    contentDir = "content.es"

  [languages.val]
    languageName = "Valencià"
    weight = 2
    contentDir = "content.val"

[params]
  # Origen de los ficheros a renderizar
  BookSection = '/'
```

# **Crear la estructura de directorios para los idiomas**
```bash
mkdir -p content.es content.val
```

# **Crear una página de ejemplo en cada idioma**
- **Castellano**: `content.es/_index.md`
  ```markdown
  ---
  title: "Bienvenido"
  ---

  ¡Hola! Esta es la página en castellano.
  ```

- **Valenciano**: `content.val/_index.md`
  ```markdown
  ---
  title: "Benvingut"
  ---

  Hola! Aquesta és la pàgina en valencià.
  ```

---

# **Probar el sitio localmente con Podman**
Ejecuta el siguiente comando para iniciar el servidor de desarrollo de Hugo en el contenedor:
```bash
hugo server -D
```
- Abre tu navegador y ve a: [http://localhost:1313](http://localhost:1313).
- Deberías ver tu sitio web con los dos idiomas disponibles.

---

# **Configurar el archivo `.gitlab-ci.yml`**
Crea un archivo `.gitlab-ci.yml` en la raíz de tu proyecto para automatizar la generación y despliegue de la web estática:
```yaml
image: docker.io/klakegg/hugo:latest

variables:
  GIT_SUBMODULE_STRATEGY: recursive

pages:
  script:
    - hugo --minify --baseURL "$CI_PAGES_URL"
  artifacts:
    paths:
      - public
  only:
    - main
```
- Este archivo usa la imagen de Hugo para generar el sitio estático y lo despliega en **GitLab Pages**.
- La variable `GIT_SUBMODULE_STRATEGY: recursive` asegura que el tema `hugo-book` se clona correctamente.

---

# **Habilitar GitLab Pages**
1. Ve a **Settings > Pages** en tu repositorio de GitLab.
2. Asegúrate de que la opción **Pages** esté habilitada.
3. Una vez que el pipeline se ejecute correctamente, tu sitio estará disponible

---

# **Despliegue**:
   - Haz `git add .`, `git commit -m "Mensaje descriptivo"` y `git push` para actualizar el repositorio.
   - GitLab CI/CD se encargará de generar el sitio y desplegarlo en GitLab Pages.
