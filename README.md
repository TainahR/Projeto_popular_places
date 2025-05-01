Projeto de pipeline de dados que utiliza Python para acessar a API do Google Maps, realizar a extração e transformação dos dados, e armazená-los. Os resultados da análise sobre a movimentação das academias em diferentes horários do dia são então visualizados no Power BI.
Cansada de tentar treinar em horários em que a academia está sempre lotada, decidi criar uma solução para encontrar a academia mais vazia no horário em que eu posso ir.

Para isso, comecei pesquisando sobre a API do Google Maps, pois ela disponibiliza dados de "tempos de pico" para diversos estabelecimentos. Durante a pesquisa, encontrei a biblioteca Python Popular Times, que facilita esse processo: basta fornecer os place IDs das academias e ela retorna diretamente as informações de lotação por dia e horário.

Coleta de Dados
O primeiro passo do projeto foi criar uma função capaz de consultar a API do Google Maps para retornar todos os place IDs de academias com base em uma query de busca. Essa query é passada como parâmetro da função.

Com os place IDs em mãos, utilizei a biblioteca Popular Times, que permite acessar dados públicos de popularidade por dia e horário a partir desses IDs. Dessa forma, consegui automatizar a coleta das informações que indicam os períodos de maior e menor movimento em cada academia da região.

A função desenvolvida ficou assim:

Conteúdo do artigo
Exemplo de chamada da função:

dados = get_popularidade('academias Mococa', keyAPI)
Transformação dos Dados
O segundo passo foi transformar os dados coletados — inicialmente em formato de dicionário — em uma estrutura mais adequada para análise no Power BI. Para isso, utilizei o pandas para organizar as informações em um DataFrame e salvei em parquet.

A estrutura dos dados incluía o nome da academia, o endereço, o dia da semana, a hora e o nível de popularidade naquele horário:

Conteúdo do artigo
Visualização no Power BI
Por fim, no Power BI, criei medidas de agrupamento que permitiram identificar a academia com menor popularidade em cada horário do dia. Para tornar a análise mais realista, desconsiderei horários entre 00:00 e 04:00, já que muitas academias não funcionam nesse período (em uma análise mais aprofundada, o ideal seria extrair e considerar os horários reais de funcionamento de cada estabelecimento).

A visualização foi construída da seguinte forma:

Eixo Y: horários do dia (das 05:00 às 23:00).
Eixo X: valor da menor popularidade (de 0 a 100).
Rótulo de dados: nome da academia.

Além disso, adicionei uma segmentação de dados por dia da semana, permitindo ao usuário filtrar e analisar qual academia tende a estar mais vazia em cada horário e em cada dia:

Conteúdo do artigo
Melhorias Futuras
Como próxima etapa, pretendo automatizar essa pipeline de extração integrando-a a uma DAG no Apache Airflow. Com isso, seria possível manter as análises sempre atualizadas com base nos dados mais recentes
