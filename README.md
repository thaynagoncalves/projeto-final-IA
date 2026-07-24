# Predição da Retenção de Habilidades de Estudantes com Aprendizado de Máquina

## Objetivo

Este projeto tem como objetivo desenvolver e comparar modelos de regressão capazes de estimar a variável `Skill_Retention_Score`, que representa a pontuação de retenção de habilidades de estudantes.

A previsão é realizada a partir de informações acadêmicas, hábitos de estudo tradicional, uso de ferramentas de inteligência artificial generativa, habilidade de engenharia de prompts, dependência percebida de IA, ansiedade durante provas e outras características presentes no conjunto de dados.

O projeto inclui compreensão dos dados, análise exploratória, pré-processamento, divisão entre treino e teste, treinamento dos modelos, validação cruzada, avaliação e discussão dos resultados.

---

## Integrantes

- Luan Dantas Melo
- Lucas Lima Celino
- Thayná Luzia Gonçalves Lima

---

## Fonte dos dados

O conjunto de dados utilizado foi obtido no Kaggle:

**AI Impact on Students**

Link: https://www.kaggle.com/datasets/laveshjadon/ai-impact-on-students

O dataset contém 50.000 registros e 16 atributos relacionados ao desempenho acadêmico, uso de IA, hábitos de estudo e características dos estudantes.

---

## Tipo da tarefa

Este projeto utiliza uma tarefa de **regressão supervisionada**.

A variável-alvo é:

```text
Skill_Retention_Score
```

Como essa variável é numérica e contínua, os modelos devem prever um valor, e não uma classe.

---

## Notebook no Google Colab

O notebook pode ser aberto diretamente no Google Colab:

**Link do Colab:**  
https://colab.research.google.com/github/thaynagoncalves/projeto-final-IA/blob/main/ProjetoFInal_IA.ipynb

### Como executar

1. Abra o link do Colab.
2. Acesse o menu **Ambiente de execução**.
3. Clique em **Executar tudo**.
4. Aguarde o carregamento do dataset e a execução das células.
5. Consulte as métricas, os gráficos e a discussão no final do notebook.

---

## Organização dos arquivos

```text
projeto-machine-learning/
│
├── README.md
├── ProjetoFinal_IA.ipynb
├── ai_student_impact_dataset.csv
└── imagens/
    ├── distribuicao_alvo.png
    ├── matriz_correlacao.png
    ├── horas_IA_retencao.png
    ├── estudo_tradicional_retencao.png
    ├── ano_estudo_retencao.png
    ├── boxplot_alvo.png
    ├── engenharia_prompts_retencao.png
    ├── media_retencao_uso_IA.png
    ├── comparacao_modelos.png
    ├── valores_reais_previstos.png
    ├── distribuicao_residuos.png
    └── importancia_atributos.png
```

### Descrição

- `README.md`: documentação geral do projeto.
- `projeto_final_regressao_skill_retention.ipynb`: notebook principal.
- `ai_student_impact_dataset.csv`: conjunto de dados.
- `imagens/`: gráficos exportados utilizados na documentação.

---

## Variável-alvo e atributos

A variável-alvo é `Skill_Retention_Score`.

As seguintes colunas foram removidas dos atributos preditivos:

- `Student_ID`: é apenas um identificador;
- `Post_Semester_GPA`: informação obtida após o semestre, podendo causar vazamento temporal;
- `Burnout_Risk_Level`: avaliação final relacionada ao estudante, que poderia tornar a previsão artificialmente otimista.

---

## Pré-processamento

O pré-processamento foi implementado com `Pipeline` e `ColumnTransformer`.

Foram aplicadas as seguintes transformações:

- preenchimento de valores ausentes numéricos com a mediana;
- preenchimento de valores ausentes categóricos com a moda;
- codificação de categorias com `OneHotEncoder`;
- padronização das variáveis numéricas com `StandardScaler`;
- tratamento de categorias desconhecidas com `handle_unknown="ignore"`.

Essa organização evita que informações do conjunto de teste sejam usadas durante o treinamento.

---

## Modelos utilizados

### DummyRegressor

Utilizado como baseline. Ele prevê sempre a mediana da variável-alvo e serve como referência mínima.

### Regressão Linear

Modelo linear utilizado para identificar relações lineares entre os atributos e o alvo.

### Árvore de Decisão

Modelo capaz de representar relações não lineares por meio de divisões sucessivas dos dados.

### Random Forest

Modelo de conjunto que combina várias árvores de decisão para aumentar a estabilidade e a capacidade de generalização.

---

## Métricas de avaliação

Os modelos foram comparados com validação cruzada de cinco divisões.

As métricas utilizadas foram:

- **MAE**: erro absoluto médio;
- **MSE**: erro quadrático médio;
- **RMSE**: raiz do erro quadrático médio;
- **R²**: proporção da variação do alvo explicada pelo modelo.

Para MAE, MSE e RMSE, valores menores são melhores. Para R², valores maiores indicam melhor desempenho.

---

## Principais resultados


| Modelo | MAE | MSE | RMSE | R² |
|---|---:|---:|---:|---:|
| DummyRegressor | 10.706 | 176.488 | 13.285 | -0.000 |
| Regressão Linear | 9.762 | 146.220 | 12.092 | 0.171 |
| Árvore de Decisão | 10.178 | 160.741 | 12.678 | 0.089 |
| Random Forest | 9.660 | 143.541 | 11.981 | 0.186 |

### Melhor modelo

**Modelo selecionado:** `Random Forest`

**Justificativa:** `O modelo Random Forest apresentou a melhor capacidade de aprendizado e generalização sobre o conjunto de teste. Ele obteve o menor Erro Quadrático Médio (RMSE) e o maior coeficiente de determinação ($R^2$), demonstrando alta precisão em capturar relações não lineares e interações complexas entre as variáveis explicativas sem incorrer em overfitting severo.`

- Todos os modelos avançados superaram a linha de base. O baseline apresentou RMSE elevado e `$R^2$` baixo. Os modelos baseados em ensemble reduziram significativamente o erro médio das predições em comparação.
- Os maiores erros de predição concentraram-se nas seguintes situações: Valores extremos da variável alvo, o modelo tende a subestimar valores muito altos e superestimar valores extremamente baixos. Registros com dados ruidosos ou incompletos, casos em que havia combinações raras de características não observadas frequentemente no conjunto de treinamento.
---

## Limitações

- **Sensibilidade a Valores Extremos:** Embora modelos baseados em árvores sejam mais robustos, a presença de valores extremos na variável alvo ainda distorce as métricas baseadas em erro quadrático (`$MSE$` e `$RMSE$`).
- **Dependência de Dados Históricos:** O modelo assume que o comportamento futuro seguirá a mesma distribuição dos dados de treino; mudanças de padrão ou sazonalidades não mapeadas podem degradar o desempenho.
- **Interpretabilidade:** Modelos como Random Forest possuem menor interpretabilidade direta se comparados a modelos lineares simples, exigindo técnicas auxiliares para explicação individual.


---

## Divisão das contribuições

### Thayná Luzia Gonçalves Lima

- Preparação do ambiente
- Carregamento dos dados
- Descrição do problema
- Compreensão dos dados

### Lucas Lima Celino

- Análise exploratória
- Pré-processamento
- Separação dos dados
- Documentação

### Luan Dantas Melo

- Modelagem
- Avaliação e discussão
- Conclusão
- Edição do vídeo


---

## Vídeo de apresentação

**Link do vídeo:**  
[ASSISTIR AO VÍDEO](https://drive.google.com/file/d/1jOmzNrqxwx3F0Edzk52DBdWvBHlZakWt/view?usp=sharing)

---

## Declaração de uso de ferramentas de inteligência artificial

Ferramentas de inteligência artificial foram utilizadas como apoio na organização do projeto, revisão textual e explicação de conceitos.

Todos os integrantes revisaram, executaram, testaram e validaram o conteúdo, assumindo responsabilidade pelos resultados, interpretações e apresentação final.

---



