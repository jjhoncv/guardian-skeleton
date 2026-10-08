# __NOMBRE__

> El título de arriba (`# ...`) es el nombre que muestra la página en producción.
> Este documento es el alcance congelado. Lo que no esté aquí va al **Parking lot**.

## 1. Problema

Qué duele hoy y a quién.

## 2. Qué es

En una o dos frases.

## 3. Qué NO es

Lo que alguien podría esperar y queda fuera a propósito.

## 4. Valor

Qué cambia para el usuario cuando esto está en producción.

## 5. Cómo sé que funcionó

Algo medible y con plazo (p. ej. «5 personas comentan en 2 semanas»). Al cerrar la última fase se compara contra esto.

## 6. Límites

Semanas disponibles, presupuesto y cuentas externas que hacen falta.

- **Semanas:** N (en total; el Guardián las reparte en una fecha objetivo por fase).
- **Tipo:** prueba / MVP rápido | producto. Define cuánto construir (Claude lo lee antes de cada ticket).

## 7. Fases (máximo 5)

Cada fase es un entregable usable en producción.

### Fase 1 — <nombre>
- <entregable>

**Valor:** <qué se puede hacer al cerrar la fase>

## 8. Criterios de aceptación

```gherkin
Escenario: <nombre>
  Dado <contexto>
  Cuando <acción>
  Entonces <resultado observable>
```

El avance del proyecto = % de estos escenarios en verde.

## 9. Parking lot

- <idea> — <fecha> — <por qué no entra ahora>

## 10. Decisiones tomadas

| Fecha | Decisión | Motivo |
|---|---|---|
