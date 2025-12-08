# Teoría de Cadenas de Markov con Espacio de Estados Continuo

## Introducción

Las cadenas de Markov constituyen una de las herramientas más poderosas en la teoría de procesos estocásticos. En este guión desarrollaremos la teoría para el caso de **espacios de estados continuos**, que es fundamental para aplicaciones en estadística bayesiana, finanzas y métodos MCMC.

---

## 1. Espacios de Probabilidad y Filtraciones

### 1.1 Espacio de Probabilidad Base

Trabajamos sobre un espacio de probabilidad $(\Omega, \mathcal{F}, \mathbb{P})$ donde:

- $\Omega$ es el espacio muestral (conjunto de todos los resultados posibles)
- $\mathcal{F}$ es una $\sigma$-álgebra sobre $\Omega$ (colección de eventos medibles)
- $\mathbb{P}$ es una medida de probabilidad sobre $(\Omega, \mathcal{F})$

### 1.2 Filtraciones

Una **filtración** es una familia de $\sigma$-álgebras $\{\mathcal{F}_t\}_{t \geq 0}$ tal que:

$$\mathcal{F}_s \subseteq \mathcal{F}_t \subseteq \mathcal{F} \quad \text{para todo } s \leq t$$

Intuitivamente, $\mathcal{F}_t$ representa la **información disponible hasta el tiempo $t$**. La condición de inclusión garantiza que la información nunca se "olvida": lo que sabemos en el tiempo $s$ sigue siendo conocido en el tiempo $t > s$.

---

## 2. Definición de Cadena de Markov

### 2.1 Espacio de Estados

Sea $(E, \mathcal{E})$ un **espacio medible**, donde:
- $E$ es el conjunto de estados posibles (puede ser $\mathbb{R}$, $\mathbb{R}^d$, un espacio polaco, etc.)
- $\mathcal{E}$ es una $\sigma$-álgebra sobre $E$ (típicamente la $\sigma$-álgebra de Borel $\mathcal{B}(E)$)

### 2.2 Propiedad de Markov

Un proceso $\{X_t\}_{t \in \mathbb{N}}$ con valores en $(E, \mathcal{E})$ es una **cadena de Markov** si satisface la **propiedad de Markov**:

$$\mathbb{P}(X_{t+1} \in A \mid \mathcal{F}_t) = \mathbb{P}(X_{t+1} \in A \mid X_t) \quad \text{para todo } A \in \mathcal{E}$$

En palabras: **el futuro es condicionalmente independiente del pasado dado el presente**. Toda la información relevante para predecir el siguiente estado está contenida únicamente en el estado actual.

---

## 3. Kernels de Transición

### 3.1 Definición

Un **kernel de transición** (o núcleo de Markov) es una función $P: E \times \mathcal{E} \to [0, 1]$ tal que:

1. Para cada $x \in E$, la función $A \mapsto P(x, A)$ es una medida de probabilidad sobre $(E, \mathcal{E})$
2. Para cada $A \in \mathcal{E}$, la función $x \mapsto P(x, A)$ es $\mathcal{E}$-medible

La interpretación es directa: $P(x, A)$ es la **probabilidad de transitar al conjunto $A$ dado que el estado actual es $x$**.

### 3.2 Kernel de $n$ Pasos

El kernel de $n$ pasos $P^n$ se define recursivamente:

$$P^n(x, A) = \int_E P^{n-1}(y, A) P(x, dy)$$

con $P^1 = P$ y $P^0(x, A) = \delta_x(A)$ (delta de Dirac).

---

## 4. Medidas Invariantes y Estacionariedad

### 4.1 Medida Invariante

Una medida $\pi$ sobre $(E, \mathcal{E})$ es **invariante** (o estacionaria) para el kernel $P$ si:

$$\pi(A) = \int_E P(x, A) \pi(dx) \quad \text{para todo } A \in \mathcal{E}$$

En notación de operadores: $\pi P = \pi$.

Esto significa que si $X_t \sim \pi$, entonces $X_{t+1} \sim \pi$. La distribución se preserva bajo la dinámica de la cadena.

### 4.2 Distribución Estacionaria

Si $\pi$ es una medida de probabilidad invariante, la llamamos **distribución estacionaria**. En este caso, si inicializamos $X_0 \sim \pi$, entonces $X_t \sim \pi$ para todo $t$.

---

## Referencias

- Douc, R., Moulines, E., Priouret, P., & Soulier, P. (2018). *Markov Chains*. Springer.
- Meyn, S. P., & Tweedie, R. L. (2009). *Markov Chains and Stochastic Stability*. Cambridge University Press.
- Roberts, G. O., & Rosenthal, J. S. (2004). General state space Markov chains and MCMC algorithms. *Probability Surveys*, 1, 20-71.
- Tierney, L. (1994). Markov chains for exploring posterior distributions. *The Annals of Statistics*, 22(4), 1701-1728.
