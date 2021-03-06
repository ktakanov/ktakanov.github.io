---
layout: post
title: "Entendendo viés e variância de forma intuitiva e interativa. #mini-article"
date: 2020-08-13 00:00:00 -0400
background: "/img/background.png"
categories: datascience
---

<center><img src="{{ site.url }}{{ site.baseurl }}/img/posts/06/overunder.png" style="max-width: 100%; float:left; margin-right:0em; border-radius:00%"/></center>


# Entendendo viés e variância de forma intuitiva e interativa. #mini-article
Olá, pessoal, nesse mini-article irei comentar dois conceitos super importantes na área de ciência dos dados, viés (bias) e variância (variance). 

A ideia neste artigo, é fazer com que você entenda de forma prática porque modelos (geralmente) não podem ser tão simples e nem tão complexos quando treinados em um dataset.

Então para isto, realizei a ideia de gerar um notebook interativo no Google Colab, da qual eu **fortemente** indico que você dê uma passada lá neste [link](https://colab.research.google.com/drive/1oZMj6SWMPT1OeHenOiMPMPd8kxdzmwkj?usp=sharing) e também está no [github](https://github.com/ktakanov/InteractiveBiasAndVariance). **A**ntes de ir para lá, peço que leia aqui brevemente a definição de viés e variância.

## Explicando viés e variância

Primeiro, você precisa entender que o que queremos é um modelo que represente não só os dados que estamos dando para ele mas também, desejamos que ele tenha um bom desempenho em novos conjuntos de dados. Quando isto ocorre de forma adequada, dizemos que um modelo contém uma boa **generalização**.

Imagine que você pretende criar um pequeno modelo para acertar as questões corretas de um conjunto de dados de texto de uma vestibular qualquer. Nessas questões, podem contar diversos, textos, assim como no ENEM e contém apenas duas alternativas **A**, **B** e **C** . E assim, seu modelo aprende que toda vez que no texto contiver as palavras ‘batata’, ‘cebola’, ‘maçã’ e ‘pera’ a resposta será **A**, toda vez que conter as palavras ‘química’, ‘raio-x’, ‘tabela periódica’ a resposta será **B**, e por fim a todo momento que contiver as palavras ‘império’, ‘D. Pedro II’, ‘república’ a resposta será **C**. 

Desta forma, treinando seu modelo em uma prova, você observa que ele acerta quase todas as respostas e consegue um ótimo desempenho. Porém, como você deve imaginar, este algoritmo não é muito inteligente, ele apenas memorizou as alternativas e infelizmente não funcionou muito bem para outras provas. 

Podemos, dizer que ocorreu aqui é que seu modelo teve um **viés baixo** no dataset de treino. Viés baixo significa que seu modelo se comportou bem no seu dataset isto porque, o viés ou bias é apenas a **diferença média** entre a **previsão do modelo** e o **real valor dos dados**.

Como dito, o modelo não terá bom desempenho em questões novas, gerando o que chamamos de variância alta. A definição de variância aqui, é a mesma da estatística. A variância mede a **propagação da previsão**, ou seja, a **variabilidade da previsão**.

Agora imagine, um modelo que simplesmente chute todas as questões na alternativa **A**. Você percebe então que seu modelo erra boa parte das questões, como esperado, e tem um erro parecido em novas provas. Quando isso acontece, é porque seu modelo não encontrou nenhum (ou pouco) padrões ou tendência nos dados de treino. Chamamos este acontecimento de **underfitting**. 

Em geral **underfitting** contém um **viés alto** já que não se encontrou nenhum (ou insuficientemente) padrão de fato, e uma **variância baixa**, visto que o modelo se comporta de forma parecida em novos dados.

```
Quando overfitting e underfitting ocorrem?

- O overfitting ocorre quando tentamos descrever as regras de aprendizagem com base em muitos parâmetros relativos ao pequeno número de observações. Além disso, o overfitting também ocorre quando tornamos o modelo excessivamente complexo para que se encaixe em todas as amostras de treinamento, como memorizar as respostas para todas as questões, conforme mencionado anteriormente. 

- O underfitting pode ocorrer se não estivermos usando dados suficientes para treinar o modelo. Também pode acontecer se estivermos tentando ajustar um modelo errado aos dados, por exemplo, quando atingimos uma pontuação baixa em quaisquer exercícios ou exames se adotarmos a abordagem errada e aprendermos da maneira errada.
```



### Trade-off

Existe um trade-off pra o viés e variância, uma vez que você diminui um a tendência é que o outro aumente. Para conseguirmos uma boa generalização, é importante que façamos um bom balanceamento entre viés e variância. No próximo, tópico mostra-se como esse trade-off ocorre de forma prática.



## Brincando com viés e variância através da regressão polinomial

Uma boa forma de se entender viés e variância é através de uma regressão polinomial. Iremos tentar mostrar como o viés pode ser aumentado e a variância diminuindo conforme o modelo torna-se mais complexo. O GIF abaixo mostra como o modelo se ajusta conforme o grau do polinômio do regressor aumenta em um conjunto de dados de um modelo senoidal. Observe a mudança do bias e da variância conforme os graus irão mudando.


<center><img src="{{ site.url }}{{ site.baseurl }}/img/posts/06/play.gif" style="max-width: 100%; float:left; margin-right:0em; border-radius:00%"/></center>


Quando o grau do polinômio é 1, perceba que o modelo tenta ajustar uma reta, o que não faz sentido, já que os dados têm uma curvatura. Podemos dizer que agora, o modelo está com underfitting, pois o viés está alto e a variância baixa. Já quando o grau é 20, já temos uma situação oposta, o modelo já está bem ajustado ao treino, com um viés baixo, mas apresenta uma variância muito alta.

Para entender melhor, sugiro que entre no notebook passado no início deste artigo, pois está bem interativo e mostra como os dados foram gerados.

Obrigado, se gostou por favor, compartilhe.

Kevin.

