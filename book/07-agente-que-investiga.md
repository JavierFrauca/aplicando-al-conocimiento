---
title: 7 · El agente que investiga
---

# 7 · El agente que investiga

Una empresa de reparto con una flota de ciento veinte vehículos recibe cada trimestre la misma tarea, y cada trimestre la resuelve igual: una persona experta dedica media tarde a cruzar la lista de vehículos — de la base de datos operativa, con sus fechas de matriculación y sus últimos servicios — contra la normativa de inspección técnica de cada territorio donde circulan — en el corpus documental, con sus plazos y sus excepciones —, calcular qué vehículos toca inspeccionar y con qué coste estimado según tarifas vigentes, y redactar un informe para dirección. La tarea no es difícil para un experto: es tediosa, estructurada y propicia al descuido en la semana en que además hay que hacer todo lo demás.

Ninguna de las tres estaciones anteriores del espectro puede con ella. El directo recupera pasajes de normativa y de paso: no consulta la base de datos operativa ni calcula. El iterativo puede encadenar recuperaciones, pero todas contra el mismo corpus: no sabe sumar ni consultar un sistema vivo. El autocorrectivo verifica la cosecha, pero su cosecha sigue siendo solo texto. La pregunta exige cuatro capacidades que hasta ahora eran de las personas: planificar, consultar fuentes heterogéneas, operar sobre los datos — calcular, comparar, ordenar — y volver sobre el plan cuando un hallazgo intermedio lo cambia.

La cuarta estación del espectro, el **RAG agéntico**, es el patrón que traslada esa capacidad al sistema. Su idea central invierte la arquitectura de todo lo anterior: la recuperación del corpus deja de ser el centro del sistema y se convierte en **una herramienta más** al servicio de un planificador. El sistema deja de responder y empieza a investigar.

## El salto de jerarquía: del pipeline al planificador

Todos los patrones anteriores comparten una forma: un pipeline — una secuencia de etapas fijas que la pregunta atraviesa siempre igual. La variación entre patrones era cuántas etapas y cuántas vueltas, pero el recorrido estaba prediseñado. El agente rompe la forma: no hay recorrido fijo, hay un **plan** que se construye para cada pregunta y se corrige durante la ejecución.

Un agente, dicho con precisión de arquitectura, es la combinación de tres órganos. El primero es la **planificación**: la capacidad de descomponer el objetivo en pasos — "primero necesito la lista de vehículos, luego la normativa de cada territorio, luego las tarifas, luego el cálculo" — y de revisar el plan cuando la ejecución enseña algo nuevo. El segundo son las **herramientas**: las capacidades externas que el plan puede invocar — buscar en el corpus, consultar la base de datos, calcular, obtener fechas, llamar a una API. El tercero, el que distingue un agente de un simple encadenamiento de llamadas, es la **evaluación del progreso**: en cada paso, el sistema se pregunta qué sabe ya, qué le falta y si el plan sigue siendo el correcto. Esa pregunta en bucle es lo que permite que el hallazgo sorpresa — "esta comunidad autónoma exime del tramo urbano" — modifique el resto del trabajo en lugar de pasar desapercibido.

Conviene fijar la frontera con lo anterior porque el marketing la difumina constantemente: un pipeline con tres llamadas seguidas no es un agente, y un agente no es "un LLM con acceso a internet". Agente es el bucle completo — plan, herramientas, evaluación — gobernado por el objetivo de la pregunta y no por un diagrama preimpreso.

## Las herramientas: el recuperador, degradado con elegancia

La novedad estructural del agente es que la búsqueda en el corpus pierde su trono. Sigue siendo crucial — los dos libros anteriores no pierden vigencia: siguen siendo la fuente de la verdad normativa — pero su papel cambia: es la herramienta que responde "¿qué dice el corpus sobre X?", al mismo nivel que la herramienta que responde "¿qué vehículos hay en la flota?" o "¿cuánto es el 34 por ciento de 1.240 euros?".

Esa degradación con elegancia tiene una consecuencia de diseño que separa los agentes serios de los juguetes: **cada herramienta necesita su propio contrato**, del mismo tipo que el contrato de respuesta del capítulo tres, pero en miniatura. Qué recibe la herramienta — con qué formato y qué límites. Qué devuelve — y en qué condiciones devuelve "no encontrado" en lugar de inventar. Qué no puede hacer — la herramienta de base de datos lee lo que sus permisos permiten y nada más. Y qué coste tiene — en latencia y en dinero, porque en el bucle del agente las herramientas se llaman muchas veces.

El catálogo típico de herramientas de un agente de conocimiento ilustra el abanico. El **buscador del corpus** — toda la maquinaria del libro anterior, empaquetada. El **consultor de sistemas operativos** — una consulta parametrizada a la base de datos del negocio, de solo lectura. La **calculadora** — humilde y decisiva: los modelos de lenguaje suman mal, y toda aritmética seria va por ella. El **consultor de calendario y fechas** — porque "este trimestre" hay que resolverlo contra una fecha real. Y el **parser de documentos** — porque el contrato o la factura que el usuario adjunta hay que convertirlo en datos antes de operar con ellos.

La regla de oro del catálogo: pocas herramientas, bien contratadas, mejor que muchas herramientas vagas. Cada herramienta vaga — "busca información" sin definir dónde ni cómo — es una decisión que el planificador tendrá que improvisar, y la improvisación es el enemigo declarado de este libro.

## ReAct: razonar, actuar, observar

El ciclo que gobierna la ejecución tiene nombre propio — **ReAct**, de *reason* y *act* — y se entiende mejor recorriéndolo sobre el caso de la flota, paso a paso, como lo dejaría escrito la trazabilidad del sistema.

**Razona**: "Necesito tres cosas: la lista de vehículos con sus fechas de matriculación, la normativa de inspección de cada territorio donde circulan, y las tarifas vigentes. Empiezo por la lista". **Actúa**: invoca el consultor de la base de datos operativa. **Observa**: ciento veinte vehículos en nueve territorios; dos vehículos dados de baja el mes pasado — dato que el plan no esperaba. **Razona de nuevo**: "Los de baja salen del cálculo; la normativa la consulto por territorio, nueve consultas al corpus". **Actúa**: nueve invocaciones del buscador. **Observa**: ocho territorios con plazos claros; uno con una exención para vehículos urbanos menores de cierta carga. **Razona**: "Aplico la exención a los ocho vehículos que corresponden; necesito las tarifas y calculo por vehículo". **Actúa**: recupera tarifas, invoca la calculadora por grupo. **Observa**: un total, con su desglose. **Razona**: "Tengo todo: lista, normativa aplicada, exenciones justificadas, coste. Redacto el informe".

Nueve o diez pasos, dos o tres minutos, y algo que ninguna estación anterior producía: una **trazabilidad paso a paso legible** — cada hallazgo, cada decisión, cada herramienta, con su momento. Cuando dirección pregunte por qué salen ciento diez vehículos y no ciento veinte, la respuesta está en los pasos donde la observación detectó las bajas y donde el territorio con la exención apartó a los ocho urbanos. No es un detalle de lujo: es la condición de supervivencia del patrón en producción, y vuelve enseguida.

## La entrevista al caso: qué preguntar antes de construir el agente

Los agentes se mal construyen casi siempre por el mismo motivo: se diseña el agente antes de entender el caso. La entrevista previa — media hora con quien hoy resuelve el caso a mano — deja cinco respuestas que valen el diseño entero.

Primera: **¿cuáles son los pasos que das siempre?** El experto los tiene internalizados y los dicta sin darse cuenta; son el esqueleto del plan. Segunda: **¿qué sistemas abres, y en qué orden?** Es el catálogo de herramientas real, no el deseado — y revela las herramientas que faltan. Tercera: **¿qué te hace cambiar de camino a mitad?** Los puntos de decisión — el hallazgo que reordena el plan — son exactamente los que el bucle de evaluación del progreso debe capturar; si el experto dice "nunca cambio de camino", quizá no hay agente: hay pipeline con pasos largos, que es más barato. Cuarta: **¿cómo sabes que ya acabas?** La señal de parada del experto — "cuando el cuadre da" — traducida a criterio verificable es la señal de parada del sistema. Quinta: **¿qué revisas antes de firmar?** Es la lista de verificación del paso final, y a menudo esconden ahí las comprobaciones que el agente no debe automatizar jamás.

Un detalle de la entrevista que los equipos saltan y luego pagan: preguntar por los casos que **se negaron a resolver**. Todo experto tiene su lista de preguntas que no respondía ni pagado — las que exigen criterio, contexto o responsabilidad fuera del alcance del caso. Esa lista es el borde ético del agente: lo que declarará "esto necesita a una persona" antes de intentar improvisarlo.

## MCP: el enchufe común de las herramientas

Un apunte de infraestructura que este libro trata como trata todo lo que cambia rápido: como electricidad, no como religión. Para que un agente use herramientas, alguien tiene que conectarlas — definir cómo el sistema descubre qué herramientas existen, con qué firma se invocan y qué devuelven. Ese problema, resuelto antes en cada casa con pegamento propio, tiene desde hace poco un estándar abierto: **MCP**, el protocolo que define el enchufe común entre modelos y herramientas.

Lo que importa del estándar para esta serie no es su sigla sino su consecuencia de arquitectura: las herramientas se convierten en piezas **reutilizables y reemplazables**. El conector de la base de datos de flota que hoy sirve al agente de inspecciones servirá mañana al de mantenimiento; el buscador del corpus, empaquetado una vez, se expone igual a cualquier agente. Y lo que no cambia con el estándar — porque no cambia nunca — es lo que este capítulo ya estableció: el contrato de cada herramienta, sus permisos, sus límites y su observabilidad. El enchufe común no contrata las herramientas: solo las enchufa.

Un aviso de seguridad que aquí cobra cuerpo: las herramientas son permisos. Un agente con una herramienta de escritura en la base de datos es un proceso automático con llaves de la casa — y el principio que el capítulo dieciséis formalizará se anuncia aquí: **el agente hereda los permisos de quien pregunta, jamás más**. La herramienta de flota que lee todo para el director de operaciones debe leer solo su flota para el jefe de taller.

## El precio completo: minutos, facturas y cascadas

La factura del agente tiene tres partidas que conviene contabilizar por separado, porque cada una se gestiona distinto.

La **latencia** se multiplica por pasos: cada paso del ReAct es una llamada al modelo con todo el historial acumulado — y el historial crece con cada paso, de modo que los pasos tardíos son más caros y más lentos que los tempranos. Un agente de diez pasos son minutos, no segundos: es un contrato de latencia de otra categoría, y por eso este patrón jamás compite en la cola masiva.

El **coste** sigue la misma curva que la latencia, con un agravante: los pasos tardíos procesan más contexto. Un agente mal controlado puede gastar en una investigación lo que el directo gasta en cien respuestas. El control son dos presupuestos que se heredan del capítulo anterior y aquí se vuelven obligatorios: presupuesto de pasos y presupuesto de tokens por investigación, con salida digna cuando se agotan — el informe parcial con lo que se sabe, marcado como parcial.

Y la **superficie de fallo**, que es la partida más difícil. El modo de fallo característico del agente es la **cascada**: un paso intermedio equivocado — una consulta SQL con un filtro mal puesto, una exención normativa leída al revés — no produce un error local sino una investigación entera envenenada, porque todos los pasos siguientes se construyen sobre el equivocado. Y lo peor de la cascada es que la respuesta final no da señales: sale coherente, fluida y equivocada, con la seguridad adicional de haber "investigado".

Contra la cascada no hay un antídoto sino tres, y los tres son obligatorios. La **verificación de pasos críticos** — las observaciones que alimentan decisiones estructurales se comprueban, con el mismo espíritu de la réplica interna del capítulo seis. La **trazabilidad paso a paso persistida** — no para decorar, sino para que un humano pueda auditar la investigación cuando la consecuencia lo pida. Y la **regla de la persona al final del bucle**: en las decisiones de alta consecuencia, el agente entrega el informe y el cálculo, pero la decisión la firma quien responde — el agente investiga, la persona asume.

## Cuándo el agente es la respuesta — y cuando no

El criterio de asignación se puede enunciar corto. El agente es el patrón correcto cuando la pregunta exige **operar**, no solo recuperar: calcular, comparar, cruzar el corpus con sistemas vivos, o encadenar hallazgos donde cada uno cambia el siguiente paso. Y cuando su frecuencia es baja y su consecuencia alta — el informe trimestral, el caso complejo, el análisis que hoy consume a un experto una media tarde.

Y cuando no: en el volumen, la rutina y la prisa. El agente aplicado a la cola masiva es la fotografía exacta del despilfarro — minutos y euros donde bastaban segundos y céntimos —, y además frágil: más pasos, más cascadas. El agente que se instala "porque es el futuro" y atiende todo lo que llega es el error de arquitectura más costoso de este espectro. El futuro, como todo el libro sostiene, no es un patrón: es la matriz que asigna cada pregunta al patrón justo — y el capítulo doce pondrá al router a hacer ese reparto en vivo, con el agente como última estación y no como primera.

## Preguntas que hace el oficio

**¿Necesito un agente para dar acceso a una calculadora junto a la búsqueda?** Probablemente no: eso es un directo con una herramienta fija — un pipeline con dos pasos, valioso y barato. El agente se justifica cuando el *orden y la necesidad* de los pasos no se conocen por adelantado: cuando una pregunta necesita tres herramientas y otra ninguna, y decidir cuáles corresponde al sistema en vivo. Herramientas fijas en un pipeline fijo = directo extendido; plan en vivo = agente.

**¿Qué pasa si una herramienta falla a mitad de la investigación?** Lo que su contrato diga — y por eso el contrato de cada herramienta incluye su condición de fallo: la búsqueda devuelve "sin resultados", la base de datos devuelve su error tipado, la API su tiempo de espera. El agente serio trata el fallo de herramienta como observación, no como excepción: lo registra en la trazabilidad, decide — reintentar, usar la vía alternativa, o terminar con informe parcial — y sigue. Lo inaceptable es el agente que se detiene en silencio o que redacta como si nada hubiera pasado.

**¿Cuántas herramientas es "muchas"?** Las que el catálogo puede mantener con contrato, permisos y salud vigilada. En la práctica, los agentes serios viven con cinco o diez bien contratadas; los que arrastran cincuenta vagas gastan su contexto en decidir y su crédito en cascadas. La regla del capítulo ocho aplica igual aquí: cada herramienta necesita un caso que la justifique.

**¿El usuario ve el plan?** En la audiencia de exhaustividad, sí — el plan visible es parte de la honestidad del contrato de minutos, y su primera versión enseña al sistema qué planes se descarrilan. En la audiencia de segundos, el agente no debería estar: es la señal del router de que algo se encaminó caro.

!!! note "Criterio de salida"
    El agente operando con las tres piezas mínimas por escrito: el catálogo de herramientas con su contrato (entrada, salida, condición de "no encontrado", permisos, coste); los presupuestos de pasos y tokens con su salida digna; y la trazabilidad paso a paso persistida y legible para cada ejecución. Un agente sin trazabilidad no es un investigador: es una caja negra cara.

Cuarta estación recorrida. La quinta replica fuera lo que el agente hace dentro: cuando el caso excede a un investigador — aunque el investigador sea bueno —, hace falta un equipo.
