# Calculadora de Copilot Credits

Estima el costo mensual de un agente de Copilot Studio **antes** de construirlo o cotizarlo.

**[→ Abrir la calculadora](https://TU-USUARIO.github.io/copilot-credits-calculator/)**

![Vista previa de la calculadora](docs/preview.png)

---

## El problema

Microsoft publicó `ConsumptionCentral for Microsoft Copilot`, que te dice cuánto **gastaste**. No existe una herramienta abierta que te diga cuánto vas a **gastar** antes de comprometer un presupuesto o cotizarle a un cliente.

Y el problema es real: un solo turno de conversación puede costar desde 1 crédito (tema guionado) hasta 112 o más (grounding de tenant graph + razonamiento). Un presupuesto basado en un demo con temas guionados se queda corto por un orden de magnitud frente a una carga de producción que se apoya en respuestas generativas.

## Qué hace

- Calcula créditos y costo mensual a partir de volumen, mezcla de interacciones y tipo de licenciamiento
- Muestra un **rango**, no un número falsamente preciso, con margen de incertidumbre configurable
- Desglosa de dónde viene el consumo, para que veas si una sola capacidad está dominando el costo
- Compara pay-as-you-go contra paquete de capacidad, y te avisa si vas a desperdiciar un paquete
- Aplica correctamente las exenciones de licencia, incluyendo las dos excepciones que más se pasan por alto
- Genera un resumen copiable con todos los supuestos visibles, listo para pegar en una propuesta

## Lo que la hace distinta de una hoja de cálculo

**Los supuestos son el entregable, no el total.** Cualquiera puede multiplicar volumen por tarifa. Lo que rompe presupuestos es no ver que el grounding representa 53% del costo siendo solo 30% de los turnos. La calculadora te lo señala.

**Conoce los casos borde del licenciamiento.** El harness de GitHub Copilot no tiene exención de créditos ni siquiera para usuarios con licencia M365 Copilot, y cobra desde la fase de construcción. Los agentes externos consumen créditos sin importar qué licencias tenga la organización. Ambos casos están modelados.

**Te dice cuándo NO comprar un paquete.** Si vas a usar menos del 70% de un paquete de capacidad, te muestra cuánto te costaría pay-as-you-go en su lugar — porque los créditos no consumidos no se acumulan al mes siguiente.

## Tabla de tarifas usada

| Capacidad | Créditos por interacción |
|---|---|
| Respuesta clásica (tema guionado) | 1 |
| Respuesta generativa | 2 |
| Acción de agente / llamada a herramienta | 5 |
| Grounding con tenant graph | 10 |
| Acción autónoma | 25 |
| Modelo de razonamiento | ~100 por cada 10 respuestas |

**Precios:** $0.01 por crédito en pay-as-you-go · $200/mes por paquete de 25.000 créditos (≈$0.008/crédito, compromiso anual)

Las tarifas viven en [`rate-card.json`](./rate-card.json) para que sea fácil actualizarlas. Si Microsoft cambia una tarifa, edita ese archivo y abre un PR.

## Cómo usarla

**Opción 1 — En línea:** abre el enlace de arriba. No requiere instalación ni cuenta.

**Opción 2 — Localmente:** descarga `index.html` y ábrelo en cualquier navegador. Es un archivo único, sin dependencias, sin llamadas de red. Funciona sin conexión.

**Opción 3 — En tu propio GitHub Pages:** haz fork del repo, ve a Settings → Pages, y publica desde la rama `main`.

## Cómo usarla en una conversación con un cliente

1. **Entrega el rango con los supuestos visibles**, no solo el total. La pantalla completa es el entregable — si el cliente ve que asumiste 4 turnos por conversación, puede corregirte antes de que el número sea un compromiso.
2. **Corre dos escenarios**: uno conservador (mayormente guionado) y uno realista (mezcla generativa alta). La diferencia entre ambos abre la conversación de arquitectura.
3. **Piloteen con pay-as-you-go** antes de comprometer un paquete anual. Midan un mes real primero.
4. **Si el número sale alto, la palanca es arquitectura, no precio.** Revisa si el agente necesita grounding en cada turno o solo en algunos.

## Advertencia importante

Esto es una **estimación**, no una cotización. Las tarifas cambian y el consumo real depende del comportamiento de los usuarios. Verifica siempre contra la [documentación oficial de Microsoft](https://learn.microsoft.com/en-us/microsoft-copilot-studio/billing-licensing) antes de comprometer presupuesto.

Tarifas verificadas por última vez: **septiembre de 2026**.

## Contribuir

Las tarifas de Microsoft cambian. Si notas que alguna quedó desactualizada:

1. Edita `rate-card.json` y el arreglo `RATES` en `index.html`
2. Actualiza la fecha `_lastVerified`
3. Abre un PR citando la fuente oficial del cambio

También son bienvenidos: traducciones, escenarios preconfigurados para casos de uso comunes, o mejoras al motor de recomendaciones.

## Licencia

MIT — úsala, adáptala, incorpórala a tus propias herramientas.
