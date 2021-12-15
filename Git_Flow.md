# Modelo de desenvolvimento gitflow :package:

### Branches e a operação merge :man_student:

A definição dada na documentação do git é: "Ramificação (branche) significa que você diverge da linha principal de desenvolvimento e continua a trabalhar sem alterar essa linha principal.". Ao iniciar um repositório git somos apresentados a branche prioritária chamada de master ou main. Através dela podemos fazer outras bifurcações, que iniciam com o estado atual da branche master, ou da branch da qual ela bifurcou. Por exemplo podemos dentro do projeto do livro de receitas criar outra branche qualquer:

> **git** branch DIO -- Criando uma branche chamada DIO

Note que se entrarmos dentro da branche DIO, ela estará no mesmo estado que a branche master:

> **git** checkout DIO -- Entrando na branche DIO

Tal fato pode ser visto de maneira mais concreta com a comando "git log" que nos mostra os commits já realizados dentro de uma branche. O interessante do comando git log é que além de nos mostrar todos os comiits já realizados dentro da branche atual, nos mostra também em qual estado as outras branches estão:

> **git** log

Dessa forma podemos ver que que a branche master e DIO estão com o ponteiro HEAD no mesmo commit. Para visualizarmos a independência entre as branches, podemos iniciar um arquivo texto qualquer, realizar o commit,  e migrar novamente a branche master, e ver que o arquivo que criamos dentro da branch DIO, existe apenas nela:

> **git** add *
>
> **git** commit -m "Commit inicial branche DIO"
>
> **git** checkout master
>
> **ls**

Perceba que, após o comando ls para listar os arquivos e subdiretórios no diretório atual o arquivo texto criado na branche DIO não está inserido na branch master. Dessa maneira, se quisermos trabalhar com funcionalidades específicas no nosso projeto podemos criar uma branch para trabalhar em tal funcionalidade, e dessa maneira não afetar o projeto estável inserido na branche master. Ao criar uma branche para a implementação de uma funcionalidade, e finalizar tal implementação, bem como os teste associados, podemos adicionar a alterações na branche master através da operação de **merge**. Para sua execução devemos estar dentro da branche master (ou dentro da branch que queremos mesclar), e após dizer em linha de comando qual branche alternativa queremos mesclar na branche master:

> **git** checkout master
>
> **git** merge DIO

Após tais comandos, se listarmos novamente os arquivos do nosso diretório atual, poderemos ver que o arquivo criado inicialmente na branche DIO, está agora, na branche master.

A fim de vizualização, quero sallientar que também podemos criar bifurcações de outras bifurcações da brache master. Por exemplo podemos criar outra branche dentro da branche DIO:

> **git** chekcout DIO
>
> **git** branch Avanade_bootcamp

De tal forma que chegamos a estruturas como:





# Git flow :arrow_backward:

Git flow é um fluxo de trabalho para a tecnologia git, de maneira menos formal é apenas uma ideia abstrata, que nos apresenta boas práticas e configurações para otimizarmos nossa produção. O gitflow nos apresenta uma estrutura de branches bem definidas, e metodologias para a realização de merges entre elas, de forma a promover mais organização e segurança no versonamento de código. Dessa maneira, a vantagem de adotar fluxos de trabalho, como o git flow, é conseguir uma maior organização em projetos em equipe usando o git como tecnologia de versionamento. O git flow especificamente é apropriada para projetos que tem data periódica de lançamentos, o que será melhor explicado posteriormente.



## Estrutura do gitflow :

O gitflow é baseado em algumas ramificação primárias que falaremos sobre cada uma delas a seguir, e ramificações de funcionalidades.

### Branch Master

A branch master é disparada ao iniciarmos um repositório local git. Dentro da estrutura presente no gitflow ela representa o nosso projeto a nível de consumo, isto é, a branch master constiui as versões estáveis do nosso projeto, prontas para serem usadas pelos clientes.

### Branch Develop

Bifurcando da ramificação master, a branch develop serve como uma branch para integração de funcionalidades. Isto é, eu vejo a função develop como antigamente eu via a branch master, quando criava alguma funcionalidade nova para meu projeto, usava uma bifurcação da master. A develop server exatamente para isso. Enquanto a branche master tem as versões estabilizadas do projeto, pronto para lançamento, a develop armazena o histórico do projeto. Dessa forma, quando clonamos um projeto usando gitflow em nossa máquina local, rastreamos a branche develop.

### Branch Feature

A branch feature advém da branch develop e podemos ter infinitas branches feature em nosso projeto usando gitflow. A ideia é, ter uma branch feature para cada funcionalidade que estamos trabalhando. Vendo através de um exemplo de um projeto simples, ao implementarmos uma calculadora, podemos ter uma branch feature para cada operação que nossa calculadora poderá realizar em nível de implementação. A questão é que teremos branches independentes para trabalhar em funcionalidades específicas, que quando implementadas incluímos na branch develop através da operação de merge. Quando criamos uma nova branch feature, bifurcamos a branch develop em seu estado mais recente.



### branch Release

A branch release como a branch feature, é uma bifurcação da branch develop. Tal branch é iniciada quando já se tem o suficiente para um novo lançamento de versão. Ela é dedicada para resolução de questões sobre o lançamento de uma versão, isto é, para questões como documentação, questões de segurança, especificação, etc, salientando-se que nessa branch, não pode ser desenvolvida novas funcionalidades. Possuir tal branch na estrutura do gitflow nos possibilita continuar trabalhando já em novas versões dentro da branch develop, enquanto outros detalhes estão sendo resolvidos para o lançamento da versão atual. Com a versão totalmente pronta, faz-se um merge da branch release para a branhc master, comittando de maneira a especificar a nova versão. Note que deve-se também fazer o merge com a branch develop para integrar os detalhes solucionados.



### branche hotfix

Branch hotfix é uma bifurcação da master, sua funcionalidade é servir como canal para trabalhar em atualizações a nível de consumo de maneira rápida. Com ela pode-se corrigir pequenas falhas encontradas em uma versão estável do projeto sem ter que atrapalhar o fluxo de trabalho. Assim que concluída a correção, deve ser feito o merge dela quanto na branch master, especificando a nova versão, quanto na branch develop, e se ativa, na branch release.



## Pequena prática:

Através do projeto criado no tópico do github mestrada pelo Otávio Reis vou tentar praticar o gitflow e compartilhar com vocês o acompanhamento do processo, para que possamos discutir e aprender juntos. Dessa forma, com algumas operações voltarei o projeto ao estado presente no final do tópico de git e github:

>git branch -d DIO
>
>git branch -d Avanade_bootcamp

Ok! Dessa forma, voltamos a um repositório local com apenas a branch master, possuindo o arquivo README.md e um diretório nomeado como "Receitas", cujo armazena o arquivo "Strogonoff.md". Vou passar a dividir o livro de receitas em gêneros, por exemplo sobremesas, almoço, café da manhã, etc. Dessa formas continuaremos como a seguir:

> mkdir Almoco && mkdir sobremesas -- Criação de dois diretórios

Dessa maneira, vou mover a receita de Strogonoff para o diretório Almoco:

> mv Strogonoff.md ./Almoco -- Movendo o arquivo strogonoff para o diretório Almoco

Após isso, podemos comittar e empurrar a alteração para o servidor github:

> cd .. && git add . && git commit -m "Atualização da estrutura do livro de receitas" && git push origin master

Com este estado atual, imagino que podemos ver a branch master agora como uma versão estável do livro de receita, passível para um leitor lê-lo. Para continuarmos desenvolvendo o livro para futuras versões, criemos agora a branch develop:

> git branch develop 

Com isso, supomos que queremos redigir no livro de receitas a sobremesa pavê, nesse caso, seguindo o fluxo de trabalho criaremos uma branch de feature para implementar essa "funcionalidade":

> git checkout develop
>
> git checkout -b feature_pave -- Ramificando uma branch na branch develop e entrando nela

Estando presenta na branch voltada para a nova funcionalidade, podemos criar um arquivo referente a receito do pavê, e atualizar o arquivo com a receita correspondente. Após finalizado, efetuaremos o commit na branch feature

> git add . && git commit -m "adição da receita de pavê"

Dessa forma, finalizamos a "funcionalidade" da receita de pavê. Podemos agora finalizar a branch feature_pave. Desse modo, temos que fazer o merge das nossas alterações realizadas através da branch feature, na branch develop:

> **git** checkout develop
>
> **git** merge feature_pave

Supomos agora que com a adição dessa única receita, possamos lançar uma nova versão do livro de receita. Seguindo essa ideia, a branch develop está trabalhada o suficiente para criarmos uma branch de Realease. Com isso dentro da branch develop, podemos:

> **git** chekcout -b realease/0.1

Imaginando que, resolvemos todos os detalhes para o lançamento dessa nova versão, podemos, após commitar as alterações, fazer o merge na branch master, que representa o projeto como um todo, a nível de consumo.

> **git** checkout master
>
> **git** merge realease/0.1

Agora, supomos que após a integralização com a master, encontremos um erro na receita pavê: faltou um ingrediente. Para solucionarmos o pequena problema, seguindo o fluxo de trabalho gitflow, podemos criar uma branch hotfix, bifurcando a branch master:

> git checkout -b hotfix_pave

Neste momento, dentro da branch hotfix podemos solucionar nosso pequeno problema. Após solucionado, e commitado as alterações (adição do ingrediente que estava faltando) podemos finalizar a branch de hotfix, mesclando-a na branch master, agora com as correções feitas.

> git checkout master
>
> git merge hotfix_pave

Dessa forma, conseguimos exemplificar a funcionalidade de todas as branches usadas no fluxo de trabalho gitflow.



# Perguntas :question:

> É dito neste fluxo de trabalho que ao lançar uma nova versão tanto através daa branch release, tanto da branch hotifx, que deve-se especificar a versão na master. Como essa versão é especificada, se ao fazer a operação de merge a branch master não possui dados para serem commitados ?



















