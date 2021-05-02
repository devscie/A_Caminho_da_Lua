## Exercício – Adicionar dados do peso do foguete à análise da missão

Como mencionamos em uma unidade anterior, não há muitas informações públicas sobre exatamente como os foguetes são feitos. Como não somos especialistas, usaremos os dados disponíveis para fazer suposições.

Essa é uma parte crítica da ciência de dados: garantir que as suposições que você fizer representem com precisão os dados e o impacto deles. Isso significa que este módulo específico **não** é algo que você deva usar para realmente aconselhar astronautas. Embora possa parecer óbvio aqui, é essencial entender a importância disso conforme você aplicar o que aprender neste módulo a outros conjuntos de dados e problemas a serem resolvidos.

## Revisitando o programa Apollo

O programa Apollo se concentrou em usar o foguete Saturn V [https://www.nasa.gov/centers/johnson/rocketpark/saturn_v.html] para enviar seres humanos para o espaço e para a Lua. O foguete Saturn V usado no programa Apollo é conhecido como um foguete de três estágios. Isso significa que o foguete tem três partes, cada uma das quais queima em momentos diferentes para atingir uma meta diferente.

O primeiro estágio é a parte do impulso principal, que lança o foguete cerca de 68 quilômetros no céu e, em seguida, volta à terra, deixando o foguete significativamente mais leve. O segundo estágio começa a queimar seus motores até que o foguete quase atinja a órbita da Terra e, da mesma forma, volta para a Terra. O estágio final coloca a nave espacial na órbita da Terra e a impulsiona em direção à Lua.

https://docs.microsoft.com/pt-br/learn/modules/plan-moon-mission-using-python-pandas/media/rocket-flying.png

Como aprendemos com o filme A Caminho da Lua, saber exatamente quanto peso você pode ter em cada estágio afeta significativamente o resultado da missão. Também sabemos que os astronautas no espaço precisam de muitos materiais, como comida, água, oxigênio, ferramentas e instrumentos. Vimos um pouco dos suprimentos de Fei Fei quando ela voltou para encontrar Gobi no local do pouso de emergência:

https://docs.microsoft.com/pt-br/learn/modules/plan-moon-mission-using-python-pandas/media/supplies.png

A última informação relevante sobre o programa Apollo e o Saturn V é que, para os pousos lunares, há dois módulos importantes:

* Módulo de comando: o módulo em que os astronautas ficam. Quando dois astronautas estão na superfície lunar, o terceiro astronauta permanece no módulo de comando. Esse módulo volta para a Terra.
* Módulo lunar: o módulo que se desconecta do módulo de comando após ele atingir a órbita em torno da Lua. Esse módulo pousa na superfície da Lua e pode transportar dois astronautas. Quando o módulo lunar retorna da superfície para o módulo de comando, ele deixa parte da base (o mecanismo de pouso) na superfície da Lua.

Os módulos são partes críticas da nave, pois são projetados precisamente para garantir que o astronautas possa entrar na órbita da Lua, orbitar na Lua, pousar na Lua, decolar da Lua e voltar à Terra em segurança. A quantidade de espaço e de peso em cada um desses módulos é precisa a fim de garantir a segurança e o sucesso da missão. Podemos concluir que as especificações desses módulos afetam a quantidade de amostras minerais que podem ser coletadas, pois as amostras precisam ser transportadas em cada dos módulos antes de retornar à Terra.

## Adicionar dados dos módulos lunar e de comando

Usando o Arquivo de dados coordenados de ciência espacial da NASA, reunimos informações sobre cada módulo usado em cada missão. Assim como você fez ao criar as tabelas de amostras, crie mais seis colunas, três para os módulos lunares e três para os módulos de comando:

* Nome do módulo
* Massa do módulo
* Diferença de massa do módulo

Preencha os valores de NaN com 0:

'''
missions['Lunar module (LM)'] = {'Eagle (LM-5)', 'Intrepid (LM-6)', 'Antares (LM-8)', 'Falcon (LM-10)', 'Orion (LM-11)', 'Challenger (LM-12)'}
missions['LM mass (kg)'] = {15103, 15235, 15264, 16430, 16445, 16456}
missions['LM mass diff'] = missions['LM mass (kg)'].diff()
missions['LM mass diff'] = missions['LM mass diff'].fillna(value=0)

missions['Command module (CM)'] = {'Columbia (CSM-107)', 'Yankee Clipper (CM-108)', 'Kitty Hawk (CM-110)', 'Endeavor (CM-112)', 'Casper (CM-113)', 'America (CM-114)'}
missions['CM mass (kg)'] = {5560, 5609, 5758, 5875, 5840, 5960}
missions['CM mass diff'] = missions['CM mass (kg)'].diff()
missions['CM mass diff'] = missions['CM mass diff'].fillna(value=0)

missions
'''

Podemos somar alguns dos valores totais para cada missão nos módulos lunar e de comando:

'''
missions['Total weight (kg)'] = missions['LM mass (kg)'] + missions['CM mass (kg)']
missions['Total weight diff'] = missions['LM mass diff'] + missions['CM mass diff']
missions
'''
