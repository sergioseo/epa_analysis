# Análise dos dados da EPA

<p>Os dados de teste usados para determinar as estimativas de economia de combustível são derivados dos testes veiculares feitos no <b><a href="https://www.epa.gov/compliance-and-fuel-economy-data/data-cars-used-testing-fuel-economy">Laboratório Nacional de Veículos e Emissões de Combustível da EPA</a></b> em Ann Arbor, Michigan, e pelos fabricantes de veículos que enviam seus próprios dados de teste à EPA. 
  
  Cada amostra vem com uma classificação de qualidade de 0 a 10 e o resultado de vários testes físico-químico. Em poucas palavras iremos analisar, a princípio, propriedades como quantidade de alcool, nível de acidez e o açucar residual.</p>
<p>Os arquivos estão no formato <code>CSV</code> e os seus valores separados por <code>;</code> possuindo 11 colunas de propriedades químicas + Qualidade:</p>
<ul>
  <li>Acidez fixa</li>
  <li>Acidez volátil</li>
  <li>Ácido cítrico</li>
  <li>Açúcar residual</li>
  <li>Cloretos</li>
  <li>Dióxido de enxofre livre</li>
  <li>Dióxido de enxofre total</li>
  <li>Densidade</li>
  <li>pH</li>
  <li>Sulfatos</li>
  <li>Álcool</li>
  <li>Qualidade (que tem pontuação de 0 a 10)</li>
</ul>

## Primeira Etapa

Se já tiver algum dos programas listados abaixo e quiser usá-los, apenas certifique-se de estarem atualizados e instalados corretamente. Para o bom funcionamento e a visualização correta do projeto será necessário o download de alguns arquivos:
<ul>
  <li>Fazer o donwload e instalar o <a href="https://www.anaconda.com/">Anaconda</a></li>
  <li>Fazer o donwload e instalar o <a href="https://www.winzip.com/win/en/downwz.html">WinZip</a>(Version para Win. No Mac há a opção direta sem necessidade de outro software)</li>
  <li>Fazer o download dos arquivos <code>winequality-red.csv</code> e <code>winequality-white.csv</code> no repositório <a href="https://github.com/sergioseo/Wine_quality">Wine_quality</a></li>
  <li>Depois de instalar o <code>Anaconda</code> procure pelo <code>Jupyter Notebook</code> e click em <code>install</code> e depois <code>launch</code></li>
</ul>
  
### Algumas questões antes de começar

<p>É importante pensarmos em algumas questões relevantes ao projeto antes de começar a de fato explorar o dataSet; tais como:
  <ol>
    <li>Quais características químicas são mais importantes para prever a qualidade do vinho?</li>
    <li>Existe um certo tipo de vinho (tinto ou branco) associado a uma melhor qualidade?</li>
    <li>Vinhos com maior teor alcoólico recebem classificações maiores?</li>
    <li>Vinhos mais doces (mais açúcar residual) recebem classificações maiores?</li>
    <li>Qual nível de acidez está associado a uma melhor qualidade?</li>
  </ol>

## Parte 1: Avaliando o DataSet

<p>A principio vamos inspecionar os dataSets <code>winequality-red.csv</code> e <code>winequality-white.csv</code>usando  <code>Pandas</code> e abri-los para verificar os rótulos e os separadores. No nosso caso temos <code>;</code>como separadores e não podemos esquecer de usar <code>sep=";"</code> para que as colunas sejam criadas separadamente.Também temos que nos atentar aos rótulos e uma dica q eu sempre dou é usar um editor simples (ATOM, SUBLIME, VSCODE, etc) para dar uma olhada e verificar se o dataSet possui rótulos como um cabeçalho.
<p>Passaremos para os seguintes passos:</p>
<ol>
  <li>Verificar o número de amostras em cada conjunto de dados</li>
  <li>Verificar o número de colunas em cada conjunto de dados</li>
  <li>Verificar os recursos com valores faltantes</li>
  <li>Verificar as linhas duplicadas em cada conjunto de dados</li>
  <li>Verificar o número de valores únicos para qualidade em cada conjunto de dados</li>
  <li>Verificar a densidade média em cada conjunto de dados</li>
</ol>

### Conclusão da Parte 1

<p>Através desta rápida avaliação podemos chegar a algumas repostas interessantes:</p>
  <ul>
  <li>Os conjuntos de dados não possuem dados faltantes</li>
  <li>Há valores duplicados mas a principio não tem necessidade de descartá-los</li>
  <li>A densidade média do <b>Vinho Tinto = 0.996747</b> e a do <b>Vinho Branco = 0.994027</b></li>
  </ul>
<p>Para acessar os métodos utilizados <b><a href="https://github.com/sergioseo/Wine_quality/blob/master/avaliando_parte_1.ipynb">clique aqui</a></b>  
</p>

## Parte 2: Unindo os Datasets

<p>Para podermos analisar com mais eficiência nossos datasets vamos combiná-los em um único DataFrame e acrescentar mais uma feature chamada <code>cor</code> para podermos identificar de qual conjunto de dados ele pertence. Para isso usaremos e importaremos o <code>Numpy</code></p>
<p><b>OBS:</b> Ao unir e combinar as colunas teremos uma coluna que não será combinada por estar com o nome levemente errado e para resolver teremos que renomear a coluna <code>total_sulfur-dioxide</code> para <code>total_sulfur_dioxide</code> do dataset <code>winequality-red.csv</code> para que o método <code>append()</code>funcione perfeitamente.
<p>Assim teremos um novo DataFrame chamado <code>winequality_edited.csv</code>. Para acessá-lo <b><a href="https://github.com/sergioseo/Wine_quality/blob/master/winequality_edited.csv">clique aqui</a></b>
<p>Para acessar os métodos utilizados nesta etapa <b><a href="https://github.com/sergioseo/Wine_quality/blob/master/unindo_datasets_parte_2.ipynb">clique aqui</a></b>  
</p>

## Parte 3: Começando as análises

<p>Agora que temos o DataFrame limpo, organizado com a coluna de cores acrescentada podemos discutir essas questões:</p>
<p><b>1. Será que o tipo de vinho está associado a uma qualidade superior?</b></br>
  Para esta pergunta iremos comparar a qualidade média do vinho tinto à qualidade média do vinho branco usando <code>groupby()</code>. Faremos esse agrupamento por cor e, depois, encontraremos a qualidade média de cada grupo.</p>
<p><b>2. Qual o nível de acidez (valor de ph) que recebe a maior avaliação média?</b></br>
Essa pergunta já é um pouco mais complicada porque, ao contrário da cor, que possui categorias claras pelas quais você pode agrupar (tinto ou branco), pH é uma variável quantitativa, sem categorias claras. No entanto, existe uma solução simples para isso: Iremos criar uma variável categórica de uma variável quantitativa criando suas próprias categorias. A função <code>cut()</code> do <code>Pandas</code> permite que você “corte” os dados em grupos podendo assim usar essa função. Daí iremos criar uma nova coluna chamada <code>acidity_levels</code> com essas categorias.</p>
<p>Usaremos métodos como <code>groupby()</code> e <code>cut()</code> para agrupar os valores e "cortá-los", conseguindo assim criar novos rótulos e valores para cada grupo</p>
<p>Para acessar os métodos utilizados <b><a href="https://github.com/sergioseo/Wine_quality/blob/master/comecando_as_analises_parte_3.ipynb">clique aqui</a></b>  
</p>

### Conclusão da Parte 3

<p>Desta forma podemos concluir que a qualidade média do vinho tinto é menor do que à do vinho branco e que o nível de acidez <b>"Baixo"</b> recebe a classificação média mais alta de acordo com esse diagrama:</p>
<p><b>Níveis de acidez:</b></p>
  <ul>
    <li>Alto: Abaixo de 25% dos valores de pH</li>
    <li>Moderadamente alto: 25% a 50% dos valores de pH</li>
    <li>Médio: 50% a 75% dos valores de pH</li>
    <li><b>Baixo: 75% ou mais dos valores de pH</b></li>
  </ul>

## Parte 4: Continuando
<p>Seguiremos com a análise nos atentando a mais essas questões:</p>
<p><b>1. Será que vinhos com maior teor alcóolico recebem avaliações melhores?</b></br>
Para responder a essa pergunta usaremos <code>query()</code> para criar dois grupos de amostras de vinho:</p>
  <ul>
    <li>Baixo álcool (amostras com um teor alcoólico abaixo da média)</li>
    <li>Alto álcool (amostras com um teor alcoólico maior ou igual à média)</li>
  </ul>
<p>Em seguida encontraremos a classificação média de qualidade de cada grupo<p>
<p><b>2. Será que vinhos mais suaves recebem avaliações melhores?</b></br>
Da mesma forma usaremos a mediana para dividir as amostras em dois grupos, por açúcar residual, e encontrar a classificação média de qualidade de cada grupo.</p>
<p>Usaremos métodos como <code>query()</code> para fixar padrões e obter os resultados para as questões propostas acima</p>
<p>Para acessar os métodos utilizados <b><a href="https://github.com/sergioseo/Wine_quality/blob/master/continuando_parte_4.ipynb">clique aqui</a></b></p>

### Conclusão da Parte 4
<p>Desta forma podemos concluir que os vinhos com maior teor alcoólico e os vinhos mais doces geralmente recebem melhores avaliações.</p>

## Parte 5: Data Visualization
<p>Agora que temos a maioria das nossas conclusões feitas sobre a qualidade do vinho e suas propriedas, poderemos utilizar de bibliotecas como <code>seaborn</code> e <code>matplotlib</code> para plotar gráficos e diagramas que irão nos auxiliar a entender melhor o comportamento de algumas variáveis e a fechar as conclusões restantes</p>

### Plotando algumas questões acima
<p>Iremos usar o data visualization para plotar algumas querys que foram mencionadas acima para podermos ter um melhor entendimento das conclusões usando <code>matplotlib</code>, tais como:</br>
<ul>
  <li><b>Será que vinhos com maior teor alcóolico recebem avaliações melhores?</b></li>
  <li><b>Vinhos mais suaves recebem avaliações melhores?</b></li>
  <li><b>Qual o nível de acidez que recebe a maior avaliação média?</b></li>
</ul>
<p>Para acessar os métodos utilizados <b><a href="https://github.com/sergioseo/Wine_quality/blob/master/visualizacoes_vinhos.ipynb">clique aqui</a></b></p>

### Plotando gráficos com tipo de vinho e qualidade
<p>Iremos colocar os dois tipos de vinhos em um mesmo gráfico usando <code>matplotlib</code> para podermos ver em detalhes a proporção de cada um de acordo com sua qualidade</p>
<p>Para acessar os métodos utilizados <b><a href="https://github.com/sergioseo/Wine_quality/blob/master/plotando_tipo_qualidade.ipynb">clique aqui</a></b></p>
