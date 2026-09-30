---
title: 11 · La audiencia decide, el alcance decide
---

# 11 · La audiencia decide, el alcance decide

La pregunta que recorre este capítulo se formula tres veces en una misma jornada, en una empresa de servicios profesionales con su RAG en producción. La plantea, primero, una gestora entre dos reuniones: "¿Qué documentación hace falta para una baja médica de más de quince días?". La plantea, media hora después, un letrado preparando una vista: la misma pregunta, casi palabra por palabra. Y la plantea por la tarde un cliente, por el portal, con la misma intención y otra expectativa.

Tres usuarios, una pregunta, tres respuestas legítimas y distintas. A la gestora: la lista en cuatro puntos y un enlace, en cinco segundos. Al letrado: el razonamiento completo — la norma, la excepción de los quince días, la jurisprudencia de matiz — con cita por afirmación, en un minuto si hace falta. Al cliente: la lista con sus referencias visibles y el aviso de que no constituye asesoramiento — el límite de la quinta cláusula —, en diez segundos. Ninguna de las tres respuestas sirve a los otros dos receptores: la de la gestora le parecería superficial al letrado, y la del letrado, interminable a la gestora.

Un sistema con un solo modo de responder — cualquiera que sea — falla en dos de las tres. No por torpe: por ciego. Le falta la pieza que este capítulo construye: la **matriz de decisión**, el documento que asigna a cada familia de consultas su patrón del espectro en función de quién pregunta, qué alcance tiene la pregunta, qué cuesta equivocarse y cuánta prisa corre el usuario. Es el corazón operativo del libro: el punto donde la tesis — no hay patrón mejor, hay patrones adecuados — deja de ser una frase y se convierte en una tabla firmada.

## Primer eje: la audiencia, que ya tiene contrato

El primer eje de la matriz es *quién pregunta* — y la clave para no convertirlo en una lista de nombres es que la audiencia ya está definida por el trabajo del capítulo tres: **audiencia es quien tiene firmado un contrato de respuesta**. La gestora tiene uno (latencia de segundos, cita mínima, abstención rápida), el letrado tiene otro (exhaustividad, cita por afirmación, tolerancia al minuto), el cliente un tercero (referencias visibles, límites declarados). La matriz no clasifica personas: clasifica contratos. Esa es su primera protección contra el caos — cuando un colectivo nuevo aparece, la pregunta no es "qué patrón le damos" sino "qué contrato firma", y el patrón se deduce.

Y conviene decirlo pronto: los contratos pueden convivir en el mismo sistema sin desplegar nada dos veces. El mismo corpus, el mismo acceso, la misma maquinaria — lo que cambia por audiencia es la estrategia de respuesta que se activa. El sistema multi-audiencia no es dos sistemas: es uno que sabe a quién le habla.

## Segundo eje: el alcance de la pregunta

El segundo eje es *qué pide la pregunta* — su forma, que el capítulo cinco enseñó a diagnosticar y aquí se convierte en escala de cuatro peldaños.

**Fáctica acotada**: la respuesta vive en una o pocas píldoras — un plazo, un procedimiento, una condición. Es el territorio del directo. **Panorama**: la respuesta es la suma ordenada de muchas piezas — un estado del arte, una normativa aplicable, un resumen de mercado. Es el del iterativo, con su presupuesto de vueltas. **Cruzada u operativa**: la respuesta exige combinar el corpus con otra cosa — otro corpus, un cálculo, un sistema vivo, los datos del propio usuario. Es la del agente, cuando el cruce es genuino. **De relación**: la respuesta es un recorrido sobre una red de entidades — la del grafo, cuando la recurrencia y la consecuencia lo justifican.

El diagnóstico del alcance es la parte más mecánica de la matriz y por eso la más fiable: se hace leyendo la pregunta, no opinando sobre ella. Los plurales, los cruces, los "en cuáles de", los verbos de cálculo — las señales del capítulo cinco — son los indicadores del peldaño. Cuando una familia de consultas oscila entre dos peldaños, la matriz lo registra — y el router del capítulo doce vivirá exactamente de esa frontera.

## Tercer eje: la consecuencia del error

El tercer eje es el que ordena el gasto: *qué pasa si la respuesta sale mal*. No qué probabilidad hay de que salga mal — de eso se ocupa la fiabilidad medida — sino cuánto duele cuando ocurre. La escala es del negocio, no del equipo técnico, y conviene fijarla en pocos niveles con ejemplos, porque la vaguedad aquí contamina toda la matriz.

Un nivel es el **error barato**: molesta, se corrige en la siguiente interacción, cuesta minutos — la consulta de catálogo mal respondida que el usuario resuelve probando otra vez. Otro es el **error con coste operativo**: rehace trabajo, retrasa una entrega, obliga a reabrir un expediente — la documentación mal informada que invalida una solicitud. Otro es el **error con coste legal o contractual**: genera una promesa incumplida, un plazo perdido, una cobertura mal informada que alguien ejerció de buena fe. Y hay un nivel techo en cada sector — el error clínico, el error judicial — donde la respuesta incorrecta no tiene precio de corrección aceptable.

La consecuencia del error es el eje que activa las segundas miradas. Donde el error es barato, el directo corre sin escolta. Donde es caro, la réplica interna del capítulo seis paga su seguro — porque una evaluación de cosecha cuesta segundos y céntimos y evita la indemnización. La regla de asignación es de proporcionalidad simple: **el coste del patrón debe escalar con el coste del error, no con el prestigio del patrón**.

## Cuarto eje: la urgencia

El último eje es *cuánto puede esperar* — y también se firma, no se supone. La urgencia tiene dos caras que la matriz separa: la **urgencia del usuario** — está en una llamada, tiene la vista mañana — y la **urgencia del negocio** — la respuesta vale mientras sea oportuna y vale poco después. La primera gestiona la cola; la segunda, el calendario.

La urgencia actúa como desempate de la matriz: cuando audiencia, alcance y consecuencia apuntan a dos patrones, la urgencia decide. Pero actúa también en sentido inverso, y ese caso merece subrayarse porque es el favorito de las demos: hay audiencias cuya urgencia *prohíbe* el patrón que su alcance pediría. El comercial en el teléfono con el cliente delante necesita lo que da el directo, aunque su pregunta sea de panorama: mejor una respuesta parcial inmediata con lo seguro que una completa mañana — y la matriz lo registra como una fila propia, con su compromiso explícito de parcialidad. La matriz no es una jerarquía de bondad: es un catálogo de acuerdos.

## Las filas: familias con evidencia

Los cuatro ejes definidos, la matriz se rellena por **familias de consultas** — el agrupamiento que el capítulo cuatro trajo del segundo libro. Cada fila es una familia real, con sus cuatro valores de eje y su patrón asignado. Unas filas de ejemplo, de sectores distintos, con la forma que debe tener cada una:

La *consulta de nivel uno* — catálogo, plazos, procedimientos — de una operadora: fáctica acotada, error barato, urgencia alta, contrato de segundos: **directo**. El *panorama regulatorio* de un analista: panorama, error con coste de decisión, sin prisa real: **iterativo** con presupuesto de vueltas. La *condición contractual* de un gestor de cartera: fáctica, error legal, urgencia media: **directo con réplica interna** — el evaluador de cosecha pagando su seguro. La *propuesta con cálculo* del comercial de servicios financieros: cruzada u operativa, error legal, urgencia alta del teléfono: **directo parcial en vivo, agente en el informe posterior** — dos filas, un caso real. El *expediente multi-dominio* de la constructora: cruzada, error legal, sin prisa: **multi-agente** con verificador. La *dependencia de proveedores* del fabricante de electrónica: de relación, error con coste de riesgo, recurrente semestral: **grafo**.

Nótese lo que ninguna fila contiene: la tecnología elegida, el proveedor, el modelo. La matriz asigna patrones — posiciones del espectro —, no productos. Es lo que la hace sobrevivir al trimestre siguiente, que en esta industria es la esperanza de vida de cualquier documento técnico.

## La columna de verificación: el contrato

Cada fila de la matriz se contrasta al final con el contrato de su audiencia — y esta operación humilde es la que caza los errores de asignación. La regla: **el patrón asignado debe poder firmar todas las cláusulas del contrato de esa fila**. Si la fila promete cita por afirmación y el patrón asignado no garantiza trazabilidad por píldora, la fila está mal — o el patrón, o el contrato. Si promete tres segundos y el patrón vive en minutos, está mal. La verificación dura una fila un minuto y se hace en voz alta en la reunión donde la matriz se aprueba; las incoherencias que salen ahí valen meses de producción.

Hay un tipo de hallazgo frecuente en esta verificación que merece nombre: la fila imposible — la combinación de cláusulas que ningún patrón firma. "Cita por afirmación en tres segundos para un panorama" no existe en el espectro de este libro ni en el de ninguno. Las filas imposibles no se fuerzan: se negocian — baja el grado de cita, o sube la latencia, o cambia el alcance servido —, y la negociación ocurre en la reunión de la matriz, con el negocio delante, y no en producción, con el usuario delante.

## La matriz como documento vivo

La matriz se aprueba una vez y se mantiene siempre. Su mantenimiento es el del triaje semanal que el segundo libro instituyó: los hallazgos del diario de consultas — familias nuevas que nadie asignó, filas que el router marca como dudosas, errores de encaminamiento — entran por una pata, y las salidas son filas nuevas, filas corregidas y filas retiradas con su fecha. Cada fila lleva, como toda pieza seria de esta serie, su procedencia: qué evidencia la creó — el dataset que midió su fiabilidad, la factura que midió su coste, el incidente que subió su consecuencia.

Tres errores de bautizo que conviene evitar al construirla por primera vez. Asignar por tecnología — "ya tenemos agente, busquemos preguntas para el agente" — es el error número uno, y la matriz existe precisamente para impedirlo: las filas nacen de las preguntas, no de las piezas. Crear mil filas es el segundo: las familias se cuentan por decenas, no por cientos; si una familia no tiene contrato, consecuencia ni volumen distinguibles, no es una familia. Y dejar filas sin evidencia es el tercero: una fila apoyada solo en la intuición de quien la propuso se marca como provisional y se mide en el primer mes — la matriz admite hipótesis, no admite incógnitas disfrazadas de decisiones.

## Preguntas que hace el oficio

**¿Y si una pregunta puede resolverse con dos patrones? ¿Cuál gana?** El más barato que firma el contrato — siempre. La ambigüedad entre patrones casi nunca es real: viene de no haber valorado la consecuencia del error o de tener dos audiencias mezcladas en una fila. Si tras separar audiencias la duda persiste, la fila se marca provisional, se sirve con el barato y se mide un mes: las señales de insuficiencia — reencaminamientos, repeticiones, quejas — decidirán con datos.

**¿La matriz no es burocracia? ¿No puede el modelo decidir solo?** La matriz es lo contrario de burocracia: es la decisión tomada una vez en frío, con evidencia y con firma, en lugar de mil veces en caliente sin ninguna de las dos. Lo que decide solo — el modelo, el router — ejecuta la matriz; si no hay matriz, ejecuta su entrenamiento, y el entrenamiento no sabe cuánto cuesta ni qué contrato tiene tu gestora.

**¿Con qué frecuencia se revisa?** En el triaje semanal entra material; la revisión formal — filas que cambian de patrón, contratos renegociados — es mensual o por incidente. La regla: cualquier decisión de asignación que cambie en caliente y sobreviva dos semanas se convierte en cambio de fila con su evidencia. Si no sobrevive, era una fluctuación, y las fluctuaciones no reescriben la matriz.

**¿Qué pasa con las familias nuevas que nadie previó?** Es el estado natural: toda familia nueva nace provisional. El router la encamina por sus señales al patrón más plausible, el panel vigila su primera semana, y en el triaje recibe su fila — con evidencia o con fecha de medición. La matriz que no admite provisionales obliga a clasificar sin datos, que es cómo se fabrican las asignaciones equivocadas.

**¿Cómo presento la matriz al negocio sin que parezca jerga técnica?** Traduciendo sus columnas a las preguntas del negocio, en su orden: ¿quién pregunta y qué le prometimos?, ¿qué clase de pregunta es?, ¿cuánto duele equivocarse?, ¿cuánta prisa corre?, y solo al final — y como consecuencia de las anteriores — con qué instrumento se responde. La reunión de aprobación no menciona ningún nombre de patrón hasta la última columna; si el negocio discute la tecnología antes que el contrato, la reunión se perdió. La matriz bien presentada es un documento de servicio, no de ingeniería: los nombres de los patrones son detalles de implementación de decisiones que el negocio ya tomó en su idioma.

!!! note "Criterio de salida"
    La matriz de decisión completada y aprobada: una fila por familia de consultas con sus cuatro ejes valorados (contrato de audiencia, alcance diagnosticado, consecuencia del error en escala de negocio, urgencia), el patrón asignado, la verificación de que el patrón firma el contrato, y la evidencia o el carácter provisional de cada fila. La matriz es el documento más importante del libro: es donde la tesis — ningún patrón es mejor, cada uno sirve a su pregunta — deja de ser una opinión.

La matriz decide en frío. Pero las preguntas no llegan en frío: llegan en vivo, una a una, sin etiqueta de familia. Algo tiene que leer cada pregunta y aplicarle la matriz en el momento. Ese algo es el router — el capítulo que cierra la parte de la decisión.
