# Tutorial Pré-Curso III: Abindo dados em R

# Abrindo dados no R

Neste tutorial vamos cobrir uma série de métodos disponíveis para abrirmos arquivos de texto, editores de planiljas e de outros softwares de análise de dadoss no R. Vamos dar atenção aos argumentos das funções de forma a solucionar dificuldades de abertura de dados com diferentes características ou em sistemas operacionais variados.

## Pacotes no R

Antes de avançarmos à tarefa principal, vamos aprender um pouco mais sobre pacotes. Já foi destacado diversas vezes que uma das vantagens do R é a existência de uma comunidade produtiva e que desenvolve continuamente novas funcionalidades, tudo em código aberto.

Para instalarmos um novo pacote de R que esteja disponível no CRAN -- "The Comprehensive R Archive Network" -- utilizamos a função *install.packages*. Veja o exemplo com o pacote *beepr*:

```{r, eval = F}
install.packages("beepr")
```

Note que o nome do pacote deve estar em parêntese. Além disso, é possível que você tenha sido perguntad@ sobre de qual servidor do CRAN você quer baixar o pacote. A escolha em nada muda o resultado, exceto o tempo de duração do download.

Uma vez que um pacote foi instalado, ele está disponível em seu computador, mas não ainda para uso. Apenas depois de executarmos a função *library* é que teremos o pacote em nossa "biblioteca" de funções.

```{r}
library(beepr)
```

Você pode dispensar as aspas ao usar a função *library*, pois é opcional. A função *require* é semelhante a *library* e a ignoraremos por enquanto.

## Abrindo dados com da funções do pacote utils

Quando você inicia uma nova sessão de R, alguns pacotes já estão automaticamente carregados. *utils* é um deles, e ele contém as funções mais conhecidas de abertura de dados em arquivos de texto.

A principal função é *read.table*, utilizada no primeiro tutorial. Use a função *args* para explorar seus argumentos:

```{r}
args(read.table)
```

É imprescindível que a função *read.table* receba como primeiro argumento um arquivo de dados. Note que o caminho para o arquivo deve estar completo (ex: "C:\\\\User\\\\Documents\\\\file.txt") ou ele deve estar no seu *working directory* (wd). Mas como eu descubro meu wd?

## Caminhos no R

```{r, eval = F}
getwd()
```

E como eu altero meu wd?

```{r, eval = F}
setwd("~/dados") # Aqui pode estabelecer o de sua preferência
```

Simples e muito útil para evitar escrever "labirintos de pastas" ao importar dados.

Um detalhe fundamental para quem usa Windows: os caminhos devem ser escritos com duas barras no lugar de uma, como no exemplo acima, ou com barras invertidas. É uma chatice e a melhor solução é mudar definitivamente para Linux e nunca mais pagar por software proprietário (sim, veremos a defesa do software livre por aqui!).

Vamos supor que você queira abrir diversos arquivos ("file1.txt" e "file2.txt", por exemplo) que estão em uma pasta diferente do seu wd, por exemplo "C:\\\\User\\\\Downloads\\\\". Mudar o wd pode não ser conviente, mas escrever o caminho todo é menos ainda. Uma solução é criar usar *file.path* para cadar arquivo armazenando a pasta e o caminho dos arquivos em algum objeto.

```{r}
pasta <- paste(getwd(), "dados", sep = "/")
path_file1 <- file.path(pasta, "file1.txt")
path_file2 <- file.path(pasta, "file2.txt")
```

O código acima pode parecer pouco inteligente neste momento, mas tente pensar a combinação dele com loops para abrir diversos arquivos.

Use as funções que acabamos de ver para gerenciar caminhos de pastas no R. Vale a pena.

No início da primeira aula falarei um pouco sobre os "RProjects" e como são úteis para (i) facilitar nossa vida quanto aos "caminhos" e (ii) tornar o código replicável em diferentes máquinas.

## read.table

Voltando à função *read.table*, vamos examinar os argumentos seguintes a *file* usando um exemplo de dados retirado do Portal da Transparência. Extraí dos pagamentos do programa uma amostra de tamanho 50 e salvei em diversos arquivos com caracteristicas distintas.

Para facilitar nossa vida, vamos usar como argumento "file" o endereço dos dados no repositório do curso. O primeiro arquivo está no enderço que guardaremos em "file1".

```{r}
file1 <- "https://raw.githubusercontent.com/thiagomeireles/cebraplab_raspagem_2023/main/dados/bf_amostra_hv.csv"
```

Esse arquivo contém cabeçalho, ou seja, a primeira linha traz o nome das colunas. Por esta razão, informaremos "header = T". Ignore por hora o argumento "sep".

```{r}
dados <- read.table(file1, header = T, sep = ",")
head(dados)
```

O que aconteceria se escolhessemos "header = F"?

```{r}
dados <- read.table(file1, header = F, sep = ",")
head(dados)
```

Em primeiro lugar, o nome das variáveis é inserido automaticamente e na sequência V1, V2, ..., Vn, onde n é o número de colunas. Além disso, os nomes das variáveis é lido como se fossem uma observação. A consequência é que todas as variáveis serão lidas com um texto na primeira coluna, resultando em "character" ou "factor" a depender das características dos dados.

```{r}
str(dados)
```

Vamos observar agora uma versão dos dados que não contém a primeira linha como nome das variáveis. Os dados estão no url armazenado em file2:

```{r}
file2 <- "https://raw.githubusercontent.com/thiagomeireles/cebraplab_raspagem_2023/main/dados/bf_amostra_nv.csv"
```

Como já havíamos visto, quando não há cabeçalho na primeira linha, os nomes são inseridos automaticamente:

```{r}
dados <- read.table(file2, header = F, sep = ",")
head(dados)
```

E se cometermos o erro de indicar que há cabeçalho quando não há?

```{r}
dados <- read.table(file2, header = T, sep = ",")
head(dados)
```

A primeira linha de dados se torna o nome das variáveis (inclusive os números, que aparecem antecedidos por um "X").

Ambos arquivos têm o mesmo separador: vírgula. O argumento "sep" permite indicar qual é o separador.

Não há muita graça em observar os exemplos com separadores diferente, mas vejamos como abrí-los. Os mais comuns, além da vírgula, são o ponto e vírgula e o tab, este último representado pelo símbolo "\t"

```{r}
# Ponto e virgula
file3 <- "https://raw.githubusercontent.com/thiagomeireles/cebraplab_raspagem_2023/main/dados/bf_amostra_hp.csv"
dados <- read.table(file3, header = T, sep = ";")

file4 <- "https://raw.githubusercontent.com/thiagomeireles/cebraplab_raspagem_2023/main/dados/bf_amostra_ht.csv"
dados <- read.table(file4, header = T, sep = "\t")
```

Há outras funções da mesma família de *read.table* no pacote *utils*. A diferença entre elas é o separador de colunas -- vírgula para *read.csv*, ponto e vírgula para *read.csv2*, tab para *read.delim* e *read.delim2* -- e o separador de decimal.

Por default, *read.table* considera que os campos em cada coluna estão envolvidas por aspas duplas (quote = "\\"". Para indicar que não há nada, use quote = "".

"dec" é o argumento para o separador decimais. Como o padrão brasileiro é a vírgula, e não o ponto, este argumento costuma ser importante.

Por vezes é conveniente importar apenas um subconjunto das linhas. "skip" permite que pulemos algumas linhas e "nrows" indica o máximo de linhas a serem abertas. Se o banco de dados for desconhecido e muito grande, abrir uma fração permite conhecer (tentando e errando) os demais argumentos ("header", "sep", etc) adequados para abrir os dados com um baixo custo de tempo e paciência.

Por exemplo, para pular as 3 primeiras linhas:

```{r}
dados <- read.table(file2, header = T, sep = ",", skip = 3)
head(dados)
```

Para abrir apenas 20 linhas:

```{r}
dados <- read.table(file2, header = T, sep = ",", nrows = 20)
head(dados)
```

Combinando, para abrir da linha 11 à linha 30:

```{r}
dados <- read.table(file1, header = T, sep = ",", skip = 10, nrows = 30)
head(dados)
```

Por vezes, é interessante definir as classes das variáveis a serem importadas. O argumento deve ser um vetor com uma classe para cada coluna. Por exemplo:

```{r}
dados <- read.table(file1, header = T, sep = ",", 
  colClasses = c("character", "numeric","character", "numeric", "numeric"))
str(dados)
```

Perceba que quando abrimos os dados sem especificar o tipo da coluna, a função *read.table* tenta identificá-los. Uma das grandes chatices das funções de abertura de dados pacote *utils* é que colunas de texto são normalmente identificadas como "factors", mesmo quando claramente não são. Veja os exemplos anteriores

Para evitar que textos sejam lidos como "factors" é importante informar o parâmetro "stringsAsFactors = F", pois o padrão é "T". Este argumento incomoda tanto que diversas pessoas chegam a alterar a configuração básica da função para não ter de informá-lo diversas vezes.


```{r}
dados <- read.table(file1, header = T, sep = ",", stringsAsFactors = F)
str(dados)
```

Note que, agora, "uf" e "munic" são importadas como "character".

Finalmente, é comum termos problemas para abrir arquivos que contenham caracteres especiais, pois há diferentes formas do computador transformar 0 e 1 em vogais acentuadas, cecedilha, etc. O "encoding" de cada arquivo varia de acordo com o sistema operacional e aplicativo no qual foi gerado.

Para resolver este problema, informamos ao R o parâmetro "fileEncoding", que indica qual é o "encoding" esperado do arquivo. Infelizmente não há formas automáticas infalíveis de descobrir o "encoding" de um arquivo e é preciso conhecer como foi gerado -- seja por que você produziu o arquivo ou por que você teve acesso à documentação -- ou partir para tentativa e erro. Alguns "encodings" comuns são "latin1", "latin2" e "utf8", mas há diversos outros. Como arquivo com o qual estamos trabalhando não contém caracteres especiais, não é preciso fazer nada.

## Tibbles e tidyverse

O desenvolvimento de pacotes em R levou à criação de um tipo específico de *dataframe*, chamado *tibble*. A estrura é idêntica à de um *dataframe* regular e suas diferenças se resumem à forma que os dados aparecem no console ao "imprimirmos" o objeto, ao "subsetting", ou seja, à seleção de linhas e à adoção de "stringsAsFactors = F" como padrão. Você pode ler mais sobre *tibbles* [aqui](https://cran.r-project.org/web/packages/tibble/vignettes/tibble.html).

O pacote *readr*, parte do *tidyverse* (conjunto de pacotes com o qual vamos trabalhar na primeira aula), contém funções para abertura de dados em .txt semelhantes às do pacote *utils*, mas que trazem algums vantagens: velocidade de abertura, simplicação de argumentos e a produção de *tibbles* como resultado da importação.

```{r, eval=FALSE}
install.packages('readr')
```


```{r}
library(readr)
```

A função análoga à *read.table* em *readr* chama-se *read\_delim*. Veja:

```{r}
dados <- read_delim(file1, delim = ",")
dados
```

Observe que não utilizamos *head* para imprimir as primeiras linhas. Essa é uma característica de *tibbles*: o output contém uma fração do banco, a informação sobre número de linhas e colunas, e os tipos de cada variável abaixo dos nomes das colunas. "delim" é o argumento que entra no lugar de "sep" ao utilizarmos as funções do *readr*.

O padrão de *read\_delim* é importar a primeira coluna como nome das variáveis. No lugar de *header*, temos agora o argumento *col_names*, que deve ser igual a "FALSE" para os dados armezenados em "file2", por exemplo:

```{r}
dados <- read_delim(file2, col_names = F, delim = ",")
dados
```

X1, X2, ..., Xn, com "n" igual ao número de colunas, passam a ser os nomes das variáveis neste caso.

Além dos valores lógicos, "col_names" também aceita um vetor com novos nomes para as colunas como argumento:

```{r}
dados <- read_delim(file2, col_names = c("estado", "municipio_cod", "municipio_nome",
                                         "NIS", "transferido"),
                    delim = ",")
dados
```

"skip" e "n_max" são os argumentos de *read\_delim* correspondentes a "skip" e "nrows".

Finalmente, "col_types" cumpre a função de "colClasses", com uma vantagem: não é preciso usar um vetor com os tipos de dados para cada variável. Basta escrever uma sequência de caracteres onde "c" = "character", "d" = "double", "l" = "logical" e "i" = "integer":

```{r}
dados <- read_delim(file1, delim = ",", col_types = "cicid")
dados
```

Mais econômico, não?

*read\_csv* e *read\_tsv* são as versões de *read\_delim* para arquivos separados por vírgula e arquivos separado por tabulações.

Do ponto de vista formal, as três funções de importação de dados de texto do pacote *readr* geram objetos que pertecem à classe de *dataframes* e também às classes *tbl* e *tbl\_df*, que são as classes de *tibbles* (um objeto pode ser de mais de uma clase).

Ainda no pacote *readr*, duas funções são bastante úteis

## Outra gramática para dados em R: data.table

No curso vamos trabalhar com duas "gramáticas" para dados em R: a dos pacotes básicos e a fornecida pelos pacotes do *tidyverse*. O pacote *data.table* oferece uma terceira alternativa, que não trabalharemos no curso.

```{r, eval=FALSE}
install.packages('data.table')
```

O *data.table* é excelente para abrir bancos de dados grandes, tenha isso em mente.

```{r}
library(data.table)
```

Há, porém, duas funções excepcionalmente boas neste pacote: *fread* e *fwrite*, que servem, respectivamente, para importar e exportar dados de texto.

```{r}
class(dados)
```

As duas grandes vantagens de utilizar *fread* são: a função detecta os automaticamente as características do arquivo de texto para definir delimitador, cabeçalho, tipos de dados das colunas, etc; e é extremamente rápida em comparação às demais. "f" vem de "Fast and friendly file finagler".

```{r}
dados <- fread(file1)
head(dados)
```

Além de ser um *data.frame*, os objeto criado com *fread* também é da classe *data.table* e aceita a "gramática" do pacote de mesmo nome.

Obviamente, se você precisar especificar os argumentos para *fread* ler um arquivo, não há problemas. Eles são muito parecidos aos de *read.table*.

Abrir dados com o *data.table* não permmitiam integração com as ferramentas do pacote *tidyverse*, o que era uma desvantagem mesmo sendo muito útil para abrir arquivos maiores em menor tempo. No entanto, hoje é possível utilizar as ferramentas do *tidyverse*, especialmente a "linguagem alternativa" *dplyr* que nos economiza bastante código.

## Dados em arquivos editores de planilhas

Editores de planilha são, em geral, a primeira ferramenta de análise de dados que aprendemos. Diversas organizações disponibilizam (infelizmente) seus dados em formato .xls ou .xlsx e muitos pesquisadores utilizam editores de planilha para construir bases de dados.

Vamos ver como obter dados em formato .xls ou .xlsx diretamente, sem precisar abrir os arquivos e exportá-los para um formato de texto.

Há dois bons pacotes com funções para dados em editores de planilha: *readxl* e *gdata*. Vamos trabalhar apenas com o primeiro, mas convém conhecer o segundo se você for trabalhar constantemente com planilhas e quiser editá-las, e não só salvá-las. *readxl* também é parte do *tidyverse*.  Primeiro vamos instalá-lo:

```{r, eval=FALSE}
install.packages("readxl")
```

Importe o pacote:

```{r}
library(readxl)
```

## Um pouco sobre donwload e manipulação de arquivos

Nosso exemplo será a Pesquisa Perfil dos Municípios Brasileiros de 2005, produzida pelo IBGE e apelidade de MUNIC. Diferentemente das demais funções deste tutorial, precisamos baixar o arquivo para o computador e acessá-lo localmente. Faça o download diretamente do [site do IBGE](ftp://ftp.ibge.gov.br/Perfil_Municipios/2005/base_MUNIC_2005.zip) e descompacte. Ou, mais interessante ainda, vamos automatizar o download e descompactação do arquivo (se tiver algum erro no  Windows avise para corrigir -- use Linux!).

Em primeiro lugar, vamos guardar o endereço url do arquivo em um objeto e fazer o download. Note que na função *download.file* o primeiro argumento é o url e o segundo é o nome do arquivo que será salvo (nota: você pode colocá-lo em qualquer pasta utilizando *file.path* para construir o caminho completo para o arquivo a ser gerado).

```{r}
url_arquivo <- "ftp://ftp.ibge.gov.br/Perfil_Municipios/2005/base_MUNIC_2005.zip"
download.file(url_arquivo, "temp.zip", quiet = F)
```

O argumento "quiet = F" serve para não imprimirmos no console "os números" do download (pois o tutorial ficaria poluído), mas você pode retirá-lo ou alterá-lo caso queira ver o que acontece.

Com *unzip*, vamos extrair o conteúdo da pasta:

```{r}
unzip("temp.zip")
```

Use *list.files* para ver todos os arquivos que estão na sua pasta caso você não saiba o nome do arquivo baixado. No nosso caso utilizaremos o arquivo "Base 2005.xls"

```{r, eval = F}
list.files()
```

Vamos aproveitar e excluir nosso arquivo .zip temporário: 

```{r}
file.remove("temp.zip")
```

## Voltando às planilhas

Para não repetir o nome do arquivo diversas vezes, vamos criar o objeto "arquivo" que contém o endereço do arquivo no seu computador (ou só o nome do arquivo entre aspas se você tivê-lo no seu wd):

```{r}
arquivo <- "Base 2005.xls"
```

Com *excel\_sheets* examinamos quais são as planilhas existentes do no arquivo:

```{r, echo = F, results = 'hide'}
excel_sheets(arquivo)
```

No caso, temos 11 planilhas diferentes (e um bocado de mensagens de erro estranhas). O dicionário, para quem já trabalhou alguma vez com a MUNIC, não é uma base de dados, apenas textos espalhados entre células. As demais, no entanto, têm formato adequado para *dataframe*.

Vamos importar os dados da planilha "Variáveis externas". As duas maneiras abaixo se equivalem:

```{r, echo = F, results = 'hide'}
# 1
transporte <- read_excel(arquivo, "Variáveis externas")

# 2
transporte <- read_excel(arquivo, 11)
```

A função *read\_excel* aceita os argumentos "col_names", "col_types" e "skip" tal como as funções de importação do pacote *readr*.

```{r, include = F}
file.remove("Base 2005.xls")
```

## Dados de SPSS, Stata e SAS

R é bastante flexível quanto à importação de dados de outros softwares estatísticos. Para este fim também há dois pacotes, *foreign*, mais antigo e conhecido, e *haven*, que é, advinhe só, parte do *tidyverse*. São parecidos entre si e recomendo o uso do segundo, que examinaremos abaixo. Primeiro o instalamos caso não esteja na nossa biblioteca

```{r, eval=FALSE}
install.packages("haven")
```

Agora carregamos:

```{r}
library(haven)
```

Basicamente, há cinco funções de importação de dados em *haven*: *read\_sas*, para dados em SAS; *read\_stata* e *read\_dta*, idênticas, para dados em formato .dta gerados em Stata; e *read\_sav* e *read\_por*, uma para cada formato de dados em SPSS. O uso, como era de se esperar, é bastante similar ao que vimos no tutorial todo.

Vamos usar como exemplo o [Latinobarômetro 2015](http://www.latinobarometro.org/latContents.jsp), que está disponível para SAS, Stata, SPSS e R. Como os arquivos são grandes demais e o portal do Latinobarômetro é "cheio de javascript" (dá mais trabalho pegar dados de um portal com funcionalidades construídas nesta linguagem). Para facilitar, os arquivos foram incluídos nos dados do repositório do curso. Vamos ignorar SAS por razões que não interessam agora e por não ser uma linguagem popular nas ciências se sociais, mas se você tiver interesse em saber mais, me procure.

```{r}
# SPSS
download.file("https://raw.githubusercontent.com/thiagomeireles/cebraplab_raspagem_2023/main/dados/F00004529-Latinobarometro_2015_sav.zip", "latino2015_sav.zip")
unzip("latino2015_sav.zip")

# STATA
download.file("https://raw.githubusercontent.com/thiagomeireles/cebraplab_raspagem_2023/main/dados/F00004530-Latinobarometro_2015_dta.zip",  "latino2015_dta.zip")
unzip("latino2015_dta.zip")
```

Deixo abaixo uma breve descrição do Latinobarômetro que foi "roubado" de outro curso compartilhado com o Leonardo Barone. O Latinobarômetro dado que é um banco de dados com muitas (muitas mesmo) variáveis categóricas, posto que é survey.

#### 2015 Latinobarometer

"The second dataset is the 2015 Latinobarometer. This is a very popular survey on democracy and elections in Latin America and the original can be found [here](http://www.latinobarometro.org/latContents.jsp). Latinobarometer contains data on several Latin American Countries. We are going to use the data on Brazil only. "Barometers" are very good source of public opinion data regarding issues of political regime, civil liberties, economic performance of governments and etc. I am sure this dataset can be source of lots of dissertations of students in political economy and comparative politics."

## Abrindo os dados com haven

Vejamos o uso das funções em arquivos de diferentes formatos:

```{r}
# SPSS
latino_barometro <- read_sav("Latinobarometro_2015_Eng.sav", encoding = "latin1")
head(latino_barometro)

# Stata
latino_barometro <- read_dta("Latinobarometro_2015_Eng.dta", encoding = "latin1")
head(latino_barometro)
```

Simples assim.

Há critérios de conversão de variáveis categóricas, rótulos e etc, adotados pelo R ao importar arquivos de outras linguagens, mas você pode descobrí-los testando sozinh@ ou olhando a documentação das funções.

## Arquivos .RData

Faça download do arquivo do Latinobarômetro 2015 para formato R. Você verá que o arquivo tem a extensão .RData. Este é o formato de dados do R? Sim e não.

```{r}
download.file("https://raw.githubusercontent.com/thiagomeireles/cebraplab_raspagem_2023/main/dados/F00004532-Latinobarometro_2015_r.zip",
              "latino2015_rdata.zip")
unzip("latino2015_rdata.zip")
```


Começando pelo "não": um arquivo .RData não é um arquivo de base de dados em R, ou seja, não contém um *dataframe*. Ele contém um workspace inteiro! Ou seja, se você salvar o seu workspace agora usando o "botão de disquete" do RStudio que está na aba "Enviroment" (provavelmente no canto superior à direita), você salvará todos os objetos que ali estão -- *dataframes*, vetores, funções, gráficos, etc -- e não apenas um único *dataframe*.

Não há um arquivo de dados do R que se pareça com os arquivos de SPSS e Stata. Existem funções para exportar arquivos de texto com as famílias de funções "write", primas das funções "read", dos mesmos pacotes que usamos neste tutorial.

Para salvar um arquivo .RData sem usar o "botão do disquete", use a função *save.image*:

```{r}
save.image("myWorkspace.RData")
```

Para abrir um arquivo .RData, por exemplo, o do Latinobarômetro ou o que você acabou de salvar, use a função *load*:

```{r}
# Latinobarometro
load("Latinobarometro_2015_Eng.rdata")

# Workspace do tutorial
load("myWorkspace.RData")
```

```{r, include = F}
file.remove("myWorkspace.RData")
```

## Fim