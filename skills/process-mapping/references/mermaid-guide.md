# Guía Mermaid — Diagramas de flujo de procesos

Referencia de sintaxis para generar diagramas de procesos operativos.
Usar flowchart TD (top-down) como default para procesos lineales.
Usar flowchart LR (left-right) para procesos con muchos actores en paralelo.

---

## Estructura básica

```mermaid
flowchart TD
    A([Inicio]) --> B[Paso 1: acción]
    B --> C{¿Decisión?}
    C -- Sí --> D[Paso 2a]
    C -- No --> E[Paso 2b]
    D --> F[Paso 3]
    E --> F
    F --> G([Fin])
```

---

## Formas y su significado

| Forma | Sintaxis | Uso |
|---|---|---|
| Rectángulo | `[texto]` | Paso o acción |
| Rombo | `{texto}` | Decisión o condición |
| Óvalo | `([texto])` | Inicio o fin del proceso |
| Cilindro | `[(texto)]` | Base de datos o sistema |
| Estadio | `([texto])` | Evento o disparador |
| Nota | Solo texto en el nodo | Aclaración |

---

## Patrones comunes en procesos operativos

### Proceso lineal simple
```mermaid
flowchart TD
    A([Disparador]) --> B[Paso 1]
    B --> C[Paso 2]
    C --> D[👤 REVISIÓN HUMANA]
    D --> E[Paso 3]
    E --> F([Resultado])
```

### Proceso con aprobación
```mermaid
flowchart TD
    A([Solicitud recibida]) --> B[Clasificar solicitud]
    B --> C{¿Urgente?}
    C -- Sí --> D[Notificar inmediato]
    C -- No --> E[Cola normal]
    D --> F[👤 Gerente aprueba]
    E --> F
    F --> G([Respuesta enviada])
```

### Proceso con múltiples actores
```mermaid
flowchart TD
    subgraph Rep ["Representante Médico"]
        A[Registrar visitas en Excel]
        B[Subir a SharePoint]
    end
    subgraph Gerente ["Gerente Comercial"]
        C[Revisar consolidado]
        D[Aprobar reporte]
    end
    subgraph Agente ["Agente IA"]
        E[Consolidar 8 reportes]
        F[Detectar alertas]
        G[Enviar a Director]
    end
    A --> B --> E --> F --> G --> C --> D
```

---

## Convenciones para procesos con IA

Marcar siempre:
- `👤 REVISIÓN HUMANA` en pasos que requieren aprobación
- `🤖 AGENTE IA` en pasos que el agente ejecuta
- `📋 SharePoint` / `📧 Outlook` / `📊 Excel` en pasos que usan sistemas

Ejemplo:
```mermaid
flowchart TD
    A([Viernes 5pm]) --> B["🤖 AGENTE A\nRevisa carpeta SharePoint"]
    B --> C{¿Reportes completos?}
    C -- No --> D["🤖 Envía alerta\nvía Outlook"]
    C -- Sí --> E([Fin — sin acción])
    D --> F["👤 Representante\nrecibe alerta"]
    F --> G["📋 Sube reporte\na SharePoint"]
```

---

## Errores comunes a evitar

- No usar acentos dentro de los nodos (pueden romper el render): usar "accion" no "acción"
- No dejar nodos sin conectar
- No poner más de 8-10 palabras por nodo
- No anidar más de 2 niveles de subgraph
- Siempre cerrar el diagrama con un nodo de Fin
