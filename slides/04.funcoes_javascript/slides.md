class: center, middle
# Funções JavaScript
### ©2017 Dalton Serey, Programação 3, UFCG
.center[![Right-aligned image](https://i.stack.imgur.com/Mmww2.png)]
---
# funções JavaScript

- funções JS são as “caixas de código” de JavaScript

   - são fortemente inspiradas em funções matemáticas
   - podem ou não ser puras: depende do programador
   - por isso, assumem diversos papéis na linguagem

--
count: false
- outras linguagens precisam de múltiplos tipos de “caixas de código”

   - subrotinas
   - procedures
   - classes
   - construtores
   - métodos
   - packages
   - módulos

--
count: false
- JavaScript tem apenas “funções” (pra todos os fins)

   - permite programação estruturada, OO, por eventos e _funcional_

---
## sintaxe: comando de declaração 👍

```javascript
function nome(<params>) {
   <corpo>
}
```

--
count: false
## sintaxe: expressão 👍👍👍 👏👏👏

```javascript
... function (<params>) { ... };

... function nome(<params>) { ... };

(<params>) => <expressão do valor a retornar>;

(<params>) => { <statements> };

parametro => <expressão>;

parametro => { <statements> };
```

---
# funções são “cidadãs de primeira classe”

```javascript
const f = function (...) { ... };
const g = (x, y) => ...;

const funcoes = [f, g, ...];
const o = { x: f, ... };
const i = outra_funcao(f, g, ...);
```

```javascript
function outra_funcao(k) {
    const x = k(...);
    ...
    return function (...) {...}
}
```
--
count: false

observe que (expressões de) funções podem ser anônimas (_lambdas_)

```javascript
(function () {
})
```
- convenientes, simples, atrativas, mas...  podem dificultar a depuração ☹
- além disso, nomes permitem recursividade

---
# funções definem escopo

- JavaScript tem escopo baseado em funções

   - cuidado: diferente de linguagens com escopo baseado em blocos

   - felizmente, ES6 adiciona escopo por blocos 👍

- escopo de funções JS é léxico (ou estático)

   - logo, definido em tempo de codificação

   - a função acessa “suas” variáveis e as dos escopos acima

---
# escopos aninhados e _closures_

- funções internas têm acesso ao escopo da função externa

- _closure_: função interna que usa os valores do escopo externo

---
## pequeno exemplo de _closure_

```javascript
function multiplicador(a) {
    return x => x * a;
}
```

--
count: false
```javascript
const duplica = multiplicador(2);
const triplica = multiplicador(3);
console.log(duplica(25)); // 50
console.log(triplica(25)); // 75
```

- _closures_ são uma das melhores ideias já vistas em programação
- mais adiante voltaremos a tratar do assunto

---
## (um parênteses: veja como a notação arrow ajuda)

```javascript
function multiplicador(a) {
    return x => x * a;
}
```
Mesmo código com a notação original de funções.
```javascript
function multiplicador(a) {
    return function (x) {
        return x * a;
    };
}

const duplica = multiplicador(2);
const triplica = multiplicador(3);
console.log(duplica(25)); // 50
console.log(triplica(25)); // 75
```

---
# const, let e var

- adote-os nessa ordem... comece por `const`

   - implica que você está definindo uma constante

- se você precisar _mutar_ o valor, opter por `let`

   - `let` define uma variável em escopo de bloco

- em raríssimas ocasiões, precisaremos de `var`

   - `var` define uma variável no escopo da função
   - `var` implica também em _hoisting_
   - antes de ES, var é a única opção

- e *nunca* defina funções sem nenhuma delas (são globais)

---
## exemplo: const

```javascript
function zero(x) {
    if (x < Number.EPSILON) {
        ...
        const r = "eh zero";
    } else {
        ...
        const r = "nao zero";
    }
    return r; // ReferenceError: r is not defined
}
```

Tente com `let` e com `var`.

---
# use const em for..of

```javascript
const nums = [1, 2, 3];
for (const e of nums) {
    console.log(`e = ${e}`);
}
```
--
count: false
```javascript
const nums = [1, 2, 3];
for (const e of nums) {
    e = e + 1; // mas a variável não pode ser alterada
    console.log(`e = ${e}`);
}
```

--
count: false
```javascript
const nums = [1, 2, 3];
for (let e of nums) {
    e = e + 1; // agora ok
    console.log(`e = ${e}`);
}
```

---
# const não torna objetos imutáveis

```javascript
function duplica(o, i) {
    o[i] = o[i] * 2;
}

const nums = [1, 1, 1];
duplica(nums, 0);
console.log(nums); // [ 2, 1, 1 ]
```

---
# IIFE 

### _immediately invoked function expression_

```javascript
(function () {})()

(function () {}())

(function () {
    ...
})()
```

- muito usados para empacotar um trecho de código
- delimita o escopo do código 
- permite não poluir o global (antes de ES6)

---
# lições aprendidas

---
# leituras indicadas

- [Funções parciais](https://en.wikipedia.org/wiki/Partial_function)
- Capítulo de Funções de The Good Parts
- [Arrow Functions ES6](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/Arrow_functions) 
