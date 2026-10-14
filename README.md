# Rotación de agua · ZMG · v1

Mapa interactivo para consultar los **días de suspensión del servicio** del *Esquema de Rotación de Agua Limpia* del Gobierno de Jalisco en la zona metropolitana de Guadalajara.

Toca tu colonia en el mapa o búscala en la lista para ver su zona y los días de la semana en que se le suspende el servicio.

> ⚠️ **Proyecto no oficial.** Los datos se transcribieron a mano de las imágenes publicadas por el Gobierno de Jalisco y pueden contener errores. Antes de tomar decisiones, consulta siempre los canales oficiales del Gobierno de Jalisco y del SIAPA.

## Funciones

- **Límites por colonia**: 408 de las 570 colonias del calendario aparecen con su polígono real, coloreadas por zona. Las que están en varias zonas van en gris.
- **Modo "Hoy"**: colorea las colonias en rojo (suspendido hoy), naranja (solo parte de la colonia) o verde (con servicio).
- **Ficha de colonia**: al tocar una colonia se muestran su zona y sus días de suspensión. Desde la lista, "Ver en mapa →" lleva a la colonia.
- **Municipios**: Guadalajara, Zapopan, Tlaquepaque y Tonalá, con sus límites como referencia. Al tocar uno se acerca el mapa y la lista se filtra.
- **Búsqueda de colonias**: acepta coincidencias parciales y no distingue acentos.
- **Filtros** por municipio, por zona y por día de la semana.
- **Aviso del día**: muestra qué zonas tienen el servicio suspendido hoy y marca las colonias afectadas.
- **Colonias en varias zonas**: las que se abastecen de varios acueductos aparecen una sola vez, con los días de cada zona y un aviso de que, según la calle, solo aplica una.
- **Diseño adaptable** a celular y con modo oscuro automático.
- **Sin dependencias de servidor**: es un solo archivo HTML estático.

## Calendario semanal

| Zona | Días de suspensión |
|:---:|---|
| 1 | Viernes, Domingo |
| 2 | Martes, Jueves, Domingo |
| 3 | Miércoles, Sábado |
| 4 | Lunes, Jueves |
| 5 | Martes, Miércoles, Sábado |
| 6 | Lunes, Viernes |
| 7 | Sábado, Domingo |

## Archivos

| Archivo | Descripción |
|---|---|
| `index.html` | La página del mapa. Incluye los datos de las colonias, sus polígonos y los límites municipales (unos 320 KB). |
| `rotacion_agua_colonias.xlsx` | *(Opcional)* Hoja de cálculo con municipio, colonia, zona, días de suspensión y si la colonia tiene límite en el mapa, más el calendario semanal. |
| `README.md` | Este archivo. |

## Publicar en GitHub Pages

1. Crea un repositorio **público** en GitHub, por ejemplo `rotacion-agua-zmg`.
2. Sube `index.html` y `README.md` a la raíz del repositorio, ya sea con **Add file → Upload files** o con git:
   ```bash
   git init
   git add index.html README.md
   git commit -m "Mapa de rotación de agua ZMG"
   git branch -M main
   git remote add origin https://github.com/TU_USUARIO/rotacion-agua-zmg.git
   git push -u origin main
   ```
3. En el repositorio entra a **Settings → Pages**.
4. En **Source** elige **Deploy from a branch**, selecciona la rama `main` y la carpeta `/ (root)`, y guarda.
5. Después de uno o dos minutos, el sitio estará en:
   `https://TU_USUARIO.github.io/rotacion-agua-zmg/`

### Verlo en tu computadora

Basta con abrir `index.html` en el navegador. Necesita conexión a internet para cargar Leaflet y el mapa base de OpenStreetMap.

## Actualizar los datos

Todo está dentro de `index.html`, en el bloque `<script>`.

**Cambiar los días de una zona**: edita el objeto `CAL`. Los días van de `0` (lunes) a `6` (domingo).

```js
const CAL = {1:[4,6], 2:[1,3,6], 3:[2,5], 4:[0,3], 5:[1,2,5], 6:[0,4], 7:[5,6]};
```

**Agregar o corregir colonias**: edita el arreglo `rows` dentro de `const DATA = {...}`. Cada colonia ocupa una entrada con este formato:

```js
["Municipio", "NOMBRE DE LA COLONIA", zona]
```

Por ejemplo, `["Zapopan", "CIUDAD GRANJA", 6]`. Si una colonia pertenece a varias zonas, agrega una entrada por cada zona; la página las une automáticamente.

## Cómo se ubican las colonias en el mapa

El calendario oficial y el catálogo de colonias del IIEG no siempre escriben igual los nombres, así que el cruce se hizo en dos pasos:

1. **Coincidencia exacta (375 colonias)**: el nombre es el mismo, sin importar acentos, mayúsculas ni prefijos como "Fraccionamiento" o "Residencial".
2. **Coincidencia aproximada (33 colonias)**: nombres equivalentes revisados a mano, por ejemplo "Cinco de Mayo" y "5 de Mayo", o una sección como "Campesina Sur" dentro de "La Campesina". En el mapa llevan la etiqueta *coincidencia aproximada*.

Ante la duda, la colonia se deja sin polígono para no pintar una equivocada. Por ejemplo, "Chapalita" de Zapopan no se asigna al polígono de "Residencial Chapalita".

Para asignarle un polígono a una colonia, edita el objeto `poly` dentro de `const DATA = {...}`. La clave es `"Municipio|NOMBRE"` y el valor es `["e" | "a" | "s", id del polígono...]`, donde `e` significa exacto, `a` aproximado y `s` sin polígono.

## Pendientes

- [ ] Agregar las colonias de la **Zona 7**. Las imágenes oficiales que se usaron no incluían su lista.
- [x] Mostrar los límites de cada colonia en el mapa (v1: 408 de 570).
- [ ] Completar las 162 colonias sin límite. Muchas son colonias nuevas o con nombre comercial que no existen en el catálogo del INE. En el Excel están marcadas con "No" en la columna *Límite en mapa v1*.

## Fuentes y créditos

- **Calendario y colonias**: *Esquema de Rotación de Agua Limpia*, Gobierno del Estado de Jalisco.
- **Límites de colonias**: Instituto de Información Estadística y Geográfica de Jalisco (IIEG), shapefile *Colonias INE 2024*, tomado del proyecto [Y8-dot/mapa.agua.zmg](https://github.com/Y8-dot/mapa.agua.zmg) (licencia MIT). Los polígonos se simplificaron para reducir el peso del archivo.
- **Límites municipales**: [angelnmara/geojson](https://github.com/angelnmara/geojson). Son límites aproximados, útiles para orientarse pero no precisos a nivel calle.
- **Mapa**: [Leaflet](https://leafletjs.com/) y teselas de [© OpenStreetMap](https://www.openstreetmap.org/copyright).

## Autor

Elaborado por [tovarj4@gmail.com](mailto:tovarj4@gmail.com). Proyecto no oficial y sin fines de lucro.
