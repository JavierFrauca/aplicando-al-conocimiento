---
title: 3 · El contrato de respuesta
---

# 3 · El contrato de respuesta

A las nueve y cuarto de una mañana cualquiera, tres personas formulan esencialmente la misma pregunta a tres sistemas de conocimiento distintos. Un responsable de cuenta de una compañía de seguros pregunta a su asistente interno qué cubre exactamente la cláusula de daños por agua de una póliza que tiene encima de la mesa, porque está a punto de enviar una propuesta. Una ingeniera de soporte de nivel dos pregunta a la base de conocimiento de su fabricante qué versiones del firmware son vulnerables a un boletín de seguridad publicado ayer, porque tiene cuatrocientos equipos en vuelo. Y una gerente de una cadena de tiendas pregunta al portal corporativo cuántos días tiene para reclamar una subvención de formación, entre dos reuniones.

Tres preguntas gemelas en intención, tres contextos opuestos en todo lo demás. El responsable de cuenta necesita exactitud jurídica con la referencia textual, porque si la promete mal la firma su empresa; no le importa esperar veinte segundos. La ingeniera necesita una respuesta completa *ahora*, porque cada minuto son equipos expuestos; le basta la lista sin florituras, pero no admite que falte una versión. La gerente necesita un número con un enlace, en menos de cinco segundos, o desistirá y preguntará por el pasillo.

Si los tres sistemas responden igual —y en el mundo real, responden igual, porque los tres fueron configurados con el mismo modo por omisión—, dos de los tres usuarios quedan mal servidos. No por incapacidad del sistema: por ausencia de un contrato. Este capítulo trata de escribirlo.

## El espejo del contrato de recuperación

El segundo libro consagró un capítulo entero al **contrato de recuperación**: el acuerdo explícito entre el corpus y quien pregunta sobre qué significa acertar — qué es relevante, con qué trazabilidad, dentro de qué presupuesto de resultados. Ese contrato resolvió una ambigüedad mortal: "relevante" sin definición produce motores que devuelven lo parecido y lo llaman acierto.

La respuesta tiene su propia ambigüedad mortal, y este capítulo la resuelve con la misma herramienta. **El contrato de respuesta** es el acuerdo explícito sobre qué se le promete a quien pregunta cuando el sistema contesta: en cuánto tiempo, con qué frescura, con qué comprobabilidad, y qué pasará cuando el corpus no sepa. Sin él, ocurre lo que ocurre en los tres sistemas de la escena: el sistema promete de hecho — por su comportamiento observable — un contrato que nadie escribió, y cada usuario descubre sus cláusulas por sorpresa.

Hay una diferencia importante entre ambos contratos, y explica por qué este capítulo existe. El contrato de recuperación lo firma el equipo consigo mismo: sus cláusulas son internas, las comprueba un dataset, nadie del negocio las ve. El contrato de respuesta lo firma el sistema *con su usuario*: se incumple delante de él, en su pantalla, con su paciencia. Un incumplimiento del primero es una métrica que baja; un incumplimiento del segundo es un usuario que vuelve al pasillo, al teléfono o al Excel.

## Primera cláusula: la latencia

El contrato debe comprometer cuánto tarda el sistema — y hacerlo en términos que el usuario pueda sentir. No basta "rápido": hay que comprometer un número realista y, sobre todo, comprometer *qué se espera del usuario mientras tanto*.

La latencia tiene dos caras que ya presentamos y que aquí se vuelven cláusula. La **latencia real** se mide en percentiles, no en medias: prometer "tres segundos" y cumplir el noventa por ciento de las veces con diez no es un contrato, es una lotería. Los contratos serios comprometen el percentil noventa y cinco — el tiempo bajo el cual responde el 95 por ciento de las consultas — porque los usuarios no viven en la media: viven en la cola. La **latencia percibida** se gestiona con honestidad: un indicador de progreso que dice "estoy consultando tres fuentes" convierte una espera de veinte segundos en una espera legítima, mientras que el silencio convierte diez segundos en una eternidad con sospecha.

Cada patrón del espectro puede firmar esta cláusula con valores distintos, y ahí está su gracia. El directo firma "segundos, casi siempre" — es el único que puede prometer tres. El iterativo firma "segundos, decenas a veces, con progreso visible". El autocorrectivo firma lo mismo con una cola más gruesa. El agéntico firma "minutos, con plan visible y pasos documentados". Ninguno firma lo que no puede cumplir: esa es la definición misma de contrato.

La trampa clásica de esta cláusula es prometer el mínimo histórico: "una vez tardó dos segundos, luego prometemos dos". El contrato se escribe para el día malo, no para la demo.

## Segunda cláusula: la frescura

Todo lo que un sistema de conocimiento contesta tiene fecha. La cláusula de frescura compromete qué garantía de vigencia lleva la respuesta: ¿el dato que contesta estaba vigente cuando el corpus lo ingirió por última vez?, ¿cuándo fue?, ¿y qué pasa si desde entonces cambió?

Parece una obviedad hasta que se piensa en cuántos sistemas responden sin decir su fecha. El asistente de pólizas que contesta con la tarifa del año pasado, el portal académico que cita el plan de estudios anterior, el soporte que recomienda el procedimiento que se derogó el mes pasado — ninguno miente exactamente, y todos engañan. La fluidez borra las fechas.

El contrato serio hace dos promesas. Primera: la respuesta lleva su trazabilidad temporal — "según el documento X, vigente desde Y" — de forma que el usuario pueda juzgar si necesita verificar. Segunda: cuando el corpus sabe que algo tiene fecha de caducidad — porque el capítulo de ciclo de vida del primer libro la registró —, la respuesta no lo presenta como vigente. El sistema que responde "ese procedimiento quedó derogado en marzo; el vigente es este otro" no está haciendo un extra: está cumpliendo la cláusula que su contrato firma.

Fíjese el lector en que esta cláusula no la compra el modelo: la compra el corpus y el acceso, con sus metadatos de vigencia y su post-filtrado. El contrato de respuesta honesto declara dependencias — y esta es la primera: la frescura se hereda del trabajo de los dos libros anteriores.

## Tercera cláusula: la cita

La tercera cláusula compromete la **comprobabilidad**: qué parte de la respuesta viene con referencia verificable. Sus grados son dos, y conviene nombrarlos porque se confunden constantemente. El grado mínimo es la **trazabilidad**: la respuesta indica de qué documentos bebió — "según el manual de servicio, sección 7.3". El grado máximo es la **cita por afirmación**: cada dato concreto de la respuesta remite a su píldora concreta — la cifra, el plazo, la dosis, el plazo de garantía, cada uno con su fuente.

La diferencia no es de estilo: es de uso. La trazabilidad sirve para quien quiere *leer más*; la cita por afirmación sirve para quien quiere *actuar* — firmar, dosificar, responder a un cliente, presentar ante un juez. Cuanto mayor es la consecuencia del error, más arriba debe firmar el sistema en esta escala. Es una consecuencia directa del capítulo anterior: la cita por afirmación exige redacción por ensamblaje — el modelo no compone libremente, se apoya en píldoras citables — y por tanto una plantilla de generación pensada para ello (capítulo trece) y una verificación de que la cita dice lo que la respuesta dice que dice (capítulo catorce).

Hay sistemas donde esta cláusula se firma en su grado máximo siempre — el jurídico, el clínico, el regulatorio — y sistemas donde el grado mínimo basta casi siempre y el máximo se reserva a las respuestas de alta consecuencia. La escalera existe; el contrato dice en qué peldaño se para cada audiencia.

## Cuarta cláusula: la abstención

La cláusula más contraintuitiva y la más importante: el contrato compromete **qué hará el sistema cuando no sepa**. Y la promesa correcta es incómoda de enunciar para quien viene del mundo de las demos: *el sistema dirá que no lo sabe, y eso cuenta como un buen servicio*.

Sin esta cláusula, el comportamiento por omisión del modelo de lenguaje ocupa su lugar: responder siempre, con cortesía y fluidez, aun cuando el contexto no alcanza. Es el comportamiento que produjo las tres desgracias del capítulo uno — el procedimiento mezclado, la dosis redondeada, el plazo inventado — y produce muchas otras cada día en sistemas que nadie ha parado a preguntar: "¿qué haces cuando no lo sabes?".

La cláusula bien escrita tiene tres partes. La **condición**: qué estado del contexto activa la abstención — cosecha insuficiente, contradicción irresoluble, pregunta fuera del dominio del corpus. La **forma**: qué dice el sistema exactamente — no un "no puedo ayudarte" de teleoperadora, sino algo útil: "el corpus no contiene información sobre X; lo más cercano que contiene es Y". Y el **registro**: cada abstención queda anotada, porque — como veremos en el capítulo diecisiete — cada "no lo sé" es el dato más valioso que el sistema puede darle al corpus sobre dónde le duele el conocimiento.

Para la audiencia, esta cláusula es la que transforma la confianza. Un sistema que se abstiene a veces es un sistema cuyas respuestas se pueden creer. Un sistema que responde todo, uniforme y fluido, es un sistema que hay que verificar siempre — y los usuarios lo aprenden en una semana, aunque nadie se lo diga.

## Quinta cláusula: los límites

La última cláusula declara lo que el sistema **no** promete — y es la que más se olvida porque escribir límites no entretiene. Todo sistema de conocimiento tiene fronteras: dominios que no cubre, decisiones que no toma, consejos que no da. El contrato las nombra antes de que un usuario las descubra.

El asistente de pólizas no asesora sobre qué póliza contratar: informa coberturas. El portal de protocolos clínicos no decide tratamientos: expone protocolos vigentes con su referencia. El sistema del despacho no valora si conviene demandar: expone plazos, jurisprudencia y criterios. Ninguno de estos límites resta utilidad — todos la *hacen posible*, porque delimitan el terreno donde las demás cláusulas se pueden cumplir. Un sistema que promete todo no puede firmar ninguna de las cuatro cláusulas anteriores: no puede comprometer latencia sobre cualquier pregunta, ni frescura sobre dominios que no cubre, ni cita por afirmación sobre materias que no tiene, ni abstención útil sobre cosas de las que no sabe ni que existen.

Los límites también protegen al sistema de su éxito. El portal que un día responde bien una pregunta de formación subvencionada recibirá al mes siguiente preguntas de asesoramiento fiscal — y necesita una forma digna de decir "eso está fuera de este servicio" que no suene a máquina malhumorada.

## Quién firma, y cuándo se renegocia

Una última pieza de gobernanza, porque un contrato sin firmante es una intención. El contrato de respuesta se firma en dos manos: el negocio compromete las cláusulas — qué promesa hace a sus usuarios — y el equipo técnico compromete el patrón capaz de cumplirlas. La firma no es simbólica: es la que hace auditable el incumplimiento. Cuando la latencia p95 rompe el contrato dos semanas seguidas, nadie discute si "iba lento": se abre una incidencia contra una cláusula escrita. Cuando una audiencia nueva exige cita por afirmación que el corpus no puede sostener, la negociación ocurre en la reunión de firma — no en producción.

Y las tres ocasiones de renegociación, para que el documento no se fosilice: cuando cambia la audiencia (un colectivo nuevo con consecuencias nuevas), cuando cambia el negocio (una consecuencia que sube de grado, un plazo que se aprieta) y cuando cambia la capacidad (un patrón nuevo que firma lo que antes nadie firmaba — o una medición que demuestra que lo firmado era ilusión). Fuera de esas ocasiones, el contrato no se toca: su valor está en la estabilidad, que es lo único que permite a los usuarios saber —y a los equipos responder— qué se les debe.

## El contrato primero, el patrón después

Escritas las cinco cláusulas, la secuencia de diseño se invierte respecto a lo que hace la industria. Lo habitual: se elige una tecnología (o se hereda de una demo), se pone en producción, y las promesas se descubren a posteriori observando su comportamiento. Lo propuesto: se escribe el contrato para cada audiencia real — cuánto puede esperar, qué grado de cita necesita, qué consecuencia tendría un error — y **el patrón del espectro se elige como consecuencia**: el más barato capaz de firmar ese contrato.

El directo puede firmar latencia pero no puede firmar abstención bien informada en preguntas complejas. El agéntico puede firmar exhaustividad pero no puede firmar tres segundos. Ningún patrón firma todo — y por eso el sistema que sirve a varias audiencias con un solo modo sirve mal a todas. El contrato escrito es la herramienta que convierte esa observación en decisiones por escrito.

Un matiz de gobernanza que este libro hereda de los dos anteriores: el contrato es un **documento vivo**, versionado como el manifiesto del primer libro y revisado en el mismo triaje semanal que gestiona el retorno del segundo. Cuando el negocio cambia — una audiencia nueva, una consecuencia que sube de grado —, el contrato cambia primero y el sistema después. Nunca al revés: el contrato escrito *después* del sistema no es un contrato, es un memorándum justificativo.

## Preguntas que hace el oficio

**¿Quién escribe este contrato? ¿El equipo técnico?** No en solitario. Las cláusulas de latencia, cita y límites las compromete el negocio — porque son promesas a sus usuarios —, y el equipo técnico las valida — porque debe certificar que algún patrón puede cumplirlas. El contrato firmado solo por técnicos promete lo que el negocio no sabe que promete; el firmado solo por el negocio promete lo que el sistema no puede cumplir. La firma es de dos manos, y ese es el punto.

**¿Un contrato por usuario? Tenemos cientos.** No: por *contrato*, no por persona. Los cientos de usuarios se agrupan en pocas audiencias con expectativas homogéneas — el expediente rápido, el expediente exhaustivo, el usuario externo —. Si dos colectivos piden lo mismo, comparten fila; si un usuario concreto pide algo distinto, o es una audiencia nueva con su contrato, o es una preferencia que el sistema no atiende.

**¿Qué pasa si incumplimos el contrato? ¿Se cae algo?** Se registra. El incumplimiento de contrato es una métrica del panel del capítulo dieciséis — latencia fuera de compromiso, abstención fuera de política —, y su tratamiento es el del triaje: incidencia con destino, responsable y semana. La diferencia entre un contrato y un deseo es precisamente que el incumplimiento tiene consecuencias visibles y gestionadas.

!!! note "Criterio de salida"
    El contrato de respuesta escrito en una página por cada audiencia real del sistema: las cinco cláusulas con valores concretos (percentil de latencia comprometido, garantía de frescura, grado de cita, condición y forma de la abstención, límites declarados) y el patrón del espectro elegido como consecuencia de firmarlas. Sin contrato, el patrón se elige por entusiasmo y se defiende por costumbre.

El espectro está trazado y el contrato da criterio para moverse por él. Ahora toca recorrerlo a fondo: seis capítulos, seis patrones, empezando por el que responde la gran mayoría de las preguntas — el RAG directo.
