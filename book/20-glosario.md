---
title: Glosario (Apéndice G)
---

# Glosario (Apéndice G)

> Los términos de este libro, definidos en una o dos líneas con su capítulo de origen. Los conceptos heredados de los libros anteriores — píldora de información, doble campo, dominio, dataset áureo, híbrido, top-k, rerank, contrato de recuperación — se remiten a los glosarios de origen.

## El espectro y los patrones

- **Patrón de respuesta** — La estrategia con la que el sistema produce una respuesta a partir del corpus: cuánto piensa, qué herramientas usa, cuántas vueltas da (cap. 2). La unidad de decisión de este libro.
- **Espectro de la respuesta** — El ordenamiento de los patrones por una sola pregunta: ¿cuánto piensa el sistema antes de hablar? (cap. 2).
- **RAG directo (one-shot)** — Recuperar, ensamblar y redactar en una sola pasada. Rápido, barato, predecible, auditable en una página; sin segunda mirada (cap. 4).
- **Iterativo / multi-hop** — El patrón que recupera varias veces: la pregunta genera subpreguntas y cada respuesta alimenta la siguiente (cap. 5).
- **Self-ask** — Técnica iterativa en la que el sistema descompone la pregunta en subpreguntas y las resuelve en orden, verificando cada resultado antes de usarlo (cap. 5).
- **FLARE** — Redactar mientras se recupera: el borrador avanza y vuelve al rack cuando una frase se queda sin respaldo (cap. 5).
- **Recursivo** — Descomponer por unidades (países, productos, centros), resolver cada subconsulta con una pasada y sintetizar con trazabilidad (cap. 5).
- **Réplica interna (autocorrección)** — El sistema critica su propia cosecha antes de responder y decide redactar, reintentar, reescribir la consulta o abstenerse (cap. 6).
- **CRAG** — Corrective RAG: un evaluador de cosecha con tres veredictos (suficiente, insuficiente, equivocado) y una acción correctiva por veredicto (cap. 6).
- **Self-RAG** — Autocorrección integrada en la generación: el modelo decide cuándo recuperar, critica lo que recupera y critica su propio borrador (cap. 6).
- **RAG agéntico** — Un planificador orquesta herramientas — el recuperador es una más — y evalúa en cada paso qué sabe y qué le falta (cap. 7).
- **Herramienta (tool)** — Cualquier capacidad externa invocable por el sistema — buscador, calculadora, SQL, calendario, API — con su propio contrato: entrada, salida, condición de "no encontrado", permisos y coste (cap. 7).
- **ReAct** — El ciclo razón–actúa–observa que gobierna la ejecución del agente: planificar, invocar, examinar el resultado, corregir el plan (cap. 7).
- **MCP** — Estándar abierto que define cómo un modelo descubre e invoca herramientas: el enchufe común de la orquestación agéntica (cap. 7).
- **Multi-agente** — Varios agentes especializados con un coordinador que reparte y un verificador que cruza antes de firmar (cap. 8).
- **Coordinador** — El órgano del equipo que analiza la pregunta, la descompone en subtareas y las asigna a especialistas (cap. 8).
- **Verificador** — El órgano que cruza las piezas del equipo (completitud, contradicciones, citas, fronteras) antes de dar paso a la redacción (cap. 8).
- **Ficha de traspaso** — El handoff estructurado entre agentes, con campos fijos (hallazgos, citas, condiciones, excepciones, dudas) que impide que el matiz se evapore (cap. 8).
- **GraphRAG** — El patrón que recorre entidades y relaciones del corpus: la respuesta como recorrido, no como pasaje (cap. 9).
- **Entidad / arista** — Los nodos y las relaciones del grafo del corpus; toda arista lleva la cita de la píldora que la establece (cap. 9).
- **Comunidad (de grafo)** — Zona densa de entidades relacionadas, resumida para responder panoramas sin recorrer arista a arista (cap. 9).

## El precio y la decisión

- **Las cuatro monedas** — Latencia (en percentiles), coste por consulta, complejidad de operación y superficie de fallo: la contabilidad completa de cada patrón (cap. 10).
- **Latencia percibida** — Lo que el usuario siente que espera, gestionada con streaming, progreso legible y expectativa honesta; no es la latencia real (caps. 1, 10).
- **Presupuesto de vueltas / de pasos** — El máximo de iteraciones o de pasos de investigación que una consulta puede pagarse; cláusula del contrato traducida a arquitectura (caps. 5, 7, 10).
- **Señal de parada** — El criterio verificable que corta la iteración: "ya sabes lo suficiente, redacta" (cap. 5).
- **Fallo en cascada** — El modo de fallo de los patrones que encadenan pasos: un paso intermedio equivocado contamina todos los siguientes (caps. 7, 10).
- **Falso barato / falso caro** — Los dos errores del encaminamiento: la pregunta compleja respondida con una pasada, o la fácil investigada en profundidad (cap. 12).
- **Contrato de respuesta** — Lo prometido a cada audiencia: latencia, frescura, grado de cita, abstención y límites; se escribe antes de elegir el patrón, que es su consecuencia (cap. 3).
- **Matriz de decisión** — El documento que asigna patrón por familia de consultas según audiencia, alcance, consecuencia del error y urgencia (cap. 11). Documento vivo, revisada en el triaje.
- **Router** — El componente que aplica la matriz en vivo: lee las señales de cada pregunta (audiencia, composición, dominios, historia, superficie) y encamina al patrón mínimo suficiente, con derecho a escalar en caliente (cap. 12).
- **Escalado progresivo** — Barato primero, caro con pruebas: cada pregunta recibe el patrón más barato que su diagnóstico justifica y sube de peldaño solo con evidencia de insuficiencia (cap. 12).
- **Matriz de confusión (del router)** — La medición de en qué familias y en qué dirección se encamina mal, revisada en el triaje (cap. 12).

## La redacción y la vara

- **Contrato de generación** — El prompt de generación como pieza de arquitectura: papel, material, prohibiciones, citas, formato y regla de abstención — versionado y probado (cap. 13).
- **Redacción por ensamblaje** — El modelo compone con material citado en lugar de componer libre: cada afirmación remite a su píldora (cap. 13).
- **Abstención** — La respuesta "no consta en el corpus": un éxito que se celebra y se registra, con condición, forma útil y destino en el triaje (caps. 3, 13).
- **Alucinación con contexto** — Las deformaciones sobre material verdadero: añadido, redondeo, extrapolación, mezcla y cita decorativa (cap. 14).
- **Groundedness (fidelidad)** — Que cada frase de la respuesta esté respaldada en el contexto entregado; se verifica frase a frase (cap. 14).
- **Verificación de citas** — El control universal: toda cita existe, corresponde y dice lo que la respuesta dice que dice (cap. 14).
- **Regla del no promediar** — Las discrepancias entre piezas se explican y se citan, nunca se promedian ni se funden (caps. 5, 14).
- **Juez de la respuesta** — La evaluación por lote de la capa final con rúbrica de cuatro caras: fidelidad, utilidad, cita y formato-abstención (cap. 15).
- **Dataset áureo de respuesta** — Consultas reales con respuesta de referencia verificada y firmada; crece por heridas y sostiene la regresión (cap. 15).
- **Regresión de respuesta** — El examen obligatorio antes de cualquier cambio de patrón, modelo o plantilla: cero diferencias sin explicación, no cero diferencias (caps. 15, 16).
- **Cobertura graduada de la verificación** — Verificación completa por consecuencia alta, citas universales, muestreo en la cola barata (cap. 14).

## Producción y ciclo

- **Panel de respuesta** — La observabilidad por patrón: latencia en percentiles, reintentos, abstenciones, coste diario, acierto del router, fidelidad y volumen por familia (cap. 16).
- **Guardarraíles** — Los límites hasta la última milla: permisos heredados por quien pregunta, PII que no sale, auditoría completa de la respuesta (cap. 16).
- **Despliegue graduado** — El cambio serio sale a un canal de prueba antes de generalizar, con los paneles vigilando (cap. 16).
- **Triaje semanal** — La cadencia del bucle: lectura de señales y reparto de hallazgos con destino, responsable y semana límite; media hora en el libro anterior, cuarenta y cinco minutos desde que la cuarta corriente gana su fila (cap. 17; heredado del libro 2).
- **Cuarta corriente del retorno** — Los aprendizajes de la capa de respuesta con sus cuatro destinos: corpus (abstenciones), acceso (fallos de cosecha confirmados), decisión (matriz y router), generación (veredictos del juez e incidentes de fidelidad) (cap. 17).
- **Métrica del círculo completo** — El tiempo desde que una respuesta delata una carencia hasta que la carencia está resuelta; su tendencia es la prueba de que el organismo aprende (cap. 17).
- **Percentil (p50 / p95)** — La forma honesta de prometer latencia: el tiempo bajo el cual responde el 95 % de las consultas; los usuarios viven en la cola, no en la media (caps. 3, 10).

## Términos heredados de la serie

Los que este libro usa como material recibido, definidos en versión breve; sus glosarios de origen los justifican a fondo.

- **Píldora de información** — La unidad mínima del corpus: un pasaje autocontenible con su texto legible, su referencia y sus metadatos (libro 1).
- **Doble campo** — Las dos representaciones hermanas de cada píldora: el texto para leer y el vector para buscar (libro 1).
- **Dominio** — La frontera de conocimiento del corpus — la materia, el ámbito — dentro de la cual las píldoras comparten vocabulario y fuentes; acotar para dominar (libro 1).
- **Bronce / Silver / Gold** — Las calidades del material del corpus: crudo capturado, depurado y clasificado, y sintetizado y validado (libro 1).
- **Dataset áureo** — El conjunto de consultas con verdad conocida y firmada que valida cada capa; aquí tiene tres hijos: el del acceso, el de respuesta y el del corpus (libro 1).
- **Contrato de recuperación** — El acuerdo explícito sobre qué significa acertar al buscar: relevancia definida, trazabilidad, presupuesto de resultados (libro 2).
- **Híbrido** — La combinación del canal semántico y el léxico en la búsqueda: dos motores mediocres que juntos hacen uno excelente (libro 2).
- **Top-k / el tope** — Cuántos resultados entran y con qué umbrales: una política, no un número copiado (libro 2).
- **Rerank** — El segundo filtro que reordena la cosecha con un modelo más fino que el índice (libro 2).
- **Ensamblado del contexto** — La construcción final del material que el modelo leerá: orden, diversidad, presupuesto, trazabilidad (libro 2).
- **Criterio de salida** — La prueba concreta de que una pieza está dominada antes de pasar a la siguiente; el hábito de toda la serie.
- **Diario de consultas** — El registro vivo de lo que la gente pregunta y cómo fue servida: la materia prima del bucle (libro 2).
- **Bucle de retorno** — El mecanismo que convierte las señales del sistema en trabajo con destino: aquí con sus cuatro corrientes (libro 2, cap. 21).
- **Triaje** — La cadencia semanal del bucle tal como la instituyó el libro 2: media hora para leer señales y repartir hallazgos con destino, responsable y semana límite (libro 2).
- **Rack** — El corpus en producción, en la metáfora de la serie: la estantería donde cada píldora duerme junto a su doble campo (libro 1).
