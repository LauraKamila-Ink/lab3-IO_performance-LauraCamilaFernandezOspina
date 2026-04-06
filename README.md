!![Banner]/(banner.png)

# Laboratorio 3: Análisis de Rendimiento de E/S
> **Estructura Física de la Información** > **Estudiante:** Laura Camila Fernández Ospina  
> **Institución:** Universidad de Antioquia

---

## Especificaciones del Sistema

* **Procesador (CPU):** Intel Core i7
* **Memoria RAM:** 12 GB
* **Almacenamiento:** Unidad de Estado Sólido (SSD) NVMe M.2
* **Interfaz:** PCIe Gen 3.0
* **Entorno de Ejecución:** Local (Visual Studio Code + Jupyter Extension)

![Banner](bannerfinal.png)

## Punto de control 1 — Revisión conceptual

1. **¿Qué representa la latencia en este laboratorio?**
    Se considera el tiempo de espera que tiene el disco para prepararse antes de comenzar a soltar datos. Tambien puede verse como un retraso inicial antes de comenzar con transferencia de datos.

2. **¿Qué representa el throughput?**
    Es la velocidad real en la que los datos se mueven y tambien representa cuanta informacion es capaz de mover por segundo.

3. **¿Por qué en acceso secuencial normalmente se asume que M≈1 ?
    Por que los datos estan seguidos unos de otros en el disco entonces el sistema hace ese esfuerzo de busqueda al principio y no tiene por que realizar saltos a otras posicisiones.

4. **¿Por qué en acceso aleatorio $M$ tiende a ser mayor?**
    Por que los datos no estan pegados si no dispersos y el sistema a cada rato debe volver a hacer ese salto o busqueda entonces tarda mas tiempo y acumula retrasos.

![Banner](separador.png)

## Punto de Control 2 - Reflexión sobre la Configuración

* **Tamaño del archivo:** ¿Es suficiente para superar la caché RAM de su equipo?
    Con esos 256MB el archivo no superara el cache de mi RAM, aun asi el archivo deja ver aquella diferencia en los patrones de acceso al ejecutarse pero sigue siendo un archivo pequeño

* **Tamaño de bloque:** Los tamaños evaluados (4 KB, 16 KB, 64 KB, 256 KB) corresponden a tamaños típicos de páginas en sistemas operativos y motores de bases de datos. ¿Cuál esperaría que tuviera mejor rendimiento en acceso aleatorio y por qué?
    Deberia tener mejor rendimiento el de 256 KB por que sin importar si tiene la misma latencia, cuando llega a su inicio entonces lee mas datos y se hace mas eficiente.

* **Entorno de ejecución:** ¿Está ejecutando en local o en Google Colab?
    En local, usando Visual Studio Code con la extension de Jypiter, personalmente es mas comodo y manejable.

![Banner](separador.png)

## Análisis del Experimento

* **¿Qué papel cumple este archivo dentro del experimento?**
    Como tal es un bloque de datos que nos muestra o permite medir cuanto tiempo le cuesta al hardware del disco buscar y leer informacion en condiciones distintas.

* **¿Por qué es útil trabajar con un archivo relativamente grande?**
    Mas que todo ayuda a minizar el efecto de la memoria RAM, eso se logra forzando un poco el sistema para buscar los datos directamente en el disco.

* **¿Qué cree que ocurriría si el archivo fuera demasiado pequeño?**
    Lo que sucederia es que como es tan pequeño el sistema lo cargaria mucho mas rapido pero tiene desventaja de que en las pruebas podria suceder que no se mida la velocidad de memoria del disco y los resultados no representarian el rendimiento real.


![Banner](separador.png)

## Análisis de Resultados Empíricos

* **¿Cuál patrón de acceso fue más rápido para cada tamaño de bloque?**
    El patron secuancial fue mas rapido en los bloques pequeño y hay un detalle de que en el bloque de 256KB se marco un throughput mayor, en este caso de 2990 MiB/s contra 2653 MiB/s entonces se puede deducir que  mi SSD es muy rapida y el bloque es muy minimo comparado a la velocidad de transferencia.

* **¿El throughput cambió al aumentar el tamaño de bloque?**
    Si cambio y bastante, tanto en secuencias como en aleatorio, 
    - En secuencial pase de 371 MiB/s (4 KB) a 2653 MiB/s (256 KB) y en aleatorio pase de 282 MiB/s (4 KB) a 2990 MiB/s (256 KB).

* **¿En qué caso observó la mayor diferencia entre secuencial y aleatorio?**
    Segun el analisis, se ve mucho en el bloque de 16KB que el secuencial (1187 MiB/s) superó al aleatorio (877 MiB/s) por unos 300 MiB/s de diferencia.

![Banner](separador.png)

## Punto de Control 3 - Modelo Teórico vs Práctica

Dispositivo modelado: SSD Aproximado
Latencia asumida: 10e-6
Throughput asumido: 5GB

Elegi este modelo por que los resultados que se mostraron anteriormente muestran velocidades superiores a los 2000 MiB/s, lo cual es físicamente imposible para un disco mecánico o un SSD SATA antiguo. Se parece a mni entorno real ya que el equipo con el que trabajo usualmente tiene una unidad de estado solido moderna

![Banner](separador.png)

## Interpretación Comparativa

* **¿Los tiempos empíricos son mayores o menores que los teóricos?**
    Los tiempos empiricos son mayores que los teoricos, en la columna de elapse_ratio muestra que en el tiempo fue 13.7 veces mas lento de lo que el modelo calculaba.

* **¿En cuál patrón de acceso la teoría se aproxima mejor?**
    Se aproxima mucho mas o mejor en el patron aleatorio, viendo la misma columna de elapsed_ratio para los accesos aleatorios, los valores están muy cerca de 1 (entre 1.28 y 1.42), basicamente el modelo teórico de latencia que elegi es muy preciso para describir como salta el disco entre bloques, pero para el secuencial el modelo ignora muchos procesos intermedios.

* **¿Qué factores reales podrían explicar las diferencias?**
    El cache del sistema, la carga del sistema o el overhead del sofware.

![Banner](separador.png)

### Interprete la gráfica de throughput

* **¿Qué barras son más altas?**
    Las barras mas altas son las del bloque de 256KB, y en el ultimo punto la barra azul de aleatorio supera un poco a la barra naranja de secuencial.

* **¿Qué significa eso en términos de rendimiento?**
    El rendimiento varia mucho o no es constante, mas bien es dependiente del tamaño del bloque.

* **¿Cuál patrón aprovecha mejor la lectura en bloques?**
    El patron aleatorio muestra una mejorar lectura, el secuencial es rapido desde el inicio pero el aleatorio va aumentando poco a poco mientras que el bloque crece

![Banner](separador.png)

### Interprete la gráfica de tiempo
**Explique cómo cambia el tiempo total cuando cambia el tamaño de bloque.**
Cuando aumenta el tamaño del bloque las curvas se comportan de manera diferente, el tiempo secuencial disminuye mucho al principio y luego se estabiliza, mientras que el tiempo aleatorio aumenta de forma lineal y constante. Las curvas divergen mas  en los extremos y el punto de mayor divergencia inicial ocurre en el bloque de 4.0 KiB, donde el acceso secuencial es mucho más lento que el aleatorio.

![Banner](separador.png)

### Interprete la comparación empírico vs teórico - Grafica Sencencial
* **¿Las curvas tienen una tendencia similar?**
    No tienen una tendencia similar ya que la curva teorica se mantiene practicamente plana y constante cerca de los 0.05 segundos mientras que la curva empirica empieza con un tiempo muy alto y cae drasticamente conforme el bloque crece buscando estabilizarse solo al final del grafico

* **¿Dónde se separan más?**
    Se separan mas en el tamaño de bloque mas pequeño de 4.0 kib donde la diferencia es de 0.7 segundos

* **¿Qué le sugiere eso sobre el modelo usado?**
    Que el modelo usado no trata de forma coherente la operacfion en cargas de trabajo con muchos bloques pequeños y no tiene en cuenta el sobrecosto de procesamiento del sistema operativo.

![Banner](separador.png)

### Interprete la comparación empírico vs teórico - Grafica Aleatoria

* **¿Las curvas tienen una tendencia similar?**
    Si, las curvas tienen una tendencia muy similar ya que ambas son lineas rectas que suben de forma proporcional al tamaño del bloque lo que demuestra que el modelo funciona mucho mejor para predecir el comportamiento aleatorio que el secuencial

* **¿Donde se separan mas?**
    Se separan mas en el extremo derecho de la grafica con el bloque de 256.0 kib donde la separacion entre la linea azul y la naranja es mas visible indicando que a mayor cantidad de datos por salto el sistema real empieza a acumular mas retrasos que la formula ideal

* **¿Que le sugiere eso sobre el modelo usado?**
    Que el modelo subestima el tiempo real ya que la latencia elegida de 10 microsegundos se ajusta muy bien al ssd aunque la pequeña diferencia final se debe a que el sistema operativo y el hardware real tardan mas gestionando el orden de cada uno de los saltos

![Banner](separador.png)

### Interprete la ventaja del acceso secuencial 

* **¿Cuál fue el mayor factor de mejora observado?**
    El mayor factor de mejora fue de aproximadamente 1.35x y se observo en el tamaño de bloque de 16.0 kib

* **¿Cómo cambia esa ventaja con el tamaño de bloque?**
    La ventaja tiende a disminuir conforme el bloque se hace mas grande cayendo por debajo de 1.0x al llegar a los 256.0 kib entonces en un ssd nvme la diferencia entre leer secuencial y aleatorio no es mucha.

* **¿Qué implicación tiene esto para el diseño de software?**
    Esto implica que para aplicaciones que manejan bases de datos o archivos pesados es mejor diseñar el software para que pida los datos en bloques de 64 kib o mas ya que esto minimiza la penalizacion por saltos aleatorios y permite que el disco trabaje a su maxima potencia sin importar si la informacion esta dispersa o seguida


![Banner](banner final.png)

## Conclusión final

Los datos en sistemas de almacenamiento, están organizados en bloques fisicos. Entender esto es esencial ya que define la eficiencia, con la cual el hardware accede a la informacion. En este experimento, se verifico que inclusive en una unidad SSD NVMe, sin componentes mecánicos, el acceso secuencial y aleatorio muestran rendimientos diferenciados.Esto es debido al overhead logico del controlador, y a la gestión de capas del sistema operativo. Aunque, el modelo teórico de latencia fija, predijo el comportamiento aleatorio con exactitud, subestimo el secuencial omitiendo el costo de procesar muchísimas solicitudes pequeñas y a pesar de eso, se observo que la ventaja secuencial frente al aleatorio es mucho menor que en discos tradicionales, logrando un speedup máximo de 1.35x con bloques de 16 KiB. Sin embargo, usando bloques mas grandes de 256 KiB, el sistema satura el ancho de banda, logrando un throughput de 2990 MiB/s, casi igualando ambos patrones.
Entendiendo los requerimientos de diseño para optimizar un sistema funcional, es crucial dar precedencia al empleo de transferencias agrupadas de gran escala, o batching, con el objetivo de mitigar la latencia, asi posibilitando que el hardware opere dentro de su rango optimo de rendimiento.

---

## Preguntas de cierre

* **Comparación de patrones:** Con base en sus mediciones, ¿cuántas veces más rápido fue el acceso secuencial respecto al aleatorio en su equipo? ¿Ese resultado era el esperado según la teoría?
    En mi grupo de trabajo, la lectura secuencial mostro un rendimiento alrededor de 1.35 veces superior al acceso aleatorio, medido con bloques de 16 KiB. Teoricamente se anticipaba esta superioridad en el rendimiento secuencial, sin embargo, en las unidades de estado solido esta disparidad se atenuo mucho más frente a los discos duros tradicionales donde usualmente, la diferencia supera el 100x.


* **Efecto del tamaño de bloque:** ¿Qué ocurrió con el throughput del acceso aleatorio a medida que aumentó el tamaño de bloque? ¿Por qué cree que sucede eso?
    El rendimiento en el acceso aleatorio mostro un incremento notable, al ampliar el tamaño del bloque pasando de 282 MiB/s (4 KiB) a unos 2990 MiB/s (256 KiB). Esta alza se manifiesta, pues al realizar lecturas de bloques de mayor dimensión, el costo base de la latencia para cada salto se distribuye sobre una cantidad más amplia de informacion transferida. El disco requiere prácticamente el mismo tiempo para ubicar la informacion, sin embargo, entrega datos mucho mayor por cada solicitud y ahi es donde se ve la eficiencia.

* **Teoría vs práctica:** Identifique un caso en sus resultados donde la medición empírica se alejó del modelo teórico. ¿A qué factor atribuye esa diferencia?
    El caso de disparidad mas pronunciada frente al modelo es en el acceso secuencial empleando bloques de 40 KiB, la practica mostró una lentitud 137 veces mayor que lo predicho. Esta desviacion se debe al overhead al software y al sistema operativo.


* **Tipo de disco:** Compare sus resultados con los valores de referencia de la tabla de la guía. ¿Su equipo se comportó como un HDD, un SSD SATA o un SSD NVMe?
   El rendimiento de mi equipo evidencio las cualidades de un SSD NVMe. Con velocidades proximas a los 3000 MiB/s, sobrepasa  el límite teórico de las interfaces SATA algo asi como 600 MB/s y se distancia considerablemente de un HDD tradicional, unos 150 MB/s.
 
* **Aplicación práctica:** Imagine que debe almacenar una tabla de estudiantes con 1 millón de registros. Con base en lo que midió, ¿preferiría leerla toda de forma secuencial o acceder a registros individuales de forma aleatoria? ¿Por qué?
    Para un millon de entradas optar por una lectura secuencial de la tabla sería mi elección. El acceso secuencial habilita al sistema operativo a implementar lectura anticipada minimizando interrupciones al controlador de disco, eso es muy importante. El acceso aleatorio a un millón de registros requeriría que el sistema gestione un millón de peticiones individualmente un proceso que invariablemente demostrará ser menos eficiente comparado con la fluidez que ofrece la lectura continua y simple de la tabla entera.

---

## Preguntas de Análisis Científico

* **Diferencial de Desempeño:** ¿Cuál patrón de acceso resultó ser más eficiente en su máquina y cuál es la proporción de diferencia (ventaja secuencial)?
    El patrón secuencial resultó el más impactante, usando segmentos de 16.0 KiB y exhibiendo un rendimiento beneficioso de 1.35x. Pero, al utilizar trozos más grandes, como 256.0 KiB, el acceso al azar se emparejó con el secuencial, alcanzando una velocidad de 2990 MiB/s. Esto revela que, en las SSDs NVMe de hoy día, la diferencia de rendimiento desaparece al saturar el ancho de banda del bus.

* **Efecto del Tamaño de Bloque:** ¿Cómo influye el tamaño de la unidad de lectura en la mitigación del costo del acceso aleatorio?
    Un enorme bloque se desempeña como amortiguador de latencia, transferiendo un montón de datos por acción, unos 256 KiB, el intervalo fijo de búsqueda se desvanece por el tiempo real de transferencia. Esto permite que el hardware trabaje cerca de su maximo, suavizando entonces el costo del acceso aleatorio.

* **Correlación con la Teoría:** ¿En qué puntos su hardware se alejó más del modelo teórico y qué factores físicos (interfaz, temperatura, caché) podrían explicarlo?
    El rendimiento del hardware divergío significativamente del modelo teórico de bloques secuenciales de 40 KiB, mostrando una lentitud 13.7 veces superior como se ha reiterado varias veces. Esta discrepancia encuentra sus raíces en la sobrecarga del software, asi como tambien a las restricciones impuestas por las operaciones por segundo (IOPS) en la interfaz PCIe, factores que la formulación ideal ignora.

* **Costo de Acceso:** Explique por qué, incluso en unidades de estado sólido (SSD) sin componentes mecánicos, el acceso aleatorio sigue siendo más costoso que el secuencial.
    Aunque no hay partes móviles, el acceso aleatorio es más costoso porque el controlador del SSD debe consultar constantemente la Tabla de Mapeo (FTL) para traducir direcciones lógicas a celdas físicas dispersas. Esto genera micro-retrasos lógicos y una gestión de canales más compleja que una lectura lineal.

* **Implicaciones en Sistemas:** Si usted estuviera diseñando un Motor de Base de Datos, ¿de qué manera utilizaría estos hallazgos para optimizar la velocidad de recuperación de registros?
    Para optimizar un motor de base de datos, implementaría lectura por lotes. Configuraría el sistema para recuperar datos en páginas de 256 KiB, minimizando las peticiones pequeñas al SO y garantizando que el disco trabaje siempre en su zona de máximo throughput.

![Banner](bannerfinal.png)