# Análise dos dados da EPA

<p>Os dados de teste usados para determinar as estimativas de economia de combustível são derivados dos testes veiculares feitos no <b><a href="https://www.epa.gov/compliance-and-fuel-economy-data/data-cars-used-testing-fuel-economy">Laboratório Nacional de Veículos e Emissões de Combustível da EPA</a></b> em Ann Arbor, Michigan, e pelos fabricantes de veículos que enviam seus próprios dados de teste à EPA. 
 
<p>Mas afinal o que é <b>economia de combustível?</b></br>
Tirado da página <i>Wikipédia</i> sobre Economia de combustível em automóveis:</p>
<ul>
 <i><b>A economia de combustível de um automóvel e a relação de eficiência de combustível entre a distância percorrida e a quantidade de combustível consumida pelo veículo. O consumo pode ser expresso em termos de volume para percorrer determinada distância ou pela distância percorrida por unidade de volume de combustível consumida.</b></i>
</ul>
<p>Foram colhidas 2 amostras <b>(2008 e 2018)</b> no próprio site da <i>EPA</i> e no formato <code>xlsx</code> e, então, tiveram q passar por limpeza antes de se tornarem um arquivo <code>CSV</code>:</p>
<p>
 <ul>
   <li><a href="https://github.com/sergioseo/epa_analysis/blob/master/data/all_alpha_08.csv"><b>all_alpha_08.csv</b></a></li>    <li><a href="https://github.com/sergioseo/epa_analysis/blob/master/data/all_alpha_18.csv"><b>all_alpha_18.csv</b></a></li>
  </ul>
<p>Os dados estão separados assim:</p> 
<ul>
 <li><b>2008:</b> 2404 rows e 18 columns</li>
 <li><b>2018:</b> 1611 rows e 18 columns</li>
</ul>
<p>Lembando que esse exercício visa uma prévia organização das colunas e a limpeza completa do dataset para no final conseguirmos unir os 2 conjuntos de dados e transformá-lo em apenas 1. Criaremos uma coluna com os anos <code>2008</code> e <code>2018</code> para separar os dados em seus respectivos anos e descartaremos as colunas que não tiver correlação com o estudo. Assim teremos um dataframe limpo e pronto para maiores análises!</p>

## Primeira Etapa
Se já tiver algum dos programas listados abaixo e quiser usá-los, apenas certifique-se de estarem atualizados e instalados corretamente. Para o bom funcionamento e a visualização correta do projeto será necessário o download de alguns arquivos:
<ul>
  <li>Fazer o donwload e instalar o <a href="https://www.anaconda.com/">Anaconda</a></li>
  <li>Fazer o donwload e instalar o <a href="https://www.winzip.com/win/en/downwz.html">WinZip</a>(Version para Win. No Mac há a opção direta sem necessidade de outro software)</li>
  <li>Fazer o download destes dataFrames: <a href="https://github.com/sergioseo/epa_analysis/blob/master/data/all_alpha_08.csv"><b>all_alpha_08.csv</b></a> e <a href="https://github.com/sergioseo/epa_analysis/blob/master/data/all_alpha_18.csv"><b>all_alpha_18.csv</b></a>
  <li>Depois de instalar o <code>Anaconda</code> procure pelo <code>Jupyter Notebook</code> e click em <code>install</code> e depois <code>launch</code></li>
</ul>

### Entendendo os rótulos do dataSet
<p>É importante lermos e entendermos a <i>documentation</i> do projeto antes de começar a de fato explorar o dataSet. Essas informações estão disponíveis neste link: <a href="https://github.com/sergioseo/epa_analysis/blob/master/documentation/GreenVehicleGuide_Documentation.pdf"><b>Green_documentation</b></a> e <a href="https://github.com/sergioseo/epa_analysis/blob/master/documentation/Leia_me_doc.txt"><b>Read_me_doc</b></a>.</p>
<p>Desta forma, de acordo com os docs acima sabemos que os rótulos são esses:</p>
<ol>
 <li><b>Model:</b>	Fabricante e modelo do veículo</li>
 <li><b>Displ:</b>	Deslocamento do motor - o tamanho de um motor em litros</li>
 <li><b>Cyl:</b>	O número de cilindros de um motor específico</li>
 <li><b>Trans:</b> Tipo de transmissão e número de marchas</li>
 <li><b>Drive:</b>	Tipo de eixo de tração (2WD = tração em 2 rodas, 4WD = tração nas 4 rodas)</li>
 <li><b>Fuel:</b> Tipo de combustível</li>
 <li><b>Cert Region*:</b> Código da região de certificação</li>
 <li><b>Área de vendas**:</b> Código da região de certificação</li>
 <li><b>Stnd:</b> Código de normas para emissões de veículos</li>
 <li><b>Stnd Description*:</b> Descrição das normas para emissões de veículos</li>
 <li><b>Underhood ID:</b> Número de identificação de 12 dígitos encontrado na etiqueta de emissão sob o capô de todos os veículos. É uma exigência da EPA designar seu “grupo de teste” ou “família do motor”.</li>
 <li><b>Veh Class:</b> Classe do veículo na EPA</li>
 <li><b>Air Pollution Score:</b> Pontuação de poluição do ar (classificação de emissão)</li>
 <li><b>City MPG:</b> Mpg estimado na cidade (milhas/galão)</li>
 <li><b>Hwy MPG:</b> Mpg estimado na estrada (milhas/galão)</li>
 <li><b>Cmb MPG:</b> Mpg combinado estimado (milhas/galão)</li>
 <li><b>Greenhouse Gas Score:</b> Classificação de emissão de gases do efeito estufa</li>
 <li><b>SmartWay:</b> Sim, Não ou Elite</li>
 <li><b>Comb CO2*:</b> Emissões de CO2 cidade/estrada combinados em gramas por milha</li>
</ol>
<p>
 <ul><b>
  * Não incluídos no conjunto de dados de 2008</br>
  ** Não incluídos no conjunto de dados de 2018</b>
 </ul>
</p>

## Parte 1: Primeiras Avaliações do DataSet
<p>A principio vamos inspecionar os dataSets <code>all_alpha_08.csv</code> e <code>all_alpha_18.csv</code>usando  <code>Pandas</code> e abri-los para verificar os rótulos. É sempre bom nos atentarmos aos rótulos e uma dica q eu sempre dou é usar um editor simples (ATOM, SUBLIME, VSCODE, etc) para dar uma olhada e verificar se o dataSet possui rótulos e cabeçalho.
<p>Passaremos para os seguintes passos:</p>
<ol>
  <li>Verificar o número de amostras em cada conjunto de dados</li>
  <li>Verificar o número de colunas em cada conjunto de dados</li>
  <li>Verificar se valores faltantes nas columns (missings)</li>
  <li>Verificar as linhas duplicadas em cada conjunto de dados</li>
  <li>Verificar o tipo de cada rótulo</li>
  <li>Verificar os números dos valores únicos de cada conjunto de dados</li>
  <li>Verificar os valores únicos da column "Fuel" para saber os tipos de combustível</li>
</ol>

### Conclusão da Parte 1
<p>Ao abrir os dataSets podemos perceber que:</p>
<ul>
  <li>Existem valores nulos em ambos os conjuntos e depois teremos que escolher entre dropar, preencher, usar uma média geral ou apenas compilar com o número anterior/posterior para substituição.</li>
  <li>O conjuntos de dado de 2008 tem 25 linhas duplicadas e mais pra frente (dependendo da situação) teremos que examinar com cautela para entender se há a possibilidade de apenas descartá-las ou não</li>
 <li>Há valores diferentes na coluna <code>Fuel</code> em ambos os conjuntos. Depois teremos que entender como trabalhar com esses diferentes valores e se irá afetar nossa análise.</li>
 <li> A coluna <code>Cyl.</code> apresenta tipo e formato diferente entre os conjuntos de dados. Se quisermos trabalhar com essa coluna teremos que padronizá-la para não gerar erro no futuro</li>
</ul>
<p>Para acessar os métodos utilizados <b><a href="https://github.com/sergioseo/epa_analysis/blob/master/parte_1_avaliando_e_limpando.ipynb">clique aqui</a></b>  
</p>

## Parte 2: Limpando e Renomeando o dataSet
<p>Ao verificar que o dataSet possui coluas que não apresentam valores consistentes (não estão presentes em ambos os conjuntos de dados) ou não são relevantes para as perguntas a primeira coisa a fazer é dropar essas colunas usando <code>drop()</code>. Colunas a descartar:</br>
<p>
 <ul>
  <li><b>Do conjunto de dados de 2008:</b> 'Stnd', 'Underhood ID', 'FE Calc Appr', 'Unadj Cmb MPG'</li>
  <li><b>Do conjunto de dados de 2018:</b> 'Stnd', 'Stnd Description', 'Underhood ID', 'Comb CO2'</li>
 </ul>
</p>
<p>Também teremos que trocar o rótulo da coluna <code>Sales Area</code> do conjunto de 2018 para <code>Cert Region</code> para manter a consistência dos valores e continuar renomeando todos os rótulos de coluna para substituir espaços por sublinhados e colocar tudo em letra minúscula. Os sublinhados são muitos mais fáceis de se trabalhar no Python do que espaços.</br>

Por exemplo, quando se tem espaços, não é possível usar <code>df.column_name</code> em vez de <code>df['column_name']</code> para selecionar colunas ou usar o método <code>query()</code>. Ser consistente com letras minúsculas e sublinhados também facilita na hora de lembrar o nome das colunas.</p>

<p>Para acessar os métodos utilizados <b><a href="https://github.com/sergioseo/epa_analysis/blob/master/parte_2/parte_2_limpando_e_renomeando.ipynb">clique aqui</a></b>  

## Parte 3: Filtrar, Remover Valores Nulos e Duplicados
<p><b>Filtrar</b></br>
Para manter a consistência só compare carros certificados segundo as normas da Califórnia. Vamos filtrar os dois conjuntos usando <code>query()</code> para selecionar somente linhas em que <code>cert_region</code> é <code>CA</code>. Em seguida vamos descartar as colunas <code>cert_region</code> já que não vão fornecer mais nenhuma informação útil (para termos certeza de que todos os valores são <code>CA</code>).</p>

<p><b>Descartar nulos</b></br>
Vamos descartar todas as linhas dos dois conjuntos de dados que contêm valores ausentes.</p>

<p><b>Remover duplicados</b></br>
Vamos descartar todas as linhas duplicadas nos dois conjuntos.</p>

<p>Para acessar os métodos utilizados <b><a href="https://github.com/sergioseo/epa_analysis/blob/master/parte_3/parte_3_filtrar_%20remover_valores_nulos_duplicados.ipynb">clique aqui</a></b>  

## Parte 4: Inspecionando Tipo de Dados 
<p>Nesta etapa iremos apenas inspecionar algumas colunas usando o método <code>type()</code> para descobrir quais as colunas tem um tipo diferente e posteriormente iremos alinhá-las de acordo com o seu rótulo.</p>

<p>Para acessar os métodos utilizados nesta etapa <b><a href="https://github.com/sergioseo/epa_analysis/blob/master/parte_4/parte_4_Insp_tipos_dados.ipynb">clique aqui</a></b>  
</p>

## Parte 5.1: Corrigindo tipo de dados
<p>Vamos começar trabalhando os dados da coluna <code>cyl</code> do dataFrame de 2018 extraindo <code>int</code> de <code>string</code> e convertendo de <code>float</code> para <code>int</code></p>

<p>Para acessar os métodos utilizados <b><a href="https://github.com/sergioseo/epa_analysis/blob/master/parte_5.1/parte_5.1_corrigindo_dados.ipynb">clique aqui</a></b>  
</p>

## Parte 5.2: Corrigindo tipo de dados 2
<p>Neste caso teremos um pouco mais de dificuldade pois teremos que primeiro separar as colunas que possuem 2 valores em cada coluna separasas por <code>/</code> para depois converte-las como no passo anterior.</p>

<p>Para acessar os métodos utilizados <b><a href="https://github.com/sergioseo/epa_analysis/blob/master/parte_5.2/parte_5.2_corrigindo_tipo_de_dados_2.ipynb">clique aqui</a></b>  
</p>

## Parte 5.3: Corrigindo tipo de dados 3
<p>Seguiremos corrigindo as colunas restantes como feito na parte 5.1</p>

<p>Para acessar os métodos utilizados <b><a href="https://github.com/sergioseo/epa_analysis/blob/master/parte%205.3/parte_5.3_corrigindo_tipo_de_dados_3.ipynb">clique aqui</a></b>  
</p>

## Parte 6: Plotando os gráficos
<p>Agora que temos todas as colunas padronizadas e convertidas poderemos utilizar de bibliotecas e ferramentas como  <code>seaborn</code> e <code>matplotlib</code> para plotar gráficos e diagramas que irão nos auxiliar a entender melhor o comportamento de algumas variáveis e a fechar as conclusões restantes</p>

<p>Para acessar os métodos utilizados <b><a href="https://github.com/sergioseo/epa_analysis/blob/master/parte%206/parte_6_plotando_os_graficos.ipynb">clique aqui</a></b></p>

<p>Pronto! Agora estamos com os histogramas iniciais gerados e a limpeza dos dados terminada. Dessa forma poderemos iniciar nossa segunda etapa: Análise dos dados usando <code>Pandas</code>, <code>Numpy</code> e <code>matpoltlib</code>.</p>
