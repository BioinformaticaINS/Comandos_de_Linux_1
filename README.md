# Comandos de Linux 1

## **Objetivo de la Clase**

Al finalizar la sesión, los estudiantes serán capaces de utilizar comandos avanzados de manipulación de texto (`grep`, `sort`, `uniq`, `tr`, `wc`, `rev`, `fold`) para procesar y analizar datos en un proyecto de NGS.

---

## **Estructura del Proyecto NGS**

Primero, crearemos la estructura de carpetas y archivos para simular un proyecto de NGS. Luego, utilizaremos los comandos para manipular y analizar los datos.

#### **1. Creación de la Estructura del Proyecto**

1. **Crear el directorio del proyecto**:
   ```bash
   mkdir Proyecto_NGS
   cd Proyecto_NGS
   ```

2. **Crear subdirectorios**:
   ```bash
   mkdir -p raw_data quality_control assembly annotation results scripts
   ```

3. **Crear archivos FASTQ de ejemplo** en el directorio `raw_data`:
   ```bash
   touch raw_data/sample1.fastq raw_data/sample2.fastq
   ```

4. **Agregar contenido a los archivos FASTQ**:
   ```bash
   echo -e "@SEQ_ID1\nATGCGATCGAT\n+\nIIIIIIIIIII\n@SEQ_ID2\nCGATCGATAGC\n+\nIIIIIIIIIII\n@SEQ_ID3\nTTAGCTAGCTA\n+\nIIIIIIIIIII" > raw_data/sample1.fastq

   echo -e "@SEQ_ID4\nGCGTATAGCTA\n+\nIIIIIIIIIII\n@SEQ_ID5\nCTAGATCGCTA\n+\nIIIIIIIIIII" > raw_data/sample2.fastq
   ```

---

### **2. Práctica con los Comandos**

Ahora que tenemos la estructura del proyecto, practicaremos con los comandos.

---

#### **Comando `grep`**

- **Función**: Buscar patrones en archivos de texto.
  
- **Opciones comunes**:
  - `-i`: Ignorar mayúsculas y minúsculas.
  - `-v`: Invertir la búsqueda, mostrando las líneas que no coinciden.
  - `-c`: Contar las líneas que coinciden con el patrón.
  - `-n`: Mostrar el número de línea donde se encuentra el patrón.
  - `--color`: Resaltar las coincidencias encontradas.

**Ejercicios**:
1. Buscar todas las líneas que contienen "atg" en los archivos FASTQ sin importar mayúsculas o minúsculas.
   ```bash
   grep -i "atg" raw_data/sample1.fastq
   ```
   
2. Mostrar las líneas que **no** contienen la secuencia "CG".
   ```bash
   grep -v "CG" raw_data/sample1.fastq
   ```

3. Contar el número de líneas que contienen la secuencia "ATGC".
   ```bash
   grep -c "ATGC" raw_data/sample1.fastq
   ```

4. Mostrar el número de línea y resaltar las coincidencias con la secuencia "TAGC".
   ```bash
   grep -n --color "TAGC" raw_data/sample1.fastq
   ```

---

#### **Comando `sort`**

- **Función**: Ordenar líneas de texto.

- **Opciones comunes**:
  - `-n`: Ordenar numéricamente.
  - `-r`: Ordenar en orden inverso.
  - `-k N`: Ordenar según la columna N.
  - `-t "delimitador"`: Definir un delimitador de campos.

**Ejercicios**:
1. Ordenar el archivo `lecturas.txt` por la segunda columna de forma numérica.
   ```bash
   sort -k2 -n Analisis/lecturas.txt
   ```

2. Ordenar el archivo `lecturas.txt` en orden inverso.
   ```bash
   sort -r Analisis/lecturas.txt
   ```

3. Ordenar un archivo con datos tabulados usando `TAB` como delimitador y la tercera columna.
   ```bash
   echo -e "Seq1\tGeneA\t300\nSeq2\tGeneB\t200" > Analisis/datos.txt
   sort -t $'\t' -k3 -n Analisis/datos.txt
   ```

---

#### **Comando `uniq`**

- **Función**: Eliminar o contar líneas duplicadas.

- **Opciones comunes**:
  - `-c`: Contar cuántas veces se repite cada línea.
  - `-d`: Mostrar solo las líneas duplicadas.
  - `-u`: Mostrar solo las líneas únicas.

**Ejercicios**:
1. Contar las veces que aparece cada secuencia en `sample1.fastq`.
   ```bash
   grep -v "@" raw_data/sample1.fastq | sort | uniq -c
   ```

2. Mostrar solo las secuencias duplicadas.
   ```bash
   grep -v "@" raw_data/sample1.fastq | sort | uniq -d
   ```

3. Mostrar únicamente las secuencias únicas.
   ```bash
   grep -v "@" raw_data/sample1.fastq | sort | uniq -u
   ```

---

#### **Comando `tr`**

- **Función**: Traducir o eliminar caracteres.

- **Opciones comunes**:
  - `-d`: Eliminar caracteres específicos.
  - `-s`: Comprimir repeticiones consecutivas del mismo carácter.

**Ejercicios**:
1. Convertir todas las letras a minúsculas en `sample1.fastq`.
   ```bash
   cat raw_data/sample1.fastq | tr 'A-Z' 'a-z'
   ```

2. Eliminar todos los números del archivo `sample1.fastq`.
   ```bash
   cat raw_data/sample1.fastq | tr -d '0-9'
   ```

3. Comprimir repeticiones de espacios en una línea con múltiples espacios.
   ```bash
   echo "ATGC     CGTA   TAGC" | tr -s ' '
   ```

---

#### **Comando `wc`**

- **Función**: Contar líneas, palabras y caracteres.

- **Opciones comunes**:
  - `-l`: Contar líneas.
  - `-w`: Contar palabras.
  - `-c`: Contar caracteres.

**Ejercicios**:
1. Contar el número total de líneas en `sample1.fastq`.
   ```bash
   wc -l raw_data/sample1.fastq
   ```

2. Contar el número de palabras en un archivo.
   ```bash
   wc -w raw_data/sample1.fastq
   ```

3. Contar los caracteres totales en `sample1.fastq`.
   ```bash
   wc -c raw_data/sample1.fastq
   ```

---

#### **Comando `rev`**

- **Función**: Invertir el orden de los caracteres en cada línea.

- **Opciones comunes**:
  - No tiene opciones adicionales comunes.

**Ejercicios**:
1. Invertir las secuencias de ADN en el archivo `sample1.fastq`.
   ```bash
   grep -v "@" raw_data/sample1.fastq | grep -v "+" | rev
   ```

---

#### **Comando `fold`**

- **Función**: Ajustar líneas de texto a un ancho específico.

- **Opciones comunes**:
  - `-w N`: Definir el ancho máximo de cada línea.

**Ejercicios**:
1. Ajustar las secuencias de ADN en `sample1.fastq` a un ancho de 5 caracteres.
   ```bash
   grep -v "@" raw_data/sample1.fastq | grep -v "+" | fold -w 5
   ```

2. Ajustar las secuencias a un ancho de 10 caracteres.
   ```bash
   grep -v "@" raw_data/sample1.fastq | grep -v "+" | fold -w 10
   ```

---

### **3. Ejercicio Integrador**

Utiliza todos los comandos y opciones aprendidas para realizar el siguiente ejercicio:

1. **Filtrar y contar secuencias**:
   - Extrae las secuencias que contienen "CG" del archivo `sample1.fastq`.
   ```bash
   grep "CG" raw_data/sample1.fastq > Resultados/secuencias_filtradas.txt
   ```

   - Cuenta cuántas secuencias únicas contienen "CG".
   ```bash
   grep -v "@" Resultados/secuencias_filtradas.txt | grep -v "+" | sort | uniq -u | wc -l
   ```

2. **Invertir y ajustar las secuencias**:
   - Invierte las secuencias y ajústalas a un ancho de 4 caracteres.
   ```bash
   grep -v "@" Resultados/secuencias_filtradas.txt | grep -v "+" | rev | fold -w 4
   ```

---

### **Resumen de Comandos**

| Comando  | Función                      | Opciones comunes                                |
|----------|------------------------------|-------------------------------------------------|
| `grep`   | Buscar patrones en texto      | `-i`, `-v`, `-c`, `-n`, `--color`              |
| `sort`   | Ordenar líneas de texto       | `-n`, `-r`, `-k N`, `-t "delimitador"`          |
| `uniq`   | Eliminar líneas duplicadas    | `-c`, `-d`, `-u`                                |
| `tr`     | Traducir o eliminar caracteres | `-d`, `-s`                                      |
| `wc`     | Contar líneas, palabras, etc. | `-l`, `-w`, `-c`                                |
| `rev`    | Invertir líneas               | No tiene opciones comunes adicionales           |
| `fold`   | Ajustar ancho de texto        | `-w N`                                          |

---

### **Actividad Asincrónica**

1. **Exploración de Datos**:
   - Descarga un archivo FASTQ real de una base de datos pública.
   - Utiliza los comandos aprendidos para filtrar, ordenar y analizar las secuencias.

2. **Desafío**:
   - Modifica el script `procesar_fastq.sh` para incluir opciones avanzadas como la compresión de espacios o el conteo de bases específicas.

---

### **Referencias**

- Blum, R., & Bresnahan, C. (2021). *Linux Command Line and Shell Scripting Bible*. Wiley. 
