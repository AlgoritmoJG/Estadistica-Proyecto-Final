# Pruebas y casos aplicados

Se evaluó la lógica de cálculo con Node.js. Los valores se redondean a siete cifras significativas. Tolerancia: 0,000001 para probabilidades y 0,0001 para estadísticas. En cada fila, el resultado obtenido coincide con el esperado dentro de esa tolerancia. La interfaz debe verificarse también en el navegador del equipo de entrega.

| Módulo y entradas | Esperado / obtenido | Referencia |
|---|---|---|
| Descriptiva: 120,130,140,150,160 | media 140; varianza poblacional 200 y muestral 250 | Σ(x−media)²/N y Σ(x−media)²/(N−1) |
| Conteo: n=5, r=2; P(A)=0,4; P(B)=0,5; intersección=0,2 | combinaciones 10; unión 0,7; condicional 0,4; Bayes 0,4 | fórmulas de combinaciones, unión, condicional y Bayes |
| Binomial: n=10, p=0,5, k=5 | exacta 0,24609375; hasta 0,623046875; al menos 0,623046875 | C(10,5)·0,5¹⁰ y suma de términos |
| Hipergeométrica: N=20, K=7, n=5, k=2 | 0,3873839 | C(7,2)C(13,3)/C(20,5) |
| Poisson: tasa 4 por unidad, duración 2, k=3 | λ=8; exacta 0,02862614; acumulada 0,04238011 | e⁻⁸8³/3! y suma desde 0 |
| Uniforme: a=0, b=10, x=2, y=7 | izquierda 0,2; derecha 0,3; intervalo 0,5 | longitud del intervalo / 10 |
| Normal: μ=100, σ=15, x=85, y=115 | z=−1 y 1; intervalo 0,6826895 | tabla normal estándar |
| Gamma: α=2, θ=3, x=2, y=8 | intervalo 0,6009225 | F(x)=1−e⁻ˣ/³(1+x/3), forma 2 |
| Exponencial: λ=0,5, x=1, y=3 | intervalo 0,3834005 | e⁻⁰·⁵ − e⁻¹·⁵ |
| Ji cuadrada: ν=4, x=2, y=6 | intervalo 0,5366106 | gamma con α=2, θ=2 |
| Lognormal: μ=0, σ=1, x=1, y=2 | intervalo 0,2558914 | Φ(ln 2)−Φ(0) |
| TLC: μ=100, σ=20, n=100, media entre 98 y 102 | error estándar 2; probabilidad 0,6826895 | z=(x−μ)/(σ/√n), tabla normal |
| Inválida: datos «1 texto» | mensaje de datos no numéricos; rechazado | validación de datos |
| Inválida: normal con σ=0 | mensaje que exige σ positiva; rechazado | definición de distribución normal |

## Dos cálculos manuales

1. Descriptiva: las desviaciones son −20, −10, 0, 10 y 20; sus cuadrados suman 1000. Poblacional: 1000/5=200. Muestral: 1000/4=250.
2. Binomial: C(10,5)=252 y P(X=5)=252(0,5)¹⁰=0,24609375.

## Situaciones de Ingeniería en Sistemas

**Descriptiva — tiempos de respuesta simulados.** Cinco solicitudes tardaron 120, 130, 140, 150 y 160 ms. La media es 140 ms y la desviación poblacional 14,14214 ms. Este resumen ayuda a evaluar el centro y la variabilidad del servicio; se usa población si esos cinco registros son el conjunto completo de interés. Para generalizar a otras solicitudes, seleccione la opción muestral.

**Probabilidad — errores simulados en transmisiones.** Suponga 10 transmisiones independientes, cada una con probabilidad de error 0,5. La binomial es apropiada bajo independencia y probabilidad constante. Exactamente cinco errores tienen probabilidad 0,24609375. El supuesto es didáctico: en un sistema real debería comprobarse con registros y no asumir independencia ni esta tasa.

## Método numérico

La aplicación no usa bibliotecas estadísticas externas. Calcula combinaciones y probabilidades discretas con fórmulas; normal, gamma y ji cuadrada mediante aproximaciones numéricas de la función gamma incompleta. Los límites de cálculo indicados en pantalla y las aproximaciones pueden afectar valores extremos.

## Verificación de los casos basados en las mediciones reales

Se probaron los 12 botones CARGAR CASO y sus cálculos en Chromium mediante un servidor local, bloqueando solicitudes externas. Los resultados de abajo son esperados y obtenidos, con tolerancia absoluta de 0.000001. Se contrastaron con fórmulas y las funciones de distribución de SciPy 1.17.0; SciPy se usó únicamente para comprobar, no es una dependencia de la aplicación. Los valores originales, fecha y método de medición se detallan en CASO_REAL.pdf. Los casos de distribución son aproximaciones con supuestos, no mediciones adicionales.

| Módulo | Resultado comparado | Esperado | Obtenido |
|---|---|---:|---:|
| Descriptiva | media (ms) | 0.0060191 | 0.0060191 |
| Conteo y probabilidad | unión | 0.9 | 0.9 |
| Binomial | P(X=10) | 0.7124714 | 0.7124714 |
| Hipergeométrica | P(X=9) | 0.3333333 | 0.3333333 |
| Poisson | P(X=0) | 0.7165313 | 0.7165313 |
| Uniforme | intervalo | 0.1942299 | 0.1942299 |
| Normal | intervalo | 0.4403470 | 0.4403470 |
| Gamma | intervalo | 0.3902385 | 0.3902385 |
| Exponencial | intervalo | 0.3246272 | 0.3246272 |
| Ji cuadrada | intervalo | 0.8089902 | 0.8089902 |
| Lognormal | intervalo | 0.7391189 | 0.7391189 |
| Teorema del límite central | intervalo de media | 0.7221185 | 0.7221185 |

Las pruebas anteriores con datos didácticos permanecen como controles independientes; no describen mediciones reales. Al editar los datos de un caso, su explicación se oculta para evitar atribuir los valores nuevos a las mediciones originales.

## Comprobaciones adicionales de la interfaz

- Con P(A)=0, P(B)=0.5 y P(A∩B)=0, la aplicación informa que P(B|A) no está definida y calcula directamente P(A|B)=0.
- Tras modificar una entrada, el contexto del caso y el resultado anterior desaparecen; el resultado se actualiza al volver a pulsar CALCULAR.
- El botón de mediciones reales carga el mismo conjunto y contexto que CARGAR CASO en Descriptiva.
