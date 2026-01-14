

---

# 📊 Análise de Cancelamentos de Clientes

Este projeto tem como objetivo analisar os cancelamentos de clientes de uma empresa, identificando padrões e possíveis causas para reduzir a taxa de churn.

## 🚀 Passo a passo

### 1. Importar bibliotecas e carregar a base de dados
```python
import pandas as pd

tabela = pd.read_csv("cancelamentos.csv")
```
- Utilizamos **pandas** para manipulação da base de dados.
- A base `cancelamentos.csv` contém informações sobre clientes e se cancelaram ou não o serviço.

---

### 2. Visualizar e limpar a base
```python
tabela = tabela.drop(columns="CustomerID")
display(tabela)
```
- Removemos colunas desnecessárias (ex.: `CustomerID`).
- Visualizamos os dados para entender a estrutura.

---

### 3. Corrigir problemas da base
```python
display(tabela.info())
tabela = tabela.dropna()
display(tabela.info())
```
- Remoção de linhas duplicadas e valores nulos.
- Garantia de consistência nos formatos dos dados.

---

### 4. Análise inicial
```python
display(tabela["cancelou"].value_counts())
display(tabela["cancelou"].value_counts(normalize=True))
```
- Contamos quantos clientes **cancelaram (1)** e quantos **não cancelaram (0)**.
- Calculamos a porcentagem de cancelamentos.

---

### 5. Análise detalhada
```python
import plotly.express as px

for coluna in tabela.columns:
    grafico = px.histogram(tabela, x=coluna, color="cancelou", barmode="group", text_auto=True)
    grafico.show()
```
- Criamos gráficos para cruzar informações e identificar padrões de cancelamento.

#### 🔎 Insights encontrados:
1. **Duração do contrato**  
   - Contrato mensal → alta taxa de cancelamento.  
   - Contrato anual → baixa taxa de cancelamento.  
   - Contrato semestral → taxa intermediária.  
   👉 Estratégia: oferecer descontos para contratos anuais.

2. **Ligações para o call center**  
   - Clientes com mais de 4 ligações → quase todos cancelam.  
   👉 Estratégia: criar equipe especializada para atender clientes com 3+ ligações.

3. **Dias de atraso no pagamento**  
   - Clientes com mais de 20 dias de atraso → alta taxa de cancelamento.  
   👉 Estratégia: alerta vermelho para atrasos acima de 15 dias.

---

### 6. Filtragem da base
```python
# Remover clientes com contrato mensal
tabela = tabela[tabela["duracao_contrato"] != "Monthly"]

# Manter clientes com até 4 ligações
tabela = tabela[tabela["ligacoes_callcenter"] <= 4]

# Manter clientes com até 20 dias de atraso
tabela = tabela[tabela["dias_atraso"] <= 20]

display(tabela["cancelou"].value_counts(normalize=True).map("{:.1%}".format))
```
- Após aplicar filtros, a taxa de cancelamento cai significativamente.

---

## 📈 Conclusão
- Taxa inicial de cancelamento: **56%**.  
- Após aplicar filtros e estratégias: redução expressiva da taxa.  
- Principais fatores de cancelamento:
  - Contrato mensal.
  - Muitas ligações ao call center.
  - Atrasos elevados no pagamento.

---

## 🛠️ Tecnologias utilizadas
- **Python 3**
- **Pandas** (manipulação de dados)
- **Plotly Express** (visualização gráfica)

---
