# [Cebrap.lab 2023] Raspagem de dados com R

## Informações básicas

### Instrutor: 
	
[Thiago Meireles](https://thiagomeireles.github.io/)

### Data, Hora e Local

De 23 a 27 de outubro de 2023, das 19h00 às 22h0  0.

Os encontros serão nos dias 23, 25 e 27 de outubro via Zoom.

## Apresentação

O laboratório apresenta as principais ferramentas de raspagem de dados na Internet e manipulação de texto utilizando R. Além de ser um software livre voltado para estatística computacional e análise de dados, R é uma linguagem focada na aplicação de funções que, entre outras possibilidades, permite a captura de dados de forma automatizada na internet. A partir de informações disponíveis em portais de notícias, apresentaremos esse processo de raspagem de dados de páginas web (especialmente de tabelas e de páginas construídas em html) e construção de bases de dados com textos de Internet, permitindo introduzir as ferramentas mais básicas de mineração de texto. Faremos um exercício empírico partindo de uma questão de pesquisa que conduzirá a experimentação, de forma a capacitar os participantes com ferramentas e procedimentos que depois poderão ser usadas para a construção de suas próprias bases de dados. Para participação no curso, espera-se conhecimento prévio da linguagem R ou uma preparação de nivelamento por meio de tutoriais indicados antes do início das aulas.

Esse repositório será alimentado ao longo do curso com roteiros de aula e tutoriais atualizados tentado atender as particularidades da turma.

### Dinâmica das aulas

As aulas terão conteúdo expositivo sobre conceitos e ferramentas básicas utilizados durante o curso, mas com um grande foco na realização de tutoriais assistidos. Trabalharemos em grupos, cada um em seu computador, com o professor acompanhando o andamento de cada um deles, tirando as dúvidas (sim, elas surgirão com frequência mesmo com a exposição anterior).

### Presença e avaliação

Em todos os roteiros teremos links para a sala do Zoom e para a lista virtual.

O requisito para a emissão de certificado é a presença em dois dos três encontros virtuais.

No entanto, ressalto a importância das atividades de terça e quinta-feira.

## Requisitos

### Preparação

A participação no curso requer uma exposição prévia à linguagem R e ao ambiente de tabalho do RStudio.

Caso não tenha nenhum contato com a linguagem, é mandatória a realização de um [roteiro de tutoriais de preparação](https://github.com/thiagomeireles/cebraplab_raspagem_2023/blob/main/roteiros/pre_curso/01_basico.md) antes do início das aulas. 

Ainda que tenha conhecimento básico das estruturas da linguagem, é fortemente recomendado que tambem o façam.

O tempo estimado para o tutorial é de *aproximadamente 4 horas*.

Além disso, um [Tutorial de manipulação de dados com dplyr](https://github.com/thiagomeireles/cebraplab_raspagem_2023/blob/main/tutoriais/pre_curso/Tutorial_05.md) é mandatório e tem tempo estimado para realização em *aproximadamente 2 horas*.

### Equipamento

Como a boa parte do curso é baseada em tutoriais em que vocês aprenderão "colocando a mão na massa", é essencial que acompanhem as aulas no computador.

### Softwares

Foi preparado um [Roteiro de instalação](https://github.com/thiagomeireles/cebraplab_raspagem_2023/blob/main/roteiros/pre_curso/00_instalacao.md) onde estão as instruções para a instalação dos softwares necessários.

## Objetivos

Os participantes, ao fim do curso, serão capazes de:
- Coletar dados de sites de estrutura mais simples, como jornais e legislativos brasileiros;
- Realizar tarefas básicas de mineração de texto mais focadas na manipulação dos dados

## Roteiros e tutoriais

### Roteiros

Todas os dias de curso terão roteiros a cumprir. Pouco antes de cada encontro, as linhas abaixo serão preenchidas com links com as descrições do que esperamos em cada dia de curso e como o faremos.

[Dia 1](https://github.com/thiagomeireles/cebraplab_raspagem_2023/blob/main/roteiros/dia_1.md) - O básico da raspagem de dados

[Dia 2](https://github.com/thiagomeireles/cebraplab_raspagem_2023/blob/main/roteiros/dia_2.md) - Aplicação da raspagem de dados em pesquisas

[Dia 3](https://github.com/thiagomeireles/cebraplab_raspagem_2023/blob/main/roteiros/dia_3.md) - Automatizando a raspagem de dados e manipulando o conteúdo

[Dia 4](https://github.com/thiagomeireles/cebraplab_raspagem_2023/blob/main/roteiros/dia_4.md) - Desafios de raspagem de dados com automatização da raspagem em *html* e novos desafios

[Dia 5](https://github.com/thiagomeireles/cebraplab_raspagem_2023/blob/main/roteiros/dia_5.md) - *RSelenium* e formulários


### Tutoriais

Os links para os tutoriais estarão abaixo minutos antes de cada aula.

[Tutorial 1](https://github.com/thiagomeireles/cebraplab_raspagem_2023/blob/main/tutoriais/tutorial_01.md): Introdução ao rvest e revisão. Versão [Rmd](https://github.com/thiagomeireles/cebraplab_raspagem_2023/blob/main/tutoriais/tutorial_01.Rmd)

[Tutorial 2](https://github.com/thiagomeireles/cebraplab_raspagem_2023/blob/main/tutoriais/tutorial_02.md): html com o sistema de busca da Folha de São Paulo. Versão [Rmd](https://github.com/thiagomeireles/cebraplab_raspagem_2023/blob/main/tutoriais/tutorial_02.Rmd)

[Tutorial 3](https://github.com/thiagomeireles/cebraplab_raspagem_2023/blob/main/tutoriais/tutorial_03.md): Automatizando a raspagem de notícias. [Versão Rmd](https://github.com/thiagomeireles/cebraplab_raspagem_2023/blob/main/tutoriais/tutorial_03.Rmd)

[Tutorial 4](https://github.com/thiagomeireles/cebraplab_raspagem_2023/blob/main/tutoriais/tutorial_04.md): Manipulação de texto no R: pacote _stringr_. [Versão Rmd](https://github.com/thiagomeireles/cebraplab_raspagem_2023/blob/main/tutoriais/tutorial_04.Rmd)

[Tutorial 5](https://github.com/thiagomeireles/cebraplab_raspagem_2023/blob/main/tutoriais/tutorial_05.md): Datafolha e download de conteúdo. [Versão Rmd](https://github.com/thiagomeireles/cebraplab_raspagem_2023/blob/main/tutoriais/tutorial_05.Rmd)

[Tutorial 6](https://github.com/thiagomeireles/cebraplab_raspagem_2023/blob/main/tutoriais/tutorial_06.md): Introdução RSelenium. [Versão Rmd](https://github.com/thiagomeireles/cebraplab_raspagem_2023/blob/main/tutoriais/tutorial_06.Rmd)

[Tutorial 7](https://github.com/thiagomeireles/cebraplab_raspagem_2023/blob/main/tutoriais/tutorial_07.md): Capturando as publicações do DOU. [Versão Rmd](https://github.com/thiagomeireles/cebraplab_raspagem_2023/blob/main/tutoriais/tutorial_07.Rmd)

[Tutorial 8](https://github.com/thiagomeireles/cebraplab_raspagem_2023/blob/main/tutoriais/tutorial_08.md): Formulários na web. [Versão Rmd](https://github.com/thiagomeireles/cebraplab_raspagem_2023/blob/main/tutoriais/tutorial_08.Rmd)


## Referências

- Grolemund, Garrett (2014). Hands-On Programming with R. Ed: O'Reilly Media. Disponível gratuitamente [aqui](https://rstudio-education.github.io/hopr/)
- Wichkam, Hadley e Grolemund, Garrett (2016). R for Data Science. Ed: O'Reilly Media. Disponível gratuitamente [aqui](http://r4ds.had.co.nz/data-visualisation.html)
- Wichkam, Hadley (2014). Advanced R. Ed: Chapman and Hall/CRC. Disponível gratuitamente Disponível gratuitamente [aqui](http://adv-r.had.co.nz/)
- Gillespie, Colin e Lovelace, Robin (2016). Efficient R programming. Ed: O'Reilly Media. Disponível gratuitamente [aqui](https://csgillespie.github.io/efficientR/)
