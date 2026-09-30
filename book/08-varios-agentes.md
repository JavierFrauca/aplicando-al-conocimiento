---
title: 8 · Varios agentes, un despacho
---

# 8 · Varios agentes, un despacho

La consulta que abre este capítulo llegó a la dirección técnica de una constructora un jueves por la mañana: "¿Podemos ampliar el almacén de la obra del puerto? Necesito saber qué exige la autoridad portuaria para obras en el recinto, qué licencias pide el municipio, y si el convenio del sector permite los turnos de noche que necesitamos para el plazo". Tres preguntas en una, y tres mundos distintos: la normativa de la autoridad portuaria vive en un corpus, las licencias municipales en otro con otro vocabulario, y el convenio colectivo en un tercero con su propia terminología y su propia vigencia.

En la organización, esa consulta siempre tuvo un recorrido conocido: se repartía a mano entre tres personas — el técnico que conocía el recinto portuario, el jurista municipal, el responsable de relaciones laborales — y un cuarto coordinador recogía las tres piezas, las cruzaba y firmaba la respuesta. El recorrido funcionaba porque respetaba las fronteras reales del conocimiento: cada especialista dominaba su corpus, su vocabulario y sus matices, y la pieza final solo salía cuando alguien con autoridad la había verificado entera.

La quinta estación del espectro, el **sistema multi-agente**, es ese reparto formalizado dentro del sistema: un coordinador que encamina, especialistas que investigan en paralelo, y un verificador que cruza antes de firmar. No inventa el trabajo en equipo — lo automatiza donde ya existía — y esta distinción, como veremos, es la línea que separa la arquitectura del teatro.

## El equipo, en tres órganos

Un sistema multi-agente tiene tres roles, y conviene definirlos con precisión porque en la literatura aparecen con nombres que se solapan.

El **coordinador** — a veces *orchestrator*, a veces *supervisor* — recibe la pregunta del usuario y hace tres cosas: la analiza para detectar qué dominios toca, la descompone en subtareas asignables, y reparte. Su criterio de reparto es el mapa de dominios — la herencia directa del primer libro: los dominios que el corpus acotó son exactamente las especialidades que el equipo puede cubrir. Devuelve además al usuario una expectativa honesta: "esto requiere consultar tres áreas, tardará unos minutos".

Cada **especialista** es un agente en el sentido del capítulo anterior — planificación, herramientas, evaluación — pero con un territorio acotado: su corpus de dominio, sus herramientas propias, su vocabulario, sus permisos. El especialista portuario no ve el convenio colectivo; el laboral no consulta la base de datos de licencias. Esa acotación no es una limitación: es la fuente de su calidad. Un especialista que sabe todo lo de su dominio y nada de lo demás equivale, en fiabilidad, a lo que el primer libro llamaba acotar para dominar — la especialización es al equipo de agentes lo que el dominio es al corpus.

El **verificador** es el tercero, y es el que más se omite en los diseños apresurados. Recibe las piezas de los especialistas y hace el trabajo del cuarto coordinador humano de la constructora: comprueba que las piezas responden a lo que se les pidió, caza contradicciones entre ellas — la normativa portuaria exige un distanciamiento que el plan municipal no contempla —, verifica que cada afirmación lleva su cita de su dominio, y solo entonces da paso a la redacción final. Es el control de calidad del equipo, y su lección de nacimiento fue amarga: los equipos sin verificador producen respuestas Frankenstein — correctas por partes, incoherentes en la costura.

## Los especialistas: los dominios hechos agentes

Hay una continuidad histórica que este capítulo completa y conviene subrayar, porque demuestra que la serie es una sola historia. El primer libro acotó el corpus en **dominios** — porque buscar en todo es no buscar en nada — y construyó cada uno con sus fuentes, su Silver, su Gold. El segundo libro usó esos dominios como **coordenadas de filtrado** — porque el filtro previo evita el ruido de otros mundos. Este capítulo los usa por tercera vez, en un plano superior: los dominios se convierten en **especialidades del equipo**. La frontera que dibujamos para los datos gobierna ahora a los agentes.

Cada especialista hereda, además, la madurez de su dominio. Un dominio con corpus maduro, dataset áureo en verde y observabilidad operativa sostiene un especialista fiable desde el primer día. Un dominio inmaduro — recién abierto, con vacíos de cobertura — produce un especialista que responde con el seguro aspecto de la fluidez sobre un terreno hueco. La regla de admisión al equipo es, por tanto, la misma que la del corpus: no entra el especialista de un dominio que no pasaría su propia prueba de aceptación.

El reparto entre especialistas no siempre es por materia. Hay equipos donde la especialidad es la **tarea**: un especialista que solo busca y resume, otro que solo calcula, otro que solo redacta. Es una división válida — y a veces más barata —, pero tiene un techo que la división por dominio no tiene: no respeta las fronteras del conocimiento, y por tanto no hereda su fiabilidad. La regla práctica: dividir por dominio cuando el caso cruza territorios del corpus; dividir por tarea solo cuando el cuello de botella es la calidad de una fase concreta y verificable.

## El verificador: la puerta antes de la calle

Merece sección propia porque es la pieza que convierte un conjunto de agentes en un sistema responsable. El verificador del equipo cumple, trasladado a la capa de respuesta, la función que el capítulo seis puso dentro del bucle: la segunda mirada. Pero con una diferencia de escala: no replica contra la cosecha de una búsqueda, sino contra el **conjunto de piezas** que el equipo ha producido.

Su lista de comprobación es corta y valiosa. **Completitud**: cada subtarea del reparto tiene pieza entregada — sin silencios que el redactor rellenaría con imaginación. **Contradicciones**: las piezas cruzadas no se pelean entre sí; si se pelean, la discrepancia sube explicada, no resuelta por voto. **Trazabilidad por pieza**: cada afirmación lleva la cita de su dominio, y las citas existen y dicen lo que se dice que dicen. **Fronteras**: ninguna pieza invade el dominio de otra — el especialista laboral opinando de licencias es exactamente el fallo que la especialización debía impedir. Y **contrato**: la respuesta agregada cumple el contrato de la audiencia que preguntó — el grado de cita, los límites declarados, la abstención bien dicha.

Cuando la lista falla, el verificador tiene dos salidas dignas: devolver la pieza al especialista con el motivo — un reintento acotado, con presupuesto — o declarar la respuesta incompleta y entregar lo que hay, marcado como tal. Lo que no puede hacer es coser: un verificador que rellena huecos se convierte en el peor especialista del equipo.

## Cuándo es arquitectura y cuándo es teatro

El multi-agente es el patrón más caro del espectro y el que más se implanta por moda. La prueba para distinguir arquitectura de teatro cabe en una pregunta: **¿este reparto existía a mano en la organización?** Si la consulta cruzada se repartía entre áreas, con coordinador y revisión, el multi-agente la formaliza: reduce minutos a segundos-muchos, documenta lo que antes era conversación, y escala lo que antes dependía de que las tres personas estuvieran disponibles. Es arquitectura con retorno medible.

Si el reparto no existía — si una persona sola resolvía el caso completo consultando tres corpus —, el multi-agente no formaliza nada: añade coordinación, latencia, facturas y costuras donde había un trabajo continuo. Es teatro: varios agentes disfrazando lo que es un agente con buen acceso a tres fuentes. La regla de oro es la del capítulo anterior, elevada de herramientas a dominios: **cada agente del equipo debe tener un caso real que justifique su plaza** — y si un agente no tiene caso propio, no tiene plaza.

## El precio del equipo: la coordinación que se factura

El coste de coordinación, medido sin piedad, es la otra cara. Cada pieza que viaja entre agentes es contexto que se serializa, se transmite y se relee — latencia y tokens —; cada ronda de coordinación es otra vuelta completa del ciclo. Un equipo de tres especialistas con verificador puede multiplicar por cinco o por diez el coste del agente único del capítulo anterior. Paga solo donde el valor de la respuesta cruzada supera ese múltiplo: la decisión de obra, el análisis regulatorio multi-país, el expediente que cruza materia y personas.

## Tres contraejemplos, para calibrar

Nada afina el criterio como el caso que parece encajar y no encaja. Tres contraejemplos recurrentes — consultas que *parecen* de equipo y no lo son — valen su sección.

El primero: la consulta **larga y multi-tema pero secuencial**. "Revisa el contrato, extrae las obligaciones, y cítalas con la normativa aplicable." Parece de tres especialistas — contrato, obligaciones, normativa —, pero es una cadena donde cada paso depende del anterior y ninguno cruza dominios en paralelo: es un agente con buen plan y acceso a dos corpus. El equipo añadiría costura sin añadir paralelismo real.

El segundo: la consulta que cruza dominios pero **en baja consecuencia y alta frecuencia**. "¿Puedo cambiar la fecha del turno y a qué afecta?" toca el convenio y la operativa — pero la respuesta es rutinaria, su error es barato y su volumen es enorme. El multi-agente aquí es el despilfarro canónico: pagar coordinación de expertos por una consulta de nivel uno con dos fuentes. Un agente único con acceso a ambos corpus, o incluso un directo con dos cosechas, la sirve a fracción del precio.

El tercero, el más seductor: el equipo **como arquitectura de futuro** — "lo montamos así para cuando crezca". Los equipos de agentes no se apolillan de espera: se apolillan de mantenimiento — permisos que rotan, contratos de herramienta que se desactualizan, especialistas cuyos dominios cambian debajo. El multi-agente no se construye por anticipado por la misma razón que el grafo del capítulo nueve no se construye por anticipado: su coste es continuo, y su valor solo existe con el caso del presente pagándolo. El futuro, cuando llegue, llegará con su propia evidencia — y con un mejor multi-agente disponible.

## Los fallos nuevos del equipo

Trabajar en equipo introduce fallos que ningún agente individual sufre, y una arquitectura honesta los prevé por nombre.

El **bucle entre agentes** — el ping-pong: el verificador devuelve la pieza, el especialista la vuelve a entregar casi igual, el verificador la devuelve de nuevo. Es el bucle del capítulo cinco con traje de equipo, y su freno es el mismo: presupuesto de rondas — dos devoluciones máximo por pieza — y salida digna: la discrepancia se declara en la respuesta, con su origen, en lugar de perseguir la perfección eterna.

La **dilución de responsabilidad** — cuando todos investigan, nadie firma. Las respuestas de equipo sin dueño son imposibles de auditar y de mejorar: ante un fallo, cada agente señala al anterior. El antídoto es organizativo, no técnico: la respuesta final tiene **un único responsable nominado** — el verificador, en el diseño canónico — que es a quien el diario de consultas y el bucle de retorno reclamarán.

La **pérdida de contexto en los traspasos** — el *handoff* — es el fallo más silencioso. La pieza que el especialista portuario entrega al coordinador no es su investigación entera: es un resumen, y todo resumen pierde matices. El matiz perdido — "el distanciamiento se exige salvo autorización expresa de la capitanía" — es precisamente el que la respuesta final necesitaba. El antídoto es estructurar el traspaso: no un texto libre que el especialista redacta, sino una **ficha** con campos fijos — hallazgos, citas, condiciones, excepciones, dudas — que nada se deja sin sitio. El traspaso estructurado es a los equipos de agentes lo que el payload versionado del segundo libro fue a las píldoras: el contrato que impide que la información se evapore en los cambios de mano.

## A quién sirve

El multi-agente tiene una audiencia clara y exigente: las consultas que cruzan dominios con consecuencia alta — la decisión de obra, el expediente regulatorio, el análisis de un grupo empresarial que toca varios cuerpos normativos — y las organizaciones cuyo trabajo diario ya es interdisciplinar. Su promesa no es responder más rápido que el equipo humano: es responder *siempre igual de bien* que el mejor día del equipo humano — sin esperar a que los tres especialistas tengan la mañana libre.

Y cierra con la advertencia que ordena todo el espectro desde el capítulo dos: este es el final de la línea, no el principio. Nadie empieza por aquí. El camino sano hacia un equipo de agentes pasa por un directo medido, un iterativo con presupuesto, una réplica interna calibrada y un agente trazable — cada estación resuelta antes de pagar la siguiente. El multi-agente es lo que ocurre cuando la organización ya sabe, con datos, que sus preguntas cruzan fronteras: entonces, y solo entonces, el equipo paga su sitio.

## Preguntas que hace el oficio

**¿Los agentes "discuten" entre ellos como los equipos humanos?** No, y conviene matar la imagen: la discusión humana produce criterio porque hay un moderador con autoridad y una verdad externa. Entre agentes, el debate sin árbitro produce rotación de opiniones con gasto de tokens. Lo que sí existe es la **divergencia estructurada**: el verificador detecta que dos piezas discrepan, y la discrepancia sube al informe — explicada y citada, no votada. La discrepancia es información; la votación, su eliminación.

**¿Puedo añadir agentes a un sistema que ya funciona para "prepararlo para el futuro"?** La pregunta del capítulo nueve sobre el grafo sirve idéntica: el coste del equipo es continuo, su valor solo existe con el caso del presente pagándolo. Se añade el especialista cuando el caso existe y está medido — y se quita cuando el caso desaparece. La arquitectura de futuro se construye con contratos y fronteras limpias, no con agentes anticipados.

**¿Un especialista puede ser solo un buen directo con su corpus?** Sí — y es el diseño más frecuente en equipos reales: la mayoría de los especialistas son patrones bajos bien acotados, y solo los casos verdaderamente investigadores llevan agente dentro. El multi-agente no impone su patrón interno: cada especialista usa el suyo, según su familia. El equipo orquesta patrones; no los unifica.

**¿Quién responde al usuario, el equipo o el coordinador?** La respuesta final la redacta una pieza única — el verificador o un redactor final que él supervisa — y el responsable nominado responde por ella. Lo que el usuario nunca debe recibir es el collage: tres estilos, tres trazabilidades sin coser. Un equipo sin redacción final unificada es un muro de piezas.

!!! note "Criterio de salida"
    El mapa del equipo por escrito: cada agente con su dominio, sus herramientas, sus permisos y el caso real que justifica su plaza; el traspaso entre piezas estructurado en fichas con campos fijos; el presupuesto de rondas de devolución; y un único responsable nominado de cada respuesta final. Un equipo sin mapa ni responsable no es un sistema multi-agente: es una reunión de máquinas.

Quinta estación recorrida. El sexto patrón no cambia cuánto piensa el sistema — cambia cómo está guardado el conocimiento. Cuando la respuesta vive en las relaciones y no en los pasajes, el vector se queda corto: GraphRAG.
