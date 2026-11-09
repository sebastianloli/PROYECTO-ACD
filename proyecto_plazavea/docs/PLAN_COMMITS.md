# Plan de commits y publicación en GitHub

Secuencia sugerida de commits lógicos para publicar el proyecto en GitHub, de modo que el historial refleje la evolución técnica (raw → preprocesamiento → features → análisis → documentación) y el aporte de cada integrante. La secuencia coincide con la Tabla 23 del informe final (§11.3), que la presenta como plan hasta que exista el historial real. Este documento es de trabajo interno: una vez hechos los commits, puede retirarse de `docs/` (la convención de mensajes ya está en README §12) y la Tabla 23 del informe puede reemplazarse por el `git log` real.

**Regla principal:** cada commit lo hace **el integrante responsable desde su propia cuenta de GitHub** (con su nombre y el correo asociado a esa cuenta). Así, la pestaña *Insights → Contributors* y el historial (`git log`) sirven como evidencia del criterio «Aporte del integrante de grupo».

Convención de mensajes: `tipo(ámbito): descripción` (ver README §12).

## 1. Secuencia de commits

| N.º | Autor (cuenta) | Mensaje de commit | Archivos |
|---|---|---|---|
| 1 | Joaquin Justo | `chore: estructura inicial, README y .gitignore` | `.gitignore`, `README.md` (versión inicial: título, problema y estructura) |
| 2 | Sebastian Loli | `data: agrega dataset raw oficial (1438 x 12)` | `data/raw/productos_plazavea_raw.csv`, `notebooks/01_acquisition.ipynb` |
| 3 | Piero Ortiz | `docs: diccionario inicial y auditoría del dataset raw` | `docs/diccionario_inicial.docx`, `docs/diccionario_inicial.pdf`, `docs/auditoria_inicial.md` |
| 4 | Sebastian Loli | `feat(prep): integración, reglas de dominio y faltantes` | `notebooks/02_preprocessing.ipynb` (secciones 1–4), `reports/figures/fig01_valores_faltantes.png` |
| 5 | Sebastian Loli | `feat(prep): outliers, transformación y registro de calidad` | `notebooks/02_preprocessing.ipynb` (secciones 5, 6 y 9), `reports/figures/figA1_outliers_precio_actual.png`, `figA2_outliers_precio_regular.png`, `figA3_outliers_descuento_pct.png`, `reports/tables/outliers_precio_actual.csv`, `reports/tables/registro_calidad.csv` |
| 6 | Piero Ortiz | `feat(features): variables derivadas y dataset final` | `notebooks/02_preprocessing.ipynb` (secciones 7, 8 y 10, versión final ejecutada), `data/processed/productos_plazavea_clean.csv`, `data/processed/productos_plazavea_final.csv`, `reports/metrics_preprocesamiento.json` |
| 7 | Diego Alarcon | `feat(analysis): análisis A–G, figuras y metrics.json` | `notebooks/03_analysis.ipynb`, `reports/figures/fig02`–`fig14`, `figA4`, `figA5`, `reports/tables/` (resto de tablas), `reports/metrics.json` |
| 8 | Piero Ortiz | `docs: diccionario final` | `docs/diccionario_final.docx`, `docs/diccionario_final.pdf`, `scripts/build_diccionario_final.py`, `templates/Formato_Diccionario_de_datos.docx` |
| 9 | Joaquin Justo | `build: requirements.txt y verificación de reproducibilidad` | `requirements.txt`, `README.md` (versión completa), `scripts/diagrama_pipeline.py`, `reports/figures/pipeline.png`, `docs/checklist_rubrica.md`, `docs/PLAN_COMMITS.md` |
| 10 | Sofía García | `docs: informe final y presentación` | `docs/informe_final.docx`, `docs/informe_final.pdf`, `docs/presentacion_final.pptx`, `docs/presentacion_final.pdf`, `docs/guion_presentacion.md`, `scripts/figuras_presentacion.py`, `reports/figures/presentacion/` |
| 11 | Joaquin Justo | `docs: enlace del repositorio en README, informe y presentación` | `README.md`, `docs/informe_final.*`, `docs/presentacion_final.*` (tras reemplazar `[enlace al repositorio GitHub del equipo]`) |

Notas:
- Los commits 4–6 corresponden a secciones del mismo notebook (Sebastian Loli: §1–6 y 9; Piero Ortiz: §7, 8 y 10). Cada commit debe reflejar el trabajo que efectivamente aporta su autor; si el notebook se sube completo en un solo commit, se recomienda declarar la coautoría en el mensaje con una línea `Co-authored-by: Nombre <correo>` en lugar de fabricar versiones intermedias. Lo importante es que el historial sea fiel y que **no se suba todo el proyecto en un único commit**.
- La carpeta `_inputs/` (plantillas, rúbrica e informe de la Parte I) no se versiona: está en `.gitignore`.
- Todos los archivos pesan menos de 10 MB, por debajo del límite de 100 MB de GitHub; no se necesita Git LFS.
- Antes de cada `push`, ejecutar `git status` y confirmar que solo se agregan los archivos de la fila correspondiente.

## 2. Configuración inicial (cada integrante, una sola vez)

```bash
git config --global user.name  "Nombre Apellido"
git config --global user.email "correo-de-su-cuenta@ejemplo.com"   # el mismo correo de su cuenta de GitHub
```

Para autenticarse al hacer `push`, usar GitHub CLI (`gh auth login`) o un token personal (HTTPS) o una clave SSH registrada en la cuenta.

## 3. Crear el repositorio (Joaquin Justo, commit 1)

1. En GitHub: *New repository* → nombre `proyecto_plazavea` (u otro acordado), **público**, sin README ni .gitignore (se suben desde local).
2. *Settings → Collaborators*: invitar a Sebastian, Piero, Diego y Sofía con permiso de escritura.
3. Desde la carpeta del proyecto (con la versión inicial del README):

```bash
cd proyecto_plazavea
git init -b main
git add .gitignore README.md
git commit -m "chore: estructura inicial, README y .gitignore"
git remote add origin https://github.com/<usuario-o-organizacion>/proyecto_plazavea.git
git push -u origin main
```

4. Compartir con el equipo la carpeta completa del proyecto (por ejemplo, un .zip en el drive del grupo) para que cada integrante copie sus archivos.

## 4. Commits de cada integrante (en orden)

Cada integrante, cuando le toque según la tabla:

```bash
# Solo la primera vez
git clone https://github.com/<usuario-o-organizacion>/proyecto_plazavea.git
cd proyecto_plazavea

# Cada vez: traer los commits anteriores
git pull origin main

# Copiar desde la carpeta compartida SOLO los archivos de su fila, respetando las rutas, y luego:
git add <archivos de su fila>
git status                                   # revisar que no haya archivos de más
git commit -m "<mensaje de su fila>"
git push origin main
```

Ejemplos concretos:

```bash
# Commit 2 (Sebastian Loli)
git add data/raw/productos_plazavea_raw.csv notebooks/01_acquisition.ipynb
git commit -m "data: agrega dataset raw oficial (1438 x 12)"

# Commit 6 (Piero Ortiz)
git add notebooks/02_preprocessing.ipynb data/processed/ reports/metrics_preprocesamiento.json
git commit -m "feat(features): variables derivadas y dataset final"

# Commit 7 (Diego Alarcon)
git add notebooks/03_analysis.ipynb reports/figures/fig0*.png reports/figures/fig1*.png \
        reports/figures/figA4_top_marcas_productos.png reports/figures/figA5_regular_vs_actual.png \
        reports/tables/ reports/metrics.json
git commit -m "feat(analysis): análisis A–G, figuras y metrics.json"

# Commit 9 (Joaquin Justo)
git add requirements.txt README.md scripts/diagrama_pipeline.py reports/figures/pipeline.png \
        docs/checklist_rubrica.md docs/PLAN_COMMITS.md
git commit -m "build: requirements.txt y verificación de reproducibilidad"

# Commit 10 (Sofía García)
git add docs/informe_final.docx docs/informe_final.pdf docs/presentacion_final.pptx \
        docs/presentacion_final.pdf docs/guion_presentacion.md \
        scripts/figuras_presentacion.py reports/figures/presentacion/
git commit -m "docs: informe final y presentación"
```

Si dos integrantes trabajan a la vez y el `push` es rechazado, ejecutar `git pull --rebase origin main` y volver a hacer `git push`.

## 5. Verificación final (Joaquin Justo)

```bash
git log --pretty=format:"%h | %an | %s"      # debe mostrar los 11 commits con su autor
git ls-files | wc -l                          # todos los archivos del árbol del README
```

Luego, en un directorio limpio, comprobar que el repositorio publicado se reproduce:

```bash
git clone https://github.com/<usuario-o-organizacion>/proyecto_plazavea.git prueba && cd prueba
python -m venv .venv && source .venv/bin/activate
pip install -r requirements.txt
sha256sum data/raw/productos_plazavea_raw.csv   # 3a3833e8312f8d27566083b81cd02a86535055587ba38f25de4e5d05f4348f3a
jupyter nbconvert --to notebook --execute --inplace notebooks/02_preprocessing.ipynb
jupyter nbconvert --to notebook --execute --inplace notebooks/03_analysis.ipynb
git status                                      # sin cambios en data/processed ni reports/metrics.json
```

Por último, copiar la URL del repositorio en `README.md`, en el informe (§11) y en la presentación, regenerar los PDF y subirlo como commit 11.
