---
title: 6 · La réplica interna: autocorregirse
---

# 6 · La réplica interna: autocorregirse

El material que alimenta este capítulo llega de un sector donde el error se paga caro: una aseguradora con un corpus de condiciones generales y especiales que se actualiza por producto y por año. Una consulta corriente — "¿cubre este seguro la asistencia en viaje en el extranjero?" — recibe del motor una cosecha en apariencia buena: tres píldoras pertinentes, bien rankeadas, del producto correcto. El problema está en la tercera: corresponde a la versión de la póliza del año anterior, que en este producto cambió la cobertura extranjero de incluida a opcional. Dos píldoras dicen una cosa, una dice la contraria, y ninguna viene marcada como muerta porque el proceso de ciclo de vida — su capítulo en el primer libro — la dejó viva por un error de vigencia.

Un RAG directo redacta sobre esa cosecha. Y redacta *algo*: probablemente tome las dos píldoras coincidentes — mayoritarios, verosímiles — y acierte; quizá tome la tercera por estar bien posicionada y mienta con fundamento aparente. Nadie en el pipeline lo sabrá: no hay una segunda mirada en ninguna parte del recorrido. La respuesta saldrá fluida, citada, y —la mitad de las veces— correcta. Esa estadística, "la mitad de las veces", con apariencia de éxito, es exactamente el terreno donde los sistemas de conocimiento pierden la confianza de sus usuarios: no cuando fallan del todo, sino cuando fallan a veces y no se distingue cuándo.

La tercera estación del espectro existe para esa mirada que faltaba. El **RAG autocorrectivo** introduce en el propio bucle de respuesta un evaluador que replica contra la cosecha antes de redactar: ¿lo recuperado basta?, ¿es de esta pregunta?, ¿se contradice?, ¿está vivo? Y según las respuestas, decide: redactar, volver a recuperar con otra consulta, o declarar que no hay respuesta. En esta serie la llamamos **réplica interna** porque ese es exactamente su mecanismo: el sistema se replica a sí mismo sobre su propia evidencia, en tiempo de ejecución, antes de comprometerse.

## El concepto: el juez que entra en el bucle

El segundo libro presentó al **LLM como juez** dentro del propio pipeline: un modelo que, con veredictos cerrados — responde, responde parcialmente, no responde —, calificaba los candidatos de la cosecha antes de ensamblar el contexto, en el mismo camino del usuario. La réplica interna es esa misma herramienta ascendida de cargo: el juez deja de certificar candidatos uno a uno y replica contra la cosecha entera — ¿basta lo recuperado para esta pregunta? —, en vivo, en el camino entre la recuperación y la redacción.

La diferencia de consecuencias es total. Un veredicto que filtra candidatos separa lo útil de lo inútil, pero el pipeline sigue su curso con lo que queda. Un veredicto sobre la cosecha entera, en cambio, puede *no contestar mal*: reintentar, reformular, abstenerse. Y cuando la medición ocurre después de los hechos — la vara del acceso —, el problema detectado solo produce un informe que alguien leerá el lunes. Es la diferencia entre el filtro, el freno y la autopsia.

Y conviene nombrar de entrada lo que la réplica interna **no** es. No es un modelo más listo: usa el mismo modelo, con otra instrucción y otra rúbrica. No es más contexto: no añade información, solo juicio sobre la existente. No es magia: es una llamada más, pagada en latencia y coste. Todo lo que sigue son maneras de que esa llamada pague su sitio.

## CRAG: el evaluador con tres veredictos

La versión esquemática y más implantable de la réplica interna está inspirada en el **CRAG** de la literatura — *corrective RAG*, RAG correctivo —, simplificado para producción: donde el CRAG original corrige saliendo a buscar a la web, esta versión mantiene la corrección dentro del corpus o declara el vacío. Se entiende como un mini-contrato entre el recuperador y el redactor, mediado por un evaluador. El evaluador recibe la pregunta y la cosecha, y emite uno de tres veredictos, cada uno con su acción asociada por diseño.

El primer veredicto es **suficiente**: la cosecha cubre la pregunta — cada elemento pedido tiene respaldo, no hay contradicciones sin resolver, el material está vivo y vigente. Acción: continuar a la redacción normal. Es el veredicto que esperaría el noventa por ciento de las veces en un corpus sano, y por eso el evaluador debe ser rápido y barato: una llamada corta con rúbrica estricta, no un ensayo.

El segundo es **insuficiente**: la cosecha es pertinente en tema pero no alcanza — le falta una pieza, responde a medias, dejó fuera el elemento concreto que la pregunta pedía. Acción: **reescribir la consulta y volver a recuperar**. Y aquí está la parte más valiosa del diseño: la reescritura no es un reintento idéntico — eso devolvería lo mismo — sino una reforma informada: el evaluador indica qué falta ("falta la cobertura del año en vigor"), y la consulta se reescribe con esa indicación — términos más específicos, el año explícito, el sinónimo del dominio. El segundo intento llega donde el primero no llegó *porque sabe por qué el primero falló*.

El tercero es **equivocado**: la cosecha es de otro asunto — el motor encontró parecido superficial, o el corpus simplemente no cubre la pregunta. Acción: declararlo. Qué significa declararlo — abstenerse, escalar a un humano, salir a una fuente externa autorizada — es decisión del contrato de respuesta de la audiencia, no del patrón. Lo que CRAG aporta es la detección temprana: el sistema se entera *antes* de redactar de que su cosecha era de otro tema, que es exactamente el momento en que aún no se ha hecho daño.

## Self-RAG: la reflexión dentro de la redacción

La segunda versión de la réplica interna, **Self-RAG**, no pone el juez entre recuperación y redacción: lo integra en la propia generación. El modelo que redacta también se evalúa, en tres momentos.

Antes de recuperar: el modelo decide si la pregunta siquiera merece recuperación — para saludos y metapreguntas, no; para cualquier contenido de corpus, sí. Es un detalle menor con efecto real en sistemas con tráfico mixto.

Durante la lectura del contexto: el modelo evalúa cada píldora que usa — ¿es pertinente a esta frase que estoy construyendo?, ¿la respaldo?, ¿entra en conflicto con otra que ya acepté? — y las que no pasan el filtro no entran en la redacción. Es el mecanismo que habría capturado la píldora caducada de la aseguradora: la contradicción con las dos vivas la habría señalado.

Después de cada tramo del borrador: el modelo critica su propio texto — ¿esta afirmación está respaldada por lo que tengo?, ¿estoy extrapolando? — y si la crítica falla, o recupera para ese hueco (conectando con la estación iterativa del capítulo anterior) o reformula la frase hacia lo que sí está respaldado.

Self-RAG es más fino que CRAG y más caro: las críticas por tramo multiplican llamadas. Su terreno natural son las respuestas largas y de alta consecuencia — informes, resúmenes normativos, documentación técnica — donde el fallo localizado en el momento justo evita la respuesta entera contaminada. CRAG es el comodín general: barato, claro, auditable; el elegido cuando la réplica interna se quiere para toda una familia de consultas.

## El coste, y cuándo lo paga el negocio

La aritmética de la réplica interna se puede escribir en una línea: **se justifica donde el coste del error supera el coste de la segunda mirada**. La segunda mirada cuesta una o dos llamadas más — segundos, céntimos. El error cuesta lo que cueste en cada sector: una indemnización, una dosis, una decisión de cliente mal informada, la confianza que se evapora en un equipo de ventas que descubre que el asistente inventó una cobertura.

Esa comparación es la que decide, por familia de consultas, si la réplica interna entra o no. Las consecuencias altas — jurídico, clínico, regulatorio, contractual, lo que en el libro llamamos debido proceso — la pagan encantadas: es el seguro más barato del mercado. Las consecuencias bajas y el volumen masivo — el nivel uno del soporte, las consultas de catálogo — rara vez: allí el error se detecta en la siguiente interacción y cuesta poco; la segunda mirada permanente sería un seguro más caro que el siniestro.

Y hay un término medio que conviene nombrar porque es donde mejor trabaja el router del capítulo doce: la réplica interna **condicionada**. El evaluador CRAG es tan barato que puede correr siempre — su primer veredicto, "suficiente", cuesta una llamada corta —, y solo los veredictos de insuficiencia activan las acciones caras. Esa arquitectura — evaluar siempre, corregir a veces — es el punto dulce de la estación: el seguro permanente al precio de una pregunta de control.

## Tres arquitecturas de la segunda mirada

Una vez el evaluador existe, queda la decisión de **cuándo corre**, y hay tres arquitecturas honestas, con costes muy distintos. Nombrarlas evita el error más común de la estación: poner la segunda mirada más cara donde bastaba la barata.

La primera es la **evaluación permanente**: el evaluador corre en cada consulta de la familia, y solo los veredictos de insuficiencia activan acciones caras. Es la arquitectura CRAG canónica — el seguro permanente al precio de una llamada corta — y es la recomendada por omisión: su coste base es tan bajo que rara vez se justifica prescindir de ella.

La segunda es la **evaluación por confianza**: solo se evalúa cuando la propia cosecha da señales de debilidad — pocas píldoras, scores bajos, familias históricamente problemáticas. Ahorra el evaluador en la mayoría de las consultas, pero delega en una heurística previa la decisión que el evaluador estaba llamado a tomar; en los sistemas reales, las señales previas fallan justo donde más duele — la cosecha mala que parece buena. Se justifica solo cuando el volumen hace insostenible la permanente y el margen de error lo permite.

La tercera es la **evaluación por muestreo**: se evalúa un porcentaje de consultas, no para proteger a esa muestra sino para medir la salud del conjunto — es la vara del capítulo catorce en su peldaño bajo, no una réplica interna de verdad. Conviene no confundirla: muestrear mide; no protege. La protección permanente en la cola de baja consecuencia es la primera arquitectura, no esta.

## El arte de los umbrales: qué significa "cosecha suficiente"

Todo lo anterior depende de una definición que ningún proveedor regala: qué entiende el evaluador por cosecha suficiente. Es la rúbrica del juez en el bucle, y conviene diseñarla en cuatro dimensiones verificables, porque los umbrales vagos producen los dos fracasos simétricos de esta estación.

La primera dimensión es la **cobertura**: cada elemento de la pregunta tiene respaldo en la cosecha. La pregunta de la aseguradora pedía una cobertura concreta de un producto concreto en un ámbito concreto — extranjero —; una cosecha que hable de la cobertura doméstica no cubre, aunque sea afín. La segunda es la **coherencia**: no hay contradicciones sin resolver entre piezas. Si dos píldoras discrepan y la discrepancia no es explicable — versiones, vigencias, ámbitos —, la cosecha no es suficiente aunque su mayoría sea correcta. La tercera es la **vitalidad**: las piezas están vigentes, no marcadas como sustituidas, con fecha de control. La cuarta es la **pertinencia estricta**: responde *esta* pregunta, no una vecina — el fallo más común de la similitud semántica, que confunde el tema con la pregunta.

Con la rúbrica clara, los dos fracasos simétricos se pueden nombrar y vigilar. El evaluador **complaciente** — que aprueba casi todo — convierte la réplica interna en teatro: cuesta llamadas y no captura nada; se detecta midiendo su tasa de rechazo, que en un corpus sano debería estar en un rango razonable y estable, y se corrige endureciendo la rúbrica con ejemplos de rechazo real. El evaluador **paranoico** — que rechaza casi todo — convierte el sistema en un abstencionista crónico: seguro e inútil; se detecta igual, por la tasa de abstención disparada, y se corrige en la dirección contraria. La calibración del evaluador es, por tanto, un trabajo de medición continua con el juez del capítulo quince — no una configuración de un solo día.

## Los fallos propios de la estación

El autocorrectivo añade sus propios modos de fallo, y un libro que se precie los enumera. El **bucle de reintentos**: insuficiente, reescribe, insuficiente, reescribe — sin presupuesto de reintentos, la réplica degenera en el bucle del capítulo anterior; el freno es el mismo: máximo de correcciones y salida digna a la abstención. La **corrección que descarrila**: la reescritura de consulta, mal acotada, cambia de tema y la segunda cosecha es peor que la primera; el antídoto es reescribir *informado por el motivo del rechazo*, no generando una consulta nueva cualquiera. Y el **falso positivo de coherencia**: tres píldoras coincidentes pero todas caducadas — la coherencia sin vitalidad es la unanimidad del error; por eso la rúbrica exige las cuatro dimensiones, no solo la coincidencia.

Queda un detalle de honestidad técnica que cierra el capítulo: la réplica interna no garantiza verdad — garantiza *segunda mirada sobre la evidencia*. Si el corpus entero sostiene un error — si las tres píldoras están caducadas en la misma dirección —, el evaluador verá coherencia, cobertura y vitalidad aparente, y aprobará. Contra el error sistémico del corpus no hay patrón que valga: hay gobierno del corpus, que es el territorio del primer libro y del bucle de retorno del capítulo diecisiete. Los patrones deciden qué se hace con la evidencia; solo el gobierno decide si la evidencia es verdad.

## Preguntas que hace el oficio

**¿El evaluador no puede también equivocarse? ¿Quién vigila al vigilante?** Puede, y por eso su calibración es parte del criterio de salida: la tasa de rechazo medida contra muestras humanas, mes a mes, con la misma disciplina que cualquier otra pieza medida de la serie. La respuesta corta a la paradoja es la del oficio entero: nadie está sin vigilancia — el evaluador la recibe del dataset humano y del triaje, igual que el acceso la recibe de su dataset y la plantilla de su regresión. Cadenas de confianza, no oráculos.

**¿Puedo usar un modelo más grande solo para evaluar?** Sí, y es una decisión razonable — el evaluador corre pocas veces por consulta y solo necesita juicio, no redacción. La regla de coste: el evaluador debe costar una fracción del redactor por llamada, porque corre en todas; si el modelo evaluador es más caro que el redactor, la arquitectura del capítulo diez descuadrará.

**¿La réplica interna sustituye a la vara del capítulo catorce?** No: son la misma segunda mirada en dos momentos. La del bucle actúa antes de responder, contra la cosecha — protege a ese usuario; la de la vara vigila la respuesta terminada — respuesta a respuesta donde la consecuencia lo paga, por muestreo en el resto —, y lo que mide protege a todos los siguientes. Un sistema con réplica interna sigue necesitando vara: la primera mirada interna no exime del control de calidad del conjunto.

**¿Funciona con corpus pequeños?** Mejor todavía — los corpus pequeños tienen bordes cercanos, y el evaluador detecta pronto que la pregunta cayó fuera. Su riesgo en corpus pequeños es el opuesto: abstenerse demasiado, porque casi toda cosecha es limitada. Los umbrales de suficiencia se calibran con el corpus que se tiene, no con el que se quisiera.

!!! note "Criterio de salida"
    La réplica interna operativa por escrito: la rúbrica de "cosecha suficiente" con sus cuatro dimensiones (cobertura, coherencia, vitalidad, pertinencia estricta) y sus umbrales calibrados con la tasa de rechazo medida; la acción asociada a cada veredicto (redactar, reescribir informado, abstenerse o escalar); y el presupuesto máximo de reintentos por consulta. Un evaluador sin tasa de rechazo medida no está calibrado: está decorando el pipeline.

Tercera estación recorrida. La cuarta da un salto de jerarquía: dejar de tratar la recuperación como el centro y convertirla en una herramienta más de un planificador. Cuando la pregunta exige calcular, consultar sistemas vivos y volver atrás sobre el plan, hace falta un agente.
