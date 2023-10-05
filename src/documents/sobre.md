# Sobre o Projeto
## HTML
A estrutura inicial do projeto é bem simples em sua projeção. Basicamente, dentro do ***``body``***, dividiremos o código em duas partes, sendo a primeira delas o contúdo principal que conterá as informações de cada personagem e a segunda parte é uma lista de botões para fazermos nossa navegação.

Falaremos então da primeira parte onde esta o conteúdo principal. Por ser o principal do código ele será encapsulado pela tag ***``main``*** e dentro dessa tag, cada personagem será dividido por ***``section's``***. Mais a fundo, cada ***``section``*** será dividida em duas partes, uma que terá apenas a imagem do personagem e a outra que será uma ***``div``*** onde estará toda descrição do mesmo:

```html
<main class="personagens">
    <section class="personagem selecionado">
        <img class="imagem" src="" alt="">
        <div class="conteudo">
            <i class="logo"> </i>
            <h2 class="nome-personagem"> </h2>
            <p class="descricao"> </p>
        </div>
    </section>
...
```
A segunda parte é uma lista não ordenada de botões, onde cada botão será uma imagem, cada elemento aqui seguirá a mesma ordem dos personagens a cima:

```html
<ul class="botoes selecionado">
    <li><button class="botao"><img src="" alt=""></button></li>
...
```

> Repare que, tanto uma das sections como um dos botões possuem duas classes, a classe "selecionado" serve para especificar qual personagem estará visivel na tela, para a section e no caso do botão, qual botão está destacado na tela.

---
## CSS
Ao terminar de montar o esqueleto do código, precisamoes estiliza-lo. Como queremos que apareça somente um elemento na tela, sará aplicado um ***``display: none``*** em todos os personagens, mas no que tiver a ***``class``*** "selecionado", este terá o ***``display: block``***, sendo o único elemento visivel do projeto.

```css
.personagem {
    display: none;
}

.selecionado {
    display: block;
}
```

Em questão de estilização, vai do gosto de cada um. Só mencionarei o sombreado feito para dar destaque ao texto, sendo esse um ***``content``***.

```css
main::after {
    content: "";
    position: fixed;
    top: 0;
    left: 0;
    width: 80vw;
    min-height: 100vh;
    background: linear-gradient(-233deg, #000000 40%, rgba(0,0,0,0) 65%) no-repeat;
}
```

Como ele vem após a tag ***``main``*** ele acaba ficando em cima do texto, então usaremos o ***``z-index``*** para trazer esse texto a frente.

```css
.conteudo {
    z-index: 1;
}
```

---
## JS
A primeira coisa a se fazer é pensar em como o código vai funcionar, uma dica é escrever em forma de comentário o que precisa ser feito:

>O que precisamos fazer? - quando clicar no botão do personagem na lista, temos que marcar o botão como selecionado e mostrar o personagem correspondente.
>
>OBJETIVO 1 - quando clicar no botão do personagem na lista, marcar o botao como selecionado. 
>- passo 1 - pegar os botões no JS pra poder verificar quando o usuário clicar em cima de um deles;
>- passo 2 - adicionar a classe "selecionado" no botão que o usuário clicou;
>- passo 3 - verificar se já existe um botão selecionado, se sim, devemos remover a seleção dele;
>
>OBJETIVO 2 - quando clicar no botão do personagem mostrar as informações do personagem.
>- passo 1 - pegar os personagens no JS pra poder mostrar ou esconder ele;
>- passo 2 - adicionar a classe "selecionado" no personagem que o usuário selecionou;
>- passo 3 - verificar se já existe um personagem selecionado, se sim, devemos remover a seleção dele;

Então nós já temos o nosso objetivo estabelecido, agora precisamos seguir os passos, descritos a cima, para concluir nosso objetivo principal.

A primeira coisa a se fazer é pegar os botoes do ***``html``*** e inseri-los no ***``js``***. Analisando-os é perceptivel que todos possuem a mesma classe (botao) e isso nos possibilita a pegar todos de uma vez sem ter que repetir código desnecessariamente. No ***``js``*** os chamaremos de "botoes" mesmo.

```js
const botoes = document.querySelectorAll (".botao");
```

O segundo passo é adicionar a classe "selecionado" no botão que o usuário clicou. Para isso precisamos adicionar a ele um ouvinte que escute um evento especifico e assim insira essa classe nele. 
> Um ***``addEventListener``*** (ouvinte) é um intermediário que, ao disparar um evento especifico ele executa uma ***``arrow function``***. Por exemplo o "click" do mouse.

```js
botoes.addEventListener("click", () => {

});
```

Nesse caso não funcionará, pois "botoes" é um ***``array``*** e o ***``addEventListener``*** só pode ser executado em um indivíduo único. Para que ele funcione, eu preciso percorrer essa lista e para cada item da dela adicionar o ***``addEventListener``*** e isso é possivel através do ***``forEach``***.

Então adicionaremos esse método a nossa lista, dentro de seus parênteses passaremos um argumento (chamaremos de "botao") abre e fecha chaves. Pronto, agora dentro desse laço de repetição inseriremos o ***``addEventListener``***.

```js
botoes.forEach(botao => {
    botao.addEventListener("click", () => {

    });
});
```

Agora é possivel adicionar uma nova classe a cada elemento dessa lista. O ***``js``*** oferece um método chamado ***``classList``*** esse método permite saber qual classe aquele elemento tem, junto dele podemos usar uma extensão ***``.add``*** onde adicionaremos uma nova classe ao elemento.

```js
botoes.forEach(botao => {
    botao.addEventListener("click", () => {
        botao.classList.add("selecionado");
    });
});
```

O passo três é:  verificar se já existe um botão selecionado, se sim, devemos remover a seleção dele. Então, nós precisamos pegar esse elemento e traze-lo para o ***``js``*** e podemos fazer isso através do ***``querySelector``***. Assim que tivermos em posse dele, usaremos novamente o ***``classList``*** e sua extensão será o ***``.remove``***.

```js
botoes.forEach(botao => {
    botao.addEventListener("click", () => {
        const botaoSelecionado = document.querySelector(".botao.selecionado");
        botaoSelecionado.classList.remove("selecionado");

        botao.classList.add("selecionado");
    });
});
```
> Essa função precisa vir antes da adição da classe pois se viesse depois, ao adicionar a classe no botão, ela seria retirada rapidamente e ai o botão ficaria como se não estivesse selecionado.

O objetivo um está finalizado, agora faremos o segundo objetivo e ele é parecido com o primeiro. Então quando clicarmos no botão do personagem as informações do personagem tem que aparecer e pra isso precisamos ter acesso ao personagem, da mesma forma como foi feito com os botões, sará feito com os peronagens.

```js
const personagens = document.querySelectorAll (".personagem");
```

Agora precisamos adicionar a classe "selecionado" nele, só que nesse caso, nós não sabemos qual o personagem foi selecionado, ao contrário do botão que sabemos quem qual deles estamos clicando.

Para se ter noção disso, podemos usar o indice do ***``array``*** em que eles estaram inseridos. Como bem sabemos, "botoes" é um ***``array``*** e cada item presente dentro dele é enumerado, esse número é o seu indice. O que fazeremos é atrelar cada personagem a um número.

Então, passaremos um novo parametro para o ***``forEach``***, esse parametro é o "indice", como temos um novo parametro, precisamos coloca-los dentro de parênteses.

```js
botoes.forEach((botao, indice) => {
...
```

Agora pegaremos "personagens" e atrelaremos a ele o parametro "indice", usando "[]" e pronto. Usando o método ***``classList``*** e a extensão ***``.add``*** ele aparecerá na tela.

```js
const personagens = document.querySelectorAll (".personagem");

botoes.forEach(botao => {
    botao.addEventListener("click", () => {
    ...
        personagens[indice].classList.add("selecionado");
    });
});
```

> Importante dizer que, para que isso de certo, a ordem das div's que contem os personagens tem que ser a mesma dos botões, caso contrário, quando clicado um botão o personagem que aparecerá não será correspondente

Feito tudo isso, precisamos verificar se já existe um personagem selecionado, se sim, devemos remover a seleção dele e seguiremos a mesma logica do "botaoSelecionado".

```js
const personagens = document.querySelectorAll (".personagem");

botoes.forEach(botao => {
    botao.addEventListener("click", () => {
    ...
        const personagemSelecionado = document.querySelector(".personagem.selecionado");
        personagemSelecionado.classList.remove("selecionado");
        personagens[indice].classList.add("selecionado");
    });
});
```

Muito bom, já esta tudo funcionando, o que precisa ser feito agora é uma refatoração, onde pegaremos quase todos os processos e os retiraremos da função principal para ficar melhor legivel.

```js
const botoes = document.querySelectorAll (".botao");
const personagens = document.querySelectorAll (".personagem");

botoes.forEach((botao, indice) => {
    botao.addEventListener("click", () => {

        trocaDePersonagem();
        desselecionarBotao();

        botao.classList.add("selecionado");
        personagens[indice].classList.add("selecionado");
    });
});

function trocaDePersonagem() {
    const personagemSelecionado = document.querySelector(".personagem.selecionado");
    personagemSelecionado.classList.remove("selecionado");
}

function desselecionarBotao() {
    const botaoSelecionado = document.querySelector(".botao.selecionado");
    botaoSelecionado.classList.remove("selecionado");
}
```