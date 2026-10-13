# Rotación de agua · ZMG

Mapa interactivo para consultar los **días de suspensión del servicio** del *Esquema de Rotación de Agua Limpia* del Gobierno de Jalisco en la zona metropolitana de Guadalajara.

Toca un municipio en el mapa o busca tu colonia para ver su zona y los días de la semana en que se le suspende el servicio.

> ⚠️ **Proyecto no oficial.** Los datos se transcribieron a mano de las imágenes publicadas por el Gobierno de Jalisco y pueden contener errores. Antes de tomar decisiones, consulta siempre los canales oficiales del Gobierno de Jalisco y del SIAPA.

## Funciones

- **Mapa por municipio**: Guadalajara, Zapopan, Tlaquepaque y Tonalá. Al tocar uno se resalta y la lista se filtra.
- **Búsqueda de colonias**: acepta coincidencias parciales y no distingue acentos.
- **Filtros** por municipio, por zona y por día de la semana.
- **Aviso del día**: muestra qué zonas tienen el servicio suspendido hoy y marca las colonias afectadas.
- **Colonias en varias zonas**: las que se abastecen de varios acueductos aparecen una sola vez, con los días de cada zona.
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
| `index.html` | La página del mapa. Incluye los datos de las colonias y los límites municipales. |
| `rotacion_agua_colonias.xlsx` | *(Opcional)* Hoja de cálculo con municipio, colonia, zona y días de suspensión, más el calendario semanal. |
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

## Pendientes

- [ ] Agregar las colonias de la **Zona 7**. Las imágenes oficiales que se usaron no incluían su lista.
- [ ] Mostrar cada colonia en el mapa, lo que requiere sus polígonos o coordenadas.

## Fuentes y créditos

- **Calendario y colonias**: *Esquema de Rotación de Agua Limpia*, Gobierno del Estado de Jalisco.
- **Límites municipales**: [angelnmara/geojson](https://github.com/angelnmara/geojson). Son límites aproximados, útiles para orientarse pero no precisos a nivel calle.
- **Mapa**: [Leaflet](https://leafletjs.com/) y teselas de [© OpenStreetMap](https://www.openstreetmap.org/copyright).
