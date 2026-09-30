---
title: 9 · Cuando la relación importa: GraphRAG
---

# 9 · Cuando la relación importa: GraphRAG

El director de compras de un fabricante de electrónica hace cada semestre la misma pregunta a su equipo, y el equipo tarda cada vez más en responderla, porque la cadena de suministro crece: "¿Qué proveedores de nuestros proveedores dependen de un único fabricante de chips?". La pregunta no es difícil por la información — todo está en el corpus: contratos, certificados de origen, comunicaciones de riesgo, notas de proveedor —. Es difícil por la *forma*: la respuesta no está escrita en ningún documento. Es un recorrido. Del fabricante a sus proveedores, de cada proveedor a sus componentes, de cada componente a los fabricantes de chip que lo nutren, y el conteo: los que dependen de uno solo.

Búsquese esa pregunta en el mejor motor semántico del mundo y devolverá pasajes parecidos a "proveedores", "chips", "dependencia" — documentación útil, ninguna respuesta. Porque la similitud vectorial, todo el músculo de los capítulos anteriores, compara *textos con textos*: encuentra el pasaje que suena como la pregunta. Pero esta pregunta no suena a nada: es una estructura. Para responderla hay que recorrer relaciones — quién es de quién, qué depende de qué — y los recorridos no viven en los pasajes.

Ese es el territorio del único patrón que no ocupa una estación del espectro — es su eje transversal —: **GraphRAG**, el que añade al corpus una tercera forma de guardar el conocimiento. El primer libro dejó el corpus en dos representaciones hermanas — el texto para leer y el vector para buscar, el doble campo. GraphRAG añade la tercera: la **arista**, la relación entre entidades. Texto, vector, arista: leer, buscar, recorrer.

## El límite de la píldora: el conocimiento relacional

La píldora de información — la unidad autocontenida que toda la serie ha tratado como ladrillo del corpus — brilla para un tipo de pregunta: la que se puede responder *dentro* de una unidad. Un plazo, una cobertura, un procedimiento, una dosis. Su límite es simétrico: no hay pregunta relacional que una píldora responda, porque lo relacional, por definición, está *entre* unidades.

El conocimiento relacional es más abundante de lo que parece y aparece en todos los sectores, con una firma reconocible: preguntas que empiezan por "¿qué... de los que... tienen...?" y cuyo verbo conecta mundos. ¿Qué empresas del grupo tienen contratos con cláusulas de revisión? ¿Qué cursos de la titulación dependen de qué asignaturas suspendidas? ¿Qué medicamentos del catálogo interactúan con el que acaba de prescribirse? ¿Qué piezas del inventario comparten proveedor con el componente retirado? En todos los casos, la respuesta es la lista resultante de recorrer una red — y la red no es una metáfora: es una estructura de datos tan concreta como la tabla y tan antigua como la contabilidad.

Conviene fijarse en una propiedad que ordena todo el capítulo: las preguntas relacionales suelen ser **recurrentes** — el director de compras la hace cada semestre, el gestor del grupo cada trimestre — y **de consecuencia alta** — la lista de dependencia única es exactamente el mapa del riesgo. Es el perfil de pregunta que merece inversión estructural; las relacionales ocasionales se pueden seguir resolviendo a mano.

## El grafo: entidades y relaciones

La construcción del grafo es, en el vocabulario de la serie, un capítulo más del Silver: una capa de extracción y curado sobre el material ya depurado. De cada píldora — o de cada documento — se extraen **entidades** — los nombres propios del dominio: empresas, personas, productos, componentes, normas, centros — y **relaciones** — los verbos que los conectan: *pertenece a*, *suministra a*, *depende de*, *modifica*, *deroga*, *requiere*, *está certificada para*.

Cada relación es un dato con tres campos y una procedencia: sujeto, verbo, objeto, y la cita de la píldora donde se leyó. Esa procedencia no es un adorno: es lo que distingue un grafo de conocimiento de un adivino. Cuando alguien pregunte "¿por qué el sistema dice que B depende de A?", la respuesta es la píldora que lo establece — la arista se verifica como se verifica una cita.

El diseño del grafo obedece a la misma regla que todo el primer libro: **mínimo viable, acotado al dominio, con esquema versionado**. No se extraen todas las entidades concebibles — se extraen las que las preguntas recurrentes van a recorrer. El grafo del fabricante tiene empresas, componentes y fabricantes de chips; no tiene, porque nadie pregunta por ellas, las ciudades de registro ni los colores de logotipo. Un grafo grande no es un grafo rico: es un grafo caro y ruidoso. La extracción es además propensa a un error particularmente traicionero que merece su propia advertencia: confundir la mención con la relación. Que un documento mencione a dos empresas juntas no significa que una suministre a la otra; que una norma aparezca citada no significa que derogue otra. La extracción automática propone, y el curado — humano o verificado — dispone. El grafo envenenado es más elegante que el vector envenenado: su respuesta equivocada llega con un recorrido convincente que parece un razonamiento.

## Resúmenes de comunidad: el mapa a media escala

El grafo puro tiene un problema de escala que aparece pronto: los recorridos largos. Si la pregunta es "¿qué riesgos hay en la rama asiática de la cadena?", recorrer arista a arista los cientos de nodos de esa rama es lento y produce una respuesta de doscientas líneas que nadie lee.

La solución que el enfoque GraphRAG popularizó son los **resúmenes de comunidad**: detectar en el grafo las zonas densamente conectadas — comunidades — y generar para cada una un resumen de su contenido y sus relaciones. La comunidad "proveedores de semiconductores del sudeste asiático" tiene su ficha: quiénes la componen, de qué dependen, qué riesgos concentran, con sus citas. Las preguntas de panorama se sirven de los resúmenes — media escala —; las preguntas quirúrgicas bajan al recorrido — escala fina. Es la misma lógica de la serie que puso síntesis de conocimiento en el Gold: el resumen no sustituye a la pieza, la organiza.

Los resúmenes añaden su propia obligación de mantenimiento — se recalculan cuando el grafo cambia — y su propia regla de honestidad: un resumen de comunidad hereda la frescura de sus aristas, y cuando una arista caduca, el resumen que la contaba miente con elegancia documental. El capítulo diecisiete volverá sobre esto: el grafo entra al bucle de retorno como entra el corpus, por sus heridas.

## Recorridos: la consulta que camina

Sobre el grafo construido, el patrón GraphRAG ejecuta **recorridos** — consultas que caminan por las aristas siguiendo la forma de la pregunta. "Los proveedores de segundo nivel que dependen de un único fabricante" es: desde cada proveedor de primer nivel, subir a sus componentes, de cada componente a sus fabricantes, contar, filtrar los que tienen uno, devolver la lista con las aristas que justifican cada salto.

Dos integraciones con lo anterior cierran el diseño. La primera: el grafo también puede ser **una herramienta del agente** — el capítulo siete anticipó que la búsqueda del corpus era una herramienta más; el recorrido del grafo lo es igualmente, y es la combinación natural para las preguntas híbridas: el agente recupera el contrato en el corpus, extrae la empresa, la busca en el grafo, recorre sus dependencias, y vuelve al corpus por la normativa aplicable. Corpus y grafo no compiten: se turnan.

La segunda: la respuesta de un recorrido se sirve con la misma vara que todas las respuestas del libro — trazabilidad por afirmación. Cada elemento de la lista lleva su cadena: el proveedor, el componente, la dependencia, y la píldora que la atestigua. Un recorrido sin citas es una afirmación de autoridad; con citas, es un argumento.

## El coste: un segundo corpus que envejece

Ahora la factura, que en GraphRAG es de las más caras del espectro porque no se paga una vez: se paga **en cada actualización del mundo**. Las relaciones caducan — las empresas cambian de grupo, los componentes cambian de fabricante, las normas derogan a otras — y cada caducidad convierte una arista en mentira. El grafo es, dicho en términos de la serie, un segundo corpus con su propio ciclo de vida: su propio Bronce de menciones, su propio Silver de relaciones curadas, su propio Gold de comunidades resumidas, y su propio bucle de heridas — cada consulta cuyo recorrido chocó con una arista muerta es una señal de mantenimiento, con su destino y su responsable en el triaje semanal del capítulo diecisiete.

Por eso la decisión de construir grafo es de las más sobrias del libro, y su test de entrada cabe en dos preguntas. Primera: ¿hay **familias de preguntas recurrentes cuya forma sea un recorrido** — no un pasaje —, con consecuencia alta y frecuencia medible? Segunda: ¿existe la disciplina de mantenimiento para que las aristas estén vivas? Si las dos respuestas son sí, GraphRAG convierte en segundos lo que consumía tardes de experto — y es el mejor dinero del año. Si la primera es no — si el noventa y cinco por ciento de las preguntas sigue siendo fáctica —, el grafo es un acuario en el desierto: hermoso, costoso, innecesario; el vector y el doble campo bastan y sobran.

## Preguntas que hace el oficio

**¿El grafo sustituye al vector?** Nunca en los sistemas reales: lo complementa. El vector sigue siendo la puerta de entrada a casi todo — las preguntas fácticas no cambian —, y el grafo responde la minoría relacional. Los sistemas GraphRAG maduros conservan el doble campo intacto y añaden la capa de entidades y aristas encima; quien derriba el vector para poner un grafo ha entendido mal ambos.

**¿Puedo construir el grafo con extracción automática y sin curar?** Se puede empezar así — la extracción automática es el Bronce del grafo —, pero sin curado la tasa de aristas falsas hace del grafo un inventador elegante: sus respuestas llegan con recorridos convincentes que parecen razonamientos. La regla de la serie vale con fuerza aquí: la extracción propone, el curado dispone — y las aristas de consecuencia alta se verifican contra su píldora antes de usarse.

**¿Cuánto cuesta de mantener, en horas reales?** Depende del ritmo del mundo, no del tamaño del grafo: cada cambio relevante en el dominio — un grupo que se reorganiza, un componente que cambia de fabricante, una norma que deroga otra — genera trabajo de aristas. Los sistemas sanos lo integran en el triaje semanal como una corriente más, y su métrica de salud es la edad media de las aristas usadas en respuestas: si sube, el grafo está envejeciendo más rápido de lo que se cura.

**¿No puedo responder preguntas relacionales con un iterativo que recorra documentos?** Para casos concretos y esporádicos, sí — y es una vía legítima de transición. La diferencia es de economía y de fiabilidad: el iterativo paga el recorrido en tokens y latencias en cada consulta y puede saltarse una pieza del encadenamiento sin notarlo; el grafo paga el recorrido una vez — al construir — y luego cada consulta lo recorre completo. A partir de cierta frecuencia y consecuencia, el grafo es más barato y más completo; por debajo, el iterativo basta.

**¿El grafo necesita su propio patrocinio de negocio?** Más que ningún otro patrón, por su coste continuo. La recomendación operativa del libro: el grafo se aprueba con un caso ancla — la pregunta recurrente de relación con frecuencia y consecuencia medidas — y se revisa contra él en el triaje semanal. Si el caso ancla desaparece o deja de doler, el grafo entra en revisión de retiro; mantenerlo por inercia es la forma más cara de coleccionar estructuras de datos. Lo que no significa que sea frágil: bien curado y con su caso ancla vivo, es la pieza más difícil de sustituir del sistema.

!!! note "Criterio de salida"
    La decisión sobre el grafo documentada con la evidencia del test de entrada: la lista de familias de preguntas recurrentes de forma relacional (con frecuencia y consecuencia medidas) y, si procede, el esquema mínimo viable de entidades y relaciones — versionado, con la procedencia de cada arista en su píldora de origen y el plan de mantenimiento de vigencias. Un grafo sin plan de caducidad no es un activo: es una colección de mentiras con fecha.

Cinco estaciones y un eje transversal: el espectro está completo. La parte tercera ya no construye patrones — aprende a elegirlos, y a pagarlos. Primera pregunta: ¿cuánto cuesta pensar?
