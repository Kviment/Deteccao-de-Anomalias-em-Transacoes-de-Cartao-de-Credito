# Detecção de Anomalias em Transações de Cartão de Crédito

Estudo experimental sobre identificação de transações potencialmente fraudulentas por meio de técnicas de detecção de anomalias.

O projeto compara dois métodos de aprendizado de máquina: Isolation Forest e Autoencoder. A principal característica da abordagem é não utilizar a classificação de fraude durante o treinamento dos modelos.

## 1. Proposta

Fraudes representam uma pequena parcela das operações realizadas com cartões. Essa característica dificulta o uso de métodos tradicionais de classificação, principalmente quando novos padrões de fraude ainda não possuem exemplos rotulados.

Neste projeto, os algoritmos são utilizados para aprender características do comportamento considerado normal e atribuir pontuações maiores às operações que apresentam maior divergência desse padrão.

A coluna `Class` permanece separada durante o treinamento e é utilizada posteriormente para medir a qualidade das detecções.

## 2. Base de dados

Foi utilizado o conjunto de dados Credit Card Fraud Detection, disponibilizado pela ULB no Kaggle.

Características principais:

| Informação                      |            Valor |
| ------------------------------- | ---------------: |
| Total de operações              |          284.807 |
| Fraudes identificadas           |              492 |
| Variáveis preditoras            |               30 |
| Percentual aproximado de fraude |            0,17% |
| Período                         | Setembro de 2013 |

As variáveis `V1` até `V28` são atributos transformados por PCA. Também estão presentes as colunas `Time` e `Amount`.

Fonte: https://www.kaggle.com/datasets/mlg-ulb/creditcardfraud

## 3. Organização

```text
.
├── data/
├── notebooks/
│   └── analise_transacoes.ipynb
├── src/
│   ├── data_processing.py
│   ├── anomaly_models.py
│   └── metrics.py
├── requirements.txt
├── .gitignore
└── README.md
```

## 4. Tratamento das informações

O processamento da base contempla:

* carregamento dos registros;
* remoção de duplicidades;
* criação de informações relacionadas ao horário das operações;
* transformação das informações temporais em variáveis cíclicas;
* aplicação de RobustScaler ao valor das transações;
* separação das operações normais para treinamento do Autoencoder.

O atributo `Class` não é utilizado como entrada dos modelos.

## 5. Estratégias utilizadas

### Isolation Forest

O Isolation Forest trabalha identificando observações que apresentam maior facilidade de isolamento em relação ao conjunto de dados.

A pontuação produzida pelo algoritmo é convertida em um indicador de anormalidade, permitindo ordenar as transações de acordo com seu grau de divergência.

### Autoencoder

O Autoencoder utiliza uma rede neural para aprender uma representação compacta das transações normais.

A rede possui uma etapa de codificação e outra de reconstrução:

```text
Entrada
  ↓
18 neurônios
  ↓
8 neurônios
  ↓
18 neurônios
  ↓
Saída
```

Durante a avaliação, o erro entre a entrada original e sua reconstrução é utilizado como indicador de anomalia.

## 6. Avaliação dos modelos

Devido ao forte desbalanceamento da base, a análise não utiliza somente acurácia.

São consideradas métricas como:

* Precision;
* Recall;
* F1-score;
* Average Precision.

A Average Precision recebe atenção especial por resumir o desempenho ao longo da curva Precision-Recall.

## 7. Resultados

Os experimentos realizados anteriormente com essa base apresentaram:

| Técnica          |    AP |
| ---------------- | ----: |
| Isolation Forest | 0,134 |
| Autoencoder      | 0,304 |

Nesse cenário, o Autoencoder obteve o melhor resultado entre as duas abordagens.

Os valores podem variar quando o projeto é executado novamente, principalmente devido a alterações nos parâmetros, processamento dos dados e treinamento da rede neural.

## 8. Instalação

Clone o repositório:

```bash
git clone https://github.com/Kviment/Deteccao-de-Anomalias-em-Transacoes-de-Cartao-de-Credito.git
cd Deteccao-de-Anomalias-em-Transacoes-de-Cartao-de-Credito
```

Instale as bibliotecas necessárias:

```bash
pip install -r requirements.txt
```

## 9. Obtendo os dados

A base deve ser baixada separadamente do Kaggle.

Com a API configurada, utilize:

```bash
python -m kaggle datasets download \
    -d mlg-ulb/creditcardfraud \
    -p data \
    --unzip
```

O arquivo resultante deve ficar dentro de:

```text
data/
```

Por exemplo:

```text
data/creditcard.csv
```

## 10. Execução

A análise principal pode ser realizada pelo notebook:

```text
notebooks/analise_transacoes.ipynb
```

Execute as etapas na sequência para realizar o carregamento, tratamento, treinamento e avaliação.

## 11. Conhecimentos explorados

O projeto permite trabalhar conceitos de:

* aprendizado não supervisionado;
* detecção de anomalias;
* processamento de dados;
* redução de dimensionalidade;
* redes neurais;
* métricas para bases desbalanceadas;
* análise de transações financeiras;
* interpretação de resultados de modelos.

## 12. Próximos experimentos

Algumas possibilidades de continuidade incluem:

* testar diferentes parâmetros do Isolation Forest;
* modificar a dimensão do espaço latente do Autoencoder;
* comparar outras técnicas de detecção de anomalias;
* estudar diferentes métodos de tratamento do desbalanceamento;
* analisar a influência das variáveis temporais;
* comparar diferentes critérios para definir o limite de anomalia.

## 13. Observação

Este repositório possui finalidade educacional. Os resultados apresentados não devem ser interpretados como um sistema de detecção de fraude pronto para utilização em ambiente financeiro real.
