### EXECUTION CONTEXT

> Consider the code:

        let x = 10

        function timesTen(a){
            return a * 10
        }

        let y = timesTen(x)

        console.log(y) // 100

> In this code:

    - First, assign 10 to the x variable.
    - Second, declare a function timesTen() that multiplies its argument with 10.
    - Third, call the timesTen() function by passing in x as a parameter and store the return value in the variable y.
    - Finally, output the variable y to the Console.

> Quando um mecanismo JavaScript (JavaScript Engine) executa um script, ele cria contextos de execução. Cada contexto de execução tem duas fases: a fase de **criação** e a fase de **execução**.

1. **The Creation Phase**

> Quando um script é executado pela primeira vez, a engine do JavaScript cria um Contexto de execução global. Durante esta fase de criação, ele executa as seguintes tarefas:

        1. Cria um objeto global, ou seja, janela no navegador da web ou global em Node.js.
        2. Cria uma associação de objeto que aponta para o objeto global acima.
        3. Configura um heap de memória para armazenar variáveis e referências de função.
        4. Armazena as declarações de função na pilha de memória e variáveis dentro do contexto de execução global com os valores iniciais como indefinidos.

> Em nosso exemplo, durante a fase de criação, a engine do JavaScript armazena as variáveis X e Y e a declaração da função timesTen () no contexto de execução global. Além disso, inicializa as variáveis X e Y para indefinidas.

![](Pictures\3.png)

> Após a fase de criação, o contexto de execução global passa para a fase de execução.

2. **The Execution Phase**

> Durante a fase de execução, o mecanismo JavaScript executa o código linha por linha. Nesta fase, ele atribui valores às variáveis e executa as chamadas de função.

![](Pictures\2.png)

> Para cada chamada de função, o mecanismo JavaScript cria um novo Contexto de Execução de Função. O Function Execution Context é semelhante ao Global Execution Context, mas em vez de criar o objeto global, ele cria o objeto arguments que contém uma referência a todos os parâmetros passados para a função:

![](Pictures\1.png)

> Em nosso exemplo, o contexto de execução da função cria o objeto de argumentos que faz referência a todos os parâmetros passados para a função, define esse valor para o objeto global e inicializa o parâmetro a como indefinido.

> Durante a fase de execução do contexto de execução da função, ele atribui 10 ao parâmetro a e retorna o resultado (100) ao Contexto de execução global:

![](Pictures\4.png)

> Para manter o controle de todos os contextos de execução, incluindo o Contexto de execução global e os contextos de execução de função, o mecanismo JavaScript usa uma estrutura de dados chamada pilha de chamadas.

### CALL STACK

> O mecanismo JavaScript usa uma pilha de chamadas para gerenciar contextos de execução: o contexto de execução global e os contextos de execução de função.

> A pilha de chamadas funciona com base no princípio LIFO, ou seja, last-in-first-out./

> Quando você executa um script, o mecanismo JavaScript cria um contexto de execução global e o coloca no topo da pilha de chamadas.

> Sempre que uma função é chamada, o mecanismo JavaScript cria um Contexto de Execução de Função para a função, coloca-o no topo da Pilha de Chamadas e começa a executar a função.

> Se uma função chamar outra função, o mecanismo JavaScript criará um novo Contexto de Execução de Função para a função que está sendo chamada e o colocará no topo da pilha de chamadas.

> Quando a função atual é concluída, o mecanismo JavaScript a tira da pilha de chamadas e retoma a execução de onde parou na última listagem de código.

> O script irá parar quando a pilha de chamadas estiver vazia.

> Acompanhe esse script:

        function add(a, b) {
            return a + b
        }

        function average(a, b) {
            return add(a, b) / 2
        }

        let x = average(10, 20)

![](Pictures\5.png)

### HOISTING

> A engine do Javascript trata todas as variáveis declaradas com var como se fossem declaradas no topo do escopo de uma função (se colocadas dentro de uma), ou no topo do escopo global (se declaradas fora de uma função), independentemente de onde a declaração real ocorrer. Isso essencialmente é “hoisting”.

> Então, variáveis podem realmente estar disponíveis antes de sua declaração. Veja:

        // Saída (Output): undefined
        console.log(shape)
        var shape = "square"
        // Saída (Output): "square"
        console.log(shape)

> Se você está vindo de linguagens baseadas em C, você estava esperando que um erro fosse lançado ao chamar o primeiro console.log, já que a variável shape não foi definida naquele momento. Entretanto, o interpretador do Javascript vai além, e iça (hoists) todas as declarações de variáveis pro top, e a suas inicializações permanecem no mesmo local.

> É isso que acontece por baixo dos panos:

        //a declaraçã da variável é içada (hoisted)
        var shape
        // Saída (Output): undefined
        console.log(shape)
        shape = "square"
        // Saída (Output): "square"
        console.log(shape)

> Abaixo está outro exemplo, dessa vez no escopo de uma função para deixar as coisas mais claras:

        function getShape(condition) {
        // shape existe aqui com o valor "undefined"
        // Saída (Output): undefined
        console.log(shape)
        if (condition) {
                var shape = "square"
                // outro código qualquer
                return shape
            } else {
                // shape existe aqui com o valor "undefined"
                return false
            }
        }

> Perceba que no exemplo acima a declaração de shape é içada (hoisted) para o topo da função getShape. Isso acontece pois os blocos if/else não criam escopos locais como vemos em outras linguagens. O escopo local é essencialmente o escopo de uma função em JavaScript. Portanto, “shape” é acessível em todos os lugares fora do bloco if e dentro da função com um valor “undefined”.

> Troque var por let para declarar uma variável para que seu escopo fique apenas naquele bloco. Coloque a declaração do seu let no topo do bloco para que ele esteja disponível para o bloco inteiro. Exemplo:

        function getShape(condition) {
        // shape não existe aqui
        // console.log(shape) => ReferenceError: shape is not defined
        if (condition) {
                let shape = "square"
                return shape
            } else {
                // shape também não existe
                return false
            }
        }

> Acessar uma variável com let ou const antes de ser declarada lançará um ReferenceError. Esse período entre a entrada do escopo e a declaração de onde eles não podem ser acessados é chamado de Zona Temporal Morta (Temporal Dead Zone).

> Sempre use const, pois gera menos bugs. Atualmente eu raramente encontro uma situação onde precise usar o var.

> Como regra geral, use let somente para contadores de loop ou se você realmente precisará alterar o valor da variável depois. Pra qualquer outro caso, vá de const. Abandone loops para usar filter (), map () & reduce ().

> Primeiro, é importante notar que apenas as variáveis ​​declaradas usando a palavra-chave var são içadas. Variáveis ​​declaradas usando as palavras-chave let e const mais recentes não são içadas. Em vez disso, as variáveis ​​declaradas usando-as só podem ser usadas após o ponto de declaração. (Eles também têm escopo de bloco, ao contrário de var, que tem escopo de função).

##### Vantagens do Hoisting

> O que aconteceu foi que o JavaScript implementou o levantamento de declarações de função para que os programadores não fossem forçados a colocar as funções mais internas na parte superior do bloco de script e as funções mais externas (nível superior) na parte inferior . Essa ordem, que é forçada em linguagens ML (como LISP), é dolorosa porque os programadores preferem ler o código de cima para baixo, em vez de de baixo para cima. Linguagens como C / C ++ contornam esse problema usando arquivos de cabeçalho e declarações autônomas, que o JavaScript não tem. Além disso, era necessário içar para implementar a recursão mútua.

> A elevação de funções foi implementada fazendo com que o interpretador JavaScript realizasse duas passagens sobre o código. Na primeira passagem, as declarações foram extraídas e na segunda passagem o código foi realmente executado. Isso resultou no efeito colateral de também elevar as declarações de variáveis ​​ao topo de seu escopo.

> Muitos programadores que vêm para o JavaScript de linguagens como C / C ++ e Java, onde as declarações de variáveis ​​não são utilizadas, não gostaram desse comportamento. Assim, let e const foram introduzidos na linguagem como parte do ES6 para que se comportasse mais como essas outras linguagens.

### LEXICAL & GLOBAL SCOPE

> Um escopo léxico em JavaScript significa que uma variável definida fora de uma função pode ser acessível dentro de outra função definida após a declaração da variável. Mas o oposto não é verdade; as variáveis definidas dentro de uma função não estarão acessíveis fora dessa função.

> Este conceito é muito usado em closures em JavaScript.

        var x = 2;
        var add = function() {
            var y = 1
            return x + y
        }
        // Quando vc chamar add() vai retornar 3

### DOM

-----

> Existem dois jeitos de capturar eventos, (gatilhos de eventos, event triggers) de **cima para baixo** (Event Capturing, captura de evento) e o outro é de **baixo para cima** (Event bubbling, bolha de evento)

-----

1. Event Delegation

> É uma técnica para ouvir eventos em que você delega um elemento pai como ouvinte de todos os eventos que acontecem dentro dele.

> Por exemplo, se você quiser detectar a qualquer momento qualquer campo alterado em valor dentro de um formulário específico, você pode fazer isso:

        var form = document.querySelector('#hogwarts-application')

        // Listen for changes to fields inside the form
        form.addEventListener('input', function (event) {

            // Log the field that was changed
            console.log(event.target)

        }, false)

2. Event Bubbling

> Se você já viu as bolhas em um copo de refrigerante, você vai entender como funciona o event bubbling.

> O início do evento é o elemento que o acionou (dizendo, alterando o campo #email em nosso exemplo acima). Em seguida, ele borbulha em cada um de seus elementos pais até atingir o elemento html.

> Quando um evento acontece em um elemento, ele primeiro executa os escutadores (handlers) nele, depois em seu pai e, em seguida, em todos os outros ancestrais.

> Digamos que temos 3 elementos aninhados FORM> DIV> P com um escutador (handler) em cada um deles:

        <style>
        body * {
            margin: 10px;
            border: 1px solid blue;
        }
        </style>

        <form onclick="alert('form')">FORM
        <div onclick="alert('div')">DIV
            <p onclick="alert('p')">P</p>
        </div>
        </form>

> Um clique no <p> interno primeiro executa onclick:

1. Nesse <p>.
Em seguida, no <div> externo.
Em seguida, no <form> externo.
E assim por diante até o DOM.

![](Pictures\6.png)

> Pra resumir: So if we click on <p>, then we’ll see 3 alerts: p → div → form!

> O processo é chamado de “borbulhamento”, porque os eventos “borbulham” do elemento interno até os pais como uma bolha na água.

> Quase todos os eventos borbulham.

> A palavra-chave nesta frase é “quase”.

> Por exemplo, um evento de foco não borbulha. Existem outros exemplos também, vamos conhecê-los. Mas ainda é uma exceção, ao invés de uma regra, a maioria dos eventos borbulha.

> Para parar o bubbling: event.stopPropagation() e também o event.stopImmediatePropagation()

3. Event Capturing

> Seta addEventListener() pra true para capturar eventos.

        document.addEventListener('focus', function (event) {
            console.log(event.target)
        }, true)

> Para capturar um evento na fase de captura, precisamos definir a opção de captura do manipulador como verdadeira:
        
        elem.addEventListener(..., {capture: true})
        // or, just "true" is an alias to {capture: true}
        elem.addEventListener(..., true)

> Existem dois valores possíveis para a opção de captura:

> **false** (padrão), o manipulador está definido na fase de bolhas (then the handler is set on the bubbling phase).
> **true**, o manipulador está definido na fase de captura (then the handler is set on the capturing phase).

### IIFE

        (function(){})()

> O IIFE significa “Immediately-invoked function expression”, mas podemos chamá-lo de função imediata. Como o próprio nome diz, ela executa a função imediatamente depois de criada.

> Mas por que usar? Encapsulamento! Tenha em mente que variáveis em Javascript têm como escopo a função pela qual elas foram definidas (podem ser acessadas somente dentro da função, jamais fora). Ao criar uma função anônima com execução imediata, podemos criar um escopo temporário para nossas funções e variáveis. Com isso, evitamos poluição no nosso escopo global e possíveis conflitos de variáveis ou funções com o mesmo nome.

> Exemplo:

        var adder = (function() {
        var myPhrase = ""
        return function(x) { 
        return myPhrase = 
        !!myPhrase ? myPhrase.concat(" ", x) : myPhrase.concat(x)
        }
        })()
        
        adder("Olá") // "Olá"
        adder("Mundo!") // "Olá Mundo!"
        
        myPhrase // myPhrase is not defined

> Neste exemplo, criamos uma função anônima imediata que retorna uma outra função que concatena uma string na variável chamada myPhrase. Note que a variável criada myPhrase está no escopo da IIFE e não na função retornada. Portanto, o myPhrase não é definido toda vez que invocamos a função adder. Mas o mais importante disso tudo é que o myPhrase está **limitado a escopo da função anônima imediata**, não permitindo o seu acesso direto de maneira alguma.

1. Sintaxe:

> Sabemos que podemos definir uma função assim: 

        function doSomething() { /* codigo */ }

> Até aqui está lindo, mas e se eu colocar o conjunto de parênteses () para invocar a função enquanto definimos. Aí temos a função imediata, certo?

        function doSomething() { /* codigo */ }() // SyntaxError: Unexpected token )
        
        // Agora com função anônima
        function() { /* codigo */ }() // SyntaxError: Unexpected token (

> **Errado!** Como você pode ver, se tentamos invocar uma função enquanto definimos, temos um erro.

> De acordo com a norma do ExpressionStatement (estado de expressão), não podemos começar com a keyword function, pois quando usada em escopo global ou dentro de outra função, temos um estado de declaração de uma função (Function Declaration). Portanto, se tentarmos definir uma IIFE da maneira mostrada acima (iniciando com a keyword function), estaremos lidando com declaração e o que precisamos é de uma **expressão** ***(Function Expression)***.

> Essa é a forma correta:

        (function (){

        })() // undefined

        //OU

        (function doSomething() { /* codigo */ })() // undefined

> Utilizando parâmetros:

        //Função imediata com parâmetro
        (function doSomething(x) { console.log(x) })(1) // 1
        
        // Agora com função anônima
        (function(x) { console.log(x) })(1) // 1

> Obs: Tem outras maneiras como o método de Douglas Crockford:

        (function() { /* codigo */ }()) // Método de Douglas Crockford

> Em alguns casos, o parser já espera uma Function Expression, então não precisamos colocar o grupo de operadores.

        var doSomething = function() { return "done" }() // Sem o grupo de operadores

> E olha essa sintaxe:! Economizamos um byte transformando para Function Expression sem usar o grupo de operadores, mas operadores unários:

        !function(){ /* codigo */ }()
        ~function(){ /* codigo */ }()
        -function(){ /* codigo */ }()
        +function(){ /* codigo */ }()

### CALL(), APPLY() & BIND()

1. CALL()

        this.name = 'Ana' // Ana

        function sayName(age) {
            this.age = age
            console.log(this.name, this.age)         
        }

        const dev = {
            name: 'Isabela'
        }

        console.log(sayName.call(this, 18)) // Ana 18
        console.log(sayName.call(dev, 19)) // Isabela 19

        console.log(this.age) // 18
        console.log(dev.age) // 19

> O **primeiro parâmetro que recebe é o valor de this que que será atribuído à função**. Os **demais parâmetros são os parâmetros da função** que invoca o método Call.

> Outro exemplo:

        function sayAnyCharacter() {  
        console.log(this.name + ": " + this.character)
        }
        
        var actor1 = {  
        name:"Clint Eastwood",
        character: "The Good"
        }

        var actor2 = {  
        name:"Lee Van Cleef",
        character: "The Bad"
        }

        // Clint Eastwood: The Good
        sayAnyCharacter.call(actor1)

        // Lee Van Cleef: The Bad
        sayAnyCharacter.call(actor2)

> O exemplo acima mostra uma capacidade interessante do JavaScript, ele é capaz de separar totalmente os dados do comportamento e ainda assim facilmente torná-los simultaneamente úteis pelo poder de combiná-los. Imagine que você tivesse um arquivo de dados JSON. A demonstração acima mostra que seria facilmente possível, através do método call, transformar dados em objetos ao conferir aos primeiros um comportamento desejado. Essa capacidade é uma das coisas que torna JavaScript tão interessante.

-----

> **Estrutura de dados são dados sem comportamento.**

> **Função é comportamento sem dados.**

> **Objetos são dados com comportamento(métodos).**

-----

> Exemplo de uso: Reaproveitar métodos nativos.

> Os inputs retornados pelo seguinte trecho de código var inputs = document.getElementsByTagName("input") possuem a mesma estrutura de arrays, mas não possuem o método filter no qual estamos realmente interessados.

> Sabemos que se inputs fosse realmente array tudo estaria resolvido. Mas existe uma segunda alternativa. Podemos aproveitar a estrutura do filter dos arrays, modificando apenas o objeto(this) que sofrerá a ação do método. E foi exatamente o que fizemos ao criar a função filterDomElements:

        function filterDomElements(elements, filterCallback) {  
                return Array.prototype.filter.call(elements, filterCallback) 
        }

> Ou ainda:

        function filterDomElements(elements, filterCallback) {  
                return [].filter.call(elements, filterCallback)
        }


2. APPLY()

> Apply é o segundo método de funções capaz de alterar o valor this. Ele funciona exatamente como o método Call, porém seu segundo parâmetro recebe um Array ou Array-like dos parâmetros da função. Exemplo:

        function sayThis(n1,n2){  
        console.log(this * n1 * n2)
        }

        // 2 é this, n1 e n2 são 3
        sayThis.apply(2, [3, 3]) // 18 

        // 1 é this, n1 e n2 são 3
        sayThis.apply(1, [3, 3]) // 9

> O método apply é especialmente útil para se trabalhar com o Arguments Object (poder das Javascript Functions).

> Douglas Crockford em uma de suas palestras propôs o seguinte exercício: "Escreva uma função que recebe uma outra função como parâmetro e a retorna de forma que ela só possa ser invocada uma única vez". Uma resolução:

        function multiply() {
        var length = arguments.length
        var total = 1

        if (length > 0) {
                for(var i = 0; i < length; i += 1) {
                total = total * arguments[i]
                }
                return console.log(total)
        }
        console.log(0)
        }

        function once(func) {
                return function() {
                        var f = func
                        func = null
                        return f.apply(this, arguments)
                }
        }

        var multiply_once = once(multiply)
        multiply_once(3, 4)  // 12
        multiply_once(3, 4)  //Error: Cannot read property 'apply' of null

3. BIND()

> Ao contrário dos outros, ele não executa a função, mas retorna uma outra.

> O primeiro argumento recebe o valor do this a ser usado na função a ser retornada. Os demais argumentos são os parâmetros que terão valores permanentemente atribuídos dentro da função a ser retornada.

> Assim o método Bind acaba cumprindo dois papéis:

- Mudar o valor do this;
- Alterar o número de parâmetros de uma função, caso o programador deseje. Essa capacidade de alterar a quantidade de argumentos de uma função é chamada de Currying.

        function sayThis(n1,n2){  
        console.log(this * n1 * n2)
        }

        var thisIs2 = sayThis.bind(2)
        var thisIs2N1Is3 = sayThis.bind(2,3)
        var thisIs2N1Is3N2Is3 = sayThis.bind(2,3,3)
        thisIs2(3,3) //18 - muda this  
        thisIs2N1Is3(3) //18 - muda this e atribui n1  
        thisIs2N1Is3N2Is3() //18 - muda this e atribui n1 e n2

> Um bom exemplo de quando o método Bind se torna especialmente útil é quando você precisa manipular valores dentro de callbacks:

        var handler = {
        id:"handlerId",
        click: function(event){
                alert(this.id)
        }
        }

        var button = document.getElementById('buttonId')
        var button2 = document.getElementById('buttonId2')
        button.addEventListener('click', handler.click)
        button2.addEventListener('click', handler.click.bind(handler))

### MVC

> A **MODEL** seria o coração da sua aplicação. É nas models que vai ter as *regras de negócio*, vão ter as suas *entidades*, vai ter a sua *camada de acesso a dados*, *validações no back-end*.

> A **VIEW** é responsável por renderizar a resposta

> Uma boa analogia é a de um prédio. Vc está em frente a um prédio de uma grande empresa com um envelope (requisição) para entregar para uma pessoa que você nunca viu, vc só tem o nome dela.

> A primeira coisa que vc encontra entrando no prédio é a recepcionista. Ela no caso será o próprio Framework MVC, ou em alguns contextos ela também será chamada de Front-Controler. É ele quem recebe a requisição primeiramente na sua aplicação.

> Ele, o framework, pode ler metadados para saber e coordenar pra onde enviar essa requisição na sua aplicação.

> O responsável por enviar sua requisição é o **Controller**. Ou seja, vc tem o framework que tem inteligência, que sabe distribuir esses requests dentro da aplicação, e quem leva a requisição e traz a resposta é o CONTROLLER.

-----

![](Pictures\7.png)

1. Você tem o **browser** gerando uma requisição a partir de uma URL;

2. Essa requisição vai bater no seu **web server** (que pode ser o TOMCAT, Engine Next, Apache...);

3. Chegando no web server, a partir da URL, ele vai saber pra qual aplicação ele vai direcionar. Ele direciona para o alvo da requisição;

4. E dentro da aplicação, o seu framework vai saber pra onde ele vai direcionar. Dentro da aplicação, a primeira camada que vai receber a requisição, no caso do modelo MVC, é o **controller**;

-----

![](Pictures\8.png)
