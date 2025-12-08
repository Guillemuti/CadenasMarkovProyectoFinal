# Coeficientes de Mezcla y el Estimador de McDonald

## Introducción

Los **coeficientes de mezcla** son herramientas fundamentales para cuantificar la dependencia temporal en procesos estocásticos. Permiten medir qué tan rápido "olvida" un proceso su pasado, lo cual tiene implicaciones profundas para la teoría asintótica y las aplicaciones prácticas.

En este guión desarrollamos la teoría de los coeficientes de mezcla, con énfasis en el coeficiente $\beta$ (regularidad absoluta), y presentamos el estimador empírico propuesto por McDonald (2015).

---

## 1. Motivación: ¿Por qué Medir la Dependencia Temporal?

### 1.1 El Problema

En el análisis de series temporales, frecuentemente asumimos que las observaciones son "aproximadamente independientes" para aplicar resultados clásicos como:

- **Ley de los Grandes Números (LLN)**
- **Teorema del Límite Central (CLT)**
- **Intervalos de confianza y tests de hipótesis**

Sin embargo, en la práctica las observaciones consecutivas suelen estar correlacionadas. La pregunta fundamental es:

> ¿Qué tan fuerte es la dependencia entre observaciones separadas por $n$ unidades de tiempo?

---

## 2. Definición de Coeficientes de Mezcla

### 2.1 Notación Preliminar

Sea $\{X_t\}_{t \in \mathbb{Z}}$ un proceso estocástico estrictamente estacionario. Definimos las $\sigma$-álgebras:

- $\mathcal{F}_a^b = \sigma(X_t : a \leq t \leq b)$ — información del proceso entre tiempos $a$ y $b$
- $\mathcal{F}_{-\infty}^0 = \sigma(X_t : t \leq 0)$ — el "pasado"
- $\mathcal{F}_n^{\infty} = \sigma(X_t : t \geq n)$ — el "futuro" a partir de $n$

### 2.2 Distancia de Variación Total

Antes de presentar la forma equivalente del coeficiente $\beta$, definimos la **distancia de variación total** entre dos medidas de probabilidad.

**Definición:** Sean $\mu_1$ y $\mu_2$ dos medidas de probabilidad sobre un espacio medible $(\Omega, \mathcal{F})$, con densidades $f_{\mu_1}$ y $f_{\mu_2}$ respecto a una medida dominante común $\lambda$. La **distancia de variación total** se define como:

$$TV(\mu_1, \mu_2) = \frac{1}{2} \int_\Omega \left| f_{\mu_1}(x) - f_{\mu_2}(x) \right| d\lambda(x)$$

**Propiedades:**
- $0 \leq TV(\mu_1, \mu_2) \leq 1$
- $TV(\mu_1, \mu_2) = 0$ si y solo si $\mu_1 = \mu_2$
- Forma equivalente (supremo): $TV(\mu_1, \mu_2) = \sup_{A \in \mathcal{F}} |\mu_1(A) - \mu_2(A)|$

### 2.3 Coeficiente $\beta$ (Regularidad Absoluta)

El **coeficiente de mezcla $\beta$** se define usando la distancia de variación total como:

$$\beta(n) = TV\left(\mathcal{L}(X_0, X_n), \mathcal{L}(X_0) \otimes \mathcal{L}(X_n)\right)$$

Es decir, $\beta(n)$ mide la distancia de variación total entre:
- La distribución conjunta $\mathcal{L}(X_0, X_n)$
- El producto de las distribuciones marginales $\mathcal{L}(X_0) \otimes \mathcal{L}(X_n)$

En forma integral, cuando las densidades existen:

$$\beta(n) = \frac{1}{2} \int \int \left| f_{X_0, X_n}(x, y) - f_{X_0}(x) f_{X_n}(y) \right| dx \, dy$$

donde $f_{X_0, X_n}$ es la densidad conjunta y $f_{X_0}$, $f_{X_n}$ son las densidades marginales.

---

## 3. Propiedades Fundamentales del Coeficiente $\beta$

### 3.1 Propiedades Básicas

**Acotamiento:**
$$0 \leq \beta(n) \leq 1 \quad \text{para todo } n$$

- $\beta(n) = 0$ implica que $X_0$ y $X_n$ son independientes
- $\beta(n) = 1$ implica dependencia máxima

---

## 4. Procesos $\beta$-Mezclados

### 4.1 Definición

Un proceso estacionario $\{X_t\}$ es **$\beta$-mezclado** (o absolutamente regular) si:

$$\beta(n) \to 0 \quad \text{cuando} \quad n \to \infty$$

### 4.2 Interpretación

La condición $\beta(n) \to 0$ significa que:

1. **Pasado y futuro se vuelven asintóticamente independientes**
2. El proceso tiene **memoria corta**
3. La información sobre $X_0$ se "diluye" conforme observamos tiempos más lejanos

### 4.3 Tasas de Decaimiento

La velocidad con que $\beta(n) \to 0$ es crucial:

| Tasa | Expresión | Tipo de Proceso |
|------|-----------|-----------------|
| Exponencial | $\beta(n) \leq C\rho^n$, $\rho < 1$ | Cadenas geométricamente ergódicas |
| Polinomial | $\beta(n) \leq C n^{-\gamma}$, $\gamma > 0$ | Algunos procesos ARMA |
| Logarítmica | $\beta(n) \leq C (\log n)^{-\gamma}$ | Dependencia más fuerte |

---

## 5. Estimador de McDonald (2015)

### 5.1 Idea Principal

McDonald propone estimar $\beta(n)$ usando **estimadores por histogramas** de las densidades involucradas.

En lugar de trabajar con $\beta(n)$ directamente (que involucra variables infinito-dimensionales), se trabaja con una versión de **dimensión finita** $\beta^d(a)$ basada en bloques de tamaño $d$.

### 5.2 Definición del Estimador

Para bloques de tamaño $d$ separados por lag $a$:

$$\hat{\beta}^d(a) = \frac{1}{2} \int_{\Omega^{2d}} \left| \hat{f}^{2d}_a(x, y) - \hat{f}^d(x) \hat{f}^d(y) \right| dx \, dy$$

donde:

- $\hat{f}^d$ es el estimador por histograma de la densidad de bloques de tamaño $d$
- $\hat{f}^{2d}_a$ es el estimador de la densidad conjunta de dos bloques separados por lag $a$

### 5.3 Estimadores por Histogramas

El estimador de la densidad marginal de bloques es:

$$\hat{f}^d(x) = \frac{1}{n - d + 1} \sum_{i=1}^{n-d+1} \frac{\mathbf{1}_{[X_{i:i+d-1} \in B(x)]}}{h_n^d}$$

donde:
- $X_{i:i+d-1} = (X_i, X_{i+1}, \ldots, X_{i+d-1})$ es el bloque de $d$ observaciones consecutivas
- $B(x)$ es el bin del histograma que contiene a $x$
- $h_n$ es el **ancho de bins**

### 5.4 Parámetros Adaptativos

El estimador tiene dos parámetros críticos que deben elegirse cuidadosamente:

**Dimensión de bloques $d_n$:**
$$d_n = \max\left\{1, \left\lfloor \log_2\left(\frac{n}{20}\right) \right\rfloor \right\}$$

**Ancho de bins $h_n$:**
$$h_n = n^{-1/(2d_n + 1)}$$

Estas fórmulas garantizan que los parámetros escalen correctamente con el tamaño de muestra $n$.

---

## 6. Condiciones de Convergencia

### 6.1 Teorema Principal (McDonald, 2015)

**Teorema:** Bajo condiciones de regularidad, el estimador $\hat{\beta}^{d_n}(a)$ es consistente:

$$\hat{\beta}^{d_n}(a) \xrightarrow{P} \beta(a) \quad \text{cuando} \quad n \to \infty$$

siempre que se cumplan las siguientes condiciones:

### 6.2 Las Cuatro Condiciones

| # | Condición | Interpretación |
|---|-----------|----------------|
| 1 | $n h_n^{d_n} \to \infty$ | Suficientes observaciones por bin |
| 2 | $d_n h_n \to 0$ | Los bins se hacen más pequeños |
| 3 | $d_n \to \infty$ | La dimensión de bloques crece |
| 4 | $h_n \to 0$ | El ancho de bins tiende a cero |

**Condición 1** asegura que cada bin del histograma contenga suficientes observaciones para una buena estimación de la densidad.

**Condiciones 2-4** aseguran que el estimador capture cada vez más información sobre la distribución verdadera.

---

## Referencias

- Bradley, R. C. (2005). Basic properties of strong mixing conditions. *Probability Surveys*, 2, 107-144.
- McDonald, D. J. (2015). Histogram estimation from dependent data. *arXiv preprint arXiv:1508.07322*.
- Doukhan, P. (1994). *Mixing: Properties and Examples*. Springer.
- Rio, E. (2000). *Théorie asymptotique des processus aléatoires faiblement dépendants*. Springer.
