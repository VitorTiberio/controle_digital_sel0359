# 📘Modelo de espaço de estados em tempo discreto📘 #
> Neste resumo, encontra-se a análise e controle de sistemas SLIT em tempo discreto, considerando a representação em espaço de estados. 
---
# Representação em Espaço de Estados # 
Pode-se ter a seguinte representação em espaço de estados: 

$$
\dot x(t)= f(x(t), u(t)
$$ 

$$
y(t) = g(x(t), u(t)
$$ 

Em que a primeira equação é denominada equação de estado e a segunda equação de saída. x(t), u(t) e y(t) são, respectivamente, vetores de estado, entrada e saída. Se o conjunto de equações em questão for linearizado em torno de algum ponto de equilíbrio
tem-se a seguinte representação em espaço de estados: 

$$ 
\dot x(t) = Ax(t) + Bu(t)
$$

$$
y(t) = Cx(t) + Du(t)
$$

* A = Matriz de estado;
* B = Matriz de entrada;
* C = Matriz de saída;
* D = Matriz de transição direta.

## Linearização de um sistema ## 
Este processo é a linearização em torno de um ponto de equilíbrio $(x_0, y_0)$. Para cada equação dinâmica do sistema, pode-se efetuar uma expansão em série de Taylor em torno do ponto de equilíbrio. Este processo de linearização resultará no mesmo modelo em espaço de estados. 

### Exemplo ### 
Linearize o modelo do pêndulo invertido da figura abaixo, cujo conjuto de equações é dado por: 

$$
(M + m)\ddot{x} - mlcos(\theta)\ddot{\theta} + ml(\dot{\theta})^2sin(\theta) = F
$$

$$
l\ddot{\theta} - \ddot{x}cos(\theta) - gsin(\theta) = 0
$$
