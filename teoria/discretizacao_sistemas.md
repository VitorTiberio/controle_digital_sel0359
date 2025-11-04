# 📘 Discretização de Sistemas 📘 #
---
O objetivo nesta parte é definir uma função de transferência discreta Cd(z) que seja equivalente a função de transferência C(s) de um sistema contínuo. Para a realização desta operação, temos 4 métodos diferentes que podem ser implementados: 
* Mapeamento casado de polos e zeros;
* Integração Numérica (Forward, Backward e Tustin);
* Controladores PID;

---
# 1 - Mapeamento casado de polos e zeros # 
O método do mapeamento casadi de polos e zeros consiste em mapear diretamente polos e zeros do plano-s para o plano-z. Neste procedimento, considera-se $z = e^{sTs}$ como transformação entre z e s. Para calcularmos o equivalente discreto, segue-se o seguinte procedimento: 
1. Todos os polos e zeros finitos no plano-s são mapeados no plano-z como $z = e^{sTs}$. Por exemplo, se um dos polos da função contínua é s = -a, logo, em Z será $Z = e^(-aTs)$;
2. Os seros em $\text{s} \to \infty$ ou fora da faixa primária são mapeados em z = -1, que representa a maior frequência possível da funçãao de transferência discreta;
3. O ganho de Cd(z) deve ser ajustado em uma frequência crítica. Normalmente, este ponto escolhido é em baixas frequências:

$$
C(s) = Cd(z)
$$

A condição acima ocorre tanto para s = 0 ou para z = 1. O comando em MATLAB para transformar uma função de transferência contínua em uma discreta, com período de amostragem Ts, por meio deste método, é o **c2d**. Segue-se um exemplo de demonstração: 
## Exemplo 1 ##
Encontre o equivalente discreto de: 

$$ 
C(s) = \frac{5}{s+5}
$$

Utilizando o MATLAB, usamos o seguinte código para resolver este exemplo:
```matlab
clear all;
close all;
clc;
num = 5;
den = [1 5];
Ts = 0.1;
C_s = tf(num, den);
pretty(C_s)
C_d = c2d(C_s,Ts,'matched')
```
---
# 2 - Equivalente discreto por Integração #
Para este método, temos 3 possíveis aproximações, como mostra a imagem abaixo: 

<p align="center">
  <img src="img/metodos_discretizacao_linear.PNG" alt="Estabilidade do Sistema no plano-s">
</p>
<p align="center"><em> Métodos de Discretização Linear </em></p>

Da esquerda para a direita, tem-se os métodos de : Forward, Backward e Tustin. Abaixo, apresenta-se um pequeno resumo sobre cada um dos métodos: 

| Méotodo | Aproximação |
| --- | --- |
| Backward | s = $\frac{z-1}{z.Ts}$ |
| Forward | s = $\frac{z-1}{Ts}$ |
| Tustin | s = $\frac{2}{Ts} \frac{(z-1)}{(z+1)}$ |

Cabe aqui algumas observações sobre os métodos: 

> [!CAUTION]
> Antes de deixarmos alguns exemplos, cabe aqui algumas observações importantes. Entre elas, temos:
> * Quanto menor o período de amostragem (Ts), melhor é a aproximação do sistema em tempo contínuio original pelo sistema discreto. Mas, cuidado com a diminuição exagerada de Ts;
> * Para usar o método de euler-forward, deve-se sempre verificar se a frequência de amostragem não gerará polos instáveis;
> * Se a frequência de amostragem for muito próxima da taxa de Nyquist, recomenda-se usar métodos com menor distorção, tais como Tustin ou mapeamento de polos e zeros;
> * O método de Tustin não é adequado para aproximação de derivadas puras.
---
# 3 - Aproximação por segurador de ordem zero (ZOH) # 
A aproximação por ZOH também é conhecida como invariância ao degrau. Para realizar um projeto de controle diretamente no plano-z a partir de uma planta contínua, deve-se, primeiramente, obter o equivalente discreto da planta pelo método ZOH. 

>[!CAUTION]
> Uma atenção especial deve ser destacada quanto a ordem relativa do sistema (a diferença entre o número de zeros e polos). No caso de a ordem relativa ser maior ou igual a dois, se o período de amostragem foi suficientemente pequeno, o sistema em tempo discreto pode apresentar zeros de fase não mínimas, mesmo que o sistema em tempo contínuo seja de fase mínima.

## Exemplo ## 
Suponha que queremos determinar a função de transferência discreta da planta, considerando o efeito do ZOH e um período de amostragem Ts = 0,1s: 

$$
G(s) = \frac{1}{s+4}
$$

Utilizando o MATLAB, obtem-se a seguinte solução: 
```matlab
clear all;
close all;
clc
%% Definindo a função G(S) %%
num = 1; den = [1 4]
G_s = tf(num, den)
Ts = 0.1;
%% Calculando Gd(z) %%
G_z = c2d(G_s, Ts, 'zoh')
```

---
# Exemplos de códigos em MATLAB para cálculo dos métodos de discretização # 
## Exemplo 01 ## 
Suponha a que temos a seguinte função transferência: 

$$ 
G(s) = \frac{(s+3)}{(s+1)(s+2)}
$$

A função de transferência equivalente em tempo discreto pode ser computada usando o MATLAB atraves da função c2d para os metodos degrau-invariante (zoh), impulso-invariante
(impulse), mapeamento casado de polos e zeros (matched), e o metodo Bilinear (tustin), alem de outros. Encontre o equivalente de G(s) para cada um dos métodos. 
### Resolução ###
```matlab
close all %fecha todas janelas
clear all %limpa memoria
clc %limpa command window
%%
num=[1 3];
den=conv([1 1],[1 2]);
G=tf(num,den); %FT em tempo continuo
T=0.1;%tempo de amostragem
Gd1=c2d(G,T,'zoh') %obtem a FT discreta usando metodo degrau−invariante
Gd2=c2d(G,T,'impulse') %obtem a FT discreta usando metodo impulso−invariante
Gd3=c2d(G,T,'matched') %obtem a FT discreta usando método map. casado polos/zeros
Gd4=c2d(G,T,'tustin') %obtem a FT discreta usando metodo bilinear (tustin)
```
Após a discretização da função de transferência, pode-se comparar a resposta em frequência resultante de cada método de discretização usando o comando **bode(contínua, discreta)**
```matlab
figure
bode(G,Gd1)
title('ZOH')
figure
bode(G,Gd2)
title('Impulse')
figure
bode(G,Gd3)
title('Matched')
figure
bode(G,Gd4)
title('tustin')
```
## Exemplo 02 ## 
### Resolução ### 
Faça o mesmo que o exercício anterior, mas agora considerando que a função de transferência seja: 

$$
G(s) = \frac{s^2+s+1}{s^3+2*s^2+3*s+2}
$$

```matlab
clear all;
close all; 
clc 
% Definindo a função de transferência contínua: 
num = [1 1 1];
den = [1 2 3 2];
G = tf(num, den);
% Definindo a função de transferência contínua simbólica (para simples conferência):
syms s
num_sym = poly2sym(num, s);
den_sym = poly2sym(den, s);
G_sym = num_sym / den_sym;
pretty(G_sym)
% Obtendo as funções de transferência discretas:
Ts = 0.1;
Gd_zoh = c2d(G, Ts, 'zoh');
Gd_impulse = c2d(G, Ts, 'impulse')
Gd_matched = c2d(G, Ts, 'matched')
Gd_tustin = c2d(G, Ts, 'tustin')
% Traçando a resposta em frequência de cada função
figure 
bode(G, Gd_zoh)
title('ZOH - Questão 2')
legend('Contínuo', 'Discreto')
figure 
bode(G, Gd_impulse)
title('Impulse - Questão 2')
legend('Contínuo', 'Discreto')
figure 
bode(G, Gd_matched)
title('Matched - Questão 2')
legend('Contínuo', 'Discreto')
figure
bode(G, Gd_tustin)
title('Tustin - Questão 2')
legend('Contínuo', 'Discreto')
```
> [!IMPORTANT]
>  Em relação à resposta em frequência para os diferentes métodos de discretização que foram utilizados, nota-se que o método de tustin é o único método que tem um "casamento" de fase e magnitude em relação ao sinal contínuo. Todos os outros métodos possuem variações (ou de fase, ou de magnitude ou ambos) do sinal discreto em relação ao contínuo.
> Em relação a estas mudanças, nota-se:
> * **zoh** insere uma atenuação para altas frequências;
> * O **impulse invariant** gera aliasing;
> * O **matched** não preserva exatamente o ganho e a fase (garante estabilidade (pólos mapeados corretamente), mas pode distorcer a resposta em frequência).

## Exemplo 03 ## 
Compare os métodos de discretização invariante ao degrau (zoh) e impulso. Para isso, considere a seguinte função de transferência: 

$$
\frac{(s+2)(s+1)}{(s+5)(s+3)(s+7)}
$$

O que pode-se observar em relação às respostas temporais entre os dois métodos de discretização ? 

### Resolução ### 
Primeiramente, vamos desenvolver o código em MATLAB: 
```matlab
clear all
close all 
clc
% Definindo a função de transferência contínua % 
num = [1 3 2];
den = [1 7 8 56 105];
G = tf(num, den);
% Resposta ao impulso e ao degrau:
dt = 0.001;
t = 0:dt:5; %vetor tempo contínuo
[y_degrau, t] = step(G, t);
[y_impulso, t] = impulse(G, t);
Ts = 0.1;
% Obtendo as funções de transferência discretas:
Gd_zoh = c2d(G, Ts, 'zoh');
Gd_impulse = c2d(G, Ts, 'impulse');
td = 0:Ts:5; %vetor tempo discreto
[y_du1,td] = step(Gd_zoh, td);
[y_du2,td] = step(Gd_impulse, td);
[y_di1,td] = impulse(Gd_zoh, td);
[y_di2,td] = impulse(Gd_impulse, td);
figure
plot(t, y_degrau, 'LineWidth',1.5)
hold on
stairs(td, y_du1, 'LineWidth', 1.5)
stairs(td, y_du2, 'LineWidth', 1.5)
xlabel('t(s)')
ylabel('y(t)')
title('Atividade 03 - Resposta ao Degrau')
legend('Sinal Contínuo', 'Invariante ao Degrau', 'Invariante ao Impulso')
figure
plot(t, y_impulso, 'LineWidth',1.5)
hold on 
stairs(td, y_di1, 'LineWidth',1.5)
stairs(td, y_di2, 'LineWidth',1.5)
title('Atividade 03 - Resposta ao Impulso')
xlabel('t(s)')
ylabel('y(t)')
legend('Sinal Contínuo', 'Invariante ao Degrau', 'Invariante ao Impulso')
```
Nota-se que, na resposta ao degrau, o ZOH, "respeita" o sinal contínuo, enquanto o sinal discreto que é dado pela resposta ao impulso não. O mesmo ocorre para o outro sinal, onde a discretização invariante ao impulso "casa" com o sinal contínuo, enquanto o sinal discreto invariante ao degrau não. Isso mostra que os métodos de ZOH e Impulse representam bem o sinal contínuo e que dependendo da entrada, um dos métodos apresenta uma melhor performace do que outro. 
