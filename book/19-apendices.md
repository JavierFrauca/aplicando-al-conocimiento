---
title: Apéndices A-F
---

# Apéndices A-F

> Los apéndices reúnen, en versión operativa, las piezas que el libro fue construyendo: las plantillas del capítulo correspondiente, listas para adaptar; las métricas del panel de respuesta con sus definiciones; los criterios para evaluar tecnología sin casarse con ella; la brújula completa en versión imprimible; cuatro casos de extremo a extremo recorridos con el método entero; y el libro condensado en veinte decisiones.

## Apéndice A · Plantillas de la respuesta

Las cinco plantillas del libro, con su contenido en cuerpo real. Conviene adaptarlas al dominio antes de usarlas — y versionarlas, porque son piezas de arquitectura, no formularios de una sola noche.

### A.1 — Contrato de respuesta (capítulo 3)

Una página por audiencia. Campos obligatorios:

- **Audiencia y descripción**: quién pregunta y en qué contexto (el gestor entre reuniones; el especialista preparando un expediente; el cliente por el portal).
- **Latencia comprometida**: percentil 50 y percentil 95 — "2-4 s en el 95 % de las consultas" —, y qué se muestra durante la espera (progreso legible: sí/no, con qué vocabulario).
- **Frescura**: garantía de vigencia de lo respondido — "solo material vigente; la fecha de cada fuente visible".
- **Grado de cita**: trazabilidad mínima o cita por afirmación — y para qué afirmaciones (todas las cifras y plazos; cada párrafo).
- **Abstención**: condición ("sin respaldo suficiente en el corpus"), forma ("no consta en el corpus; lo más cercano es X") y destino del registro (triaje semanal).
- **Límites**: qué no se le pide al sistema (asesoramiento, decisiones, dominios fuera del corpus).
- **Patrón asignado y fecha**: el punto del espectro que firma este contrato, con la fecha de la última revisión.

### A.2 — Matriz de decisión (capítulo 11)

Una fila por familia de consultas. Columnas: **familia** (nombre y volumen mensual aproximado), **contrato de audiencia** (referencia a A.1), **alcance** (fáctica acotada / panorama / cruzada u operativa / de relación), **consecuencia del error** (barata / con coste operativo / con coste legal / techo del sector), **urgencia** (segundos / decenas / minutos; plazo de negocio), **patrón asignado** (del espectro), **verificación** (el patrón firma el contrato: sí/no — y si no, qué cláusula falla), **evidencia** (dataset, medición o carácter provisional con fecha de revisión), **estado** (aprobada / provisional / en revisión).

Reglas de mantenimiento: las filas nacen de las preguntas, nunca de la tecnología; se cuentan por decenas; y toda fila sin evidencia se marca provisional y se mide en su primer mes.

### A.3 — Tabla del precio (capítulo 10)

Una fila por patrón en uso. Columnas: latencia p50/p95 medida; coste por consulta en múltiplos del directo; coste de operación estimado (piezas a vigilar, horas de mantenimiento); superficie de fallo (modos característicos); y **presupuesto aprobado por el negocio** — el precio máximo por consulta que esta familia paga. La tabla se revisa cuando cambian los volúmenes, los modelos o los precios de inferencia.

### A.4 — Plantilla de generación (capítulo 13)

Una por contrato de audiencia, con las seis piezas del contrato de generación:

- **Papel**: "Redactas la respuesta para [audiencia] a partir exclusivamente del contexto entregado. No tienes conocimiento propio aplicable: todo lo que digas debe venir del contexto."
- **Material**: estructura de los bloques de contexto, reglas de uso ("cada bloque lleva su identificador; úsalo para citar", "los bloques caducados no responden", "si dos bloques contradicen, expón la discrepancia").
- **Prohibiciones**: lo que el modelo no hace — cortesía inflada, repeticiones, frases de relleno —.
- **Instrucciones de cita**: el grado del contrato — "cada cifra, plazo y condición lleva su referencia [fuente N]"; o "las referencias van al pie".
- **Formato**: estructura de la respuesta (dato primero / razonamiento primero) y longitud máxima.
- **Regla de abstención**: condición ("si el contexto no respalda la respuesta, no respondas"), forma ("El corpus no contiene información sobre [tema]. Lo más cercano que contiene es [X], que trata [aspecto]"), registro ("todas las abstenciones quedan anotadas").

Cada edición de la plantilla es una hipótesis con prueba: se escribe el cambio esperado, se ejecuta la regresión (ver D y capítulo 15) y se despliega con su versión y su fecha.

### A.5 — Rúbrica del juez de respuesta (capítulo 15)

Cuatro dimensiones con sus preguntas y sus escalas: **fidelidad** (¿cada frase está respaldada en el contexto? — lista de frases sin respaldo), **utilidad** (¿responde lo que la audiencia preguntó, según su contrato? — escala 1-4), **cita correcta** (¿existen, corresponden y dicen lo que se afirma? — binaria con hallazgos), **formato y abstención** (¿cumple el formato del contrato?; ante pregunta sin corpus, ¿se abstuvo bien?). Incluye el protocolo de calibración: divergencia juez-humano medida por mes, casos de fallo del juez estudiados, rúbrica ajustada con evidencia.

## Apéndice B · Métricas del panel de respuesta

El panel del capítulo dieciséis, con definiciones operativas. Todas las métricas se cortan **por patrón** y por familia de la matriz; ninguna se interpreta en media global.

- **Latencia p50 / p95** por patrón, contra el compromiso de cada contrato. Alerta: p95 por encima del contrato durante dos días consecutivos.
- **Tasa de reintentos** — consultas que consumen su presupuesto de vueltas o de reescritura. Lectura: alta sostenida = evaluador paranoico o familia mal asignada; nula = evaluador complaciente.
- **Tasa de abstención** por familia, en tendencia. Lectura: estable y razonable = salud; desplome = el sistema contestó de todo; disparo = corpus viejo o umbral endurecido. Se celebra y se estudia; no se castiga.
- **Coste diario por patrón** — consumo de inferencia por familia, contra presupuesto. Alerta: desvío de más del umbral acordado respecto de su familia.
- **Acierto del router** — muestra contrastada contra la matriz, con su matriz de confusión por familia. Revisión en el triaje semanal.
- **Fidelidad** — frases sin respaldo (tendencia) y citas verificadas (universal). Alerta: subida sostenida de frases sin respaldo o una sola cita falsa.
- **Volumen por familia** — la composición del tráfico, que contextualiza todas las anteriores.

De la definición a la alerta, un ejemplo del método completo sobre una sola métrica. La **tasa de abstención** de la familia farmacológica de un sistema clínico arrancó en producción en el 3,1 %, que su triaje validó como el nivel real de preguntas fuera de corpus. Su banda de operación acordada: 2 %–5 % semanal. A las nueve semanas la serie cayó a 0,4 % — ninguna alerta sonaba, porque nada pasaba el umbral alto —. La lectura en tendencia sí la destapó: una plantilla desplegada esa semana había suavizado la condición de abstención, y el sistema llevaba seis días respondiendo preguntas que antes declaraba vacías. El hallazgo llegó del contraste entre las dos métricas hermanas — abstención baja y tasa de reintentos del usuario al alza —, no del umbral. De ahí la regla final del panel: **las bandas se ponen por métrica, pero las alertas se leen por pares** — ninguna métrica de respuesta significa nada sola, porque cada una se disimula en otra.

## Apéndice C · Revisión de tecnología: cómo evaluar sin casarse

Este libro es deliberadamente agnóstico: sus unidades son patrones y decisiones. Cuando toque elegir herramienta — motor vectorial, orquestador agéntico, proveedor de modelos, estándar de herramientas —, valen los criterios que la serie ha usado siempre, traducidos a preguntas de compra:

1. **¿Qué patrón del espectro encarna, y con qué pieza del libro lo comparo?** Si la propuesta no se puede colocar en el espectro, no se ha entendido todavía — y no se compra lo que no se entiende.
2. **¿Es reemplazable?** ¿Su contrato — entradas, salidas, límites — está tan claro que podría sustituirse por otra sin reescribir el sistema? Los contratos del apéndice A son, también, la póliza anti-captura.
3. **¿Da trazabilidad de lo que importa?** Del encaminamiento, de los pasos, de las citas. Una pieza sin observabilidad no se opera: se sufre.
4. **¿Hereda permisos?** ¿Puede ejecutarse con los permisos de quien pregunta, sin privilegios propios? Si no, es un riesgo de gobernanza disfrazado de funcionalidad.
5. **¿Envejece bien?** ¿Resuelve un patrón estable o una marca de moda? La regla de la serie: los motores cambian cada trimestre; los principios, no. Y la contraria, de humildad: cuando el mercado demuestre que una pieza nueva sirve mejor a un patrón, se cambia — con regresión aprobada y despliegue graduado, como todo.

Un apéndice de uso: los cinco criterios son también la **lectura crítica de cualquier propuesta comercial** que llegue al correo. "Agentes autónomos que revolucionan su acceso a la información" se traduce con la lista: ¿qué patrón del espectro encarna y dónde entra en su matriz?, ¿cómo se encamina — hay router con matriz o todo va al agente?, ¿qué trazabilidad por paso ofrece?, ¿hereda permisos?, ¿qué pasa cuando se retira? Un proveedor serio contesta las cinco en una página; uno que solo tenga demo, contestará con otra demo. Y la prueba inversa, la más reveladora: preguntarle **cuándo no usar su producto** — la respuesta que no saben dar es la que mejor describe lo que venden.

## Apéndice D · La brújula de la respuesta, en versión imprimible

| Paso | Capítulos | Criterio de salida |
|---|---|---|
| 1. Entender la respuesta | 1–3 | Las tres decisiones auditadas en diez respuestas reales; el espectro situado en tres sistemas; el contrato de respuesta escrito por audiencia |
| 2. Conocer los patrones | 4–9 | Lista de familias asignadas al directo con su exclusión; política de iteración; réplica calibrada; catálogo de herramientas con contratos; mapa del equipo; decisión sobre el grafo |
| 3. Pagar el precio | 10–12 | Tabla del precio medida y aprobada; matriz de decisión completa y verificada; router operativo con matriz de confusión |
| 4. Redactar y verificar | 13–15 | Plantillas de generación versionadas con regla de abstención probada; vara de fidelidad con cobertura graduada; juez calibrado con regresión en verde |
| 5. Operar y cerrar el ciclo | 16–17 | Panel de respuesta por patrón con alertas; guardarraíles hasta la última milla; cuarta corriente en el triaje con la métrica del círculo completo en tendencia |

Una página, una pared. El resto es la serie entera.

## Apéndice E · Cuatro casos de extremo a extremo

Cuatro casos de extremo a extremo — compuestos sobre patrones de sistemas reales, con nombres cambiados y cifras redondeadas — recorridos con el método completo del libro: su matriz, sus decisiones, sus tropiezos y su resultado. No son casos de éxito: son casos contados, que es más útil. Cada uno cierra con lo que enseña que ningún capítulo enseñaba solo.

### Caso 1 · Soporte técnico en dos niveles — la matriz pequeña que lo arregló todo

**El sistema.** Un fabricante de maquinaria de embalaje con una red de distribuidores y una base de conocimiento de seis años: manuales de servicio, notas de avería, boletines técnicos, procedimientos de garantía. El asistente interno atendía a dos audiencias de perfil muy distinto: los técnicos de los distribuidores — preguntas de avería concreta, con la máquina delante y el cliente al lado — y el propio equipo de soporte de nivel dos — expedientes que escalan, con tiempo y consecuencia —.

**La matriz.** Cuatro familias. La de *avería típica por modelo* — el 64 % del volumen: códigos de error, procedimientos estándar, listas de repuestos — quedó en **directo** con tres segundos de p95 y cita de manual obligatoria. La de *avería compuesta* — síntomas cruzados, varias hipótesis — quedó en **iterativo** con presupuesto de tres vueltas: la subpregunta "¿qué sistemas tocan este síntoma?" antes de la búsqueda final. La de *expediente de garantía* quedó en **directo con réplica interna**: la cosecha que mezclara dos versiones de procedimiento — la herida fundacional del sistema, un procedimiento de purga aplicado a la generación anterior del equipo — se detectaba antes de redactar. La de *consulta fuera de catálogo* — máquinas antiguas sin manual digitalizado — iba a **abstención con escalada a nivel dos**, y su registro alimentó el roadmap de digitalización.

**Los tropiezos.** El primero fue de asignación: durante el piloto, las averías compuestas iban al directo y las respuestas "casi correctas" — que citaban el procedimiento del modelo parecido — confundían más que un no-respuesta. La señal que las destapó no fue una queja: fue la tasa de repetición — los mismos técnicos reformulaban la misma consulta —. La segunda vuelta del iterativo se activó al mes. El segundo tropiezo fue de contrato: el nivel dos exigía cita por afirmación y el nivel uno bastaba con trazabilidad al pie; durante seis semanas ambas audiencias compartieron plantilla, y los distribuidores leían expedientes interminables. Dos plantillas, y el problema desapareció sin tocar nada más.

**Lo que enseña.** Que la matriz pequeña — cuatro filas — resuelve organizaciones grandes; que las señales de repetición delatan fronteras de familia mejor que cualquier análisis previo; y que el contrato por audiencia es tan barato de separar — una plantilla más — que no hay excusa para la plantilla única. Los capítulos cuatro, cinco y seis operando juntos, sin nada exótico: ese es el estado estacionario de la mayoría de los sistemas maduros.

### Caso 2 · Protocolos clínicos internos — la vara donde el error no tiene precio

**El sistema.** Un grupo hospitalario con un corpus de protocolos, guías farmacoterapéuticas y circulares internas, consultado por personal sanitario en punto de cuidado. Aquí la consecuencia del error tiene techo propio — la dosificación —, y todo el diseño se subordina a ella.

**La matriz.** Tres familias con la consecuencia como eje. *Consulta de procedimiento* — ubicación de material, pasos de un protocolo administrativo —: **directo**, cita obligatoria, abstención rápida. *Consulta farmacológica* — dosis, interacciones, ajustes por peso o función renal —: **directo con réplica interna permanente** — evaluador en cada consulta, cita por afirmación literal: la cifra se copia del protocolo tal cual, sin redondeo posible, y la unidad va junto al número. *Consulta de panorama* — "¿qué protocolos tocan el alta en 24 horas?" —: **iterativo** con síntesis que declara qué protocolos no pudieron consultarse.

**Los tropiezos.** El diseño pasó tres pruebas humillantes antes de producción. La primera: en la vuelta de validación, la plantilla original resumía "2,5 mg/kg" como "unos 3" — el redondeo del capítulo uno, en su versión verbal. La regla de la plantilla que lo mató fue de las más cortas del libro: *las cifras se copian, no se escriben*. La segunda: la réplica interna se calibró paranoica — el evaluador, entrenado con demasiados ejemplos de rechazo, abstenía el 22 % de consultas farmacológicas — y la tasa de escalada manual se disparó; dos semanas de recalibración con casos reales la dejaron en el 3 %, que era el nivel del corpus, no el del miedo. La tercera: se descubrió que una circular de 2022 seguía viva en el corpus junto a su sustituta de 2025 — el post-filtrado de vigencia del acceso la dejaba pasar por un defecto de metadato —. Lo interesante fue el detector: no fue una auditoría, fue la vara — tres respuestas con cita a la circular muerta en la misma semana la destaparon.

**Lo que enseña.** Que la consecuencia del error no es una columna de la matriz: es el eje que reordena todo — plantilla, verificación, umbrales. Que la réplica interna se calibra con el corpus que hay, y que su tasa de abstención es su termómetro. Y que la vara de fidelidad es, además de control, el mejor detector de defectos del corpus — la cuarta corriente del capítulo diecisiete en su forma más literal: la respuesta delatando al material.

### Caso 3 · Condiciones de pólizas — el iterativo que sostiene al comercial

**El sistema.** Una aseguradora multirrama con un corpus de condiciones generales y especiales, apuntes técnicos y circulares de suscripción. Sus usuarios: la red comercial — cotizando con el cliente delante —, el área de siniestros — resolviendo con consecuencia legal — y el portal de mediadores.

**La matriz.** Cinco familias. *Cobertura puntual de producto*: directo con réplica interna — la póliza contradictoria de la versión anterior era la herida fundacional. *Panorama de garantías* ("¿qué cubre y qué excluye la de responsabilidad?"): iterativo de dos vueltas con síntesis por secciones. *Comparativa entre productos*: iterativo recursivo — descomponer por producto, resolver cada rama con directo, sintetizar con la discrepancia declarada. *Consulta con el expediente* (siniestros): agéntico de bajo alcance — el corpus más dos herramientas: el expediente del siniestro y el calculador de indemnizaciones; presupuesto de seis pasos. *Consulta del mediador por portal*: directo puro, tres segundos, referencias al pie.

**Los tropiezos.** El más instructivo fue de precio: el agente de siniestros se estrenó encaminando también las consultas de cotización — ambas "tocan expedientes" —, y el mes multiplicó su factura por tres sin mejorar ninguna respuesta. La corrección no fue técnica: fue de matriz — la familia de cotización bajó al iterativo, y el agente quedó donde su consecuencia lo justifica. El segundo fue de síntesis: la comparativa de productos promediaba exclusiones — "cobertura parcial" para lo que era cubierto en una rama y excluido en otra —; la regla del no promediar del capítulo catorce, instrumentada como prohibición de plantilla más control de contradicciones, lo resolvió: la comparativa correcta lista por producto, con sus diferencias, nunca fusionada.

**Lo que enseña.** Que la matriz mal asignada cuesta facturas — y que su corrección es un cambio de fila, no un proyecto. Que el iterativo recursivo es el patrón natural de las comparativas, y que la honestidad de la síntesis — discrepancias declaradas — es lo que la hace utilizable por un comercial delante de un cliente. Y que cinco familias bastan para una aseguradora multirrama: la matriz crece por casos reales, no por posibilidades.

### Caso 4 · Cumplimiento multi-territorio — el equipo, el grafo y la factura

**El sistema.** La operadora logística de los capítulos cinco y dieciséis: normativa de etiquetado de mercancías en doce países, catálogo propio de miles de referencias, y dos preguntas recurrentes que consumían tardes de experto: qué cambió y qué afecta, y de qué proveedores de segundo nivel depende la cadena. Aquí convergen los patrones altos del libro — y su contabilidad.

**La matriz.** Cuatro familias con volúmenes radicalmente asimétricos. *Consulta puntual de normativa por país* — el 82 %: directo por dominio del país, tres segundos. *Panorama de cambios del trimestre*: iterativo recursivo — una subconsulta por país, doce cosechas, síntesis con tabla. *Cruce catálogo-normativa*: iterativo con dos herramientas — el catálogo vive en base de datos, la normativa en el corpus. *Cadena de dependencia de proveedores*: **grafo** — entidades empresa-producto-fabricante, aristas con su cita, recorrido semestral que sustituyó una tarde de experto por cuarenta segundos con trazabilidad.

**Los tropiezos.** El grafo se construyó dos veces. La primera extracción automática, sin curado, produjo un 14 % de aristas falsas — proveedorías mal leídas de contratos que solo mencionaban a ambas partes —, y un recorrido de la primera versión devolvió una dependencia única que no existía: el informe de riesgo equivocado que el proyecto entero existía para evitar. La reconstrucción con curado — extracción propuesta, verificación contra la píldora, firma del responsable de cada arista crítica — la dejó en menos del 1 %, y el bucle de retorno incorporó la corriente de aristas: cada recorrido contradicho por el experto es una arista a revisar con semana límite. El segundo tropiezo fue de encaminamiento: la familia de cruces catálogo-normativa empezó en agéntico — "toca base de datos, toca corpus" — y su factura la delató: el patrón correcto era el iterativo con dos herramientas fijas, y el agente quedó para los expedientes de contingencia que sí planifican en vivo.

**Lo que enseña.** Que los patrones altos se pagan con evidencia y se retiran con evidencia — el agente que sobra baja de la matriz sin drama cuando la fila lo dice. Que el grafo vale exactamente lo que valga su curado: sin arista firmada, no hay recorrido creíble. Y que la asimetría de volúmenes es la regla — un 82 % de la cola en el patrón barato financia el 0,1 % de investigaciones y recorridos donde la consecuencia lo justifica. Es, en un solo sistema, el libro entero: el directo sosteniendo el volumen, el iterativo sosteniendo el panorama, el grafo sosteniendo la relación, el panel sosteniendo la verdad de los números, y el triaje semanal — con su fila de aristas y su fila de encaminamientos — sosteniendo que todo ello siga siendo cierto el mes que viene.

!!! note "Criterio de salida"
    Haber leído los cuatro casos con lápiz en mano: para cada uno, identificar qué capítulo del libro sostiene cada decisión — y qué habría pasado en ese sistema sin esa pieza. Quien pueda contar los cuatro sistemas con sus propias palabras, tiene el libro.

## Apéndice F · El libro en veinte decisiones

El método completo, condensado en las decisiones que un sistema de respuesta serio toma — cada una con su capítulo y su regla en tres frases. Es la versión de consulta: la discusión de cada una está en su capítulo.

**1. ¿Con qué estrategia responde cada familia de consultas?** (caps. 2, 11) — Con la posición del espectro que su fila de la matriz determina: audiencia, alcance, consecuencia y urgencia deciden; el patrón es la consecuencia, nunca la elección inicial.

**2. ¿Qué se le promete a cada audiencia?** (cap. 3) — Un contrato escrito de una página: latencia en percentiles, frescura, grado de cita, abstención y límites. Se firma a dos manos — negocio y técnica — antes de elegir nada.

**3. ¿Cuándo basta una pasada?** (cap. 4) — Cuando la pregunta es fáctica y acotada, la respuesta vive en pocas píldoras y la fiabilidad de esa familia está medida. Con su lista de asignación y su lista de exclusión — nunca como costumbre sin fecha.

**4. ¿Cuándo recuperar de nuevo?** (cap. 5) — Cuando la pregunta se compone: plurales con cruce, subpreguntas encadenadas, panoramas. Con presupuesto de vueltas y señal de parada verificable — sin ellos, no es iteración: es bucle.

**5. ¿Cuándo mirar dos veces la cosecha?** (cap. 6) — Donde el coste del error supera el coste de la segunda mirada. Rúbrica de suficiencia en cuatro dimensiones, tasa de rechazo medida, y evaluación permanente barata como omisión.

**6. ¿Cuándo un planificador con herramientas?** (cap. 7) — Cuando hay que operar — calcular, consultar sistemas vivos, encadenar hallazgos que cambian el plan — con baja frecuencia y alta consecuencia. Con catálogo contratado, presupuestos y trazabilidad paso a paso.

**7. ¿Cuándo un equipo de agentes?** (cap. 8) — Cuando el reparto entre dominios ya existía a mano y la consecuencia lo paga. Cada especialista con su caso que justifique la plaza, traspasos en fichas y un único responsable de la respuesta.

**8. ¿Cuándo un grafo?** (cap. 9) — Cuando hay familias de preguntas recurrentes cuya forma sea un recorrido — no un pasaje — y disciplina de mantenimiento para que las aristas estén vivas. Con su caso ancla y su cita por arista.

**9. ¿Cuánto cuesta pensar?** (cap. 10) — Las cuatro monedas: latencia en percentiles, coste por consulta, operación y superficie de fallo. Medidas en el sistema propio, aprobadas por quien paga, revisadas cada trimestre.

**10. ¿Quién encamina en vivo?** (cap. 12) — El router, con las señales en orden de fuerza: audiencia, composición, dominios, historia, superficie. Escalado progresivo barato-primero, escalas en caliente con techo, y matriz de confusión en el triaje.

**11. ¿Cómo redacta el modelo?** (cap. 13) — Con una plantilla por contrato de audiencia: papel, material, prohibiciones, citas obligatorias, formato y regla de abstención. Versionada, con hipótesis y regresión en cada edición.

**12. ¿Qué pasa cuando no sabe?** (caps. 3, 13) — Se abstiene: condición verificable (sin respaldo, no sin respuesta), forma útil (qué falta, qué hay cerca) y registro en el triaje. La abstención es un éxito que se celebra, no un error que se esconde.

**13. ¿Cómo sé que la respuesta no miente?** (cap. 14) — Verificación de citas universal; fidelidad frase a frase completa donde la consecuencia lo paga; muestreo en el resto. Con calibración contra falsos acusados y ninguna condena automática.

**14. ¿Cómo sé que mejora y no que deriva?** (cap. 15) — Juez de la respuesta con rúbrica de cuatro caras calibrada contra juicio humano, dataset que crece por heridas firmadas, y regresión de respuesta como puerta de todo cambio.

**15. ¿Qué miro en producción?** (cap. 16) — Un panel por patrón, siete métricas, lectura en cascada de quince minutos: volumen, router, latencia y coste, abstenciones, reintentos, fidelidad.

**16. ¿Qué frena un despliegue?** (caps. 15, 16) — La regresión no aprobada, y después el canal de prueba con sus paneles. Nada entra a toda la cola el primer día; nada se degrada en silencio.

**17. ¿De quién son los permisos de la respuesta?** (cap. 16) — De quien pregunta, siempre: cada herramienta, cada especialista, cada paso intermedio hereda su alcance. La auditoría cubre el recorrido entero, no la última milla.

**18. ¿A dónde va lo que el sistema aprende?** (cap. 17) — A la cuarta corriente del triaje semanal: abstenciones al corpus, fallos de cosecha al acceso, errores de encaminamiento a la matriz, veredictos a la plantilla. Toda señal con destino, responsable y semana.

**19. ¿Cuándo cambia la matriz?** (cap. 11) — Por incidente o por evidencia acumulada — nunca por moda ni por demo. Las decisiones tomadas en caliente que sobreviven dos semanas se convierten en cambio de fila con su dato.

**20. ¿Cuándo está terminado?** (cap. 17) — Nunca, y esa es la buena noticia: es un organismo con bucle, no un proyecto con entrega. Su métrica de salud es el tiempo desde que una respuesta delata una carencia hasta que la carencia está resuelta — en tendencia, cayendo.

!!! note "Criterio de salida"
    Las veinte decisiones respondidas para el sistema propio — cada una con su pieza instalada, su documento y su métrica. Es la lista de comprobación final del libro: quien las tenga respondidas por escrito tiene un sistema de respuesta diseñado; quien tenga alguna sin responder, tiene su próximo sprint.
