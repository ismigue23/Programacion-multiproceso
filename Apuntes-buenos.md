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
La multitarea puede ser real (hay tantas unidades de proceso como procesos a ejecutar) o simulada (hay menos unidades de proceso que procesos a ejecutar).


---

# 1.1.1 EL PROCESADOR

## 📘 RESUMEN
El procesador ejecuta instrucciones de programas. Puede tener uno o varios núcleos.

## 📗 RESUMEN DETALLADO (Parte importante)
- **Núcleo**: Unidad de procesamiento independiente dentro del CPU
- **Multitarea mononúcleo**: Se logra mediante concurrencia (alternancia rápida)
- **Multitarea multinúcleo**: Paralelismo real con ejecución simultánea
- **Puntos claves**: Un procesador mononúcleo SÍ puede realizar multitarea mediante planificación del SO. La multitarea depende más del SO que del procesador. Un procesador simple con SO adecuado puede tener multitarea, mientras que un multicore con SO inadecuado no aprovechará los recursos.

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

**Optimización:** Los lenguajes interpretados usan técnicas de optimización (caché de bytecode) para no repetir el análisis del código en sucesivas ejecuciones.

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

**Nota importante:** En programas interpretados (Python/Java), el proceso listado en el SO es el del intérprete/JVM, no el nombre del programa.

**Flujo de creación:**
Programador → Código → Compilador → Ejecutable → Usuario → Proceso

**Proceso vs Servicio:**
- **Proceso**: Interactivo, iniciado por usuario, tiempo limitado
- **Servicio**: Automático, sin interfaz, siempre activo, se arranca automáticamente por el SO


---

# 1.1.4 COMPUTACIÓN CONCURRENTE Y PARALELA

## 📘 RESUMEN CORTO (hasta el dibujo)
Concurrencia: tareas alternan en un núcleo. Paralelismo: tareas simultáneas en múltiples núcleos.

## 📗 RESUMEN DETALLADO (Parte importante)

**Computación Concurrente:**
- Ejecución en sistemas mononúcleo mediante time-slicing
- El SO alterna rápidamente entre procesos
- Crea ilusión de simultaneidad
- Responsabilidad principal del sistema operativo

**Computación Paralela:**
- Ejecución real simultánea en múltiples núcleos
- Divide procesos en hilos que se ejecutan en paralelo
- Reduce tiempos de ejecución
- Responsabilidad compartida: SO + programador

**Definiciones clave:**
- **Procesamiento concurrente**: Varios procesos se ejecutan alternativamente en una misma unidad de proceso
- **Procesamiento paralelo**: Divisiones de un proceso se ejecutan simultáneamente en diversos núcleos

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
Fork crea copia exacta de un proceso. Original = "padre", copia = "hijo". Cada uno tiene PID diferente y memoria independiente. **Ambos procesos continúan ejecución desde el punto del fork() con su propia memoria.**

### 🎯 PUNTOS CLAVE DEL CÓDIGO C

```c
#include <stdio.h>
#include <unistd.h>
#include <sys/types.h>

int main(void) {
    int contador = 0;
    printf("Comenzando la ejecución\n");
    
    pid_t idHijo;
    pid_t idPadre;
    idPadre = getpid();
    
    printf("Antes de bifurcar\n");
    contador++;
    
    idHijo = fork();  // ⚡ PUNTO CRÍTICO - SE CREA LA BIFURCACIÓN
    
    contador++;  // 🔄 AMBOS PROCESOS EJECUTAN ESTA LÍNEA
    
    printf("Después de bifurcar\n");
    
    if (idHijo == 0) {
        printf("Id. hijo:%ld Id. padre:%ld Contador:%d \n", 
               (long)getpid(), (long)idPadre, contador);
    } else {
        printf("Id. padre:%ld Id. hijo:%ld Contador:%d \n", 
               (long)getpid(), (long)idHijo, contador);
    }
    return 0;
}
```
### 🔍 EXPLICACIÓN PASO A PASO

**Líneas 1-3: INCLUSIÓN DE BIBLIOTECAS**
- `stdio.h`: Para funciones de entrada/salida como printf
- `unistd.h`: Para la función fork()
- `sys/types.h`: Para el tipo de dato pid_t

**Líneas 5-9: INICIALIZACIÓN**
- Se declara `contador = 0`
- Se muestra "Comenzando la ejecución" (SOLO UNA VEZ)
- Se declaran variables para almacenar PIDs
- Se obtiene el PID del proceso padre con `getpid()`

**Líneas 11-13: ANTES DEL FORK**
- Se muestra "Antes de bifurcar" (SOLO UNA VEZ)
- Se incrementa `contador` a 1 (SOLO UNA VEZ)

**Línea 15: PUNTO DE BIFURCACIÓN**
- `fork()` crea una copia exacta del proceso
- A partir de aquí existen DOS procesos independientes
- **Padre**: `idHijo` recibe el PID del proceso hijo
- **Hijo**: `idHijo` recibe 0

**Líneas 17-19: DESPUÉS DEL FORK**
- `contador++` se ejecuta en AMBOS procesos
- `printf("Después de bifurcar")` se ejecuta DOS veces
- Cada proceso tiene su propia copia de `contador`

**Líneas 21-27: IDENTIFICACIÓN DE PROCESOS**
- `if (idHijo == 0)`: El hijo ejecuta este bloque
- `else`: El padre ejecuta este bloque
- Cada proceso muestra sus PIDs y el valor de su `contador`

### 📊 SALIDA COMPLETA Y EXPLICADA
- Comenzando la ejecución ← SOLO UNA VEZ (antes de fork)
- Antes de bifurcar ← SOLO UNA VEZ (antes de fork)
- Después de bifurcar ← PRIMERA vez (proceso padre)
- Después de bifurcar ← SEGUNDA vez (proceso hijo)
- Id. padre:143 Id. hijo:144 Contador:2 ← Proceso PADRE
- Id. hijo:144 Id. padre:143 Contador:2 ← Proceso HIJO

**¿Por qué contador = 2 en ambos procesos?**
- **Proceso Padre**: contador = 1 → fork() → contador++ → contador = 2
- **Proceso Hijo**: hereda contador = 1 → contador++ → contador = 2

**Framework Java:** Existe ForkJoin framework desde Java 7 para procesamiento paralelo.
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
**Objetivo del planificador:** Conseguir que todos los procesos terminen lo antes posible aprovechando al máximo los recursos del sistema.

**Planificador de Procesos:**
- Componente del SO que gestiona asignación de CPU
- Objetivos: maximizar rendimiento, equidad, minimizar tiempos
- Funciona como un "responsable de almacén" de recursos limitados

### 🔄 DIAGRAMA DE ESTADOS DE PROCESOS

[NUEVO] → [LISTO] ↔ [EN EJECUCIÓN] → [FINALIZADO]
                ↑         ↓
                └── [BLOQUEADO] ←──┘
                
**Estados de Procesos:**
| Estado | Descripción |
|--------|-------------|
| **Nuevo** | Proceso recién creado |
| **Listo** | En memoria, esperando CPU (listo para ejecutar) |
| **En ejecución** | Actualmente usando CPU |
| **Bloqueado** | Esperando evento externo (E/S, recurso) |
| **Finalizado** | Proceso terminado, recursos liberados |

**Transiciones clave:**
- **Listo → Ejecución**: Planificador asigna CPU
- **Ejecución → Listo**: Time-out o interrupción
- **Ejecución → Bloqueado**: Espera por recurso/E/S
- **Bloqueado → Listo**: Recurso disponible

**Fenómeno Lag:** Pausas momentáneas por sobrecarga del sistema cuando una tarea acapara recursos temporalmente

**Recordatorio:** Ni el programador ni el usuario controlan los estados - es responsabilidad exclusiva del planificador del SO.
---

# 1.2.2 COMUNICACIÓN ENTRE PROCESOS

## 📗 RESUMEN DETALLADO (Parte importante)

**IPC (Inter-Process Communication):** Mecanismos para que procesos intercambien información.

### 🔄 MÉTODOS DE IPC

**1. Sockets**
- Comunicación entre máquinas diferentes
- Bidireccional, bajo nivel
- Ejemplo: Cliente-Servidor
- **Ventaja:** Procesos en distintos ordenadores y lenguajes

**2. Flujos E/S (Entrada/Salida)**
- Redirección stdin/stdout/stderr
- **Requisito:** Procesos deben estar relacionados (uno creó al otro)
- Ejemplo: `proceso1 | proceso2` en terminal

**3. RPC/RMI**
- **RPC:** Llamadas a procedimientos remotos (genérico)
- **RMI:** Equivalente en Java, orientado a objetos
- Transparencia de ubicación (no importa dónde esté el proceso)
- **Ventaja:** El proceso llamante no sabe la ubicación del proceso llamado

**4. Sistemas de Persistencia**
- Archivos, bases de datos compartidas
- Sencillo pero menos eficiente
- **Ventaja:** Solución simple para múltiples escenarios

**5. Servicios Web/Internet**
- HTTP, REST, APIs, Cloud
- FTP para transferencia de archivos
- Altamente escalable
- **Ventaja:** Utiliza infraestructura existente de internet

### 🎯 CARACTERÍSTICAS CLAVE

- **Procesos son elementos estancos:** Cada uno tiene su propia memoria, CPU y registros
- **Necesidad de comunicación:** Surgen dependencias naturales entre procesos para E/S de datos
- **Flexibilidad:** Algunos métodos funcionan entre procesos en distintas máquinas (Sockets, RPC, Servicios Web)
---

# 1.2.3 SINCRONIZACIÓN ENTRE PROCESOS

## 📘 RESUMEN + TABLA

La sincronización permite coordinar la ejecución de procesos según sus resultados y códigos de terminación. Es esencial para construir flujos de trabajo donde la ejecución de un proceso depende del resultado de otro.

**Mecanismos necesarios:**
- **Ejecución**: Lanzar procesos desde otros procesos
- **Espera**: Bloquear ejecución hasta que otro proceso termine  
- **Generación de códigos**: Indicar cómo terminó la ejecución
- **Obtención de códigos**: Leer el resultado de otros procesos

### 📋 TABLA 1.1 - MECANISMOS JAVA PARA SINCRONIZACIÓN

| Mecanismo | Clase | Método | Descripción |
|-----------|-------|--------|-------------|
| Ejecución | Runtime | exec() | Ejecuta comando del sistema operativo |
| Ejecución | ProcessBuilder | start() | Crea y ejecuta proceso con más control |
| Espera | Process | waitFor() | Espera a que el proceso termine (bloqueante) |
| Generación código | System | exit(valor) | Termina la JVM con código de retorno |
| Obtención código | Process | exitValue() | Devuelve código de salida (no espera) |
| Obtención código | Process | waitFor() | Devuelve código de salida (espera primero) |

### 🔄 EJEMPLO DE FLUJO DE SINCRONIZACIÓN

- PROCESO 1
- ↓
- ├─ Si código = 0 → PROCESO 1.1 → Si código = 0 → PROCESO 1.1.1
- │ └→ Si código = 1 → PROCESO 1.1.2
- │
- └─ Si código = 1 → PROCESO 1.2 → (cualquier código) → PROCESO 1.2.1
- 
**Explicación del flujo:**
- **PROCESO 1** se ejecuta primero
- Si termina con **código 0** → ejecuta **PROCESO 1.1**
  - Si 1.1 termina con **código 0** → ejecuta **PROCESO 1.1.1**
  - Si 1.1 termina con **código 1** → ejecuta **PROCESO 1.1.2**
- Si termina con **código 1** → ejecuta **PROCESO 1.2**
  - Cuando 1.2 finalice (cualquier código) → ejecuta **PROCESO 1.2.1**

### 💡 APLICACIÓN PRÁCTICA

**Manual de Explotación:** Documento que debe recoger los códigos de finalización de los procesos junto con su descripción, permitiendo construir flujos de trabajo automatizados.

**Ejemplo de códigos:**
- `0`: Proceso completado correctamente
- `1`: Error de conexión a base de datos
- `2`: Archivo de entrada no encontrado
- `3`: Error de permisos
- `4`: Timeout en operación
---
# 1.3.1 CREACIÓN DE PROCESOS CON RUNTIME

**Clase Runtime:**
- Instancia única por aplicación JVM mediante `Runtime.getRuntime()`
- Propósito: Interacción con el entorno de ejecución del sistema operativo
- Método principal: `exec()` para ejecutar comandos externos

**Ejemplos de uso:**
```java
// Ejecutar Notepad en Windows
Runtime.getRuntime().exec("Notepad.exe");

// Con parámetros en array
String[] comando = {"Notepad.exe", "archivo.txt"};
Process proceso = Runtime.getRuntime().exec(comando);

// Con gestión y espera
int resultado = proceso.waitFor();
System.out.println("Proceso terminó con código: " + resultado);
```
| Método | Descripción |
|--------|-------------|
| `destroy()` | Termina forzosamente el proceso |
| `exitValue()` | Devuelve valor de retorno del proceso |
| `getErrorStream()` | Obtiene InputStream de la salida de error |
| `getInputStream()` | Obtiene InputStream de la salida estándar |
| `getOutputStream()` | Proporciona OutputStream hacia la entrada estándar (stdin) del proceso |
| `isAlive()` | Verifica si el proceso está activo |
| `waitFor()` | Espera a que el proceso termine |

# 1.3.2 CREACIÓN DE PROCESOS CON PROCESSBUILDER

## 📗 RESUMEN DETALLADO (Parte importante) + TABLA

**Clase ProcessBuilder:**
- Alternativa más flexible que Runtime
- Permite configurar proceso antes de ejecutarlo
- Método `start()` inicia la ejecución

**Características avanzadas:**
- Configuración de directorio de trabajo
- Acceso al entorno de ejecución
- Redirección de flujos de entrada/salida
- Reutilización para múltiples procesos

**Ejemplos de uso:**
```java
// Creación básica
Process proceso = new ProcessBuilder("Notepad.exe", "datos.txt").start();

// Con configuración de directorio
ProcessBuilder pb = new ProcessBuilder("Notepad.exe", "datos.txt");
pb.directory(new File("/ruta/directorio/"));
Process proceso = pb.start();

// Múltiples procesos desde misma instancia
ProcessBuilder pb = new ProcessBuilder("Notepad.exe");
for (int i = 0; i < 5; i++) {
    pb.start(); // Crea 5 instancias de Notepad
}

// Acceso a variables de entorno
ProcessBuilder pb = new ProcessBuilder("Notepad.exe");
java.util.Map<String, String> env = pb.environment();
System.out.println("Número de procesadores: " + env.get("NUMBER_OF_PROCESSORS"));
```
### 📋 TABLA 1.3 - MÉTODOS PRINCIPALES DE PROCESSBUILDER

| Método | Descripción |
|--------|-------------|
| `start()` | Inicia nuevo proceso con atributos configurados |
| `command()` | Get/Set del programa y argumentos |
| `directory()` | Get/Set del directorio de trabajo |
| `environment()` | Obtiene variables de entorno del proceso |
| `redirectError()` | Configura destino de salida de errores |
| `redirectInput()` | Configura origen de entrada estándar |
| `redirectOutput()` | Configura destino de salida estándar |

**Ventajas sobre Runtime:**
- Mayor control sobre la configuración del proceso
- Reutilización de instancias para múltiples procesos
- Redirección flexible de flujos
- Acceso al entorno de ejecución
