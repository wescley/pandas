# Automatização de tarefas com Pandas: Um Caso de Uso Prático
A biblioteca Pandas do Python é amplamente utilizada para manipulação e análise de dados. Sua simplicidade e poder permitem automatizar tarefas repetitivas de processamento de dados, economizando tempo e reduzindo erros. Este artigo apresenta um caso de uso prático que demonstra como usar o Pandas para automatizar a leitura e análise de arquivos de entrada.

## Contexto do Problema
Imagine uma empresa que recebe diariamente relatórios em formato CSV contendo informações de vendas. Cada arquivo possui as colunas: "Data", "Produto", "Quantidade" e "Valor". A tarefa é calcular:

O total de vendas do dia.

O produto mais vendido.

Relatórios consolidados mensais.

Fazer isso manualmente pode ser demorado, mas com Pandas, é possível automatizar todo o processo.

## Solução com Pandas
Aqui está um exemplo de código que implementa a solução:

import pandas as pd
import os

### Caminho da pasta contendo os arquivos CSV
pasta_arquivos = 'relatorios_vendas'

### Lista para consolidar os dados mensais
dados_mensais = []

### Iteração pelos arquivos na pasta
for arquivo in os.listdir(pasta_arquivos):
    if arquivo.endswith('.csv'):
        # Lê o arquivo
        caminho = os.path.join(pasta_arquivos, arquivo)
        df = pd.read_csv(caminho, delimiter=';')

        # Total de vendas do dia
        df['Total'] = df['quantidade'] * df['valor']
        total_vendas = df['Total'].sum()
        print(f"Total de vendas no arquivo {arquivo}: R$ {total_vendas:.2f}")

        # Produto mais vendido
        produto_mais_vendido = df.groupby('produto')['quantidade'].sum().idxmax()
        print(f"Produto mais vendido em {arquivo}: {produto_mais_vendido}")

        # Adiciona ao consolidado mensal
        df['data'] = pd.to_datetime(df['data'])
        dados_mensais.append(df)

### Consolida os dados em um único DataFrame
consolidado = pd.concat(dados_mensais)

### Relatório mensal consolidado
relatorio_mensal = consolidado.groupby(consolidado['data'].dt.to_period('M')).agg({
    'Total': 'sum',
    'quantidade': 'sum'
})

print("\nRelatório Mensal Consolidado:")
print(relatorio_mensal)

### Exporta o relatório consolidado para um arquivo CSV
relatorio_mensal.to_csv('relatorio_mensal.csv') 

Explicação do Código
Leitura de Arquivos: A função pd.read_csv() lê cada arquivo CSV e converte os dados em um DataFrame.

Cálculos Diários: São calculados o total de vendas e o produto mais vendido por dia.

Consolidação Mensal: Todos os DataFrames diários são combinados em um único DataFrame usando pd.concat(), e os dados são agrupados por mês com groupby().

Exportação: O relatório final é exportado para um arquivo CSV.


Benefícios
Automatização: Elimina a necessidade de processar os relatórios manualmente.

Escalabilidade: O código pode ser adaptado para lidar com milhares de arquivos.

Flexibilidade: Permite ajustes rápidos para incluir novas colunas ou métricas.


## Conclusão
Com a biblioteca Pandas, é possível automatizar o processamento de arquivos de entrada de forma eficiente e escalável. Este caso de uso exemplifica como tarefas repetitivas podem ser simplificadas, permitindo que os profissionais foquem em análises mais estratégicas.
