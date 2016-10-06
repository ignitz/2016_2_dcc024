# Linguagens de Programação

* * *

## Índice de Aulas

1.  [01/08 - Introdução - A grande diversidade](#lesson01)
2.  [08/08 - Sintaxe](#lesson02)
3.  [15/08 - Mais sobre árvores de sintaxe](#lesson03)
4.  [17/08 - Sistemas de Computação](#lesson04)
5.  [XX/XX - Introdução a ML](#lesson05)
6.  [XX/XX - Casamento de padrões em ML](#lesson06)
7.  [XX/XX - Tipos de dados](#lesson07)
8.  [XX/XX - Polimorfismo](#lesson08)
9.  [XX/XX - O cálculo lambda](#lesson09)
10.  [XX/XX - Funções de alta ordem](#lesson10)
11.  [19/09 - Escopo de variáveis](#lesson11)
12.  [26/09 - Tipos algébricos](#lesson12)
13.  [28/09 - Registros de ativação](#lesson13)  
    03/10 - Aula de revisão  
    05/10 - Primeira avaliação
14.  [10/10 - Introdução à linguagem Python](#lesson14)
15.  [10/10 - Gerenciamento de memória](#lesson15)
16.  [17/10 - Tipos abstratos de dados](#lesson16)
17.  [19/10 - Programação orientada a objetos](#lesson17)
18.  [24/10 - Tratamento de erros](#lesson18)
19.  [26/10 - Passagem de parâmetros](#lesson19)
20.  [31/10 - Introdução à linguagem Prolog](#lesson20)
21.  [31/10 - Unificação](#lesson21)
22.  [02/11 - Modelo de Custos](#lesson22)
23.  [07/11 - Predicados numéricos em Prolog](#lesson23)
24.  [09/11 - Semântica Formal](#lesson24)
25.  [14/11 - História das linguagens de programação](#lesson25)

* * *

<a name="lesson01"></a>

## Introdução

### Conceitos que devem ser entendidos.

*   Existem linguagens de programação imperativas, funcionais e lógicas. A fronteira entre estes paradigmas não é bem definida.
*   Diferentes paradigmas facilitam a implementação de diferentes tipos de aplicações.

### Questões para discutir

*   Por que existem linguagens de programação?
*   Existe uma linguagem de programação mais "poderosa"?
*   Por que existem **tantas** linguagens de programação?
*   Por que algumas linguagens de programação são mais populares que outras?
*   Por que algumas linguagens de programação são mais eficientes que outras?
*   O que é uma boa linguagem de programação?
*   Por que é importante aprender sobre linguagens de programação?
*   Considere a sua linguagem de programação favorita. Há algo que você adicionaria ou mudaria nesta linguagem?
*   Pense em exemplos de aplicações que são mais facilmente implementadas em cada um dos três paradigmas principais de linguagens de programação.

### Leitura

*   [Introdução à Linguagens de Programação](readingMat/IntroLecture.pdf)
*   [A pesquisa em Linguagens de Programação na UFMG](readingMat/PesquisaLLP.pdf)
*   Why you should learn other [languages](http://www.joelonsoftware.com/articles/ThePerilsofJavaSchools.html).
*   **Importante**: Capítulo 1 do livro [Introduction to Programming Languages](http://en.wikibooks.org/wiki/Introduction_to_Programming_Languages).

### Exercícios

1.  Para ser entregue: [Lista 1](listas/lista1.pdf). Lembre-se: respostas escritas à mão.  

2.  (**P.O.F**): Abrace uma pessoa pela primeira vez.

[Notas e exemplos](lesson1) usados em aula.

* * *

<a name="lesson02"></a>

## Sintaxe

### Conceitos que devem ser entendidos.

*   A sintaxe e estrutura léxica de uma linguagem de programação determinam a aparência de seus programas
*   A semântica de uma linguagem de programação determina o significado de cada um de seus construtos.
*   Como escrever uma gramática simples.

### Questões para discutir

*   Haveria algum outro jeito de descrever uma linguagem de programação?
*   Qual o problema de representar os tokens dentro da gramática?

### Leitura

*   [Sintaxe de Linguagens de Programação](http://www.cs.uiowa.edu/~slonnegr/plf/Book/Chapter1.pdf) Seções 1.1, 1.2 e 1.3.
*   Um pouco de motivação com o famoso triângulo de [Sierpinski](readingMat/sierpinski.pdf) .
*   Uma linguagem que gera sistemas-L: [LinF](http://www.dcc.ufmg.br/~fernando/projects/LinF) .
*   Exemplo de gramática. Neste caso, trata-se de [minijava](http://compilers.cs.ucla.edu/cs132/project/minijava.html) , um subconjunto de Java usado em trabalhos práticos.
*   **Importante**: Capítulo 2 do livro [Introduction to Programming Languages](http://en.wikibooks.org/wiki/Introduction_to_Programming_Languages).

### Exercícios

1.  Para ser entregue: [Lista 2](listas/lista2.pdf). Lembre-se: respostas escritas à mão.  

2.  (**P.O.F**): Escolha uma figura histórica e escreva uma carta para essa pessoa, contando como está o mundo hoje. Meio ponto extra para quem postar em nossa lista de discussão. Alguns P.O.F.s valem pontos extra, mas você pode conseguir somente meio ponto extra total via P.O.F.s.

[Notas e exemplos](lesson2/) usados em aula.

* * *

<a name="lesson03"></a>

## Mais sobre árvores de sintaxe.

### Conceitos que devem ser entendidos.

*   Precedência entre operadores
*   Associatividade
*   Gramáticas ambíguas permitem a construção de duas diferentes árvores de sintaxe para o mesmo texto.
*   Árvores de sintaxe abstrata

### Questões para discutir

*   Quantos níveis de precedência possui a sua linguagem favorita?
*   O que significa este trecho de código C: "`a = b < c ? * p + b * c : 1 << d ()`"?
*   Qual é a história de gramáticas em linguagens de programação?

### Leitura

*   [Sintaxe de Linguagens de Programação](http://www.cs.uiowa.edu/~slonnegr/plf/Book/Chapter1.pdf) Seções 1.4.
*   [Gramáticas em Prolog](http://www.cs.uiowa.edu/~slonnegr/plf/Book/Chapter2.pdf) Páginas 46 (Prolog Grammar Rules) até 50.
*   [Tabela](http://www.uni-bonn.de/~manfear/javaoperators.php) de precedência em Java.
*   [Tabela](http://www.ime.usp.br/~pf/algoritmos/apend/precedence.html) de precedência em C.
*   **Importante**: Capítulo 3 do livro [Introduction to Programming Languages](http://en.wikibooks.org/wiki/Introduction_to_Programming_Languages).

### Exercícios

1.  Para ser entregue: [Lista 3](listas/lista3.pdf). Lembre-se: respostas escritas à mão.  

2.  (**P.O.F.**): Faça um desenho de quem você goste. Novamente, meio ponto extra para quem postar em nosso grupo de discussão **um trabalho feito durante este semestre**. O trabalho precisa ser feito à mão, e não valem desenhos estilo [boneco-palito](http://en.wikipedia.org/wiki/Stick_figure)!

[Notas e exemplos](lesson3) usados em aula.

* * *

<a name="lesson04"></a>

## Sistemas de Computação

### Conceitos que devem ser entendidos.

*   Compiladores. Ex: gcc
*   Assemblers. Ex: gcc -S
*   Linkers. Ex: gcc -c
*   Loaders. Ex: gcc -ld
*   Interpretadores. Ex: SML/NJ (próxima aula)
*   Máquinas virtuais. Ex: JavaVM
*   Depuradores. Ex: gdb
*   Just in time Compilers. Ex: Mozilla lendo Javascript.
*   Tempo de ligação:
    *   Durante a definição da linguagem
    *   Durante a implementação da linguagem
    *   Durante a compilação
    *   Durante a ligação (link time)
    *   Durante o carregamento (load time)
    *   Durante a execução (run time).

### Questões para discutir

*   O que acontece quando você digita `gcc main.c ; ./a.out` na linha de comando e aperta `enter`?
*   O que acontece quando você digita `javac Main.java ; java Main` na linha de comando e aperta `enter`?
*   O que acontece quando seu navegador executa uma applet Java?
*   Como funciona gdb?
*   Quais as vantagens de um compilador sobre um interpretador? E vice-versa?

### Leitura

*   [Compiladores](http://en.wikipedia.org/wiki/Compiler)
*   [Debugadores](http://en.wikipedia.org/wiki/Debugger)
*   Um [tutorial](http://www.cs.cmu.edu/~gilpin/tutorial/) sobre gdb.
*   [Interpretadores](http://en.wikipedia.org/wiki/Interpreter_(computing))
*   [Máquinas virtuais](http://en.wikipedia.org/wiki/Virtual_machine)
*   [Compilador just-in-time.](http://en.wikipedia.org/wiki/Just_in_time_compiler)
*   **Importante**: Capítulo 4 do livro [Introduction to Programming Languages](http://en.wikibooks.org/wiki/Introduction_to_Programming_Languages).

### Exercícios

1.  Para ser entregue: [Lista 4](listas/lista4.pdf). Lembre-se: respostas escritas à mão.  

2.  (**P.O.F.**): Dê uma flor para uma pessoa que você encontre na rua, e que nunca tenha visto antes.

[Notas e exemplos](lesson4) usados em aula.

* * *

<a name="lesson05"></a>

## Introdução à ML

### Conceitos que devem ser entendidos.

*   Máquina de Turing versus Cálculo Lambda.
*   ML é uma linguagem funcional.
*   O conceito de referência não é definido em ML básica.
*   Tuplas e listas são tipos de dados em ML.
*   Construtores de tipos, por exemplo '*', '->' e list.
*   Recursividade de Cauda

### Questões para discutir

*   Quais as semelhanças entre ML e Java?
*   E quais as diferenças entre essas linguagens?
*   Quais são as vantagens de se usar uma linguagem sem referências para dados mutáveis?

### Leitura

*   [SML](http://en.wikipedia.org/wiki/Standard_ML)
*   [Tutorial](http://homepages.inf.ed.ac.uk/stg/NOTES/) sobre SML (contribuição do Wladston).

### Exercícios

1.  Para ser entregue: [Lista 5](listas/lista5.pdf). Lembre-se: respostas escritas à mão.  

2.  (**P.O.F.**): Tire uma foto artística de algum lugar/coisa no Campus.

[Notas e exemplos](lesson5) usados em aula.

* * *

<a name="lesson06"></a>

## Casamento de Padrões em ML

### Conceitos que devem ser entendidos.

*   Casamento de padrões são típicos em linguagens funcionais.

### Questões para discutir

*   Por que linguagens como C ou Java não possuem casamento de padrões?

### Leitura

*   [Casamento de padrões](http://en.wikipedia.org/wiki/Pattern_matching)

### Exercícios

1.  Para ser entregue: [Lista 6](listas/lista6.pdf). Lembre-se: respostas escritas à mão.  
    Por favor, consulte os [slides](http://www.webber-labs.com/wp-content/uploads/2015/08/mpl-07.pdf) do livro para resolver estes exercícios.  

2.  (**P.O.F.**): Tome um banho de chuva.

[Notas e exemplos](lesson6) usados em aula.

* * *

<a name="lesson07"></a>

## Tipos de dados

### Conceitos que devem ser entendidos.

*   Um tipo de dado é um conjunto.
*   Existem tipos de dados primitivos e compostos.
*   Linguagens fortemente tipadas versus linguagens fracamente tipadas.
*   Verificação de tipos dinâmica versus verificação estática.
*   Equivalência de tipos estrutural versus equivalência nominal.

### Questões para discutir

*   Encontre exemplos de tipagem fraca na linguagem C.
*   Compare verificação dinâmica versus estática.
*   Compare equivalência estrutural versus equivalência nominal.
*   Em equivalência estrutural, int * int * int é um subtipo de int * int. Explique este paradoxo.

### Leitura

*   [Tipos de dados](http://en.wikipedia.org/wiki/Type_system)
*   [What to know before debating about types](http://blogs.perl.org/users/ovid/2010/08/what-to-know-before-debating-type-systems.html)
*   **Importante**: Capítulo 7 (Types) do livro [Introduction to Programming Languages](http://en.wikibooks.org/wiki/Introduction_to_Programming_Languages).

### Exercícios

1.  Para ser entregue: [Lista 7](listas/lista7.pdf). Lembre-se: respostas escritas à mão.  
    Por favor, consulte os [slides](http://www.webber-labs.com/wp-content/uploads/2015/08/mpl-06.pdf) do livro para resolver estes exercícios.  

2.  (**P.O.F.**): Passe a tarde na piscina do CEU (assim que ela for consertada).

[Notas e exemplos](lesson7) usados em aula.

* * *

<a name="lesson08"></a>

## Polimorfismo

### Conceitos que devem ser entendidos.

*   Uma função é polimórfica se ela tem pelo menos dois tipos diferentes.
*   Polimorfismo ad-hoc (número finito de variações):
    *   Overloading
    *   Coerção
*   Polimorfismo universal (número potencialmente infinito de variações):
    *   Polimorfismo paramétrico
    *   Polimorfismo de subtipagem

### Questões para discutir

*   Como polimorfismo facilita a programação?
*   Compare polimorfismo paramétrico em Java e ML

### Leitura

*   [Polimorfismo de tipos](http://en.wikipedia.org/wiki/Polymorphism_(computer_science))
*   [Polimorfismo em linguagens orientadas a objetos.](http://en.wikipedia.org/wiki/Polymorphism_in_object-oriented_programming#Parametric_Polymorphism)

### Exercícios

1.  Para ser entregue: [Lista 8](listas/lista8.pdf). Lembre-se: respostas escritas à mão.  

2.  (**P.O.F.**): Aprenda um poema de cor.

[Notas e exemplos](lesson8) usados em aula.

* * *

<a name="lesson09"></a>

## O Cálculo Lambda

### Conceitos que devem ser entendidos.

*   O Cálculo Lambda é uma notação para descrever computações tão poderosa quanto o máquina de Turing.
*   Variáveis livres.
*   Funções Currificadas.
*   Reduções Lambda:
    *   Reduções alfa.
    *   Reduções beta.
    *   Reduções eta.
*   Existem diferentes estratégias de redução.

### Questões para discutir

*   Por que Máquinas de Turing são utilizadas no ensino de fundamentos de computação, ao invés do Cálculo Lambda?

### Leitura

*   [O Cálculo Lambda.](http://www.cs.uiowa.edu/~slonnegr/plf/Book/Chapter5.pdf) Seções 5.1 e 5.2, i.e, pag 139 a 159
*   Representando [números](http://www.inf.fu-berlin.de/lehre/WS03/alpi/lambda.pdf) e valores booleanos usando lambdas.
*   Um pouco da história da [computabilidade.](http://www.people.cs.uchicago.edu/~soare/History/turing.pdf) Leia até a seção três.
*   a [filosofia](http://www.iep.utm.edu/lambda-calculi/) e o cálculo lâmbda.
*   [Slides para a aula.](readingMat/lambdaHistory.pdf)

### Exercícios

1.  Para ser entregue: [Lista 9](listas/lista9.pdf). Lembre-se: respostas escritas à mão.  

2.  (**P.O.F.**): Tire uma foto do por do sol.

[Notas e exemplos](lesson9) usados em aula.

* * *

<a name="lesson10"></a>

## Funções de Alta Ordem

### Conceitos que devem ser entendidos.

*   Uma função que não usa outra função como parâmetro ou valor de retorno possui ordem zero.
*   Uma função que usa outra função como parâmetro ou valor de retorno possui ordem _n+1_, onde _n_ é a mais alta ordem entre as funções usadas.
*   `map`, `foldr` e `foldl` são funções de alta ordem.

### Questões para discutir

*   Como são funções de alta ordem em C?
*   Como são funções de alta ordem em Java?
*   Por que essas funções demoraram tanto para aparecer em Java?

### Leitura

*   [Funções de alta ordem](http://en.wikipedia.org/wiki/Higher-order_function)
*   **Importante**: Capítulo 11 (High-Order Functions) do livro [Introduction to Programming Languages](http://en.wikibooks.org/wiki/Introduction_to_Programming_Languages).

### Exercícios

1.  Para ser entregue: [Lista 10](listas/lista10.pdf). Lembre-se: respostas escritas à mão.  

2.  (**P.O.F.**): Tente ficar de cabeça para baixo no gramado da reitoria.

[Notas e exemplos](lesson10) usados em aula.

* * *

<a name="lesson11"></a>

## Escopo

### Conceitos que devem ser entendidos.

*   O escopo de uma definição é a parte to programa onde aquela definição é válida
*   Linguagens de programação usam blocos e espaços de nomes para criar escopos
*   Escopo estático versus escopo dinâmico
*   Closures

### Questões para discutir

*   Qual é a vantagem de linguagens com escopo dinâmico.
*   Quais os mecanismos usados por sua linguagem favorita para criar escopos?

### Leitura

*   [Structs e Signatures em SML](http://people.cs.umass.edu/~creichen/csci3155-s07/handout-F.pdf)
*   [Escopo](http://en.wikipedia.org/wiki/Scope_(programming))
*   [Closures](http://en.wikipedia.org/wiki/Closure_(computer_science))

### Exercícios

1.  Para ser entregue: [Lista 11](listas/lista11.pdf). Lembre-se: respostas escritas à mão.  

2.  (**P.O.F.**): Realize um P.O.F. tribal. Esse P.O.F. vale 0.5 pontos para cada membro do grupo que cumprir todas as [regras](pofs/group.html).

[Notas e exemplos](lesson11) usados em aula.

* * *

<a name="lesson12"></a>

## Tipos Algébricos

### Conceitos que devem ser entendidos.

*   Em ML, novos tipos de dados são definidos com a palavra `datatype`
*   `Bool` e `list` são tipos algébricos
*   Construtores de tipos.
    *   Simples
    *   Com valores-parâmetros
    *   Recursivos

### Questões para discutir

*   Compare a implementação de árvores de dados em ML e em Java
*   Quais os aspectos mais interessantes de ML?
*   Por que linguagens funcionais são pouco populares?

### Leitura

*   [Tipos de dados algébricos.](http://en.wikipedia.org/wiki/Algebraic_data_type)
*   [Datatypes em SML.](http://www2.imm.dtu.dk/courses/02153/fun/chapter7.4.pdf)
*   [Por que linguagens funcionais não são tão populares.](http://portal.acm.org/citation.cfm?id=286387)

### Exercícios

1.  Para ser entregue: [Lista 12](listas/lista12.pdf). Lembre-se: respostas escritas à mão.  

2.  (**P.O.F.**): Ligue para a sua mãe para dizer-lhe que a ama.

[Notas e exemplos](lesson12) usados em aula.

* * *

<a name="lesson13"></a>

## Registros de ativação

### Conceitos que devem ser entendidos.

*   As variáveis em um programa precisam ser armazenadas em algum lugar
*   Registros de ativação (_activation record_)
    *   Estático
    *   Dinâmico
*   Ponteiro de aninhamento (_nesting link_)
*   Variáveis estáticas

### Questões para discutir

*   Como recursão de cauda é otimizada?
*   O que existe em um registro de ativação?

### Leitura

*   [Pilha de Memória (stack frame)](http://en.wikipedia.org/wiki/Call_stack)
*   [Registro de ativação](http://www.cs.princeton.edu/courses/archive/spring04/cos320/notes/7-1.pdf)

### Exercícios

1.  Para ser entregue: [Lista 13](listas/lista13.pdf). Lembre-se: respostas escritas à mão.  

2.  (**P.O.F.**): Escreva um [haiku](http://en.wikipedia.org/wiki/Haiku). Meio ponto extra para quem postar em nossa lista de discussão um trabalho original, feito **neste** semestre.

[Notas e exemplos](lesson13) usados em aula.

* * *

## Aula de Revisão.

Dicas para se dar bem na prova:

1.  Entenda as notas e exemplos dados em aula.
2.  Leia os [Slides](http://www.webber-labs.com/mpl/lectures/index.html) do livro.
3.  Faça as listas de exercícios.
4.  Faça todos os P.O.F's.

Abaixo estão alguns exemplos de provas dados em anos anteriores. Se quiserem uma dica, chamem seus colegas, amuquifem-se no reduto de algum deles, e dêem uma olhada em cada questão. Se ficarem cansados, parem tudo para escutar [Build Me Up Buttercup](http://www.youtube.com/watch?v=iol0B-clFFM):

*   [Prova 1](exams/midterm1.pdf).
*   [Prova 2](exams/midterm2.pdf).
*   [Prova 3](exams/midterm3.pdf).
*   [Prova 4](exams/midterm4.pdf).
*   [Prova 5](exams/midterm5.pdf).
*   [Prova 6](exams/midterm6.pdf).
*   [Prova 7](exams/midterm7.pdf).
*   [Prova 8](exams/midterm8.pdf).
*   [Prova 9](exams/midterm9.pdf).
*   [Prova 10](exams/midterm10.pdf).
*   [Prova 11](exams/midterm11.pdf).
*   [Prova 12](exams/midterm12.pdf).
*   [Prova 13](exams/midterm13.pdf).
*   [Prova 14](exams/midterm14.pdf).
*   [Prova 15](exams/midterm15.pdf).
*   [Prova 16](exams/midterm16.pdf).

* * *

## Primeira avaliação.

<span class="entryTitle">Duração:</span> <span class="entryInf">90 minutos.</span>  
<span class="entryTitle">Valor:</span> <span class="entryInf">30 Pontos.</span>  
<span class="entryTitle">Conteúdo:</span> <span class="entryInf">Toda a matéria até a aula de revisão</span>  
<span class="entryTitle">Modo:</span> <span class="entryInf">Sem consulta.</span>

* * *

<a name="lesson14"></a>

## Introdução à Linguagem Python

### Conceitos que devem ser entendidos.

*   Linguagens Imperativas
    *   Referências
    *   Comandos e iterações
    *   Efeitos colaterais (_Side Effects_)
*   Linguagens dinâmicas
    *   Tipagem dinâmica
    *   Execuçã de código carregado dinamicamente
    *   Alocação dinâmica de memória
*   Máquinas virtuais, Interpretadores

### Questões para discutir

*   Qual o por quê dos princípios de programação que norteiam o projeto de Python? Em que outras linguagens estes princípios são vistos?
*   Quais as vantagens e desvantagens de máquinas virtuais?

### Leitura

*   [Site official de Python](http://www.python.org/)
*   [Python wiki](http://en.wikipedia.org/wiki/Python_%28programming_language%29)

*   O [tutorial](http://docs.python.org/tutorial/) definitivo sobre Python.

### Exercícios

1.  Para ser entregue: [Lista 14](listas/lista14.pdf). Lembre-se: respostas escritas à mão.  

2.  (**P.O.F.**): Assista um dos filmes que ganharam o Oscar de melhor filme em língua estrangeira.

[Notas e exemplos](lesson17) usados em aula.

* * *

<a name="lesson15"></a>

## Gerenciamento de memória

### Conceitos que devem ser entendidos.

*   Memory Heap versus Memory Stack
    *   Função
    *   Forma de implementação
*   O gerenciamento do Heap
    *   Como encontrar novos blocos vagos: First-fit e Best-fit
    *   Fragmentação e coalescing (fusão)
    *   Coleta de Lixo
        *   Marcação e varredura (mark and sweep)
        *   Cópia e coleta
        *   Contagem de referências (reference counting)
*   Erros devido ao mau uso da memória
    *   Vazamentos (memory leak)
    *   Ponteiro cego (dangling pointer)

### Questões para discutir

*   Como programadores gerenciam memória em C/C++?
*   Valgrind

### Leitura

*   Diversas técnicas de [Gerenciamento de memória](http://www.ibm.com/developerworks/linux/library/l-memory/)
*   [Coleta de Lixo](http://en.wikipedia.org/wiki/Garbage_collection_(computer_science))
*   [Valgrind](http://en.wikipedia.org/wiki/Valgrind)

### Exercícios

1.  Para ser entregue: [Lista 15](listas/lista15.pdf). Lembre-se: respostas escritas à mão.  
    Esta classe será necessária:
    *   [HeapManager.py](srcs/l15/HeapManager.py)
2.  (**P.O.F.**): Saia a noite para dançar _rock'n roll_.

[Notas e exemplos](lesson18) usados em aula.

* * *

<a name="lesson16"></a>

## Tipos Abstratos de Dados

### Conceitos que devem ser entendidos.

*   Um tipo abstrato de dados é um tipo definido pelas operações e dados ao invés de ser definido como um conjunto de valores.
*   Diferentes linguagens fornecem diferentes recursos para a implementação de Tipos Abstratos de Dados:
    *   **C**: descrição de interfaces em _header files_ e implementações em arquivos c.
    *   **SML**: _signatures_ para a descrição de interfaces e _structs_ para as implementações.
    *   **Python**: classes sem mecanismos de encapsulação.
    *   **Java**: _interfaces_ para a descrição de interfaces e _classes_ para as implementações. Encapsulação de propriedades.
*   Algumas linguagens, como Python, provêem herança múltipla (mas isto traz problemas).

### Questões para discutir

*   Herança múltipla: bom ou ruim?
    *   Como simular herança múltipla?
*   Como simular polimorfismo paramétrico?

### Leitura

*   [Tipos abstratos de dados](http://en.wikipedia.org/wiki/Abstract_data_type)
*   [Herança Múltipla](http://en.wikipedia.org/wiki/Multiple_inheritance)
*   [Mixins](http://en.wikipedia.org/wiki/Mixin)

### Exercícios

1.  Para ser entregue: [Lista 16](listas/lista16.pdf). Lembre-se: respostas escritas à mão.  
    As classes nos arquivos [List.py](srcs/l16/List.py) e [Worklist.py](srcs/l16/Worklist.py) serão necessárias.
2.  (**P.O.F.**): Aprenda a cozinhar um prato novo.

[Notas e exemplos](lesson19) usados em aula.

* * *

<a name="lesson17"></a>

## Programação Orientada à Objetos

### Conceitos que devem ser entendidos.

*   Programação Orientada à Objetos não é exatamente um paradigma de linguagens de programação; é mais uma filosofia, um estilo de programação.
    *   Visão antropomórfica de objetos: um objeto sabe como executar ações em si mesmo.
*   Linguagens orientadas por objetos possuem algumas funcionalidades comuns:
    *   Objetos.
    *   Métodos.
    *   Subtipagem.

### Questões para discutir

*   Por que a programação orientada por objetos é tão popular na indústria?

### Leitura

*   [Programação Orientada à Objetos](http://en.wikipedia.org/wiki/Object-oriented_programming)
*   [POO em Python.](http://www.ibiblio.org/g2swap/byteofpython/read/oops.html)
*   [POO em Java.](http://java.sun.com/docs/books/tutorial/java/concepts/)
*   [Nem todo mundo gosta de POO.](http://www.reocities.com/tablizer/oopbad.htm)

### Exercícios

1.  Para ser entregue: [Lista 17](listas/lista17.pdf). Lembre-se: respostas escritas à mão.  
    Estas [classes](srcs/l17/) podem facilitar o seu trabalho.
2.  (**P.O.F.**): Viage com dois ou mais de seus colegas de aula.

[Notas e exemplos](lesson20) usados em aula.

* * *

<a name="lesson18"></a>

## Tratamento de Erros

### Conceitos que devem ser entendidos.

*   Exceções são eventos anormais que podem ocorrer durante a execução de um programa.
*   Mecanismos de tratamento de exceções em SML:
    *   Podemos disparar exceções com `raise`.
    *   Declaramos as exceções com `exception`.
    *   Capturamos as exceções com `handle`.
*   Mecanismos de tratamento de exceções em Python:
    *   Blocos `try` e `except`.
    *   Não é necessário estender uma classe em particular.
    *   `finally`.
*   Mecanismos de tratamento de exceções em Java:
    *   Blocos `try` e `catch`.
    *   Declarações `throws` e classes `throwable`.
    *   `finally` e `finalize`.
*   Em Java, exceções podem ser verificáveis (`checked`) ou não-verificáveis (`unchecked`).

### Questões para discutir

*   Qual é o papel de exceções em guarantir a forte tipagem de linguagens tais como Python, Java e ML?
*   Quais as vantagens e desvantagens entre exceções `checked` e `unchecked`?

### Leitura

*   Como exceções são implementadas em [C++](http://www.codeproject.com/kb/cpp/exceptionhandler.aspx).
*   10 boas [dicas](http://www.wikijava.org/wiki/10_best_practices_with_Exceptions) para o tratamento de exceções.
*   [Tratamento de exceções em linguagens de programação.](http://en.wikipedia.org/wiki/Exception_handling)
*   [Exceções em Python.](http://docs.python.org/tutorial/errors.html)
*   Mais [exceções em Java.](http://java.sun.com/docs/books/tutorial/essential/exceptions/)
*   [Exceções `checked` e `unchecked`.](http://www.javapractices.com/topic/TopicAction.do?Id=129)

### Exercícios

1.  Para ser entregue: [Lista 18](listas/lista18.pdf). Lembre-se: respostas escritas à mão.  
    Talvez você queira usar as classes no arquivo [Emp.py](srcs/l18/Emp.py).
2.  (**P.O.F.**): Ligue para um amigo que você já não vê faz tempo.

[Notas e exemplos](lesson21) usados em aula.

* * *

<a name="lesson19"></a>

## Passagem de parâmetros

### Conceitos que devem ser entendidos.

*   Parâmetros formais versus parâmetros reais.
*   Correspondência posicional versus correspondência nominal.
    *   Em correspondência posicional, parâmetros formais são casados com parâmetros reais de acordo com a posição na chamada da função. Exemplo: quase todas as linguagens de programação.
    *   Em correspondência nominal, parâmetros são anexados a nomes, que os identificam durante a invocação da função. Exemplo: Ada.
*   Tipos de passagem de parâmetros:
    *   **Por valor** o parâmetro é como uma variável local no registro de ativação da função chamada, iniciada com o valor do parâmetro no momento em que a função foi chamada.
    *   **Por resultado** o parâmetro é como uma variável local, porém não inicializada. Antes que a função retorne, um valor é copiado no endereço do parâmetro.
    *   **Por valor e resultado** mistura passagem por valor e por resultados, isto é, o parâmetro é como uma variável local no registro de ativação da função chamada. Esta variável é inicializada com o valor do parâmetro real, antes da chamada da função. Antes que a função termine, um novo valor pode ser copiado naquela variável.
    *   **Por referência** o endereço do parâmetro real é computado antes que a função seja chamada. Dentro do método, este endereço é usado como o endereço do parâmetro formal. Isto quer dizer que o parâmetro formal é um alias para o parâmetro real.
    *   **Por expansão de macros** o corpo da macro é computado no contexto em que a função é chamada. Cada ocorrência do parâmetro formal é avaliado antes de ser usado.
    *   **Por nome** cada parâmetro real é avaliado no contexto em que a função é chamada, cada vez que um parâmetro formal é usado.
    *   **Por necessidade** cada parâmetro real é avaliado no contexto em que a função é chamada, mas somente na primeira vez que o correspondente parâmetro formal é usado. O resultado do processamento de um parâmetro é armazenado, para otimizar subsequentes usos.

### Questões para discutir

*   Quais as formas de passagem de parâmetros em Python, Java e C?
*   Quais as vantagens e desvantagens da passagem de parâmetros por referências?

### Leitura

*   [Passagem de parâmetros](http://en.wikipedia.org/wiki/Call-by-value#Call_by_value)
*   [Passagem de parâmetros em C++](http://www.brpreiss.com/books/opus4/html/page591.html)
*   [Passagem de parâmetros em linguagens funcionais](http://www.dcs.ed.ac.uk/home/stg/NOTES/node71.html)
*   **Importante**: Capítulo 20 (Parameter Passing) do livro [Introduction to Programming Languages](http://en.wikibooks.org/wiki/Introduction_to_Programming_Languages).

### Exercícios

1.  Para ser entregue: [Lista 19](listas/lista19.pdf). Lembre-se: respostas escritas à mão.  

2.  (**P.O.F.**): Escreva a lista das 100 melhores coisas do mundo. Para ajudar no exercício, aqui vai a minha [lista](pofs/100coisas.html).

[Notas e exemplos](lesson22) usados em aula.

* * *

<a name="lesson20"></a>

## Introdução à linguagens Prolog

### Conceitos que devem ser entendidos.

*   Prolog é uma linguagens de programação lógica.
*   Programas escritos em prolog consistem em termos. Existem três tipos de termos: constantes (ou átomos), variáveis e termos compostos.
*   Programas Prolog são executados via _queries_. Uma query pergunta ao interpretador se ele é capaz de provar alguma coisa.
*   Regras são importante termos. Uma regra define uma relação lógica, por examplo, `A :- B, C` significa que, se o interpretador consegue provar que B e C são verdade, então A é verdade também.
*   Prolog é uma linguagem declarativa.
*   Os nomes de átomos não tem significado particular para o interpretador Prolog. Eles são úteis somente para que humanos possam entender os programas.
*   Assim como em ML, listas são importante estruturas de dados em Prolog.
*   Negação em Prolog é provada como a falha da query inversa.

### Questões para discutir

*   Prolog é uma boa linguagem para que tipos de aplicações?
*   Quais as vantagens e desvantagens entre linguagens procedurais e declarativas?

### Leitura

*   [A linguagem Prolog](http://en.wikipedia.org/wiki/Prolog)
*   O manual [SWI Prolog.](http://www.swi-prolog.org/pldoc/refman/) Vocês provavelmente usarão este ambiente de desenvolvimento.
*   [Programação Lógica em Prolog.](http://www.cs.uiowa.edu/~slonnegr/plf/Book/AppendixA.pdf)
*   Tem gente que de fato [ganha dinheiro](http://www.cs.kuleuven.ac.be/~dtai/projects/ALP/newsletter/archive_93_96/comment/belcore.html) com prolog.

### Exercícios

1.  Para ser entregue: [Lista 20](listas/lista20.pdf). Lembre-se: respostas escritas à mão.  
    Para exs 1 - 6, use [estas](srcs/l20/relations.txt) relações.  

2.  (**P.O.F.**): Vá soprar bolhas de sabão em uma praça pública.

[Notas e exemplos](lesson23) usados em aula.

* * *

<a name="lesson21"></a>

## Unificação

### Conceitos que devem ser entendidos.

*   A essência de Prolog é um algoritmo chamado [unificação](http://en.wikipedia.org/wiki/Unification).
*   **Importante:** nesta aula o aluno deve entender como representar uma query como uma árvore de busca.
*   Prolog escolhe sempre o unificador mais geral (MGU - most general unifier) para unificar dois termos. Dizemos que U é mais geral que S se S é uma instância de U, mas o contrário não ocorre. Por exemplo, `parent(fred, Y)` é mais geral que `parent(fred, mary)`.
*   Não se pode unificar termos de syntax diferente que incluem a mesma variável dos lados direito e esquerdo.
*   Árvores de prova (clause-trees ou proof-trees).

### Questões para discutir

*   O que é mais complicado a respeito de Prolog, quando comparado com outras linguagens?

### Leitura

*   [Tutorial sobre unificação em Prolog](http://www.amzi.com/AdventureInProlog/a10unif.htm) (Faça os exercícios).

### Exercícios

1.  Para ser entregue: [Lista 21](listas/lista21.pdf). Lembre-se: respostas escritas à mão.  

2.  (**P.O.F.**): Visite um lugar em que você nunca esteve antes.

[Notas e exemplos](lesson24) usados em aula.

* * *

<a name="lesson22"></a>

## Modelos de Custo

### Conceitos que devem ser entendidos.

*   Um modelo de custos descreve a complexidade das várias operações providas por uma linguagem de programação, em termos de tempo de execução e espaço.
    *   Alguns modelos de custos são parte da especificação da linguagem, por exemplo, armazenamento de arranjos em C. Outros dependem da implementação da linguagem, como armazenamento de arranjos em Ada.
*   Em Prolog e ML, a leitura do primeiro elemento de uma lista é O(1). Reverter a lista, ou concatenar duas listas é O(n).
*   Recursão de cauda é uma otimização aplicada em funções recursivas, em que a chamada recursiva é a última operação realizada no corpo da função.
*   Arranjos são armazenados em sequências de linhas, ou sequências de colunas, ou como vetores de vetores. O acesso sequencial de dados é sempre preferível.
*   Buscas em Prolog apresentam um caráter exponencial. Às vezes o reordenamento de cláusulas melhora o programa.

### Questões para discutir

*   Até que ponto um programador deve conhecer a especificação de uma linguagem de programação?
*   Existem operações que são inerentemente mais eficientes em linguagens que possuem arranjos?

### Leitura

*   [Chamada de cauda](http://en.wikipedia.org/wiki/Tail_call)
*   [Array programming](http://www.vector.org.uk/archive/v223/smill222.htm)
*   [Recursão de cauda](http://en.wikipedia.org/wiki/Tail_recursion)

### Exercícios

1.  Para ser entregue: [Lista 22](listas/lista22.pdf). Lembre-se: respostas escritas à mão.  

2.  (**P.O.F.**): Acorde cedo para assistir ao nascer do sol.

[Notas e exemplos](lesson25) usados em aula.

* * *

<a name="lesson23"></a>

## Predicados numéricos em Prolog

### Conceitos que devem ser entendidos.

*   Prolog possui vários testes de igualdade, por exemplo:
    *   **is** A cláusula `X is Y` avalia Y e unifica o resultado com X. A cláusula `3 is 1+2` é verdadeira, mas a cláusula `1+2 is 3` não é. A cláusula `X is 1+Y` produz uma exceção se a variável Y não estiver associada a nenhum valor.
    *   **=** A cláusula `X = Y` unifica X e Y. Unificação não causa a avaliação de valores numéricos. Assim, `3 = 1+2` não é verdade. Tampouco é verdade `1+2 = 3`.
    *   **=:=** A cláusula `X =:= Y` avalia tanto X quanto Y e sucede caso estes termos tenham o mesmo valor. Tanto `3 =:= 1+2` quanto `1+2 =:= 3` são verdade.
*   Prolog é uma linguagem dinamicamente tipada.
*   Prolog é uma boa linguagem para formular soluções para problemas NP-completos.
    *   Veja, por exemplo, a cláusula **findall**.

### Questões para discutir

*   A linguagem Prolog deveria ser parte da ementa de linguagens de programação?

### Leitura

*   [Existem empregos que requerem Prolog?](http://www.cs.kuleuven.ac.be/~dtai/projects/ALP/newsletter/archive_93_96/net/world/jobs.html)
*   [99 Problemas em Prolog](https://prof.ti.bfh.ch/hew1/informatik3/prolog/p-99/) .
*   [Aplicações escritas em prolog?](http://www.sciencedirect.com/science?_ob=ArticleURL&_udi=B6V0J-3VSCT52-4&_user=686413&_rdoc=1&_fmt=&_orig=search&_sort=d&view=c&_acct=C000037539&_version=1&_urlVersion=0&_userid=686413&md5=6e81b10c3169c63b3bd26e13e922a303) (Artigo: Applications of Logic Programming, por L. Sterling)

### Exercícios

1.  Para ser entregue: [Lista 23](listas/lista23.pdf). Lembre-se: respostas escritas à mão.  
    O autor do livro que adotamos disponibilizou alguns predicados para ajudar nestes exercícios:
    *   A versão simples do problema das [oito-rainhas](http://www.webber-labs.com/mpl/source%20code/Chapter%20Twenty-Two/eightqueens1.txt).
    *   E a versão [complicada](http://www.webber-labs.com/mpl/source%20code/Chapter%20Twenty-Two/eightqueens2.txt).
    *   O problema da [mochila](http://www.webber-labs.com/mpl/source%20code/Chapter%20Twenty-Two/knapsack.txt).
2.  (**P.O.F.**): Leia algum autor que já ganhou o Nobel de literatura.

[Notas e exemplos](lesson26) usados em aula.

* * *

<a name="lesson24"></a>

## Semântica Formal

### Conceitos que devem ser entendidos.

*   Enquanto a sintaxe define a aparência de uma linguagem de programação, a semântica define seu significado.
*   Um jeito de definir a semântica formal de uma linguagem L1 é construir um interpretador de L1 usando uma linguagem L2 de semântica bem definida.
    *   Mas como definir a semântica de L2?
*   Existem notações formais para definir semântica de linguagens de programação. Por exemplo: semântica operacional, semântica axiomática e semântica denotacional.
*   A semântica operacional especifica, passo a passo, o que ocorre com o programa enquanto ele é executado.

### Questões para discutir

*   Até que ponto um programador deve conhecer a semântica da linguagem de programação que ele usa?
*   Como ter certeza de que o programa faz o que se supõem que ele faça?

### Leitura

*   [Semântica operacional. Seções 8.5 (não é preciso ler sobre "completeness and consistency") e 8.6.](http://www.cs.uiowa.edu/~slonnegr/plf/Book/Chapter8.pdf)

### Exercícios

1.  Para ser entregue: [Lista 24](listas/lista24.pdf). Lembre-se: respostas escritas à mão.  
    O autor do livro que adotamos disponibilizou os interpretadores em sua página:
    *   [Interpretador](lesson27/val3.txt) com escopo dinâmico.
    *   [Interpretador](lesson27/val4.txt) com escopo estático.
2.  (**P.O.F.**): Dê uma sugestão para algum professor do departamento de como ele pode melhorar suas aulas.

[Notas e exemplos](lesson27) usados em aula.

* * *

<a name="lesson25"></a>

## História das Linguagens de Programação

### Conceitos que devem ser entendidos.

*   Algoritmos surgiram muito antes dos transístores.
*   Entre as primeiras linguagens de programação, cita-se Plankalkul, Fortran, Lisp e Algol.
*   Smalltalk popularizou orientação por objetos.
*   SML, Prolog e Java vieram depois.

### Questões para discutir

*   Pela lei de Moore, a velocidade do Hardware dobra a cada 18 meses. Existe alguma lei semelhante para a produtividade dos programadores?
*   Para onde estão indo as linguanges de programação?

### Leitura

*   [A História das Linguagens de Programação](readingMat/historyPL.pdf)
*   [A história das linguagens de programação.](http://en.wikipedia.org/wiki/History_of_programming_languages)
*   [Um linha do tempo](http://oreilly.com/news/languageposter_0504.html) publicada pela editora O'reilly.
*   [Mais linha de tempo](http://www.levenez.com/lang/lang.pdf) (Desta vez a original).
*   [How to shoot yourself in the foot](http://www.thealmightyguru.com/Humor/Docs/ShootYourselfInTheFoot.html) using any programming language.
*   [Beating the Average](http://www.paulgraham.com/avg.html), um artigo bem legal escrito por Paul Graham.

### Lista de Exercícios

1.  Para ser entregue: [Lista 25](listas/lista25.pdf). Lembre-se: respostas escritas à mão.  

2.  (**P.O.F.**): Escreva uma bela carta e deixe-a para algum sortudo, dentro de um livro da biblioteca.

* * *

## Aula de Revisão.

Dicas para se dar bem nesta próxima prova:

1.  Entenda as notas e exemplos dados em aula.
2.  Leia os [Slides](http://www.webber-labs.com/mpl/lectures/index.html) do livro.
3.  Faça as listas de exercícios.
4.  Não deixe nenhum P.O.F. sem fazer.

Abaixo estão alguns exemplos de provas dados em anos anteriores. A matéria agora é muito maior, afinal, conceitos da primeira metade do curso podem ser cobrados novamente. É claro que a ênfase será sobre a matéria da segunda metade. As dicas são as mesmas da primeira prova, porém desta vez [Build Me Up Buttercup](http://www.youtube.com/watch?v=iol0B-clFFM) pode não ser suficiente. Assim, se ficarem cansados enquanto treinam fazendo as provas, eu recomendo agora que vocês parem tudo para escutar [Dancing in the Moonlight](http://www.youtube.com/watch?v=hMc8naeeSS8). O solo de piano logo no início da música coloca qualquer um para cima.

*   [Prova 1](exams/final1.pdf).
*   [Prova 2](exams/final2.pdf).
*   [Prova 3](exams/final3.pdf).
*   [Prova 4](exams/final4.pdf).
*   [Prova 5](exams/final5.pdf).
*   [Prova 6](exams/final6.pdf).
*   [Prova 7](exams/final7.pdf).
*   [Prova 8](exams/final8.pdf).
*   [Prova 9](exams/final9.pdf).
*   [Prova 10](exams/final10.pdf).
*   [Prova 11](exams/final11.pdf).
*   [Prova 12](exams/final12.pdf).
*   [Prova 13](exams/final13.pdf).
*   [Prova 14](exams/final14.pdf).
*   [Prova 15](exams/final15.pdf).

* * *

## Segunda avaliação.

<span class="entryTitle">Duração:</span> <span class="entryInf">90 minutos.</span>  
<span class="entryTitle">Valor:</span> <span class="entryInf">40 Pontos.</span>  
<span class="entryTitle">Conteúdo:</span> <span class="entryInf">Todo o curso.</span>  
<span class="entryTitle">Modo:</span> <span class="entryInf">Sem consulta.</span>

* * *

## Exame Especial

Bom, se você ficou de exame especial, então sua situação não é muito melhor que [aquela](http://en.wikipedia.org/wiki/Carthage#Fall) dos cartagineses frente às legiões de Cipião Emiliano... Mas nem tudo está perdido! Abaixo estão alguns exemplos de provas dos últimos semestres que podem servir como combustível para o heroismo. Contudo, se lhe faltar o ânimo, eu sugiro escutar um [tango argentino](http://www.youtube.com/watch?v=qLUL9h5Xmc4) que seja dramático o suficiente.

*   [Prova 1](exams/special1.pdf).
*   [Prova 2](exams/special2.pdf).
*   [Prova 3](exams/special3.pdf).
*   [Prova 4](exams/special4.pdf).

<script type="text/javascript">var gaJsHost = (("https:" == document.location.protocol) ? "https://ssl." : "http://www."); document.write(unescape("%3Cscript src='" + gaJsHost + "google-analytics.com/ga.js' type='text/javascript'%3E%3C/script%3E"));</script> <script type="text/javascript">try{ var pageTracker = _gat._getTracker("UA-8958830-1"); pageTracker._trackPageview(); } catch(err) {}</script>
