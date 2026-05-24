# Limpeza-de-dados-no-R
Repositório com exemplos práticos de limpeza de dados no R utilizando um conjunto de dados de funcionários.

---

# Pacotes utilizados

```r
pacotes <- c("tidyverse", "readr", "readxl", "haven")  # Uma forma mais prática de instalar vários pacotes ao mesmo tempo é armazenando os nomes em um vetor.
install.packages(pacotes)
```

```r
library(tidyverse)
library(readr)
library(readxl)
library(haven)
```

---

# 1. Importação dos dados

## Importação de arquivo CSV

```r
dados_funcionarios <- read_csv("dados/dados_de_funcionarios.csv")
```

## Importação de arquivo Excel

```r
dados_excel <- read_excel("dados/dados_de_funcionarios.xlsx")
```

## Importação de arquivo SPSS

```r
dados_spss <- read_sav("dados/dados_de_funcionarios.sav")
```

---

# 2. Exploração inicial dos dados

```r
glimpse(dados_funcionarios)
str(dados_funcionarios)
```

---

# 3. Renomeação de variáveis

```r
dados <- dados_funcionarios |> 
  rename(
    id = Employee_ID,
    nome = First_Name,
    sobrenome = Last_Name,
    idade = Age,
    departamento = Department_Region,
    status = Status,
    data_admissao = Join_Date,
    salario = Salary,
    email = Email,
    telefone = Phone,
    desempenho = Performance_Score,
    trabalho_remoto = Remote_Work
  )
```

---

# 4. Seleção de colunas

```r
dados <- dados |> 
  select(-id)
```

---

# 5. Padronização de texto

## Conversão da variável idade para numérica

```r
idade_num <- suppressWarnings(as.numeric(dados$idade))
```

## Identificação de valores inválidos

```r
table(dados$idade[
  is.na(idade_num) | idade_num < 0 | idade_num > 120
])
```

## Correção dos valores

```r
dados <- dados %>% 
  mutate(
    idade = case_when(
      idade == "35 anos" ~ 35,
      idade == "40 anos" ~ 40,
      idade < 0 ~ NA,
      TRUE ~ as.numeric(idade)
    )
  )
```

## Conversão da variável salário

```r
dados$salario <- as.numeric(dados$salario)
```

---

# 6. Padronização de categorias

```r
dados$status <- as.factor(dados$status)
```

---

# 7. Tratamento de valores ausentes

## Verificação dos valores ausentes

```r
colSums(is.na(dados))
```

## Remoção de valores ausentes

```r
dados <- dados |> 
  drop_na()
```

---

# 8. Criação de novas variáveis

```r
dados <- dados |>
  mutate(nome_completo = paste(nome, sobrenome))
```

---

# 9. Remoção de duplicatas

```r
dados[duplicated(dados), ]
```

```r
dados <- dados |>
  distinct()
```

---

# 10. Filtragem dos dados

## Funcionários ativos

```r
funcionarios_ativos <- dados |> 
  filter(status == "Ativo")
```

---

# 11. Exportação dos dados limpos

```r
write_csv(dados, "dados/dados_renomeados.csv")
```
---ores
Estephany Duarte
Emanuel Carneiro
