### 📊 Análise de Dados - Preços de Carros Usados

Este projeto foi desenvolvido como parte do bootcamp de Análise de Dados. Utilizamos uma base contendo informações sobre veículos usados, com o objetivo de aplicar técnicas de correlação e regressão linear para investigar fatores que influenciam o preço de venda dos veículos.

---

<br>

#### ⚠️ Observação sobre a leitura do arquivo `car_price.xls`

Durante o carregamento dos dados, foi identificado que o arquivo nomeado como `car_price.xls`, apesar de possuir a extensão `.xls` (formato tradicional do Excel), apresenta internamente a estrutura de um **arquivo CSV** (valores separados por vírgula).

Ao tentar realizar a leitura com o Pandas utilizando `pd.read_excel()` e o engine adequado (`xlrd`), o seguinte erro foi retornado:

```python
XLRDError: Unsupported format, or corrupt file: Expected BOF record; found b'Make,Mod'
Esse erro indica que o conteúdo do arquivo não está no formato binário de uma planilha Excel, e sim em formato texto, típico de arquivos .csv.
```

Para contornar essa inconsistência, a leitura correta foi realizada da seguinte forma:

```python
import pandas as pd

df = pd.read_csv('car_price.xls')  # Lido como CSV, apesar da extensão .xls
```

Mesmo com a extensão incorreta, o Pandas conseguiu interpretar o conteúdo textual corretamente. Para evitar confusões futuras, recomenda-se renomear o arquivo para car_price.csv.

#### 🔎 Análises realizadas

<br>

7. Matriz de Correlação (variáveis numéricas)
Selecionamos as variáveis numéricas: `Price`, `Year`, `Kilometer`, `Length`, `Width`, `Height`, `Seating Capacity` e `Fuel Tank Capacity`, e geramos uma matriz de correlação:

Length e Width: Correlação positiva moderada → carros mais compridos tendem a ser mais largos.

Kilometer e Price: Correlação negativa → veículos mais rodados tendem a ser mais baratos.

Fuel Tank Capacity e Seating Capacity: Correlação fraca → pouca ou nenhuma relação.

<br>

8. Gráficos de Dispersão (variáveis numéricas vs. Price)
Foi gerado um gráfico de dispersão para cada variável numérica em relação ao Price, permitindo visualizar tendências:

Observações visuais:

Year: preços mais altos estão concentrados em veículos mais novos.

Kilometer: quanto maior a quilometragem, menor o preço.

Height, Fuel Tank Capacity: dispersão aleatória, sem padrão visível.

<br>

9. Regressão Linear Simples (exemplo: Year vs. Price)
Utilizamos statsmodels.OLS para criar o modelo:

```python
X = sm.add_constant(df['Year'])
modelo = sm.OLS(df['Price'], X).fit()
print(modelo.summary())
```

Também foi plotada a reta estimada sobre os dados reais:

```python
sns.lineplot(x=df['Year'], y=modelo.fittedvalues, color='red')
```

O modelo mostrou uma relação positiva entre o ano de fabricação e o preço, como esperado.

<br>

10. Gráfico dos Resíduos (Regressão Simples)
Plotamos os resíduos do modelo simples ao lado da plotagem da reta estimada para verificar a aleatoriedade dos erros:

Os resíduos estão razoavelmente distribuídos em torno de zero, o que indica que a regressão linear simples é aceitável — embora não muito precisa isoladamente.

<br>

11. Regressão Linear Múltipla
Utilizamos múltiplas variáveis como preditoras do preço:

O gráfico dos resíduos mostrou melhor distribuição do que no modelo simples, sugerindo um ganho ao usar múltiplas variáveis para prever o preço.

#### 📌 Conclusão
A análise demonstrou que, individualmente, poucas variáveis têm alta correlação com o preço. O uso de regressão linear simples revelou relações como:

- Veículos mais novos tendem a ter preços maiores;

- Veículos com maior quilometragem tendem a valer menos.

Contudo, a regressão múltipla se mostrou mais eficaz, capturando melhor os fatores que influenciam o preço, apesar de ainda haver dispersão nos resíduos.

Modelos mais avançados, como regressão polinomial ou modelos com variáveis categóricas tratadas via one-hot encoding, poderiam ser testados em etapas futuras para melhorar a precisão.

<br>

#### 📁 Linguagens e Bibliotecas utilizadas:
Python, Pandas, NumPy, Seaborn, Matplotlib, Statsmodels, Scipy
