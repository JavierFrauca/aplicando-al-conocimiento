---
title: 13 · Cómo redacta el modelo
---

# 13 · Cómo redacta el modelo

La respuesta que da título a la escena de este capítulo constaba de trece palabras: "No consta en el corpus; lo más cercano es este criterio de 2023". La formuló un sistema de consulta normativa interna de una consultora de farmacovigilancia, ante una pregunta sobre una guía técnica que se había publicado esa misma semana y que todavía no había entrado en el corpus. El usuario — un responsable de calidad con veinte años de oficio — leyó la abstención, no la tomó como un fallo sino como una señal, y fue a verificar la fuente por su cuenta. Encontró la guía, encontró además que cambiaba un criterio que su equipo estaba aplicando de forma inversa, y evitó con esa media hora lo que habría sido un desvío de meses. La abstención no fue el final de la respuesta: fue el principio del hallazgo.

En el diario del sistema quedó registrado el otro desenlace posible, y conviene mirarlo porque es el desenlace por omisión: la pregunta hecha a un sistema sin cláusula de abstención habría recibido una redacción fluida sobre la cosecha parcial — lo más cercano, el criterio de 2023 — presentada como vigente. Fluida, citada, equivocada. La diferencia entre las dos escenas no es el modelo — era el mismo — ni el corpus: es la **instrucción de redacción**. Este capítulo trata de esa instrucción, que merece el estatus que esta serie da a sus piezas: un contrato. El prompt de generación es el contrato que el equipo firma con el modelo sobre cómo se escribirán todas las respuestas.

## El contrato de generación: papel, material, prohibiciones

Todo prompt de generación serio tiene tres secciones, y la disciplina de escribirlas como contrato — versionadas, revisadas, probadas — es lo que separa la redacción diseñada de la heredada.

La primera sección define el **papel**: qué es el modelo en esta conversación. No un role-play decorativo — "eres un experto mundial" es teatro que además alienta la sobreconfianza — sino una definición funcional de su posición: "Redactas respuestas para [audiencia] a partir exclusivamente del contexto que se te entrega. No tienes conocimiento propio aplicable: todo lo que digas debe venir del contexto". El papel incluye la relación con la verdad: el modelo no sabe — recita lo que el corpus le entrega —, y la instrucción que lo deja claro cambia su comportamiento de forma medible: baja la extrapolación, sube la fidelidad.

La segunda define el **material**: qué recibe, con qué estructura y qué puede esperar de él. Las píldoras llegan con su texto, su referencia y sus metadatos — dominio, vigencia, tipo —, y el contrato lo explicita: "Cada bloque de contexto lleva su identificador de fuente; úsalo para citar", "Los bloques marcados como caducados no son válidos para responder", "Si dos bloques contradicen, expón la discrepancia, no elijas". Esta sección es la que conecta la redacción con todo el trabajo del acceso: el modelo no puede citar lo que no llega etiquetado.

La tercera define las **prohibiciones** — y es la más contraintuitiva, porque prohibir a un modelo de lenguaje es contravenir su entrenamiento entero. No añadas conocimiento externo. No completes lo que el contexto deja a medias. No redondees cifras. No unifiques versiones distintas. No respondas preguntas que el contexto no cubre: di que no están cubiertas. Cada prohibición existe porque su incumplimiento existe — son el inventario de los tres modos de fallo del capítulo uno traducidos a instrucciones —, y la plantilla que no tiene prohibiciones es una plantilla que no ha conocido producción.

## Citas obligatorias: ensamblar, no componer

De todas las instrucciones de generación, la que más cambia el carácter de las respuestas es la de **cita por afirmación**: cada dato concreto de la respuesta remite a la píldora que lo respalda. No es una recomendación de estilo — es una reconstrucción de la naturaleza del texto: obliga al modelo a *ensamblar* en lugar de *componer*.

La diferencia se ve en un fragmento. Redacción compuesta, la natural del modelo: "El plazo de respuesta es de 20 días hábiles, ampliables en casos complejos". Redacción ensamblada, la que exige el contrato: "El plazo de respuesta es de 20 días hábiles [fuente 2], ampliables en expedientes complejos según el procedimiento de ampliación [fuente 5]". El segundo texto es peor literatura y mejor ingeniería: cada afirmación es comprobable, cada comprobación lleva a un documento, y el fallo del capítulo catorce — la cita que no dice lo que se dice que dice — se vuelve detectable por máquina. La serie ya viene diciendo que la respuesta es la verdad del corpus contada; la cita obligatoria es la forma técnica de esa ética.

Hay una objeción frecuente que conviene responder porque está bien intencionada: que la cita ensucia la lectura, sobre todo para audiencias no expertas. La respuesta es de arquitectura, no de principiísmo: el grado de cita es una cláusula del contrato de audiencia — el capítulo tres lo dijo — y la plantilla lo implementa. La respuesta del portal de clientes lleva las referencias discretas, al pie; la respuesta del expediente técnico las lleva inline. Lo que no existe en ningún contrato serio es la cláusula "sin citas": existe "con citas visibles" y "con citas disponibles", y la diferencia entre ambas es de presentación, nunca de existencia.

## El formato como cláusula de audiencia

La plantilla de generación no es una: es una por contrato de audiencia. La estructura de la respuesta — su longitud, su forma, qué va primero, qué va al pie — sigue la misma lógica que el resto del contrato: sirve a quien pregunta.

La audiencia de segundos recibe estructura de dato: la respuesta en la primera línea, el desarrollo después, la referencia al final. Es la plantilla que respondía a la gestora del capítulo once. La audiencia de exhaustividad recibe estructura de razonamiento: la pregunta reformulada, el criterio aplicado, la cadena normativa con sus citas, las excepciones. Es la del letrado. La audiencia externa recibe, además, los límites declarados: qué es información del sistema y qué no constituye asesoramiento. Tres plantillas, un mismo modelo, un mismo corpus — lo que cambia es la sección de formato, y esa diferencia es tan arquitectónica como cualquier otra del libro.

El formato también lleva sus prohibiciones propias, aprendidas a base de incidentes: sin introducciones de cortesía — la audiencia de segundos mide la respuesta en líneas y cada "¡Claro que sí!" le sale caro —, sin repeticiones del contexto, sin conclusiones que vayan más allá de lo citado, sin preguntas retóricas que simulen diálogo donde hay un informe. El formato es donde el entusiasmo del modelo se hace visible primero; la plantilla es donde se contiene.

## La abstención: el "no lo sé" bien dicho

La instrucción más importante de todo el contrato de generación es la que activa la cuarta cláusula del capítulo tres: **cuándo y cómo el sistema dice que no lo sabe**. Su diseño tiene las tres partes que ya se anunciaron, y aquí se especifican como instrucción.

La **condición** es una regla que el modelo puede evaluar sobre el material: "Si el contexto no contiene información suficiente para responder la pregunta de forma respaldada, no respondas". Nótese la asimetría deliberada: el umbral es la falta de respaldo, no la falta de respuesta. El modelo no juzga si "sabe" — no sabe nada —; juzga si lo que recibió alcanza para afirmar con respaldo. Con esa redacción, la abstención es verificable: o hay bloques que sostienen cada afirmación o no los hay.

La **forma** es lo que distingue una abstención útil de un muro. El contrato no dice "no lo sé" y punto — dice qué falta y qué hay: "El corpus no contiene información sobre [el tema preguntado]. Lo más cercano que contiene es [X], que trata [aspecto relacionado]". La abstención útil orienta: le dice al usuario dónde está el borde del conocimiento, y convierte el "no" en un mapa. La escena de apertura de este capítulo funcionó precisamente por esa forma: la abstención nombró lo que sí había — el criterio de 2023 — y el usuario pudo ir más allá.

El **registro** es la parte que la serie más defiende: cada abstención queda anotada con su pregunta, y entra en el triaje semanal como la señal más limpia que un sistema puede producir — lo que la gente pregunta y el corpus no sabe. La abstención bien diseñada no es un fracaso del sistema: es su mejor sensor. El capítulo diecisiete le dará destino formal; aquí basta el principio: ninguna abstención se desperdicia.

## El peligro de la cortesía: fluido no es verdadero

Queda el adversario de fondo de toda esta arquitectura, y conviene presentarlo con nombre: el entrenamiento del modelo, que lo ha preparado para agradar. Un modelo de lenguaje llega a la plantilla con hábitos de conversador ideal — nunca duda, nunca molesta con matices, siempre tiene algo que decir, y lo dice con una serenidad tipográfica que no significa nada. En un chat de charla, esos hábitos son cortesía. En un sistema de conocimiento, son riesgo operativo: la fluidez presta credibilidad a lo que no la ha ganado.

La plantilla contraataca donde el hábito se expresa. Contra la no-duda: instrucciones explícitas de marcar la incertidumbre — "si el contexto es parcial para un aspecto, di qué parte está respaldada y cuál no" —, que producen la respuesta honesta en dos niveles, muy superior a la respuesta uniforme. Contra la complacencia: las prohibiciones de la tercera sección. Contra la dicción de autoridad — el tono enciclopédico que sugiere saber —: la regla de que la seguridad del texto la da la cita, no el tono; el usuario que ve referencias sabe que puede comprobar, y el usuario que no las ve también aprende qué significa.

Hay además una decisión de expectativas que conviene tomar una sola vez y defender: **el sistema no imita a una persona**. No tiene opiniones, no empatiza, no consuela, no usa la primera persona de la intimidad. Trata al usuario como lo que es la relación — un profesional consultando un instrumento —, y esa sobriedad no es frialdad: es el tono correcto para un texto cuya única ambición es decir la verdad del corpus y dejar que se compruebe.

## La misma pregunta, tres plantillas

Nada enseña el trabajo de la plantilla como verla actuar. Toma la pregunta de un gestor de flota — "¿cada cuánto hay que pasar la revisión técnica de un vehículo de más de 3.500 kilos?" — y déjala caer en las tres plantillas del mismo sistema, con el mismo contexto: la ficha del reglamento, la tabla de intervalos y la excepción para uso agrícola.

La plantilla de la **audiencia de segundos** — el gestor con el teléfono — responde en tres líneas: "Cada 12 meses o 100.000 km, lo que antes ocurra. Excepción: los vehículos de uso agrícola, cada 24 meses. Fuente: reglamento de inspecciones, art. 14 y tabla anexa." Sin saludo, sin contexto, sin matices que no se pidieron — el dato primero, la excepción necesaria, la referencia al final. La plantilla de la **audiencia de exhaustividad** — el jefe de taller — añade la capa que su contrato exige: el fundamento ("el intervalo se aplica desde la primera matriculación, no desde la puesta en servicio — art. 14.2"), la diferencia entre versiones del reglamento si el corpus tiene historia, y la regla práctica de cómputo. Y la plantilla de la **audiencia externa** — el portal de clientes — además de la referencia visible, lleva la cláusula de límites ("información del reglamento vigente; no constituye asesoramiento sobre su caso concreto").

Tres respuestas, un corpus, un modelo. La diferencia no está en la inteligencia de ninguna: está en la instrucción — qué papel, qué formato, qué grado de cita, qué límites. Es la demostración operativa del capítulo once: la audiencia decide, y la plantilla ejecuta lo decidido. Cambiar de respuesta no exige cambiar de sistema; exige tener la plantilla de cada contrato versionada y a mano — y saber qué plantilla tocó cada pregunta. Esa última pieza, de nuevo, es el router.

## La plantilla como documento vivo

El contrato de generación termina siendo lo que toda la serie hace con sus piezas decisivas: un documento **versionado, probado y desplegado con ceremonia**. Versionado, porque cambiar una frase de la plantilla cambia todas las respuestas del sistema — es un despliegue, no un retoque, y lleva su fecha y su autor. Probado, porque toda modificación pasa por la regresión del capítulo quince: el lote de consultas con respuestas de referencia se ejecuta antes y después del cambio, y las diferencias se examinan — todas, no solo las que parecen peores. Y desplegado con ceremonia, porque la plantilla es una de las pocas piezas del sistema cuyo cambio es invisible hasta que duele: no hay caída de servicio, no hay error en el panel — hay respuestas ligeramente distintas, durante semanas, hasta que alguien nota que la abstención dejó de abstener.

El hábito que deja este capítulo es entonces de disciplina industrial: **toda edición de la plantilla es una hipótesis con prueba**. "Si cambiamos X, las respuestas harán Y" — se escribe, se prueba contra la regresión, se despliega con su versión. Las plantillas que crecen por acumulación de parches de urgencia — la instrucción que arregla un caso y rompe cinco — son el equivalente exacto del corpus sin manifiesto del primer libro: funcionan hasta que dejan de funcionar, y entonces nadie sabe por qué.

## Preguntas que hace el oficio

**¿Más instrucciones hacen plantillas mejores?** Más largas, no mejores. La plantilla crece por prohibiciones con causa — cada regla responde a un incidente real — y se poda por coste: cada instrucción compite por la atención del modelo, y las redundantes diluyen las críticas. La plantilla madura se lee en dos minutos y cada una de sus reglas tiene historia. Si una regla no puede contar la suya, es sospechosa de decoración.

**¿La plantilla no debería dejar al modelo "ser creativo"?** En un sistema de conocimiento, la creatividad del redactor es otro nombre para la invención. Lo que sí se diseña es el estilo: claridad, estructura de la audiencia, economía. Eso no se consigue pidiendo "sé claro" — se consigue con la estructura del formato: dato primero, razonamiento después, citas en su sitio. La forma disciplina la prosa mejor que el adjetivo.

**¿Cada modelo necesita su propia plantilla?** Las secciones de papel, material y abstención son portables; las de formato y tono, casi siempre ajustables. El cambio de modelo es, por tanto, un despliegue de plantilla con su regresión completa — no un intercambio de piezas transparente. Los equipos que cambian de modelo "a ver qué pasa" están haciendo experimentación en producción con usuarios que no se apuntaron.

**¿Quién aprueba los cambios de plantilla?** El mismo mecanismo que todo cambio de la capa final: hipótesis escrita, regresión aprobada, despliegue graduado. La diferencia con otros cambios es la visibilidad — un cambio de plantilla altera todas las respuestas a la vez —, y por eso su ceremonia es más estricta, no más laxa.

**¿Dónde vive la plantilla — junto al código, con los documentos?** Como documento versionado con el resto de las piezas de arquitectura: con historial, autor, fecha y prueba de cada versión. La tentación del primer trimestre es tenerla en un archivo de configuración sin dueño; el resultado, a los seis meses, es la plantilla fosilizada que nadie se atreve a tocar porque nadie sabe qué romperá. El criterio práctico: si cambiar una frase de la plantilla no tiene el mismo proceso que cambiar una fila de la matriz — hipótesis, regresión, despliegue —, la plantilla no está gobernada, está depositada.

!!! note "Criterio de salida"
    Las plantillas de generación versionadas — una por contrato de audiencia — con sus tres secciones completas (papel con relación a la verdad, material con estructura de las píldoras, prohibiciones con su regla de abstención en condición, forma y registro), y su proceso de cambio probado: toda edición pasa la regresión de respuesta antes de desplegarse, con la hipótesis del cambio escrita. Una plantilla sin versión ni prueba no es un contrato: es una superstición.

La plantilla obliga a ensamblar, pero obligar no es garantizar. La siguiente pregunta es la del control: ¿dice la respuesta lo que el corpus dice? La fidelidad — la vara de la serie aplicada a la capa final — es el capítulo catorce.
