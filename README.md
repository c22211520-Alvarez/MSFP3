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
4. [Formulación matemática](#modelo)
5. [Análisis temporal y de estabilidad](#analisis)
6. [Implementación computacional (Simulink y Python)](#implementacion)
7. [Control en lazo cerrado y sintonización PI](#pid)
8. [Resultados](#resultados)
9. [Estructura del repositorio](#estructura)
10. [Referencias](#referencias)

---

<a id="resumen"></a>
## Resumen

El modelado de sistemas fisiológicos permite comprender la dinámica del cuerpo humano y evaluar estrategias de control. En esta práctica, se analiza un modelo mecánico simplificado del sistema musculoesquelético basado en la representación viscoelástica descrita por Khoo. El sistema considera una fuerza activa desarrollada por el elemento contráctil, un amortiguador viscoso que representa la disipación en el tejido, y elementos elásticos en paralelo y en serie que simulan la compliancia del sarcolema y los tendones.

Mediante la **analogía fuerza-voltaje**, se evalúan dos condiciones fisiológicas: un individuo sano (control, \(R = 100\ \Omega\)) y un caso simulado de atrofia muscular (caso, \(R = 10\ \text{k}\Omega\)). Utilizando la respuesta del individuo sano como referencia, se diseña un controlador PI con retroalimentación para reducir el error. El desarrollo se realiza exclusivamente de manera *in silico* mediante MATLAB/Simulink y Python, complementado con un diagrama fisiológico elaborado en BioRender.

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

El sistema musculoesquelético se aproxima mediante un circuito eléctrico. Las equivalencias principales son: fuerza \(\leftrightarrow\) voltaje, velocidad \(\leftrightarrow\) corriente, desplazamiento \(\leftrightarrow\) carga eléctrica, amortiguador \(\leftrightarrow\) resistencia, y compliancia \(\leftrightarrow\) capacitancia.

| Parámetro | Control (Sano) | Caso (Atrofia) | Interpretación Fisiológica |
| :--- | :---: | :---: | :--- |
| **\(F(t)\)** | 1 V | 1 V | Fuerza aplicada al sistema |
| **\(\alpha\)** | 0.25 | 0.25 | Factor de transmisión de fuerza |
| **\(C_s\)** | \(10\ \mu\text{F}\) | \(10\ \mu\text{F}\) | Elasticidad en serie (tendón) |
| **\(C_p\)** | \(100\ \mu\text{F}\) | \(100\ \mu\text{F}\) | Elasticidad en paralelo (fascia/tejido pasivo) |
| **\(R\)** | \(100\ \Omega\) | \(10\ \text{k}\Omega\) | Viscosidad/Amortiguamiento del tejido |

*La diferencia entre el control y el caso radica en el aumento drástico de la resistencia \(R\), que simula la pérdida de movilidad y mayor rigidez del tejido atrofiado.*

[Volver al contenido](#contenido)

---

<a id="modelo"></a>
## Formulación matemática

*(Nota: El desarrollo algebraico paso a paso se encuentra documentado en los apuntes anexos en este repositorio).*

A partir del modelo mecánico y la aplicación del principio de superposición y la transformada de Laplace (con condiciones iniciales nulas), se obtiene la función de transferencia \(G(s) = \frac{X(s)}{F(s)}\) que relaciona el desplazamiento con la fuerza.

La estructura general de la función de transferencia de segundo orden para ambos casos es:

$$
G(s) = \frac{\alpha (C_s R s + 1)}{C_p C_s R s^2 + (C_p + C_s) s}
$$

**Ecuaciones de estado (representación integro-diferencial):**
El sistema puede representarse mediante variables de estado asociadas a los elementos almacenadores de energía (los capacitores).

[Volver al contenido](#contenido)

---

<a id="analisis"></a>
## Análisis temporal y de estabilidad

- **Error en estado estacionario:** Al evaluar el límite de \(sG(s)\) cuando \(s \to 0\), se observa que el sistema integra la entrada, comportándose como un sistema tipo 1. El error en estado estacionario ante un escalón es teóricamente cero.
- **Estabilidad:** Los polos del sistema se obtienen de igualar el denominador de \(G(s)\) a cero. Para ambos casos (control y caso), los polos se encuentran en el semiplano izquierdo del plano complejo \(s\), garantizando un sistema estable. Sin embargo, el aumento de \(R\) en el caso desplaza los polos hacia el eje real, provocando una respuesta mucho más lenta y sobreamortiguada.

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
\[
e(t) = x_{\text{control}}(t) - x_{\text{caso}}(t)
\]

**Ganancias Sintonizadas (Herramienta Tune):**
- **\(k_P\):** 0.10609
- **\(k_I\):** 7515.256
- **\(k_D\):** N/A (Controlador PI)

**Criterios de desempeño alcanzados:**
- Tiempo de establecimiento (\(t_s\)): 0.00526 s (Cumple con \(\leq 0.01\) s).
- Sobreimpulso (\(M_p\)): 0.851 % (Cumple con \(\leq 10.2\) %).
- Valor pico: 1.01.

[Volver al contenido](#contenido)

---

<a id="resultados"></a>
## Resultados

El resultado principal del proyecto se resume en la comparación de las respuestas en lazo abierto y cerrado ante un pulso de fuerza aplicado de \(t=1\) a \(t=2\) segundos:

- **Línea Control (Sana):** Alcanza un desplazamiento de \(\approx 0.75\) V.
- **Línea Caso (Atrofia en lazo abierto):** Alcanza solo \(\approx 0.48\) V debido a la alta resistencia (\(10\ \text{k}\Omega\)).
- **Línea Tratamiento (Caso con PI):** Gracias a la acción del controlador, la respuesta del caso alcanza con éxito la misma amplitud que el individuo sano (\(\approx 0.75\) V), demostrando la eficacia del controlador diseñado.

*Nota: Las gráficas comparativas detalladas se encuentran anexadas en los archivos PDF del repositorio.*

[Volver al contenido](#contenido)

---

<a id="estructura"></a>
## Estructura del repositorio

```text
.
├── README.md                                  # Este archivo
├── AlvarezC22211520.mlx                       # Cuaderno computacional de MATLAB (Live Script)
├── AlvarezC22211520.pdf                       # Cuaderno computacional de MATLAB en PDF
├── AlvarezC22211520.py                        # Implementación en Python
├── SistemaP3.slx                              # Modelo de Simulink
├── Analisis_Matematico.pdf                    # Apuntes del análisis algebraico e integro-diferencial
├── Parametros_Controlador.pdf                 # Parámetros del controlador PID
├── Diagrama_BioRender.pdf                     # Diagrama fisiológico del sistema
├── Caso_matlab.pdf                            # Gráfica de resultados exportada desde MATLAB
└── Caso_python.pdf                            # Gráfica de resultados exportada desde Python
