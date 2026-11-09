# Guion de la presentación final — Proyecto Final Parte II (DS3021)

**Tema:** Precios y promociones observables en Abarrotes de Plaza Vea
**Archivo:** `docs/presentacion_final.pptx` (19 diapositivas, 16:9; PDF en `docs/presentacion_final.pdf`)
**Duración:** 15 minutos de exposición + 5 minutos de preguntas (Zoom). Participan los cinco integrantes.
**Regla de oro:** toda cifra que se diga en voz alta proviene de `reports/metrics.json`. Usar siempre lenguaje descriptivo: "en la muestra extraída", "frecuencia promocional observable", "asociación descriptiva". No hablar de ventas, demanda, competitividad ni causalidad.

---

## 1. Distribución de diapositivas por integrante

| Integrante | Diapositivas | Tiempo aprox. | Mensaje central |
|---|---|---|---|
| Sofía García | 1 Portada · 2 Problema · 3 Objetivos · 18 Conclusiones y limitaciones · 19 Cierre | 3 min 20 s | Qué problema resolvemos, por qué ajustamos el objetivo y qué podemos (y no podemos) concluir |
| Joaquin Justo | 4 Dataset · 10 Repositorio y reproducibilidad · 17 Recomendaciones | 2 min 25 s | Cifra oficial 1438 → 1437, cómo se reproduce todo y qué debe hacer el analista |
| Sebastian Loli | 5 Pipeline · 6 Descuento reconstruido · 7 Decisiones de limpieza | 2 min 55 s | Flujo de datos en seis fases y decisiones de calidad, empezando por el descuento inválido |
| Piero Ortiz | 8 Feature engineering · 9 Dataset final · 16 Insights | 2 min 45 s | Cómo se construyen las variables derivadas y qué patrones muestran |
| Diego Alarcon | 11 Precios · 12 Tasa por grupo · 13 Profundidad del descuento · 14 BELL'S y marcas · 15 Casos extremos y precio por contenido | 3 min 35 s | Lectura de los gráficos clave con una conclusión por gráfico |

Total aproximado: 15 min. Orden de turnos: Sofía (1–3) → Joaquin (4) → Sebastian (5–7) → Piero (8–9) → Joaquin (10) → Diego (11–15) → Piero (16) → Joaquin (17) → Sofía (18–19). Las notas del orador de cada diapositiva del PPTX indican quién expone y qué decir. Las figuras de las diapositivas 11–14 son versiones para proyección de las Figuras 3, 6, 8 y 7 del informe (misma numeración que el informe; generadas por `scripts/figuras_presentacion.py`).

### Detalle por integrante

**Sofía García**
- *Diap. 1 (20 s):* saludo, presentación del equipo y orden de exposición.
- *Diap. 2 (50 s):* situación, necesidad y pregunta. Usuario objetivo: analista que monitorea precios y promociones publicados en el canal web (p. ej., en pricing o category management, inteligencia de mercado o una marca proveedora). Alcance: snapshot de una fecha, 30 de ~65 páginas, solo Abarrotes, sin datos de ventas.
- *Diap. 3 (55 s):* el objetivo de la Parte I (marcas con mayores descuentos, posicionamiento comercial) se ajustó a uno descriptivo porque el descuento no se capturó y la muestra es parcial. Objetivo general con la misma redacción del informe y seis objetivos específicos (OE1–OE6, Tabla 3 del informe), incluido el de reproducibilidad.
- *Diap. 18 (55 s):* tres conclusiones ligadas a los objetivos y cinco limitaciones.
- *Diap. 19 (20 s):* cierre, responsabilidades y apertura de preguntas.

**Joaquin Justo**
- *Diap. 4 (50 s):* raw 1438 × 12 → limpio 1438 × 29 → final 1437 × 21. 0 duplicados por SKU. La fila excluida es de Frutas y Verduras (fuera de alcance).
- *Diap. 10 (45 s):* estructura del repositorio y tres pasos para reproducir (instalar requirements, ejecutar 02 y 03). El notebook 01 no se re-ejecuta.
- *Diap. 17 (50 s):* cinco recomendaciones para el analista de monitoreo.

**Sebastian Loli**
- *Diap. 5 (55 s):* pipeline de seis fases (ingestión, zona raw, transformación, zona processed, preparación analítica y consumo), las mismas de la Figura 1 y la Tabla 7 del informe; por qué Selenium; `metrics.json` como fuente única de cifras.
- *Diap. 6 (60 s):* `discount` = 0 en el 100 % pese a 479 productos con precio actual menor al regular; diagnóstico, verificación y fórmula de reconstrucción.
- *Diap. 7 (60 s):* seis decisiones: `presentation` imputada (138 nulos, 9,6 %), `image_url` excluida (94,51 % nulos), varianza cero (`seller`, `stock`), marcas 275 → 272, outliers conservados (74 sobre S/ 27,34), categoría en tres niveles.

**Piero Ortiz**
- *Diap. 8 (55 s):* features de promoción, de producto y de calidad, y cómo se calcula cada una.
- *Diap. 9 (50 s):* dataset final de 1437 productos, 271 marcas, 12 grupos y 70 subcategorías; 479 con rebaja y 958 sin rebaja; variables reducidas y por qué.
- *Diap. 16 (60 s):* los cinco insights del informe (§9), con su cifra.

**Diego Alarcon**
- *Diap. 11 (45 s, Figura 3 del informe):* histograma de precios: mediana S/ 7,90 < media S/ 10,71; 64,2 % hasta S/ 10.
- *Diap. 12 (45 s, Figura 6):* tasa promocional por grupo con IC 95 %: de 10,5 % a 100,0 %; global 33,3 %. Tres grupos destacan claramente sobre la global (Packs, Arroz, Fideos); Galletas y Golosinas (33,9 %) queda prácticamente en la global.
- *Diap. 13 (40 s, Figura 8):* histograma del descuento entre promocionados: mediana 14,71 %, IQR 10,38–20,08 %.
- *Diap. 14 (45 s, Figura 7 simplificada):* BELL'S 184 productos con tasa 10,3 % frente a 36,7 % del resto; cinco marcas alcanzan o superan el 90 % y tres tienen 0 %.
- *Diap. 15 (40 s, Tabla 20 y Figura 5):* los cinco mayores descuentos (35,2–38,1 %) y 12 rebajas simbólicas (< 3 %), todos válidos; cociente Q3/Q1 del precio por kg o litro de 1,20 a 9,35 (mediana 1,90).

---

## 2. Distribución de responsabilidades de la Parte II (asignación del equipo)

> Propuesta de asignación definida por el equipo. Debe coincidir con la tabla del informe final.

| Integrante | Responsabilidad | Entregables / evidencia |
|---|---|---|
| Sebastian Loli | Pipeline y notebook de preprocesamiento (limpieza, valores faltantes, outliers) | `notebooks/02_preprocessing.ipynb` (secciones 1–6 y 9: perfil, integración, reglas de dominio, faltantes, outliers, transformación, registro de calidad); `reports/figures/fig01_valores_faltantes.png`, `figA1`–`figA3`; secciones del informe "Propuesta de pipeline" y "Preprocesamiento de datos"; commits del notebook 02 y del diagrama de pipeline |
| Piero Ortiz | Feature engineering y dataset/diccionario final | `notebooks/02_preprocessing.ipynb` (secciones 7, 8 y 10: feature engineering, reducción, exportación); `data/processed/productos_plazavea_final.csv`; `docs/diccionario_final.pdf` y `.docx`; secciones del informe "Feature Engineering", "Dataset final" y "Diccionario de datos final"; commits de features y diccionario |
| Diego Alarcon | Análisis y visualizaciones | `notebooks/03_analysis.ipynb`; `reports/figures/fig02`–`fig14` y `figA4`–`figA5` (Figuras 3–12 y A4–A8 del informe); `reports/tables/*.csv`; `reports/metrics.json`; sección del informe "Análisis y visualización de datos"; commits del notebook 03 |
| Joaquin Justo | Repositorio GitHub, README, requirements y reproducibilidad | `README.md`, `requirements.txt`, `.gitignore`, estructura de carpetas; nota de trazabilidad en `notebooks/01_acquisition.ipynb`; sección del informe "Documentación y reproducibilidad en GitHub"; commits de estructura, README y verificación de ejecución limpia |
| Sofía García | Contexto y objetivos actualizados, insights, conclusiones, edición final del informe y presentación | Secciones del informe "Contexto actualizado", "Objetivos actualizados", "Insights encontrados" y "Conclusiones"; `docs/informe_final.pdf` y `.docx`; `docs/presentacion_final.pptx`; `docs/guion_presentacion.md`; commits de documentos |
| Todos | Presentación final | Exposición de sus diapositivas (ver sección 1) y respuesta a preguntas de su área |

---

## 3. Banco de preguntas probables (respuesta corta justificada)

Responde primero el integrante indicado entre corchetes; cualquiera puede complementar.

### A. Adquisición y cifra oficial

**¿Por qué usaron Selenium y no solo requests + BeautifulSoup?** [Joaquin / Sebastian]
Porque Plaza Vea completa las tarjetas de producto con JavaScript. Con `requests` la página respondía, pero solo llegaban contenedores `g-producto` vacíos, sin nombre ni precio. Selenium en modo headless renderiza la página; luego `driver.page_source` se analiza con BeautifulSoup. La paginación se resolvió con `?page=N` porque el scroll y los botones no eran estables.

**¿Por qué el dataset tiene 1438 registros y no 1397 o 1494?** [Joaquin]
La cifra oficial es la del CSV entregado en la Parte I: 1438 registros × 12 variables, 1438 SKU únicos y 0 duplicados, la misma que reporta el informe de la Parte I. 1407 → 1397 corresponde a otra corrida del mismo código (los outputs visibles del notebook 01, con un timeout en una página), y ~1506 → 1494 a otra corrida distinta. El sitio cambia de forma continua, así que cada ejecución es una foto diferente; por trazabilidad usamos solo el CSV oficial, sin modificar y verificado con su hash sha256.

**¿Por qué no re-ejecutan el notebook de adquisición?** [Joaquin]
Requiere Google Colab, instalar Chrome y acceso de red a plazavea.com.pe, y el resultado cambiaría porque el catálogo cambia. El pipeline reproducible parte del CSV raw oficial; el notebook 01 se conserva como evidencia del proceso, con una celda de trazabilidad añadida.

**¿Por qué solo 30 páginas?** [Sofía]
Fue el alcance de extracción definido en la Parte I (30 de ~65 páginas del listado). Por eso hablamos de muestra en el orden del listado, no de censo, y no generalizamos al catálogo completo.

### B. Preprocesamiento

**¿Por qué el `discount` original era inválido?** [Sebastian]
Vale 0 en el 100 % de los registros, pero 479 productos tienen precio actual menor que el regular: la rebaja existe y no se capturó. El selector del texto "-X%" no encontró el elemento y el `fillna(0)` del scraper ocultó el fallo. Los precios sí son confiables (0 casos con precio actual mayor al regular, 0 precios ≤ 0), así que reconstruimos `discount_pct` a partir de ellos. El rango 0–59 % que mencionaba la Parte I era de otra corrida.

**¿Por qué imputaron `presentation` en vez de eliminar filas?** [Sebastian]
Tenía 138 nulos (9,6 %), no 0,07 %. El faltante no es aleatorio: esos productos tienen más rebajas (55,1 % vs 31,0 %) y 37 están en Packs Abarrotes, de modo que eliminarlos sesgaría la tasa promocional. Además, el nombre del producto termina con envase y cantidad ("… Bolsa 5Kg"), así que la información existe. Se imputó por regex y se marcó `presentation_imputed`; se recuperaron los 138 y ninguno quedó como "No especificado". En el dataset final hay 137 imputados, y reportamos un análisis de sensibilidad.

**¿Por qué no imputaron `image_url`?** [Sebastian]
Tiene 1359 nulos (94,51 %), probablemente por el lazy-loading de imágenes en Selenium. Es una variable de trazabilidad sin valor analítico y no hay de dónde recuperarla; imputarla sería inventar datos. Se conserva en el dataset limpio y se excluye del analítico.

**¿Por qué eliminaron `seller` y `stock`?** [Sebastian / Piero]
Tienen varianza cero: `seller` = "plaza vea" y `stock` = True en el 100 % de los registros. No segmentan ni aportan información. `stock` siempre es verdadero porque el listado muestra productos disponibles. Se conservan en el dataset limpio.

**¿Por qué conservaron los outliers?** [Sebastian]
Los 74 productos sobre el límite IQR de S/ 27,34 (5,15 %) son precios reales: aceites de oliva, endulzantes, packs y displays (máximo S/ 89,20, LAKANTO). No son errores de captura, y eliminarlos distorsionaría la descripción de la oferta. Se marcan con `is_price_outlier` y el análisis usa mediana e IQR, que son robustos. Los 6 outliers de descuento (sobre 34,63 %) también se conservan porque son coherentes con sus precios.

**¿Cómo normalizaron las marcas?** [Sebastian]
`strip`, mayúsculas y una clave sin acentos para detectar variantes: CASAVERDE → CASA VERDE, LAYS → LAY'S, NESTLE → NESTLÉ (275 → 272 marcas). Además se reconstruyó el apóstrofo perdido en 24 registros (LIBBY S → LIBBY'S, HELLMANN S → HELLMANN'S…). Las submarcas (VICTORIA / VICTORIA GLACITAS) se conservan porque así las publica el sitio. Se guarda `brand_original` como respaldo.

**¿Por qué la fila de Frutas y Verduras sale del dataset final?** [Joaquin]
El proyecto se delimita a Abarrotes. Es un único registro (Verduras Orientales, sin rebaja); se marca con `in_scope` y queda en el dataset limpio. Por eso el final tiene 1437 registros.

### C. Reducción de variables

**¿Por qué se redujo cada variable?** [Piero]
- `discount`: inválida (0 en el 100 %), reemplazada por `discount_pct`.
- `seller`, `stock`: varianza cero.
- `image_url`: 94,51 % nulos, solo trazabilidad.
- `product_url`: trazabilidad; el `sku` ya identifica al producto.
- `category`: reemplazada por `category_level1`, `category_group` y `subcategory`.

Todas siguen en el dataset limpio (1438 × 29); el final queda en 1437 × 21.

### D. Feature engineering

**¿Cómo se calcula cada feature?** [Piero]
- `has_discount` = 1 si `regular_price − current_price` > S/ 0,005 (la tolerancia evita falsos positivos por redondeo).
- `discount_amount` = `regular_price − current_price`, en soles.
- `discount_pct` = (`regular_price − current_price`) / `regular_price` × 100, con 2 decimales.
- `promotion_intensity`: Sin descuento (0), Bajo (0–10 %], Medio (10–20 %], Alto (> 20 %).
- `category_group` / `subcategory`: niveles 2 y 3 de `category` (si no hay nivel 3, se repite el 2; para Packs, "Packs Abarrotes").
- `content_qty`, `content_unit`, `has_multiplier`: regex de cantidad y unidad (g, ml, un) sobre `presentation`.
- `unit_price`: precio por kg, litro o unidad; `content_validated` indica que el contenido coincide con el nombre.
- `presentation_imputed`, `is_price_outlier` (IQR sobre `current_price`), `is_pack` (categoría Packs o nombre que empieza con "Pack").

**¿Por qué los cortes 10 % y 20 % en `promotion_intensity`?** [Piero]
Coinciden casi con los cuartiles reales del descuento entre promocionados (Q1 = 10,38 %, Q3 = 20,08 %) y son legibles en términos comerciales. Probamos cortes alternativos (intervalos [a, b) y cuartiles exactos) y el nivel predominante sigue siendo Medio, con 52,0 % de los promocionados.

**¿Para qué sirve `unit_price`?** [Piero]
Para comparar precios dentro de una subcategoría sin el efecto del tamaño del envase. Por ejemplo, el cociente Q3/Q1 del precio por kg va de 1,20 (Arroz Extra) a 9,35 (Sal y Pimienta).

### E. Visualizaciones

**¿Por qué cada gráfico?** [Diego] (numeración del informe final)
- *Histograma de precios (Figura 3, diap. 11):* la primera pregunta es cómo se distribuyen los precios; muestra la asimetría (2,79) y justifica usar la mediana.
- *Barras horizontales con IC 95 % (Figura 6, diap. 12):* comparan tasas entre grupos de distinto tamaño y muestran la incertidumbre (Wilson); se ordenan para leer el ranking.
- *Histograma del descuento con bandas (Figura 8, diap. 13):* muestra la profundidad de las rebajas y valida los cortes de `promotion_intensity`.
- *Barras de tasa por marca (Figura 7 simplificada, diap. 14):* extremos entre las 32 marcas con n ≥ 10 y BELL'S, con n en la etiqueta. La Figura A4 del Anexo A (15 marcas con más productos, barras apiladas) es material complementario.
- *Respaldo (en el informe):* dispersión precio vs descuento (Figura 11, ρ = 0,02) y packs frente al resto (Figura 12).

**¿Por qué usan la mediana y no la media?** [Diego]
Porque la distribución es asimétrica a la derecha (mediana S/ 7,90 frente a media S/ 10,71): pocos productos caros inflan la media.

**¿Por qué exigen n ≥ 10 para rankear marcas?** [Diego]
Con pocos productos la tasa es muy inestable: con n entre 10 y 14, el IC 95 % tiene un ancho de 22,8 a 51,9 puntos porcentuales. Siempre mostramos n.

### F. Insights

**¿Qué significa cada insight?** [Piero / Sofía] (mismos títulos que el informe §9, el README §8 y la diapositiva 16)
1. *La frecuencia promocional se concentra en pocos grupos y el promedio global la oculta:* la tasa global (33,3 %) esconde diferencias de más de 50 puntos; Packs Abarrotes 100,0 %, Arroz 61,8 % y Fideos, Pastas y Salsas 52,9 %, y Comidas Instantáneas 10,5 %.
2. *El descuento típico es moderado (10–20 %) y no se asocia al nivel de precio:* entre los 479 promocionados la mediana es 14,71 % y la mitad está entre 10,38 % y 20,08 %; más de 30 % es excepcional (21 productos, 4,4 %). Spearman ρ = 0,02 entre precio actual y % de descuento (sin asociación monótona); el ahorro en soles sí crece con el precio (ρ = 0,81), por construcción.
3. *BELL'S: amplia presencia, rebajas poco frecuentes y menor precio publicado por kg o litro:* 184 productos (12,8 %) con tasa 10,3 % frente a 36,7 % del resto; la diferencia persiste al controlar por su mezcla de categorías (esperada 29,6 %). Su precio por kg o litro es menor que el del resto en 9 de 10 subcategorías comparables. Los datos no explican el motivo.
4. *Comportamiento polarizado entre marcas, entrelazado con la categoría:* entre las 32 marcas con n ≥ 10, cinco alcanzan o superan el 90 % (PRIMOR, NICOLINI, DON VITTORIO, MOLITALIA y ALACENA, esta con exactamente 90,0 %) y tres tienen 0 %; varias de las primeras publican casi todo en Fideos, Pastas y Salsas.
5. *Packs Abarrotes: una categoría con rebaja en el 100 % de sus productos que eleva la tasa global:* 60/60 con rebaja, pero los packs de otras categorías tienen 0/4: se asocia a la categoría del sitio, no al formato pack. Sin esa categoría, la tasa global baja de 33,3 % a 30,4 %.

**¿Esto significa que una marca o categoría vende más o es más competitiva?** [Sofía]
No. Medimos lo que el sitio publica (precios y rebajas) en una fecha; no tenemos ventas, demanda ni márgenes. Son asociaciones descriptivas en la muestra extraída.

**¿Las pruebas estadísticas permiten generalizar?** [Diego / Sofía]
No al catálogo completo, porque la muestra no es aleatoria (30 páginas en el orden del listado). Los IC y pruebas se usan como evidencia de que una diferencia no se debe solo a la variabilidad de muestras pequeñas.

### G. Limitaciones

**¿Cuáles son las principales limitaciones?** [Sofía]
- Snapshot de una sola fecha de extracción.
- 30 de ~65 páginas, en el orden del listado: muestra no aleatoria.
- Solo Abarrotes y sin datos de ventas o demanda.
- El descuento se deriva del precio regular publicado; si el sitio infla el precio regular, no podemos detectarlo.
- 137 presentaciones imputadas en el dataset final, con una tasa de rebaja distinta (55,47 % vs 31,0 %); por eso reportamos tasas con y sin imputados.
- `image_url` perdida casi por completo.

**¿Qué harían para mejorar?** [Joaquin]
Corregir el selector del descuento en el scraper (sin `fillna(0)` silencioso), validar automáticamente que `discount` sea coherente con los precios, extraer todas las páginas y repetir la extracción en varias fechas para comprobar si los patrones se sostienen.
