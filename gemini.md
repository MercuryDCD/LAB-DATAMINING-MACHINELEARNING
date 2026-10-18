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
    - `finals/labXX-<tema>-notes.tex`: Documentos modulares, autocontenidos y de alta densidad técnica por laboratorio.

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

### Enfoque: Documentos Modulares con Compilador Maestro
- **Ubicación:** `finals/labXX-<tema>-notes.tex` y compilador `finals/main.tex`.
- **Ventaja pedagógica y técnica:** Cada laboratorio genera un documento conciso (3–6 páginas) inmediatamente tras cerrar el cuaderno. Es 100% autocontenido para compilar individualmente en Overleaf, y gracias al paquete `docmute` en `main.tex`, puede compilarse como un único libro completo sin colisión de preámbulos.
- **Idioma del documento:** **Inglés** (idioma vehicular de la asignatura y de los notebooks), con explicaciones y justificaciones de nivel de máster.

### Estructura Obligatoria de Cada Fichero de Notas:
1. **Core Machine Learning Concepts & Theory:**
   - Fundamentos teóricos aplicados en el lab (escalas de medición de Stevens, asimetría y curtosis, robustez estadística y punto de ruptura, desbalanceo de clases y falacia del accuracy, paradoja de Simpson, taxonomía de valores perdidos MCAR/MAR/MNAR).
   - Uso obligatorio de cajas destacadas `\begin{definitionbox}[Concepto]` y `\begin{formulabox}` para teoremas, definiciones y fórmulas.
2. **Canonical Code Snippets & EDA Recipes:**
   - Fragmentos de código idiomático y reutilizable con `listings` (`lstdefinestyle{moderncode}`).
   - Recetas para: detección de centinelas, flags de missingness, reglas Tukey IQR, correlación bivariada vs estratificada, matrices de confusión triviales.
3. **Pitfalls, Traps & Anti-patterns:**
   - Errores conceptuales y de implementación típicos (ej. confundir valores duplicados por columna con filas duplicadas; evaluar modelos con accuracy en clases raras; inferir causalidad de correlaciones agregadas).
4. **Master-Level Exam & Defense Questions:**
   - 3 a 5 preguntas técnicas profundas de auto-evaluación con sus respectivas respuestas argumentadas (estilo examen oral / preguntas de profesor de máster).

### Reglas de Calidad LaTeX:
- **PROHIBIDO COMPILAR EN LOCAL:** No invocar `pdflatex` ni generar PDFs localmente para evitar errores espurios por paquetes de TeX Live faltantes. El usuario compila directamente en Overleaf.
- Seguir estrictamente el preámbulo moderno (`mathpazo`, `geometry`, `tcolorbox`, `fancyhdr`, `listings`, `hyperref`) definido en la skill `latex-notes-generator`.
- Usar `\lstinline!code|with|pipe!` para código en línea con pipes.
