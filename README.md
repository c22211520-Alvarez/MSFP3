[![Open in MATLAB Online](https://www.mathworks.com/images/responsive/global/open-in-matlab-online.svg)](https://matlab.mathworks.com/open/github/v1?repo=c22211520-Alvarez/MSFP3)

# Práctica 3: Modelado y control del sistema musculoesquelético

Repositorio académico de la asignatura **Modelado de Sistemas Fisiológicos**, correspondiente al programa de **Ingeniería Biomédica** del Tecnológico Nacional de México / Instituto Tecnológico de Tijuana. Esta práctica integra el modelado matemático mediante analogías electromecánicas, el análisis de estabilidad, el diseño de controladores clásicos y la experimentación *in silico* (Simulink y Python) enfocada en el sistema musculoesquelético.

El caso de estudio representa la dinámica viscoelástica de un músculo mediante un circuito eléctrico equivalente (analogía fuerza-voltaje), comparando la respuesta de un individuo sano (control) frente a un caso simulado de atrofia muscular, y aplicando un controlador PI para mitigar la diferencia entre ambas respuestas.

**Docente:** Dr. Paul Antonio Valle Trujillo  
**Correo:** paul.valle@tectijuana.edu.mx  
**Departamento:** Ingeniería Eléctrica y Electrónica  
**Institución:** Tecnológico Nacional de México / Instituto Tecnológico de Tijuana  
**Asignatura:** Modelado de Sistemas Fisiológicos  
**Programa:** Ingeniería Biomédica  
**Estudiante:** Gabriela Gissele Alvarez Garcia · C22211520 · c22211520@tijuana.tecnm.mx

**Repositorio de la asignatura:** https://github.com/DrPaulValle/MSF-IBF1020
**Sede:** Blvd. Alberto Limón Padilla s/n, Tijuana, C. P. 22454, B. C., México. 

---

<a id="contenido"></a>
## Contenido

1. [Resumen](#resumen)
2. [Competencia y objetivos](#objetivos)
3. [Modelo físico y analogía eléctrica](#analogia)
4. [Formulación matemática y Procedimiento Algebraico](#modelo)
5. [Análisis temporal y de estabilidad](#analisis)
6. [Implementación computacional (Simulink y Python)](#implementacion)
7. [Control en lazo cerrado y sintonización PI](#pid)
8. [Diagrama Fisiológico (BioRender)](#biorender)
9. [Resultados](#resultados)
10. [Estructura del repositorio](#estructura)
11. [Referencias](#referencias)

---

<a id="resumen"></a>
## Resumen

El modelado de sistemas fisiológicos permite comprender la dinámica del cuerpo humano y evaluar estrategias de control. En esta práctica, se analiza un modelo mecánico simplificado del sistema musculoesquelético basado en la representación viscoelástica descrita por Khoo. El sistema considera una fuerza activa desarrollada por el elemento contráctil, un amortiguador viscoso que representa la disipación en el tejido, y elementos elásticos en paralelo y en serie que simulan la compliancia del sarcolema y los tendones.

Mediante la **analogía fuerza-voltaje**, se evalúan dos condiciones fisiológicas: un individuo sano (control, $R = 100\ \Omega$) y un caso simulado de atrofia muscular (caso, $R = 10\ \text{k}\Omega$). Utilizando la respuesta del individuo sano como referencia, se diseña un controlador PI con retroalimentación para reducir el error. El desarrollo se realiza exclusivamente de manera *in silico* mediante MATLAB/Simulink y Python, complementado con un diagrama fisiológico elaborado en BioRender.

**Palabras clave:** Sistema musculoesquelético; Analogía fuerza-voltaje; Modelo viscoelástico; Atrofia muscular; Controlador PI; Simscape Electrical; Python; BioRender.

[Volver al contenido](#contenido)

---

<a id="objetivos"></a>
## Competencia y objetivos

### Competencia a desarrollar
Modeliza sistemas fisiológicos mediante analogías entre sistemas físicos, ecuaciones diferenciales y funciones de transferencia para analizar su comportamiento dinámico y diseña estrategias de control mediante técnicas de control clásico.

### Objetivos específicos
- Analizar el modelo mecánico simplificado del sistema musculoesquelético.
- Construir el circuito eléctrico equivalente mediante la analogía fuerza-voltaje.
- Obtener el modelo de ecuaciones integro-diferenciales y las funciones de transferencia del control y del caso.
- Evaluar la estabilidad y el error en estado estacionario de ambos escenarios.
- Implementar el modelo en Simulink (ecuaciones, función de transferencia y circuito) y Python.
- Sintonizar un controlador PI para reducir la diferencia entre el caso de atrofia y la referencia sana.
- Elaborar un diagrama biológico del sistema en BioRender.

[Volver al contenido](#contenido)

---

<a id="analogia"></a>
## Modelo físico y analogía eléctrica

El sistema musculoesquelético se aproxima mediante un circuito eléctrico. Las equivalencias principales son: fuerza $\leftrightarrow$ voltaje, velocidad $\leftrightarrow$ corriente, desplazamiento $\leftrightarrow$ carga eléctrica, amortiguador $\leftrightarrow$ resistencia, y compliancia $\leftrightarrow$ capacitancia.

| Parámetro | Control (Sano) | Caso (Atrofia) | Interpretación Fisiológica |
| :--- | :---: | :---: | :--- |
| **$F(t)$** | 1 V | 1 V | Fuerza aplicada al sistema |
| **$\alpha$** | 0.25 | 0.25 | Factor de transmisión de fuerza |
| **$C_s$** | $10\ \mu\text{F}$ | $10\ \mu\text{F}$ | Elasticidad en serie (tendón) |
| **$C_p$** | $100\ \mu\text{F}$ | $100\ \mu\text{F}$ | Elasticidad en paralelo (fascia/tejido pasivo) |
| **$R$** | $100\ \Omega$ | $10\ \text{k}\Omega$ | Viscosidad/Amortiguamiento del tejido |

*La diferencia entre el control y el caso radica en el aumento drástico de la resistencia $R$, que simula la pérdida de movilidad y mayor rigidez del tejido atrofiado.*

[Volver al contenido](#contenido)

---

<a id="modelo"></a>
## Formulación matemática y Procedimiento Algebraico

A partir del análisis del circuito equivalente y aplicando la transformada de Laplace (con condiciones iniciales nulas), se obtiene el siguiente sistema de ecuaciones integro-diferenciales que rige la dinámica del sistema:

**1. Ecuación diferencial del desplazamiento:**

$$
\frac{dx(t)}{dt} = -\frac{x(t)}{R(C_p + C_s)} + \frac{F(t)}{R(C_p + C_s)}
$$

**2. Ecuación de la fuerza de salida:**

$$
F_s(t) = \frac{C_s}{C_p + C_s}F(t) + x(t)\left(1 - \alpha - \frac{C_s}{C_p + C_s}\right)
$$

**Procedimiento Algebraico para obtener la Función de Transferencia (Lazo Abierto):**
Para obtener $G(s) = \frac{F_s(s)}{F(s)}$, aplicamos la Transformada de Laplace a la primera ecuación para despejar $X(s)$:

$$
sX(s) = -\frac{1}{R(C_p + C_s)}X(s) + \frac{1}{R(C_p + C_s)}F(s)
$$

$$
X(s) \left[ s + \frac{1}{R(C_p + C_s)} \right] = \frac{1}{R(C_p + C_s)}F(s)
$$

$$
X(s) = \frac{\frac{1}{R(C_p + C_s)}}{s + \frac{1}{R(C_p + C_s)}} F(s)
$$

Ahora, aplicamos Laplace a la segunda ecuación:

$$
F_s(s) = \frac{C_s}{C_p + C_s}F(s) + \left(1 - \alpha - \frac{C_s}{C_p + C_s}\right)X(s)
$$

Sustituyendo $X(s)$ en la ecuación de $F_s(s)$ y simplificando, se obtiene la función de transferencia de lazo abierto que relaciona el desplazamiento con la fuerza aplicada. Esta ecuación se implementa directamente mediante el método de Euler en el Live Script para su simulación numérica.

[Volver al contenido](#contenido)

---

<a id="analisis"></a>
## Análisis temporal y de estabilidad

- **Error en estado estacionario:** Al evaluar el límite de $sG(s)$ cuando $s \to 0$, se observa que el sistema integra la entrada, comportándose como un sistema tipo 1. El error en estado estacionario ante un escalón es teóricamente cero.
- **Estabilidad:** Los polos del sistema se obtienen de igualar el denominador de $G(s)$ a cero. Para ambos casos (control y caso), los polos se encuentran en el semiplano izquierdo del plano complejo $s$, garantizando un sistema estable. Sin embargo, el aumento de $R$ en el caso desplaza los polos, provocando una respuesta mucho más lenta y sobreamortiguada (como se evidencia en el caso sin control).

[Volver al contenido](#contenido)

---

<a id="implementacion"></a>
## Implementación computacional (Simulink y Python)

La experimentación *in silico* se llevó a cabo utilizando dos herramientas para realizar una verificación cruzada:

1. **MATLAB / Simulink:** Se construyeron tres representaciones equivalentes:
   - Ecuaciones integro-diferenciales mediante bloques.
   - Función de transferencia.
   - Circuito equivalente mediante *Simscape Electrical*.
2. **Python (Visual Studio Code):** Se implementaron las funciones de transferencia del control y del caso. Se utilizó la librería `control` para simular el sistema en lazo abierto y cerrado utilizando la misma señal de entrada (pulso de 1 a 2 segundos).

[Volver al contenido](#contenido)

---

<a id="pid"></a>
## Control en lazo cerrado y sintonización PI

Se utilizó un controlador PI en Simulink para que la respuesta del caso (atrofia) intentara igualar la respuesta del control (sano). La señal de error se define como:

$$
e(t) = x_{\text{control}}(t) - x_{\text{caso}}(t)
$$

**Ganancias Sintonizadas (Herramienta Tune):**
- **$k_P$:** 0.10609
- **$k_I$:** 7515.256
- **$k_D$:** N/A (Controlador PI)

**Criterios de desempeño alcanzados:**
- Tiempo de establecimiento ($t_s$): 0.00526 s (Cumple con $\leq 0.01$ s).
- Sobreimpulso ($M_p$): 0.851 % (Cumple con $\leq 10.2$ %).
- Valor pico: 1.01.

[Volver al contenido](#contenido)

---

<a id="biorender"></a>
## Diagrama Fisiológico (BioRender)

Para vincular el modelo matemático y eléctrico con la anatomía real, se elaboró un diagrama del sistema musculoesquelético utilizando la plataforma **BioRender**. 

En este diagrama se ilustra la extensión del músculo y se mapean los componentes de la siguiente manera:
- **$F(t)$:** Fuerza externa aplicada al sistema.
- **$C_p$ (Elemento en paralelo):** Representa la elasticidad de la fascia y el tejido conectivo pasivo que envuelve al músculo.
- **$C_s$ (Elemento en serie):** Representa la elasticidad del tendón que transmite la fuerza al hueso.
- **$R$ (Amortiguador):** Representa la viscosidad interna del tejido. El diagrama incluye un recuadro específico para el **Caso de Atrofia/Daño**, mostrando visualmente cómo el tejido presenta una mayor resistencia ($R = 10\ \text{k}\Omega$).
- **$x(t)$ y $x_1(t)$:** Desplazamiento total y desplazamiento a nivel del tendón.

*El archivo PDF original del diagrama se encuentra anexado en este repositorio.*

[Volver al contenido](#contenido)

---

<a id="resultados"></a>
## Resultados

El resultado principal del proyecto se resume en la comparación de las respuestas en lazo abierto y cerrado ante un pulso de fuerza aplicado de $t=1$ a $t=2$ segundos (ver gráficas en los PDF anexos):

- **Línea Control (Sana):** Alcanza un desplazamiento de $\approx 0.75$ V.
- **Línea Caso (Atrofia en lazo abierto):** Alcanza solo $\approx 0.48$ V debido a la alta resistencia ($10\ \text{k}\Omega$).
- **Línea Tratamiento (Caso con PI):** Gracias a la acción del controlador, la respuesta del caso alcanza con éxito la misma amplitud que el individuo sano ($\approx 0.75$ V), demostrando la eficacia del controlador diseñado.

[Volver al contenido](#contenido)

---

<a id="referencias"></a>
## Referencias

[1] M. C. Khoo, *Physiological Control Systems: Analysis, Simulation, and Estimation*, 2nd ed., IEEE Press, 2018, sección 4, p. 93.

[2] F. C. Hoppensteadt y C. S. Peskin, *Modeling and Simulation in Medicine and the Life Sciences*, Springer.

[3] J. E. Hall, *Guyton and Hall Textbook of Medical Physiology*, 14th ed., Elsevier, 2021.

[4] K. Ogata, *Ingeniería de Control Moderna*, 5.ª ed., Pearson Educación, 2010.

[5] N. S. Nise, *Control Systems Engineering*, 8th ed., John Wiley & Sons, 2020.

[6] P. A. Valle, *Syllabus para Modelado de Sistemas Fisiológicos*, Tecnológico Nacional de México / Instituto Tecnológico de Tijuana, Tijuana, B. C., México, 2026. Disponible en: https://biomath.xyz/course/

[7] Tecnológico Nacional de México, *Programa de estudio de la asignatura Modelado de Sistemas Fisiológicos, IBF-1020*.

---

## Uso académico

Este repositorio se utiliza como evidencia de una práctica introductoria de **Modelado de Sistemas Fisiológicos**. El procedimiento se fundamenta en materiales y resultados de referencia proporcionados por la asignatura. Su finalidad es desarrollar habilidades de interpretación, reproducción computacional, verificación experimental y documentación técnica; cualquier cambio de parámetros, estructura del controlador o procedimiento debe identificarse y justificarse.
