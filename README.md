# Forecast_CO2
Script para fazer a previsão da concentração de CO2 na atmosfera; Os dados foram obtidos em: https://gml.noaa.gov/ccgg/trends/

Séries temporais apresentam algumas características que impactam o desenvolvimento
de modelos de aprendizagem de máquina, como sazonalidade (seasonality), 
tendência (trend), auto-correlação, dentre outras.

Este notebook utiliza algumas práticas para a modelagem de uma série 
temporal que contém a concentração de CO2 em ppm fornecido pelo observatório 
Maina Loa, parte da Administração Nacional Oceânica e Atmosférica (NOAA) 
e comparar os resultados obtidos.

Os dados cobrem o período de março de 1958 a setembro de 2022, com informações 
mensais; não existem dados inválidos nas observações.

O valor mínimo da série é de 313.33 ppm, obtido em outubro de 1959, e o valor 
máximo da série é de 419.13 ppm, obtido em maio de 2021. 

As medições têm valor médio de 357.339 ppm, desvio padrão de 29.688 e 
skew de 0.349.

A variável 'average' representa o valor médio mensal da concentração de CO2; 
essa variável será a variável alvo do modelo desenvolvido.
    
Os dados foram obtidos em: https://gml.noaa.gov/ccgg/trends/

Estratégia:
    A estratégia utilizada para o desenvolvimento do modelo foi composta por 
    4 passos:
        1)  Treinamento de um modelo de regressão linear com variáveis que 
            representam o mês e o ano de cada observação e uma sequencia de
            variáveis com a concentração de CO2 dos meses anteriores; os dados
            de treino cobriam o período de 01 de 1960 a 12 de 2009; os dados 
            de teste de 01/2010 a 12/2019. O menor erro medido pela métrica 
            rmse foi 0.443.
        2)  A mesma aboragem anterior, com perído de treino (10 anos,
            01/2010 a 12/2019) e teste (2 anos, 01/2020 a 12/2021) menores - 
            uma tentativa de explorar a taxa de crescimento da série temporal.
            O menor erro obtido foi de 0.331.
        3)  Utilização de uma janela de treinamento mais curta (120 meses) e 
            uma janela de teste com 24 meses; o valor médio do erro nesse 
            passo foi de 0.445.
        4)  Utilização de mais uma variável independente, que representa a 
            sazonalidade da série temporal analisada, obtida pela rotina 
            seasonal_decompose, disponível no módulo statsmodels.tsa.seasonal, 
            que decompÕe uma série temporal em 3 componentes: tendência, 
            sazonalidade e ruído; também foram empregadas as jamelas 
            de tamanhos idênticos às utilizadas nos passo anterior. Nesse caso 
            o valor médio do erro foi de 0.421.
        Os passos 3 e 4, de certa forma, realizam uma "validação cruzada" no 
        modelo, adaptada às características da série temporal analisada, que
        possui uma taxa de crescimento que se acelara ao longo dos anos, 
        como observado nos gráficos gerados.
        
        @author: Miguel Argollo, miguel.argollo@gmail.com.
