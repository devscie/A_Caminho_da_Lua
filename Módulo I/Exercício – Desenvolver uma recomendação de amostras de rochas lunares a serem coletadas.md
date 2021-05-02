## Exercício – Desenvolver uma recomendação de amostras de rochas lunares a serem coletadas

Vamos voltar um pouco e ver como o número de amostras se compara à quantidade das amostras. Podemos comparar o peso total do DataFrame **needed_samples** com o DataFrame **rock_samples**. Ou seja, vamos comparar as amostras que identificamos como estando quase no fim com todas as amostras coletadas nas missões Apollo.

'''
needed_samples.groupby('Type')['Weight (kg)'].sum()

rock_samples.groupby('Type')['Weight (kg)'].sum()
'''

Uma informação se destaca bastante: nunca chegamos a ter uma grande quantidade de rochas Crustais.

Podemos adicionar as rochas Crustais ao conjunto de amostras necessárias:

'''
needed_samples = needed_samples.append(rock_samples.loc[rock_samples['Type'] == 'Crustal'])
needed_samples.info()
'''

**Resumo das amostras necessárias**

A etapa final é consolidar tudo que sabemos em uma tabela que pode ser compartilhada com o astronautas. Primeiro, precisamos de uma coluna para cada tipo de rocha que já identificamos como rochas das quais desejamos mais amostras:

'''
needed_samples_overview = pd.DataFrame()
needed_samples_overview['Type'] = needed_samples.Type.unique()
needed_samples_overview
'''

Em seguida, queremos o peso total de cada tipo de rocha coletado originalmente:

'''
needed_sample_weights = needed_samples.groupby('Type')['Weight (kg)'].sum().reset_index()
needed_samples_overview = pd.merge(needed_samples_overview, needed_sample_weights, on='Type')
needed_samples_overview.rename(columns={'Weight (kg)':'Total weight (kg)'}, inplace=True)
needed_samples_overview
'''

Quando os astronautas estão na Lua, uma forma de identificar as rochas é pelo tamanho. Se pudermos informar a eles o tamanho estimado de cada tipo de rocha, isso poderá facilitar o processo de coleta.

'''
needed_sample_ave_weights = needed_samples.groupby('Type')['Weight (kg)'].mean().reset_index()
needed_samples_overview = pd.merge(needed_samples_overview, needed_sample_ave_weights, on='Type')
needed_samples_overview.rename(columns={'Weight (kg)':'Average weight (kg)'}, inplace=True)
needed_samples_overview
'''

As rochas crustais são pequenas! Provavelmente, elas são muito mais difíceis de identificar, portanto não é de se admirar que não tenhamos muitas.

Poderíamos indicar aos astronautas o quanto queremos que eles coletem de cada amostra. Portanto, para os três tipos de rochas que estamos procurando, devemos usar o número total de cada tipo e obter o percentual restante de cada tipo de rocha.

'''
total_rock_count = rock_samples.groupby('Type')['ID'].count().reset_index()
needed_samples_overview = pd.merge(needed_samples_overview, total_rock_count, on='Type')
needed_samples_overview.rename(columns={'ID':'Number of samples'}, inplace=True)
total_rocks = needed_samples_overview['Number of samples'].sum()
needed_samples_overview['Percentage of rocks'] = needed_samples_overview['Number of samples'] / total_rocks
needed_samples_overview
'''

E, por fim, para conectar tudo isso a uma recomendação para o programa Artemis, podemos determinar o peso médio das amostras que estimamos na unidade anterior.

'''
artemis_ave_weight = artemis_mission['Estimated sample weight (kg)'].mean()
artemis_ave_weight
'''

Podemos usar esse número para determinar quantas de cada rocha queremos que os astronautas busquem coletar:

'''
needed_samples_overview['Weight to collect'] = needed_samples_overview['Percentage of rocks'] * artemis_ave_weight
needed_samples_overview['Rocks to collect'] = needed_samples_overview['Weight to collect'] / needed_samples_overview['Average weight (kg)']
needed_samples_overview
'''

Portanto, podemos dizer aos astronautas da Artemis para tentarem coletar 13 rochas de Basalto, 35 rochas de Brecha e 21 rochas Crustais. Nossa!