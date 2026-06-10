# Guía BPMN 2.0 — Process Mapping

Referencia de notación, patrones, y generación de archivos `.bpmn` (XML) compatibles con Camunda, Bizagi, draw.io, y Signavio.

---

## Clasificación de tareas para automatización

Cada elemento BPMN tiene un rol de automatización predefinido.
Usar esta tabla al completar el campo Task Type en la ficha de proceso:

| Elemento BPMN | Clasificación | Ruta de automatización |
|---|---|---|
| User Task | Manual | El humano permanece; candidato a automatización parcial |
| Service Task | AI-automatable | Reemplazar con agente o script Python |
| Exclusive Gateway (XOR) | Rule-based | Reemplazar con motor de reglas o clasificador ML |
| Data Object | AI-automatable | Reemplazar con script de transformación de datos |
| Start / End Event | Rule-based | Disparar con scheduler o llamada API |

Cada tarea AI-automatable en el diagrama BPMN corresponde a un agente o endpoint independiente.
Ver `ai-agent-builder` skill para el diseño de cada agente.

---

## Cuándo usar BPMN en lugar de Mermaid

| Usar BPMN | Usar Mermaid |
|---|---|
| El proceso se entregará a un equipo técnico o de TI | El proceso es para presentación rápida |
| Se necesita importar a software de modelado | Solo se necesita visualizar en reunión |
| Hay múltiples actores / pools / lanes | El flujo es lineal con pocos actores |
| Se diseñará automatización real (RPA, BPA, agente) | El output es solo documentación |
| El cliente o empleador usa estándar BPMN 2.0 | No se requiere interoperabilidad |

---

## Elementos BPMN 2.0 — Referencia rápida

### Eventos (Events)

| Elemento | Nombre | Uso |
|---|---|---|
| ○ círculo delgado | Start Event | Inicio del proceso |
| ○ círculo doble | End Event | Fin del proceso |
| ◎ círculo con icono | Intermediate Event | Algo ocurre en el medio (timer, mensaje, error) |
| ⊙ con reloj | Timer Event | Disparador por tiempo |
| ✉ con sobre | Message Event | Disparador por mensaje externo |

### Tareas (Tasks)

| Elemento | Nombre | Uso |
|---|---|---|
| ▭ rectángulo | User Task | Una persona ejecuta la acción |
| ▭ con engrane | Service Task | Sistema o automatización la ejecuta |
| ▭ con persona+engrane | Manual Task | Tarea física sin sistema |
| ▭ con script | Script Task | Código o regla automática |
| ▭ con flecha | Send/Receive Task | Envío o recepción de mensajes |

### Gateways (Decisiones y sincronización)

| Elemento | Nombre | Uso |
|---|---|---|
| ◇ con X | Exclusive Gateway (XOR) | Solo un camino posible (if/else) |
| ◇ con + | Parallel Gateway (AND) | Todos los caminos se ejecutan |
| ◇ con O | Inclusive Gateway (OR) | Uno o más caminos posibles |
| ◇ con pentágono | Event-Based Gateway | La decisión depende de qué evento ocurre primero |

### Artefactos

| Elemento | Nombre | Uso |
|---|---|---|
| [ ] con oreja | Data Object | Dato que entra o sale de una tarea |
| Cilindro | Data Store | Base de datos o sistema consultado |
| Nota | Annotation | Comentario o aclaración |

### Contenedores

| Elemento | Uso |
|---|---|
| **Pool** | Representa a una organización o proceso completo |
| **Lane** | Carril dentro del pool — representa un actor o sistema |

---

## Convenciones de tiempo y sistemas en BPMN

Cada tarea debe llevar en su etiqueta o en la documentación asociada:

```
[Nombre de la tarea]
Sistema: [nombre del sistema]
Tiempo de ejecución: [N min]
Tiempo de espera: [N min / horas / días]
Tipo: [User Task / Service Task / Manual Task]
```

Cuando se genere el XML, incluir en el campo `<documentation>` de cada `<task>`:

```xml
<documentation>
  Sistema: SAP
  Tiempo ejecucion: 15 min
  Tiempo espera: 2 horas
  Actor: Analista de Compras
</documentation>
```

---

## Patrones de proceso comunes en BPMN

### Proceso secuencial con aprobación

```
[Start] → [User Task: Capturar solicitud] → [XOR: ¿Aprobada?]
           Sistema: Email / Forms                ├─ Sí → [User Task: Ejecutar] → [End]
                                                 └─ No → [User Task: Notificar rechazo] → [End]
```

### Proceso paralelo (dos tareas simultáneas)

```
[Start] → [Parallel Gateway +] → [User Task A] → [Parallel Gateway +] → [End]
                                → [User Task B] ↗
```

### Proceso con timer (espera programada)

```
[Start] → [User Task] → [Timer Intermediate Event: 24h] → [Service Task: Auto-notificar] → [End]
```

### Proceso multi-carril (swim lanes)

```
Pool: Proceso de Compras
├── Lane: Solicitante    → [Crear solicitud] → [Esperar aprobación]
├── Lane: Gerente        → [Revisar] → [XOR: Aprobar/Rechazar]
└── Lane: Compras        → [Emitir OC] → [Confirmar proveedor]
```

---

## Cómo generar el archivo .bpmn (XML)

### Estructura base del XML BPMN 2.0

```xml
<?xml version="1.0" encoding="UTF-8"?>
<definitions xmlns="http://www.omg.org/spec/BPMN/20100524/MODEL"
             xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
             xmlns:activiti="http://activiti.org/bpmn"
             xmlns:bpmndi="http://www.omg.org/spec/BPMN/20100524/DI"
             xmlns:omgdc="http://www.omg.org/spec/DD/20100524/DC"
             xmlns:omgdi="http://www.omg.org/spec/DD/20100524/DI"
             typeLanguage="http://www.w3.org/2001/XMLSchema"
             expressionLanguage="http://www.w3.org/1999/XPath"
             targetNamespace="http://www.activiti.org/test">

  <process id="proceso_[nombre]" name="[Nombre del Proceso]" isExecutable="false">

    <!-- START EVENT -->
    <startEvent id="start" name="[Disparador]"/>

    <!-- TAREAS -->
    <userTask id="task_1" name="[Nombre tarea]">
      <documentation>
        Sistema: [sistema]
        Tiempo ejecucion: [N min]
        Tiempo espera: [N min]
        Actor: [actor]
      </documentation>
    </userTask>

    <!-- GATEWAY -->
    <exclusiveGateway id="gw_1" name="[Pregunta de decisión]"/>

    <!-- END EVENT -->
    <endEvent id="end" name="[Resultado]"/>

    <!-- SEQUENCE FLOWS -->
    <sequenceFlow id="flow_1" sourceRef="start" targetRef="task_1"/>
    <sequenceFlow id="flow_2" sourceRef="task_1" targetRef="gw_1"/>
    <sequenceFlow id="flow_3" name="Sí" sourceRef="gw_1" targetRef="end"/>

  </process>

  <!-- DIAGRAM INTERCHANGE (posicionamiento visual) -->
  <bpmndi:BPMNDiagram id="BPMNDiagram_1">
    <bpmndi:BPMNPlane id="BPMNPlane_1" bpmnElement="proceso_[nombre]">

      <bpmndi:BPMNShape id="start_di" bpmnElement="start">
        <omgdc:Bounds x="100" y="200" width="36" height="36"/>
      </bpmndi:BPMNShape>

      <bpmndi:BPMNShape id="task_1_di" bpmnElement="task_1">
        <omgdc:Bounds x="180" y="180" width="120" height="80"/>
      </bpmndi:BPMNShape>

      <bpmndi:BPMNEdge id="flow_1_di" bpmnElement="flow_1">
        <omgdi:waypoint x="136" y="218"/>
        <omgdi:waypoint x="180" y="218"/>
      </bpmndi:BPMNEdge>

    </bpmndi:BPMNPlane>
  </bpmndi:BPMNDiagram>

</definitions>
```

### Reglas de generación del XML

1. IDs únicos por elemento: `start`, `task_1`, `task_2`, `gw_1`, `flow_1`, etc.
2. Todos los `sequenceFlow` deben tener `sourceRef` y `targetRef` válidos.
3. Incluir `<documentation>` en cada task con sistema, tiempo, y actor.
4. El `bpmndi:BPMNDiagram` es necesario para render visual en software BPMN.
5. Posiciones X/Y: incrementar x en 160 por cada paso en secuencia. Y base: 180.
6. Para swim lanes, usar `<laneSet>` dentro del `<process>`:

```xml
<laneSet id="laneSet_1">
  <lane id="lane_actor1" name="[Actor 1]">
    <flowNodeRef>task_1</flowNodeRef>
    <flowNodeRef>task_2</flowNodeRef>
  </lane>
  <lane id="lane_actor2" name="[Actor 2]">
    <flowNodeRef>task_3</flowNodeRef>
  </lane>
</laneSet>
```

### Compatibilidad por herramienta

| Herramienta | Compatibilidad | Notas |
|---|---|---|
| draw.io / diagrams.net | ✅ Importación directa | File → Import → XML |
| Bizagi Modeler | ✅ Completa | Requiere namespace Bizagi si se exporta de ahí |
| Camunda Modeler | ✅ Completa | Soporta `activiti:` extensions |
| Signavio | ✅ Completa | Mejor con namespace OMG puro |
| Lucidchart | ✅ Parcial | No soporta todos los elementos de DI |
| Visio | ⚠️ Manual | No importa BPMN XML directamente |

---

## Output esperado al generar BPMN

1. Ficha de proceso completa (SKILL.md nivel 3) con columnas de sistema y tiempo
2. Diagrama Mermaid (para visualización rápida en chat)
3. Archivo `.bpmn` (XML) guardado como artifact descargable
4. Slide de PowerPoint con swim-lane diagram (leer `pptx-process-diagram.md`)
