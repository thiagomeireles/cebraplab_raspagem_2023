# [Cebrap.lab 2023] Raspagem de dados com R: Desafios de raspagem de dados com auutomatização da raspagem em *html* e novos desafios

## Objetivos gerais

Hoje iremos automatizar a coleta dos dados das páginas do Datafolha.

No final, veremos que mesmo páginas aparentemente simples possuem algumas complexidades extras, como as páginas dinâmicas. Quando iniciarmos a análise para a coleta das publicações do Diário Oficial da União (DOU), novos desafios serão colocados e nos prepararemos para lidar com eles no próximo encontro.

Aprenderemos hoje a acessar um navegador pelo R, criando um ambiente virtual do qual extraíremos dados na segunda-feira.

## Roteiro para a aula

1- Iniciaremos coma coleta de todos os links e fazer download de arquivos no DataFoha com o [Tutorial 5](https://github.com/thiagomeireles/cebraplab_raspagem_2023/blob/main/tutoriais/tutorial_05.md).

2- Por fim, [Tutorial 6](https://github.com/thiagomeireles/cebraplab_raspagem_2023/blob/main/tutoriais/tutorial_06.md) inicia o processo de raspagem das publicações do Diário Oficial da União (DOU). Nele veremos que o *rvest* nem sempre é capaz de nos fornecer o que precisamos. Em páginas dinâmicas, por exemplo, precisamos de alternativas como o *RSelenium*, com o qual teremos um primeiro contato no final do tutorial.
