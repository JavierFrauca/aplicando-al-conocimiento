---
title: 10 · El precio de pensar
---

# 10 · El precio de pensar

La reunión que da pie a este capítulo es reconocible en cualquier organización que haya estrenado un patrón sofisticado sin contabilidad. Una distribuidora mayorista estrenó en septiembre un piloto agéntico para las consultas de sus comerciales — propuesta entusiasta, demo brillante, patrocinio de dirección — y en noviembre la factura de inferencia del mes llegó multiplicada por cuatro. Nadie había robado nada. Lo que ocurrió es aritmético: cada consulta que antes costaba un contexto y una redacción pasaba a costar un plan de ocho pasos con contexto creciente, cuatro llamadas a herramientas y una verificación. Multiplicado por el volumen de la cola — que nadie midió porque el piloto "era solo un piloto" —, el resultado salió a finales de mes, como salen todas las sorpresas presupuestarias: ya consumido el dinero.

La discusión siguiente es la que este libro quiere provocar, y no fue sobre tecnología. Fue: "¿cuántas de nuestras consultas merecían ese precio?". La respuesta — la mayoría, ninguna; una minoría valiosa, todas — es el punto de partida de la parte tercera del libro. Los patrones del espectro se eligen con la matriz del capítulo siguiente, pero la matriz solo funciona si antes se sabe **cuánto cuesta cada patrón**. Este capítulo es esa contabilidad.

## Primera moneda: la latencia

El precio más visible del pensamiento es el tiempo, y conviene medirlo con la disciplina que el segundo libro aplicó a la recuperación: en percentiles, nunca en medias.

La media engaña porque las colas de latencia no son simétricas: el directo responde casi siempre en dos segundos, pero su percentil noventa y cinco — el caso con el corpus cargado o el modelo con ráfagas — puede estar en seis. El agente, en cambio, tiene una distribución doble: gran parte de sus ejecuciones termina en dos minutos, pero una fracción — las que encadenan pasos extra, las que esperan una herramienta lenta — se dispara a diez. Promediar esas dos colas produce un número que no describe a nadie: el usuario no experimenta la media, experimenta su consulta. Por eso los contratos de latencia del capítulo tres se escriben sobre el percentil noventa y cinco, y por eso la contabilidad de este capítulo también.

El orden de magnitud por patrón, con valores de referencia de sistemas reales — no promesas de marketing: el directo vive en segundos bajos, el iterativo en decenas de segundos, el autocorrectivo entre ambos con una cola más gruesa por sus reintentos, el agente en minutos, el multi-agente en minutos largos. Son escalas distintas, no matices: un sistema que mezcla patrones sin encaminar mezcla usuarios que esperan tres segundos con usuarios que esperan tres minutos, y ambos juzgarán al sistema por el otro.

## Segunda moneda: el coste por consulta

La segunda moneda es el dinero, y su aritmética tiene una propiedad que sorprende a quien no la ha contado: **en los patrones que piensan, el coste crece más rápido que la latencia**. Cada paso de un agente reenvía al modelo todo el historial de la investigación — el plan, las observaciones acumuladas —, de modo que el paso diez procesa mucho más contexto que el paso dos. El resultado es una curva: la investigación no cuesta diez veces una pasada, cuesta veinte o cuarenta, según cuánto se haya acumulado en el camino.

A esa curva se suma la de los tokens de salida, los de las herramientas que también consumen, y la de la réplica interna — que es una llamada de evaluación por cosecha —. El inventario de coste por patrón, en múltiplos del directo tomados como uno: el directo es 1; el iterativo de dos o tres vueltas, entre 3 y 6; el autocorrectivo con evaluación permanente, entre 2 y 4 — su evaluador es corto, y ahí su gracia —; el agente, entre 15 y 40; el multi-agente, múltiplo del agente por el número de piezas y rondas. Son órdenes, no tarifas — cada sistema debe medir los suyos —, pero el orden de magnitud ya instruye: **cada estación del espectro no es un upgrade, es un salto de presupuesto**.

La consecuencia de gobernanza es directa y será la columna de la matriz del capítulo siguiente: el coste por consulta es el único precio del pensamiento que el negocio puede *autorizar*. La latencia la siente el usuario, la superficie de fallo la sufre el equipo, pero el coste lo firma quien paga — y por eso la contabilidad se escribe y se aprueba, patrón a patrón, antes de estrenar nada.

## Tercera moneda: la complejidad de operación

La tercera moneda no aparece en la factura y es a menudo la más cara: lo que cuesta **mantener el sistema entendible**. Cada patrón añade piezas que alguien debe observar, depurar, explicar y enseñar a los nuevos.

El directo se opera con lo que el segundo libro ya instaló: observabilidad de la consulta, dataset de regresión, diario de consultas. El iterativo añade la política de vueltas y su supervisión — cuántas vueltas come cada familia, dónde se cortan. El autocorrectivo añade la calibración continua del evaluador — su tasa de rechazo medida y vigilada. El agente añade otra liga: trazabilidad persistida de cada ejecución, salud de cada herramienta del catálogo, presupuestos de pasos que hay que revisar cuando el negocio cambia. El multi-agente, todo lo anterior por especialista, más la salud de la coordinación: rondas de devolución, traspasos, verificador.

Una regla práctica que las organizaciones aprenden tarde: el coste de operación escala con el número de *decisiones en vivo* que el sistema toma sin humano delante. El directo decide poco — todo lo dejó decidido el diseño. El agente decide en cada paso. Cada decisión en vivo es una superficie que hay que poder inspeccionar después, y esa capacidad de inspección — su almacenamiento, su legibilidad, su coste de consulta — es infraestructura, se pague o no en la factura de inferencia.

## Cuarta moneda: la superficie de fallo

La última moneda es el riesgo, y se contabiliza como el resto. Cada transformación intermedia — cada recuperación extra, cada evaluación, cada herramienta, cada traspaso — es un punto donde el software puede fallar o el razonamiento puede desviarse. La diferencia entre fallo de software y desvío de razonamiento importa en la contabilidad: el primero es visible y se arregla; el segundo es silencioso y se propaga.

El directo falla de una vez y de una pieza: si la cosecha es mala, la respuesta es mala, y el diario de consultas la enseña. El iterativo falla en las juntas: una subpregunta mal resuelta contamina a las siguientes — el fallo en cadena del capítulo cinco. El agente sistematiza esa cadena: la cascada del capítulo siete es su firma. El multi-agente añade las costuras: traspasos que evaporan matices, piezas que se contradicen. Y el autocorrectivo, curiosamente, es la única estación cuya misión es *reducir* la superficie efectiva — compra fallos baratos y visibles (un reintento, una abstención) para evitar fallos caros e invisibles (la respuesta errónea). Por eso su contabilidad se lee al revés: lo que parece gasto es, a veces, el mejor seguro de la casa.

## La tabla del precio

Reunidas las cuatro monedas, el cuadro del precio queda así — con valores cualitativos que cada equipo debe sustituir por los suyos medidos:

| Patrón | Latencia (p95) | Coste por consulta | Operación | Superficie de fallo |
|---|---|---|---|---|
| Directo (one-shot) | Segundos bajos | 1× | Mínima — la del acceso | Baja — fallo de una pieza |
| Iterativo / multi-hop | Decenas de segundos | 3–6× | Política de vueltas y parada | Media — falla en las juntas |
| Autocorrectivo | Segundos altos / cola gruesa | 2–4× | Calibración del evaluador | Media-baja — compra fallos visibles |
| Agéntico | Minutos | 15–40× | Trazabilidad por ejecución, salud de herramientas | Alta — cascada paso a paso |
| Multi-agente | Minutos largos | Múltiplo del agente | Todo lo anterior × especialistas | Alta — cascada más costuras |
| GraphRAG (transversal) | Según el patrón que lo consulte | Recorrido bajo, mantenimiento alto | Ciclo de vida del grafo | Media — arista muerta = mentira elegante |

Dos lecturas que la tabla provoca y convienen decir. Primera: el GraphRAG aparece marcado como transversal porque su precio dominante no es el de consulta sino el de mantenimiento — el capítulo nueve lo dijo y la tabla lo confirma. Segunda: ninguna columna se puede leer sola. El patrón que parece caro por coste puede ser barato por superficie de fallo; el barato por coste puede ser caro por operación. El precio completo es la fila entera, y la fila entera se compara contra el valor de la respuesta para la familia de consultas que la paga — que es exactamente el trabajo del capítulo once.

## Las monedas contadas: un mes entero, en una página

La contabilidad se entiende mejor sobre un caso corrido de principio a fin. Una editorial técnica con su asistente interno para autores y correctoras lo hizo, y su página de números — con las cifras alteradas, el método intacto — es el ejemplo que este capítulo recomienda imitar.

Su tráfico mensual: 18.400 consultas. La matriz les reconocía cuatro familias. La de consultas fácticas de estilo — citas, formatos, normas — era el 71 por ciento del volumen: directo, dos segundos de p95, coste por consulta tomado como unidad. La de panoramas — "¿qué tratamiento dan las obras de referencia a X?" — era el 19 por ciento: iterativo, 24 segundos de p95, cinco veces la unidad. La de expedientes — derechos, liquidaciones, cruces con el sistema de royalties — era el 9 por ciento: agéntico, 3 minutos de p95, treinta y dos veces la unidad. Y una familia residual del 1 por ciento, el autocorrectivo para las consultas contractuales.

El total, contundente a primera vista: el directo consumía el 71 por ciento de las consultas y apenas el 16 por ciento del coste; el agente, el 9 por ciento de las consultas y el 63 por ciento — dos tercios de la factura para una décima parte de la cola. La lectura correcta no fue "el agente es caro" sino la inversa y más útil: **el coste estaba exactamente donde la consecuencia estaba** — los expedientes, que mueven pagos a autores, eran las consultas más caras y las mejor pagadas. Y el experimento que la página habilitó: ajustar el presupuesto de vueltas del iterativo bajó su p95 seis segundos y recortó el coste mensual total un 8 por ciento — más que cualquier optimización del agente — porque la familia grande media, no la pequeña.

Esa es la disciplina que este capítulo deja: el precio no se adivina en el patrón, se contabiliza en la **combinación patrón × volumen × consecuencia** — y casi siempre revela que el coste está donde el negocio ya sabía que estaba la consecuencia, solo que ahora se puede gestionar.

## La latencia percibida: el precio que se siente

Falta una pieza de la contabilidad que no sale en ningún panel: cómo se vive la espera. La **latencia percibida** — presentada en el capítulo uno como juicio del usuario — es también una moneda de cambio: se paga o se cobra según cómo el sistema gestione la espera.

Tres técnicas, en orden de honestidad. La primera es el **streaming**: servir la respuesta mientras se redacta, en lugar de al final. Convierte veinte segundos de silencio en veinte segundos de lectura progresiva, y cambia la experiencia sin cambiar el sistema. Su advertencia: en los patrones que piensan, el texto que llega al final puede corregir al del principio — streamer un agente es servir un borrador que se arrepiente; se hace, pero declarando su naturaleza.

La segunda es el **progreso legible**: decir qué está pasando — "consultando la normativa de tres territorios" — con el vocabulario del dominio, no barras abstractas. El progreso legible convierte la espera en evidencia de trabajo, y tiene un efecto secundario valioso: enseña al usuario qué preguntas son de investigación, y con eso educa el uso del sistema.

La tercera es la **expectativa previa**: el contrato del capítulo tres, mostrado en el momento justo. El portal que al recibir una pregunta compuesta responde "esto requiere cruzar varias fuentes: tardaré un minuto" está cumpliendo la cláusula de latencia — la percibida, gestionada con honestidad — y, de paso, encaminando al patrón correcto. El silencio, en cambio, es la única espera que siempre se cobra cara.

## La regla del margen

Todo el capítulo se condensa en una regla que la matriz del capítulo siguiente formalizará y el router del doce ejecutará: **el patrón caro solo se paga donde la pregunta lo exige — y la exigencia se demuestra, no se supone**. La demostración son las cuatro monedas medidas en el sistema propio: el cuadro del precio rellenado con números reales, aprobado por quien paga, revisado cuando el negocio cambia.

Con esa contabilidad, la conversación de la distribuidora cambia de tono. Ya no es "¿quitamos el agente?" ni "¿lo ponemos en todo?": es "¿qué familias de consultas del comercial pagan minutos y cuarenta veces el coste de una pasada?". Las que sí — las propuestas grandes, los expedientes de riesgo — lo siguen pagando con gusto. Las que no vuelven al directo, y nadie echa de menos lo que nunca necesitó. Esa redistribución — no la tecnología — fue la que devolvió la factura a su sitio el trimestre siguiente.

## Preguntas que hace el oficio

**¿Estos números durarán? Los modelos cambian cada trimestre.** Los órdenes de magnitud, sí; las tarifas, no. El directo será siempre un orden por debajo del iterativo, y este por debajo del agente, porque las diferencias vienen de la estructura — cuántas llamadas, cuánto contexto — y no del precio unitario del token. Cuando los modelos abaratan, todos los patrones bajan juntos y la escalera se conserva; por eso la contabilidad se repite cada trimestre, pero la escalera no se reordena.

**¿No se soluciona el coste del agente con modelos más baratos o cachés?** Ayuda, y se usa — caché de respuestas por familia, modelos pequeños en los pasos de clasificación, grandes solo en la síntesis —. Pero ninguna optimización cambia la asimetría estructural: el agente hace muchas llamadas con contexto creciente y el directo hace una. Optimizar un patrón caro lo hace menos caro; no lo convierte en otro patrón. La matriz del capítulo siguiente sigue siendo la herramienta correcta — con números mejores.

**¿La latencia no se soluciona con paralelismo?** Parte: los especialistas del multi-agente corren en paralelo, y las búsquedas independientes de una iteración también. Pero los pasos que dependen de resultados anteriores — la esencia de la investigación — son secuenciales por naturaleza: no se puede calcular antes de recuperar, ni sintetizar antes de calcular. El paralelismo recorta; no aplana la curva.

**¿Quién aprueba el presupuesto, en la práctica?** El mismo rol que firma los contratos de respuesta: el negocio que paga la factura y asume la consecuencia del error. La contabilidad de este capítulo es el documento que traduce entre ambos lenguajes — lo que cuesta pensar, dicho en el idioma del presupuesto.

!!! note "Criterio de salida"
    La tabla del precio rellenada con los valores medidos del sistema propio — latencia en percentiles, coste por consulta en múltiplos del directo, coste de operación estimado y superficie de fallo por patrón — y un presupuesto por consulta aprobado por el negocio para cada patrón en uso. Sin esta contabilidad, la matriz del capítulo siguiente reparte entusiasmos; con ella, reparte dinero y tiempo donde valen.

Con el precio de cada estación sobre la mesa, la parte tercera puede hacer su trabajo: decidir qué pregunta merece qué patrón. La herramienta es una matriz — audiencia, alcance, consecuencia, urgencia — y el capítulo siguiente la construye.
