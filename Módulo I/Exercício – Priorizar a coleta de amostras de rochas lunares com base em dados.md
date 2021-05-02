## Exercício – Priorizar a coleta de amostras de rochas lunares com base em dados

Determinar quais tipos de amostras devem ser coletados da Lua requer experiência, mas podemos começar a fazer algumas suposições para aprender a limpar e manipular os dados.

Primeiro, podemos determinar quanto resta de cada amostra que foi trazida das missões Apollo, considerando a quantidade que foi coletada originalmente e o percentual da amostra íntegra restante.

'''
rock_samples['Remaining (kg)'] = rock_samples['Weight (kg)'] * (rock_samples['Pristine (%)'] * .01)
rock_samples.head()
'''

**Observação**

Multiplique a coluna Integridade (%) por 0,01, pois ela estava sendo representada como um número inteiro.

Examinar **head()** ou **info()** do DataFrame **rock_samples** não é útil neste momento. Com mais de 2.000 amostras, é difícil entender o que são os valores. Para isso, você pode usar a função **describe()**:

'''
rock_samples.describe()
'''

Isso nos ajuda a ver que, em média, cada amostra pesa cerca de 0,16 kg e tem cerca de 84% da quantidade original restante. Podemos usar esse conhecimento para extrair apenas as amostras que estão próximas de acabar, o que significa que foram usadas muito por pesquisadores.

'''
low_samples = rock_samples.loc[(rock_samples['Weight (kg)'] >= .16) & (rock_samples['Pristine (%)'] <= 50)]
low_samples.head()
'''

'''
low_samples.info()
'''

Vinte e sete amostras parece ser uma quantia pequena na qual basear uma recomendação. Provavelmente, podemos encontrar outras amostras necessárias para mais pesquisas aqui na Terra. Para descobri-las, podemos usar a função **unique()** para ver quantos tipos exclusivos temos nos DataFrames **low_samples** e **rock_samples**.

'''
low_samples.Type.unique()
'''

'''
rock_samples.Type.unique()
'''

Podemos ver que, embora seis tipos exclusivos tenham sido coletados entre todas as amostras, as amostras que estão quase acabando são de apenas quatro tipos exclusivos. Mas isso não nos diz tudo sobre as amostras nas quais talvez queiramos nos concentrar. Por exemplo, no DataFrame **low_samples**, quantos de cada tipo são considerados pouca quantidade?

'''
low_samples.groupby('Type')['Weight (kg)'].count()
'''

**Observação**

Aqui, estamos usando a coluna Peso (kg) para contar o número de linhas para cada tipo segundo o qual agrupamos. O peso real não tem impacto.

Observe que há mais rochas dos tipos Basalto e Brecha com poucas amostras do que aquelas de Núcleo e Solo. Além disso, como é alta a probabilidade de que cada missão tenha alguns requisitos de coleta de Núcleo e de Solo, podemos nos concentrar nos tipos de rocha Basalto e Brecha para as amostras que precisamos que sejam coletadas:

'''
needed_samples = low_samples[low_samples['Type'].isin(['Basalt', 'Breccia'])]
needed_samples.info()
'''

Mas Basalto e Brecha são os dois únicos tipos de rocha que queremos procurar?


