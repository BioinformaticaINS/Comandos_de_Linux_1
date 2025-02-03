# Comandos de Linux 1

## 1. **Objetivo de la Clase**

Al finalizar la sesión, los estudiantes serán capaces de utilizar comandos avanzados de manipulación de texto (`grep`, `sort`, `uniq`, `tr`, `wc`, `rev`, `fold`) para procesar y analizar datos en un proyecto de NGS.

---

### **Estructura del Proyecto NGS**

Primero, crearemos la estructura de carpetas y archivos para simular un proyecto de NGS. Luego, utilizaremos los comandos para manipular y analizar los datos.

#### **1. Creación de la Estructura del Proyecto**

1. **Crear el directorio del proyecto**:
   ```bash
   mkdir Proyecto_NGS
   cd Proyecto_NGS
   ```

2. **Crear subdirectorios**:
   ```bash
   mkdir -p Datos_Crudos Analisis Resultados
   ```

3. **Crear archivos de ejemplo**:
   - En `Datos_Crudos`, creamos un archivo de secuencias en formato FASTA:
     ```bash
     touch Datos_Crudos/secuencias.fasta
     ```
   - En `Analisis`, creamos un archivo de resultados intermedios:
     ```bash
     touch Analisis/resultados_intermedios.txt
     ```
   - En `Resultados`, creamos un archivo final:
     ```bash
     touch Resultados/resultados_finales.txt
     ```

4. **Agregar contenido a los archivos**:
   - En `secuencias.fasta`, agregamos algunas secuencias de ADN:
     ```bash
     echo -e ">Seq1\nATGCGATCG\n>Seq2\nCGATCGATA\n>Seq3\nTTAGCTAGC\n>Seq4\nATGCGATCG" > Datos_Crudos/secuencias.fasta
     ```
   - En `resultados_intermedios.txt`, agregamos algunos datos de análisis:
     ```bash
     echo -e "Seq1\t10\nSeq2\t15\nSeq3\t20\nSeq4\t10" > Analisis/resultados_intermedios.txt
     ```

---

### **2. Práctica con los Comandos**

Ahora que tenemos la estructura del proyecto, practicaremos con los comandos.

#### **Comando `grep` y Expresiones Regulares**

- **Función**: Buscar patrones en archivos de texto.
- **Ejemplo**: Buscar todas las secuencias que contienen "ATGC" en `secuencias.fasta`.
  ```bash
  grep "ATGC" Datos_Crudos/secuencias.fasta
  ```
  **Resultado esperado**:
  ```
  >Seq1
  ATGCGATCG
  >Seq4
  ATGCGATCG
  ```

---

#### **Comando `sort`**

- **Función**: Ordenar líneas de texto.
- **Ejemplo**: Ordenar los resultados intermedios por el número de lecturas.
  ```bash
  sort -k2 -n Analisis/resultados_intermedios.txt
  ```
  **Resultado esperado**:
  ```
  Seq1    10
  Seq4    10
  Seq2    15
  Seq3    20
  ```

---

#### **Comando `uniq`**

- **Función**: Eliminar líneas duplicadas.
- **Ejemplo**: Mostrar secuencias únicas en `secuencias.fasta`.
  ```bash
  grep -v ">" Datos_Crudos/secuencias.fasta | sort | uniq
  ```
  **Resultado esperado**:
  ```
  ATGCGATCG
  CGATCGATA
  TTAGCTAGC
  ```

---

#### **Comando `tr`**

- **Función**: Traducir o eliminar caracteres.
- **Ejemplo**: Convertir todas las letras de `secuencias.fasta` a mayúsculas.
  ```bash
  cat Datos_Crudos/secuencias.fasta | tr 'a-z' 'A-Z'
  ```
  **Resultado esperado**:
  ```
  >SEQ1
  ATGCGATCG
  >SEQ2
  CGATCGATA
  >SEQ3
  TTAGCTAGC
  >SEQ4
  ATGCGATCG
  ```

---

#### **Comando `wc`**

- **Función**: Contar líneas, palabras y caracteres.
- **Ejemplo**: Contar el número de secuencias en `secuencias.fasta`.
  ```bash
  grep ">" Datos_Crudos/secuencias.fasta | wc -l
  ```
  **Resultado esperado**:
  ```
  4
  ```

---

#### **Comando `rev`**

- **Función**: Invertir líneas de texto.
- **Ejemplo**: Invertir las secuencias en `secuencias.fasta`.
  ```bash
  grep -v ">" Datos_Crudos/secuencias.fasta | rev
  ```
  **Resultado esperado**:
  ```
  GCTACGTA
  ATAGCTAG
  CGATCGAT
  GCTACGTA
  ```

---

#### **Comando `fold`**

- **Función**: Ajustar líneas de texto a un ancho específico.
- **Ejemplo**: Mostrar las secuencias en `secuencias.fasta` con un ancho de 5 caracteres.
  ```bash
  grep -v ">" Datos_Crudos/secuencias.fasta | fold -w 5
  ```
  **Resultado esperado**:
  ```
  ATGCG
  ATCG
  CGATC
  GATA
  TTAGC
  TAGC
  ATGCG
  ATCG
  ```

---

### **3. Ejercicio Integrador**

1. **Filtrar secuencias**:
   - Usa `grep` para extraer las secuencias que contienen "CG" y guárdalas en un archivo llamado `secuencias_filtradas.txt`.
     ```bash
     grep "CG" Datos_Crudos/secuencias.fasta > Resultados/secuencias_filtradas.txt
     ```

2. **Contar secuencias únicas**:
   - Usa `sort` y `uniq` para contar cuántas secuencias únicas hay en `secuencias_filtradas.txt`.
     ```bash
     grep -v ">" Resultados/secuencias_filtradas.txt | sort | uniq | wc -l
     ```

3. **Invertir y formatear secuencias**:
   - Usa `rev` y `fold` para invertir las secuencias y mostrarlas con un ancho de 4 caracteres.
     ```bash
     grep -v ">" Resultados/secuencias_filtradas.txt | rev | fold -w 4
     ```

---

### **Resumen de Comandos**

| Comando | Función | Ejemplo |
|---------|---------|---------|
| **`grep`** | Buscar patrones en texto. | `grep "ATGC" archivo.txt` |
| **`sort`** | Ordenar líneas de texto. | `sort -k2 -n archivo.txt` |
| **`uniq`** | Eliminar duplicados. | `sort archivo.txt | uniq` |
| **`tr`**   | Traducir o eliminar caracteres. | `tr 'a-z' 'A-Z' < archivo.txt` |
| **`wc`**   | Contar líneas, palabras y caracteres. | `wc -l archivo.txt` |
| **`rev`**  | Invertir líneas de texto. | `rev archivo.txt` |
| **`fold`** | Ajustar líneas a un ancho específico. | `fold -w 10 archivo.txt` |

---

### **Actividad Asincrónica**

1. **Exploración de Datos**:
   - Descarga un archivo FASTA de secuencias de ADN de una base de datos pública (por ejemplo, NCBI).
   - Usa los comandos aprendidos para filtrar, ordenar y contar las secuencias.

2. **Desafío**:
   - Crea un script que automatice el proceso de filtrado, ordenado y conteo de secuencias en un archivo FASTA.

---

### **Referencias**

- Blum, R., & Bresnahan, C. (2021). *Linux Command Line and Shell Scripting Bible*. Wiley.

---

Esta clase práctica integra los comandos de manera efectiva en un contexto de bioinformática, permitiendo a los estudiantes aplicar lo aprendido en un escenario realista. ¡Espero que sea útil para tu clase! 😊
