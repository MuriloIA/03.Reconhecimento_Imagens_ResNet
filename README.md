# <center>Classificação de CIFAR-10 com ResNet:<br> Uma Implementação em PyTorch 2.0, Lightning e Torchvision</center>

## 📚 1. Introdução

<p style="font-family: 'serif'; font-size: 16px; text-align: justify;">Caro explorador do universo da Inteligência Artificial, é com grande entusiasmo que lhe convido a embarcar nesta jornada pelo meu projeto de Reconhecimento de Imagens utilizando a arquitetura ResNet. Este trabalho é mais do que um simples projeto, é um marco em minha odisseia profissional e estou ansioso para compartilhar cada detalhe desta aventura com você.</p>

<p style="font-family: 'serif'; font-size: 16px; text-align: justify;">Nesta expedição, navegaremos pelas águas das bibliotecas Python Pytorch 2.0, Lightning e Torchvision, ferramentas poderosas que nos auxiliarão a construir um modelo de aprendizado profundo para reconhecimento de imagens. Nosso navio é a arquitetura ResNet, uma verdadeira obra-prima da engenharia de aprendizado de máquina, conhecida por sua robustez e eficácia em tarefas de visão computacional.</p>

<p style="font-family: 'serif'; font-size: 16px; text-align: justify;">Nosso mapa do tesouro é o conjunto de dados CIFAR10. Este conjunto é como uma bússola para a comunidade de aprendizado de máquina, composto por 60.000 imagens coloridas de 32x32 distribuídas em 10 classes distintas, cada uma contendo um total de 6.000 imagens. É neste vasto oceano de dados que treinaremos e avaliaremos nosso modelo.</p>

## 📦 2. Instalação & Carga de Pacotes 

<div style="background-color: #f0f0f0; padding: 10px; border-radius: 10px; margin: 10px;">
<ol>
    
<li><p style="font-family: 'serif'; font-size: 16px; text-align: justify;"><strong>os</strong>: Módulo Python para interação com o sistema operacional.</p></li>
    
<li><p style="font-family: 'serif'; font-size: 16px; text-align: justify;"><strong>math</strong>: Módulo Python para tarefas matemáticas.</p></li>

<li><p style="font-family: 'serif'; font-size: 16px; text-align: justify;"><strong>torch</strong>: Biblioteca de aprendizado de máquina, usada para aplicações como processamento de linguagem natural.</p></li>
    
<li><p style="font-family: 'serif'; font-size: 16px; text-align: justify;"><strong>pickle</strong>: Módulo Python para serialização e des-serialização de estruturas de objetos Python.</p></li>
    
<li><p style="font-family: 'serif'; font-size: 16px; text-align: justify;"><strong>numpy</strong>: Biblioteca Python para suporte a grandes arrays e matrizes multidimensionais.</p></li>

<li><p style="font-family: 'serif'; font-size: 16px; text-align: justify;"><strong>pandas</strong>: Biblioteca de software para manipulação e análise de dados.</p></li>

<li><p style="font-family: 'serif'; font-size: 16px; text-align: justify;"><strong>seaborn</strong>: Biblioteca de visualização de dados Python baseada em matplotlib.</p></li>

<li><p style="font-family: 'serif'; font-size: 16px; text-align: justify;"><strong>matplotlib</strong>: Biblioteca de plotagem para Python.</p></li>

<li><p style="font-family: 'serif'; font-size: 16px; text-align: justify;"><strong>lightning</strong>: Wrapper leve do PyTorch para pesquisa de IA de alto desempenho.</p></li>

<li><p style="font-family: 'serif'; font-size: 16px; text-align: justify;"><strong>torchvision</strong>: Pacote do PyTorch que consiste em conjuntos de dados populares, arquiteturas de modelos e transformações comuns de imagens para visão computacional.</p></li>
    
<li><p style="font-family: 'serif'; font-size: 16px; text-align: justify;"><strong>torchmetrics</strong>: Biblioteca PyTorch de métricas para modelos de aprendizado de máquina e aprendizado profundo.</p></li>
    
<li><p style="font-family: 'serif'; font-size: 16px; text-align: justify;"><strong>pkg_resources</strong>: O módulo pkg_resources distribuído com setuptools fornece uma API para bibliotecas Python acessarem seus arquivos de recursos, e para aplicações e frameworks extensíveis descobrirem automaticamente plugins.</p></li>
    
</ol>
</div>
    
## 3. 💻 Configuração de Ambiente

### 3.1 Reprodutibilidade dos Experimentos

A função `set_seed` é usada para definir a semente para geradores de números aleatórios no NumPy e PyTorch. Isso é útil para garantir que os experimentos sejam reproduzíveis, ou seja, que os mesmos resultados sejam obtidos sempre que o código for executado com a mesma semente.

Aqui estão as funcionalidades de cada parte do código:

- `torch.manual_seed(seed)`: Define a semente para o gerador de números aleatórios do PyTorch para a CPU.

- `os.environ['PYTHONHASHSEED'] = str(seed)`: Define a semente para as funções hash do Python

- `if torch.cuda.is_available()`: Verifica se uma GPU está disponível.

- `torch.cuda.manual_seed_all(seed)`: Define a semente para todas as GPUs disponíveis.

- `torch.backends.cudnn.deterministic = True`: Garante que o backend cuDNN use apenas algoritmos determinísticos.

- `torch.backends.cudnn.benchmark = False`: Desativa o uso de um algoritmo de convolução heurístico.

## 4. 🖼️ Carregamento e Pré-processamento de Imagens

- **Média e Desvio Padrão:** O objetivo é criar uma classe Python que calcule a média e o desvio padrão das imagens originais. Esses valores são importantes porque serão usados posteriormente para padronizar as imagens.

- **Criação dos Transformadores:** Após calcular a média e o desvio padrão, o próximo passo é criar os transformadores. Estamos utilizando o módulo `torchvision.transforms` para isso. Os transformadores são usados para aplicar transformações nas imagens, como redimensionamento, recorte, normalização, etc. Neste caso, estamos utilizando a média e o desvio padrão obtidos na etapa anterior para padronizar as imagens. A padronização é uma técnica comum de pré-processamento de dados que ajuda a acelerar o treinamento e a convergência dos modelos de aprendizado de máquina.
    
- `Normalização:` A normalização é realizada usando a seguinte fórmula:

$$x^{'} = \frac{x - \bar{x}}{\sigma}$$

- `Criação dos DataLoaders:` Finalmente, criamos os DataLoaders com o conjunto de dados CIFAR-10. Os DataLoaders são usados para carregar os dados em lotes durante o treinamento do modelo. Eles também podem embaralhar os dados e aplicar transformações. Neste caso, estamos utilizando o DataLoader para carregar as imagens do CIFAR-10 que foram padronizadas na etapa anterior.

## 5. 🧬 Visão Geral da Arquitetura ResNet


### 5.1 Introdução à ResNet

A ResNet, ou Rede Residual, é uma arquitetura de rede neural convolucional que se tornou uma referência no campo da visão computacional. Proposta pelos pesquisadores Kaiming He, Xiangyu Zhang, Shaoqing Ren e Jian Sun em 2015, a ResNet introduziu o conceito de "conexões residuais", que permitem o treinamento eficaz de redes muito mais profundas do que era possível anteriormente.

As redes neurais convolucionais tradicionais tentam aprender representações de nível superior à medida que aprofundam a rede, com cada camada tentando aprender algo novo. No entanto, à medida que essas redes se tornam mais profundas, elas começam a sofrer de um problema conhecido como "desaparecimento do gradiente", onde as camadas mais profundas da rede são incapazes de aprender efetivamente.

A ResNet aborda esse problema através de suas conexões residuais, que efetivamente permitem que os gradientes sejam retropropagados para camadas mais anteriores. Isso significa que, em vez de tentar aprender uma representação inteiramente nova em cada camada, cada camada na ResNet aprende apenas a diferença (ou "resíduo") entre sua entrada e saída. Isso permite que a ResNet treine redes significativamente mais profundas, com muitos modelos ResNet tendo centenas ou mesmo milhares de camadas.

Desde a sua introdução, a ResNet tem sido amplamente utilizada em uma variedade de aplicações de visão computacional, desde o reconhecimento de imagens até a detecção de objetos, e continua a ser uma das arquiteturas de rede neural convolucional mais populares e influentes até hoje.

### 5.2 Conexões Residuais

As conexões residuais são a inovação central da arquitetura ResNet e são a razão pela qual a ResNet pode treinar redes muito mais profundas do que as arquiteturas anteriores.

Em uma rede neural convencional, cada camada aprende uma nova representação da entrada. No entanto, à medida que a rede se torna mais profunda, isso pode se tornar um problema. As camadas mais profundas têm que aprender representações cada vez mais complexas e, eventualmente, a rede pode sofrer do problema do "desaparecimento do gradiente", onde as camadas mais profundas têm dificuldade em aprender.

A ideia por trás das conexões residuais é contornar esse problema. Em vez de cada camada aprender uma nova representação, cada camada em uma ResNet aprende apenas a diferença, ou "resíduo", entre sua entrada e saída. Isso é feito adicionando a entrada original diretamente à saída da camada (daí o nome "conexão residual").

Matematicamente, se a entrada para uma camada é x e a função que a camada aprende é F(x), então a saída da camada em uma rede convencional seria $F(x)$. Em uma ResNet, a saída seria F(x) + x. Isso significa que a função F(x) não precisa aprender a representação completa; ela só precisa aprender o resíduo.

Essa abordagem simples, mas poderosa, permite que a ResNet treine redes com centenas ou mesmo milhares de camadas, superando o problema do desaparecimento do gradiente.

### 5.3 Blocos Residuais
    
Os blocos residuais são os componentes fundamentais da arquitetura ResNet. Cada bloco residual consiste em uma série de camadas convolucionais e uma "conexão de atalho" que pula essas camadas.

Em um bloco residual, a entrada passa por uma série de camadas convolucionais, cada uma seguida por uma função de ativação não linear. No entanto, em vez de passar a saída dessas camadas diretamente para a próxima camada, a entrada original é adicionada à saída das camadas convolucionais. Isso é chamado de "conexão de atalho" ou "conexão residual".

Matematicamente, se a entrada para o bloco residual é $x$ e a saída das camadas convolucionais é $F(x)$, então a saída do bloco residual é $F(x) + x$. Isso significa que o bloco residual está realmente aprendendo a função $F(x) = y - x$, onde $y$ é a saída desejada. Em outras palavras, o bloco residual está tentando aprender o "resíduo" ou a diferença entre a entrada e a saída desejada.

Essa estrutura permite que a ResNet treine redes muito mais profundas do que seria possível com redes neurais convencionais. Ao adicionar a entrada original à saída das camadas convolucionais, a ResNet pode efetivamente evitar o problema do "desaparecimento do gradiente", onde as camadas mais profundas da rede têm dificuldade em aprender devido à diminuição dos gradientes durante a retropropagação.<br>

<center><img src="https://miro.medium.com/v2/resize:fit:1122/1*RTYKpn1Vqr-8zT5fqa8-jA.png"></center>

### 5.4 Profundidade da ResNet

A profundidade de uma rede neural se refere ao número de camadas que ela possui. Uma das principais vantagens da ResNet é a sua capacidade de suportar redes muito mais profundas do que as arquiteturas anteriores.

As redes neurais convencionais tendem a sofrer de um problema conhecido como "desaparecimento do gradiente" à medida que se tornam mais profundas. Isso ocorre porque, durante o treinamento, os gradientes que são retropropagados para as camadas mais antigas tendem a se tornar muito pequenos. Como resultado, as camadas mais antigas da rede têm dificuldade em aprender.

A ResNet aborda esse problema através do uso de conexões residuais. Ao adicionar a entrada original diretamente à saída de cada bloco residual, a ResNet permite que os gradientes sejam retropropagados diretamente através da rede. Isso permite que a ResNet treine redes com centenas ou mesmo milhares de camadas.

Existem várias variantes da ResNet, cada uma com um número diferente de camadas. Por exemplo, a ResNet-18 tem 18 camadas, a ResNet-34 tem 34 camadas, a ResNet-50 tem 50 camadas, e assim por diante. Em geral, as redes mais profundas são capazes de aprender representações mais complexas, mas também são mais difíceis de treinar e mais propensas a overfitting.

### 5.5 Treinamento da ResNet

O treinamento da ResNet é semelhante ao de outras redes neurais convolucionais. O processo começa com a inicialização dos pesos da rede, geralmente com pequenos valores aleatórios. Em seguida, a rede é treinada iterativamente usando um conjunto de dados de treinamento. Em cada iteração, a rede faz uma previsão com base em sua entrada atual, e essa previsão é comparada com a verdadeira saída usando uma função de perda. A função de perda quantifica o quão longe a previsão está da verdadeira saída.

Os gradientes da função de perda em relação aos pesos da rede são então calculados usando a retropropagação. Esses gradientes são usados para atualizar os pesos da rede na direção que minimiza a função de perda. Este processo é repetido muitas vezes até que a rede seja capaz de fazer previsões precisas sobre os dados de treinamento.

Um aspecto importante do treinamento da ResNet é a escolha do otimizador. O otimizador determina como os pesos da rede são atualizados com base nos gradientes calculados. Alguns otimizadores comuns usados no treinamento da ResNet incluem SGD (Stochastic Gradient Descent), Adam e RMSprop.

Além disso, durante o treinamento, várias técnicas de regularização podem ser usadas para evitar o overfitting. Isso inclui coisas como dropout, weight decay e data augmentation.

### 5.6 Aplicações da ResNet

A ResNet tem uma ampla gama de aplicações na área de visão computacional, graças à sua capacidade de treinar redes profundas e eficientes. Aqui estão algumas das principais aplicações da ResNet:

- **Reconhecimento de Imagens:** A ResNet é frequentemente usada para tarefas de reconhecimento de imagens, onde o objetivo é identificar o objeto principal em uma imagem. Por exemplo, a ResNet pode ser treinada para reconhecer se uma imagem contém um gato ou um cachorro.

- **Detecção de Objetos:** A ResNet também pode ser usada para detecção de objetos, que é uma tarefa mais complexa que envolve identificar vários objetos em uma imagem e desenhar uma caixa delimitadora ao redor de cada um. Por exemplo, a ResNet pode ser usada para identificar carros, pessoas e sinais de trânsito em uma imagem de uma cena de rua.

- **Segmentação Semântica:** A segmentação semântica é uma tarefa de visão computacional que envolve classificar cada pixel em uma imagem como pertencente a uma determinada classe. A ResNet pode ser adaptada para tarefas de segmentação semântica através do uso de uma arquitetura de rede totalmente convolucional.

- **Transferência de Aprendizado:** A ResNet pré-treinada no conjunto de dados ImageNet é frequentemente usada como ponto de partida para muitas tarefas de visão computacional. O modelo pré-treinado pode ser ajustado fino em um novo conjunto de dados com um número relativamente pequeno de imagens, permitindo que o modelo se beneficie do aprendizado prévio da ResNet.

## 🚀 6. Carregando e Treinando o Modelo Pré-treinado ResNet

### 6.1 Carregando os Pesos do ResNet-18<br>

Este bloco de código define uma função chamada `load_resnet18` que carrega o modelo ResNet-18 e ajusta suas configurações para a tarefa de classificação de imagens no conjunto de dados CIFAR-10.

Aqui está uma explicação detalhada:

- `model = models.resnet18(pretrained=pretrained)`: Esta linha carrega a arquitetura ResNet-18. Se `pretrained=True`, o modelo é inicializado com pesos pré-treinados no ImageNet.

- `for param in model.parameters(): param.requires_grad = True`: Este loop habilita o ajuste fino de todas as camadas do modelo. Isso significa que os gradientes serão calculados para os parâmetros do modelo durante o treinamento, permitindo que seus valores sejam atualizados.

- `model.fc = nn.Linear(in_features=512, out_features=10, bias=True)`: Aqui, a última camada totalmente conectada do modelo (que originalmente foi projetada para a classificação de 1000 classes no ImageNet) é substituída por uma nova camada totalmente conectada para a classificação de 10 classes (o número de classes no CIFAR-10).

- `if torch.cuda.is_available(): model = model.cuda()`: Se uma GPU estiver disponível, o modelo será movido para a GPU para acelerar os cálculos.

- `modelo = load_resnet18(pretrained=True)`: Finalmente, a função `load_resnet18` é chamada para carregar o modelo ResNet-18 com pesos pré-treinados e retornar o modelo ajustado para a tarefa de classificação do CIFAR-10. O modelo retornado é armazenado na variável `modelo`.

### 6.2 Configurações de Otimização<br>

Este bloco de código define o otimizador, o agendador e a função de custo que serão usados para treinar o modelo ResNet-18 no conjunto de dados CIFAR-10. Aqui está uma explicação detalhada:

- `optimizer = torch.optim.AdamW(modelo.parameters(), lr=0.001, weight_decay=0.01)`: Esta linha define o otimizador como AdamW, que é uma variação do algoritmo de otimização Adam que inclui a decaimento de peso (também conhecido como regularização L2). O otimizador AdamW é conhecido por ter um bom desempenho em tarefas de aprendizado profundo. Os parâmetros do modelo são passados para o otimizador, juntamente com a taxa de aprendizado (`lr=0.001`) e o fator de decaimento de peso (`weight_decay=0.01`). A fórmula de atualização do peso no AdamW é a seguinte:

<center><img src="imagens/AdamW.png"></center>

- `scheduler = torch.optim.lr_scheduler.StepLR(optimizer, step_size=10, gamma=0.5)`: Esta linha define o agendador de taxa de aprendizado, que ajusta a taxa de aprendizado durante o treinamento. Neste caso, a taxa de aprendizado é multiplicada por `gamma=0.5` a cada `step_size=10` épocas. Isso é útil para reduzir a taxa de aprendizado à medida que o treinamento progride, o que pode levar a um melhor desempenho do modelo.

- `criterion = nn.CrossEntropyLoss()`: Esta linha define a função de custo como a perda de entropia cruzada, que é comumente usada para tarefas de classificação. A perda de entropia cruzada mede a dissimilaridade entre a distribuição de probabilidade prevista pelo modelo e a distribuição de probabilidade verdadeira dos rótulos. A função de perda de entropia cruzada (CrossEntropyLoss) é calculada usando a seguinte fórmula:

$$ℓ(x, y) = L = {l_1, …, l_N}^T, \quad l_n = - w_{y_n} \log \frac{\exp(x_{n, y_n})}{\sum_{c=1}^C \exp(x_{n, c})} \cdot 1_{y_n ≠ ignore\_index}$$

Na fórmula:

- $ℓ(x,y)$ é a perda total para o minibatch.

- $L$ é o vetor de perdas individuais para cada observação no minibatch.

- $l_n$ é a perda para a n-ésima observação no minibatch.

- $w_{y_n}$ é o peso associado à classe verdadeira para a n-ésima observação.

- $x_{n,y_n}$ é a pontuação de logit para a classe verdadeira da n-ésima observação.

- O denominador da fração dentro do logaritmo é a soma das exponenciais das pontuações de logit para todas as classes, que é basicamente a parte do softmax da entropia cruzada.

- $1_{y_n \neq ignore_index}$ é uma função indicadora que é igual a $1$ quando $y_n$ é diferente do índice de ignorar (se houver), e $0$ caso contrário.

### 6.3 Treino e Validação do Modelo

O processo de treinamento do modelo é realizado através de uma série de etapas cuidadosamente planejadas para garantir a melhor performance possível. Aqui estão os critérios usados para treinar o modelo:

1. **Checkpoint Callback**: Durante o treinamento, o modelo é salvo no diretório "best/" sempre que a acurácia de validação (`valid_acc`) atinge um novo máximo. Isso garante que sempre tenhamos acesso ao melhor modelo treinado.
2. **Parada Antecipada**: O treinamento é interrompido se a acurácia de validação não melhorar após 3 épocas (`patience=3`). Isso evita o desperdício de recursos computacionais e previne o overfitting.
3. **Treinador**: O treinador é configurado para usar aceleração automática e aproveitar a GPU, se disponível. Os logs do treinamento são salvos no diretório "./logs". O número máximo de épocas para o treinamento é definido como 100.
4. **Treinamento**: O modelo é então treinado usando os dados de treino e validação fornecidos.

Este processo garante que o modelo seja treinado de forma eficiente e eficaz, levando a um modelo de alta performance.

## 📊 6. Avaliação do Modelo ResNet

Nesta seção, mergulharemos profundamente na avaliação do nosso modelo de aprendizado de máquina. A avaliação é uma etapa crucial no processo de aprendizado de máquina, pois nos permite entender o desempenho do nosso modelo. Vamos explorar várias métricas e técnicas para avaliar a precisão, a robustez e a eficácia geral do nosso modelo. Isso nos ajudará a entender onde o nosso modelo brilha e onde ele pode precisar de melhorias. 

### 6.1 Processo de Aprendizado do ResNet

Neste tópico, vamos visualizar o processo de aprendizado da arquitetura ResNet através de gráficos que mostram a acurácia e o erro de treinamento e validação para cada época. Esses gráficos são ferramentas poderosas que nos permitem entender como nosso modelo está aprendendo durante o treinamento.

Ao observar a acurácia de treinamento e validação, podemos ver como nosso modelo está se saindo em termos de aprendizado e generalização. Idealmente, queremos ver a acurácia de treinamento e validação aumentar ao longo do tempo.

Da mesma forma, ao olhar para o erro de treinamento e validação, podemos ver quão longe as previsões do nosso modelo estão dos rótulos verdadeiros. Neste caso, nosso objetivo é minimizar esses erros ao longo do tempo.

<img src="imagens/grafico.png">

### 6.2 Relatório de Classificação

Neste tópico, vamos explorar o desempenho do nosso modelo ResNet em detalhes. Para isso, vamos avaliar as métricas de Accuracy (Acurácia), Precision (Precisão), Recall (Revocação) e F1-Score. Essas métricas nos fornecem uma visão abrangente da performance do nosso modelo.

- **Accuracy (Acurácia)**: Esta métrica nos dá uma visão geral de quão bem o nosso modelo está performando. É calculada como a proporção de previsões corretas feitas pelo modelo em relação ao total de previsões. A fórmula para a acurácia é:

    $$\text{Accuracy} = \frac{\text{Número de previsões corretas}}{\text{Número total de previsões}}$$

- **Precision (Precisão)**: Esta métrica nos diz qual proporção das identificações positivas foi realmente correta. É calculada como:

    $$\text{Precision} = \frac{\text{Verdadeiros Positivos}}{\text{Verdadeiros Positivos} + \text{Falsos Positivos}}$$

- **Recall (Revocação)**: Esta métrica nos diz qual proporção dos positivos reais foi identificada corretamente. É calculada como:

    $$\text{Recall} = \frac{\text{Verdadeiros Positivos}}{\text{Verdadeiros Positivos} + \text{Falsos Negativos}}$$

- **F1-Score**: Esta é uma métrica que combina Precision e Recall em um único número. O F1-Score é a média harmônica de Precision e Recall, e dá mais peso a valores baixos, de forma que ambas as métricas devem ser altas para obter um F1-Score alto. É calculado como:

    $$\text{F1-Score} = 2 \times \frac{\text{Precision} \times \text{Recall}}{\text{Precision} + \text{Recall}}$$

---

|           | precision | recall | f1-score | support |
|-----------|-----------|--------|----------|---------|
|         0 |       0.96|   0.94 |     0.95 |    1000 |
|         1 |       0.98|   0.97 |     0.97 |    1000 |
|         2 |       0.91|   0.95 |     0.93 |    1000 |
|         3 |       0.89|   0.88 |     0.88 |    1000 |
|         4 |       0.94|   0.97 |     0.95 |    1000 |
|         5 |       0.91|   0.92 |     0.91 |    1000 |
|         6 |       0.98|   0.94 |     0.96 |    1000 |
|         7 |       0.98|   0.96 |     0.97 |    1000 |
|         8 |       0.95|   0.97 |     0.96 |    1000 |
|         9 |       0.95|   0.97 |     0.96 |    1000 |
|           |           |        |          |         |
| accuracy  |           |        |     0.94 |   10000 |
| macro avg |       0.95|   0.94 |     0.94 |   10000 |
|weighted avg|      0.95|   0.94 |     0.94 |   10000 |

Com base nos resultados da tabela, o seu modelo ResNet apresentou um desempenho geral sólido. A precisão geral do modelo foi de 0,94, indicando que o modelo fez previsões corretas em 94% dos casos no conjunto de teste de 10.000 imagens.

Ao analisar as métricas para cada classe individualmente, podemos ver que o modelo teve um desempenho excepcionalmente bom nas classes 1, 6 e 7, com uma precisão de 0,98. Isso significa que o modelo foi capaz de identificar corretamente as imagens dessas classes na maioria das vezes.

As classes 0, 4, 8 e 9 também tiveram um bom desempenho, com uma precisão de mais de 0,94. No entanto, as classes 2, 3 e 5 tiveram um desempenho ligeiramente inferior em comparação com as outras classes, com uma precisão de cerca de 0,91.

Em termos de recall, que é a capacidade do modelo de encontrar todas as amostras positivas, o modelo teve um desempenho semelhante em todas as classes, variando entre 0,88 e 0,97.

O F1-score, que é uma média harmônica entre precisão e recall, também foi consistente em todas as classes. Isso indica que o modelo tem um bom equilíbrio entre precisão e recall.

## 🗺️ 7. Mapas de Recursos

Neste tópico, vamos explorar os mapas de recursos gerados pelo nosso modelo ResNet. Os mapas de recursos, também conhecidos como mapas de ativação, são uma maneira eficaz de entender o que uma rede neural convolucional aprendeu durante o treinamento.

Cada camada em uma rede neural convolucional produz um mapa de recursos como saída. Esses mapas de recursos representam as características que a rede extraiu da imagem de entrada em cada camada. Ao visualizar esses mapas de recursos, podemos ter uma ideia das características que a rede considera importantes para fazer suas previsões.

Neste tópico, vamos analisar os mapas de recursos do nosso modelo treinado. Isso nos permitirá entender melhor como o nosso modelo está tomando suas decisões e quais características ele está usando para fazer suas previsões.

<img src="imagens/sapo.png">

### 7.1 Saída da Camada de Convolução

<img src="imagens/conv1.png">

### 7.2 Saída da Função de Agregação - MaxPool2d

<img src="imagens/maxpool2d.png">