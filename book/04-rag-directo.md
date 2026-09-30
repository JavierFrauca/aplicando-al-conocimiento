---
title: 4 · Una pasada: el RAG directo
---

# 4 · Una pasada: el RAG directo

Los viernes a última hora, la cola de consultas de cualquier servicio de atención se comporta igual en todos los sectores: se multiplica. En una comercializadora de energía, las preguntas de fin de ciclo de facturación llenan el buzón del asistente interno: ciclos estimados, lecturas, cambios de titular, potencias contratadas. Son preguntas que un empleado de back office formula a toda prisa, con prisa de viernes, y a las que espera una respuesta única: un plazo, un procedimiento, una tarifa, un enlace. El sistema que las atiende hace exactamente una cosa: busca en el corpus de normativa y procedimientos, toma lo mejor que encuentra y redacta. Una pasada. Segundos. Y en la inmensa mayoría de los casos, acierta.

Este capítulo defiende una tesis que el mercado actual hace sonar contracorriente: el RAG directo — la pasada única, el *one-shot* — no es la versión pobre de los patrones sofisticados. Es el patrón que responde la mayoría abrumadora de las consultas reales de las organizaciones, y dominarlo bien — saber dónde termina su competencia — vale más que cualquier entusiasmo por los agentes. El error no está en usarlo: está en usarlo uniformemente, para todo y para siempre, sin haber decidido nada.

## La anatomía de una pasada

Conviene empezar desmontando lo que ocurre en esa única vuelta, porque cada pieza viene del trabajo de los dos libros anteriores y la respuesta es donde todo ello se cobra.

Primero, la **recuperación**: la consulta — tras su normalización mínima — golpea el motor de búsqueda. Aquí está todo el segundo libro: el mapa de dominios y etiquetas que acota el espacio, el filtro que protege permisos y vigencia, el híbrido que combina lo semántico con lo léxico, el tope que decide cuánto cabe, el reranker que ordena. Ninguna de esas piezas se repite aquí: se hereda. En el patrón directo, la recuperación tiene una característica definitoria: ocurre **una vez, sin retorno**. Lo que entrega el tamiz es la cosecha final; no habrá segunda mirada.

Segundo, el **ensamblado del contexto**: las píldoras recuperadas se disponen en el orden y el formato que el modelo va a leer, con sus referencias, dentro del presupuesto de contexto. Es un trabajo de carpintería fina — el capítulo 17 del libro anterior lo cuenta — y en el directo es el último punto donde se puede corregir algo antes de que salga.

Tercero, la **redacción**: el modelo lee su plantilla y el contexto, y produce la respuesta. Aquí está la mitad de este libro — la plantilla, las citas, la abstención — pero por ahora basta fijar la estructura: tres pasos, una dirección, ningún retorno. La respuesta sale como sale la cosecha: si la cosecha fue buena, la respuesta puede ser buena; si fue coja, no hay segunda oportunidad.

Esa estructura tiene una consecuencia que conviene decir con todas las letras: **en el patrón directo, la calidad de la respuesta es esencialmente la calidad de la recuperación más la calidad de la redacción**. No hay un tercer factor que salve la combinación. Por eso el directo solo brilla donde el corpus y el acceso son serios — y por eso los dos libros anteriores son, en cierto modo, el prerrequisito de este.

## Las cuatro virtudes

Lo que la pasada única compra con su falta de imaginación son cuatro propiedades que ningún patrón superior puede igualar, y que conviene nombrar con precisión porque serán moneda corriente en el capítulo diez.

La primera es la **latencia**. Una llamada de recuperación — milisegundos en un motor bien indexado — y una llamada de generación — segundos. No hay planificación que redactar, vueltas que esperar, herramientas que contestar. El percentil noventa y cinco de un sistema directo bien construido cabe en los tres o cuatro segundos, y eso significa que puede firmar el contrato de latencia más duro que existe: el del usuario impaciente, el de la cola masiva, el de la pregunta entre reuniones.

La segunda es el **coste**. Un contexto y una redacción: eso es todo el gasto en tokens. Comparado con un iterativo de tres vueltas o un agente de diez pasos — donde cada paso reenvía todo el historial —, el directo cuesta una fracción. En volúmenes altos la aritmética es brutal: cuarenta mil consultas al mes a tres céntimos o a sesenta céntimos es la diferencia entre una herramienta y una partida presupuestaria.

La tercera es el **determinismo**: a igual cosecha, igual respuesta — o casi. El sistema directo con una plantilla estable es el patrón más predecible del espectro: sus variaciones son acotadas, sus regresiones detectables, su comportamiento explicable en una reunión sin paciencia para matices. Esta virtud no luce en demos y se echa de menos en producción.

La cuarta, la más infravalorada, es la **auditabilidad total**. La trazabilidad de una respuesta directa — consulta original, consulta normalizada, píldoras recuperadas con sus puntuaciones, contexto ensamblado, plantilla aplicada, respuesta final — cabe en una página. Un auditor humano la puede leer entera, de principio a fin, y señalar exactamente dónde se torció algo. Cuando el regulador pregunta, el contrato social del sistema depende de que exista esa página. Los patrones superiores producen trazabilidades de veinte páginas que nadie lee; el directo produce una que todos pueden leer. En sistemas con consecuencias legales, esta virtud vale más que toda la sofisticación del mundo.

## El precio, que el capítulo diez contabilizará

Dos de las cuatro virtudes son, en la contabilidad del capítulo diez, las monedas que el directo firma mejor que nadie: la latencia — segundos bajos, el percentil más duro del espectro — y el coste — la unidad contra la que se miden todos los demás patrones. Las otras dos monedas se las lleva casi gratis: su operación es la mínima de la tabla y su superficie de fallo, una sola pieza. El precio no es donde el directo pierde. Pierde en otra parte, y conviene nombrarla con la misma precisión.

## El límite estructural: no hay segunda mirada

Ahora la otra mitad del capítulo, porque un libro que solo cantara virtudes sería un folleto. El límite del directo no es de calibración ni de modelo: es **estructural**. No tiene réplica interna — no se examina a sí mismo — y de ahí derivan todas sus formas de fallo.

La formulación precisa es esta: **en una pasada, la calidad de la cosecha no se verifica, se presupone**. El sistema no sabe si recuperó lo mejor que había, ni si lo recuperado basta para la pregunta, ni si las piezas se contradicen entre sí. Recibe una cosecha y redacta. Cuando el corpus y el acceso están maduros — cuando el dataset áureo del acceso está en verde y el tamiz está afinado —, la presuposición es razonable la mayoría del tiempo. Pero la mayoría del tiempo no es siempre, y el directo no tiene mecanismo para distinguir los dos casos. *No sabe que no sabe*. Esa frase, escrita aquí en cursiva, es la única que hay que memorizar de este capítulo.

Y hay preguntas que una pasada no puede responder *ni con la mejor cosecha posible*, no por falta de calidad sino por geometría: las que necesitan combinar piezas que ninguna búsqueda única recupera juntas, las que requieren operar sobre los datos — sumar, comparar, ordenar por criterios no textuales —, las que dependen de información que no está en el corpus sino en sistemas vivos. Para ellas, ninguna optimización del directo sirve. Son fronteras, no pendientes.

## Los tres modos de fallo, con nombre

Para poder decidir cuándo el directo basta, hace falta un inventario honesto de cómo fracasa. Son tres modos, y conviene darles nombre porque en el capítulo doce el router los buscará en las preguntas antes de encaminarlas.

El primero es el fallo **multi-hop**: la pregunta cuya respuesta requiere encadenar piezas. "¿Qué productos de la línea que compré tienen garantía extendida?" necesita resolver "qué es la línea que compré" y luego consultar las garantías de esa línea. Ningún pasaje del corpus contiene la respuesta completa; cada búsqueda individual recupera piezas de una mitad, y la redacción sobre cualquier mitad produce una respuesta parcial presentada como completa. Es el modo de fallo más frecuente en producción y el más silencioso: la respuesta multi-hop fallida no suena a error, suena a respuesta.

El segundo es la **contradicción no resuelta**: la cosecha trae dos piezas que discrepan — dos versiones de un procedimiento, dos tarifas con fechas distintas, dos protocolos de departamentos diferentes — y el sistema, sin capacidad de replicar, redacta con la contradicción dentro. A veces elige una arbitrariamente (la primera, la más alta, la más reciente), a veces las mezcla en una frase imposible. La respuesta coherente construida sobre evidencia contradictoria es más peligrosa que un fallo evidente, porque no da señales.

El tercero es la **confianza mal calibrada**: la cosecha mediocre — tangencial, parcial, vieja — redactada con la misma seguridad que la excelente. El modelo no modula su tono según la calidad de lo que lee; si no se le diseña para ello, la respuesta sobre una cosecha pobre sale con la misma serenidad tipográfica que la buena. El usuario no puede distinguirlas por el tono, y ahí está el daño: la respuesta mediocre no parece mediocre.

## Cuándo basta, y cómo se sabe

Unidas las virtudes y los fallos, el criterio de uso se puede enunciar con limpieza. El RAG directo es el patrón correcto cuando se cumplen tres condiciones a la vez. **La pregunta es fáctica y acotada** — busca un dato, un procedimiento, una regla, no una síntesis ni un cálculo. **La respuesta vive en una o pocas píldoras** — no requiere encadenar piezas ni operar sobre ellas. **La cosecha del acceso para esa familia de preguntas está demostradamente fiable** — está medido, con dataset, que el tamiz la clava.

Ese triángulo cubre, en las organizaciones reales, una proporción enorme del tráfico: las preguntas de nivel uno del soporte, las consultas de normativa interna, los procedimientos, los datos de producto, los plazos administrativos. No es un resto descartable: es la mayor parte de la cola, y el directo la atiende a un coste que ningún otro patrón puede ofrecer. Por eso la primera obra de arquitectura de la respuesta no es añadir sofisticación — es **medir qué familias de preguntas el directo ya resuelve bien** y declararlo.

Y cómo se sabe que ya resuelve bien: con la vara del segundo libro aplicada a la capa final. Se toma la familia — "preguntas de plazos de portabilidad", "garantías de la línea doméstica", "licencias de formación" —, se construye un puñado de consultas reales con su respuesta correcta, y se mide. Si la tasa de acierto sostiene el contrato de esa audiencia, la familia queda asignada al directo. Si no, o se mejora el acceso — que es muchas veces la respuesta correcta — o se reasigna a un patrón superior. Nada de intuiciones: la asignación es una decisión medida, familia a familia.

El registro también enseña al revés. Cuando el diario de consultas muestra repeticiones — la misma pregunta reformulada tres veces en una semana, la escalada manual a un experto tras una respuesta correcta-en-teoría, la abstención del usuario que no vuelve a preguntar —, el directo está anunciando el borde de su competencia. Esos bordes, escritos, son la materia prima del capítulo siguiente: para las preguntas que una pasada no puede, hace falta recuperar de nuevo.

## La defensa del directo, que nadie hace

Queda una resistencia que este capítulo debe desmontar porque no es técnica: la política. En muchas organizaciones, proponer "el patrón simple" para la mayoría de la cola suena a modestia mal entendida — como si la sofisticación del patrón midiera la seriedad del equipo. El agente impresiona en la demo de dirección; el directo, no. Y así se toman decisiones de arquitectura por efecto de sala, con facturas de tres cifras por consulta donde bastaba una de una.

La defensa del directo se hace con los argumentos que este capítulo dejó medidos, y conviene tenerlos a mano en la reunión correspondiente. Primero: es el patrón **auditable de una página**, y en sectores con regulador, esa página vale más que cualquier demo. Segundo: su fiabilidad no es una promesa sino una **medición con dataset** — la familia asignada lleva su tasa de acierto a la vista, cosa que ningún piloto agéntico puede mostrar el primer mes. Tercero: cada euro de investigación que el router ahorra en la cola fácil es un euro disponible para la cola difícil — defender el directo es financiar el agente, no competir con él. Y cuarto, el más incómodo para el entusiasmo: el directo uniforme ya está en producción en casi todas partes — la única diferencia que este libro propone es que deje de ser una costumbre sin fecha y pase a ser una decisión medida, familia a familia, con su lista y su exclusión.

## El hábito que deja este capítulo

El criterio de salida de esta estación es una lista, y conviene entender su forma porque es el primer documento operativo de la parte práctica del libro: la **lista de familias asignadas al directo**. Cada entrada lleva el nombre de la familia de consultas, la evidencia de su fiabilidad (dataset y tasa de acierto), el contrato de latencia que firma y la fecha de la última medición. Junto a ella, su complementario igual de importante: la **lista de exclusión** — las familias que explícitamente *no* se le asignan, con el motivo que las excluye — un modo de fallo (multi-hop, contradicción) o una frontera geométrica (cálculo, datos vivos).

Estas dos listas son el núcleo de la matriz de decisión del capítulo once: las filas del directo, escritas con evidencia. Y su mantenimiento es el del sistema entero: cada mes, el diario de consultas revisa los bordes, y cada familia migrada a otro patrón — o recuperada para el directo tras mejorar el acceso — deja rastro en la lista. La simplicidad, cuando está medida y declarada, es la decisión de arquitectura más barata y más sólida que existe.

## Preguntas que hace el oficio

**Si el directo es tan bueno, ¿por qué todas las demos venden agentes?** Porque la demo enseña la pregunta difícil, no la cola real. El agente exhibe mejor — planifica, investiga, impresiona —, pero la producción se mide en la cola: miles de consultas fácticas que necesitan segundos, no investigación. La diferencia entre el criterio de la demo y el de la matriz es, precisamente, el tema de este libro.

**¿Un reranker o un híbrido no convierten al directo en otra cosa?** No: lo convierten en un directo mejor. El patrón se define por su forma — una pasada, sin segunda mirada —, no por la calidad de su cosecha. Todo el arsenal del segundo libro — híbrido, topes, rerank — cabe dentro del directo, y de hecho es su condición de fiabilidad: el directo con acceso mediocre es una apuesta; el directo con acceso medido es una decisión.

**¿Puedo empezar el sistema entero por el directo y añadir patrones después?** No solo puedes: es el camino recomendado. El directo obliga a construir y medir el corpus y el acceso — la base de todo —, y sus bordes medidos te dirán, con datos y no con entusiasmo, qué familias necesitan el siguiente peldaño. El error no es empezar por el directo: es quedarse en él sin medir sus bordes, o saltar por encima de él sin base.

**¿Cómo mido "la tasa de acierto" de una familia si no hay verdad de referencia?** Construyéndola: treinta consultas reales de la familia, resueltas a mano por alguien del dominio, con su firma — la regla de la serie: quien resuelve a mano firma la verdad. Es exactamente el dataset áureo del acceso aplicado a la respuesta, y es menos trabajo del que parece: una tarde por familia.

**¿Qué relación tiene el directo con el "chat con documentos" que todo el mundo ha probado?** Es la misma forma — recuperar y redactar en una pasada — con una diferencia de oficio que lo es todo: el chat de catálogo confía; el directo medido comprueba. Fronteras escritas, familia asignada con su tasa de acierto, abstención con cláusula, cita verificada. La forma es la del tutorial; el sistema, no. Y esa distinción es el resumen del libro entero: los patrones son pocos y viejos; lo que los separa de una demo es la arquitectura que los rodea.

!!! note "Criterio de salida"
    La lista de familias de consulta asignadas al RAG directo — cada una con su evidencia de fiabilidad (dataset de consultas reales, tasa de acierto medida, contrato de latencia que sostiene) — y su lista de exclusión con el modo de fallo que excluye a cada familia. Un sistema directo sin listas no es un patrón elegido: es una costumbre sin fecha.

Primera estación recorrida. La segunda trata las preguntas que la pasada única no puede: las que se responden por partes, en varias vueltas, con subpreguntas que el sistema se hace a sí mismo. El iterativo.
