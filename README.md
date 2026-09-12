# Detecção de Anomalias em Transações de Cartão de Crédito

Projeto desenvolvido para estudar técnicas de **detecção de anomalias (Anomaly Detection)** aplicadas à identificação de possíveis fraudes em transações realizadas com cartão de crédito.

A análise utiliza dados públicos disponibilizados pela **ULB no Kaggle** e compara duas abordagens: **Isolation Forest** e **Autoencoder**.

---

## Visão geral

A identificação de fraudes pode ser realizada por meio de modelos supervisionados, utilizando exemplos previamente classificados. Neste projeto, a proposta é diferente: os algoritmos procuram reconhecer o comportamento predominante das transações e destacar registros que apresentam características fora desse padrão.

O treinamento dos modelos não utiliza a coluna `Class`. Essa informação é reservada para a etapa de avaliação, permitindo verificar posteriormente se as anomalias identificadas correspondem às fraudes reais.

---

## Dados utilizados

A base escolhida é o **Credit Card Fraud Detection**, disponível publicamente no Kaggle.

**Principais características:**

* 284.807 registros;
* 492 transações fraudulentas;
* aproximadamente 0,17% de fraudes;
* 30 variáveis utilizadas como entrada;
* dados referentes a transações de cartões europeus;
* variáveis `V1` a `V28` obtidas por transformação PCA;
* informações adicionais de `Time` e `Amount`.

Fonte:

[Credit Card Fraud Detection — Kaggle](https://www.kaggle.com/datasets/mlg-ulb/creditcardfraud)

---

## Estrutura

```text
.
├── data/
│   └── creditcard.csv
│
├── notebooks/
│   └── 01_exploracao.ipynb
│
├── src/
│   ├── preprocessing.py
│   ├── models.py
│   └── evaluation.py
│
├── requirements.txt
└── README.md
```

### Principais arquivos

| Arquivo               | Função                              |
| --------------------- | ----------------------------------- |
| `01_exploracao.ipynb` | Executa a análise e os experimentos |
| `preprocessing.py`    | Realiza a preparação dos dados      |
| `models.py`           | Contém os modelos de detecção       |
| `evaluation.py`       | Calcula as métricas utilizadas      |
| `requirements.txt`    | Lista as dependências do projeto    |

> O arquivo `creditcard.csv` não deve ser versionado no GitHub. A base pode ser obtida diretamente pelo Kaggle.

---

## Fluxo de processamento

O projeto segue, de forma geral, o seguinte processo:

```text
Dataset
   ↓
Análise exploratória
   ↓
Tratamento dos dados
   ↓
Criação das variáveis
   ↓
Normalização
   ↓
Treinamento dos modelos
   ↓
Identificação das anomalias
   ↓
Avaliação dos resultados
```

---

## Preparação dos dados

Antes do treinamento, são realizadas algumas etapas de tratamento:

1. análise da estrutura e distribuição dos dados;
2. verificação de registros duplicados;
3. tratamento das duplicidades encontradas;
4. criação da variável `Hour` a partir de `Time`;
5. transformação da informação temporal para uma representação cíclica;
6. aplicação de `RobustScaler` nas variáveis `Amount` e `Hour`.

O tratamento busca reduzir a influência de valores extremos e fornecer aos modelos dados em uma escala mais adequada.

---

## Modelos testados

### Isolation Forest

O **Isolation Forest**, disponível no `scikit-learn`, identifica observações que podem ser separadas com maior facilidade do restante dos dados.

Neste experimento, o algoritmo é utilizado como uma abordagem independente para atribuir um grau de anormalidade às transações.

### Autoencoder

O segundo método utiliza uma rede neural implementada com **PyTorch**.

O Autoencoder aprende a reconstruir transações consideradas normais. Durante a detecção, registros que apresentam maior erro de reconstrução são tratados como possíveis anomalias.

Arquitetura utilizada:

```text
Entrada: 30
    ↓
Camada intermediária: 14
    ↓
Saída: 30
```

O modelo é treinado somente com transações normais, sem utilizar a classificação de fraude como variável de treinamento.

---

## Avaliação

O conjunto de dados possui um forte desbalanceamento entre operações legítimas e fraudulentas. Por esse motivo, utilizar somente **accuracy** poderia gerar uma interpretação equivocada do desempenho.

Foram priorizadas as seguintes métricas:

* Precision;
* Recall;
* F1-score;
* Average Precision (AP);
* curva Precision-Recall.

A variável `Class` é utilizada somente nesta etapa para comparar as previsões dos modelos com os registros de fraude conhecidos.

---

## Resultados

Os experimentos produziram os seguintes valores de **Average Precision**:

| Modelo           | Average Precision |
| :--------------- | ----------------: |
| Isolation Forest |             0,134 |
| Autoencoder      |             0,304 |

Entre as duas abordagens avaliadas, o **Autoencoder apresentou o melhor resultado**, alcançando uma Average Precision de `0,304`.

Esse desempenho sugere que a capacidade da rede neural de representar relações não lineares entre as variáveis pode contribuir para a identificação de padrões associados às transações fraudulentas.

---

## 🛠️ Tecnologias

* Python
* pandas
* scikit-learn
* PyTorch
* matplotlib
* seaborn
* Jupyter Notebook

---

## Como executar

### 1. Clonar o projeto

```bash
git clone <URL_DO_REPOSITORIO>
cd <NOME_DO_REPOSITORIO>
```

### 2. Instalar as dependências

```bash
pip install -r requirements.txt
```

### 3. Configurar o Kaggle

Configure a autenticação da API do Kaggle conforme as credenciais disponíveis em sua conta.

Depois, faça o download da base:

```bash
python -m kaggle datasets download \
  -d mlg-ulb/creditcardfraud \
  -p data \
  --unzip
```

### 4. Executar a análise

Abra:

```text
notebooks/01_exploracao.ipynb
```

e execute as células do notebook em sequência.

---

## Objetivos de aprendizado

Este projeto foi utilizado para praticar conceitos relacionados a:

* aprendizado não supervisionado;
* detecção de anomalias;
* tratamento de dados desbalanceados;
* pré-processamento de dados;
* redes neurais;
* avaliação de modelos;
* análise de fraude financeira;
* utilização de métricas Precision-Recall.

---

## Possíveis melhorias

Como continuidade do estudo, algumas possibilidades seriam:

* testar outros algoritmos de detecção de anomalias;
* ajustar os hiperparâmetros dos modelos;
* comparar diferentes arquiteturas de Autoencoder;
* avaliar diferentes estratégias de normalização;
* investigar técnicas específicas para conjuntos altamente desbalanceados;
* analisar os falsos positivos e falsos negativos obtidos.

---

## Licença e dados

Este projeto possui finalidade **educacional e experimental**.

O dataset utilizado pertence à sua respectiva fonte e deve ser obtido de acordo com os termos de uso estabelecidos pelo Kaggle.

---
