# Triagem Inteligente na Área da Saúde — Machine Learning + Lógica Fuzzy

Trabalho Final Prático da disciplina de Inteligência Artificial (Tema 3 — Diagnóstico Inteligente na Área da Saúde). O projeto classifica a condição clínica de pacientes com base em sinais vitais, combinando um modelo de **Machine Learning** (Árvore de Decisão) com um **Sistema de Inferência Fuzzy**, comparando as decisões das duas abordagens (Abordagem A — Comparação).

- **Disciplina:** Inteligência Artificial — Lógica Fuzzy e Machine Learning
- **Professor:** William Malvezzi
- **Integrantes:**
  - Mikael Regetz
  - Yasmin Rezende
  - Matheus Brandao
  - Matheus de Castro
- **Arquivo principal:** `Projeto_Final_IA.ipynb`

---

## Como executar

### Opção 1 — Google Colab (recomendada)

1. Acesse https://colab.research.google.com/drive/1nr0zQ0ts4rHZu4-77VREkmOt9c6EScuN?usp=sharing.
2. Vá em **Arquivo → Fazer upload de notebook** e selecione `Projeto_Final_IA.ipynb`.
3. No menu, clique em **Ambiente de execução → Executar tudo** (ou `Ctrl+F9`).
4. Pronto. O notebook instala automaticamente a dependência `scikit-fuzzy` e executa todas as células em ordem.

Ao rodar, o notebook gera e faz download automático de `base_triagem.csv` (base de dados simulada). Os demais arquivos gerados (gráficos e modelo serializado) ficam disponíveis no ambiente local do Colab e são baixados ao executar a **célula de exportação final**.

### Opção 2 — Jupyter Notebook (local)

Requer Python 3.9 ou superior. Instale as dependências:

```bash
pip install numpy pandas matplotlib seaborn scikit-learn scikit-fuzzy
```

Em seguida, abra e execute o notebook:

```bash
jupyter notebook Projeto_Final_IA.ipynb
```

> A célula `!pip install scikit-fuzzy --quiet` no início é específica do Colab. Ao rodar localmente, ela simplesmente reinstala a biblioteca sem causar problemas.

---

## Bibliotecas utilizadas

| Biblioteca | Função no projeto |
|---|---|
| `numpy` | Geração dos dados sintéticos e operações numéricas |
| `pandas` | Manipulação do DataFrame e exportação do CSV |
| `matplotlib` / `seaborn` | Gráficos da análise exploratória e dos resultados |
| `scikit-learn` | Árvore de Decisão, pré-processamento e métricas de avaliação |
| `scikit-fuzzy` | Construção e execução do sistema de inferência fuzzy |
| `pickle` | Serialização do modelo treinado para reutilização |

---

## Estrutura do notebook

O notebook está dividido em quatro partes, executadas de cima para baixo:

- **Parte 1 — Base de dados e análise exploratória:** geração de uma base sintética com 300 pacientes distribuídos em três classes (Normal, Atenção, Risco), com valores de temperatura, pressão arterial e frequência cardíaca baseados em referências da OMS e da AHA. Inclui estatísticas descritivas, gráfico de distribuição de classes, boxplots por variável e matriz de correlação.

- **Parte 2 — Machine Learning:** pré-processamento (codificação da variável-alvo com `LabelEncoder`, divisão 80/20 com estratificação, normalização com `StandardScaler`), treinamento de uma Árvore de Decisão (`max_depth=4`, critério Gini) e avaliação com acurácia, matriz de confusão, relatório de classificação, visualização da árvore e importância das variáveis.

- **Parte 3 — Sistema Fuzzy:** três variáveis de entrada (temperatura, pressão arterial e frequência cardíaca), uma variável de saída (índice de risco de 0 a 100), funções de pertinência triangulares e conjunto de regras SE…ENTÃO. O índice numérico de saída é convertido em classe (Normal / Atenção / Risco) por limiar.

- **Parte 4 — Comparação (Abordagem A):** aplicação das duas abordagens ao mesmo conjunto de teste, análise de concordância e divergências, seis gráficos comparativos e sumário para o relatório.

> **Importante:** execute as células em ordem, sem pular. Cada parte depende dos resultados da anterior.

---

## Base de dados

A base é **simulada** com distribuições normais, prática permitida pelo enunciado quando não há dados reais disponíveis. Os parâmetros de geração seguem referências clínicas:

| Classe | Registros | Temperatura | Pressão (mmHg) | Freq. Cardíaca (bpm) |
|---|---|---|---|---|
| Normal | 120 | ~36,7°C | ~110 | ~75 |
| Atenção | 100 | ~37,8°C | ~135 | ~100 |
| Risco | 80 | ~39,2°C | ~160 | ~118 |

Os valores são limitados a faixas fisiologicamente plausíveis (temperatura 35–42°C, pressão 70–200 mmHg, frequência 40–160 bpm). A semente `np.random.seed(42)` garante reprodutibilidade total.

---

## Arquivos gerados

Ao executar o notebook completo, são produzidos os seguintes arquivos:

| Arquivo | Conteúdo |
|---|---|
| `base_triagem.csv` | Base de dados sintética (300 registros) |
| `modelo_arvore.pkl` | Modelo serializado (Árvore + Scaler + Encoder) |
| `funcoes_pertinencia.png` | Gráficos das funções de pertinência do sistema fuzzy |
| `resultado_fuzzy.png` | Resultado do sistema fuzzy sobre o conjunto de teste |
| `comparacao_ml_fuzzy.png` | Painel comparativo ML × Fuzzy (6 subgráficos) |

---

## Observações

- O sistema fuzzy utiliza as três variáveis de sinais vitais, assim como o modelo de Machine Learning, o que torna a comparação entre as abordagens direta e consistente.
- A célula de **verificação final** confirma os objetos em memória e os arquivos gerados, e inclui uma demonstração ao vivo com valores configuráveis — útil para a apresentação.
- A célula de **exportação final** baixa todos os artefatos de uma vez antes de encerrar a sessão do Colab.
