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

Para resolver este problema, pode-se implementar o seguinte código em MATLAB: 
```matlab
clear all
close all
clc
%% Definindo as variáveis do sistema %%
syms m M l F g
syms th th_d th_dd
syms x x_d x_dd
%% Definindo as equações de movimento %%
Eq_1 = (M+m)*x_dd - m*l*th_dd*cos(th) + m*l*th_d^2*sin(th) - F;
Eq_2 = l*th_dd - x_dd*cos(th)-g*sin(th);
u = F;
S = solve(Eq_1 == 0, Eq_2 == 0, x_dd, th_dd);
%% Vetor de estados e sua derivada %%
x_vet = [x; x_d; th; th_d];
x_vet_dot = [x_d; S.x_dd; th_d; S.th_dd];
%% Linearizando o sistema %%
Am = simplify(jacobian(x_vet_dot, x_vet));
Bm = simplify(jacobian(x_vet_dot, u));
%% Ponto de Equilíbrio %%
x = 0; x_d = 0; th = 0; th_d = 0; u = 0;
A = simplify(subs(Am));
B = simplify(subs(Bm));
```

## Discretização de equação de estados contínua ## 
No MATLAB, o comando **c2d** pode ser utilizado para encontrar a representação discreta com o segurador de ordem zero, da seguinte forma: [Phi, Gamma] = c2d(A, B, Ts).
### Exemplo ###
Suponha que queremos discretizar o seguinte modelo de espaço de estados: 

Obtenha a discretização do sistema no MATLAB. 
```matlab
1 close all
2 clear all
3 clc
%% Definindo as matrizes A e B: 
A=[1 1;...
0 2];
B=[1;...
    3];
10 Ts=0.1;
%% Discretizando o sistema %% 
[Ad2,Bd2]=c2d(A,B,Ts)
```
# Estabilidade Assintótica de Modelos de Espaço de Estados #
Para explicarmos a estabilidade por meio de um exemplo, considere o seguinte modelo: 

Um modelo em espaço de estados em tempo discreto é assintoticamente estável se os autovalores da matriz dinâmica Ad estiverem dentro de um circulo unitário. Outra forma de avaliar é através da equação de Lyapunov. No MATLAB, pode-se implementar o seguinte código:
```matlab
close all
clear all
clc
%% Degfinindo as matrizes A e B: 
Ad=[1 −2;...
2 1];
Bd=[1;...
2];
%% Avaliando se os autovalores da matriz dinâmica estão dentro de um circulo unitário %% 

eig(Ad) %obtem autovalores de Ad
if any(abs(eig(Ad))≥1) %verifica se existe algum autovalor maior que 1
    disp('Sistema nao e assintoticamente estavel!')
else
    disp('Sistema e assintoticamente estavel!')
end

%% Verificando pela equação de Lyapunov %%

Q=eye(size(Ad))
P=dlyap(Ad,Q) %obtem solucao da eq. de lyapunov
if any(eig(P)≤0) %verifica se P>0
    disp('Sistema nao e assintoticamente estavel!')
else
    disp('Sistema e assintoticamente estavel!')
end
```
