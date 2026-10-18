# Guía de Proyecto y Flujo de Laboratorios — Machine Learning & Data Mining

Este archivo define el flujo de trabajo recurrente semanal y los estándares técnicos para los laboratorios de Machine Learning (UniBo / Máster IA).

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
   - Justificar siempre el "por qué" de las decisiones estadísticas (asimetría, robustez frente a outliers, paradoja de Simpson, naturaleza de valores perdidos MCAR/MNAR).
   - Completar íntegramente la tabla de registro **EDA log** (hallazgo, evidencia, decisión, estado).
3. **Verificación de ejecución**:
   - Todo el notebook debe ejecutarse secuencialmente de arriba a abajo sin errores ni warnings críticos (emulando *Kernel -> Restart & Run All*).
   - Comprobar que las aserciones de integridad de datos crudos se satisfacen.

### Paso 4: Publicación a GitHub
```powershell
git add "ML_Lab/notebooks/labXX-<tema>/labXX-<tema>-dani.ipynb"
git commit -m "Completar y corregir labXX-<tema>: respuestas conceptuales y codigo verificado"
git push origin main
```

### Paso 5: Generación de Material de Estudio en LaTeX (`finals/`)
Generar el documento modular correspondiente en `finals/labXX-<tema>-notes.tex` mediante la skill `latex-notes-generator`, e incluirlo en `finals/main.tex`.

---

## 3. Estándar para Apuntes en LaTeX (`finals/` & `latex-notes-generator`)

### Enfoque: Práctico, Modular y Centrado en Código
- **Ubicación:** `finals/labXX-<tema>-notes.tex` y compilador `finals/main.tex`.
- **Filosofía didáctica:** 
  - **Foco en el código:** Cómo se usa, por qué se hace así y para qué sirve en el pipeline de ML.
  - **Teoría concisa y aplicada:** Extraída directamente de las diapositivas de la asignatura (Prof. Claudio Sartori) y del laboratorio, sin demostraciones matemáticas densas ni desarrollos innecesarios.
  - **Sin preguntas de examen:** El documento sirve como guía rápida de referencia y recetario técnico.
- **Idioma del documento:** **Inglés** (idioma vehicular de la asignatura y de los notebooks).

### Estructura Obligatoria de Cada Fichero de Notas:
1. **Essential Theory \& Concepts (Class Notes \& Lab Context):**
   - Conceptos clave de las diapositivas de clase (escalas de medida de Stevens, valores centinela / "nulls in disguise", outliers vs procesos raros, correlación agregada vs estratificada, desbalanceo de clases).
   - Uso de cajas destacadas `\begin{definitionbox}[Concepto]` para reglas clave.
2. **The EDA Checklist / Workflow:**
   - La secuencia metodológica aplicada en el laboratorio (inspección de tipos $\to$ univariado $\to$ calidad y centinelas $\to$ target $\to$ bivariado/confusión $\to$ registro de decisiones).
3. **Practical Code Recipes: How, Why \& What For:**
   - Fragmentos de código idiomático con `listings` (`lstdefinestyle{moderncode}`).
   - Cada receta debe detallar:
     - **How:** Cómo se invoca la función o método de Pandas/Seaborn.
     - **Why:** Por qué se elige ese método frente a alternativas ingenuas (evitar mutaciones `inplace`, evitar distorsiones por escalas).
     - **What For:** Para qué sirve ese paso en la preparación de datos para los modelos.
4. **Key Pitfalls \& Practical Traps:**
   - Errores típicos de programación y concepto en el laboratorio (duplicados por columna vs fila, identificadores filtrados en matrices numéricas, imputación destructiva en datos informativos).

### Reglas de Calidad LaTeX:
- **PROHIBIDO COMPILAR EN LOCAL:** No invocar `pdflatex` ni generar PDFs localmente para evitar errores espurios por paquetes de TeX Live faltantes. El usuario compila directamente en Overleaf.
- Seguir estrictamente el preámbulo moderno (`mathpazo`, `geometry`, `tcolorbox`, `fancyhdr`, `listings`, `hyperref`) definido en la skill `latex-notes-generator`.
- Usar `\lstinline!code|with|pipe!` para código en línea con pipes.
