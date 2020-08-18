# Classifica-o-de-produtos

## Linha de Raciocínio

## Descrição do Problema:

Construção de um classificador de produtos que recebe um conjunto de características de um produto e retorna a categoria dele.

# Parte 1 - Análise Exploratória

### 1 : Anaĺise dos campos que podem ser considerados classificatórios para a identificação do produto

O dataset disponivel para a solução do problema é composto de uma amostragem de dados da plataforma do Elo7.
O Elo7 é o maior site brasileiro de compra e venda de artesanato.
Através da plataforma online compradores podem compra diretamente de milhares de pessoas que transformam ideias criativas em produtos únicos e diferenciados.
o dataset disponivel contém 38.507 registros distribuídos em 5 categorias (Bebê, Bijuterias e Jóias, Decoração, Lembrancinhas, Papel e Cia e Outros).
Estes registros foram gerados através de cada clique em um produto a partir de um termo de busca do usuário no site.

Para construir um modelo classificador de produtos a partir de suas características precisamos analisar se existem campos disponiveis no dataset que são determinantes para a identificação da categoria dos produtos.

Intuitivamente baseado no nosso conhecimento do negocio e fazendo uma analogia dos campos que usamos para identificar os produtos podemos avaliar os campos que seriam determinantes para identificar a categoria dos produtos.

Uma análise intuitiva dos campos disponivel no dataset podemos avaliar item a item do dataset, como uma visão prévia dos possiveis candidatos.

* **product_id -** identificação de produto. Não é um fator determinante para a identificação do produto, número sem qualquer correlação com a categoria.
* **seller_id** - identificação do vendedor. Não é um fator determinante para a identificação do produto, número de identificação do vendedor, podendo vender qualquer categoria e não sendo uma caracteristica do produto.
* **query** - termo de busca inserido pelo usuário. Forte candidato por ser termos cadastrados pelo vendedor para retornar o produto, ou sejam são incluidas aqui caracterististicas que determimam o produto.
* **search_page** - número da página que o produto apareceu nos resultados de busca (mín 1 e máx 5). Não é um fator determinante para a identificação do produto, não importante para caracteristica do produto.
* **position** - número da posição que o produto apareceu dentro da página de busca (mín 0 e máx 38). Não é um fator determinante para a identificação do produto, não é importante para caracteristica do produto.
* **title** - título do produto. Forte candidato por possuir termos cadastrados pelo vendedor para o produto, ou sejam são incluidas aqui caracterististicas que determimam o produto.
* **concatenated_tags** - tags do produto inseridas pelo vendedor (as tags estão concatenadas por espaço). Forte candidato por possuir termos cadastrados pelo vendedor para retornar o produto, ou sejam são incluidas aqui caracterististicas que determimam o produto.
* **creation_date** - data de criação do produto na plataforma do Elo7. Não é um fator determinante para a identificação do produto, não importante para caracteristica do produto.
* **price** - preço do produto em reais. Neste caso deve ser feito uma análise exploratória das distribuições de preços por categorias para identificar se tem alguma correlação do preço com a categorias. Provavelmente terá uma alta distribuição, sendo assim não será um fator determinante para categorizar o produto.
* **weight** - peso em gramas da unidade do produto reportado pelo vendedor. Neste caso deve ser feito uma análise exploratória das distribuições de preços por categorias para identificar se tem alguma correlação do preço com a categorias. provavelmente terá uma alta distribuição, sendo assim não será um fator determinante para categorizar o produto.
* **express_delivery** - indica se o produto é pronta entrega (1) ou não (0). Não é um fator determinante para a identificação do produto, não é importante para caracteristica do produto.
* **minimum_quantity** - quantidade de unidades mínima necessária para compra. Não é um fator determinante para a identificação do produto, não é importante para caracteristica do produto.
* **view_counts** - número de cliques no produto nos últimos três meses. Não é um fator determinante para a identificação do produto, não é importante para caracteristica do produto. 
* **order_counts** - número de vezes que o produto foi comprado nos últimos três meses. Não é um fator determinante para a identificação do produto, não é importante para caracteristica do produto.
* **category** - categoria do produto. Categoria do produto cadastrada pelo usuário.

Analisando a base de dados através do conhecimento de negócio os itens relevantes para a determinação do produto são dados de textos livres, não estruturados como o campo "query" que é o termo de busca inserido pelo usuário, o campo "title", que é o título do produto, e a "concatenated_tags", que são as tags do produto inseridas pelo vendedor. 
Temos o preço (price) e o tamanho (weight) que devem ser avaliados se existisse alguma correlação forte entre eles com a categoria.

### 2 : Matriz de Correlação de Pearson

Como provas matematicamente da baixa correlação entre os campos podemos criar uma matriz de correlação para demonstrar as correlações entre os campos. Através da matriz de correlação podemos ter uma visão dos valores de correlação de Pearson entre todos os campos disponiveis no dataset.
Desta forma teremos o embasamento matemático da hipotese da análise dos campos de maior relevância para a identificação da classificação do produto. 
A avaliação dos campos através da matriz de correlação que medirá o grau de relação linear entre cada par de itens disponivel no dataset.
Os valores de correlação da matriz são compostos por uma variação entre -1 e +1.
Matematicamente a matriz de correlação avaliará a força e a direção da relação entre duas caracteristica, sendo consideradas variáveis altamente correlacionados as que variavies que obterem valores de correlação maiores do que 0,7 tanto positivos como negativos.

**Matriz de correlação**

Na Matriz de correlação podemos notar que os campos 'product_id' , 'seller_id', 'search_page', 'position','creation_date', 'express_delivery', 'minimum_quantity', 'view_counts', 'order_counts', possuem correlações insignificantes entre eles, sendo encontrado somente uma correlação de 0.6 entre os campos view_counts e order_counts, porém este valores não representam nenhuma correlação com a categorização do produto.

Medindo a correlação de pearson da categoria com os campos "price" and "weight" podemos avaliar que também não existem correlações suficientes, tendo o tamanho uma correlação da categoria de -0.017 e com o preço de -0.219, ou seja estes campos também não contrubiem para a identificação do produto.


### 3 - Exclução dos campos que não são fatores determinantes para categorização dos produtos  

Podemos excluir os campos do dataset que evidenciamos matematicamente que não possuem correlação.
Sendo os campos que não tem correlação matemática com a categorização dos produtos
|Campos sem correlação|
|-----------------|
| 1 - product_id |
| 2 - seller_id |
| 3 - search_page |
| 4 - position |
| 5 - creation_date|
| 6 - price|
| 7 - weight |
| 8 - express_delivery |
| 9 - minimum_quantity |
| 10 - view_counts |
| 11 - order_counts|

##### Campos de alta correlação usada pelo usuário para categorização do produto

|Campos com correlação|
|-----------------|
| 1 - query |
| 2 - title |
| 3 - concatenated_tags |


##### Campo de categoria

|Campos|
|-----------------|
| 1 - category |

Dos 15 itens disponivel no dataset 11 deles não apresentaram correlação suficiente para a identificação da categoria do produto, sendo 3 itens de preenchimento de texto livre não estruturado usados pelo usuário para a categorização do produto, como titulo do produto, query de busca do produto e as tags para a busca do produto.
Entretando estes campos são campos não estruturados, impossibilitando a construção de modelos por meio de tecnicas que necessitam de analisar caracteristicas estruturadas.

Para a solução do problema baseado nos campos de valores não estruturados precisaremos criar um modelo que seja capaz de aprender baseado em textos livres, baseados na análise dos campos como titulo, query e tags.

O modelo teria que fazer um minetismo da forma humana de classificar os produtos através de textos não estruturados.

### 4 - Anaĺise da distribuição das categorias dos produtos no dataset de amostragem

Precisamos avaliar a distribuição das categorias no dataset, a fim de avaliar se temos registros suficientes de cada categoria para que o modelo possa "aprender" a classificar o produto.

Estão disponiveis no dataset 38.507 registros, sendo:
|Categoria|Quantidade|
|---------|--------|
|Lembrancinhas| 17759|
|Decoração| 8846|
|Bebê| 7026|
|Papel e Cia| 2777|
|Outros| 1148|

Sendo a distribuição de: 
|Categoria|Quantidade|
|---------|--------|
|Lembrancinhas|46,12%|
|Decoração|22,97%|
|Bebê|18,24%|
|Papel e Cia|7,21%|
|Outros|2,98%|

A partir da análise da distribuição podemos avaliar os produtos com maior porcentagem de registros.
Estes produtos terão maior probabilidade de aprendizado do modelo na classificação de suas categorias.

### 5 - Identificação do Corpus 

Partindo do principio que não temos campos disponiveis no dataset de forma estruturado que sejam relevantes para a identificação da categoria dos produtos precisamos analisar o corpus nos campos de textos livres.

Dado que temos os campos:
* query - termo de busca inserido pelo usuário;
* title - título do produto e 
* concatenated_tags - tags do produto inseridas pelo vendedor,

Precisamos avaliar qual deles teriam o melhor corpus para a identificação do produto.
O modelo deve analisar o campo com melhor corpus para identificação da categoria dos produtos.
Uma análise humana lendo apenas 1 destes campos poderia identificar com precisão a categoria dos produtos tendo em mente que temos apenas 5 possiveis categorias.
O modelo para a identificação da categoria do produto pelo corpus também teria que ter a mesma habilidade.
Lembrando que no caso do modelo, ele não terá aprendizagem anterior como um ser humano teria para a identificação dos produtos.
O modelo aprenderá apenas com o corpus disponivel no dataset de treinamento, sendo assim, podemos ter categorias com menor probabilidade de assertividade devido a quantidade limitada de registros, conforme demonstrado na análise de distribuição exposta no passo 4.  

##### Análisando as palavras unicas disponivel em cada campo temos:

Precisamos analisar o tamanho do corpus disponivel em cada campo.

|Campo    |Quantidade|
|---------|--------|
|title    |25.355|
|tags     |23.011|
|query    |6.397 |

Note que o corpus que temos de palavras no titulo e nas tags estão muito próximas, estão entre 23 mil e 26 mil e para a busca este corpus reduz para +- 6 mil palavras.

Precisamos então avaliar qual seria o campos com maior indice de precisão para classificação de produto, o campo com maior quantidade de palavras unicas ou um campo com menor quantidade de palavras.

Estes são os valores do tamanho do corpus de cada campo não estruturado, ou seja a quantidade de palavras unicas usados para determinar a classificação dos produtos.

Uma análise humana lendo apenas 1 destes campos poderia identificar com precisão a categoria dos produtos tendo em mente que temos apenas 5 possiveis categorias.

### 6 - Tratamento do Corpus

Para que um modelo possa analisar estes dados precisamos transformar estes dados em valores numéricos. Valores numéricos serão atribuídos a um número de n-grams por sequência.
Vamos precisar usar uma função de tokenização que determinará um index para cada palavra presente no corpus.
Para que o modelo possa analisar estes dados é necessário formatá-los de modo que todas as sequências tenham o mesmo tamanho, que no caso do texto livre podemos analisar que eles tem tamanhos distintos.
Então para uma análise de máquina em valores númericos precisamos utilizar outra função que será responsável por truncar as sequências maiores e completar as sequências menores com zeros.
A função criará o corpus para o treinamento do modelo com as variaveis de title e query, e desta forma podemos avaliar qual variavel tem o melhor corpus para o treinamento do modelo.

##### Criação da base do corpus

Vamos criar um conjunto de base de treinamento contendo o corpus disponivel no 'title', onde teremos a variável X contendo os corpus do title e a variável Y contendo a classificação dos produtos, campo 'category'. 
Este campo será usado pelo modelo para "aprender" através do corpus qual a categoria do produto.
Da mesma forma teremos o corpus de 'query', onde a variável X contém o corpus do query e a variável Y contendo a classificação dos produtos.

Tendo o conjunto de arrays de X_title de tamanho de [38507,14] and e Y_title [38507,6] e X_query de tamanho de [38507,15] and e Y_title [38507,6].


### 7 - Dataset de Treinamento e Validação

Dado que temos então 2 possivel candidatos de corpus para a identificação dos produtos, precisamos então separar o conjunto de treinamento e validação do modelo.
Usaremos de cada corpus, 20% para validação e 80% para treinamento.

Tendo o dataset de treinamento de 30.805 divididos por 6 para treinamento e 7.702 divididos por 6 para validação do modelo tanto para o corpus de 'title' como o de 'query'.

Temos então 30.805 registros para treinamento do modelo e 7.702 registro para a validação do modelo para cada corpus.

Com a base de dados de treinamento e validação definida precisamos agora pensar na criação de um modelo que seja capaz de "aprender" dado o corpus.

# Parte 2 - Sistema de Classificação de Produtos

### 8 - Analogia com o Pensamento humano

Na análise de correlação feita nos campos disponivel no dataset não foram encontradas caracteristicas estruturadas que tivessem correlação com a categoria do produto. Devido a falta de correlação entre as variáveis estruturadas com a categoria, não teremos uma solução pláusivel usando modelos que façam a análise levando em consideração variaveis estruturadas.

Partindo deste premissa para seja possivel criar um modelo de aprendizado através do corpus precisamos criar um modelo usando tecnicas avançadas de treinamento de máquina.

O objetivo é criar um modelo que seja capaz de classificar os produtos como os seres humanos fariam, dado que temos somente campos que identifiquem a categoria do produto com textos livres e não estruturados.

Fazendo uma analogia com o pensamento humano, se fossemos identificar os produtos através de sua descrição tanto do titulo como na query, nós não analisariamos do zero a cada segundo, ou a cada palavra.
Nós interpretaríamos as palavras com base na compreensão das palavras anteriores. Desta forma vamos identificando e classificando o texto.
Nós não jogamos compreendemos cada palavra com um significado único, mas vamos lendo as palavras e formando a interpretação baseada na palavra anterior, ou seja nossos pensamentos tem uma persistência.

Para resolver este problema através da análise do corpus, o modelo criado também terá que ser capaz de ter esta persistência para poder classificar as categorias dos produtos baseados na descrição do textos.

### 9 - Técnica escolhida para desenvolvimento do modelo

Com modelos tradicionais seria impossivel a classificação dos produtos pelo corpus, até mesmo se tratando de redes neurais tradicionais.
A necessidade de ter esta persistência dificulta a aplicação para resolver este problema de classificação de produto baseado em textos não estruturados.

Por exemplo, aqui temos um problema onde para a classificação de produtos a partir da descrição temos que não somente usar as palavras, mas levar em consideração as palavras anteriores. Temos que minetizar uma compreensão das frases, assim como um ser humano faria.

Um modelo estatístico ou uma rede neural tradicional não seria capaz de ter este aprendizado consistente.

Precisamos então usar as Redes Neurais Recorrentes para resolver esse problema, pois estas redes possuem um aprendizado com loops, e isso tecnicamente permite que o nosso modelo tenha está consistencia e "aprenda" com informações persistentes.

O nosso modelo será baseado em rede neural recorrente, tendo redes de múltiplas com cópias da mesma rede.
Cada rede passará uma mensagem a rede sucessora e desta forma criaremos a persistencia que precisamos para resolver o problema de classificação de produto baseado nos textos livres dos campos como titulo ou query.

### 10 - Redes Neurais Recorrentes

Definindo a técnica que melhor se adequa ao problema, sendo as Redes Neurais Recorrentes, precisamos definir as camadas do tipo Embedding, LSTM e Dense da rede, sendo:

* **Embedding** que são as camadas da rede que aprende as representações mais relevantes para as camandas posteriores.
* **LSTM** que são as camadas da rede que permite que as sequências sejam tratadas como um conjunto de dados onde o contexto relevante para que o determinado atributo. 
* **Dense** que são as camadas utilizadas para fazer o processo de transformação da representação produzida pelo LSTM até se atingir a camanda final de neurônios que servirá de camanda de saída.

O corpus máximo determinado conforme as palavras únicas encontradas em cada corpus.
* max_features_title = 25355
* max_features_query = 6397

O modelo terá camadas de Embedding de 128 e LSTM de 196, e um dropout de 0.5 para evitar que a rede decorre o dado ao invés de aprender.
Como temos 6 categorias teremos uma Dense de 6 neuromios para representar as 6 possiveis saidas.


#### Para o modelo para corpus do campo Titulo
_________________________________________________________________

embedding_1 (Embedding)      (None, 14, 128)           3245440   
spatial_dropout1d_1 (Spatial (None, 14, 128)           0         
lstm_1 (LSTM)                (None, 196)               254800    
dense_1 (Dense)               (None, 6)               1182    
_______________________________________________________________
Total params: 3,501,422
Trainable params: 3,501,422
Non-trainable params: 0


#### Para o modelo para o corpus do campo Query 

embedding_1 (Embedding)      (None, 15, 128)           818816   
spatial_dropout1d_1 (Spatial (None, 15, 128)           0         
lstm_1 (LSTM)                (None, 196)               254800    
dense_1 (Dense)               (None, 6)               1182    
______________________________________________________________
Total params: 1,074,798
Trainable params: 1,074,798
Non-trainable params: 0

### 11 - Treinamento dos modelos 

Para o treinamento do modelo vamos utilizar 20 epocas como ciclos de treinamento completo no conjunto de treinamento.
Vamos testar e comparar os modelos usando 20 ciclos, pois as quantidade de epocas suficientes para convergência variam de dados para dados. 
O objetivo de usar a mesma quantidade de epocas é ter a mesma metrica para avaliar a convergencia dos modelos.
A quantidade de epocas no treinamento pode ser interrompida quando o erro ficar abaixo de um valor X determinado.
As 20 epocas também entra no território da prevenção do excesso de ajustes, este é um valor inicial para validação dos modelos treinados no corpus do campo title e query.

### 12 - Perda e Acurácia do modelo por época

Análise dos modelos durante os 20 cliclos de treinamento, notamos que o modelo treinado no corpus de 'title' convergiu com maior acurácia com 20 ciclos do que o modelo treinando no corpus de 'query'. 
Conforme demonstrado abaixo:
##### Modelo com aprendizagem no corpus do 'title'
Com épocas de 20/20, o modelo de aprendizagem no corpus do titulo obteve: uma perda de 0.1504 e acurácia de 0.9432.
##### Modelo com aprendizagem no corpus do 'query'
Com épocas de 20/20, o modelo de aprendizagem no corpus do titulo obteve: uma perda de 0.3621 e acurácia de 0.8677.

Note que para o modelo de aprendizagem pelo corpus do campo query seriam necessárioss mais épocas para maior aprendizagem do modelo.
Os modelos são comparados com o mesmo número de epocas, a fim de avaliar qual o corpus que podemos utilizar para a classificação do modelo baseado em dados não estruturados.

# Parte 3 - Avaliação do Sistema de Classificação

### 13 - Validação da Precisão do Modelo

Para a avaliação dos modelos podemos utilizar 4 metricas, sendo:

* A **acurácia** é uma boa indicação geral de como o modelo performou.
* A **precisão** pode ser usada em uma situação em que os Falsos Positivos são considerados mais prejudiciais que os Falsos Negativos.
* O recall pode ser usada em uma situação em que os Falsos Negativos são considerados mais prejudiciais que os Falsos Positivos. 
* O **f1-Score** é simplesmente uma maneira de observar somente 1 métrica ao invés de duas (precisão e recall) em alguma situação.

##### Modelo de Redes Neurais Recorrentes usando o corpus do campo "title"
O modelo obteve a acurácia de 88.18% em 7.702 registros de validação.

**Acurácia de 88.18%**

|Category|Precision | Recall   | F1-Score   |
|--------|----------|----------|------------|
|Bebê                |0.89|0.86|0.88|
|Bijuterias e Jóias  |0.93|0.94|0.94|
|Decoração           |0.91|0.88|0.90|
|Lembrancinhas       |0.90|0.92|0.91|
|Papel e Cia         |0.75|0.75|0.75|
|Outros              |0.78|0.73|0.76|

##### Modelo de Redes Neurais Recorrentes usando o corpus do campo "query"
O modelo obteve a acurácia de 83.47% em 7.702 registros de validação.

**Acurácia de 83.47%**

|Category|Precision | Recall   | F1-Score   |
|--------|----------|----------|------------|
|Bebê                |0.89|0.79|0.84|
|Bijuterias e Jóias  |0.87|0.85|0.86|
|Decoração           |0.86|0.82|0.84|
|Lembrancinhas       |0.85|0.92|0.88|
|Papel e Cia         |0.79|0.56|0.66|
|Outros              |0.81|0.49|0.61|

### 14 - Conclusão 

Levando em consideração toda a análise exploratória dos dados e as metodologia científica que foram usados na elaboração do solução deste problema podemos concluir que a classificação de produtos baseado no corpus é possivel através de um modelo usando redes neurais recorrentes.

O modelo de categorização de produtos usando o corpus do title obteve melhor métrica com menor ciclo de treinamento, tendo -+88,% de acurácia e o modelo usando o corpus do campo 'query' também obteve bons resultados sendo necessário usar mais epocas para obter um maior ganho de precisão. 
