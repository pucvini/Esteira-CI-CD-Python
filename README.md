# Projeto CI/CD - Vinícius

Projeto acadêmico de construção de uma esteira de CI/CD com GitHub Actions.

A estrutura de aplicação, dados, testes e dependências parte do projeto-base fornecido pela disciplina. A implementação do workflow foi construída especificamente para atender às etapas da atividade.

## Estrutura

```text
app/
  pipeline.py

data/
  sales.csv

tests/
  conftest.py
  test_pipeline.py

.github/workflows/
  ci-cd.yml

requirements.txt
```

## CI

O job `validate` executa em `ubuntu-latest` com Python 3.11 e realiza:

1. Checkout do repositório;
2. Instalação das dependências;
3. Lint com Flake8;
4. Verificação de tipagem com Mypy;
5. Verificação de segurança com Bandit;
6. Análise de complexidade com Radon;
7. Testes automatizados com Pytest;
8. Execução do pipeline Python;
9. Publicação dos arquivos gerados como artefato.

## CD

O job `deploy` depende do sucesso do `validate` e é executado somente em push na branch `main`. Ele baixa o artefato aprovado e o promove para uma pasta que representa o ambiente de produção.

## Branches

- `feature/esteira`: execução da validação para desenvolvimento da esteira.
- `main`: execução da validação e, após sucesso, do deploy.

Essa configuração permite demonstrar o fluxo CI/CD solicitado pela atividade.

## Reflexão técnica

### 1. O que pertence ao CI e o que pertence ao CD?

CI corresponde à integração e validação do código: checkout, preparação do ambiente, instalação de dependências, lint, verificações de qualidade, testes e geração do artefato. CD corresponde à promoção do artefato aprovado para um ambiente de entrega, representada neste projeto pelo job `deploy` na branch `main`.

### 2. Qual a diferença entre o script Python e a esteira que o valida?

O script Python executa o processamento dos dados e produz os resultados. A esteira automatiza a validação desse código, garantindo que ele seja testado e analisado antes de o resultado ser promovido para a etapa de entrega.

### 3. Se houvesse um ambiente de staging, em que ponto ele entraria?

O staging entraria depois da validação do CI e antes da promoção para produção. O artefato aprovado poderia ser implantado primeiro em staging para validações adicionais e, somente após essa etapa, promovido para produção.

## Execução local

```bash
python -m venv .venv
.venv\\Scripts\\Activate.ps1
pip install -r requirements.txt
python app/pipeline.py
pytest
```

O pipeline gera `output/summary.csv` e `output/annual_summary.csv`.
