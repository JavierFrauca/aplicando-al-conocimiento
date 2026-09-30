---
title: 14 · Fidelidad: la respuesta no miente
---

# 14 · Fidelidad: la respuesta no miente

La respuesta que abre este capítulo salió un martes por la mañana desde el asistente técnico de un fabricante de maquinaria de laboratorio, y era, en apariencia, de las buenas. Pregunta de un técnico de mantenimiento: "¿Qué torque de apriete lleva el cabezal de la serie 400?". La respuesta llegó en tres segundos, con estructura limpia: el valor, la nota de calibración, y una referencia — "según el manual de servicio de la serie 400, sección 7.3". El técnico fue a la sección 7.3 y encontró el capítulo de limpieza. El torque no estaba allí — estaba en la sección 9.1 — y la nota de calibración, resulta, pertenecía a otro modelo. El dato era casi correcto; la cita era una decoración.

El incidente no se descubrió por el panel — no hay métrica que huela a cita falsa —. Se descubrió porque el técnico de turno era de los que comprueban. Esa es la estadística incómoda que motiva el capítulo: los fallos de fidelidad solo los detecta quien verifica, y el sistema no puede asumir que sus usuarios verificarán — precisamente porque el contrato del sistema es que **no hace falta**. Si el usuario tiene que comprobarlo todo, el sistema no ha cambiado nada: es un buscador con mejor redacción.

Este capítulo construye el mecanismo que evita apelar a la suerte del técnico: la **verificación de la respuesta** — comprobar, después de redactar, que lo escrito está respaldado por el contexto y que las citas dicen lo que se dice que dicen. Es la mitad de la ética de la serie que faltaba ejecutar: *que la respuesta diga la verdad del corpus, y que se pueda comprobar* — el primer libro garantizó la verdad al construir, el segundo al recuperar, este lo garantiza al comprobar.

## La alucinación con contexto: el enemigo específico

Conviene nombrar bien al enemigo, porque el término de mercado — *alucinación* — mezcla dos fenómenos distintos que se combaten distinto. El primero es la invención desde cero: preguntar sin contexto y recibir un tejido convincente. Ese fenómeno es ruidoso, famoso y — la buena noticia — casi irrelevante en un RAG bien construido: con material delante, el modelo rara vez inventa de la nada.

El segundo fenómeno es el de este capítulo, y es más peligroso precisamente porque parece benigno: la **alucinación con contexto** — las deformaciones que el modelo introduce *sobre* material verdadero. Su taxonomía es corta y todo el que opere un sistema la reconocerá.

Está el **añadido**: la frase que completa lo que el contexto dejaba a medias — el plazo que el documento no especificaba y la respuesta sí; el inciso técnico que nadie escribió. Está el **redondeo**: la cifra exacta convertida en aproximada cómoda — el "2,5 mg/kg" que se volvió "unos 3", el caso del hospital del capítulo uno. Está la **extrapolación**: la regla general aplicada al caso particular que el documento no contemplaba — "ampliable en casos complejos" convertido en "ampliable hasta 30 días". Está la **mezcla**: dos versiones del procedimiento fundidas en una que no existe. Y la más traicionera de todas, la **cita decorativa**: la referencia plausible que no corresponde — la sección 7.3 —, que le presta al añadido la autoridad de la comprobabilidad sin tenerla.

La característica común de las cinco es que ninguna da señal por sí misma. La respuesta deformada es coherente, bien construida y de tono fiable — de hecho, más fiable de tono que la verdad fragmentaria del corpus. Por eso la fidelidad no se pide al modelo en la redacción y se da por cumplida: se **verifica** como se verifica cualquier otra pieza de esta serie — con criterio, con mecanismo y con métrica.

## La primera vara: la fidelidad frase a frase

El primer control compara la respuesta con su contexto, frase a frase, y responde una pregunta binaria por cada una: **¿esta afirmación está respaldada por algún bloque del contexto?** Esa comprobación — la literatura la llama *groundedness*, fidelidad al terreno — es la que caza los añadidos, los redondeos y las extrapolaciones: cada frase sin respaldo es una candidata a deformación, y el conjunto de frases sin respaldo mide el grado de invención del sistema.

El mecanismo es un evaluador — un modelo con una rúbrica, como el juez de cosecha del capítulo seis, con otro domicilio — que recibe el contexto y la respuesta y marca las frases. Su salida útil no es un número global — "fidelidad 0,87" no le dice nada a nadie — sino el inventario: qué frases quedaron sin respaldo, y qué se hace con ellas. Porque el control sin consecuencia es decoración, y aquí las consecuencias son dos, por gravedad: la frase sin respaldo **eliminada** — la respuesta sale sin ella, más corta y más honesta — o la frase **señalada** — la respuesta sale, pero marcando la parte no respaldada como tal, "no confirmado en el corpus". Ambas son legítimas; la elección es del contrato de la audiencia, como todo en este libro.

Y una precisión de método que evita el falso trabajo: la fidelidad frase a frase **no juzga si la frase es cierta en el mundo** — juzga si está respaldada *en el contexto entregado*. Esa reducción es su fuerza: la convierte en mecánica, auditable y barata. La verdad en el mundo la garantiza el corpus — el trabajo de los dos primeros libros —; la correspondencia entre respuesta y corpus, este control. Cada vara en su terreno.

## La segunda vara: la verificación de citas

El segundo control es más mecánico todavía, y por eso más barato y más sistemático: **toda cita de la respuesta debe existir y decir lo que se dice que dice**. Dos comprobaciones. La primera, puramente programática: el identificador citado existe en el contexto entregado — caza la cita decorativa, que invoca documentos que nunca se entregaron. La segunda, un juicio semántico barato — un modelo pequeño con rúbrica, no una comparación literal de texto: el fragmento citado contiene lo afirmado — caza la cita equivocada, la del manual que cita la sección 7.3 para un dato que vive en la 9.1.

Es un control humilde que produce un efecto desproporcionado, por una razón estructural: la cita es la promesa de comprobabilidad del sistema entero. Una respuesta sin citas puede ser verificada a mano con esfuerzo; una respuesta con cita falsa **rompe el contrato** en su cláusula más visible, y lo peor: lo hace en la audiencia que más confía — la que creyó la cita y no comprobó. Por eso la verificación de citas es el control de cobertura universal: corre sobre toda respuesta que declare citas, en todas las audiencias, sin muestreo. Es barata — un chequeo mecánico y un juicio de sí/no, no una redacción entera — y su falta es cara.

## La tercera vara: las contradicciones, explicadas y no promediadas

El tercer control no busca frases sueltas sino relaciones entre ellas: **las contradicciones del contexto que llegan a la respuesta sin declararse**. El corpus — vivo, versionado, con historia — contiene discrepancias legítimas: versiones de vigencias distintas, criterios de departamentos distintos, casos generales y excepciones. El redactor bien instruido — el capítulo trece — las expone. El mal instruido las resuelve en silencio: promedia, elige la primera, funde. El control comprueba que cada discrepancia presente en el contexto aparece en la respuesta como discrepancia — con sus dos versiones y sus citas —, y que ninguna se ha "resuelto" sin fundamento.

Es la vara que protege la regla de oro de la síntesis enunciada en el capítulo cinco y que este capítulo ejecuta en la capa final: **las discrepancias se explican y se citan, nunca se promedian**. El usuario ante una discrepancia declarada sabe que debe mirar vigencias — es información —; el usuario ante una respuesta unificadora cree haber recibido la verdad — es trampa.

## El coste de verificar y su cobertura

La objeción honesta a todo el capítulo es la factura: cada control es cómputo, y aplicarlo entero a cada respuesta de cada audiencia sería — en volumen — un segundo sistema de coste. La respuesta de arquitectura es la misma que dio el libro al escalado de patrones: **la cobertura de la verificación se gradúa por la consecuencia del error** — el tercer eje de la matriz, otra vez al servicio del presupuesto.

La escala de cobertura se gradúa por peldaños, y debajo de todos corre un suelo común. El suelo es la **verificación de citas universal**: barata, siempre, en todas las audiencias, porque la cita falsa rompe el contrato dondequiera que aparezca. Encima, el peldaño alto es la **verificación completa y obligatoria** para la audiencia de alta consecuencia: fidelidad frase a frase, verificación de citas, control de contradicciones — todo, cada respuesta, porque aquí el error no tiene precio de corrección aceptable. Y el peldaño bajo es el **muestreo** para la cola de baja consecuencia: un porcentaje de respuestas verificadas a fondo, no para corregir aquella respuesta concreta sino para medir la salud del conjunto — la métrica de fidelidad en tendencia, la alerta cuando la proporción de frases sin respaldo sube.

Con esa arquitectura, la vara deja de ser un lujo: es una asignación de presupuesto como cualquier otra del libro, con su justificación por fila de matriz y su métrica de retorno — frases de invención interceptadas por euro de verificación, que en los sectores de consecuencia alta es la mejor inversión del sistema.

## Los falsos acusados: cuando la vara se equivoca

Un capítulo sobre verificación debe incluir su propia autocrítica, porque la vara también falla — y su fallo característico es el **falso acusado**: la frase correcta que el evaluador marca como sin respaldo. Ocurre por tres mecanismos, y conocerlos evita tanto el daño como el desprestigio de la vara.

El primero es la **paráfrasis lejana**: la respuesta dice "el plazo se suspende durante la negociación" y la píldora decía "el cómputo queda interrumpido mientras se sustancie el procedimiento conciliatorio". Misma verdad, vocabulario distinto — y un evaluador literal la marca. El antídoto está en la rúbrica: la fidelidad no es igualdad textual, es respaldo semántico; se instruye al evaluador con ejemplos de paráfrasis legítimas. El segundo es la **inferencia trivial**: la respuesta une dos frases del contexto que juntas respaldan lo dicho, pero ninguna por separado lo hace completa — el evaluador frase a frase, ciego a la combinación, acusa. El antídoto es dejar al evaluador ver la frase con su vecindad, no aislada. El tercero es el **formato**: tablas, cifras con unidades distintas, fechas normalizadas — el contexto dice "15 min" y la respuesta "quince minutos"; la igualdad es real, la comparación superficial no la ve.

La consecuencia práctica de los tres es la misma: **ningún hallazgo de la vara es condena automática**. Las frases señaladas pasan por revisión — automática con segunda pasada, humana en la muestra de alta consecuencia — antes de eliminar o marcar. La vara que elimina frases correctas envenena la respuesta por prudencia, y acaba siendo más cara que el mal que perseguía. Verificar, como todo en esta serie, es una disciplina con calibración — no un interruptor.

## Lo que la vara no puede

Y el límite, dicho sin rodeos porque esta serie nunca disfraza: la verificación garantiza la correspondencia entre respuesta y corpus — no la verdad del corpus mismo. Si el corpus entero está equivocado — si el manual de servicio del fabricante tiene el torque mal escrito desde la edición de 2021 —, la respuesta será fiel, citada, verificada... y equivocada. Contra eso no hay vara en esta capa: hay gobierno del corpus, actualización de fuentes, el bucle de retorno que el capítulo diecisiete completa. La fidelidad es el último eslabón de la cadena de confianza, y como todo eslabón, solo sostiene lo que los anteriores le entregan.

Dicho esto, lo que queda en manos del sistema es enorme: la garantía de que **cada palabra de la respuesta tiene respaldo y toda cita dice lo que dice**. Es la diferencia entre un instrumento y un oráculo — y los instrumentos, que son comprobables, son los únicos que las instituciones serias pueden poner en producción.

## Preguntas que hace el oficio

**¿No basta con pedirle al modelo que "no invente"?** Se pide — es el contrato de generación del capítulo trece —, pero pedir no es verificar. El modelo entrenado para agradar cumple la instrucción hasta que el caso la tensa: el hueco a medias, la cifra que redondea sola, la cita plausible. La instrucción baja la frecuencia; la verificación pone el número. Los sistemas serios hacen las dos cosas y miden la distancia entre ellas.

**¿Verificar cada respuesta no duplica el coste del sistema?** Con la cobertura graduada, no: la verificación de citas es mecánica y barata — corre en todo; la fidelidad frase a frase corre completa solo donde la consecuencia la paga, y por muestreo en el resto. El reparto del capítulo — completo arriba, universal en citas, muestras abajo — mantiene la vara en una fracción del coste de la redacción.

**¿Qué hago con la primera medición, si sale mal?** Celebrarla y triarjarla: cada frase sin respaldo es un hallazgo con tres destinos posibles — la plantilla (instrucción que faltaba), la cosecha (píldora que faltaba o sobraba) o la verificación (paráfrasis mal calibrada). La primera medición de fidelidad no es un examen que se suspende: es el diagnóstico que faltaba.

**¿La fidelidad del 100 % es el objetivo?** No — es la señal de que la vara está ablandada. Un sistema vivo con corpus vivo tiene muestreos con hallazgos; la tendencia estable con incidencias raras y curadas es la salud. El cero absoluto se consigue de dos maneras: con perfección — improbable — o con una vara que no mira — frecuente.

**¿La fidelidad se mide también de las respuestas de los patrones que investigan?** Sí — y con un matiz: en los patrones con pasos intermedios, la vara verifica la respuesta final *y* los respaldos que cada paso afirmó usar. Una investigación agéntica puede salir bien de punta a punta o fallar en el paso dos con buena cara al final; por eso su trazabilidad paso a paso — el capítulo siete la exigió — es también material de la vara: la fidelidad de un agente se audita sobre su recorrido, no solo sobre su conclusión. Es la misma vara con más páginas que leer, y es la razón por la que los patrones altos necesitan la vara más, no menos.

!!! note "Criterio de salida"
    La vara de fidelidad operativa con su cobertura graduada por consecuencia: verificación de citas universal en todas las audiencias; fidelidad frase a frase y control de contradicciones completos en la audiencia de alta consecuencia; muestreo medido en la cola de baja consecuencia, con la métrica de frases sin respaldo en tendencia y su umbral de alerta. Una respuesta verificada no garantiza la verdad del mundo — garantiza que dice la verdad del corpus. Esa es la promesa que un sistema serio puede firmar.

La vara funciona en vivo, respuesta a respuesta. Pero un sistema que quiere mejorar necesita saber si está mejorando — necesita evaluar por lote, con dataset y con criterio. El juez de la respuesta, el capítulo quince, cierra la parte cuarta.
