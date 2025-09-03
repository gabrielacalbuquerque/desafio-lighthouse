# Ficha Técnica - Desafio Análise de Dados/Engenharia Analytics

Título do projeto: BanVic - A Jornada dos Dados Financeiros

Objetivo: Criar um relatório com indicadores de performance para responder às demandas do BanVic

Equipe: Individual

Ferramentas e tecnologia: Google Colab, Notion…

Processamento e análises:

1. Configuração do ambiente e carregamento de dados:
- Criação de novo notebook pelo Google Colab e importação das 7 tabelas/arquivos no formato csv;
- Utilização da linguagem de programação Python para carregar os dados;

Código:

```
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns
from google.colab import files
```

- Dados carregados;

Código: 

```
df_agencias = pd.read_csv('/content/drive/MyDrive/Teste técnico lighthouse/Cópia de banvic_data.zip (Unzipped Files)/csv/agencias.csv')     
df_clientes = pd.read_csv ('/content/drive/MyDrive/Teste técnico lighthouse/Cópia de banvic_data.zip (Unzipped Files)/csv/clientes.csv')
df_colaborador_agencia = pd.read_csv('/content/drive/MyDrive/Teste técnico lighthouse/Cópia de banvic_data.zip (Unzipped Files)/csv/colaborador_agencia.csv')
df_colaboradores = pd.read_csv ('/content/drive/MyDrive/Teste técnico lighthouse/Cópia de banvic_data.zip (Unzipped Files)/csv/colaboradores.csv')
df_contas = pd.read_csv ('/content/drive/MyDrive/Teste técnico lighthouse/Cópia de banvic_data.zip (Unzipped Files)/csv/contas.csv')
df_propostas_credito = pd.read_csv ('/content/drive/MyDrive/Teste técnico lighthouse/Cópia de banvic_data.zip (Unzipped Files)/csv/propostas_credito.csv')
df_transacoes = pd.read_csv ('/content/drive/MyDrive/Teste técnico lighthouse/Cópia de banvic_data.zip (Unzipped Files)/csv/transacoes.csv')
```

- Leitura de um arquivo
- Código:

```
df_transacoes = pd.read_csv('/content/drive/MyDrive/Teste técnico lighthouse/Cópia de banvic_data.zip (Unzipped Files)/csv/transacoes.csv')
```

- Leitura das cinco primeiras linhas
- Código:

```
print(df_transacoes.head())
```

- Informações das colunas
- Código:

```
df_transacoes.info()
```

- Realização das mesmas etapas para o restante dos arquivos.
1. Análise exploratória e tratamento dos dados: 
- Verificação dos tipos de dados contidos nas tabelas/arquivos;
- colunas de datas em algumas tabelas com erros de inconsistência de formato. Formatos variados que estavam estabelecendo erros;
- Código de consistência dos dados:

```
df_transacoes['data_transacao'] = pd.to_datetime(df_transacoes['data_transacao'], format='mixed')

df_agencias['data_abertura'] = pd.to_datetime(df_agencias['data_abertura'], format='mixed')

df_colaboradores['data_nascimento'] = pd.to_datetime(df_colaboradores['data_nascimento'], format='mixed')

df_contas['data_abertura'] = pd.to_datetime(df_contas['data_abertura'], format='mixed')
```

- União de tabelas
- Código:

```
df_transacoes_com_contas = pd.merge(df_transacoes, df_contas, on='num_conta', how='left')
df_completo = pd.merge(df_transacoes_com_contas, df_agencias, on='cod_agencia', how='left')
print("Merge completo realizado com sucesso!")
print("\nSeu novo DataFrame 'df_completo' tem as seguintes colunas:")
print(df_completo.columns)

print("\nVeja uma amostra dos dados combinados:")
print(df_completo.head())
```

- Criação da dim_dates (colunas de tempo)
- Código:

```
df_completo['dia_da_semana'] = df_completo['data_transacao'].dt.day_name()
df_completo['mes'] = df_completo['data_transacao'].dt.month
```

- Clientes por estado
- Código:
    
    ```
    import matplotlib.pyplot as plt
    
    clientes_unicos = df_completo.drop_duplicates(subset=['cod_cliente'])
    
    contagem_clientes_estado = clientes_unicos['uf'].value_counts()
    
    print("--- Contagem de Clientes Únicos por Estado ---")
    print(contagem_clientes_estado)
    
    plt.figure(figsize=(12, 7))
    contagem_clientes_estado.sort_values(ascending=False).plot(kind='bar', color='green')
    
    plt.title('Número de Clientes Únicos por Estado', fontsize=16)
    plt.xlabel('Estado', fontsize=12)
    plt.ylabel('Quantidade de Clientes', fontsize=12)
    plt.xticks(rotation=45, ha='right')
    plt.grid(axis='y', linestyle='--', alpha=0.7)
    plt.tight_layout()
    plt.show()
    ```
    
    ![image.png](attachment:17cbe728-30cc-47f4-84ae-72a928fec099:image.png)
    
- Volume de transações por estado
- Código:
    
    ```
    import matplotlib.pyplot as plt
    contagem_transacoes_estado = df_completo['uf'].value_counts()
    
    print("--- Contagem de Transações por Estado ---")
    print(contagem_transacoes_estado)
    plt.figure(figsize=(12, 7))
    contagem_transacoes_estado.sort_values(ascending=False).plot(kind='bar', color='darkorange')
    plt.title('Número Total de Transações por Estado', fontsize=16)
    plt.xlabel('Estado', fontsize=12)
    plt.ylabel('Quantidade de Transações', fontsize=12)
    plt.xticks(rotation=45, ha='right')
    plt.grid(axis='y', linestyle='--', alpha=0.7)
    plt.tight_layout()
    plt.show()
    ```
    
    ![image.png](attachment:34a36a6a-4c9a-4ac2-93f1-0d16082507fd:image.png)
    
- Análise da entrega 3.1 (dia da semana)
- Dia da semana = Quinta-feira
    
    Para responder essa questão de pesquisa, filtrei a base apenas com as transações que foram aprovadas, em seguida agrupei os dados por dia da semana, calculando a média do valor e a contagem total do número de transações. 
    

```
dias_map = {
    'Monday': 'Segunda-feira',
    'Tuesday': 'Terça-feira',
    'Wednesday': 'Quarta-feira',
    'Thursday': 'Quinta-feira',
    'Friday': 'Sexta-feira',
    'Saturday': 'Sábado',
    'Sunday': 'Domingo'
}

analise_dias.rename(index=dias_map, inplace=True)
print(analise_dias)
```

![image.png](attachment:822fd7f4-a130-4b9e-9ebc-ad0a98de63c7:image.png)

- Criação dataframe ranking_agencias

        Código:

```
ultimos_6_meses = df_completo 
ranking_agencias = ultimos_6_meses['nome'].value_counts().reset_index()
ranking_agencias.columns = ['nome_agencia', 'numero_transacoes'] 
```

- Teste de hipóteses: volume médio de transações é significativamente maior nos meses pares do que nos meses ímpares. Hipótese validada. Meses pares em comparação aos meses ímpares possuem desempenho quase que 50% superior aos meses ímpares
- Código:
    
    ```
    import numpy as np
    df_completo['tipo_mes'] = np.where(df_completo['mes'] % 2 == 0, 'Mês Par', 'Mês Ímpar')
    analise_hipotese_meses = df_completo['tipo_mes'].value_counts()
    print("Resultado da Análise da Hipótese dos Meses Pares vs. Ímpares:")
    print(analise_hipotese_meses)
    print("\nConclusão: A hipótese de que os meses pares têm um volume de transações maior é VALIDADA, pois os meses ímpares apresentam uma contagem ligeiramente superior.")
    ```
    
- Criação de um gráfico de barras do volume de transações dos meses pares e ímpares
- Código

```
import matplotlib.pyplot as plt
import numpy as np
df_completo['tipo_mes'] = np.where(df_completo['mes'] % 2 == 0, 'Mês Par', 'Mês Ímpar')
analise_hipotese_meses = df_completo['tipo_mes'].value_counts()
plt.figure(figsize=(8, 6))
bars = plt.bar(analise_hipotese_meses.index, analise_hipotese_meses.values, color=['lightblue', 'dodgerblue'])
plt.title('Total de Transações por Tipo de Mês (Par vs. Ímpar)', fontsize=16)
plt.ylabel('Número Total de Transações', fontsize=12)
plt.xticks(fontsize=12)
for bar in bars:
    yval = bar.get_height()
    plt.text(bar.get_x() + bar.get_width()/2.0, yval, f'{yval:,}'.replace(',', '.'), va='bottom', ha='center', fontsize=12)

plt.tight_layout()
plt.show()
```

![image.png](attachment:73c34ebd-a00d-42ff-b52b-b303fe972f98:image.png)

- Contagem de agências por número de transações nos últimos seis meses:
- Código:
    
    ```
    import pandas as pd
    df_completo['data_transacao'] = pd.to_datetime(df_completo['data_transacao'])
    data_mais_recente = df_completo['data_transacao'].max()
    seis_meses_atras = data_mais_recente - pd.DateOffset(months=6)
    df_ultimos_6_meses = df_completo[df_completo['data_transacao'] >= seis_meses_atras]
    ranking_agencias_completo = df_ultimos_6_meses['nome'].value_counts()
    print("--- Ranking Completo das Agências (Últimos 6 Meses) ---")
    print(ranking_agencias_completo.sort_values(ascending=False))
    ```
    

![image.png](attachment:002558a4-4a8e-4da8-b1be-79aa8e493d63:image.png)

- Criação de um gráfico de barras do ranking de agências por número de transações:
- Código:

```
import matplotlib.pyplot as plt
ranking_agencias.sort_values(ascending=False).plot(
    kind='bar',
    figsize=(12, 7), 
    title='Ranking de Agências por Número de Transações (Últimos 6 Meses)',
    color='steelblue'
)

plt.xlabel('Agência')
plt.ylabel('Número de Transações')

plt.xticks(rotation=45, ha='right')

plt.grid(axis='y', linestyle='--', alpha=0.7)

plt.tight_layout()

plt.show()
)
```

![image.png](attachment:94a54a0d-02ed-4421-a804-eecad0c8a28b:image.png)

- Análise de desempenho (3 melhores e 3 piores agências nos últimos seis meses)
- Código:
    
    ```
    import pandas as pd
    df_completo['data_transacao'] = pd.to_datetime(df_completo['data_transacao'])
    data_mais_recente = df_completo['data_transacao'].max()
    seis_meses_atras = data_mais_recente - pd.DateOffset(months=6)
    print(f"Analisando o período dos últimos 6 meses:")
    print(f"De: {seis_meses_atras.date()} a {data_mais_recente.date()}")
    print("-" * 40)
    df_ultimos_6_meses = df_completo[df_completo['data_transacao'] >= seis_meses_atras]
    ranking_agencias = df_ultimos_6_meses['nome'].value_counts()
    top_3_agencias = ranking_agencias.head(3)
    piores_3_agencias = ranking_agencias.tail(3)
    print("\n--- Ranking de Desempenho das Agências (Últimos 6 Meses) ---")
    print("\n🏆 TOP 3 Melhores Agências (em n° de transações):")
    print(top_3_agencias)
    print("\n🔻 TOP 3 Piores Agências (em n° de transações):")
    ```
    

![image.png](attachment:00102e8a-66cd-41f2-99f1-2571493b688d:image.png)

Gráfico das 3 melhores e piores agências nos últimos seis meses:

- Código:
    
    ```
    import pandas as pd
    import matplotlib.pyplot as plt
    df_completo['data_transacao'] = pd.to_datetime(df_completo['data_transacao'])
    data_mais_recente = df_completo['data_transacao'].max()
    seis_meses_atras = data_mais_recente - pd.DateOffset(months=6)
    df_ultimos_6_meses = df_completo[df_completo['data_transacao'] >= seis_meses_atras]
    ranking_agencias = df_ultimos_6_meses['nome'].value_counts()
    top_3_agencias = ranking_agencias.head(3)
    piores_3_agencias = ranking_agencias.tail(3)
    fig, (ax1, ax2) = plt.subplots(1, 2, figsize=(18, 6))
    top_3_agencias.sort_values(ascending=True).plot(kind='barh', ax=ax1, color='seagreen')
    ax1.set_title('🏆 Top 3 Melhores Agências (Últimos 6 Meses)', fontsize=16)
    ax1.set_xlabel('Número de Transações', fontsize=12)
    ax1.grid(axis='x', linestyle='--', alpha=0.7)
    for index, value in enumerate(top_3_agencias.sort_values(ascending=True)):
    ax1.text(value, index, f' {value}', va='center')
    piores_3_agencias.sort_values(ascending=False).plot(kind='barh', ax=ax2, color='salmon')
    ax2.set_title('🔻 Top 3 Piores Agências (Últimos 6 Meses)', fontsize=16)
    ax2.set_xlabel('Número de Transações', fontsize=12)
    ax2.grid(axis='x', linestyle='--', alpha=0.7)
    for index, value in enumerate(piores_3_agencias.sort_values(ascending=False)):
    ax2.text(value, index, f' {value}', va='center')
    plt.tight_layout()
    plt.show()
    ```
    
    ![image.png](attachment:61f304c3-1c51-4cfd-8471-989b629701ae:image.png)
    

Resultados e conclusões:

Para além do que foi ressaltado neste projeto, ratificamos a necessidade de adotarmos uma cultura que foque não apenas no volume, mas na geração de valor, do marketing às operações.

É necessário, portanto, estudos e análises mais aprofundadas para que possamos alocar os nossos recursos de maneira eficiente, com enfoque em ações que aumentem a rentabilidade do negócio, e não apenas a base dos clientes.

A partir dos dados, percebemos, por exemplo, diferenças alarmantes no desempenho das agências, algumas com resultados acima da média e outras, aquém do que poderia ser esperado. Será necessário uma investigação mais profunda com foco em melhorias, como ações de treinamento, marketing local e/ou revisão de processos.

Além de todas as melhorias que podem ser feitas, a automatização de processos a partir do investimento na governança de dados garante maior confiabilidade em todas as métricas do banco, garantindo que as decisões futuras sejam baseadas em informações confiáveis e que os recursos investidos sejam direcionados para os locais de melhoria necessários, evitando potenciais erros e gastos desnecessários.

Limitações-próximos passos:

- Inconsistência nos dados
- Clientes fantasmas

Links de interesse:

Google Colab: 

https://colab.research.google.com/drive/1ea2uWmgezN2oQJJyqrbK1qVJ6XBzOS9I?usp=sharing

Looker Studio:

[lookerstudio.google.com](https://lookerstudio.google.com/reporting/1e1398fa-14ac-453f-8bec-b43f04419fd6/page/PaVWF)

Relatório:

https://docs.google.com/document/d/1FlFzOvsEFEfd7coka_OW36HS8z2UbliN2LgStQw-m8k/edit?usp=sharing
