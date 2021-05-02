## Exercício – Inserir os dados das amostras lunares no Visual Studio Code

Agora é hora de inserir dados do Catálogo de fotos e amostras lunares [https://curator.jsc.nasa.gov/lunar/samplecatalog/index.cfm] no Visual Studio Code. Fazendo isso, você pode usar o Python para obter rapidamente insights de milhares de amostras coletadas nas seis missões Apollo que aterrissaram na Lua.

## Configurar o ambiente local

Este módulo mostrará a você como limpar e manipular dados relacionados a amostras de rochas lunares. Para fazer isso, você precisa configurar algum tipo de ambiente de desenvolvimento de notebook do Python. Se você ainda não fez isso, recomendamos seguir as etapas apresentadas aqui. A maneira mais fácil de fazer isso é instalando o pacote de codificação do Visual Studio Code para Python [https://aka.ms/LearnOnVSCode]. Como alternativa, você pode acompanhar os documentos de configuração de ciência de dados do Visual Studio Code [https://code.visualstudio.com/docs/python/data-science-tutorial].

Os três itens que você precisa configurar primeiro são:

* Visual Studio Code
* Python
* Miniconda

Após instalar os três itens listados anteriormente, siga estas etapas para preparar o ambiente:

1. Crie uma pasta chamada over-the-moon.
2. Abra a pasta no Visual Studio Code.
3. Dentro dessa pasta, crie uma pasta chamada sample-return.
4. Dentro dessa pasta, crie uma pasta chamada data.
5. Crie um arquivo chamado sample-return.ipynb na pasta sample-return.
6. Abra o arquivo sample-return.ipynb no Visual Studio Code.

O ambiente deve ser semelhante a este:

https://docs.microsoft.com/pt-br/learn/modules/plan-moon-mission-using-python-pandas/media/vscode-sample-return.png

## Coletar e importar dados

Os dados que você vai explorar neste módulo estão em um arquivo completo com todas as amostras coletadas nas seis missões Apollo que aterrissaram na Lua. O arquivo rocksamples.csv [https://aka.ms/LearnWithDrG/OverTheMoon/Data2] foi criado usando informações do Catálogo de fotos e amostras lunares [https://curator.jsc.nasa.gov/lunar/samplecatalog/index.cfm].

Baixe o arquivo rocksamples.csv [https://aka.ms/LearnWithDrG/OverTheMoon/Data2] e salve-o em sua pasta de dados.

| **Dica**
| 
| Para baixar um arquivo CSV no GitHub:
| 
|   1. Na lista de arquivos no repositório GitHub, selecione o arquivo.
|   2. No canto superior direito, selecione Bruto. O arquivo é aberto como um arquivo CSV bruto em seu navegador.
|   3. Clique com o botão direito do mouse em qualquer lugar na janela do navegador e selecione Salvar como.
|   4. Na caixa de diálogo Salvar arquivo como, escolha o nome do arquivo (rocksamples), o tipo de arquivo (CSV) e o local de download (a pasta de dados do projeto).

Depois que você baixar o arquivo CSV e salvá-lo na pasta de dados, o ambiente do Visual Studio Code terá esta aparência:

https://docs.microsoft.com/pt-br/learn/modules/plan-moon-mission-using-python-pandas/media/vscode-data.png

Na primeira célula do Python no arquivo sample-return.ipynb, importe o pandas e leia o arquivo de dados nele como um DataFrame do pandas:

'''
import pandas as pd 

rock_samples = pd.read_csv('data/rocksamples.csv') 
'''

Para verificar se tudo foi carregado corretamente, imprima as cinco primeiras linhas do novo DataFrame usando **head()** e o resumo das informações usando **info()**:

'''
rock_samples.head()

rock_samples.info()
'''

**Observação**

A saída está truncada para incluir apenas o resumo em tabela dos dados.

Nessa saída, podemos ver que 2.229 amostras foram coletadas pelas missões Apollo. Examinando uma amostra dos dados, podemos ver que cada linha contém:

* ID – a ID exclusiva usada para acompanhamento da amostra na NASA.
* Mission – a missão responsável por recuperar a amostra.
* Type – o tipo de amostra (tipo de rocha ou outra classificação).
* Subtype – uma classificação de tipo mais específica.
* Peso (g): o peso original da amostra, em gramas.
* Integridade (%): o percentual restante da amostra (algumas amostras são usadas durante as pesquisas).
