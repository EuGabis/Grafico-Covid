import pandas as pd
import matplotlib.pyplot as plt
import streamlit as st

# Lendo o dataset a partir de uma URL e armazenando-o em um DataFrame chamado 'df'
df = pd.read_csv('https://raw.githubusercontent.com/wcota/covid19br/master/cases-brazil-states.csv')

# Melhorando o nome das colunas do DataFrame
df = df.rename(columns={'newDeaths': 'Novos óbitos',
                        'newCases': 'Novos casos',
                        'deaths_per_100k_inhabitants': 'Óbitos por 100 mil habitantes',
                        'totalCases_per_100k_inhabitants': 'Casos por 100 mil habitantes'})

# Obtendo a lista de estados presentes no DataFrame
estados = list(df['state'].unique())

# Criando um seletor de estado na barra lateral da aplicação
state = st.sidebar.selectbox('Qual estado? ', estados)


# Definindo as opções para seleção de coluna
colunas = ['Novos óbitos', 'Novos casos', 'Óbitos por 100 mil habitantes', 'Casos por 100 mil habitantes']

# Criando um seletor de coluna na barra lateral da aplicação
column = st.sidebar.selectbox('Qual tipo de informação? ', colunas)

# Filtrando as linhas do DataFrame que correspondem ao estado selecionado
df_state = df[df['state'] == state]

# Criando um gráfico de linha usando o Matplotlib com base nos dados do estado selecionado
fig, ax = plt.subplots()
ax.plot(df_state['date'], df_state[column])
ax.set_title(f'{column} - {state}')
ax.set_xlabel('Data')
ax.set_ylabel(column.upper())

# Exibindo o gráfico
st.pyplot(fig)
