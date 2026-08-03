# Apuntes DAW — sitio web (MkDocs Material + GitHub Pages)

Sitio de apuntes generado con MkDocs Material. Publicación automática en GitHub Pages con cada push.

## Publicar por primera vez (10 minutos)

1. Crea un repositorio **público** en tu cuenta personal de GitHub llamado `apuntes-daw` (sin README inicial).
2. En esta carpeta:
   ```bash
   git init
   git add .
   git commit -m "Sitio de apuntes inicial"
   git branch -M main
   git remote add origin https://github.com/TU-USUARIO/apuntes-daw.git
   git push -u origin main
   ```
3. El workflow de Actions se ejecutará solo (pestaña **Actions** del repo) y creará la rama `gh-pages`.
4. En el repo: **Settings → Pages → Source: Deploy from a branch → Branch: `gh-pages` / (root) → Save**.
5. En 1-2 minutos el sitio estará en `https://TU-USUARIO.github.io/apuntes-daw/`.
6. Descomenta y ajusta `site_url` y `repo_url` en `mkdocs.yml`, haz commit y push.

## Añadir una unidad nueva

1. Genera la unidad con la skill `generador-apuntes` en modo "sitio web (MkDocs)".
2. Copia el archivo a `docs/<modulo>/udN-<tema>.md`.
3. Añádela al `nav` de `mkdocs.yml` y al índice del módulo.
4. Commit + push → se publica sola.

## Ver el sitio en local (opcional)

```bash
pip install -r requirements.txt
mkdocs serve   # → http://127.0.0.1:8000
```

## Reglas

- **NUNCA** subir a este repositorio los documentos de SOLUCIONES del docente ni nada con datos de alumnado.
- El repositorio es tuyo (cuenta personal): el material viaja contigo entre centros.
