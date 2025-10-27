# 📘 Transformada Z 📘 #

# Defininição e Exemplos 

A transformada Z é como se ela fizesse o papel da transformada de Laplace (𝓛) em tempo discreto.

Considere que o sinal em tempo discreto x[n] = x(nTs) foi obtido por amostragem periódica.  
A transformada-Z bilateral de uma sequência x[n] é definida como:

$$
Z\{x[n]\} = X(z) = \sum_{n=-\infty}^{\infty} x[n] z^{-n}
$$

Para sinais causais (tempo ≥ 0), tem-se a transformada-Z unilateral:

$$
Z\{x[n]\} = X(z) = \sum_{n=0}^{\infty} x[n] z^{-n}
$$

---

## Exemplo 1

Determine a transformada-Z da sequência obtida pela amostragem periódica de:

$$
u_d(t) = 
\begin{cases}
1, & t \ge 0 \\
0, & t < 0
\end{cases}
$$

No caso, temos:

$$
Z\{x[n]\} = \sum_{n=0}^{\infty} x[n] z^{-n}
$$

Neste caso, x[n] = 1, para \( n = 0, 1, 2, 3, ...)

Logo, escrevemos:

$$
Z\{x[n]\} = \sum_{n=0}^{\infty} 1 \cdot z^{-n}
$$

Para que esse somatório exista, é necessário que \( |z| > 1 \).  
Essa condição define uma região no plano-z chamada **ROC (Region of Convergence)**.

Portanto:

$$
U_d(z) = \frac{1}{1 - z^{-1}} = \frac{z}{z - 1}, \quad |z| > 1
$$

---

## Exemplo 2

Determine a transformada-Z da sequência:

$$
x[n] =
\begin{cases}
a^n, & n \ge 0 \\
0, & n < 0
\end{cases}
$$

Pela definição da transformada-Z, temos:

$$
X(z) = \sum_{n=0}^{\infty} a^n z^{-n}
$$

Logo, temos uma série geométrica de razão \( a z^{-1} \).  
Sabemos que:

$$
\sum_{n=0}^{\infty} r^n = \frac{1}{1 - r}
$$

Portanto:

$$
X(z) = \frac{1}{1 - a z^{-1}} = \frac{z}{z - a}
$$

---

## Exemplo 3

Determine a transformada-Z da sequência obtida pela amostragem periódica de:

$$
x(t) =
\begin{cases}
e^{-a t}, & t \ge 0 \\
0, & t < 0
\end{cases}
$$

Pela definição da transformada-Z, temos:

$$
X_d(z) = \sum_{n=0}^{\infty} e^{-a n T_a} z^{-n} = \sum_{n=0}^{\infty} (e^{-a T_a} z^{-1})^n
$$

Novamente, temos uma série geométrica cuja razão é \( e^{-a T_a} z^{-1} \).  
Logo:

$$
X_d(z) = \frac{1}{1 - e^{-a T_a} z^{-1}} = \frac{z}{z - e^{-a T_a}}
$$

---

## Exemplo 4

Obtenha a transformada discreta-Z de:

$$
X(s) = \frac{2}{s(s + 2)}
$$

Escrevendo \( X(s) \) em frações parciais, temos:

$$
X(s) = \frac{1}{s} - \frac{1}{s + 2}
$$

Logo, aplicando a transformada inversa de Laplace:

$$
x(t) = 1 - e^{-2t}, \quad t \ge 0
$$

Amostrando \( x(t) \) com período igual a \( T_a \), temos:

$$
x[n] = 1 - e^{-2nT_a}
$$

Aplicando a transformada-Z, temos:

$$
X_d(z) = Z\{1 - e^{-2nT_a}\} = \frac{z}{z - 1} - \frac{z}{z - e^{-2T_a}}
$$

--- 
# Como calcular a Transformada-Z usando o Matlab ? 

No MATLAB, o comando **ztrans()** de matemática simbólica pode ser utilizado para obter a transformada-z. Um exemplo de aplicação pode ser encontrado abaixo: 

```matlab
clear all;
close all;
clc
%% Definindo as variáveis simbólicas %%
syms n T_s
%% Definindo x[n] %%
x_n = n*T_s
%% Calculando a transformada-Z %%
X_z = ztrans(x_n)
pretty (X_z)
```



