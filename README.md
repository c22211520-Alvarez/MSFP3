# Práctica 3: Modelado y control del sistema musculoesquelético

## Información de la estudiante

Judith Valeria Soriano Angeles [23212231]; l23212231@tectijuana.edu.mx

Modelado de Sistemas Fisiológicos

Ingeniería Biomédica

## Docente

Dr. Paul Antonio Valle Trujillo; paul.valle@tectijuana.edu.mx

Departamento de Ingeniería Eléctrica y Electrónica, Tecnológico Nacional de México/IT Tijuana, Blvd. Alberto Limón Padilla s/n, Tijuana, C.P. 22454, B.C., México.

## Descripción de la asignatura

El modelizado de sistemas fisiológicos es una herramienta importante en Ingeniería Biomédica, permite comprender el funcionamiento del cuerpo humano, así como diseñar y evaluar terapias y dispositivos médicos; se define como el proceso de formular modelos matemáticos o computacionales que representan el comportamiento y la interacción de los sistemas biológicos y fisiológicos. Esta asignatura pretende aportar al perfil del Ingeniero Biomédico la capacidad de realizar investigación científica en el área de Biología de Sistemas con la finalidad de dirigir y participar en equipos de trabajo interdisciplinarios en contextos nacionales e internacionales, así como de proporcionar soluciones informáticas para resolver problemas en el campo de la Ingeniería Biomédica con ética profesional; lo anterior al proporcionar al estudiante bases sólidas para modelizar sistemas y diseñar controladores para la solución de problemas en las áreas de atención médica y del sector industrial médico. La construcción de analogías entre circuitos eléctricos y sistemas fisiológicos para la formulación de modelos matemáticos y el diseño de controladores mediante la experimentación in silico brindan herramientas de gran aplicación en el quehacer profesional del Ingeniero Biomédico.

La asignatura de Modelado de Sistemas Fisiológicos forma parte del plan de estudios de la carrera en Ingeniería Biomédica con la siguiente competencia general del curso: Modeliza sistemas fisiológicos mediante analogías entre sistemas físicos, ecuaciones diferenciales y funciones de transferencia para analizar su comportamiento dinámico y diseña estrategias de control mediante técnicas de control clásico[cite: 1].

## Objetivos

1. Analizar el modelo mecánico simplificado del sistema musculoesquelético compuesto por elementos contráctiles, amortiguadores y resortes[cite: 1].
2. Construir el circuito eléctrico equivalente mediante la analogía fuerza-voltaje considerando las correspondencias entre componentes mecánicos y eléctricos[cite: 1].
3. Formular el modelo matemático de ecuaciones diferenciales ordinarias y calcular la función de transferencia del sistema[cite: 1].
4. Aplicar el análisis matemático inicial mediante el principio de superposición y la transformada de Laplace para obtener las funciones de transferencia del individuo sano (control) y del caso simulado de atrofia muscular (caso)[cite: 1].
5. Implementar la simulación in silico del sistema en MATLAB/Simulink y Simscape Electrical evaluando la respuesta en lazo abierto[cite: 1].
6. Diseñar y sintonizar un controlador PID en Simulink utilizando la herramienta Tune para reducir el error entre la respuesta del caso de atrofia muscular y la referencia del individuo sano[cite: 1].
7. Desarrollar un cuaderno computacional en MATLAB y un script complementario en Python para verificar, graficar y contrastar los resultados de la simulación[cite: 1].
8. Ilustrar mediante un diagrama biológico en BioRender la relación entre las estructuras del sistema musculoesquelético, el modelo mecánico y su equivalente eléctrico[cite: 1].

## Descripción detallada del sistema

En esta práctica se analiza un modelo mecánico simplificado del sistema musculoesquelético basado en la representación viscoelástica descrita por Khoo[cite: 1]. El sistema considera una fuerza activa desarrollada por el elemento contráctil $F_0(t)$, un amortiguador viscoso ($R$) que representa la disipación en el tejido, y elementos elásticos en paralelo ($C_p$) y en serie ($C_s$) que simulan la compliancia del sarcolema y los tendones[cite: 1]. 

Para facilitar su análisis circuital, se utiliza la **analogía fuerza-voltaje**, donde la fuerza se equipara al voltaje, la velocidad a la corriente, el desplazamiento a la carga eléctrica, los amortiguadores a resistencias y las compliancias mecánicas a capacitores[cite: 1]. Se evalúan dos condiciones fisiológicas: un **individuo sano (control)** y un **caso simulado de atrofia muscular**, este último representado mediante un incremento notable en el valor del amortiguamiento $R$ ($10\ \text{k}\Omega$ frente a $100\ \Omega$ del control)[cite: 1]. Utilizando la respuesta del individuo sano como referencia, se diseña un sistema de control con retroalimentación empleando un controlador PID para mitigar los efectos de la atrofia muscular[cite: 1].

Palabras clave: Sistema musculoesquelético; Analogía fuerza-voltaje; Modelo viscoelástico; Atrofia muscular; Controlador PID; Simscape Electrical; BioRender.

## Lista de archivos incluidos en el repositorio

1. Cuaderno computacional de MATLAB `[.mlx y .pdf]`
2. Modelo de Simulink `[.slx]`
3. Archivo de Python `[.py]`
4. Análisis matemático realizado a mano: Modelo de ecuaciones, aplicación del principio de superposición y funciones de transferencia para cada condición `[.pdf]`.
5. Ilustración del sistema fisiológico realizada en BioRender `[.pdf]`.
6. Figuras de las simulaciones y resultados en formato PDF:
   * Respuestas en lazo abierto del individuo sano (control) y el caso de atrofia muscular `[.pdf]`.
   * Respuestas del sistema con el controlador PID sintonizado `[.pdf]`.

## Referencias

[1] P. A. Valle, *Syllabus para Modelado de Sistemas Fisiológicos*, Tecnológico Nacional de México / Instituto Tecnológico de Tijuana, Tijuana, B.C., México, 2026. Permalink: https://biomath.xyz/course/[cite: 1]

[2] M. C. Khoo, *Physiological Control Systems: Analysis, Simulation, and Estimation*, 2nd ed. Piscataway, New Jersey, USA: IEEE Press, 2018.

[3] N. S. Nise, *Control Systems Engineering*, 8th ed. Hoboken, New Jersey, USA: John Wiley & Sons, 2020.