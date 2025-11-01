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
# Equivalente discreto por Integração #
Para este método, temos 3 possíveis aproximações, como mostra a imagem abaixo: 

![Métodos de Discretização Linear](https://github.com/VitorTiberio/controle_digital_sel0359/blob/main/img/metodos_discretizacao_linear.PNG)

Da esquerda para a direita, tem-se os métodos de : Backward, Forward e Tustin. Abaixo, apresenta-se um pequeno resumo sobre cada um dos métodos: 

| Méotodo | Aproximação |
| --- | --- |
| Forward | s = $\frac{z-1}{z.Ts}$ |
| Backward | s = $\frac{z-1}{Ts}$ |
| Tustin | s = $\frac{2}{Ts} \frac{(z-1)}{(z+1)}$ |

Antes de partimos para os exercícios, cabe aqui algumas observações sobre os métodos: 

> [!CAUTION]
> Antes de deixarmos alguns exemplos, cabe aqui algumas observações importantes. Entre elas, temos:
> * Quanto menor o período de amostragem (Ts), melhor é a aproximação do sistema em tempo contínuio original pelo sistema discreto. Mas, cuidado com a diminuição exagerada de Ts;
> * Para usar o método de euler-forward, deve-se sempre verificar se a frequência de amostragem não gerará polos instáveis;
> * Se a frequência de amostragem for muito próxima da taxa de Nyquist, recomenda-se usar métodos com menor distorção, tais como Tustin ou mapeamento de polos e zeros;
> * O método de Tustin não é adequado para aproximação de derivadas puras.
