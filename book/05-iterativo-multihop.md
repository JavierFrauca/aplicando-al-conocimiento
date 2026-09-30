---
title: 5 · Recuperar de nuevo: iterativo y multi-hop
---

# 5 · Recuperar de nuevo: iterativo y multi-hop

Una empresa de logística con operaciones en doce países mantiene un corpus con la normativa de etiquetado de mercancías de cada uno de ellos. Un martes, el responsable de cumplimiento formula la pregunta que abre este capítulo: "¿Qué países de nuestra red han cambiado requisitos de etiquetado en el último año, y qué afecta de lo que distribuimos?". Es una pregunta perfectamente legítima, perfectamente del negocio — y letal para una pasada. No existe ningún documento que la responda. La respuesta existe en el corpus, pero repartida en piezas: los cambios de cada país, las listas de mercancías que la empresa distribuye, las reglas de cruce entre ambas. Para responderla habría que recorrer los doce países, anotar los cambios, y cruzarlos contra el catálogo. Una persona tardaría una tarde. Un RAG directo tarda tres segundos y contesta cualquier cosa menos eso.

Este capítulo trata del patrón que sí puede: el **RAG iterativo**, la segunda estación del espectro. Su idea fundacional cabe en una frase que cambia todo el diseño: **recuperar no tiene por qué ser un paso previo — puede ser parte del razonamiento**. El sistema recupera, mira lo que recuperó, se da cuenta de qué le falta, y vuelve al rack con una búsqueda mejor. Y otra vez, si hace falta. La respuesta no sale de una cosecha: sale del recorrido.

## El diagnóstico: la pregunta que no cabe en una pasada

Antes de la técnica, la identificación — porque el iterativo mal aplicado es un generador de latencia y facturas. Las preguntas que lo necesitan comparten rasgos reconocibles, y conviene aprender a verlos en el texto de la consulta misma.

El primer rasgo es el **plural con cruce**: "¿qué países...", "¿qué contratos...", "¿cuáles de nuestros...". Cuando la pregunta pide una lista resultante de cruzar criterios — un conjunto filtrado por condiciones que viven en documentos distintos —, ninguna búsqueda de similitud la puede servir, porque la similitud compara la consulta con pasajes individuales, y la respuesta no está en ningún pasaje: está en la intersección.

El segundo es la **composición explícita**: la pregunta contiene o implica subpreguntas encadenadas. "¿Qué garantía aplica al equipo que instalamos en la sede de Sevilla?" contiene dos preguntas — qué equipo se instaló en Sevilla, qué garantía aplica a ese equipo — y la segunda no se puede formular hasta responder la primera, porque su texto depende de su resultado. Son los saltos — los *hops* de la jerga — y su número es medible: una pregunta de dos saltos necesita dos recuperaciones mínimas, por construcción.

El tercero es el **panorama**: las preguntas que buscan un estado del arte, una síntesis, un "¿qué dice la normativa/regulación/la bibliografía sobre X?" — donde la respuesta es la suma de muchas piezas y el riesgo dominante es dejar fuera una relevante. El directo responde panoramas con las cinco píldoras más parecidas a la pregunta, que es como contestar a "¿cómo está el mercado?" con el último periódico que se encontró.

Un matiz diagnóstico que evita malas asignaciones: el iterativo no es para preguntas *difíciles* sino para preguntas *compuestas*. Una pregunta fáctica mal escrita no necesita vueltas — necesita mejor normalización, o un reranker mejor, o simplemente corpus. Encaminar al iterativo lo que es un problema de acceso es pagar investigación por lo que debía pagar orden.

## Self-ask: la pregunta que se hace subpreguntas

La técnica canónica de la estación iterativa es el **self-ask** — pregúntate a ti mismo — y se entiende mejor recorriéndola sobre un ejemplo concreto. Toma la pregunta del almacén de repuestos: "¿Qué garantía aplica al compresor que instalamos en la nave de Zaragoza?". Un sistema con self-ask la procesa así.

Primero, **descomposición**: el sistema detecta que la pregunta encadena dos preguntas y las explicita. Subpregunta uno: "¿Qué compresor se instaló en la nave de Zaragoza?". Subpregunta dos — aún no formulable en su forma final —: "¿Qué garantía aplica a [ese compresor]?".

Segundo, **resolución en orden**: recupera para la subpregunta uno — la entrega de la nave de Zaragoza, el albarán, el modelo del compresor — y verifica que lo recuperado responde de verdad (un modelo, una referencia, una fecha). Con esa respuesta, la subpregunta dos deja de ser una plantilla y se convierte en texto real: "¿Qué garantía aplica al compresor X Serie 400 instalado en marzo?".

Tercero, **recuperación para la subpregunta dos** — ahora sí, con una consulta precisa que el motor puede emparejar con la píldora de garantías del fabricante — y verificación de nuevo.

Cuarto, **síntesis**: la respuesta final encadena lo aprendido, con la trazabilidad de ambas recuperaciones: "Según el albarán de entrega (ref), en la nave de Zaragoza se instaló el compresor X; según las condiciones del fabricante (ref), ese modelo lleva 36 meses de garantía desde la puesta en marcha, que fue en marzo".

El detalle que hace funcionar la técnica no es la descomposición — eso lo hace cualquier planificador — sino la **verificación por subpregunta**: cada resultado intermedio se comprueba como respuesta completa antes de usarse como material de la siguiente. El self-ask ingenuo encadena sin verificar, y entonces una subpregunta mal resuelta contamina todas las siguientes. Con verificación, el fallo se localiza en el eslabón que falló — y eso, en producción, vale lo que cuesta la verificación.

## FLARE: redactar mientras se recupera

La segunda técnica de la estación invierte el orden natural. **FLARE** — en la jerga, *forward-looking active retrieval*, aunque basta con su nombre corto — empieza por la redacción: el sistema comienza a escribir la respuesta con lo que sabe, y cuando su propio borrador entra en territorio desconocido, se detiene, recupera para la frase que no puede respaldar, y continúa.

El ejemplo más claro llega de la redacción técnica. Un sistema de documentación de producto redacta la nota de actualización de una API: "La versión 4 incorpora autenticación por tokens temporales. Los tokens expiran a los..." — y ahí el borrador toca el borde del conocimiento: la vigencia exacta no estaba en la cosecha inicial. FLARE detecta la frase sin respaldo, recupera "vigencia tokens autenticación versión 4", consigue la píldora — "los tokens expiran a los 15 minutos, renovables" — y el borrador continúa. La recuperación ocurre *donde el texto la necesita*, no donde el pipeline la tenía prevista.

La ventaja sobre el self-ask es de eficiencia: solo se recupera lo que el texto demanda, y cada recuperación llega con la consulta más precisa posible — formulada por la propia frase que la motivó. El coste es de ingeniería: exige comprobar el borrador frase a frase contra el contexto, lo que añade evaluaciones. En la práctica, ambas técnicas conviven: el self-ask para preguntas de cruce con estructura clara, FLARE para redacciones largas donde los huecos aparecen sobre la marcha.

## El recursivo: descomponer, resolver, sintetizar

La tercera variante ataca el caso masivo del panorama con estructura: cuando la pregunta pide lo mismo sobre muchas unidades — países, productos, centros, contratos — y la respuesta es la agregación. El patrón **recursivo** divide por unidad, resuelve cada subconsulta con una pasada normal — directo, de nuevo, pero orquestado — y sintetiza los resultados en una respuesta única con su tabla o su resumen.

Sobre el ejemplo de apertura: doce subconsultas — "cambios de etiquetado en [país] último año" —, doce cosechas verificadas, un cruce contra el catálogo de mercancías, y una síntesis final: "Tres países han cambiado requisitos; afecta a estas dos líneas de producto; estos son los documentos". Cada subconsulta es trivial; el valor está en la orquestación y en la síntesis, que conserva la trazabilidad por unidad.

El recursivo es también el patrón más fácil de descarrilar, y su descarrilamiento tiene nombre: la **deriva**. En la síntesis de muchas piezas, el modelo puede empezar a promediar, a suavizar discrepancias, a presentar como homogéneo lo que no lo es. La regla que la evita es la que el capítulo catorce formalizará: las discrepancias entre piezas se explican y se citan, nunca se promedian. La síntesis que borra una discrepancia no es una síntesis: es una pátina.

## Lo que el iterativo le pide al acceso

Una relación fina que conviene dejar explícita, porque el iterativo la tensa como ningún otro patrón: cada vuelta que da es una consulta al acceso, y el rendimiento del bucle depende del rendimiento de cada golpe. Un iterativo sobre un acceso mediocre itera en falso — recupera mal dos veces en lugar de una —, y su presupuesto de vueltas se consume en reconstruir lo que un buen tamiz habría dado a la primera.

De ahí tres exigencias concretas que el patrón hace al segundo libro. La primera: **consultas bien formadas** — las subpreguntas que el sistema genera deben respetar el vocabulario del acceso (sinónimos del dominio, ortografía, jerga del corpus), porque una subpregunta mal escrita desperdicia la vuelta entera. La segunda: **el tope por consulta, no por sesión** — cada vuelta recupera con su propio top-k, y el presupuesto total de píldoras del contexto se gestiona agregando vueltas: cinco píldoras por vuelta y tres vueltas no son quince píldoras aprovechables, sino un contexto con solapes que hay que podar. La tercera: **trazabilidad por vuelta** — la respuesta final debe citar sabiendo de qué vuelta salió cada pieza, porque el usuario que comprueba va a preguntar por la segunda búsqueda, no por la primera.

El iterativo, bien entendido, no sustituye al acceso: lo estresa. Y esa es una de sus funciones menos anunciadas — las familias que iteran son las que mejor diagnostican el acceso, porque repiten la consulta donde el tamiz falla.

## El precio y el freno: presupuesto de vueltas y señal de parada

Todo el capítulo hasta aquí ha vendido capacidad; llega la hora de la factura. Cada vuelta del iterativo es una recuperación — barata — y una evaluación del resultado — una llamada al modelo, no tan barata —. El self-ask de dos saltos son cuatro o cinco llamadas; FLARE añade evaluación por frase; el recursivo multiplica por el número de unidades. La latencia sube en la misma proporción: donde el directo firma tres segundos, el iterativo firma veinte o cuarenta. Es el precio, y es justo — siempre que lo pague la pregunta adecuada.

Lo que no es justo es el **bucle**: el sistema que vuelve al rack indefinidamente porque nunca está satisfecho, o porque cada recuperación abre una puerta nueva. El freno del iterativo son dos piezas de diseño que ningún sistema de esta estación puede omitir, y conviene definirlas con precisión porque son criterio de salida de este capítulo.

El **presupuesto de vueltas** es un máximo duro — tres, cinco, lo que el contrato de latencia permita — que ninguna resolución intermedia puede superar. No es un parámetro de tuning: es una cláusula del contrato, traducida a arquitectura. El usuario que acepta veinte segundos no ha aceptado dos minutos.

La **señal de parada** es el criterio que dice al sistema "ya sabes lo suficiente, redacta". Sus formas honestas son pocas y verificables: todas las subpreguntas de la descomposición tienen respuesta respaldada; la última recuperación no aportó piezas nuevas por encima de un umbral; el borrador no tiene frases sin respaldo. Sus formas deshonestas — y abundan en sistemas mal diseñados — son las cosméticas: "ha pasado el tiempo suficiente", "ha recuperado algo en cada vuelta". La señal de parada mal puesta es la diferencia entre un iterativo y un generador de facturas con bonus de latencia.

## Cuándo sirve, y para quién

La asignación del iterativo, como la de todos los patrones, la firma el contrato de respuesta más que la tecnología. Su audiencia natural es la del **panorama y el cruce**: el responsable de cumplimiento, el analista, el auditor interno, el redactor técnico — usuarios que preguntan en plural, que aceptan decenas de segundos, que valoran la exhaustividad y que leerán las referencias. Su anti-audiencia es la del dato puntual urgente, que ya tiene su patrón en el capítulo anterior.

Y una observación de arquitectura que cierra el círculo con el segundo libro: muchas preguntas que hoy parecen necesitar iteración son en realidad señal de que el corpus o el acceso pueden mejorar. Si las preguntas de cruce son frecuentes — si cada semana alguien cruza países y etiquetados —, quizá la respuesta correcta no es iterar en la consulta sino construir el documento de síntesis que el primer libro llamaba Gold: la vista agregada, mantenida, que convierte una pregunta compuesta en una pasada directa. El iterativo puede ser una solución permanente o un andamio temporal mientras el corpus madura; decidir cuál de las dos es, en cada caso, es exactamente el tipo de decisión que distingue la arquitectura del entusiasmo.

## Preguntas que hace el oficio

**¿El iterativo no es simplemente "un directo que reintenta cuando falla"?** Es la diferencia entre un fallo y un plan. El reintento reactivo — "no encontré nada, pruebo otra vez" — es gestión de error: misma estrategia, segunda tirada. El iterativo es estrategia: cada vuelta está motivada por lo aprendido en la anterior, y la consulta nueva se formula con esa información. Un reintento repite; una iteración avanza. Si la segunda búsqueda no sabe nada que la primera no supiera, no hay iteración: hay insistencia.

**¿Cuántas vueltas es "normal"?** Las que el plan necesita, con techo. En sistemas reales bien encaminados, la mayoría de las consultas iterativas resuelve en dos vueltas, las complejas en tres o cuatro, y el techo del presupuesto casi nunca se toca. Un sistema cuya distribución dijera "todas las consultas usan todas las vueltas" no está iterando: está bucleando — y su presupuesto, o su señal de parada, o su familia asignada están mal puestos.

**¿Puedo combinarlo con el autocorrectivo?** Se combinan por diseño — el iterativo decide *qué* recuperar después; el autocorrectivo decide *si* lo recuperado sirve. La arquitectura corriente los encadena: cosecha → evaluación → si insuficiente, nueva vuelta informada → redacción. Es más caro que cada uno solo, y su sitio es la familia que lo justifica: los panoramas de alta consecuencia.

**¿El usuario nota las vueltas?** En la latencia, sí; y por eso el progreso legible importa más aquí que en ningún otro patrón: "consultando la normativa de tres territorios" convierte las vueltas en evidencia de trabajo. Ocultarlas produce la peor experiencia posible del espectro: espera larga sin explicación.

**¿Las subpreguntas que el sistema se hace pueden verse y corregirse?** Deben verse — es parte de la trazabilidad del patrón: cada subpregunta generada, su respuesta y su respaldo quedan registrados, y el triaje las revisa como revisa los encaminamientos. Es también la palanca de mejora más barata del iterativo: una subpregunta mal formulada se corrige en la plantilla de descomposición, y todas las consultas de la familia mejoran a la vez. El bucle que no enseña su razonamiento intermedio no se puede depurar; el que lo enseña, se afina semana a semana.

!!! note "Criterio de salida"
    La política de iteración por escrito: qué familias de consulta se asignan a este patrón (con su evidencia de composición — cruces, plurales, panoramas), el presupuesto de vueltas por contrato de latencia, la señal de parada verificable que corta la iteración, y la regla de síntesis que prohíbe promediar discrepancias. Un iterativo sin presupuesto ni señal de parada no es un patrón: es un bucle con fecha de factura.

Segunda estación recorrida. La tercera añade lo que el directo no tiene y el iterativo solo mejora a medias: la segunda mirada — no sobre *qué más* recuperar, sino sobre si lo recuperado sirve. La réplica interna.
