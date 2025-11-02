# 📘 Análise de Sistemas de Controle em Tempo Discreto 📘 #
> Neste resumo, encontra-se a análise de sistemas de controle digitais no plano-z. Tal análise servirá de suporte para o projeto de controladores digitais. 
---
# 1 - Revisão: do plano-s para o plano-z # 

Nesta parte do conteúdo, é esperado que você já saiba que: 
* O semiplano esquedo do plano-s é mapeado no interior do círculo unitário no plano-z;
* Linhas verticais no plano-s com $\sigma$ = $\xi$ $\omega_{n}$ constante, são mapeadas em circunferências concêntricas à origem no plano-Z.
* Linhas horizontais no plano-s com $\omega_d$ constante, são mapeadas em linhas radiais no plano-z;
* Linhas radiais no plano-s com $\xi$ constante são mapeadas em espirais no plano-z.

---

# 2 - Análise da Estabilidade # 

Para a análise da estabilidade de um sistema, primeiro, vamos exemplificar o que foi constuído no tópico 1 deste resumo, com a imagem abaixo: 

<p align="center">
  <img src="img/estabilidade_malha_fechada.PNG" alt="Estabilidade do Sistema no plano-s">
</p>
<p align="center"><em> Mapeamento de S em Z </em></p>

Logo, suponha que temos um sistema em malha fechada. Sabemos que a Função de transferência em malha fechada é dada por: 

$$
Gf(s) = \frac{G(s)}{1+G(s)}
$$

A estabilidade deste sistema é determinada pela localização dos polos de malha fechada, obtidos pela solução da seguinte equação característica: 

$$
1 + G(s) = 0
$$

O sistema é absolutamente estável se os polos possuírem a parte real negativa, pertencendo ao semiplano esquerdo do plano-s, exemplificado na imagem abaixo. Isso é como analisamos quando estamos no domínio-s. 

<p align="center">
  <img src="img/estabilidade_malha_fechada.PNG" alt="Estabilidade do Sistema no plano-s">
</p>
<p align="center"><em> Estabilidade do Sistema no plano-s </em></p>

