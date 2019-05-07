---
title: "Funções básicas do R para jornalistas - 1"
author: "Gabriela Caesar"
date: "2019-05-02"
output: html_document
---


## Tutorial 1:

![](https://cdn-images-1.medium.com/max/1200/1*xvzhZtBfl3hqnF0AMdoIUA.gif)

Este caderno mostra as funções básicas da linguagem de programação R que podem ser úteis para jornalistas. Anteriormente, eu fiz tutoriais com o [básico do Google Spreadsheet para jornalista](https://medium.com/@gabrielacaesar/o-b%C3%A1sico-de-google-spreadsheet-para-jornalistas-fun%C3%A7%C3%B5es-matem%C3%A1ticas-e3b87e5371d6).

Caso você não tenha o RStudio na sua máquina, por favor siga o passo a passo deste tutorial (em inglês) para [Mac](https://medium.com/@GalarnykMichael/install-r-and-rstudio-on-mac-e911606ce4f4) e [Windows](https://medium.com/@GalarnykMichael/install-r-and-rstudio-on-windows-5f503f708027). Também recomendo que você baixe um editor de texto, como o [Sublime](https://www.sublimetext.com/3).

Cada bloco de código, também chamado de "chunk", tem uma função diferente. Para criar um arquivo `"R Markdown"`, assim como este caderno, abra o RStudio, clique em `"File" > "New File" > "R Markdown"`. Ou, se não for escrever muito, crie um `"R Script"`.

Para rodar o código, selecione o trecho e clique em `command + enter` no Mac (ou `CTRL + enter` no Windows). No `"R Markdown"`, também é possível clicar na seta verde à direita.

São, no total, três cadernos de R para jornalistas:

* Instalação, leitura e verificação (este post);   
* Limpeza, renomeações e modificações (em breve);   
* Análise de dados e criação de gráficos (em breve).   

### Instalar bibliotecas
```{r, message = FALSE}
install.packages("data.table")
install.packages("tidyverse")
install.packages("ggplot2")
```

## Carregar bibliotecas
```{r, message = FALSE, warning = FALSE, error = FALSE}
library(data.table)
library(tidyverse)
library(ggplot2)
```

## Importar o arquivo
No caso, nós escolhemos o nome "cota_senado" para o arquivo que hospedamos no site [GitHub Gist](https://gist.github.com/). Assim, não precisamos trabalhar com um arquivo local, que só estaria disponível para a nossa máquina.

Caso nós fôssemos usar um arquivo que está no nosso computador, nós teríamos que ver onde ele está e informar esse caminho (path) para o RStudio. Ou teríamos de clicar em `"File" > "Import Dataset"` e achar o arquivo.
```{r}
cota_senado <- fread("https://gist.githubusercontent.com/gcaesar27/5faede8c1c6ffc82c7145dc3ececcbfe/raw/f3192ff17214c3c5d8eca4ebad42ba6f70d409aa/cota-senado-30-abril-2019")
```

Esta não é a única forma de importar o arquivo. Há várias formas. Abaixo, por exemplo, nós já informamos os nomes das colunas (`col.names`), bem como o separador de colunas (`sep`), o cabeçalho (`header`) e a codificação (`encoding`).

Observação: nós usamos outro nome (cota_senado2) para não sobrescrever o arquivo anterior.
```{r}
cota_senado2 <- fread("https://gist.githubusercontent.com/gcaesar27/5faede8c1c6ffc82c7145dc3ececcbfe/raw/f3192ff17214c3c5d8eca4ebad42ba6f70d409aa/cota-senado-30-abril-2019", sep = ";", header = TRUE, encoding = "UTF-8", col.names = c("ano", "mes", "senador", "categoria", "cnpj_cpj", "empresa", "n_documento", "data", "detalhamento", "valor_reembolso"))
```

E ainda com outra biblioteca (`read.table`). Perceba que nós estamos importando um arquivo CSV, mas há outras bibliotecas que importam arquivos Excel, por exemplo, ou outros formatos.
```{r}
cota_senado3 <- read.csv("https://gist.githubusercontent.com/gcaesar27/5faede8c1c6ffc82c7145dc3ececcbfe/raw/f3192ff17214c3c5d8eca4ebad42ba6f70d409aa/cota-senado-30-abril-2019")
```

## Ver a estrutura do arquivo

Abaixo, nós vemos a classe e o nome de cada coluna e também temos noções de como é o nosso arquivo. Ele tem 10 colunas e 4.477 linhas.

* int significa "inteiro" (ou seja, número)   
* chr significa "character" (ou seja, texto)   

```{r}
str(cota_senado)
```

## Verificar o arquivo
A função `summary()` é bastante interessante para usar com arquivos que tenham números. Ela reúne informações básicas sobre a coluna, como valor mínimo, valor máximo, média, mediana, primeiro quartil e terceiro quartil. Ao mesmo tempo, em colunas de texto, ela mostra o tamanho da coluna, que abaixo tem 4.477 linhas.

```{r}
summary(cota_senado)
```
## Verificar a classe de uma coluna

Abaixo, usamos `typeof()` para saber qual é a classe da coluna indicada. 

A coluna "VALOR_REEMBOLSO" do arquivo "cota_senado" foi considerada uma coluna de texto. Isso é algo em que futuramente precisamos mexer.
```{r}
typeof(cota_senado$VALOR_REEMBOLSADO)
```

## Ver os nomes das colunas do arquivo

A função `colnames()` nos informa quais são os nomes das colunas do nosso arquivo. Também podemos usar essa função para renomear as colunas ou apenas determinada coluna. No caso, por exemplo, faríamos assim: `colnames(cota_senado)[1] <- "year"`
```{r}
colnames(cota_senado)
```

## Ver as primeiras linhas do arquivo
Por padrão, a função `head()` mostra as seis primeiras linhas. Se quiser ver mais linhas ou menos linhas, faça assim: `head(cota_senado, 15)` ou `head(cota_senado, 3)`.

```{r}
head(cota_senado)
```

## Ver as últimas linhas do arquivo
A função `tail()` funciona da mesma forma. Ela mostra as seis últimas linhas do arquivo. Também podemos indicar o número de linhas que queremos ver.
```{r}
tail(cota_senado)
```
```{r}
tail(cota_senado, 3)
```
