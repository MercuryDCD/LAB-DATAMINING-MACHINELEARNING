# Guía de Proyecto y Flujo de Laboratorios — Machine Learning & Data Mining

Este archivo define el flujo de trabajo recurrente semanal y los estándares técnicos para los laboratorios de Machine Learning (UniBo / Máster IA).

---

## 0. Perfil del Alumno y Regla Pedagógica de Oro (Zero Prior ML Knowledge)

- **Cero conocimiento previo de Machine Learning / Data Mining:**
  Dani cuenta con una sólida base técnica en informática, software, telecomunicaciones y sistemas, pero **NO tiene conocimientos previos en Machine Learning ni Data Mining**.
- **Fuentes exclusivas de verdad y alcance:**
  Lo **ÚNICO** que Dani debe saber y entender es lo indicado explícitamente en los laboratorios completados y en los apuntes/diapositivas de clase proporcionados (Prof. Claudio Sartori).
- **El laboratorio como ancla y filtro obligatorio de contenidos:**
  No incluir en los documentos ni explicaciones nada que no aparezca en los laboratorios o en los apuntes. **Si todavía no ha aparecido en los laboratorios completados, NO se incluye bajo ningún concepto**. El laboratorio actúa como detonante y ancla obligatoria; los apuntes de clase sirven exclusivamente para fundamentar y expandir lo que ya se ha experimentado en el código.
- **Prohibición estricta de jerga y modelos no introducidos:**
  Queda terminantemente prohibido dar por sentado conceptos avanzados o mencionar algoritmos y técnicas que no se hayan enseñado todavía en los labs o apuntes ya vistos (p. ej., nada de árboles de decisión, KNN, PCA, transformaciones Yeo-Johnson, RFE, curvas PR-AUC, etc., hasta que se vean en su respectiva sesión).
- **Enfoque conceptual primero, datasets como evidencia empírica segundo:**
  El protagonismo de cada explicación recae sobre el **principio metodológico general de ML / ingeniería de datos** y sobre las **estructuras de datos manipuladas en memoria**. Los datasets de cada laboratorio (p. ej. *Iris*, *NordicMart*) se introducen al final de cada fase estrictamente como **casos de estudio y evidencia empírica** para contrastar la teoría con números reales, nunca como personajes de una historia narrativa.
- **Tono académico y riguroso de posgrado:**
  El tono debe ser directo, técnico, maduro y universitario (estudiante de máster con base en ingeniería). Queda prohibido el lenguaje condescendiente, el drama infantil o las metáforas teatrales (nada de "historias detectivescas", "actos", "escenas de crimen" ni "marionetas").
- **Explicación desde primeros principios e intuición lógica:**
  Cada concepto de ML o calidad de datos debe explicarse desde la lógica de manipulación de datos y la intuición práctica:
  - *Identificadores:* No se explican con jerga futura ("overfitting en árboles"), sino desde la lógica relacional: un código único por fila solo permite memorizar la fila actual y no contiene ningún patrón transferible a clientes futuros.
  - *Desbalanceo:* Se ilustra con el clasificador trivial: predecir siempre "no churn" da un 88% de exactitud aparente pero detecta 0 churners reales, siendo inútil para el negocio.
  - *Missingness (MCAR vs MNAR):* Se explica la diferencia entre perder un dato por puro azar (muestra insesgada, imputable) vs perderlo porque el cliente dejó de usar la web (la ausencia es la señal predictiva más fuerte).

---

## 1. Arquitectura del Repositorio y Remotos Git

- **`origin`**: Fork personal del alumno (`https://github.com/MercuryDCD/LAB-DATAMINING-MACHINELEARNING.git`) en rama `main`.
- **`upstream`**: Repositorio oficial de la asignatura (`https://github.com/bladerun16/MLDM_Unibo_public.git`) en rama `main`.
- **Estructura de directorios**:
  - `ML_Lab/data/`: Datasets crudos compartidos (`iris.csv`, `nordicmart_customers.csv`, etc.). **Nunca modificar directamente.**
  - `ML_Lab/notebooks/labXX-<tema>/`: Cuadernos de cada laboratorio.
    - `labXX-<tema>.ipynb`: Plantilla original de la cátedra.
    - `labXX-<tema>-dani.ipynb`: Cuaderno de trabajo de Dani.
  - `finals/`: Carpeta central que contiene toda la estructura de archivos `.tex` de apuntes, notas y resúmenes para Overleaf.
    - `finals/main.tex`: Documento maestro para compilar todos los laboratorios en un único manual de estudio mediante `\input{...}` y el paquete `docmute`.
    - `finals/labXX-<tema>-notes.tex`: Documentos modulares por laboratorio, orientados a la práctica y al código.
    - `finals/code-glossary.tex`: Glosario acumulativo de instrucciones, comandos y skeletons de pipeline recurrentes que se expande lab tras lab.
  - Apuntes de clase de referencia: ubicados en la carpeta padre (`../Machine Learning/` y `../Data Mining/`).

---

## 2. Flujo de Trabajo Semanal (Ciclo de Vida del Lab)

Cada semana se ejecuta rigurosamente el siguiente ciclo:

### Paso 1: Actualización desde Upstream
Sincronizar el material nuevo publicado por los profesores sin sobreescribir el trabajo local:
```powershell
git fetch upstream
git merge upstream/main
```

### Paso 2: Desarrollo del Laboratorio
- Trabajar sobre la copia personal `labXX-<tema>-dani.ipynb`.
- Mantener las tablas crudas (`df`, `iris`, etc.) intactas; todas las transformaciones se aplican sobre copias de trabajo (`customers`, etc.).

### Paso 3: Corrección, Auditoría y Completado (Gemini)
Al solicitar la corrección de un cuaderno:
1. **Auditoría de código**:
   - Corregir errores sintácticos, llamadas deprecadas de pandas (ej. `inplace=True` en reemplazos) y antipatrones.
   - Implementar los ejercicios o análisis que hayan quedado incompletos o en celdas vacías (`# your code here`).
   - Evitar data leakage e inclusión de identificadores en análisis de features o matrices de correlación.
2. **Respuestas conceptuales (Markdown)**:
   - Rellenar exhaustivamente todas las celdas con marcadores `*Write your answer here*`.
   - Justificar siempre el "por qué" de las decisiones estadísticas (asimetría, robustez frente a outliers, paradoja de Simpson, naturaleza de valores perdidos MCAR/MNAR) en base a lo visto en las diapositivas.
   - Completar íntegramente la tabla de registro **EDA log** (hallazgo, evidencia, decisión, estado).
3. **Verificación de ejecución**:
   - Todo el notebook debe ejecutarse secuencialmente de arriba a abajo sin errores ni warnings críticos (emulando *Kernel -> Restart \& Run All*).
   - Comprobar que las aserciones de integridad de datos crudos se satisfacen.

### Paso 4: Publicación a GitHub
```powershell
git add "ML_Lab/notebooks/labXX-<tema>/labXX-<tema>-dani.ipynb"
git commit -m "Completar y corregir labXX-<tema>: respuestas conceptuales y codigo verificado"
git push origin main
```

### Paso 5: Generación de Material de Estudio en LaTeX (`finals/`)
1. Generar el documento modular correspondiente en `finals/labXX-<tema>-notes.tex` siguiendo estrictamente el estándar de la Sección 3.
2. Actualizar el glosario acumulativo `finals/code-glossary.tex` incorporando **exclusivamente** las nuevas tablas de comandos y skeletons de pipeline aprendidos en la sesión (sin adelantar temarios futuros).
3. Incluir las nuevas notas en `finals/main.tex`.

---

## 3. Estándar para Apuntes en LaTeX (`finals/` & `latex-notes-generator`)

### Enfoque Pedagógico: Conceptos Primero, Casos Prácticos Segundo
- **Ubicación:** `finals/labXX-<tema>-notes.tex`, `finals/code-glossary.tex` y compilador `finals/main.tex`.
- **Idioma de los documentos:** **Inglés** (idioma vehicular de la asignatura y de los notebooks).
- **Prohibido compilar en local:** No invocar `pdflatex` ni generar PDFs localmente para evitar errores espurios por paquetes de TeX Live faltantes. El usuario compila directamente en Overleaf.

### Maquetación de Portada e Índice (`finals/main.tex`):
- **Portada e Índice integrados en Página 1:** Prohibido usar el entorno `titlepage` (fuerza saltos de página y hojas en blanco espurias). La primera página combina una cabecera compacta con los metadatos del máster y el bloque `\tableofcontents` (`\small`, `\setstretch{0.92}`, `tocdepth=2`). Los laboratorios comienzan en la página 2 con `\newpage`.

---

### Estructura Obligatoria de Cada Fichero de Notas (`labXX-<tema>-notes.tex`):

Cada archivo modular consta obligatoriamente de tres capítulos técnicos bien definidos (el glosario de comandos NO se incluye en cada lab, sino exclusivamente al final del documento completo en `finals/code-glossary.tex`):

#### 1. Banner Destacado de Cabecera (`tcolorbox`):
- Número de laboratorio (`\Huge\bfseries`) y título técnico formal (`\LARGE\bfseries`).
- Subtítulo conceptual con los temas clave.
- Separador `\hrule` y metadatos a dos columnas: curso, titulación, fuentes/diapositivas de clase y cuaderno de trabajo asociado (`labXX-...-dani.ipynb`).
- Inclusión en índice: `\addcontentsline{toc}{section}{...}`.

#### 2. Chapter 1: Essential Theoretical Foundations
- **Preludio teórico mínimo y riguroso:** Conceptos matemáticos, taxonómicos y estadísticos que el alumno debe dominar antes de programar.
- **Sin jerga de temas futuros:** Solo lo que se requiere en la sesión (p. ej., roles de atributos: Target, Features, IDs; escalas de Stevens; Anscombe y estimadores robustos vs no robustos; missingness MCAR vs MNAR; factores confusores y Simpson; desbalanceo y clasificador trivial).
- Cajas destacadas `\begin{definitionbox}[Concepto]` para formalizaciones clave.

#### 3. Chapter 2: The Methodological Pipeline: Principles, Code & Empirical Verification
Recorrido cronológico dividido en las fases técnicas reales del pipeline de ingeniería de datos. Cada fase sigue estrictamente una estructura de 4 bloques:

1. **`\noindent\textbf{1. Methodological Principle:}\par\vspace{0.2em}`**
   - El principio universal de machine learning / data engineering: por qué es obligatorio teórica, matemática o computacionalmente este paso.
2. **`\noindent\textbf{2. Applied Implementation:}\par\vspace{0.2em}\nopagebreak`**
   - El bloque idiomático y compacto en Python (`\begin{lstlisting}`).
3. **`\noindent\textbf{3. Key Methods:}\par\vspace{0.2em}`**
   - **Cobertura exhaustiva de herramientas:** Desglosa los métodos, funciones, atributos o sintaxis utilizados en el bloque de código que no se hayan explicado previamente.
   - **Estructura obligatoria en dos partes (Qué hace + Para qué lo usamos nosotros):**
     Cada elemento debe explicarse de forma clara y sin jerga de bajo nivel innecesaria (nada de *"consume un stream de bytes y crea un DataFrame"* o *"aplica una máscara booleana a las filas"*), siguiendo estrictamente dos partes:
     1. **Qué hace la función:** Descripción directa, natural y técnica de su operación básica.
     2. **Para qué la usamos nosotros:** Explicación simple, intuitiva y directa, **sin jerga avanzada de Machine Learning** (el alumno parte de cero en ML). Explicar la lógica práctica cotidiana: qué problema de datos resuelve, qué información nos ayuda a ver o qué trampa evita (p. ej., descartar IDs porque son etiquetas individuales que no revelan patrones generales, comparar media y mediana para ver si clientes extremos están inflando el promedio, rescatar el significado real de un código 999 antes de limpiarlo, o formular el "adivinador perezoso" para entender por qué un 88\% de acierto es un engaño en datos desbalanceados).
     *Ejemplo canónico:* `pd.read_csv()`: Carga la información de un archivo CSV en un DataFrame según los parámetros indicados. Lo usamos para traer los datos a un formato tabular manejable y darle a cada columna un nombre claro desde el inicio, evitando que la primera fila de datos se confunda con los encabezados.
4. **`\noindent\textbf{4. Case Study Verification:}\par\vspace{0.2em}`**
   - Introducción de los datasets del laboratorio (Iris, NordicMart, etc.) como evidencia empírica: hallazgos observados, cambios numéricos exactos (p. ej., caída de media de 69.51 a 50.21 días tras desinfectar centinelas; 23.1% vs 9.6% en MNAR; 64.2% de outliers en Gold) y aserciones de regresión.

#### 4. Chapter 3: Methodological Pitfalls & Practical Antipatterns
- Inventario de los 4 a 6 errores más comunes cometidos durante la sesión:
  - Antipatrón conceptual/programático vs práctica correcta.
  - Explicado desde la manipulación de datos y la teoría vista, sin recurrir a jerga no enseñada.

---

### Glosario Acumulativo de Código (`finals/code-glossary.tex`):
- **Ubicación exclusiva al final del documento completo:** Se compila como apéndice general al cierre de `finals/main.tex`. **PROHIBIDO duplicar estas tablas dentro de los documentos modulares de cada laboratorio (`labXX-...-notes.tex`)**.
- Prefijo de numeración desacoplado (`G.1`, `G.2`, etc.) para no arrastrar la numeración de los capítulos.
- Organizado por etapas del pipeline de Machine Learning (Ingesta e Inmutabilidad, Tipado de Stevens, Centinelas y Missingness, Robustez y Outliers, Desbalanceo y Baselines, Bivariado y Simpson).
- Cada sección contiene:
  1. **Tabla Práctica de Comandos (`tabularx`):** Mapeo de alta densidad acotado a `\textwidth` con 3 columnas orientadas al uso real:
     - `Command / Syntax`: Cómo se invoca / sintaxis idiomática en Pandas/NumPy.
     - `What it Does`: Qué acción técnica ejecuta exactamente sobre los datos o la memoria.
     - `When to Use (Pipeline Context)`: Cuándo y para qué usarlo (el detonante y objetivo en el pipeline de ML).
  2. **Recurring Skeleton:** Plantilla de código idiomática, minimalista y reutilizable lista para producción.
- **Estrictamente acumulativo:** Contiene exclusivamente herramientas de los laboratorios ya completados, expandiéndose sesión tras sesión.

---

### Reglas Críticas de Tipografía, Código y Maquetación LaTeX:

1. **Color Distintivo para Código (`codebase`):**
   - Configurado en tono **berry / vino oscuro** (`\definecolor{codebase}{rgb}{0.55, 0.10, 0.25}`).
   - **Resaltado activo en todo el documento:** Todo el código en línea (`\lstinline!...!` y `\texttt{...}`) debe resaltar en color vino frente al texto general negro.
   - En cajas de código (`moderncode`), los identificadores y tokens base se configuran con `basicstyle=\ttfamily\scriptsize\color{codebase}` e `identifierstyle=\color{codebase}` para resaltar sobre el fondo grisáceo.
   - **Color en las tablas del glosario:** La columna de comandos debe incluir obligatoriamente `\color{codebase}` en su especificación de columna: `>{\raggedright\arraybackslash\ttfamily\footnotesize\color{codebase}}p{5.8cm}`. La cabecera de dicha columna se protege para volver a negro con `\textbf{\textcolor{black}{\textrm{Command / Syntax}}}`. Cualquier token de código en las columnas de descripción y contexto debe ir encapsulado en `\texttt{...}` para heredar el color `codebase`.

2. **Geometría y Márgenes (`margin=2cm`):**
   - Todos los documentos (`main.tex`, `labXX-...-notes.tex`, `code-glossary.tex`) deben configurarse con `\usepackage[margin=2cm]{geometry}`.
   - Esto fija un ancho útil de texto (`\textwidth`) de **17.0 cm**, garantizando que los bloques de código y las tablas de 3 columnas encajen con holgura sin invadir los márgenes ni recortarse en Overleaf.

3. **Anti-Colisión de Títulos y Cajas de Código (CRÍTICO):**
   - **PROHIBIDO USAR `\paragraph{...}` INMEDIATAMENTE ANTES DE `\begin{lstlisting}`**:
     En la clase `article` de LaTeX, `\paragraph` es un encabezado *run-in* (en línea) con espaciado horizontal negativo (`-1em`). Al colocar un `lstlisting` a continuación, el compilador monta el marco superior de la caja sobre el texto del título.
   - **Formato obligatorio para títulos antes de listados:**
     ```latex
     \noindent\textbf{2. Applied Implementation:}\par\vspace{0.2em}\nopagebreak
     \begin{lstlisting}[language=Python]
     ...
     \end{lstlisting}
     ```
   - Además, `moderncode` debe mantener siempre `aboveskip=0.8em` para garantizar una separación nítida.

4. **Seguridad, Integridad de Cuadrícula y Control de Márgenes en Tablas (`tabularx`):**
   - **Plantilla canónica obligatoria para tablas del glosario:**
     ```latex
     \begin{tabularx}{\textwidth}{|>{\raggedright\arraybackslash\ttfamily\footnotesize\color{codebase}}p{5.8cm}|>{\raggedright\arraybackslash}X|>{\raggedright\arraybackslash}X|}
     \hline
     \textbf{\textcolor{black}{\textrm{Command / Syntax}}} & \textbf{What it Does} & \textbf{When to Use (Pipeline Context)} \\ \hline
     ... & ... & ... \\ \hline
     \end{tabularx}
     ```
   - **PROHIBIDO usar `\lstinline` dentro de tablas `tabularx`**: Provoca fallos de parseo irrecuperables en Overleaf. Usar siempre `\texttt{...}` o la columna tipada en monoespacio con los caracteres especiales debidamente escapados (p. ej., `\_`, `\$`).
   - **PROHIBIDO usar `\\` para saltos de línea dentro de celdas**: En entornos tabulares con `\arraybackslash`, `\\` actúa como terminador de fila (`\tabularnewline`). Usar `\\` dentro de una celda genera una fila fantasma incompleta que rompe las líneas verticales (`|`), crea huecos en el marco de la tabla y desalinea verticalmente el texto de las columnas contiguas.
   - **Firmas atómicas en una sola línea:** Cada llamada de código se escribe limpia en una sola línea dentro de los 5.8 cm de ancho (hasta 33-35 caracteres). Si excepcionalmente se requiere un salto dentro de una misma celda de texto, debe usarse `\newline` y nunca `\\`.

5. **Navegación e Hipervínculos del Índice (`hyperref` & `\phantomsection`):**
   - Toda llamada a `\addcontentsline{toc}{...}{...}` debe ir inmediatamente precedida de `\phantomsection` para que `hyperref` genere el anclaje interactivo exacto en Overleaf en lugar de apuntar a la página 1.
   - **Jerarquía:** Los Laboratorios y el Glosario Acumulativo entran como `section` en el índice; los capítulos teóricos y secciones temáticas del glosario entran como `subsection`. Las fases intermedias se mantienen como `\subsection*` en el texto para no saturar el índice.

6. **Tratamiento de Comillas Rectas:**
   - Incluir siempre `\usepackage{textcomp}` y `showstringspaces=false` para que las comillas en código Python se compilen rectas (`'...'` y `"..."`) y no como tildes curvas.

---

### Glosario Acumulativo de Código (`finals/code-glossary.tex`):
- Ubicado al final de `finals/main.tex` como apéndice general acumulativo.
- Prefijo de numeración desacoplado (`G.1`, `G.2`, etc.).
- Organizado por etapas del pipeline de Machine Learning.
- Cada sección contiene:
  1. **Tabla de Comandos:** Tablas estrictamente acotadas a `\textwidth` mediante `tabularx`.
  2. **Recurring Skeleton:** Plantilla idiomática, minimalista y reutilizable.
- **Estrictamente acumulativo:** Contiene exclusivamente herramientas de los laboratorios ya completados, sin adelantar temarios futuros.
