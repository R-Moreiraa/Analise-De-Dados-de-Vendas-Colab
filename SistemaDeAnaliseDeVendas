# ====================================================
# SISTEMA DE ANÁLISE DE VENDAS - COLAB
# ====================================================

# Passo 0: Instalar bibliotecas caso não estejam instaladas
# Aqui eu instalo pandas, matplotlib e seaborn para trabalhar com dados e gráficos
!pip install pandas matplotlib seaborn --quiet

# ----------------------------------------------------
# Passo 1: Conectar ao SQLite e criar a tabela
# ----------------------------------------------------
import sqlite3

# Conectar ao banco de dados (ou criar se não existir)
conexao = sqlite3.connect('dados_vendas.db')
cursor = conexao.cursor()

# Criar a tabela de vendas
# Essa tabela vai armazenar o ID da venda, a data, o produto, a categoria e o valor
cursor.execute('''
CREATE TABLE IF NOT EXISTS vendas1 (
    id_venda INTEGER PRIMARY KEY AUTOINCREMENT,
    data_venda DATE,
    produto TEXT,
    categoria TEXT,
    valor_venda REAL
)
''')

# Inserir dados de exemplo
# Aqui estou colocando algumas vendas fictícias para fazer a análise
cursor.execute('''
INSERT INTO vendas1 (data_venda, produto, categoria, valor_venda) VALUES
('2023-01-01', 'Produto A', 'Eletrônicos', 1500.00),
('2023-01-05', 'Produto B', 'Roupas', 350.00),
('2023-02-10', 'Produto C', 'Eletrônicos', 1200.00),
('2023-03-15', 'Produto D', 'Livros', 200.00),
('2023-03-20', 'Produto E', 'Eletrônicos', 800.00),
('2023-04-02', 'Produto F', 'Roupas', 400.00),
('2023-05-05', 'Produto G', 'Livros', 150.00),
('2023-06-10', 'Produto H', 'Eletrônicos', 1000.00),
('2023-07-20', 'Produto I', 'Roupas', 600.00),
('2023-08-25', 'Produto J', 'Eletrônicos', 700.00),
('2023-09-30', 'Produto K', 'Livros', 300.00),
('2023-10-05', 'Produto L', 'Roupas', 450.00),
('2023-11-15', 'Produto M', 'Eletrônicos', 900.00),
('2023-12-20', 'Produto N', 'Livros', 250.00);
''')

# Salvar alterações no banco
conexao.commit()

# ----------------------------------------------------
# Passo 2: Carregar dados com Pandas
# ----------------------------------------------------
import pandas as pd

# Ler os dados da tabela criada e colocar em um DataFrame
df_vendas = pd.read_sql_query("SELECT * FROM vendas1", conexao)

# Converter a coluna de data para o formato datetime
df_vendas['data_venda'] = pd.to_datetime(df_vendas['data_venda'])

# Mostrar as primeiras linhas para conferir se os dados estão corretos
print("Primeiras linhas da tabela:")
print(df_vendas.head())

# Verificar informações do DataFrame (tipos, quantidade de valores não nulos)
print("\nInformações do DataFrame:")
print(df_vendas.info())

# Estatísticas básicas das colunas numéricas (como valor_venda)
print("\nEstatísticas descritivas:")
print(df_vendas.describe())

# ----------------------------------------------------
# Passo 3: Análise dos dados
# ----------------------------------------------------
# Total de vendas no ano
total_vendas = df_vendas['valor_venda'].sum()
print(f"\nTotal de vendas em 2023: R$ {total_vendas:.2f}")

# Agrupar vendas por categoria
# Aqui eu quero ver quanto cada categoria faturou
vendas_categoria = df_vendas.groupby('categoria')['valor_venda'].sum().reset_index()
print("\nVendas por categoria:")
print(vendas_categoria)

# Agrupar vendas por mês
# Para isso, criei uma coluna 'mes' com o mês da data da venda
df_vendas['mes'] = df_vendas['data_venda'].dt.month
vendas_mensais = df_vendas.groupby('mes')['valor_venda'].sum().reset_index()
print("\nVendas por mês:")
print(vendas_mensais)

# ----------------------------------------------------
# Passo 4: Visualização dos dados
# ----------------------------------------------------
import matplotlib.pyplot as plt
import seaborn as sns

# Configuração estética do Seaborn
sns.set(style="whitegrid")

# Gráfico de barras: total de vendas por categoria
plt.figure(figsize=(8,5))
sns.barplot(x='categoria', y='valor_venda', data=vendas_categoria, palette='viridis')
plt.title('Total de Vendas por Categoria')
plt.xlabel('Categoria')
plt.ylabel('Valor Total (R$)')
plt.show()

# Gráfico de linha: evolução mensal das vendas
plt.figure(figsize=(8,5))
sns.lineplot(x='mes', y='valor_venda', data=vendas_mensais, marker='o', color='orange')
plt.title('Evolução Mensal das Vendas em 2023')
plt.xlabel('Mês')
plt.ylabel('Valor Total (R$)')
plt.xticks(range(1,13))  # garantir que apareça todos os meses no eixo x
plt.show()

# ----------------------------------------------------
# Passo 5: Conclusão e insights
# ----------------------------------------------------
# Aqui eu explico de forma simples o que os dados mostraram
print("\nInsights obtidos:")
print("- Eletrônicos é a categoria com maior faturamento.")
print("- Os meses de janeiro, junho e novembro tiveram picos de vendas.")
print("- Livros e Roupas vendem menos, mas de forma constante, sugerindo oportunidade de promoções.")
