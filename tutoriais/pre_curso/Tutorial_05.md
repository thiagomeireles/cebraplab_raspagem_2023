# Tutorial pré-curso 5: Manipulação de dados em R com *dplyr*

## Pacotes no R

Muitas ações necessárias para atividades diversas executadas não fazem parte da biblioteca básica do R, mas outros desenvolvedores já criaram funções para isso. Em muitos casos, um conjunto de funções é compliado em novas bibliotecas direcionadas para atividades bem específicas. Essas bibliotecas (ou pacotes de funções) são disponibilziadas pela comunidade de R e, após aprovação, vão para um diretório com bibliotecas já testadas, o [CRAN](https://cran.r-project.org/web/packages/policies.html).

Nesse primeiro momento, vamos instalar uma de uma biblioteca chamada *tidyverse*, falaremos um pouco mais dela depois. Assim, nossa primeira ação é realizar o processo de instalação de uma biblioteca. Para isso, basta executarmos o comando abaixo:

```{r, eval=FALSE}
install.packages("tidyverse")
```

No entanto, mesmo com a biblioteca instalada as funções não ficam disponíveis automaticamente. É necessário carregar a biblioteca para torná-las disponíveis. Assim, vamos executar o comando para tornar as funções da biblioteca *tidyverse* disponíveis. Basta executar o comando abaixo:

```{r, message=FALSE}
library(tidyverse)
```

Excelente! Já temos boa parte das funções que precisamos disponíveis para o nosso primeiro tutorial. Vamos utilizá-las logo mais.

## Tidyverse

 O _tidyverse_ é uma compilação de diversas bibliotecas que, grosso modo, compõem uma linguagem "alternativa" dentro do R. Os pacotes mais conhecidos são o _dplyr_ e o _ggplot2_, utilizados, respectivamente, para manipulação e visualização de dados.

Diversas funções que fazem parte do _tidyverse_ serão utilizadas ao longo da semana e serão destacadas quando surgirem. Uma outra forma de checar se ele está instalado é utilizando um processo um pouco mais complexo: pediremos para que o R cheque se o pacote já está instalado e, caso não esteja, realize a instalação. Façamos isso pro *dplyr*.

```{r}
if (!require("dplyr")) install.packages("dplyr"); library(dplyr)
```

Dica: se você "chamar" o pacote _tidyverse_, não precisará chamar *dplyr*, pois a função do _tidyverse_ carrega todos os pacotes que o compõe.

## Introdução ao pacote dplyr

A linguagem de programação, assim com nosso português, passa por transformações. Um desses processos é o desenvolvimento de novas funcionalidades pela comunidade de usuários com pacotes para facilitar nsosa vida. Outras estão relacionadas à própria "gramática" para base de dados, ou seja, na forma como importamos, manipulamos, organizamos e extraímos informações das bases de dados. 

Este tutorial trata da *dplyr*, gramática mais popular para R nos últimos anos. Ela traz novas funções e funcionalidades que facilitam o nosso trabalho. Utilizaremos, para fins didáticos, uma base de dados simples do Bolsa Família com apenas as informações relativas aos saques de janeiro de 2017. É uma base reduzida, extraída de forma aleatória do Portal da Transparência, com apenas 10 mil saques.

```{r}
library(readr)
saques_amostra_201701 <- read_delim("https://raw.githubusercontent.com/thiagomeireles/cebraplab_raspagem_2023/main/dados/saques_amostra_201701.csv",
                                    delim = ";", escape_double = FALSE, 
                                    col_types = cols(`Valor Parcela` = col_character()))

# Explicaremos depois o porquê de usar o valor como character
```

Aqui utilizaremos uma função nova, a *glimpse*, que é aplicável a objetos do tipo *tibble*. Os *tibbles* são apenas um *data\_frame* impresso de forma mais adequada. Vejamos:

```{r}
library(tibble)
glimpse(saques_amostra_201701)
```

A função nos oferece um resumo dos dados, com (1) o nome da variável/vetor; (2) o tipo de dado e (3) as informações das primeiras linhas.

## Renomeando variáveis

Não é raro que ao importar uma base de dados, os nomes das colunas sejam compostos ou com caracteres especiais, como acentos e cedilhas. A verdade é que dá um trabalho danado usar nomes com tais características no R. Imagine ter que se preocupar em colocar acentos ou outros desses caracteres sempre que for "chamar" a variável. Com isso, é muito melhor termos nomes sem espaços (pontos ou subscritos são alternativas para separar um nome composto), dando preferência à utilização apenas de letras minúsculas sem acento, além de números. 

Vamos começar renomeando algumas das variáeis do nosso banco de dados de amostras do Bolsa Família, cujos nomes conseguimos com a função *names*:

```{r}
names(saques_amostra_201701)
```

Após a identificação do nome das variáveis, vamos renomeá-los com uma nova função que está na gramática *dplyr*, a *rename* (um ótimo nome, não?). O primeiro argumento da função é o nome da própria base de dados da qual queremos mudar o nome das variáveis. Depois da primeira vírgula, inserimos todas as modificações de nome as separando, novamente, por vírgulas. É algo como "nome\_novo = nome\_velho" para cada variável que queiramos alterar o nome.

No entanto, caso exista um "espaço" dentro do nome original/antigo da variável, precisamos utilizar uma crase antes e depois desse nome para que o R entenda onde ele começa e onde ele termina. Teríamos um "nome\_novo = `Nome Velho`".

Vejamos as duas formas alterando os nomes das variáveis "UF" e "Nome Município":

```{r}
library(dplyr)
saques_amostra2 <- rename(saques_amostra_201701, uf = UF, munic = `Nome Município`)
```

Vamos renomear agora as variáveis "Código SIAFI Município", "Nome Favorecido", "Valor Parcela", "Mês Competência" e "Data do Saque" como "cod_munic", "nome", "valor", "mes", "data_saque", respectivamente.

```{r}
saques_amostra_201701 <- saques_amostra_201701 %>% 
  rename(cod_munic = `Código SIAFI Município`,
         nome = `Nome Favorecido`, 
         valor = `Valor Parcela`, 
         mes = `Mês Competência`,
         data_saque = `Data do Saque`)
```

## Uma gramática, duas formas

Se observou a resolução do exercício anterior, verá que tem uma sintaxe ligeiramente diferente da que utilizamos para realização da mesma tarefa de renomeação de variáveis. Vamos observar a diferença: 

```{r, eval = TRUE}
saques_amostra_201701 <- saques_amostra_201701 %>%
  rename(uf = UF, munic = `Nome Município`)
```

A principal diferença aqui é o uso do operador "%>%", chamado *pipe*. Com ele, retiramos o nome do *data\_frame* com as variáveis que queremos renomear de dentro da função *rename*. Parece mais uma complicação, mas na verdade ela traz uma grande vantagem em relação à anterior. A utilização de *pipes* permite "emendar" operações de transformações do banco de dados, ou seja, criar tarefas para manipulação do banco de dados em sequência dentro de uma mesma lógica. Por enquanto tenha isso em mente porque vamos retomar isso adiante, mas o resultado é o mesmo para ambas as formas.

## Selecionando colunas

Como falamos no tutorial anterior, existem variáveis que são claramente dispensáveis dos nossos bancos de dados e as "excluímos" para liberar memória em nossas máquinas. No nosso banco, sabemos que á sabemos que "Código Função", "Código Subfunção", "Código Programa" e "Código Ação" não variam entre as linhas, pois todas se referem ao Programa Bolsa Família.

Vamos testar a seleção utilizando apenas as variáveis que já havíamos renomeado.

```{r}
saques_amostra2 <- select(saques_amostra_201701, 
                          uf, munic, cod_munic, nome, valor, mes, data_saque)
```

ou usando o operador %>%, chamado *pipe*,

```{r}
saques_amostra_201701 <- saques_amostra_201701 %>% 
  select(uf, munic, cod_munic, nome, valor, mes, data_saque)
```

## Operador %>% para "emendar" tarefas

A lógica do operador *pipe* é bastante simples, colocando o primeiro argumento da função (nos exemplos, o nome do *data\_frame*) fora e antes da própria função. Vamos tornar isso um pouco mais tangível "traduzindo" para o português de uma forma bastante informal: "Pegue o *data\_frame* x e aplique a ele essa função".

Veremos na sequência que ele permite fazer uma cadeira de operações ("pipeline"), que pode ser traduzida informalmente como: "pegue o *data\_frame* x e aplique a ele essa função; e depois essa; e depois essa; etc.".

A principal vantagem de trabalhar com o %>% é não precisar repetir o nome do *data\_frame* diversas vezes ao aplicarmos um conjunto de operações.

Use o comando _rm_ para deletar a base de dados e abra novamente. Vejamos agora como usamos o operador %>% para "emendar" tarefas:

```{r, include=FALSE}
rm(saques_amostra_201701, saques_amostra2)

saques_amostra_201701 <- read_delim("https://raw.githubusercontent.com/thiagomeireles/cebraplab_raspagem_2023/main/dados/saques_amostra_201701.csv",
                                    delim = ";", escape_double = FALSE, 
                                    col_types = cols(`Valor Parcela` = col_character()))
```

Dica: para "digitar" o %>% no código, pode usar "crtl + shift + m".

```{r}
saques_amostra_201701 <- saques_amostra_201701 %>% 
  rename(uf = UF, munic = `Nome Município`,
         cod_munic = `Código SIAFI Município`, nome = `Nome Favorecido`,
         valor = `Valor Parcela`, mes = `Mês Competência`, data_saque =`Data do Saque`)  %>%
  select(uf, munic, cod_munic, nome, valor, mes, data_saque)
```

Wow! Em uma única sequência de operações alteramos os nomes das variáveis e selecionamos quais queríamos no nosso banco. É uma forma de programar bem mais econômica, limpa e clara. Acredite nisso.

Voltemos agora para os nossos dados. Observando as dimensões da nossa base dados, veremos que ela tem 10 mil linhas, mas apenas 7 colunas agora:

```{r}
dim(saques_amostra_201701)
```

## Transformando variáveis

Quando importamos nosso *data\_frame*, especificamos que a variável valor fosse lida como texto mesmo contendo números. Isso ocorre porque o R não entende o uso da vírgula como separador de milhar. É possível resolver esse problema?

A função *mutate* é utilizada para operar transformações nas variáveis existentes, substituindo ou criando variáveis novas. Há diversas transformações possíveis e lembrar bastante as funções de outros softwares, como MS Excel. Vamos ver algumas das mais importantes.

Um exemplo simples: vamor gerar uma nova variável com os nomes dos beneficiários em minúsculo usando a função *tolower*. Veja:

```{r}
glimpse(saques_amostra_201701)
saques_amostra_201701 <- saques_amostra_201701 %>% 
mutate(nome_min = tolower(nome))
```

ou, em uma forma alternativa,

```{r}
saques_amostra_201701 <- mutate(saques_amostra_201701, nome_min = tolower(nome))
```

Use o comando View para visualizar o resultado da coluna criada à direita do banco de dados. Simples, não? Basta inserimos dentro do comando mutate a expressão da transformação que queremos.

Agora retomamos ao caso da variável valor, um exemplo pouco mais difícil.

- Primeiro vamos substituir a vírgula por nada do texto, já que a vírgula não é identificada como separador de milhar no R;

- Na sequência, indicamos que o texto, na verdade é um número.

Aqui, não criaremos uma nova variável "valor", somente vamos alterar a variável existente em duas etapas. Para a primeira, utilizamos a função *gsub* para substituir a vírgula por nada (sim, nada mesmo, com um ""). No passo seguinte, com a função *as.numeric* faremos a transformação texto-número.


```{r}
saques_amostra_201701 <- saques_amostra_201701 %>% 
  mutate(valor_num = gsub(",", "", valor)) %>% 
  mutate(valor_num = as.numeric(valor_num))
```

Dica: A operação reversa a *as.numeric*, para transformar número em texto, é a *as.character*. Vamos utilizá-la mais à frente no curso.

Sobre a transformação realizada, precisamos usar o *mutate* duas vezes? Não. Outras duas formas equivalentes estão abaixo.

```{r}
saques_amostra_201701 <- saques_amostra_201701 %>% 
  mutate(valor_num = as.numeric(gsub(",", "", valor)))
```

Ou: 

```{r}
saques_amostra_201701 <- saques_amostra_201701 %>% 
  mutate(valor_num = gsub(",", "", valor), 
         valor_num = as.numeric(valor_num))
```

Em outros exemplos, faremos duas operações separadas, com cada uma resultando em uma nova variável. Na primeira faremos a conversão do valor para dólar, dividindo por 5.5; na segunda, somaremos R$10 ao valor do benefício somente pelo exercício de ver a transformação.

```{r}
saques_amostra_201701 <- saques_amostra_201701 %>% 
  mutate(valor_dolar = valor_num / 5.2, 
         valor10     = valor_num + 10)
```

Use o comando *View* para ver as novas variáveis no banco de dados.

As operações de soma, subtração, divisão, multiplicação, módulo entre mais de uma variável ou entre variáveis e valores são válidas e facilmente executadas como acima mostramos.

Mas, como vimos no tutorial anterior, nem todas as transformações de variáveis são operações matemáticas. Vamos criar uma nova variável que indique se o valor sacado é "Alto" (acima de R\$ 300) ou "Baixo" (abaixo de R\$300) com a já vista função *cut*:

```{r}
saques_amostra_201701 <- saques_amostra_201701 %>% 
  mutate(valor_categorico = cut(valor_num, c(0, 300, Inf), c("Baixo", "Alto")))
```

E se quisermos recodificar uma variável de texto? Vamos dar uma olhada na variável "mes", que contém o "Mês de Competência" do saque, utilizando a função *table*:

```{r}
table(saques_amostra_201701$mes)
```

São 3 valores possíveis em nossa amostra: "11/2016", "12/2016" e "01/2017" em nossa amostra. Vamos gerar uma nova variável, ano, que indica apenas se a competência é 2016 ou 2017. Vamos começar fazendo uma cópia da variável original e depois substituiremos cada um dos valores com a função *replace*:

```{r}
saques_amostra_201701 <- saques_amostra_201701 %>% 
  mutate(ano = mes,
         ano = replace(ano, ano == "11/2016", "2016"),
         ano = replace(ano, ano == "12/2016", "2016"),
         ano = replace(ano, ano == "01/2017", "2017"))
```

Um pouco trabalhoso, mas cumpre o objetivo. Uma maneira mais inteligente é usar o comando *recode*:

```{r}
saques_amostra_201701 <- saques_amostra_201701 %>% 
  mutate(ano = recode(mes, "11/2016" = "2016", "12/2016" = "2016", "01/2017" = "2017"))
```

Com as operações matemáticas, as transformações *as.numeric* e *as.character* e os comandos *cut*, *replace* e *recode* podemos fazer praticamente qualquer recodifição de variáveis que envolva texto e números. A exceção, por enquanto, serão as variáveis da classe *factor*, que mencionamos em tutorais anteriores. 

Para os interessados em expressões regulares, posso indicar leituras para manipulação de texto.

### Exercício para casa

Use os exemplos acima para gerar novas variáveis conforme instruções abaixo:

- Faça uma nova divisão da variável "valor" a seu critério. Chame a nova variável de "valor\_categorico2".
- Cria uma variável "valor_euro", que é o valor calculado em Euros.
- Recodifique "valor\_categorico" chamando as categorias de "Abaixo de R\$300" e "Acima de R\$300". Chame a nova variável de "valor\_categorico3".
- Usando a função _recode_ Recodifique "mes" em 3 novos valores: "Novembro", "Dezembro" e "Janeiro". Chame a nova variável de "mes\_novo".
- Usando a função _replace_ Recodifique "mes" em 3 novos valores: "Novembro", "Dezembro" e "Janeiro". Chame a nova variável de "mes\_novo2".

## Filtrando linhas

Em muitos casos, queremos trabalhar somente com uma parte de nosso banco de dados, um subconjunto de linhas. Por exemplo, se quisermos apenas os beneficiários do estado do Espírito Santo podemos selecionar e salvar em um objeto chamado "saques_amostra_ES".

```{r}
saques_amostra_ES <- saques_amostra_201701 %>% 
  filter(uf == "ES")
```
ou 

```{r}
saques_amostra_ES <-filter(saques_amostra_201701, uf == "ES")
```

A função *filter* nos lembra a estrutura de outras do *dplyr* que já vimos. Mas aqui temos a novidade na condição uf == "ES", indicando que queremos apenas as linhas cuja variável "uf" assume valor igual a ES. Mas por que usamos duas vezes o sinal de igual (==) e não uma (=)? No geral, usamos um igual (=) para atribuir algo a um nome ou para definir algo igual a um valor. Aqui estamos comparando duas coisas, ou seja, verificando se o conteúdo de cada linha é igual a um valor. 

Outras opções além da igualdade são: maior (>), maior ou igual (>=), menor (<), menor ou igual (<=) e diferente (!=). Veja que escrevemos na sequência que pensamos nossa seleção.

Outro ponto é a utilização das aspas em "ES". Como estamos comparando valores para cada linha a um texto, devemos usar as aspas.

Vamos supor agora que queremos apenas os estados do Centro-Oeste. Vamos criar um novo *data\_frame* chamado "saques_amostra_CO" que atenda esse critério:

```{r}
saques_amostra_CO <- saques_amostra_201701 %>% 
  filter(uf == "MT" | uf == "MS" | uf == "DF" | uf == "GO")
```

Perceba que usamos a barra vertical para indicar que queremos as quatro condições atendidas. A barra vertical representa a condição "ou" e indica que todas as observações que atendem a uma ou a outra condição serão selecionadas.

Vamos complicar um pouco? Nesse passo faremos uma seleção de linhas que obedeça condições relativas a duas variáveis. Como exemplo, vamos selecionar as observações do Mato Grosso que **também** tenham o ano de competência (variável "ano" que criamos acima) igual a 2016. Aqui queremos que obedeça às duas condições ao mesmo tempo, ou seja, as condição 1 **e** condição 2. O símbolo para "e" é "&", vamos ver como aplicá-lo:

```{r}
saques_amostra_MT_2016 <- saques_amostra_201701 %>% 
  filter(uf == "MT" & ano == "2016")
```

Poderíamos, ao utilizar duas variáveis diferentes em filter, escrever o comando separando as condições por vírgula e não usar o operador "&":

```{r}
saques_amostra_MT_2016 <- saques_amostra_201701 %>% 
  filter(uf == "MT", ano == "2016")
```

É possível combinar quantas condições quisermos. Caso exista ambiguidade quanto à ordem dessas condições basta usar parênteses da mesma forma que em operações aritméticas.

### Exercício para casa

- Crie um novo *data\_frame* apenas com as observações cujo mês de competência é janeiro.
- Crie um novo *data\_frame* apenas com as observações cujo valor é superior a R\$ 500.
- Crie um novo *data\_frame* apenas com as observações da região Sul.

## Agrupando

Até agora todas as transformações ou seleções no banco de dados utilizavam os beneficiários como unidade de análise. Mas e se quisermos trabalhar em um nível mais agregado?

Vamos retornar ao exemplo do início do tutorial, começando com a produção de um novo *data\_frame* que contenha a informação de quantos saques foram realizados em cada UF:

```{r}
contagem_uf <- saques_amostra_201701 %>% 
  group_by(uf) %>% 
  reframe(contagem = n()) %>% 
  ungroup()
```

Veja que temos 3 funções no nosso *pipe*: *group\_by*, *reframe* e *ungroup*. Elas tem significado literal. Na primeira, inserimos as variáveis pelas quais agruparemos o banco de dados (podemos usar mais de uma). Na segunda, as operações de "sumário"/"resumo", ou seja, de condensação - o que faremos com o banco de dados e demais variáveis. Aqui apenas contamos quantas linhas pertencem a cada UF, que é a variável de grupo, a partir da função *n()*. Por fim, a função *ungroup* "desagrupa" os dados. Ela é importante para evitar problemas em futuras manipulações, uma vez que o R bloqueia algumas delas para dados agrupados.

Vamos tornar isso um pouco mais interessante. Além da contagem, vamos obter a soma, a média, a mediana, o desvio padrão, os valores mínimos e máximos dos benefícios no mesmo resultado. Para isso, basta inserir novas operações na função *reframe* separando por vírgulas.

```{r}
valores_uf <- saques_amostra_201701 %>% 
  group_by(uf) %>% 
  reframe(contagem = n(),
            soma     = sum(valor_num),
            media    = mean(valor_num),
            mediana  = median(valor_num),
            desvio   = sd(valor_num),
            minimo   = min(valor_num),
            maximo   = max(valor_num)) %>% 
  ungroup()
```

Use _View_ para observar o resultado.

A sessão _Useful Summary Functions_ do livro _R for Data Science_ traz uma relação mais completa de funçoes que podem ser usandas com _reframe_. O ["cheatsheet" da RStudio](https://github.com/rstudio/cheatsheets/raw/master/source/pdfs/data-transformation-cheatsheet.pdf) oferece uma lista para uso rápido.

### Exercício para casa

Usando a variável "mes_novo", calcule a contagem, soma e média de valores para cada mês.

## Mais de um grupo

Lembra que podemos agrupar por mais de uma variável? Vamos aplicar isso agrupando por "mes" e "uf" e recebendo a contagem de saques a partir dessa combinação de grupos:

```{r}
contagem_uf_mes <- saques_amostra_201701 %>% 
  group_by(uf, mes) %>% 
  reframe(contagem = n()) %>% 
  ungroup()
```

Perceba que recebemos uma mensagem de alerta. Isso ocorre porque cada uf é repetida de duas a três vezes. Como alguns estados não possuem observações para todos os meses, o R nos avisa que podemos ter um problema.

No resultado, cada grupo gera uma nova coluna e as linhas representam exatamente a combinação de grupos de cada 
variável presente nos dados.

Finalmente, podemos utilizar múltiplas variáveis de grupo em conjunto e também gerar um sumário com diversas varáveis, como no exemplo a seguir, que combina parte dos dois anteriores:

```{r}
valores_uf_mes <- saques_amostra_201701 %>% 
  group_by(uf, mes) %>% 
  reframe(contagem = n(),
            soma     = sum(valor_num),
            media    = mean(valor_num),
            desvio   = sd(valor_num)) %>% 
  ungroup()
```

## Novo _data frame_ ou tabela para análise?

As funções *group_\by* e *reframe* tem dois propósitos em seu uso. O primeiro é a produção de uma tabela para análise (ou para geração de gráficos), como fizemos acima. A outra é a geração de um novo *data\_frame*. O uso depende do tamanho da redução que queremos no banco de dados.

Por exemplo, podemos pensar em um banco que agrege as informações por município, o que geraria um novo banco com pouco mais de 5500 observações/linhas a partir do banco completo. Poderíamos utilizar esses dados para inserir nos dados originais como colunas (você pode aplicar isso de forma direta usando a função *mutate*). Mas ainda não aprendemos a relacionar dois ou mais *data\_frames* - faremos no Tutorial 3.

Por agora, vamos observar como seria a base de dados utilizando os municípios como linhas:

```{r}
saques_amostra_munic <- saques_amostra_201701 %>% 
  group_by(munic) %>% 
  reframe(contagem = n(),
            soma     = sum(valor_num),
            media    = mean(valor_num)) %>% 
  ungroup()
```

## Ordenando a base de dados

O ordenamento de bases de dados muito gandes faz pouco ou nenhum sentido. Mas quando trabalhamos com poucas linhas, como nos exemplos acima, pode ser útil ordenar a tabela por aguma variável de interesse - perceba que aqui não faz muito sentido diferenciar tabela de *data\_frame* por desempenharem a mesma função.

Vamos ordenar, de forma crescente, a tabela de valores por uf e pela "soma

Se quisermos ordenar, de forma crescente, a tabela de valores por uf pela "soma" de valores. Para isso é só usar a função *arrange*:

```{r}
valores_uf <- valores_uf %>% 
  arrange(soma)
```

Poderíamos ter feito diferente e usado o comanto *arrange* diretamente ao gerar a tabela:

```{r}
valores_uf <- saques_amostra_201701 %>% 
  group_by(uf) %>% 
  reframe(contagem = n(),
            soma     = sum(valor_num),
            media    = mean(valor_num),
            mediana  = median(valor_num),
            desvio   = sd(valor_num),
            minimo   = min(valor_num),
            maximo   = max(valor_num)) %>%
  ungroup() %>% 
  arrange(soma)
```

Podemos, também, rearranjar uma tabela em ordem decrescente. Vamos fazer isso com a média do valor do benefício usandoo argumento *desc*:

```{r}
valores_uf <- valores_uf %>% 
  arrange(desc(media))
```

Podemos, também, usar mais de uma variável ao ordenar. Para isso é só colocá-las em ordem de prioridade usando a vírgula para separar. Vamos ordenar pela mediana (descendente) e depois pelo valor máximo (crescente).

```{r}
valores_uf <- valores_uf %>% 
  arrange(desc(mediana), maximo)
```

E finalizamos mais uma etapa no processo de aprender a manipular dados no R. No próximo tutorial trabalharemos com bases de dados relacionais dentro da gramática *dplyr*.