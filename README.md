![petrobras](https://github.com/AlbertoFAraujo/R_Petrobras/assets/105552990/052904a1-7784-456b-9580-92a381a4a712)

### Tecnologias utilizadas: 
| [<img align="center" alt="R_studio" height="60" width="60" src="https://github.com/AlbertoFAraujo/R_Petrobras/assets/105552990/02dff6df-07be-43dc-8b35-21d06eabf9e1">](https://posit.co/download/rstudio-desktop/) | [<img align="center" alt="ggplot" height="60" width="60" src="https://github.com/AlbertoFAraujo/R_Petrobras/assets/105552990/db55b001-0d4c-42eb-beb2-5131151c7114">](https://plotly.com/r/) | [<img align="center" alt="plotly" height="60" width="60" src="https://github.com/AlbertoFAraujo/R_Petrobras/assets/105552990/5f681062-c399-44af-a658-23e94b8b656f">](https://plotly.com/r/) | [<img align="center" alt="quantmod" height="60" width="60" src="https://github.com/AlbertoFAraujo/R_Petrobras/assets/105552990/2106b70f-0f04-475d-9c7c-cb908f911616">](https://www.rdocumentation.org/packages/quantmod/versions/0.4.26) | 
|:---:|:---:|:---:|:---:|
| R Studio | Ggplot2 | Plotly | Quantmod |
- **RStudio:** Ambiente integrado para desenvolvimento em R, oferecendo ferramentas para escrita, execução e depuração de código;
- **ggplot2:** Pacote para criação de visualizações de dados;
- **Plotly:** Biblioteca interativa para criação de gráficos e visualizações em diversas linguagens;
- **Quantmod:** Pacote para análise financeira e modelagem quantitativa em R, com foco em mercados financeiros.
<hr>

### Objetivo: 

Extrair dados das ações da Petrobrás de 2023 e realizar uma análise das ações de fechamentos.

Base de dados: Petrobrás (PETR4.SA)
<hr>

### Script R: 

```r
knitr::opts_chunk$set(message = FALSE)
knitr::opts_chunk$set(warning = FALSE)

# Definir um espelho de CRAN
options(repos = "http://cran.rstudio.com/")
```

```r
# Instalando os pacotes
utils::install.packages("quantmod")
install.packages('xts')
install.packages('moments')
```

```r
# Carregando os pacotes
library('quantmod')
library('xts')
library('moments')
```

```r
# Iniciar período de análise
startDate = as.Date('2023-01-01')
endDate = as.Date('2023-12-31')
```

```r
# Obter os dados
getSymbols('PETR4.SA',
           src = 'yahoo',
           from = startDate,
           to = endDate,
           auto.assign = T
           )
```

[1] "PETR4.SA"

```r
# Verificando os dados retornados
class(PETR4.SA)
is.xts(PETR4.SA)
```
[1] "xts" "zoo"
[1] TRUE

```r
# Analisando os primeiros registros
head(PETR4.SA)
tail(PETR4.SA)
```
Primeiros registros
| Data       | Abertura | Máxima | Mínima | Fechamento | Volume    | Fechamento Ajustado |
|------------|----------|--------|--------|------------|-----------|---------------------|
| 2023-01-02 | 23.54    | 23.81  | 22.80  | 22.92      | 78,424,700| 17.70948            |
| 2023-01-03 | 22.94    | 23.10  | 22.13  | 22.34      | 96,750,300| 17.26133            |
| 2023-01-04 | 21.96    | 23.59  | 21.83  | 23.05      | 129,504,000| 17.80992            |
| 2023-01-05 | 23.34    | 24.04  | 23.15  | 23.88      | 73,886,000| 18.45123            |
| 2023-01-06 | 23.94    | 24.32  | 23.55  | 23.74      | 51,851,500| 18.34306            |
| 2023-01-09 | 23.50    | 24.00  | 23.25  | 23.87      | 46,385,200| 18.44351            |

Últimos registros
| Data       | Abertura | Máxima | Mínima | Fechamento | Volume    | Fechamento Ajustado |
|------------|----------|--------|--------|------------|-----------|---------------------|
| 2023-12-20 | 36.37    | 36.74  | 36.29  | 36.38      | 38,947,900| 36.38               |
| 2023-12-21 | 36.67    | 36.68  | 36.07  | 36.39      | 30,511,900| 36.39               |
| 2023-12-22 | 36.50    | 36.80  | 36.37  | 36.74      | 31,234,700| 36.74               |
| 2023-12-26 | 36.86    | 37.37  | 36.83  | 37.33      | 23,466,800| 37.33               |
| 2023-12-27 | 37.32    | 37.43  | 37.13  | 37.36      | 19,588,500| 37.36               |
| 2023-12-28 | 37.23    | 37.36  | 37.04  | 37.24      | 21,421,900| 37.24               |

```r
# Dados de fechamento
PETR4.SA.Close <- PETR4.SA[,'PETR4.SA.Close']
is.xts(PETR4.SA.Close)
```
[1] TRUE

```r
# Extrai e transforma colunas de séries temporais
head(Cl(PETR4.SA.Close))
```
| Data       | Fechamento |
|------------|------------|
| 2023-01-02 | 22.92      |
| 2023-01-03 | 22.34      |
| 2023-01-04 | 23.05      |
| 2023-01-05 | 23.88      |
| 2023-01-06 | 23.74      |
| 2023-01-09 | 23.87      |

```r
# Plotando o gráfico da Petrobrás com o plot antivo do quantmod
candleChart(PETR4.SA)
```
![image](https://github.com/AlbertoFAraujo/R_Petrobras/assets/105552990/64dfc98a-782d-4ad9-8a31-96d1769e7a98)

```r
# Boolinger bands
addBBands(n = 20, sd = 2) # Ajuste às condições de mercado, com dois desvios
```
![image](https://github.com/AlbertoFAraujo/R_Petrobras/assets/105552990/ed04a0e7-5a69-413b-915f-4667bd990a18)

```r
# Indicador ADX, média 11 do tipo exponencial
addADX(n = 11, maType = 'EMA')
```
![image](https://github.com/AlbertoFAraujo/R_Petrobras/assets/105552990/fb0cb625-6732-45b1-adc6-1f20bbe3dd73)

```r
# Logaritmo para alterar a escala e analisar detalhadamente os dados
PETR4.SA.log <- diff(log(PETR4.SA.Close), lag = 1)
PETR4.SA.log
```

```r
# Removendo valores NA
PETR4.SA.log <- PETR4.SA.log[-1]
PETR4.SA.log
```

```r
# Plotando os dados de fechamento
plot(PETR4.SA.log,
     main = 'Fechamento ações Petrobrás',
     col = "red",
     xlab = 'Data',
     ylab = 'Preço',
     major.ticks = 'months',
     minor.ticks = FALSE
)
```
![image](https://github.com/AlbertoFAraujo/R_Petrobras/assets/105552990/1b386427-369f-4ecf-8ffd-e80a9714909a)

```r
# Medidas estatística
stat <- c('Média','Desvio Padrão','Assimetria', 'Curtose')
PETR4.SA.stat <- c(mean(PETR4.SA.log),
                   sd(PETR4.SA.log),
                   skewness(PETR4.SA.log),
                   kurtosis(PETR4.SA.log)
                   )
names(PETR4.SA.stat) <- stat
PETR4.SA.stat
```
| Média         | Desvio Padrão | Assimetria    | Curtose       |
|---------------|---------------|---------------|---------------|
| 0.001965075   | 0.021558859   | -0.560789901  | 4.848053883   |

```r
# Plotando o gráfico interativo com plotly
library(plotly)

df <- data.frame(Date=index(PETR4.SA),coredata(PETR4.SA))

fig <- df %>% 
  plot_ly(x = ~Date, 
          type="candlestick",
          open = ~PETR4.SA.Open, 
          close = ~PETR4.SA.Close,
          high = ~PETR4.SA.High, 
          low = ~PETR4.SA.Low) %>% 
  layout(title = "Ações Petrobrás 2023",
         width = 900)
fig
```

```r
# Salvar os dados
saveRDS(PETR4.SA, 
        file = 'PETR4.SA.rds')
fileArq <- readRDS('PETR4.SA.rds')
dir()
head(fileArq)
```
| Data       | Abertura | Máxima | Mínima | Fechamento | Volume    | Fechamento Ajustado |
|------------|----------|--------|--------|------------|-----------|---------------------|
| 2023-01-02 | 23.54    | 23.81  | 22.80  | 22.92      | 78,424,700| 17.70948            |
| 2023-01-03 | 22.94    | 23.10  | 22.13  | 22.34      | 96,750,300| 17.26133            |
| 2023-01-04 | 21.96    | 23.59  | 21.83  | 23.05      | 129,504,000| 17.80992            |
| 2023-01-05 | 23.34    | 24.04  | 23.15  | 23.88      | 73,886,000| 18.45123            |
| 2023-01-06 | 23.94    | 24.32  | 23.55  | 23.74      | 51,851,500| 18.34306            |
| 2023-01-09 | 23.50    | 24.00  | 23.25  | 23.87      | 46,385,200| 18.44351            |

