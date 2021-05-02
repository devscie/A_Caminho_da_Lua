## Exercício – Entender os dados no DataFrame das missões

Parabéns! Agora você tem uma visão de cada uma das seis missões Apollo que pousaram na Lua. Esta imagem contém informações sobre as amostras que cada missão coletou e os pesos de cada módulo lunar e de comando.

Mas o que elas significam?

## Comparar os dados

O aspecto interessante de prever quantas amostras cada missão Artemis pode trazer é que ainda não conhecemos as especificações completas da nave que a Artemis planeja usar. Usando algumas informações da Ficha de dados da NASA sobre o SLS (Sistema de lançamento espacial) e sobre os módulos Orion [https://www.nasa.gov/sites/default/files/atoms/files/0080_sls_fact_sheet_sept2020_09082020_final_0.pdf], temos dados sobre os pesos e as cargas.

Uma carga é basicamente a quantidade total de peso que um foguete consegue fazer atravessar nossa atmosfera e chegar ao espaço. Sendo assim, a probabilidade de que o número referente à carga seja mais preciso do que os pesos exatos de cada módulo é alta, pois a decisão sobre a carga provavelmente afetará cada uma das outras decisões de design.

Sabemos que a carga do Saturn V era de 43.500 kg e que os pesos dos módulos variaram de missão para missão. Portanto, para determinar as proporções que nos permitirão fazer previsões sobre as missões Artemis, podemos usar:

* Carga do Saturn V
* Peso de amostras da missão
* Peso dos módulos da missão

### Sample-to-weight ratio
'''
saturnVPayload = 43500
missions['Crewed area : Payload'] = missions['Total weight (kg)'] / saturnVPayload
missions['Sample : Crewed area'] = missions['Sample weight (kg)'] / missions['Total weight (kg)']
missions['Sample : Payload'] = missions['Sample weight (kg)'] / saturnVPayload
missions
'''

**Observação**

Estamos chamando os dois módulos de área tripulada no DataFrame, porque eles são as partes da nave em que a tripulação pode ficar e, provavelmente, em que as amostras também ficariam.

## Salvar as proporções

Podemos usar a função **mean()** para calcular a média de todas essas proporções em todas as missões.

'''
crewedArea_payload_ratio = missions['Crewed area : Payload'].mean()
sample_crewedArea_ratio = missions['Sample : Crewed area'].mean()
sample_payload_ratio = missions['Sample : Payload'].mean()
print(crewedArea_payload_ratio)
print(sample_crewedArea_ratio)
print(sample_payload_ratio)
'''

Em seguida, podemos usar essas proporções para prever a capacidade da Artemis quanto às amostras.