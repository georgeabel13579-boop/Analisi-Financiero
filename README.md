# Prompt: Análisis Financiero Senior sobre Balance de Comprobación (Bs)

## Rol
Actúa como **analista financiero senior** con experiencia en modelación, proyecciones y control de gastos.

## Entrada
- Archivo Excel adjunto: **balance de comprobación de un solo año**, expresado en **bolivianos (Bs)**.
- Antes de analizar, confirma que el archivo está presente y lee sus hojas, columnas y rangos. Si falta algo, dilo y pregúntalo; no lo inventes.

## Reglas obligatorias
1. **No inventes cifras.** Todo número debe provenir del Excel o de un cálculo trazable a él.
2. Marca cada premisa no verificable como **[SUPUESTO]**, con su justificación y rango.
3. Separa siempre en secciones distintas:
   - **HECHOS** (datos del Excel)
   - **ESTIMACIONES** (proyecciones y simulaciones)
4. Toda cifra va en **Bs**, con separador de miles y 2 decimales.
5. Si un dato es insuficiente (p. ej., un solo año sin estacionalidad), declara la limitación y su efecto en la fiabilidad.
6. Usa **mediana** como medida central de referencia (más robusta que la media ante valores atípicos).

---

## Fase 1 · Validación de datos
- Verifica que **Débitos = Créditos** y que los saldos cuadren.
- Detecta: cuentas duplicadas, saldos con naturaleza invertida, cuentas sin movimiento, valores atípicos, celdas vacías o texto en campos numéricos, y clasificaciones dudosas.
- Entrega una tabla: `Cuenta | Inconsistencia | Gravedad (Alta/Media/Baja) | Tratamiento propuesto`.
- Indica qué datos quedan excluidos o ajustados y por qué.

## Fase 2 · Proyección de ventas a 24 meses
Tres escenarios: **Conservador, Base y Optimista**.
- Método: parte de la **mediana** de ventas mensuales históricas (o de la reconstrucción mensual posible).
- Para cada escenario, tabla de supuestos explícitos:

| Supuesto | Conservador | Base | Optimista | Justificación |
|---|---|---|---|---|
| Crecimiento mensual [SUPUESTO] | | | | |
| Estacionalidad [SUPUESTO] | | | | |
| Inflación / tipo de cambio [SUPUESTO] | | | | |

- **Bandas de incertidumbre** (p. ej., P10–P90) alrededor de cada escenario.
- **Análisis de sensibilidad:** variación de ±10 % / ±20 % en las variables clave y su efecto sobre ventas y resultado.

## Fase 3 · Plan de reducción de gastos
- **Análisis de Pareto (80/20):** identifica los rubros que concentran el mayor gasto.
- Para cada medida propone:

| Medida | Rubro | Ahorro estimado (Bs) | Ahorro (%) | Plazo | Prioridad | Riesgo | Supuesto |
|---|---|---|---|---|---|---|---|

- Clasifica por plazo: **corto (0-3 m), mediano (3-12 m), largo (12-24 m)**.
- Prioriza con criterio explícito (impacto vs. facilidad de implementación).
- Cuantifica el ahorro **acumulado** y su efecto en el margen.

## Fase 4 · Estados financieros proyectados (24 meses)
1. **Estado de resultados proyectado** por escenario (mensual y anual): ventas, costo de ventas, utilidad bruta, gastos operativos, EBITDA, utilidad neta.
2. **Flujo de caja proyectado** por escenario: operativo, inversión, financiamiento, saldo inicial y final, mes de menor liquidez. Marca como [SUPUESTO] los plazos de cobro y pago.
3. **Punto de equilibrio:** separa costos fijos y variables (indica el criterio), calcula el equilibrio en Bs y en unidades/porcentaje de ventas, y el margen de seguridad por escenario.

## Fase 5 · Simulación Monte Carlo simple
- Mínimo **10.000 iteraciones**, con semilla fija (`seed=42`) para reproducibilidad.
- Variables aleatorias: crecimiento de ventas, margen bruto y nivel de gastos (distribuciones declaradas como [SUPUESTO], p. ej. triangular o normal truncada).
- Resultados: mediana, P5, P25, P75, P95, **probabilidad de utilidad negativa** y **probabilidad de caja negativa**.

## Fase 6 · Gráficas (claras, con título, ejes, unidades en Bs y leyenda)
1. Ventas proyectadas por escenario con bandas de incertidumbre
2. Pareto de gastos (barras + línea acumulada)
3. Cascada (waterfall) del ahorro: gasto actual → medidas → gasto optimizado
4. Flujo de caja proyectado por escenario
5. Tornado de sensibilidad
6. Dashboard resumen (KPIs, escenarios, equilibrio y resultados Monte Carlo)

Guarda cada gráfica en `/outputs/graficos/` en formato PNG (300 dpi).

## Formato de entrega
1. **Resumen ejecutivo** (máx. 10 líneas): hallazgos clave y recomendaciones.
2. Secciones por fase, con **HECHOS** y **ESTIMACIONES** separados.
3. Tabla consolidada de todos los **[SUPUESTO]**.
4. **Limitaciones y riesgos** del análisis.
5. Código reproducible (Python: `pandas`, `numpy`, `matplotlib`) en `/outputs/codigo/analisis.py`.
6. Excel de salida `/outputs/modelo_financiero.xlsx` con hojas: Validación, Ventas, Gastos, ER, Flujo, Equilibrio, MonteCarlo.

## Criterios de calidad
- Cada cifra es trazable a una celda del Excel o a una fórmula.
- Ninguna conclusión sin su respaldo numérico.
- Si algo no puede calcularse con los datos disponibles, se indica claramente en lugar de estimarlo sin base.
