# Global Solution – MLAM | CitizenTech

### Inclusão digital: o que explica o acesso à internet nos países?

Análise estatística em Python sobre a relação entre **telefonia celular**, **banda larga** e **uso de internet** em 193 países (2017), com distribuição normal, testes de hipóteses, regressões linear e logística e comparação com algoritmos de classificação.

---

## 👥 Integrantes

| Nome | RM |
|---|---|
| Conrado Gracie | RM569157 |
| Renato Sandreschi | RM569156 |

---

## 🎯 Problema

No desafio **CitizenTech** (cidades, campo e comunidades), o problema escolhido é a **inclusão digital**:

> Quais indicadores de infraestrutura de conectividade (celular e banda larga) estão mais associados à parcela da população que usa internet?

Entender essa relação ajuda governos e empresas a decidir **onde investir** para reduzir a exclusão digital.

---

## 📁 Arquivos do repositório

| Arquivo | Descrição |
|---|---|
| `GS_MLAM_CitizenTech.ipynb` | Notebook com todo o código, os gráficos, as saídas e as interpretações |
| `Final.csv` | Base de dados original |
| `README.md` | Este documento |

---

## 🗂️ Base de dados

- **Arquivo:** `Final.csv` · 8.867 linhas · 229 entidades · anos de 1980 a 2020
- **Fonte:** `[PREENCHER: link de onde a base foi baixada, ex.: página no Kaggle]`

| Coluna | Descrição |
|---|---|
| `Entity` / `Code` | País (ou grupo de países) e seu código |
| `Year` | Ano da observação |
| `Cellular Subscription` | Assinaturas de celular por 100 habitantes |
| `Internet Users(%)` | Percentual da população que usa internet |
| `No. of Internet Users` | Número de usuários de internet |
| `Broadband Subscription` | Assinaturas de banda larga por 100 habitantes |

### Limpeza e seleção dos dados

1. Remoção da coluna `Unnamed: 0` (apenas número de linha).
2. Remoção de linhas que **não são países** (regiões, grupos de renda e "World").
3. Seleção do ano de **2017**: cada país aparece uma única vez (observações independentes) e há boa cobertura de dados.
4. Remoção de países com valor `0` em alguma variável, pois nesta base o zero representa **dado ausente** (ex.: 0% de internautas é impossível). Foram removidos 11 países.
5. **Base final: 193 países.**

---

## 🔬 O que foi feito

| # | Etapa | Método |
|---|---|---|
| 1 | Distribuição normal | Probabilidades P(X < média) e P(X > mediana) e classificação dos eventos |
| 2 | Testes de hipóteses | Teste Z unilateral: (a) α = 6%, n = 20; (b) α = 9%, n = 25 |
| 3 | Análise bivariada | Dispersão, covariância, correlação de Pearson, regressão linear simples, R², teste F e previsão |
| 4 | Regressão múltipla | Matrizes de covariância e correlação, equação, R² ajustado, teste F, resíduos e previsão |
| 5 | Regressão logística | Equação, matriz de confusão, acurácia e classificação de novos países |
| 6 | Comparação de modelos | KNN (k = 5) e Árvore de Decisão (profundidade 3), com acurácia, precisão, recall e F1 |

---

## 📊 Principais resultados

**Distribuição normal** (% de usuários de internet): média 54,40%, mediana 58,77%. P(X < média) = 0,5000 e P(X > mediana) = 0,4381.

**Testes Z:** ambos **não rejeitam H0** (a: Z = 0,675 > Z crítico = −1,555; b: Z = 0,473 < Z crítico = 1,341).

**Regressão simples** (banda larga → internet): r = 0,823 · R² = 0,677

```
Internet(%) = 31,385 + 1,586 × Banda larga
```

**Regressão múltipla** (celular + banda larga): R² = 0,738 · R² ajustado = 0,735 · F = 267,7 (crítico = 3,04)

```
Internet(%) = 13,837 + 0,192 × Celular + 1,359 × Banda larga
```

**Regressão logística** (acesso alto = pelo menos 50% da população online):

```
ln(p / (1 − p)) = −4,0916 + 0,0194 × Celular + 0,3010 × Banda larga
```

| Modelo | Acurácia (teste) |
|---|---|
| Regressão logística | 89,7% |
| KNN (k = 5) | 93,1% |
| Árvore de Decisão | 91,4% |
| Referência (chutar a classe mais comum) | 58,6% |

### 💡 Conclusões

- A **banda larga** é a variável mais associada ao uso de internet: sozinha explica cerca de 68% da variação entre países.
- Celular e banda larga **se complementam**: juntas explicam cerca de 74%.
- Para o CitizenTech, priorizar **banda larga fixa** em comunidades pouco conectadas tende a gerar mais inclusão do que apenas ampliar linhas de celular.
- Os três classificadores têm desempenho parecido e bem acima do chute, o que reforça a robustez da conclusão.

---

## 🛠️ Bibliotecas

Python 3.9+ e apenas:

- `pandas` e `numpy`
- `matplotlib` e `seaborn`
- `scikit-learn`: `train_test_split`, `KNeighborsClassifier`, `DecisionTreeClassifier` e métricas de classificação

As funções de distribuição normal, o teste Z, as regressões (linear e logística) e o teste F foram **implementados com NumPy** no próprio notebook (seção *Funções auxiliares*). O p-valor do teste F é obtido por simulação.

---

## ⚠️ Limitações

- Os dados são **por país** e de **um único ano**; não descrevem cidades, zonas rurais ou comunidades específicas.
- Foram excluídos 11 países com dado ausente, o que pode favorecer países que reportam melhor seus dados.
- **Associação não é causalidade:** renda, educação, urbanização e preço dos planos não estão no modelo.
- A normalidade foi assumida conforme o enunciado, mas a variável é limitada entre 0% e 100% e tem distribuição achatada (curtose = −1,22).
- A relação entre banda larga e internet é **curva**, então a reta ultrapassa 100% para valores altos de banda larga; o modelo linear só deve ser usado dentro do intervalo dos dados.
- Os testes Z usam uma amostra aleatória (semente 42); outra amostra poderia levar a outro resultado.
- O conjunto de teste da classificação é pequeno (58 países), então diferenças de um ou dois acertos entre modelos não são conclusivas.
- O limiar de 50% para "acesso alto" é uma escolha do grupo.
