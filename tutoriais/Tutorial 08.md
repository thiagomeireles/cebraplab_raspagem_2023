# Tutorial 8: Formulários na web

Durante a aula de webscraping, trabalhamos com exemplos nos quais retiramos informação de páginas em html, mas não enviamos nenhuma informação ao servidor com as quais estamos nos comunicando (exceto manualmente). Contudo, é muito comum nos depararmos com formulários em páginas que queremos raspar. Formulários, na maioria das vezes, aparecerão como caixas de consulta acompanhada de um botão.

Mecanismos de busca (Google, DuckDuckGo, etc) têm formulários nas suas páginas iniciais. Portais de notícia ou de Legislativos têm formulários de busca (como o que usamos manualmente no caso da Folha de São Paulo). Por vezes, mesmo para "passar" de página nos deparamos com um formulário.

Neste tutorial vamos aprender a preencher um formulário, enviá-lo ao servidor da página e capturar sua resposta.

Faremos um exemplo simples, o buscador da ALESP.

## _rvest_, formulários e ALESP

Vamos começar carregando os pacotes _rvest_, *httr*, _dplyr_, _tidyr_ e *stringr*:

```{r}
pacman::p_load(rvest,
               httr,
               dplyr,
               tidyr,
               stringr)
```

Nosso primeiro passo ao lidar com formulários será estabelecer uma conexão com o servidor, antes mesmo de capturar a página na qual o formulário está. Damos a essa conexão o nome de "session", e utilizamos a função _session_ para criá-la observamos a página em que preenchemos o formulário para a busca observável [aqui](https://www.al.sp.gov.br/alesp/pesquisa-proposicoes/). Vamos ver como funciona?

```{r}
alesp_url <- "https://www.al.sp.gov.br/alesp/pesquisa-proposicoes/"

alesp_session <- session(alesp_url)
```

Estabelecida a conexão, precisamos conhecer o formulário. Começamos, obviamente, obtendo o código HTML da página do formulário. Na sequência, vamos extrair do código da página os formulários. Como tabelas em HTML, fomrulários tem suas tags próprias e contamos no pacote _rvest_ com uma função que extrai uma lista contendo todos os formulários da página, a *html\_form*:

```{r}
alesp_pagina <- read_html(alesp_session)

alesp_form_list <- html_form(alesp_pagina)

alesp_form_list
```

No caso do buscador da ALESP, há três formulário na página. Com dois colchetes, extraímos o formulário que está na terceira posição da lista de formulários.

```{r}
alesp_form <- alesp_form_list[[3]]

class(alesp_form)

alesp_form
```

Examine o objeto que contém o formulário. Ele é um objeto da classe "form" e podemos observar todos os parâmetros que o compõe, ou seja, tudo aquilo que pode ser preenchido para envio ao servidor, ademais dos botões de submissão.

Vá para o navegador e inspecione a caixa de busca. Você observará que cada "campo" do formulário é uma tag "input". O atributo "type", define se será oculto ("hidden"), texto ("text"), seleção de novas entradas ("select") ou botão de submissão ("button").

Alguns "inputs" já contêm valores (no atributo "values"). No nosso exemplo, os botões são destacados. Temos, por exemplo, qual o número de linhas por página ("rowsPerPage") definida com 20 resultados.

O que nos interesse preencher, obviamente, é o "input" chamado "text". Para capturar mais dados, podemos indicar quantos resultados queremos por página com "rowsPerPage". Vamos manter a busca por "coronavirus" e limitar a 400 resultados.

Vamos, então, preencher os campos com a função _html\_form\_set_:

```{r}
alesp_form <- html_form_set(alesp_form,
                          'text' = "nomeacao")
```

Simples, não? Colocamos o objeto do formulário no primeiro parâmetro da função e os campos a serem preenchidos na sequência, tal como no exemplo.

Reexamine agora o formulário. Você verá que "text" está preenchido e "rowsPerPage" teve o número de resultados alterado:

```{r}
alesp_form
```

Legal! Agora vamos fazer a submissão do formulário. Na *session\_submit*, precismos informar a sessão que criamos (conexão com o servidor), o formulário que vamos submeter e o nome do botão de submissão. No nosso exemplo, no entando, o botão de submissão é identificado como "NULL". Veja o exemplo: 

```{r}
alesp_submission <- session_submit(x = alesp_session,
                                   form = alesp_form,
                                   submit = NULL)
```

Pronto! Agora basta raspar o resultado como já haviámos feito antes. A página que queremos raspar é o objeto que resulta da função _submit\_form_. Abra o [resultado de uma busca na ALESP](https://www.al.sp.gov.br/spl_consultas/consultaProposicoesAction.do;jsessionid=B99CD5DD1520AF721E286203683CD227?direction=inicio&lastPage=0&currentPage=0&act=detalhe&rowsPerPage=400&currentPageDetalhe=1&method=search&text=coronavirus&legislativeNumber=&legislativeYear=&anoDeExercicio=&strInitialDate=&strFinalDate=&advancedSearch=S) e tente entender o código abaixo.

```{r}
dados <- alesp_submission %>% 
  read_html()  %>% 
  html_nodes(xpath = '//*[@id="lista_resultado"]/table') %>% 
  html_table() %>% 
  magrittr::extract2(1) %>% 
  filter(Autor != "") %>%
  separate("Documento", into = c("Documento", "Ementa"), 
           sep = "\r\n\t\t\t\t\t\t\r\n\t\t\t\t\t\t\r\n\t\t\t\t\t") %>% 
  mutate(Documento = str_squish(Documento)) %>% 
  mutate(links = read_html(alesp_submission) %>% 
           html_nodes(xpath = '//*[@id="lista_resultado"]/table/tbody/tr/td/a[@style="color: #000000"]') %>% 
           html_attr(name = 'href'),
         links = paste0("https://www.al.sp.gov.br/", links))
```