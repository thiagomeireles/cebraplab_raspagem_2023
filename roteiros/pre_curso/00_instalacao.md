# [Cebrap.lab 2023] Raspagem de dados com R: instalação do R e RStudio


Antes do curso, é esperado que todos os alunos instalem o R e o RStudio em suas máquinas. 

R é a linguagem que utilizaremos no curso e o software R apresenta somente um console de comandos para programação dessa linguagem.

Já o RStudio é um IDE (Integrated Development Environment ou Ambiente de Desenvolvimento Integrado) que é uma sigla bonita para falar de um programa mais amigável para trabalhar com a linguagem.

O curso todo será realizado no RStudio, mas para que funcione é necessária a instalação prévia do R.

No início da primeira aula faremos um tour rápido para entender o que esse IDE nos oferece.

## Instalação e atualização dos softwares

### Windows 10 ou 11

- Para instalar a versão mais recente de R, caso não possua uma instalação, clique [aqui](https://cran.r-project.org/).
- Para instalar a versão mais recente de R, caso já o possua instalado, execute o código abaixo e siga as instruções:

```{r, eval=FALSE}
install.packages("installr")
library(installr)
updateR()
```

- Para fazer download do RStudio, clique [aqui](https://www.rstudio.com/products/rstudio/download/#download). Mesmo você já tenha o RStudio instalado há algum tempo, repita este processo. As versões atualizadas costumam dar menos problemas e trazem algumas novas facilidades.

- **É muito imporante que tenham o R e o RStudio atualizados em sua máquina**.

### Mac e Linux

- Caso seu computador pessoal seja [Mac](https://www.datacamp.com/community/tutorials/installing-R-windows-mac-ubuntu) siga as instruções para instalação somente do R e do RStudio, ignorando os pacotes. Falaremos deles no início da primeira aula.

- Para usuários de [Linux](https://github.com/thiagomeireles/cebraplab_raspagem_2023/blob/main/tutoriais/pre_curso/00_instalacao_linux.md) foi elaborado um tutorial próprio.


**Em caso de problemas na realização dos tutoriais de instalação, entrem em contato via [e-mail](mailto:thiago.omeireles@gmail.com).**
