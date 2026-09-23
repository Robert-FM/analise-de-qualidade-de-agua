# 💧 Análise de Potabilidade da Água

Projeto de análise exploratória e classificação da potabilidade da água. O fluxo usa um conjunto de dados com características físico-químicas da água, remove registros duplicados e valores ausentes, compara modelos de classificação e salva o melhor modelo otimizado em um arquivo `.pkl`.

## 🎯 Objetivo

O projeto classifica amostras como potáveis ou não potáveis a partir da variável `Potability` no conjunto bruto. O processamento é realizado em notebooks e está dividido em análise e preparação dos dados, além de treinamento, otimização e avaliação de modelos de classificação.

## ✨ Funcionalidades

- leitura do conjunto bruto `water_potability.csv`;
- remoção de registros duplicados e linhas com valores ausentes;
- renomeação das colunas para nomes usados na análise;
- análise de informações, valores ausentes, estatísticas, correlação e distribuições;
- treinamento de Random Forest, Gradient Boosting, XGBoost, LightGBM e CatBoost;
- comparação por acurácia, precisão, recall, F1-score e ROC-AUC;
- otimização de hiperparâmetros com `RandomizedSearchCV` e validação cruzada estratificada repetida;
- geração de matrizes de confusão e relatório de classificação;
- serialização do melhor pipeline em `depoly/melhor_modelo_potabilidade.pkl`.

## 🛠️ Tecnologias utilizadas

- Python 3.14;
- pandas e NumPy para manipulação dos dados;
- Matplotlib, Seaborn e Plotly para visualização;
- missingno para inspeção de valores ausentes;
- scikit-learn para divisão dos dados, pipelines, validação, métricas e modelos;
- CatBoost, LightGBM e XGBoost para modelos de classificação;
- joblib para salvar o modelo treinado;
- uv para gerenciamento do ambiente e das dependências;
- Jupyter Notebook para execução das análises.

As dependências e suas versões mínimas estão declaradas em [pyproject.toml](pyproject.toml), e o ambiente está fixado em [uv.lock](uv.lock).

## 📁 Estrutura do projeto

```text
analise-agua/
├── analise-dados/
│   └── analise-dados.ipynb
├── data/
│   ├── 01-raw/
│   │   └── .gitkeep
│   └── 02-processed/
│       └── .gitkeep
├── depoly/
│   └── .gitkeep
├── modelos/
│   └── ml-analise.ipynb
├── .gitignore
├── .python-version
├── pyproject.toml
├── README.md
└── uv.lock
```

Os diretórios `data/01-raw`, `data/02-processed` e `depoly` são mantidos no repositório por meio de `.gitkeep`. Os arquivos de dataset (`.csv`), o modelo gerado (`.pkl`) e os logs temporários do CatBoost são ignorados pelo Git para evitar o envio de artefatos e dados ao repositório.

Localmente, `data/01-raw` deve conter `water_potability.csv`. O notebook de análise gera `data/02-processed/dados-agua-tratados.csv`, e o notebook de modelagem grava o modelo selecionado em `depoly/melhor_modelo_potabilidade.pkl`.

## 📋 Dados utilizados

O arquivo bruto contém as variáveis `ph`, `Hardness`, `Solids`, `Chloramines`, `Sulfate`, `Conductivity`, `Organic_carbon`, `Trihalomethanes`, `Turbidity` e `Potability`, que é a variável-alvo binária.

Durante o tratamento, as colunas são renomeadas para facilitar a análise. O arquivo processado usa `potabilidade` como variável-alvo e contém apenas registros sem valores ausentes após a limpeza realizada pelo notebook.

## 📦 Pré-requisitos

- Python 3.14, conforme `.python-version` e `pyproject.toml`;
- uv, para reproduzir o ambiente definido pelo projeto;
- Jupyter Notebook ou outra interface compatível para executar os arquivos `.ipynb`.

## 📦 Instalação

### ⚡ Com uv

#### Instalar o `uv`

Windows PowerShell:

```powershell
powershell -ExecutionPolicy ByPass -c "irm https://astral.sh/uv/install.ps1 | iex"
```

Linux/macOS:

```bash
curl -LsSf https://astral.sh/uv/install.sh | sh
```

Feche e abra o terminal novamente, se necessário, e confirme a instalação:

```bash
uv --version
```

#### Criar e sincronizar o ambiente

Na raiz do projeto, execute:

```bash
uv sync
```

O comando usa a versão indicada em `.python-version`, cria `.venv` automaticamente e instala as dependências definidas no projeto. No fluxo normal, não é necessário executar `pip install`.

Para atualizar explicitamente o lockfile conforme as restrições do `pyproject.toml`, execute:

```bash
uv lock
uv sync
```

#### Verificar a instalação

```bash
uv run python --version
uv run python -c "import pandas, sklearn; print('Dependências carregadas com sucesso')"
```

#### Executar os notebooks

O `uv run` executa comandos dentro do ambiente do projeto sem exigir ativação manual. Como o Jupyter não é uma dependência do pacote, instale-o no ambiente virtual:

```bash
uv pip install jupyter ipykernel
```

Depois, execute:

```bash
uv run jupyter notebook
```

Se preferir ativar o ambiente antes de abrir os notebooks:

Windows PowerShell:

```powershell
.venv\Scripts\Activate.ps1
jupyter notebook
```

Linux/macOS:

```bash
source .venv/bin/activate
jupyter notebook
```

Se o ambiente não aparecer como kernel no Jupyter, registre-o com:

```bash
uv run python -m ipykernel install --user --name analise-agua --display-name "Python (analise-agua)"
```

### 🐍 Com pip

```bash
python -m venv .venv
```

Windows PowerShell:

```powershell
.venv\Scripts\Activate.ps1
```

Linux/macOS:

```bash
source .venv/bin/activate
```

Depois, instale o projeto pelo `pyproject.toml`:

```bash
python -m pip install .
```

Não existe `requirements.txt` no repositório. O `pyproject.toml` também não declara Jupyter como dependência do pacote; instale-o separadamente se necessário, por exemplo:

```bash
python -m pip install jupyter
```

## ▶️ Como executar

O projeto não possui um script ou comando de CLI configurado no `pyproject.toml`. A execução é feita pelos notebooks, que devem ser iniciados a partir de suas respectivas pastas porque usam caminhos relativos.

Para abrir os notebooks com o Jupyter:

```bash
cd analise-dados
uv run jupyter notebook
```

ou:

```bash
cd modelos
uv run jupyter notebook
```

Os notebooks usam os caminhos `../data/01-raw`, `../data/02-processed` e `../depoly`.

### 1. Preparar os dados

Abra [analise-dados/analise-dados.ipynb](analise-dados/analise-dados.ipynb) e execute as células em ordem. O notebook lê `data/01-raw/water_potability.csv`, remove duplicidades, renomeia as colunas, inspeciona os dados, remove linhas com valores ausentes e salva `data/02-processed/dados-agua-tratados.csv`.

### 2. Treinar e selecionar o modelo

Abra [modelos/ml-analise.ipynb](modelos/ml-analise.ipynb) e execute as células em ordem. O notebook lê o arquivo tratado, separa as variáveis explicativas da variável `potabilidade`, divide os dados em treino e teste com estratificação, treina e compara os modelos, otimiza quatro deles com `RandomizedSearchCV`, avalia os melhores estimadores, seleciona o modelo com melhor F1-score e salva o pipeline em `../depoly/melhor_modelo_potabilidade.pkl`.

O arquivo `.pkl` é um artefato gerado pelo notebook. O projeto não contém uma API ou um script separado para executar inferências com ele.

## 📊 Avaliação

O notebook calcula acurácia, precisão, recall, F1-score e ROC-AUC no conjunto de teste. Também gera matrizes de confusão, relatório de classificação e tabelas de comparação dos modelos.

Não há métricas finais documentadas em arquivos separados; por isso, este README não atribui valores específicos ao desempenho dos modelos.

## ✅ Testes

O projeto não contém testes automatizados. A verificação disponível ocorre durante a execução dos notebooks, por meio das inspeções dos dados, das métricas e das matrizes de confusão.

## ⚠️ Limitações observáveis

- o fluxo de execução está implementado em notebooks, sem uma CLI ou aplicação de inferência;
- a seleção do melhor modelo depende da execução do notebook e dos dados disponíveis;
- o diretório do artefato chama-se `depoly`, conforme a estrutura atual do projeto;
- não há configuração de API, dashboard, container ou pipeline de CI/CD no repositório.

## 👨‍💻 Autor

**Robert Melo**

🔗 LinkedIn: [linkedin.com/in/robertdemelo](https://www.linkedin.com/in/robertdemelo/)

🐍 Python | pandas | scikit-learn | CatBoost | LightGBM | XGBoost | Análise de dados
