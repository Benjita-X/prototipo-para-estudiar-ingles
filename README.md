# Prototipo para estudiar inglés

App web de un solo archivo (`index.html`) para estudiar inglés nivel A1 y sacar nota 7 en la **ES4** del curso *Inglés Inicial* (INACAP, FGES01). No necesita internet, ni instalación, ni servidor: se abre con doble clic.

## Cómo usarla

1. Descarga o clona el repo.
2. Abre `index.html` en Chrome o Edge (también funciona en el celular).
3. Tu progreso se guarda solo en el navegador (`localStorage`, clave `ing_es4_trainer_v1`).

## Contexto del curso (para quien continúe el proyecto)

- **Curso:** Inglés Inicial, nivel A1 (MCER), modalidad presencial, INACAP Temuco.
- **Libro:** Oxford *Wide Angle* Level 1A y 1B (Student Book, Workbook y práctica online).
- **Evaluaciones:**

| Unidad | Evaluación | Tipo | % | Contenido |
|---|---|---|---|---|
| 1 | ES1 | Prueba escrita / online | 15% | Present simple, información personal |
| 1 | ES2 | Presentación oral | 25% | Present simple, información personal |
| 2 | ES3 | Role play | 30% | Present simple y continuous, likes y dislikes |
| 3 | **ES4** | **Prueba mixta (escrita / online)** | **30%** | **Simple past, conectores y secuencia** |

- **ES4 = Unidad 3 "Past Times" = Wide Angle 1B, unidades 7 y 8.** Fecha objetivo: **4 de diciembre de 2026**.
- Temas por clase según el cronograma oficial: clima, *was/were* (afirmativo, negativo, preguntas), fechas y expresiones de tiempo, conectores **and / but / because**, pasado simple regular e irregular (afirmativo, negativo y preguntas con *did*).
- Después de la ES4 hay un examen final escrito de todas las unidades.
- Escala de notas chilena: 60% de exigencia = 4.0, 100% = 7.0.

## Qué hace la app (v0.1)

- **Hoy toca:** sesión de 10 preguntas según el plan por fechas (`PLAN` en el código).
- **Sesión inteligente:** repetición espaciada. Las preguntas falladas aparecen más seguido (cajas 0 a 4, tipo Leitner).
- **Práctica por tema:** was/were, regulares, irregulares, negativo, preguntas, conectores, tiempo y clima, lectura, dictado.
- **Simulacro de 30 preguntas** con cronómetro de 30 minutos y nota estimada en escala chilena.
- **Fichas de verbos irregulares** (49 verbos, con traducción).
- **Listening por dictado** con la voz del navegador (Web Speech API, inglés US, velocidad normal y lenta).
- **Mis errores:** repite solo lo que fallaste.
- **Gamificación:** XP, niveles, racha diaria, meta de 20 preguntas por día y cuenta regresiva al examen.
- **Importar preguntas de ChatGPT:** pega un JSON y las preguntas quedan guardadas en tu navegador.

Banco inicial: 165 preguntas (49 irregulares, 37 regulares, 16 was/were, 15 dictados, 13 preguntas con did, 12 tiempo y clima, 10 negativo, 9 conectores, 4 lectura).

## Estructura del código

Todo está en `index.html`: CSS, HTML y JavaScript sin librerías.

| Parte del script | Qué hace |
|---|---|
| `TOPICS`, `PLAN` | Temas y plan de estudio por fechas |
| `HAND`, `IRR`, `REG` | Banco de preguntas escritas a mano y listas de verbos |
| `buildBank()` | Junta todo, más las preguntas importadas, y les asigna un `id` |
| `pick()`, `weight()` | Selección ponderada por dominio (repetición espaciada) |
| `record()` | Guarda aciertos, errores, XP y racha |
| `home()`, `renderQ()`, `finish()`, `cards()`, `importScreen()` | Pantallas |

## Formato de las preguntas (JSON)

Cuatro tipos. Estos son los mismos que se pueden importar desde ChatGPT.

```json
{"topic":"neg","type":"mcq","q":"I ___ go to school yesterday.","options":["didn't","don't","wasn't"],"answer":0,"explain":"didn't + verbo base"}
{"topic":"q","type":"type","q":"Did they like the food? — No, they ___.","answers":["didn't"],"explain":"respuesta corta"}
{"topic":"q","type":"order","answer":"Where did he go last night?","explain":"Wh- + did + sujeto + verbo base"}
{"topic":"dict","type":"dict","answer":"We didn't watch the movie."}
```

- `topic`: `be`, `reg`, `irr`, `neg`, `q`, `conn`, `time`, `read`, `dict`.
- En `mcq`, `answer` es el índice (desde 0) de la opción correcta.
- En `type`, `answers` lista todas las respuestas aceptadas. Se ignoran mayúsculas, puntuación y apóstrofes curvos.

## Ideas para seguir (backlog)

- [ ] Agregar los temas de ES1 a ES3 (present simple y continuous, likes y dislikes) para el examen final.
- [ ] Más preguntas de **secuencia** (first, then, after that, finally) si el profesor las evalúa.
- [ ] Modo "examen final" mezclando todas las unidades.
- [ ] Gráfico de progreso por día.
- [ ] Instalar como PWA para usarla como app en el celular.
- [ ] Exportar e importar el progreso (copia de seguridad).
- [ ] Publicar con GitHub Pages para abrirla desde cualquier dispositivo.
- [ ] Modo oral (hablar y escribir quedaron fuera de esta primera versión a petición del estudiante).

## Prompt para continuar el proyecto con ChatGPT

Copia esto en ChatGPT y adjunta `index.html`:

> Este es un prototipo de app web de un solo archivo para estudiar inglés A1 (Wide Angle 1B, unidades 7 y 8: pasado simple, was/were, conectores and/but/because) para una prueba de INACAP el 4 de diciembre de 2026. Lee el README para ver el contexto. Quiero que mejores la app sin romper lo que ya funciona, manteniéndola en un solo archivo HTML sin librerías externas. Devuélveme siempre el archivo completo. Primero te pido: [escribe aquí qué quieres cambiar].
