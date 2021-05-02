## Exercício – Prever a capacidade de transporte de amostras da Artemis

Conforme mencionado na unidade anterior, podemos usar a Ficha de dados da NASA sobre o SLS (Sistema de lançamento espacial) e sobre os módulos Orion [https://www.nasa.gov/sites/default/files/atoms/files/0080_sls_fact_sheet_sept2020_09082020_final_0.pdf] para reunir dados estimados sobre os foguetes e os módulos que serão usados no programa Artemis.

**Como lembrete, o Programa Artemis [https://www.nasa.gov/specials/artemis/] é o segundo conjunto de missões da NASA para levar seres humanos à superfície da Lua. O programa será lançado em 2024 e enviará não apenas o próximo par de seres humanos, mas também a primeira mulher a pisar na Lua.** A preparação para essa missão é ainda maior do que o foco em um pouso lunar. Ela também fornecerá espaço para uma carga comercial na nave e é a primeira etapa do Programa Lua para Marte. Sendo assim, embora as missões do programa Artemis provavelmente tragam amostras adicionais, há outras metas que podem afetar a quantidade de capacidade necessária para fazer isso.

## Criar um DataFrame da missão Artemis

Não temos todos os detalhes sobre a missão do Artemis, mas sabemos que três iterações do foguete serão revezadas para cada missão. Cada foguete terá uma versão destinada a transportar uma tripulação e uma apenas para a carga. Para os fins deste módulo, nos concentraremos apenas nos três foguetes destinados a abrigar a tripulação, para ficarmos mais alinhados às missões Apollo. Também sabemos que a carga esperada do SLS (Sistema de lançamento espacial) deve aumentar a cada iteração, mas que o peso atual do Orion (os módulos de comando e lunar combinados) tem um peso estimado atualmente.

Novamente, chamaremos os módulos lunar e de comando de área tripulada e podemos criar um DataFrame com as informações que temos sobre as três missões tripuladas:

'''
artemis_crewedArea = 26520
artemis_mission = pd.DataFrame({'Mission':['artemis1','artemis1b','artemis2'],
                                 'Total weight (kg)':[artemis_crewedArea,artemis_crewedArea,artemis_crewedArea],
                                 'Payload (kg)':[26988, 37965, 42955]})
artemis_mission
'''

E podemos estimar o peso das amostras com base nas proporções que determinamos usando as missões Artemis:

'''
artemis_mission['Sample weight from total (kg)'] = artemis_mission['Total weight (kg)'] * sample_crewedArea_ratio
artemis_mission['Sample weight from payload (kg)'] = artemis_mission['Payload (kg)'] * sample_payload_ratio
artemis_mission
'''

Por fim, podemos obter a média das duas previsões:

'''
artemis_mission['Estimated sample weight (kg)'] = (artemis_mission['Sample weight from payload (kg)'] + artemis_mission['Sample weight from total (kg)'])/2
artemis_mission
'''

Agora, podemos ver que as três missões Artemis podem, provavelmente, trazer 57,77 kg, 65,65 kg e 69,24 kg, respectivamente.

E a pergunta é, que tipos de rochas eles devem priorizar?