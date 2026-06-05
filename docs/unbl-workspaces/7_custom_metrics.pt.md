# Adicionar as Suas Próprias Métricas Personalizadas ao Seu Espaço de Trabalho

**Esta tradução foi gerada por IA e pode conter erros.**

A plataforma pública do UNBL atualmente oferece onze métricas dinâmicas por padrão (consulte [«Quais métricas dinâmicas estão disponíveis para o meu país?»](../unbl-public-platform/8_dynamic_metrics1.md)).

Os espaços de trabalho do UNBL fornecem aos usuários a capacidade de configurar suas próprias métricas personalizadas para realizar cálculos em tempo real e exibir estatísticas zonais para suas próprias áreas de interesse, derivadas de suas próprias camadas de dados geoespaciais carregadas.

A configuração de uma métrica personalizada em um espaço de trabalho segue 5 etapas. Cada etapa é descrita em detalhes nesta seção.

## Etapa 1: Carregar um lugar

As métricas são exibidas no UNBL por meio da seleção de um lugar específico que define a área de interesse para a qual as estatísticas zonais são calculadas. Portanto, é necessário carregar um lugar em seu espaço de trabalho para o qual você deseja visualizar a métrica personalizada. Para obter etapas detalhadas sobre como carregar um lugar em seu espaço de trabalho, consulte [«Como adiciono lugares?»](5_add_places.md).

## Etapa 2: Carregar uma camada raster no formato GeoTIFF

Para criar uma métrica personalizada, é necessário carregar uma camada raster para a qual você deseja visualizar estatísticas zonais. Os espaços de trabalho do UNBL suportam apenas cálculos de métricas usando camadas carregadas no espaço de trabalho por meio da opção «Upload de arquivo GeoTIFF». Para obter etapas detalhadas sobre como carregar uma camada usando essa opção, consulte [«Como faço upload de camadas raster no formato GeoTIFF?»](6_add_data.md/#how-do-i-upload-raster-layers-in-geotiff-format).

Para que uma métrica personalizada funcione corretamente no UNBL, é necessário observar os seguintes pré-requisitos técnicos para as camadas GeoTIFF carregadas:

- Os GeoTIFFs podem representar qualquer forma de dados contínuos ou categóricos, mas os valores de pixel devem ser do tipo inteiro ou float;

- Recomenda-se que para dados categóricos, os GeoTIFFs armazenem no máximo 25 classes inteiras discretas; um número maior de classes prejudica a legibilidade dos gráficos de métricas;

- As estatísticas zonais para sua métrica personalizada só podem ser calculadas para camadas GeoTIFF que tenham cobertura de dados no lugar que você carregou. Se você selecionar um lugar na visualização do mapa UNBL cuja extensão espacial não se sobrepõe de forma alguma à extensão espacial da camada GeoTIFF carregada para a qual você configurou a métrica personalizada, a métrica retornará um gráfico de dados vazio.

- É possível configurar uma métrica de série temporal que mostre mudanças ao longo do tempo para várias camadas GeoTIFF; nesses casos, todas as camadas GeoTIFF devem ter os mesmos valores de atributos, como intervalos de valores mín./máx. para dados contínuos, ou definições de categorias (ou seja, a configuração da legenda) para dados categóricos.

Os usuários que desejam configurar métricas personalizadas para dados de polígonos vetoriais devem primeiro convertê-los para o formato raster GeoTIFF usando técnicas de rasterização. Exemplos de técnicas de rasterização podem ser encontrados na documentação online do [QGIS](https://docs.qgis.org/3.44/en/docs/user_manual/processing_algs/gdal/vectorconversion.html#rasterize-vector-to-raster), [PyGIS](https://pygis.io/docs/e_raster_rasterize.html) e [rdrr.io](https://rdrr.io/cran/terra/man/rasterize.html). Para minimizar erros nos cálculos de métricas que podem ser introduzidos pelo processo de rasterização de dados vetoriais, considere:

- Se os dados vetoriais contiverem apenas nomes de atributos de texto, um novo campo inteiro deve ser adicionado antes da rasterização e um número único deve ser atribuído a cada classe para dados categóricos; esse campo inteiro deve ser usado para atribuir valores de pixel durante a rasterização;

- Converter dados vetoriais para um sistema de referência de coordenadas projetadas consistente, como WGS84 (EPSG: 4326), antes da rasterização;

- Polígonos sobrepostos devem ser resolvidos por prioridade de gravação de pixels durante a rasterização — por exemplo, abordagens de «último pintado» ou «ganha o valor mais alto»;

- Escolher uma resolução raster adequada que considere as compensações entre minimizar erros de borda na conversão de limites de polígonos em pixels e minimizar o tamanho do arquivo (as camadas carregadas em seu espaço de trabalho UNBL usando a opção «Upload de arquivo GeoTIFF» não podem ser maiores que 1000 MB).

## Etapa 3: Criar uma métrica

Criar uma métrica envolve escolher quais camadas GeoTIFF no espaço de trabalho devem ser usadas para calcular estatísticas zonais. Para criar uma métrica:

1.	Clique no botão «Home» na página de administração do seu espaço de trabalho para expandir o menu suspenso. Selecione «Metrics».

2.	Clique no botão «CREATE NEW METRIC» que aparece.

	![](images/en/image1-metrics.png)

3.	Na página de nova métrica, preencha as seguintes informações:

	a) *Title*: O nome da sua métrica. Deve descrever o que o conjunto de dados para o qual você está configurando a métrica mostra. Pode ser o mesmo que o da camada GeoTIFF carregada.

	b) *Metric slug*: Um slug é um identificador único para a métrica dentro do seu espaço de trabalho. Você não pode ter várias métricas dentro do seu espaço de trabalho com o mesmo slug. Deve conter apenas letras, dígitos e hifens («-»). Você pode usar o botão «GENERATE SLUG NAME» para gerar um identificador único com base no título de métrica fornecido.

	c) *Metric data source(s)*: Escolha uma camada GeoTIFF carregada em seu espaço de trabalho na lista suspensa. Para métricas personalizadas que mostram mudanças ao longo do tempo, você tem a opção de selecionar várias camadas GeoTIFF usando o botão «ADD ADDITIONAL DATA SOURCE». Selecione esta opção apenas se você tiver uma série de camadas GeoTIFF com um esquema de atributos consistente, como intervalos de valores mín./máx. para dados contínuos, ou definições de categorias (ou seja, uma configuração de legenda) para dados categóricos.

	d) *Histogram bins*: Esta opção aparece como um campo obrigatório se você selecionou uma camada GeoTIFF com uma categoria de dados contínuos. A métrica usará um histograma para calcular estatísticas zonais para camadas de dados contínuos. Portanto, é necessário especificar o número de intervalos que o histograma calculado para a camada de dados contínuos terá. Os intervalos também são conhecidos como classes. Eles dividem o intervalo de dados numéricos armazenados na camada GeoTIFF em grupos de largura igual. Escolha um número que crie um número adequado de intervalos de dados para o intervalo e a dispersão dos seus dados. Na maioria dos casos, entre 5 e 20 intervalos é ideal, mas depende do intervalo de dados específico.

	e) *Calculate for place types (optional)*: Você pode opcionalmente selecionar o tipo de lugar para o qual sua métrica personalizada deve exibir estatísticas. Isso é útil, por exemplo, para uma métrica que mostra a eutrofização costeira que cobre apenas lugares na categoria marinha. No entanto, se a camada de dados para a métrica não foi projetada para ser confinada a uma área específica, este campo deve ser deixado vazio.

	f)	Depois que todos os parâmetros forem especificados, o botão «SAVE AND VIEW DETAILS» ficará azul, desde que todas as informações inseridas sejam válidas. Clique neste botão para configurar a métrica no seu espaço de trabalho.

	![](images/en/image2-metrics.png)

4. Na página de edição de métrica que aparece, ative o botão «Published» para publicar a métrica.

## Etapa 4: Criar um widget

Depois que a métrica personalizada foi configurada, é necessário criar um widget para a métrica configurada. O widget configura como os dados da métrica serão visualizados e quais informações serão exibidas na visualização do mapa UNBL. Para criar um widget:

1.	Clique no botão «Home» na página de administração do seu espaço de trabalho para expandir o menu suspenso. Selecione «Widgets».

2.	Clique no botão «CREATE NEW WIDGET» que aparece.

	![](images/en/image3-metrics.png)

3. Na página de novo widget, preencha as seguintes informações:

	a) *Title*: Idealmente, o nome do widget deve ser o mesmo que o nome da métrica configurada na Etapa 3. Isso associa claramente o widget à sua métrica.

	b) *Widget slug*: Um slug é um identificador único para o widget dentro do seu espaço de trabalho. Deve conter apenas letras, dígitos e hifens («-»). Você pode usar o botão «GENERATE SLUG NAME» para gerar um identificador único com base no título de métrica fornecido. Idealmente, o slug do widget deve corresponder ao slug da métrica da Etapa 3.

	c) *Description (optional)*: Crie uma descrição curta para o seu widget. Deve ser uma descrição geral dos dados que sua métrica está mostrando no UNBL. Este campo é opcional.

	d) *Metric*: Escolha a métrica criada na etapa 3 para ser associada ao widget.

	e) *Widget Layer(s)*: Este campo especifica a camada de dados que pode ser visualizada na visualização do mapa UNBL junto com a métrica. É preenchido automaticamente com a camada GeoTIFF associada à sua métrica escolhida. É possível escolher camadas adicionais para inclusão no menu suspenso. No entanto, isso não é recomendado, a menos que existam camadas adicionais no espaço de trabalho que não são usadas para calcular a métrica, mas ainda são úteis para adicionar informações geoespaciais contextuais.

	f) *Widget Chart*: Determina qual tipo de gráfico será usado para visualizar as estatísticas de métricas para o seu lugar. A tabela abaixo fornece um resumo dos tipos de gráficos disponíveis com base no tipo de widget, que é detectado automaticamente com base em a) se a camada GeoTIFF mostra dados categóricos (classes discretas) ou dados contínuos (intervalo de valores numéricos), e b) se uma única camada ou várias camadas (métrica de série temporal) foram escolhidas para a métrica.

	<div style="display: flex; justify-content: center;">
	<table style="border-collapse: collapse;">
    <thead>
      <tr>
        <th style="min-width: 200px; border: 1px solid #ccc;"></th>
        <th style="text-align: center; font-size: 1.0rem; white-space: nowrap; border: 1px solid #ccc;"><strong>Dados contínuos</strong></th>
        <th style="text-align: center; font-size: 1.0rem; white-space: nowrap; border: 1px solid #ccc;"><strong>Dados categóricos</strong></th>
      </tr>
    </thead>
    <tbody>
      <tr>
        <td style="font-size: 1.0rem; border: 1px solid #ccc;"><strong>Camada única</strong></td>
        <td style="text-align: center; border: 1px solid #ccc;">Histograma<br>![](images/en/image4-metrics.png){ width="350" style="display:inline-block"}</td>
        <td style="text-align: center; border: 1px solid #ccc;">Gráfico de pizza<br>![](images/en/image5-metrics.png){ width="250" style="display:inline-block"}<br>Gráfico de barras<br>![](images/en/image6-metrics.png){ width="250" style="display:inline-block"}</td>
      </tr>
      <tr>
        <td style="font-size: 1.0rem; border: 1px solid #ccc;"><strong>Série temporal</strong></td>
        <td style="text-align: center; border: 1px solid #ccc;">Gráfico de linhas<br>![](images/en/image7-metrics.png){ width="250" style="display:inline-block"}<br>Gráfico de área<br>![](images/en/image8-metrics.png){ width="250" style="display:inline-block"}</td>
        <td style="text-align: center; border: 1px solid #ccc;">Gráfico de área<br>![](images/en/image9-metrics.png){ width="350" style="display:inline-block"}</td>
      </tr>
    </tbody>
	</table>
	</div>

	| Tipo de gráfico do widget | Exibição de dados |
	| :----------: | :----------: |
	| Histograma | Separa o intervalo numérico de dados em intervalos de largura igual, chamados de classes. O número de classes exibidas corresponde ao número de classes configuradas na página de métrica no back-end. O eixo X exibe a variável medida nos dados, e o eixo Y exibe a área medida em km<sup>2</sup> da área de interesse.
	| Gráfico de área | Para dados contínuos, também separa o intervalo numérico em classes correspondentes ao número de classes configuradas na página de métrica no back-end. Para dados categóricos, classes distintas são usadas. O eixo X exibe o tempo, e o eixo Y exibe a porcentagem da área total da área de interesse.
	| Gráfico de pizza | Mede a cobertura proporcional de cada classe categórica na área de interesse e exibe classes em setores que somam 100%.
	| Gráfico de barras | Exibe cada classe categórica como uma barra, com o eixo X exibindo os dados medidos e o eixo Y exibindo a área medida em km<sup>2</sup> da área de interesse.
	| Gráfico de linhas | Plota a variação na média entre diferentes conjuntos de dados contínuos em uma métrica de série temporal. O eixo X exibe o tempo, e o eixo Y exibe o valor médio medido de cada conjunto de dados.

	f)i)	*Widget summary (optional)*: Cria um resumo das principais estatísticas de métricas que serão exibidas pelo widget. A lista de campos de resumo disponíveis fornece parâmetros que podem ser usados dentro do texto do resumo. Um exemplo de resumo de widget para uma métrica de série temporal usando três camadas mostrando o Índice de Modificação Humana em três períodos de tempo é mostrado abaixo.

	![](images/en/image10-metrics.png)

	Quando esta métrica e seu widget associado estão ativos no UNBL, os campos de resumo no texto são automaticamente preenchidos com os parâmetros necessários, como visto abaixo.

	![](images/en/image11-metrics.png){style="width:550px !important" }

	Alternativamente, o resumo do widget para uma métrica de camada única do Índice de Modificação Humana empregaria os seguintes campos de resumo:

	- {location}: o nome do lugar atual
	- {areaKm2}: a área mapeada em quilômetros quadrados
	- {mean}: o valor médio como número bruto

	O resumo do widget portanto seria algo como:

		"In {location}, {areaKm2} square kilometers had a mean HMI score of {mean} in 2022."

	![](images/en/image12-metrics.png){style="width:550px !important" }

	f)ii) Para métricas criadas usando uma camada categórica, uma opção de alternância adicional está disponível chamada *Use layer categories*.

	A opção está ativada por padrão e especifica que o gráfico de widget categórico deve usar as mesmas categorias de camada que as configuradas na camada raster associada no UNBL. Caso os usuários desejem usar categorias de gráfico de widget diferentes das especificadas na camada raster associada, eles devem desativar a opção *Use layer categories*. Isso solicitará ao usuário que especifique uma lista exaustiva de categorias a serem usadas no gráfico, com cada categoria exigindo que os seguintes parâmetros sejam preenchidos:

	- *Label*: O nome da categoria

	- *Colour*: Um seletor de cor para escolher a cor que deve ser usada para exibir a categoria associada no gráfico do widget

	- *Values*: Número(s) único(s) na camada de dados subjacente que denotam a categoria configurada. O usuário pode especificar mais de um número para a mesma categoria.

	![](images/en/image12_5-metrics.png)

	g)	*X-Axis Label (optional)*: Para tipos de métricas com histograma, gráfico de linhas ou gráfico de área, é possível especificar um rótulo para o eixo X. O rótulo deve ser o da variável de dados sendo mostrada. Este campo é opcional.

	h)	*X-Axis Unit (optional)*: Para tipos de métricas com histograma, gráfico de linhas ou gráfico de área, é possível especificar as unidades da variável de dados. Este campo é opcional.

	i)	Depois que todos os parâmetros forem especificados, o botão «SAVE AND VIEW DETAILS» ficará azul, desde que todas as informações inseridas sejam válidas. Clique neste botão para criar o widget.

	![](images/en/image13-metrics.png)

4. Na página de edição de widget que aparece, ative o botão «Published» para publicar o widget.

## Etapa 5: Criar um painel de controle

Um painel de controle atua como a interface do usuário que exibe a métrica e o widget associado na visualização do mapa UNBL ao selecionar um lugar para visualizar métricas. É importante observar que os usuários podem criar quantas métricas e widgets quiserem, mas todos podem ser colocados dentro do mesmo painel de controle. Portanto, é necessário criar apenas um painel de controle para todas as métricas que um usuário deseja configurar. Se o seu espaço de trabalho ainda não contém um painel de controle, siga estas etapas para criar um:

1.	Clique no botão «Home» na página de administração do seu espaço de trabalho para expandir o menu suspenso. Selecione «Dashboards».

2.	Clique no botão «CREATE NEW DASHBOARD» que aparece.

	![](images/en/image14-metrics.png)

3. Na página de novo painel de controle, preencha as seguintes informações:

	a)	*Title*: O painel de controle deve ter um nome que defina claramente um grupo temático para as métricas personalizadas vinculadas a ele. Por exemplo, um painel de controle pode conter todas as métricas personalizadas definidas por um usuário específico e, portanto, ser chamado de «Custom metrics by *user x*». Alternativamente, um painel de controle pode conter métricas destinadas a serem visualizadas para um lugar específico, por exemplo, «Custom metrics for *country x*».

	b)	*Dashboard slug*: Um slug é um identificador único para o painel de controle dentro do seu espaço de trabalho. Você não pode ter vários painéis de controle dentro do seu espaço de trabalho com o mesmo slug. Deve conter apenas letras, dígitos e hifens («-»). Você pode usar o botão «GENERATE SLUG NAME» para gerar um identificador único com base no título de painel de controle fornecido.

	c)	*Dashboard description*: Você pode fornecer uma breve descrição para o grupo de métricas aqui. Por exemplo, «This dashboard contains custom metrics defined by *user x*.» Este campo é opcional.

	d)	*Included Widgets*: Escolha o(s) widget(s) a incluir neste painel de controle.

	e)	Depois que todos os parâmetros forem especificados, o botão «SAVE AND VIEW DETAILS» ficará azul, desde que todas as informações inseridas sejam válidas. Clique neste botão para criar o painel de controle.

	![](images/en/image15-metrics.png)

4. Na página de edição de painel de controle que aparece, ative o botão «Published» para publicar o painel de controle.

!!!note
	Se você já possui um painel de controle existente e deseja adicionar widgets recém-configurados a ele, você pode editar seu painel de controle existente e adicionar widgets incluídos clicando no ícone de caneta do painel de controle na lista de entradas de painel de controle que aparecem na página de administração após escolher «Dashboards» no menu suspenso.

## Visualização de painéis de controle e widgets

Para visualizar suas métricas personalizadas:

1. Na visualização do mapa UNBL, certifique-se de que seu espaço de trabalho esteja ativado.

	![](images/en/image16-metrics.png){style="width:600px !important" }

2. Selecione o lugar para o qual você deseja visualizar métricas personalizadas na aba «PLACES».

	![](images/en/image17-metrics.png){style="width:400px !important" }

3. Se você tiver a plataforma pública do UNBL e/ou outros espaços de trabalho ativos junto com seu próprio espaço de trabalho, pode ser necessário selecionar o painel de controle que contém suas métricas personalizadas para visualizá-las. Nesse caso, um menu suspenso aparecerá com uma lista de painéis de controle e os espaços de trabalho associados de cada painel de controle. Selecione seu painel de controle no menu suspenso.

	!!!note
		Se as métricas personalizadas no seu painel de controle não se sobrepõem ao lugar ativado, seu painel de controle não aparecerá no menu suspenso.

	![](images/en/image18-metrics.png){style="width:400px !important" }

4. Agora você pode visualizar as métricas personalizadas que configurou para sua camada GeoTIFF e lugar escolhidos. Todas as funcionalidades das métricas dinâmicas padrão do UNBL estão disponíveis para suas métricas personalizadas, incluindo a ativação da camada associada, visualização de informações e download de dados de métricas nos formatos .csv, .tsv e .json.

	![](images/en/image19-metrics.png)
