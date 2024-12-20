# pipeline-etl-bitcoin

Este projeto é um **pipeline de ETL (Extração, Transformação e Carga)**, que automatiza o processo de obtenção, manipulação e armazenamento de dados sobre a cotação do Bitcoin. Abaixo está a explicação detalhada das etapas do projeto:

---

### **Objetivo do Projeto**
Criar um pipeline que:
1. Extrai dados de uma API (Coindesk) e de um arquivo CSV.
2. Transforma os dados para incluir informações adicionais, como conversão para BRL.
3. Carrega os dados processados para um banco de dados MySQL.

---

### **Componentes do Projeto**

#### **1. Dependências**
As bibliotecas necessárias são instaladas com o comando:
```bash
pip install pandas requests sqlalchemy mysql-connector-python
```
Elas servem para:
- **`pandas`**: Manipulação de dados.
- **`requests`**: Comunicação com APIs.
- **`sqlalchemy`** e **`mysql-connector-python`**: Conexão e integração com o banco de dados MySQL.

---

#### **2. Configuração**
O dicionário `config` define os parâmetros do pipeline:
- **`api_url`**: URL da API Coindesk, que fornece informações sobre a cotação do Bitcoin.
- **`csv_path`**: Caminho do arquivo CSV contendo dados adicionais.
- **`database_url`**: URL de conexão para o banco MySQL.

---

#### **3. Extração**
**Função `extract_from_api`:**
- Faz uma requisição à API para obter a cotação do Bitcoin em tempo real.
- Processa os dados retornados para extrair:
  - **Código da moeda** (USD).
  - **Taxa de câmbio** (convertida para float).
  - **Horário da atualização**.

**Função `extract_from_csv`:**
- Lê os dados de um arquivo CSV especificado no caminho `csv_path`.

---

#### **4. Transformação**
**Função `transform`:**
- Adiciona uma nova coluna `rate_in_brl` com a taxa convertida para reais (usando uma taxa fixa de 5.0 como exemplo).
- Inclui uma coluna `source` para indicar a origem dos dados (API ou CSV).

---

#### **5. Combinação dos Dados**
Os dados extraídos da API e do CSV são combinados usando o método `pd.concat`, formando um único `DataFrame`.

---

#### **6. Carga**
**Função `load_to_db`:**
- Utiliza o `SQLAlchemy` para estabelecer uma conexão com o banco de dados MySQL.
- Carrega os dados transformados na tabela **`bitcoin_rates`** do banco de dados, usando a configuração `database_url`.

---

### **Fluxo Geral**
1. **Extração:**
   - Dados da API e do CSV são lidos.
2. **Transformação:**
   - Dados combinados e enriquecidos com informações adicionais.
3. **Carga:**
   - Dados transformados são armazenados em uma tabela no banco MySQL.

---

### **Potenciais Aplicações**
- Monitoramento automatizado de cotações de criptomoedas.
- Alimentação de dashboards ou sistemas analíticos com dados atualizados.
- Consolidação de múltiplas fontes de dados financeiras em um banco de dados centralizado.


### **Exemplo de Uso**
Ao executar o script, o pipeline realizará todas as etapas automaticamente, garantindo que os dados mais recentes do Bitcoin sejam processados e armazenados com segurança no banco de dados MySQL.
