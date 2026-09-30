---
title: 16 · Cada patrón en producción
---

# 16 · Cada patrón en producción

El panel que abre este capítulo era, la mañana del martes, un panel tranquilo: todo verde, todo dentro de umbrales. Y sin embargo el equipo de una operadora logística tenía la sensación — no el dato — de que el asistente interno iba más lento esa semana. Tres días de indagación produjeron el diagnóstico, y es un clásico del género: el percentil noventa y cinco de latencia había subido tres segundos sin que nadie desplegara nada. La causa tampoco era un incidente: era el router, que esa semana — por el tipo de consultas de la campaña comercial — había encaminado más tráfico del habitual al patrón autocorrectivo, cuyos reintentos silenciosos pagan su segunda mirada en segundos. Nada estaba roto. Todo estaba comportándose según su diseño. Y sin un panel que mirara **por patrón**, el sistema era indescifrable incluso para quienes lo habían construido.

La observabilidad del acceso — qué consultas entran, qué recuperan, qué resultados devuelven, con el desenlace de cada consulta — quedó construida en el capítulo veinte del libro anterior. Este capítulo la completa con la capa que faltaba: la **observabilidad de la respuesta** — no el hecho de que respondió, sino la calidad de cómo respondió: la latencia por patrón, las abstenciones, los reintentos, los comportamientos anotados —, porque un desenlace registrado no es una calidad medida. Es el capítulo menos heroico del libro y, con el siguiente, el que decide si el sistema envejece mejorando o envejeciendo a ciegas.

## Las métricas por patrón

El panel de respuesta se organiza por patrón — esa es su primera decisión de diseño, y la escena de apertura explica por qué: los patrones tienen perfiles de coste y comportamiento tan distintos que mezclarlos en una media produce números sin significado. Siete métricas mínimas, cada una con su pregunta.

La **latencia en percentiles por patrón** — p50 y p95, nunca medias — responde a "¿estamos cumpliendo el contrato de cada audiencia?". La **tasa de reintentos** — cuántas consultas del iterativo y del autocorrectivo consumen su presupuesto de vueltas — responde a "¿están los presupuestos bien puestos?": una tasa de reintento altísima indica evaluadores paranoicos o familias mal asignadas; una nula indica evaluadores complacientes. La **tasa de abstención** responde a la pregunta más delicada del panel, y su lectura correcta es contraintuitiva: una abstención razonable y estable es **salud** — el sistema sabe dónde está su borde; una tasa que se desploma es alarma — el sistema empezó a contestar de todo —, y una que se dispara, también — el corpus se quedó viejo o el evaluador se endureció. La abstención se celebra y se estudia; no se castiga.

La cuarta es el **coste diario por patrón**, que convierte la contabilidad del capítulo diez en vigilancia: no la factura del mes — eso es arqueología —, sino el consumo de ayer comparado con su familia de consultas, porque los desvíos de presupuesto se ven en días, no en meses. La quinta es el **acierto del router** con su matriz de confusión, ya presentada. La sexta es la **métrica de fidelidad** de la vara — frases sin respaldo en tendencia, citas verificadas —. Y la séptima, que atraviesa todas las demás, es el **volumen por familia de la matriz**: la composición real del tráfico, sin la cual ninguna de las anteriores se interpreta bien — tres segundos de p95 no significan lo mismo en una semana de catálogo que en una de expedientes.

## La observabilidad del agente: paso a paso o nada

El agente del capítulo siete ya prometió su trazabilidad como condición de existencia; este es el capítulo donde esa promesa se vuelve infraestructura. La ejecución de cada investigación queda persistida con su cadena completa — el plan inicial, cada razonamiento, cada invocación de herramienta con su entrada y salida, cada observación, cada corrección del plan —, y sobre esa materia prima se construyen tres vistas de producción.

La vista del **incidente**: cuando una investigación sale mal, el equipo puede leerla como quien lee el parte de un accidente — y en la mayoría de los casos el paso culpable salta a la vista, porque la cascada deja rastro. Sin persistencia, el incidente agéntico es un misterio con factura; con persistencia, es un parte con culpable y antídoto. La vista de la **salud de las herramientas**: qué herramientas consume más cada familia, cuáles fallan, cuáles responden lento — porque una herramienta degradada envenena todas las investigaciones que la usan, y su síntoma primero no es una respuesta mala: es una latencia que crece. Y la vista de los **presupuestos**: cuántas investigaciones consumen su tope de pasos o de tokens, y en qué paso se quedan — porque un tope que se consume a menudo está mal puesto, y su corrección es decisión de matriz, no de tuning.

El multi-agente hereda todo esto y añade la vista de la **coordinación**: rondas de devolución por pieza, traspasos con campos vacíos, tiempos de espera entre especialistas. Es la telemetría del equipo — menos glamorosa que el informe final, y la única que dice si el equipo funciona o solo se reúne.

## Streaming, espera y honestidad

La capa de producción de la respuesta tiene también su cara de experiencia, y dos decisiones la dominan. La primera es el **streaming**: servir la redacción mientras se produce. En el directo y el iterativo es una mejora clara — la lectura progresiva convierte la espera en uso —. En el agente tiene una reserva de honestidad: los patrones que planifican pueden corregirse a mitad de texto, y streamer sin aviso entrega borradores que se retractan; se hace, pero marcando el texto provisional como provisional. La segunda decisión es el **progreso legible**: qué se muestra durante la espera. La regla es la del capítulo diez — vocabulario del dominio, no barras abstractas: "consultando normativa de tres territorios" informa; una rueda girando excusa. Y su valor excede la estética: enseña a los usuarios qué preguntas son de investigación, y esa educación — gratis, emergente — es la que luego hace tolerable el contrato de minutos de la audiencia de exhaustividad.

## Los guardarraíles hasta la última milla

Hay una familia de controles de producción que no miden rendimiento sino **límites**, y que este libro deja para el final de su recorrido técnico a propósito: los guardarraíles, la traducción a la capa de respuesta de la gobernanza que el primer libro instaló en los datos.

El primero es el de **permisos heredados**: la respuesta — y cada paso intermedio que la produce — debe ver solo lo que quien pregunta puede ver. En el directo la herencia es trivial — el filtro del acceso ya la resolvió —; en los patrones superiores es un contrato activo: cada herramienta del agente se ejecuta con los permisos del usuario que pregunta, jamás con los del sistema; cada especialista del equipo ve su dominio *dentro* del alcance de quien pregunta. La respuesta es el punto donde los permisos se convierten en texto visible — y una fuga en esta capa es una fuga publicada.

El segundo es el de **datos personales**: lo que no debía salir del contexto no sale en la redacción. El control es doble — en la entrada, el acceso no recupera lo no autorizado; en la salida, la verificación comprueba que la respuesta no contiene identificadores que el contexto no debía revelar —, con la regla de oro de la serie: los permisos no son una preferencia del diseño, son una condición de existencia.

El tercero es el de **auditoría completa**: cada respuesta — con su patrón, su contexto, sus pasos si los hubo, su verificación si la tuvo — queda registrada y consultable. No por burocracia: porque cuando la consecuencia del error es alta, la pregunta "¿por qué dijo esto el sistema, con qué materiales, con qué permisos?" no es opcional. La auditoría de respuesta es la página que el capítulo cuatro llamó la virtud del directo — una trazabilidad legible — extendida, con más tomos, a todo el espectro.

## El panel que se lee en quince minutos

Un panel que nadie lee es un adorno caro, y los paneles de respuesta tienen la virtud — y el riesgo — de ser muy legibles. Conviene, por tanto, fijar la rutina de lectura que el triaje semanal aplicará, porque el orden importa: las métricas se leen en cascada, de la causa a la consecuencia.

Se empieza por el **volumen por familia** — qué semana ha traído el tráfico, porque todo lo demás se interpreta sobre esa composición. Se sigue con el **router**: su acierto y su matriz de confusión, porque un error de encaminamiento explica la mitad de las anomalías posteriores. Después, **latencia y coste por patrón** contra presupuesto — la factura de la semana y su contrato —. Después, las **abstenciones**: la métrica más informativa y la más fácil de malinterpretar, que se lee en tendencia y por familia, nunca en absoluto. Después, **reintentos y presupuestos consumidos**. Y al final, la **fidelidad**: la tendencia de frases sin respaldo y el recuento de citas verificadas — que es la última palabra de la semana sobre la salud del conjunto.

Quince minutos, siete métricas, un orden. Lo que esa rutina produce no es vigilancia: es el material del capítulo siguiente — las señales que el triaje convertirá en trabajo, y el trabajo, en mejora. El panel existe para que el bucle tenga con qué girar.

## La puerta de despliegue

El último mecanismo de producción del libro ya se presentó en el capítulo quince y aquí solo se instala en su sitio definitivo: **nada entra en producción sin pasar la regresión de respuesta** — ni un cambio de plantilla, ni un nuevo modelo, ni una familia migrada de patrón. Y una pieza operativa que completa la puerta: el **despliegue graduado**. Los cambios serios no salen a toda la cola el primer día: salen a un canal de prueba — una familia, un porcentaje de tráfico — donde los paneles de este capítulo vigilan sus métricas reales durante una semana, y solo entonces generalizan. Es la misma prudencia que el primer libro aplicó al corpus — cambio idempotente, con reversión — trasladada a la capa donde todo es invisible hasta que duele.

El criterio de salida de este capítulo es entonces un panel con nombre y apellidos: el **panel de respuesta por patrón**, con las siete métricas, sus umbrales y sus alertas — y la costumbre de mirarlo en el triaje semanal junto a las señales del acceso. Los paneles no mejoran sistemas: mejoran a los equipos que los miran. Pero sin panel, ni eso.

## Preguntas que hace el oficio

**¿Cuántos paneles hacen falta? ¿Uno por patrón?** Uno, cortado por patrón — siete métricas, la lectura en cascada del capítulo —. Los paneles múltiples dispersan la atención; el corte por patrón dentro de un solo panel obliga a ver las columnas juntas, que es donde está la información: la subida de reintentos del autocorrectivo al lado de la latencia que causa.

**¿Qué umbrales pongo si acabo de desplegar y no tengo historia?** Los del contrato, provisionalmente, y se recalibran con treinta días de datos reales. El error común es fijar umbrales perfectos antes de tener base — y pasar el primer trimestre ajustando alarmas en vez de leyendo señales. Un umbral provisional declarado es honesto; uno definitivo sin datos, es una apuesta con sirenas.

**¿Los guardarraíles no frenan la agilidad del sistema?** La ordenan. Los permisos heredados, la PII y la auditoría no cambian con cada consulta — cambian con el despliegue —, y una vez instalados no consumen decisión diaria: consumen diseño inicial. Lo que frenan es una sola cosa, y la deben frenar: que un patrón nuevo salga a producción sin decidir quién puede ver qué.

**¿Cada cuánto se repite la regresión completa?** En cada cambio de la capa final — patrón, modelo o plantilla — y como mínimo mensual aunque no haya cambios, porque el mundo cambia debajo: fuentes, volúmenes, modelos del proveedor actualizados sin aviso. La regresión sin cambios es el detector de deriva: el examen que aprueba lo que nadie movió.

**¿Qué hago cuando dos métricas se contradicen — latencia abajo, fidelidad arriba?** Que trabajan: cada una vigila un contrato distinto, y su tirantez es la información. El caso típico: bajar la latencia del iterativo reduciendo vueltas sube los respaldos flojos. La resolución no es métrica a métrica — es de contrato: si la audiencia firmó exhaustividad, gana la fidelidad y la latencia se negocia; si firmó segundos, gana la latencia y la familia se reasigna al patrón que pueda con ambas. Las métricas no deciden: delatan la fila de la matriz que está mal puesta — y esa, con su evidencia, es la que se corrige.

!!! note "Criterio de salida"
    El panel de respuesta operativo: las siete métricas por patrón (latencia en percentiles, reintentos, abstenciones, coste diario, acierto del router, fidelidad, volumen por familia) con umbrales y alertas; la trazabilidad completa persistida y consultable para cada respuesta — pasos incluidos en los patrones que investigan; y la puerta de despliegue con regresión aprobada y despliegue graduado. Nada se degrada en silencio: eso es todo.

El panel muestra lo que el sistema hace. Falta la pieza que usa lo que el sistema hace para mejorarlo: lo que las respuestas enseñan — sus abstenciones, sus veredictos, sus errores de encaminamiento — y cómo vuelve al corpus. El bucle completo, el capítulo diecisiete.
