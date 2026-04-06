# lab3-IO_performance-TuNombreApellido

Punto de control 1 — Revisión conceptual

¿Qué representa la latencia en este laboratorio?
Se considera el tiempo de espera que tiene el disco para prepararse antes de comenzar a soltar datos.

¿Qué representa el throughput?
Es la velocidad real en la que los datos se mueven y tambien representa cuanta informacion es capaz de mover por segundo.

¿Por qué en acceso secuencial normalmente se asume que  M≈1 ?
Por que los datos estan seguidos unos de otros en el disco entonces el sistema hace ese esfuerzo de busqueda al principio y no tiene por que realizar saltos a otras posicisiones.

¿Por qué en acceso aleatorio  M  tiende a ser mayor?
Por que los datos no estan pegados si no dispersos y el sistema a cada rato debe volver a hacer ese salto o busqueda entonces tarda mas tiempo.

--- 
Punto de control 2 — Reflexión sobre la configuración
Observe los parámetros impresos por la celda anterior y responda:

Tamaño del archivo: ¿Es suficiente para superar la caché RAM de su equipo? Compare con los valores de RAM registrados en la Etapa 1 de la guía.
Con esos 256MB el archivo no superara el cache de mi RAM, aun asi el archivo deja ver aquella diferencia en los patrones de acceso al ejecutarse pero sigue siendo un archivo pequeño


Tamaño de bloque: Los tamaños evaluados (4 KB, 16 KB, 64 KB, 256 KB) corresponden a tamaños típicos de páginas en sistemas operativos y motores de bases de datos. ¿Cuál esperaría que tuviera mejor rendimiento en acceso aleatorio y por qué?
Deberia tener mejor rendimiento el de 256 KB por que sihn importar si tiene la misma latencia, cuando llega a su inicio entonces lee mas datos y se hace mas eficiente.


Entorno de ejecución: ¿Está ejecutando en local o en Google Colab? Recuerde que en Colab los tiempos medidos corresponden al hardware de Google, no al suyo.
En local, usando Visual Studio Code con la extension de Jypiter, personalmente es mas comodo y manejable.



--- 

Analice
Después de crear el archivo, responda:

¿Qué papel cumple este archivo dentro del experimento?
Como tal es un bloque de datos que nos muestra o permite medir cuanto tiempo le cuesta al hardware del disco buscar y leer informacion en condiciones distintas

¿Por qué es útil trabajar con un archivo relativamente grande?
Mas que todo ayuda a minizar el feceto de la memoria RAM, eso se logra forzando un poco el sistema para buscar los datos directamente en el disco

¿Qué cree que ocurriría si el archivo fuera demasiado pequeño?
Lo que sucederia es que como es tan pequeño el sistema lo cargaria mucho mas rapido pero tiene desventaja de que en las pruebas podria suceder que no se mida la velocidad de memoria del disco y los resultados no representarian el rendimiento real

---
Análisis de resultados empíricos
Observe la tabla generada y responda:

¿Cuál patrón de acceso fue más rápido para cada tamaño de bloque?
El patron secuancial fue mas rapido en los bloques pequeño y hay un detalle de que en el bloque de 256KB se marco un throughput mayor, en este caso de 2990 MiB/s contra 2653 MiB/s entonces se puede deducir que  mi SSD es muy rapida y el bloque es muy minimo comparado a la velocidad de transferencia

¿El throughput cambió al aumentar el tamaño de bloque?
Si cambio y bastante, tanto en secuencias como en aleatorio, 
- En secuencial pase de 371 MiB/s (4 KB) a 2653 MiB/s (256 KB) y en aleatorio pase de 282 MiB/s (4 KB) a 2990 MiB/s (256 KB).

¿En qué caso observó la mayor diferencia entre secuencial y aleatorio?
Segun el analisis, se ve mucho en el bloque de 16KB que el secuencial (1187 MiB/s) superó al aleatorio (877 MiB/s) por unos 300 MiB/s de diferencia.


---
Punto de control 3 — Modelo teórico elegido

Indique cuál dispositivo teórico usó para comparar sus resultados:

Dispositivo modelado: SSD Aproximado
Latencia asumida: 10e-6
Throughput asumido: 5GB

Elegi este modelo por que los resultados que se mostraron anteriormente muestran velocidade ssuperiores a los 2000 MiB/s, lo cual es físicamente imposible para un disco mecánico o un SSD SATA antiguo. Se parece a mni entorno real ya que el equipo con el que trabajo usualmente tiene una unidad de estado solido moderna

---
Análisis comparativo: teoría vs práctica
Interprete la tabla comparativa:

¿Los tiempos empíricos son mayores o menores que los teóricos?
Los tiempos empiricos son mayores que los teoricos, en la columna de elapse_ratio muestra que en el tiempo fue 13.7 veces mas lento de lo que el modelo calculaba.

¿En cuál patrón de acceso la teoría se aproxima mejor?
Se aproxima mucho mas o mejor en el patron aleatorio, viendo la misma columna de elapsed_ratio para los accesos aleatorios, los valores están muy cerca de 1 (entre 1.28 y 1.42), basicamente el modelo teórico de latencia que elegi es muy preciso para describir como salta el disco entre bloques, pero para el secuencial el modelo ignora muchos procesos intermedios.

¿Qué factores reales podrían explicar las diferencias?
El cache del sistema, la carga del sistema o el overhead del sofware.

---

Interprete la gráfica de throughput
Describa con sus palabras qué muestra esta gráfica:

¿Qué barras son más altas?
Las barras mas altas son las del bloque de 256KB, y en el ultimo punto la barra azul de aleatorio supera un poco a la barra naranja de secuencial.

¿Qué significa eso en términos de rendimiento?
El rendimiento varia mucho o no es constante, mas bien es dependiente del tamaño del bloque.

¿Cuál patrón aprovecha mejor la lectura en bloques?
El patron aleatorio muestra una mejorar lectura, el secuencial es rapido desde el inicio pero el aleatorio va aumentando poco a poco mientras que el bloque crece

---
Interprete la gráfica de tiempo
Explique cómo cambia el tiempo total cuando cambia el tamaño de bloque.

Cuando aumenta el tamaño del bloque las curvas se comportan de manera diferente, el tiempo secuencial disminuye mucho al principio y luego se estabiliza, mientras que el tiempo aleatorio aumenta de forma lineal y constante. Las curvas divergen mas  en los extremos y el punto de mayor divergencia inicial ocurre en el bloque de 4.0 KiB, donde el acceso secuencial es mucho más lento que el aleatorio.

---

Interprete la comparación empírico vs teórico Grafica Sencencial
Observe las curvas y responda:

¿Las curvas tienen una tendencia similar?
No tienen una tendencia similar ya que la curva teorica se mantiene practicamente plana y constante cerca de los 0.05 segundos mientras que la curva empirica empieza con un tiempo muy alto y cae drasticamente conforme el bloque crece buscando estabilizarse solo al final del grafico

¿Dónde se separan más?
Se separan mas en el tamaño de bloque mas pequeño de 4.0 kib donde la diferencia es de 0.7 segundos

¿Qué le sugiere eso sobre el modelo usado?
Que el modelo usado no trata de forma coherente la operacfion en cargas de trabajo con muchos bloques pequeños y no tiene en cuenta el sobrecosto de procesamiento del sistema operativo.

Interprete la comparación empírico vs teórico Grafica Aleatoria

¿Las curvas tienen una tendencia similar?
Si, las curvas tienen una tendencia muy similar ya que ambas son lineas rectas que suben de forma proporcional al tamaño del bloque lo que demuestra que el modelo funciona mucho mejor para predecir el comportamiento aleatorio que el secuencial

¿Donde se separan mas?
Se separan mas en el extremo derecho de la grafica con el bloque de 256.0 kib donde la separacion entre la linea azul y la naranja es mas visible indicando que a mayor cantidad de datos por salto el sistema real empieza a acumular mas retrasos que la formula ideal

¿Que le sugiere eso sobre el modelo usado?
Que el modelo subestima el tiempo real ya que la latencia elegida de 10 microsegundos se ajusta muy bien al ssd aunque la pequeña diferencia final se debe a que el sistema operativo y el hardware real tardan mas gestionando el orden de cada uno de los saltos

---

Interprete la ventaja del acceso secuencial 
La gráfica muestra cuántas veces el acceso secuencial supera al aleatorio.

¿Cuál fue el mayor factor de mejora observado?
El mayor factor de mejora fue de aproximadamente 1.35x y se observo en el tamaño de bloque de 16.0 kib

¿Cómo cambia esa ventaja con el tamaño de bloque?
la ventaja tiende a disminuir conforme el bloque se hace mas grande cayendo por debajo de 1.0x al llegar a los 256.0 kib entonces en un ssd nvme la diferente entre leer secuencial y aleatorio no es mucha.

¿Qué implicación tiene esto para el diseño de software?
Esto implica que para aplicaciones que manejan bases de datos o archivos pesados es mejor diseñar el software para que pida los datos en bloques de 64 kib o mas ya que esto minimiza la penalizacion por saltos aleatorios y permite que el disco trabaje a su maxima potencia sin importar si la informacion esta dispersa o seguida


---

Conclusión final
Redacte una conclusión de entre 8 y 12 líneas que integre los siguientes elementos. Recuerde que esta conclusión también formará parte de su informe en el README.md de entrega.

Su conclusión debe responder, en prosa continua y con sus propias palabras, las siguientes preguntas:

¿Cómo se almacena la información en disco y por qué eso importa?
¿Por qué el acceso secuencial y el aleatorio tienen desempeños tan distintos, incluso en un SSD?
¿Qué tan bien predijo el modelo teórico el comportamiento real de su equipo?
¿Qué decisión de diseño tomaría en un sistema real con base en lo que midió?
Criterio mínimo: la conclusión debe incluir al menos un valor numérico concreto de sus resultados (por ejemplo, el factor de speedup o el throughput secuencial medido) y conectarlo con uno de los conceptos teóricos del curso.

Conclusión
Escriba aquí su conclusión final.

---

11. Preguntas de cierre
Responda en el README.md de su repositorio de entrega, apoyándose en los resultados del experimento.

Comparación de patrones: Con base en sus mediciones, ¿cuántas veces más rápido fue el acceso secuencial respecto al aleatorio en su equipo? ¿Ese resultado era el esperado según la teoría?

Efecto del tamaño de bloque: ¿Qué ocurrió con el throughput del acceso aleatorio a medida que aumentó el tamaño de bloque? ¿Por qué cree que sucede eso?

Teoría vs práctica: Identifique un caso en sus resultados donde la medición empírica se alejó del modelo teórico. ¿A qué factor atribuye esa diferencia?

Tipo de disco: Compare sus resultados con los valores de referencia de la tabla de la guía. ¿Su equipo se comportó como un HDD, un SSD SATA o un SSD NVMe?

Aplicación práctica: Imagine que debe almacenar una tabla de estudiantes con 1 millón de registros. Con base en lo que midió, ¿preferiría leerla toda de forma secuencial o acceder a registros individuales de forma aleatoria? ¿Por qué?