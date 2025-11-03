# 📚 GUÍA COMPLETA - PROGRAMACIÓN MULTIPROCESO

## ÍNDICE
1. [1.1 - Introducción a sistemas multitarea](#11-introducción-a-sistemas-multitarea)
2. [1.1.1 - El procesador](#111-el-procesador)
3. [1.1.2 - Sistema operativo y lenguajes](#112-sistema-operativo-y-lenguajes-de-programación)
4. [1.1.3 - Programas, ejecutables, procesos, servicios](#113-programas-ejecutables-procesos-servicios)
5. [1.1.4 - Computación concurrente y paralela](#114-computación-concurrente-y-paralela)
6. [1.1.5 - Programación distribuida](#115-programación-distribuida)
7. [1.1.6 - Hilos](#116-hilos)
8. [1.1.7 - Bifurcación o fork](#117-bifurcación-o-fork)
9. [1.2 - Procesos: conceptos teóricos](#12-procesos-conceptos-teóricos)
10. [1.2.1 - Gestión y estados de procesos](#121-gestión-y-estados-de-los-procesos)
11. [1.2.2 - Comunicación entre procesos](#122-comunicación-entre-procesos)
12. [1.2.3 - Sincronización entre procesos](#123-sincronización-entre-procesos)
13. [1.3.1 - Creación de procesos con Runtime](#131-creación-de-procesos-con-runtime)
14. [1.3.2 - Creación de procesos con ProcessBuilder](#132-creación-de-procesos-con-processbuilder)

---

# 1.1 INTRODUCCIÓN A SISTEMAS MULTITAREA

## 📘 RESUMEN CORTO (Solo leer)
La multitarea es la capacidad de ejecutar múltiples tareas simultáneamente. Sistemas antiguos como MS-DOS eran monotarea, mientras que UNIX ya implementaba multitarea.

---

# 1.1.1 EL PROCESADOR

## 📘 RESUMEN
El procesador ejecuta instrucciones de programas. Puede tener uno o varios núcleos.

## 📗 RESUMEN DETALLADO (Parte importante)
- **Núcleo**: Unidad de procesamiento independiente dentro del CPU
- **Multitarea mononúcleo**: Se logra mediante concurrencia (alternancia rápida)
- **Multitarea multinúcleo**: Paralelismo real con ejecución simultánea
- **Punto clave**: Un procesador mononúcleo SÍ puede realizar multitarea mediante planificación del SO

---

# 1.1.2 SISTEMA OPERATIVO Y LENGUAJES DE PROGRAMACIÓN

## 📗 RESUMEN DETALLADO (Parte importante)

### Sistema Operativo como Intermediario
- Gestiona recursos hardware (CPU, memoria, E/S)
- Intermedia entre programas y hardware
- Detecta eventos (teclado, ratón) y gestiona recursos

### Lenguajes de Programación

**Compilados:**
- Código → Compilador → Ejecutable
- Ejemplos: C, C++
- Ventajas: Alto rendimiento
- Desventajas: No portable entre SO

**Interpretados:**
- Código → Intérprete → Ejecución
- Ejemplos: Python, PHP
- Ventajas: Multiplataforma
- Desventajas: Menor rendimiento

**Máquina Virtual (Java):**
- Código → Bytecode → JVM → Ejecución
- Ventajas: "Write Once, Run Anywhere"
- Desventajas: Requiere JVM

---

# 1.1.3 PROGRAMAS, EJECUTABLES, PROCESOS, SERVICIOS

## 📗 RESUMEN DETALLADO (Parte importante)

| Término | Definición | Ejemplo |
|---------|------------|---------|
| **Programa** | Código fuente | holamundo.java |
| **Ejecutable** | Binario compilado | holamundo.exe |
| **Proceso** | Programa en ejecución | Word abierto |
| **Servicio** | Programa en segundo plano | Antivirus |

**Flujo de creación:**
Programador → Código → Compilador → Ejecutable → Usuario → Proceso

**Proceso vs Servicio:**
- **Proceso**: Interactivo, iniciado por usuario, tiempo limitado
- **Servicio**: Automático, sin interfaz, siempre activo

---

# 1.1.4 COMPUTACIÓN CONCURRENTE Y PARALELA

## 📘 RESUMEN CORTO (hasta el dibujo)
Concurrencia: tareas alternan en un núcleo. Paralelismo: tareas simultáneas en múltiples núcleos.

## 📗 RESUMEN DETALLADO (Parte importante)

**Computación Concurrente:**
- Contexto: 1 núcleo
- Mecanismo: Time-slicing (reparto de tiempo)
- Resultado: Ilusión de simultaneidad
- Responsable: Sistema Operativo

**Computación Paralela:**
- Contexto: Múltiples núcleos
- Mecanismo: Ejecución simultánea
- Resultado: Simultaneidad real
- Responsable: SO + Programador

**Tabla Comparativa:**
| Aspecto | Concurrencia | Paralelismo |
|---------|-------------|------------|
| Unidades | 1 | Múltiples |
| Ejecución | Alternada | Simultánea |
| Responsable | SO | SO + Programa |

---

# 1.1.5 PROGRAMACIÓN DISTRIBUIDA

## 📘 RESUMEN
Ejecución de software en múltiples ordenadores conectados en red.

**Características:**
- Múltiples unidades de computación independientes
- Conectadas mediante red
- Elimina limitación física de núcleos
- Escalabilidad horizontal

**Ventajas:** Mayor potencia, escalabilidad, coste reducido
**Desventajas:** Complejidad, latencia de red

---

# 1.1.6 HILOS

## 📘 RESUMEN CORTO (Solo leer)
Hilo = unidad de ejecución dentro de un proceso. Programas pueden ser monohilo (secuencial) o multihilo (paralelo).

---

# 1.1.7 BIFURCACIÓN O FORK

## 📘 RESUMEN CORTO (Solo leer, pero entender código)
Fork crea copia exacta de un proceso. Original = "padre", copia = "hijo". Cada uno tiene PID diferente y memoria independiente.

### Código C a entender:
```c
#include <stdio.h>
#include <unistd.h>

int main() {
    int contador = 0;
    printf("Antes de fork: %d\n", contador);
    contador++;
    
    pid_t idHijo = fork();  // Punto crítico
    
    contador++;  // Ambos procesos ejecutan esta línea
    
    if (idHijo == 0) {
        printf("HIJO - Contador: %d\n", contador);
    } else {
        printf("PADRE - Contador: %d\n", contador);
    }
    return 0;
}  
```  

**Salida:**


---

# 1.2 PROCESOS: CONCEPTOS TEÓRICOS

## 📗 RESUMEN DETALLADO (Parte importante)

**Definición:** Proceso = programa en ejecución

**Componentes:**
- Instrucciones a ejecutar
- Estado del proceso
- Estado de ejecución (registros CPU)
- Estado de memoria

**Elementos:**
- PID (Process ID - identificador único)
- Memoria propia
- Recursos (archivos, conexiones)
- Contexto (estado completo)

**Cambio de Contexto:**
1. Guardar estado proceso actual
2. Determinar siguiente proceso
3. Restaurar estado siguiente proceso
4. Continuar ejecución

---

# 1.2.1 GESTIÓN Y ESTADOS DE LOS PROCESOS

## 📗 RESUMEN DETALLADO (Parte importante)

**Planificador de Procesos:**
- Componente del SO que gestiona asignación de CPU
- Objetivos: maximizar rendimiento, equidad, minimizar tiempos

**Estados de Procesos:**
| Estado | Descripción |
|--------|-------------|
| **Nuevo** | Recién creado |
| **Listo** | En memoria, esperando CPU |
| **En ejecución** | Usando CPU |
| **Bloqueado** | Esperando evento externo |
| **Finalizado** | Terminado, recursos liberados |

**Diagrama de Estados:**

**Fenómeno Lag:** Pausas momentáneas por sobrecarga del sistema

---

# 1.2.2 COMUNICACIÓN ENTRE PROCESOS

## 📗 RESUMEN DETALLADO (Parte importante)

**IPC (Inter-Process Communication):** Mecanismos para que procesos intercambien información.

**Métodos de IPC:**

1. **Sockets**
   - Comunicación entre máquinas diferentes
   - Bidireccional, bajo nivel
   - Ejemplo: Cliente-Servidor

2. **Flujos E/S**
   - Redirección stdin/stdout/stderr
   - Procesos deben estar relacionados
   - Ejemplo: `proceso1 | proceso2`

3. **RPC/RMI**
   - Llamadas a procedimientos/métodos remotos
   - Transparencia de ubicación
   - Ejemplo: Java RMI

4. **Sistemas de Persistencia**
   - Archivos, bases de datos compartidas
   - Sencillo pero menos eficiente

5. **Servicios Web**
   - HTTP, REST, APIs, Cloud
   - Altamente escalable

---

# 1.2.3 SINCRONIZACIÓN ENTRE PROCESOS

## 📘 RESUMEN + TABLA

Coordinación de ejecución de procesos según sus resultados.

**Mecanismos necesarios:**
- Ejecución de procesos desde otros procesos
- Espera de finalización de procesos
- Generación de códigos de terminación
- Obtención de códigos de terminación

### 📋 TABLA 1.1 - MECANISMOS JAVA

| Mecanismo | Clase | Método | Descripción |
|-----------|-------|--------|-------------|
| Ejecución | Runtime | exec() | Ejecuta comando sistema |
| Ejecución | ProcessBuilder | start() | Crea y ejecuta proceso |
| Espera | Process | waitFor() | Espera fin del proceso |
| Generación código | System | exit(valor) | Termina con código |
| Obtención código | Process | waitFor() | Devuelve código salida |

**Ejemplo flujo:**

---

# 1.3.1 CREACIÓN DE PROCESOS CON RUNTIME

## 📗 RESUMEN DETALLADO (Parte importante) + TABLA

**Clase Runtime:**
- Instancia única por aplicación JVM
- Acceso: `Runtime.getRuntime()`
- Propósito: Interacción con entorno de ejecución

**Ejemplos de uso:**
```java
// Ejecución básica
Runtime.getRuntime().exec("Notepad.exe");

// Con parámetros
String[] comando = {"Notepad.exe", "archivo.txt"};
Process proceso = Runtime.getRuntime().exec(comando);

// Con gestión
int resultado = proceso.waitFor();
