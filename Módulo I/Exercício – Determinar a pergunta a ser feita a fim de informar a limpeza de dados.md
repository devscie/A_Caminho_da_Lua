## Exercício – Determinar a pergunta a ser feita a fim de informar a limpeza de dados

Antes de continuar, é importante estabelecer quais informações você quer usar para fazer uma recomendação para os astronautas. Nesse caso, a informação mais disponível publicamente que poderia afetar a quantidade de amostras que é possível transportar é o tipo de aeronave usado.

## Converter o peso da amostra

Embora os detalhes do design dos foguetes sejam proprietários, algumas informações estão disponíveis publicamente, como o peso dos módulos (partes do foguete) que transportarão as amostras para a Terra e o peso total que o foguete é capaz de levantar acima da atmosfera.

Vamos entrar em detalhes sobre isso em uma unidade posterior, mas a parte crítica com relação às amostras é entender que o peso do foguete muitas vezes é medido em quilogramas, e não em gramas. Assim, devemos manipular os dados originais convertendo os pesos das amostras em quilogramas para facilitar a análise de dados mais tarde.

'''
rock_samples['Weight (g)'] = rock_samples['Weight (g)'].apply(lambda x : x * 0.001)
rock_samples.rename(columns={'Weight (g)':'Weight (kg)'}, inplace=True)
rock_samples.head()
'''

Aqui, primeiro modificamos os valores da coluna Peso (g) para que tenham o mesmo valor multiplicado por 0,001. Depois, modificamos o nome da coluna para que seja mais preciso, alterando-o para Peso (kg).

## Criar um DataFrame

O pandas, a Biblioteca do Python que estamos usando para fazer nossa análise de dados, tem uma estrutura chamada DataFrame e ela é realmente eficaz para representar dados 2D. Talvez você tenha percebido que, ao executar o código **rock_samples.head()**, o que é impresso parece ser quase um instantâneo de uma planilha do Excel, o que é uma ótima forma de pensar nos DataFrames no pandas.

O DataFrame **rock_samples** tem uma linha para cada amostra coletada, mas, como mencionamos anteriormente, queremos entender as amostras de rochas no total conforme elas se relacionam com os foguetes específicos que as trouxeram.

Crie um DataFrame chamado **missions**, que será um resumo dos dados referentes a cada uma das seis missões Apollo que trouxeram amostras. Nesse DataFrame, crie uma coluna chamada **Missão** que tenha uma linha para cada missão.

'''
missions = pd.DataFrame()
missions['Mission'] = rock_samples['Mission'].unique()
missions.head()
'''

'''
missions.info()
'''

## Somar o peso total das amostras por missão

Agora, você pode adicionar uma nova coluna ao DataFrame **missions** para representar a soma de todas as amostras coletadas nessa missão.

'''
sample_total_weight = rock_samples.groupby('Mission')['Weight (kg)'].sum()
missions = pd.merge(missions, sample_total_weight, on='Mission')
missions.rename(columns={'Weight (kg)':'Sample weight (kg)'}, inplace=True)
missions
'''

Vamos detalhar um pouco esse código. A primeira linha é **sample_total_weight = rock_samples.groupby('Mission')['Weight (kg)'].sum()**, que pode ser dividida da seguinte maneira:

* **rock_samples.groupby('Mission')** – agrupa todas as linhas de acordo com os valores na coluna Missão.
* **rock_samples.groupby('Mission')['Weight (kg)']**: captura todos os valores da coluna Peso (kg), mas agrupa-os de acordo com os valores exclusivos da coluna Missão.
* **rock_samples.groupby('Mission')['Weight (kg)'].sum()**: soma todos os valores da coluna Peso (kg) para cada valor exclusivo da coluna Missão.

Se você fosse imprimir essa linha, teria uma série pandas, que é basicamente um tipo de dados em 1D, ou uma lista. A lista teria como índice o valor exclusivo da coluna Missão em vez de um número:

'''
sample_total_weight = rock_samples.groupby('Mission')['Weight (kg)'].sum()
sample_total_weight
'''

A linha seguinte, **pd.merge(missions, sample_total_weight, on='Mission')**, pode ser descrita como:

Mescle o DataFrame **missions** com a série **sample_total_weight** usando a coluna **Missão** como o índice para mesclagem. O computador fará basicamente isso: para cada valor da coluna **Missões** no DataFrame **missions**, encontrar o mesmo valor na série **sample_total_weight** e adicionar o valor da série à linha como uma nova coluna no DataFrame.

Este exemplo é bastante simples pois há somente seis linhas. Sendo assim, podemos confirmar que o número 21,55424, por exemplo, foi adicionado à linha referente à Apollo 11 no DataFrame **missions**.

A linha seguinte apenas renomeia a coluna, da mesma forma que antes, para garantir que estejamos sendo específicos com nossos dados.

A última linha imprime todo o DataFrame **missions**. Como há apenas seis missões, podemos imprimir todo o DataFrame e ainda entender o que estamos olhando. Não é necessário usar **head()** para imprimir apenas as cinco primeiras linhas.

## Obter a diferença dos pesos entre as missões

Não somos especialistas em foguetes, por isso é importante examinar uma grande quantidade de diferentes seções cruzadas de dados que estão disponíveis. Nesse caso, podemos ver que o peso total das amostras aumentou com cada missão, mas é difícil ver imediatamente o quanto. Podemos adicionar mais uma coluna ao DataFrame **missions** que simplesmente captura a diferença entre a linha atual e a linha que a precede:

'''
missions['Weight diff'] = missions['Sample weight (kg)'].diff()
missions
'''

Observe que na primeira linha, referente à missão Apollo11, o valor na coluna **Diferença de peso** é **NaN**. Isso é chamado de um valor nulo. Como a Apollo11 foi a primeira missão, não há nenhuma diferença entre o peso das rochas coletadas na Apollo11 e na missão anterior. Podemos preencher esse valor de **NaN** com 0:

'''
missions['Weight diff'] = missions['Weight diff'].fillna(value=0)
missions
'''

Este código Python fez o seguinte:

* Examinou apenas a coluna **Diferença de peso** no DataFrame missions
* Preencheu todos os valores "na" (ou nulos) com um determinado valor
* O valor a preencher nos valores na é 0
* Salvou a lista de valores modificados daquela coluna de volta na coluna

Essa última etapa é importante. O pandas é uma biblioteca criada para permitir que exploremos dados, o que significa que algumas das funções fornecem insights sobre os dados, mas não os modificam diretamente. Quando estiver em dúvida, leia a documentação e teste!