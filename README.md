# Precios e intensidad promocional en Abarrotes de Plaza Vea Perú

**DS3021 Análisis Computacional de Datos · Proyecto Final, Parte II: preprocesamiento de datos y data analytics product**
Universidad de Ingeniería y Tecnología (UTEC), Facultad de Computación, Carrera de Data Science · Lima, 2026
Profesor: Jose Luis Espinoza Melgarejo

> Repositorio: https://github.com/sebastianloli/PROYECTO-ACD.git

---

## 1. Problema

El listado web de la categoría **Abarrotes** de Plaza Vea (www.plazavea.com.pe) publica, para cada producto, un precio regular y un precio actual. La oferta y las rebajas cambian continuamente, y no existe un resumen estructurado que diga cuántos productos están rebajados, de qué tamaño son las rebajas ni cómo varían entre grupos de categoría, subcategorías y marcas.

En la Parte I se construyó, mediante web scraping, el dataset oficial (**1438 registros × 12 variables**). La auditoría de la Parte II encontró que la variable `discount` vale 0 en el 100 % de los registros, aunque 479 productos tienen `current_price < regular_price`: el descuento **no fue capturado** y debe reconstruirse a partir de los precios. El problema se replantea así como un análisis **descriptivo** de los precios y de la intensidad promocional **observable** en una muestra extraída en una fecha (snapshot).

**Usuario objetivo:** un analista que monitorea precios y promociones publicados en el canal web de Abarrotes (por ejemplo, en un equipo de pricing o category management, de inteligencia de mercado o de una marca proveedora). Hace un seguimiento descriptivo de la oferta; no evalúa ventas ni efectividad de promociones.

## 2. Objetivos

**Objetivo general.** Describir la distribución de los precios publicados y la intensidad promocional observable (frecuencia y profundidad de las rebajas reconstruidas a partir de los precios) entre grupos de categoría, subcategorías y marcas de Abarrotes de Plaza Vea Perú, a partir de una muestra de 30 páginas extraída en una fecha, mediante un pipeline reproducible de preprocesamiento y análisis.

**Objetivos específicos.**

1. Construir un dataset analítico limpio, trazable y reproducible, con el descuento reconstruido y cada decisión de calidad documentada.
2. Describir el nivel y la dispersión de los precios por grupo de categoría y, normalizados por contenido, dentro de subcategorías.
3. Medir la frecuencia promocional (`promo_rate`) por grupo, subcategoría y marca, siempre con su n e intervalo de confianza.
4. Medir la profundidad de las rebajas (`discount_pct`, `discount_amount`, `promotion_intensity`) y su relación con el nivel de precio.
5. Identificar patrones, segmentos y casos anómalos útiles para el monitoreo de la oferta.
6. Dejar el flujo completo documentado y ejecutable desde este repositorio.

## 3. Fuente de datos

| Aspecto         | Detalle                                                                                              |
| --------------- | ---------------------------------------------------------------------------------------------------- |
| Fuente          | Listado web de la categoría Abarrotes de Plaza Vea Perú (`www.plazavea.com.pe/abarrotes`)            |
| Técnica         | Web scraping con Selenium + BeautifulSoup (notebook `01_acquisition.ipynb`, Google Colab)            |
| Cobertura       | 30 de aproximadamente 65 páginas del listado, en el orden que muestra el sitio (muestra, no censo)   |
| Momento         | Snapshot único: corrida de extracción de la Parte I (el CSV no incluye un campo de fecha)            |
| Dataset oficial | `data/raw/productos_plazavea_raw.csv`: 1438 registros × 12 variables, 1438 SKU únicos, sin modificar |
| Integridad      | sha256 `3a3833e8312f8d27566083b81cd02a86535055587ba38f25de4e5d05f4348f3a`                            |

> Nota de trazabilidad: las salidas visibles en `01_acquisition.ipynb` (1407 filas → 1397 SKU) corresponden a otra corrida del mismo código. La corrida oficial es la del CSV entregado en la Parte I (1438 registros).

## 4. Estructura del repositorio

```
proyecto_plazavea/
├── README.md                        # este documento
├── requirements.txt                 # dependencias con versiones exactas
├── .gitignore
├── data/
│   ├── raw/
│   │   └── productos_plazavea_raw.csv        # dataset inicial oficial (1438 × 12), sin modificar
│   └── processed/
│       ├── productos_plazavea_clean.csv      # dataset limpio completo (1438 × 29): todas las filas + features + flags
│       └── productos_plazavea_final.csv      # DATASET FINAL analítico (1437 × 21)
├── notebooks/
│   ├── 01_acquisition.ipynb         # adquisición (scraping, Colab); documenta el origen, no se re-ejecuta
│   ├── 02_preprocessing.ipynb       # limpieza, integración, outliers, transformación, features, reducción
│   └── 03_analysis.ipynb            # análisis A–G, figuras, tablas y metrics.json
├── reports/
│   ├── figures/                     # 19 figuras de los notebooks + pipeline.png; numeración del informe:
│   │                                #   Figuras 1–12 en el cuerpo (pipeline, fig01–fig07, fig10, fig11, fig13, fig14)
│   │                                #   Figuras A1–A8 en el Anexo A (figA1–figA5, fig08, fig09, fig12)
│   ├── tables/                      # 24 tablas CSV exportadas por los notebooks
│   ├── metrics.json                 # fuente única de todas las cifras del informe y la presentación
│   └── metrics_preprocesamiento.json
└── docs/
    ├── diccionario_inicial.pdf           # diccionario de la Parte I (con nota de discrepancias)
    ├── diccionario_final.pdf             # diccionario final (21 variables + 8 de trazabilidad)
    ├── auditoria_inicial.md              # auditoría del CSV raw
    ├── informe_final.pdf                 # documento maestro de la Parte II
    └── presentacion_final.pptx / .pdf    # presentación
```

## 5. Dependencias

- Python 3 (probado con Python 3.13).
- Paquetes de `requirements.txt` (versiones exactas usadas para ejecutar los notebooks): `pandas==3.0.5`, `numpy==2.5.3`, `matplotlib==3.11.2`, `seaborn==0.13.2`, `nbconvert==7.17.2`, `nbclient==0.11.0`, `nbformat==5.11.1`, `ipykernel==7.4.0`, `jupyter_core==5.9.1`, `jupyter_client==8.10.0`.
- Solo para `01_acquisition.ipynb` (Colab): `selenium>=4.10`, `beautifulsoup4`, `requests` y Google Chrome.
- Opcional, para regenerar el diccionario: `python-docx==1.2.0` y LibreOffice (`soffice`) para exportar a PDF.

## 6. Ejecución paso a paso

```bash
# 1. Clonar e instalar
git clone [enlace al repositorio GitHub del equipo]
cd proyecto_plazavea
python -m venv .venv && source .venv/bin/activate      # Windows: .venv\Scripts\activate
pip install -r requirements.txt

# 2. Verificar que el dataset raw es el oficial
sha256sum data/raw/productos_plazavea_raw.csv
# esperado: 3a3833e8312f8d27566083b81cd02a86535055587ba38f25de4e5d05f4348f3a

# 3. (Opcional) 01_acquisition.ipynb: solo en Google Colab, con Chrome y acceso a plazavea.com.pe.
#    No forma parte del pipeline reproducible: cada ejecución produce un snapshot distinto.

# 4. Preprocesamiento: genera data/processed/ y el registro de calidad
jupyter nbconvert --to notebook --execute --inplace notebooks/02_preprocessing.ipynb

# 5. Análisis: genera reports/figures, reports/tables y reports/metrics.json
jupyter nbconvert --to notebook --execute --inplace notebooks/03_analysis.ipynb

# 6. (Opcional) Diagrama del pipeline, figuras de la presentación y diccionario final
python scripts/diagrama_pipeline.py
python scripts/figuras_presentacion.py
python scripts/build_diccionario_final.py      # usa la plantilla versionada en templates/
soffice --headless --convert-to pdf --outdir docs docs/diccionario_final.docx
```

El orden es obligatorio: **01 (opcional, Colab) → 02 → 03**. Los notebooks usan rutas relativas (`../data`, `../reports`) y deben ejecutarse desde la carpeta `notebooks/` (nbconvert lo hace automáticamente). Ejecutar 02 y 03 tarda menos de un minuto en un equipo convencional. También pueden abrirse con Jupyter (`jupyter notebook notebooks/`) y ejecutarse con _Run All_.

## 7. Pipeline de datos

Seis fases, las mismas de la Figura 1 y la Tabla 7 del informe y de la diapositiva 5 de la presentación.

| Fase                     | Entrada                            | Proceso                                                                                                                                                                                                                                                                                                                           | Salida                                                                                                                                          | Dónde                          |
| ------------------------ | ---------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------ |
| 1. Ingestión             | Listado web Abarrotes (30 páginas) | Selenium renderiza el JavaScript; BeautifulSoup extrae 12 atributos por tarjeta                                                                                                                                                                                                                                                   | CSV raw                                                                                                                                         | `01_acquisition.ipynb` (Colab) |
| 2. Zona raw              | CSV de la Parte I                  | Se conserva intacto y se verifica con sha256                                                                                                                                                                                                                                                                                      | `data/raw/productos_plazavea_raw.csv` (1438 × 12)                                                                                               | `data/raw/`                    |
| 3. Transformación        | Zona raw                           | Perfil, integración y control de duplicados por `sku`, reglas de dominio, imputación de `presentation` (+ indicador), normalización de marcas, outliers IQR marcados, jerarquía de categoría, descuento reconstruido, contenido y precio unitario, intensidad promocional, reducción de columnas y filtro de alcance (`in_scope`) | Dataset validado + variables derivadas                                                                                                          | `02_preprocessing.ipynb` §1–10 |
| 4. Zona processed        | Salida de 3                        | Exportación UTF-8 y registro de calidad                                                                                                                                                                                                                                                                                           | `productos_plazavea_clean.csv` (1438 × 29), `productos_plazavea_final.csv` (1437 × 21), `registro_calidad.csv`, `metrics_preprocesamiento.json` | `data/processed/`, `reports/`  |
| 5. Preparación analítica | Dataset final                      | Preguntas A–G con n mínimo, IC 95 % de Wilson, Spearman, χ²/Fisher                                                                                                                                                                                                                                                                | Figuras, tablas, `metrics.json`                                                                                                                 | `03_analysis.ipynb`            |
| 6. Consumo               | Métricas, figuras y datasets       | Diccionario, informe y presentación alimentados por `metrics.json`; publicación en GitHub                                                                                                                                                                                                                                         | `docs/`                                                                                                                                         | `scripts/`, `docs/`            |

## 8. Principales resultados

Todas las cifras provienen de `reports/metrics.json` (dataset final, n = 1437 productos). Son asociaciones descriptivas en la muestra extraída.

- **Dataset final:** 1437 productos × 21 variables, 271 marcas, 12 grupos de categoría y 70 subcategorías; sin nulos ni duplicados. El descuento reconstruido identifica **479 productos con rebaja** y 958 sin rebaja.
- **Precios:** mediana S/ 7,90 y media S/ 10,71 (asimetría 2,79); el 64,23 % de los productos cuesta hasta S/ 10. Hay 74 productos (5,15 %) sobre el límite IQR de S/ 27,34, conservados por ser productos válidos (aceites de oliva, endulzantes, displays).
- **Frecuencia promocional:** tasa global de 33,33 % (479/1437; IC 95 % 30,9–35,8). Varía fuertemente por grupo: Packs Abarrotes 100,0 % (n = 60), Arroz 61,8 % (n = 34), Fideos, Pastas y Salsas 52,9 % (n = 140), frente a 10,5 % en Comidas Instantáneas (n = 38). Sin Packs Abarrotes, la tasa global es 30,43 %.
- **Profundidad de las rebajas:** entre los 479 promocionados, mediana 14,71 %, IQR 10,38–20,08 % y máximo 38,14 %; solo 21 productos (4,38 %) superan el 30 %. El nivel _Medio_ (10–20 %] reúne el 52,0 % de los promocionados. El % de descuento no se asocia al nivel de precio (Spearman ρ = 0,02).
- **Marcas:** BELL'S es la marca con más productos (184; 12,8 % de la muestra) y presenta una tasa de rebaja de 10,33 %, frente a 36,71 % del resto de marcas.
- **Precio por contenido:** en 32 subcategorías con n ≥ 15 y contenido validado, el cociente Q3/Q1 del precio por kg o litro va de 1,20 (Arroz Extra) a 9,35 (Sal y Pimienta), con mediana 1,90.

**Insights** (los mismos cinco, con los mismos títulos, del informe §9, la diapositiva 16 y `metrics.json` → `insights_candidatos`):

1. **La frecuencia promocional se concentra en pocos grupos y el promedio global la oculta:** Packs Abarrotes 100,0 % (60/60), Arroz 61,8 % y Fideos, Pastas y Salsas 52,9 %, frente a 10,5 % en Comidas Instantáneas; global 33,3 %.
2. **El descuento típico es moderado (10–20 %) y no se asocia al nivel de precio:** mediana 14,71 %; nivel _Medio_ = 52,0 % de los promocionados; Spearman ρ = 0,02 entre precio actual y `discount_pct`.
3. **BELL'S: amplia presencia, rebajas poco frecuentes y menor precio publicado por kg o litro:** 184 productos, tasa 10,33 % frente a 36,71 % del resto y 29,6 % esperada por su mezcla de grupos; precio por kg o litro menor en 9 de 10 subcategorías comparables.
4. **Comportamiento polarizado entre marcas, entrelazado con la categoría:** entre 32 marcas con n ≥ 10, cinco alcanzan o superan el 90 % (PRIMOR, NICOLINI, DON VITTORIO, MOLITALIA, ALACENA) y tres tienen 0 % (TESORO DEL CAMPO, BARCIDDA, TIYAPUY).
5. **Packs Abarrotes: una categoría con rebaja en el 100 % de sus productos que eleva la tasa global:** 60/60 con rebaja frente a 0/4 packs de otras categorías; sin la categoría, la tasa global es 30,43 %.

## 9. Limitaciones

- **Snapshot de una sola fecha:** no hay serie temporal; los precios y rebajas cambian y una nueva extracción dará cifras distintas.
- **Muestra no aleatoria:** 30 de ~65 páginas, en el orden del listado; solo Abarrotes (incluidos Packs) de un único retailer.
- **Sin ventas ni demanda:** solo precios y rebajas publicados; no se pueden inferir ventas, demanda, márgenes, efectividad de promociones, competitividad ni causalidad.
- **Descuento derivado:** `discount_pct` se reconstruye desde el precio regular publicado; el `discount` original es inválido.
- **Imputación:** 137 presentaciones del dataset final se imputaron desde el nombre; el faltante no es aleatorio (tasa de rebaja 55,47 % en imputados vs 31,0 % en el resto), por lo que el análisis incluye una prueba de sensibilidad.
- **Precio unitario:** mezcla unidades (kg, litro, unidad); solo se compara dentro de una misma subcategoría y con contenido validado.

## 10. Reproducibilidad

| Notebook / script                    | Lee                                                                                    | Produce                                                                                                                                                                                                                                                     |
| ------------------------------------ | -------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `01_acquisition.ipynb`               | Sitio web (en vivo)                                                                    | CSV raw (snapshot nuevo en cada ejecución; no se re-ejecuta)                                                                                                                                                                                                |
| `02_preprocessing.ipynb`             | `data/raw/productos_plazavea_raw.csv`                                                  | `data/processed/productos_plazavea_clean.csv`, `data/processed/productos_plazavea_final.csv`, `reports/metrics_preprocesamiento.json`, `reports/tables/registro_calidad.csv`, `reports/tables/outliers_precio_actual.csv`, figuras `fig01`, `figA1`–`figA3` |
| `03_analysis.ipynb`                  | `data/processed/productos_plazavea_final.csv`, `reports/metrics_preprocesamiento.json` | `reports/figures/fig02`–`fig14`, `figA4`–`figA5`, las demás tablas de `reports/tables/` y `reports/metrics.json`                                                                                                                                            |
| `scripts/diagrama_pipeline.py`       | —                                                                                      | `reports/figures/pipeline.png`                                                                                                                                                                                                                              |
| `scripts/figuras_presentacion.py`    | `data/processed/productos_plazavea_final.csv`, `reports/tables/`                       | `reports/figures/presentacion/slide_*.png`                                                                                                                                                                                                                  |
| `scripts/build_diccionario_final.py` | CSV procesados + `templates/Formato_Diccionario_de_datos.docx`                         | `docs/diccionario_final.docx`                                                                                                                                                                                                                               |

Controles de reproducibilidad:

- **Hash del raw:** `sha256sum data/raw/productos_plazavea_raw.csv` debe devolver `3a3833e8312f8d27566083b81cd02a86535055587ba38f25de4e5d05f4348f3a`.
- **Validaciones `assert`:** el notebook 02 se detiene si el resultado no cumple las cifras oficiales (1438 registros limpios, 1437 analíticos, 0 nulos y 0 duplicados en el final, `current_price ≤ regular_price`, coherencia entre `has_discount` y `discount_pct`).
- **Determinismo:** la ejecución desde cero de 02 → 03 reproduce byte a byte los dos CSV procesados (sha256 del final: `c5137dcd32311cdab70042a0b73f2d84ed84e4adde2ae0678ffbbb80844d6023`; del limpio: `44b55d03be6614150cb8d3d7445e309eb28e098667d6912c24c0fd9f51642800`), las tablas, las figuras y `metrics.json`.
- **Fuente única de cifras:** el informe, la presentación y este README toman sus cifras de `reports/metrics.json`.

## 11. Equipo y responsabilidades (Parte II)

Distribución propuesta y acordada por el equipo (asignación del equipo). Todos los integrantes preparan y exponen la presentación final.

| Integrante                       | Código    | Responsabilidad                                                                                                            | Entregables y evidencia                                                                                                                                                                            |
| -------------------------------- | --------- | -------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Sebastian Loli Gonzalez          | 202420022 | Pipeline y notebook de preprocesamiento: integración, reglas de dominio, valores faltantes, outliers y registro de calidad | `02_preprocessing.ipynb` (§1–6 y 9); `reports/tables/registro_calidad.csv`; secciones 5 y 6 del informe; commits `data:` y `feat(prep):`                                                           |
| Piero Max Ortiz Villafuerte      | 202410409 | Feature engineering, reducción de datos, dataset final y diccionario                                                       | `02_preprocessing.ipynb` (§7, 8 y 10); `data/processed/productos_plazavea_final.csv`; `docs/diccionario_final.pdf`; secciones 4 y 7 del informe; commits `feat(features):` y `docs: diccionario …` |
| Diego Mauricio Alarcon Ballivian | 202410104 | Análisis exploratorio y visualizaciones (preguntas A–G)                                                                    | `03_analysis.ipynb`; `reports/figures/`, `reports/tables/`, `reports/metrics.json`; sección 8 del informe; commits `feat(analysis):`                                                               |
| Joaquin Emilio Justo Guillen     | 202410394 | Repositorio GitHub, README, requirements y reproducibilidad                                                                | `README.md`, `requirements.txt`, `.gitignore`, `scripts/`, estructura de carpetas, verificación de ejecución; sección 11 del informe; commits `chore:`, `build:` y enlace final                    |
| Sofía Fridkas García Chaupis     | 202310531 | Contexto y objetivos actualizados, insights, conclusiones, edición final del informe y presentación                        | Secciones 1–3, 9 y 10 del informe; `docs/informe_final.pdf`; `docs/presentacion_final.pptx`; commit `docs: informe final y presentación`                                                           |
