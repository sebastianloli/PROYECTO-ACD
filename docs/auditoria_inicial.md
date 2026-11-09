# Auditoría inicial del dataset raw — Plaza Vea, Abarrotes (Parte II)

**Archivo auditado:** `data/raw/productos_plazavea_raw.csv` (sin modificar; sha256 `3a3833e8312f8d27566083b81cd02a86535055587ba38f25de4e5d05f4348f3a`).
**Herramienta:** pandas 3.0.5 (lectura con `sku` como texto: `pd.read_csv(..., dtype={"sku": str})`).
**Alcance:** primera tarea de la Parte II — verificar volumen, tipos, duplicados, faltantes, cardinalidad y rangos; contrastar con lo declarado en el informe de la Parte I; y decidir qué transformaciones y variables derivadas tienen sentido para el análisis descriptivo de precios y promociones observadas.

> Todas las cifras de este documento fueron recalculadas directamente sobre el CSV raw. Los datos representan un *snapshot* (una fecha de extracción) de 30 de ~65 páginas de la categoría Abarrotes: son una **muestra**, no un censo, y no contienen información de ventas ni de demanda.

---

## 1. Cifra oficial

| Indicador | Valor |
|---|---|
| Registros | **1438** |
| Variables | **12** |
| SKU únicos | 1438 |
| Filas duplicadas (todas las columnas) | 0 |
| SKU duplicados | 0 |
| `name` duplicados | 0 |
| `product_url` duplicadas | 0 |

La cifra **1438 × 12** coincide con la reportada en la Parte I y es la única que se utiliza en todos los entregables. Las cifras 1407 → 1397 que aparecen en los outputs del notebook de adquisición corresponden a otra corrida del scraper (ver nota de trazabilidad en `notebooks/01_acquisition.ipynb`).

## 2. Tipos de datos

| Variable | Tipo leído por pandas | Tipo conceptual | Observación |
|---|---|---|---|
| `sku` | `int64` por defecto → forzado a `str` | Identificador | Longitud 2–8 caracteres (2: 3; 3: 43; 4: 187; 5: 201; 8: 1004). Se trata como texto, no como número. |
| `brand` | `str` | Categórica nominal | 275 valores. |
| `name` | `str` | Texto libre | Único por registro. |
| `category` | `str` | Categórica jerárquica | Ruta "nivel 1 / nivel 2 / nivel 3". |
| `seller` | `str` | Categórica | Constante. |
| `stock` | `bool` | Binaria | Constante (`True`). |
| `presentation` | `str` | Categórica (envase + cantidad) | Con nulos. |
| `current_price` | `float64` | Numérica continua (S/) | — |
| `regular_price` | `float64` | Numérica continua (S/) | — |
| `discount` | `float64` | Numérica (%) | Constante (0.0). |
| `product_url` | `str` | Texto (URL) | Trazabilidad. |
| `image_url` | `str` | Texto (URL) | Trazabilidad, con nulos masivos. |

## 3. Valores faltantes

| Variable | Nulos | % |
|---|---|---|
| `presentation` | 138 | 9.60 |
| `image_url` | 1359 | 94.51 |
| Resto de variables (10) | 0 | 0.00 |

## 4. Cardinalidad y longitud de variables categóricas / texto

| Variable | Valores únicos | Longitud mín. | Longitud máx. | Comentario |
|---|---|---|---|---|
| `sku` | 1438 | 2 | 8 | Identificador único. |
| `brand` | 275 | 2 | 25 | 32 marcas con ≥ 10 productos; 80 marcas con un solo producto. |
| `name` | 1438 | 23 | 116 | Contiene marca, descripción y presentación. |
| `category` | 71 | 23 | 72 | 2 rutas con 1 solo producto; 35 rutas con < 15 productos. |
| `seller` | 1 | 9 | 9 | "plaza vea" en el 100%. |
| `stock` | 1 | — | — | `True` en el 100%. |
| `presentation` | 464 | 8 | 23 | 25 tipos de envase distintos (primera palabra). |
| `product_url` | 1438 | 42 | 141 | 100% dominio plazavea.com.pe. |
| `image_url` | 79 (de 79 no nulos) | 90 | 98 | 100% de los no nulos en `plazavea.vteximg.com.br`. |

**Jerarquía de `category`:**

| Nivel 1 | Registros |
|---|---|
| Abarrotes | 1377 |
| Packs | 60 |
| Frutas y Verduras | 1 |

- 1376 rutas tienen 3 niveles y 62 tienen solo 2 niveles (60 de "Packs / Packs Abarrotes" y 2 de "Abarrotes / Salsas, Cremas y Condimentos").
- Nivel 2: 13 valores; nivel 3: 69 valores.
- El único registro de "Frutas y Verduras / Verduras / Verduras Orientales" está fuera del alcance declarado (Abarrotes).

| Nivel 2 (`category_group` propuesto) | Registros | Con `current_price < regular_price` |
|---|---|---|
| Salsas, Cremas y Condimentos | 305 | 74 |
| Galletas y Golosinas | 254 | 86 |
| Snacks y Piqueos | 228 | 57 |
| Fideos, Pastas y Salsas | 140 | 74 |
| Conservas | 120 | 31 |
| Chocolatería | 115 | 34 |
| Packs Abarrotes | 60 | 60 |
| Aceite | 58 | 17 |
| Menestras | 43 | 12 |
| Azúcar y Endulzantes | 42 | 9 |
| Comidas Instantáneas | 38 | 4 |
| Arroz | 34 | 21 |
| Verduras | 1 | 0 |

**Marcas más frecuentes:** BELL'S (184), COSTA (41), ALACENA (30), SIBARITA (27), VILLA NATURA (25), DON VITTORIO (24), AMBROSOLI (23), COSTEÑO (23), MOLITALIA (23), CASA VERDE (18).

## 5. Resumen numérico

| Variable | n | Media | Desv. est. | Mín. | Q1 | Mediana | Q3 | Máx. |
|---|---|---|---|---|---|---|---|---|
| `current_price` (S/) | 1438 | 10.70 | 9.49 | 0.80 | 4.90 | 7.90 | 13.88 | 89.20 |
| `regular_price` (S/) | 1438 | 11.33 | 10.03 | 0.80 | 5.12 | 8.50 | 14.50 | 105.00 |
| `discount` (%) | 1438 | 0.00 | 0.00 | 0.00 | 0.00 | 0.00 | 0.00 | 0.00 |

- Ambas distribuciones de precio tienen asimetría positiva marcada (coeficiente de asimetría 2.79 en `current_price` y 2.86 en `regular_price`).
- Regla IQR sobre `current_price`: límite superior = 13.875 + 1.5 × 8.975 = **27.34 S/** → **74** productos por encima. Sobre `regular_price` el límite es 28.57 S/ y también quedan 74 por encima.
- Los precios más altos corresponden a endulzantes (LAKANTO, S/ 89.20), aceites de oliva extra virgen de 1–2 L, packs de conservas (12 un) y displays de chocolates (20 un): son precios válidos por la naturaleza y el tamaño del producto, no errores de captura.

## 6. Reglas de consistencia verificadas

| Regla | Resultado |
|---|---|
| `current_price ≤ 0` o `regular_price ≤ 0` | 0 casos |
| `current_price > regular_price` | 0 casos |
| `current_price < regular_price` (diferencia > 0.005) | **479** casos (33.3%) |
| `current_price = regular_price` | 959 casos |
| `discount > 0` | **0** casos |
| Espacios sobrantes al inicio/fin en `brand` y `name` | 0 casos |
| `product_url` en dominio plazavea.com.pe | 1438 / 1438 |

## 7. Inconsistencias detectadas

### 7.1 Discrepancias con el informe de la Parte I

| Aspecto | Informe Parte I | CSV oficial (recalculado) | Explicación |
|---|---|---|---|
| Registros × variables | 1438 × 12 | 1438 × 12 | Coincide. |
| Nulos en `presentation` | ≈ 0.07% | **9.60%** (138) | El 0.07% (1 de 1397) proviene de los outputs de otra corrida del notebook, no del CSV entregado. |
| Nulos en `image_url` | 0% | **94.51%** (1359) | Las imágenes se cargan de forma diferida (*lazy-loading*); el atributo `src` no estaba disponible en el HTML renderizado. |
| Rango de `discount` | 0 a 59% | **0 a 0** (100% = 0) | Ver 7.2. El rango 0–59 corresponde a otra corrida. |
| Tamaño máx. `presentation` | 23 | 23 | Coincide. |
| `seller` (valor) | "plazavea" | "plaza vea" | Diferencia de escritura en el diccionario. |
| Tamaño máx. `sku` / rango de precios | 8; 0.80–89.20; 0.80–105.00 | 8; 0.80–89.20; 0.80–105.00 | Coincide. |

### 7.2 Pérdida del descuento (`discount`)

`discount` vale 0.0 en el 100% de los registros, pero 479 productos tienen `current_price < regular_price`. La variable almacenada **no captura la promoción**. Causa probable (código de `01_acquisition.ipynb`): el descuento se buscaba como un nodo de texto que empezara con "-" y contuviera "%"; si el formato del sitio difería, se obtenía `None`, y la limpieza posterior aplicaba `fillna(0)`, convirtiendo el fallo en un 0 indistinguible de "sin promoción".

Descuento reconstruido a partir de los precios, `discount_pct = (regular_price − current_price) / regular_price × 100`:

| Estadístico (solo los 479 con descuento) | Valor |
|---|---|
| Mínimo | 0.69% |
| Q1 | 10.38% |
| Mediana | 14.71% |
| Q3 | 20.08% |
| Máximo | 38.14% |
| Media | 15.45% |

| Descuento en soles (`discount_amount`, 479 casos) | Valor |
|---|---|
| Mínimo | 0.10 |
| Mediana | 1.10 |
| Máximo | 15.80 |

### 7.3 Otras inconsistencias

- **`presentation` faltante pero recuperable:** en los 138 nulos, el `name` termina con la presentación (p. ej. "Arroz Extra BELL'S Bolsa 5Kg", "Filete de Atún en Agua BELL'S Lata 140g", "Galletas Saladitas BELL'S Paquete 6un"). Un patrón regex de envase + cantidad al final del nombre (Bolsa, Paquete, Lata, Botella, Frasco, Caja, Doypack, Sachet, Envase, Pack, Display, Pote, Tableta, etc.) recupera **138 de 138**.
- **`presentation` vs. `name`:** en el 13.2% de los registros no nulos la presentación no aparece literalmente en el nombre (p. ej. nombre "…Paquete 6un" y `presentation` "Bolsa 6un" o "Paquete 226.8g"): el campo del sitio describe a veces la unidad de referencia y no el envase comercial. Se conserva el valor del sitio cuando existe.
- **Variantes de marca:** "CASA VERDE" (18) / "CASAVERDE" (1) y "LAY'S" (16) / "LAYS" (1). Al unificarlas el número de marcas pasa de 275 a 273.
- **Varianza cero:** `seller` ("plaza vea") y `stock` (`True`) son constantes; no aportan información para segmentar. `stock` siempre verdadero es coherente con que el listado solo muestre productos disponibles.
- **Registro fuera de alcance:** 1 producto de "Frutas y Verduras".
- **`sku` numérico:** pandas lo lee como entero; aunque no hay ceros a la izquierda en este archivo, debe tratarse como texto para no alterar identificadores.

## 8. Transformaciones y variables derivadas propuestas

| Transformación / feature | Definición | Justificación |
|---|---|---|
| `sku` como texto | `dtype=str` | Identificador, no magnitud; evita pérdida de formato. |
| Normalización de `brand` | `strip`, mayúsculas, unificación CASAVERDE→CASA VERDE y LAYS→LAY'S | Evita fragmentar una misma marca en rankings. |
| Imputación de `presentation` | Regex sobre el final de `name`; lo no recuperable → "No especificado" | Recupera 138/138 sin eliminar filas. |
| `presentation_imputed` | 1 si la presentación se imputó desde `name` | Trazabilidad de la imputación. |
| `category_group` | Nivel 2 de `category` | 13 grupos en el raw (12 dentro del alcance; Verduras, n = 1, queda fuera). |
| `subcategory` | Nivel 3 de `category` (si no existe, se repite el nivel 2) | Detalle fino; 2 rutas de Abarrotes y las de Packs solo tienen 2 niveles. |
| `in_scope` | 1 si nivel 1 ∈ {Abarrotes, Packs} | Excluye el producto de Frutas y Verduras del análisis. |
| `discount_pct` | (regular − current) / regular × 100, 2 decimales | Sustituye a `discount`, que es inválido (100% = 0). |
| `discount_amount` | regular − current (S/) | Magnitud absoluta del descuento. |
| `has_discount` | 1 si regular − current > 0.005 | Bandera de promoción observable (479 casos). |
| `promotion_intensity` | Sin descuento (0) / Bajo (0, 10] / Medio (10, 20] / Alto (> 20) | Q1 ≈ 10.4% y Q3 ≈ 20.1% entre promocionados: los cortes 10 y 20 son cercanos a los cuartiles y legibles comercialmente (109 / 249 / 121 productos en el dataset final, con `discount_pct` redondeado a 2 decimales; sin redondear, este cálculo preliminar daba 109 / 246 / 124 porque 3 descuentos quedan en exactamente 20,00 % tras el redondeo). |
| `is_price_outlier` | 1 si `current_price` > 27.34 (IQR) | Se marcan (74), **no se eliminan**: son precios válidos. |
| `is_pack` | 1 si nivel 1 = Packs o `name` empieza con "Pack" (64 casos) | Los packs tienen precios y promociones de naturaleza distinta; útil para control. Se mantiene solo si el análisis lo usa. |
| Exclusión de `discount`, `seller`, `stock`, `image_url`, `product_url` del dataset analítico | — | `discount` inválido; `seller` y `stock` constantes; URLs son trazabilidad (y `image_url` 94.5% nula). Se conservan en el dataset limpio completo. |

**Criterio para rankings:** exigir un mínimo de productos por grupo (marca ≥ 10, lo que deja 32 marcas; `category_group` ≥ 15) y mostrar siempre *n*.

## 9. Propuesta de dataset analítico final

Archivo: `data/processed/productos_plazavea_final.csv` — registros con `in_scope = 1` (**1437** = 1438 − 1 fuera de alcance), sin eliminar outliers.

| Columna | Tipo | Origen |
|---|---|---|
| `sku` | texto | Original |
| `brand` | categórica | Original normalizada |
| `name` | texto | Original |
| `category_group` | categórica | Derivada de `category` |
| `subcategory` | categórica | Derivada de `category` |
| `presentation` | categórica | Original + imputada |
| `presentation_imputed` | binaria (0/1) | Derivada |
| `current_price` | numérica (S/) | Original |
| `regular_price` | numérica (S/) | Original |
| `discount_amount` | numérica (S/) | Derivada |
| `discount_pct` | numérica (%) | Derivada (reemplaza a `discount`) |
| `has_discount` | binaria (0/1) | Derivada |
| `promotion_intensity` | categórica ordinal | Derivada |
| `is_price_outlier` | binaria (0/1) | Derivada |
| `is_pack` | binaria (0/1) | Derivada (opcional) |

El dataset limpio completo (`data/processed/productos_plazavea_clean.csv`, 1438 filas) conserva además `category`, `seller`, `stock`, `product_url`, `image_url` e `in_scope` para trazabilidad.

## 10. Limitaciones a declarar

- Snapshot de una sola fecha: los precios y promociones cambian; no hay serie temporal.
- Muestra de 30 de ~65 páginas del listado de Abarrotes (orden definido por el sitio), no un censo.
- El descuento se infiere de la diferencia entre precio regular y precio actual; no se capturó el porcentaje publicado por el sitio en la corrida oficial.
- No hay datos de ventas, demanda, márgenes ni consumidores: el análisis es descriptivo sobre precios y promociones observables en la muestra extraída.

---

## Addendum: decisiones finales tras la revisión del preprocesamiento

Esta auditoría describe el diagnóstico inicial. Las decisiones finales, implementadas en `notebooks/02_preprocessing.ipynb` y registradas en `reports/metrics.json` (bloque `preprocesamiento`), ajustan los siguientes puntos:

- **Marcas:** además de `CASAVERDE` y `LAYS`, se unifica `NESTLE` → `NESTLÉ` (variante detectada al quitar acentos): las marcas pasan de **275 a 272** (no a 273). También se reconstruye el apóstrofo perdido en 4 marcas (`CRICKET S` → `CRICKETS`, `HELLMANN S` → `HELLMANN'S`, `LIBBY S` → `LIBBY'S`, `SNYDER S` → `SNYDER'S`; 24 registros), sin cambiar el conteo. Las submarcas (p. ej. `VICTORIA GLACITAS`) se conservan como se publican. En el dataset analítico (1437) hay 271 marcas porque `SAN SEN` solo aparece en el registro fuera de alcance.
- **Nulos de `presentation`:** el faltante **no es aleatorio**: tasa de rebaja de 55,1 % en los 138 registros sin presentación frente a 31,0 % en el resto (38,6 % frente a 29,8 % sin la categoría Packs). Se mantiene la imputación desde `name` y `presentation_imputed` se usa para medir la sensibilidad de la tasa promocional.
- **Contenido y precio unitario:** se agregan `content_qty`, `content_unit`, `has_multiplier`, `content_validated` y `unit_price` (S/ por kg, litro o unidad). El dataset analítico final tiene **21 columnas**.
- **Outliers:** los límites IQR se aplican sin redondear (límite exacto de `current_price` = 27,3375; se muestra como S/ 27,34). El conteo no cambia (74).
- **`is_pack`:** coincide con `category_group = "Packs Abarrotes"` en 60 de 64 casos; se conserva solo para contrastar la categoría Packs con los 4 packs de otras categorías (ninguno con rebaja).
