class: center, middle
# Funções e Objetos
### ©2017 Dalton Serey, Programação 3, UFCG
.center[![Right-aligned image](https://i.stack.imgur.com/Mmww2.png)]
---
# métodos são funções em um objeto

- reforçando (mais uma vez!): em JS funções são objetos

- _método_ é só um nome dado a funções armazenadas como properties

   - não há qualquer tratamento diferenciado
   - do ponto de vista do engine, são apenas funções

--
count: false
- na prática, contudo, pode ser conveniente diferenciar entre

   - funções independentes
   - e funções que devem usadas como métodos
   - e escrever seu código considerando isso...

---
# exemplo
```javascript
function dobro(x) {
    return 2 * x;
}

dobro(10); // 20
dobro(34); // 68
```

--
count: false
se a função é property de um objeto, dizemos que é um método
```javascript
const a = {x: 10, m: dobro};
 
a.x; // 10
a.m; // [Function: dobro]
```

--
count: false
embora, desta forma, não seja exatamente o que queremos…
```javascript
a.m();      // NaN
a.m(a.x);   // 20
```

---
# o keyword this

.footnote[.red[<sup>1</sup>] Importante: `this` **não é** um
argumento escondido das funções.]

- `this` é um keyword.red[<sup>1</sup>] que tem um _valor_ em JavaScript

- refere-se sempre a _algum_ objeto

- o problema é _qual objeto_?

   - isso depende da _forma de invocação_ da função
   - mais adiante veremos isso em detalhes




---
# this

```javascript
function dobro(x) {
    return 2 * this.x;
}

const a = {x: 10, d: dobro};
 
a.x; // 10
a.d; // [Function: dobro]

a.d();      // 20
dobro(a.x); // NaN (?!!!)
```

---
# quatro padrões de invocação

há quatro padrões de invocação de funções JavaScript

1. invocação de função simples

2. invocação de função como método

3. invocação de função como construtor

4. invocação via apply / call

e há ainda `bind`...

---
# função simples

`this` é erroneamente ligado ao objeto global

```
function teste(x) {
    console.log(this.process.title);
    return 'ok';
}

console.log(teste());
```

---
# função simples: modo estrito

```
'use strict'

function teste(x) {
    console.log(this.process.title);
    return 'ok';
}

console.log(teste());
```

---
# padrão: invocação de método

`this` é ligado ao objeto _na expressão_ de invocação da função

```javascript
const a = {
  x: 0,
  conta: function () {
    this.x += 1;
  }
}

a.x; // 0
a.conta(); // this é a
a.conta(); // idem
a.conta(); // idem
a.x; // 3
```

---
# padrão: construtor

- this é um novo objeto criado antes do início da função
- é um objeto novo, criado como cópia de `<função>.prototype` (!?)

   - não confundir com `Function.prototype`
   - também não confundir com `<função>.__proto__`

- o objeto criado terá a property `__proto__` apontando para

```javascript
function Pessoa(nome) {
    this.nome = nome;
    this.fale = function () {
        console.log(`oi, eu sou ${this.nome}`)
    }
}
```

---
# funções retornadas por métodos

razão de muita confusão: pra this não vale `lexycal scoping` 👎 

```javascript
const o = {
  n: 10,
  multiplicador: function multiplicador(fator) {
    return function () {
      return fator * this.n;
    };
  }
}

const double = o.multiplicador(2);
const triple = o.multiplicador(3);
console.log(double()); // ???
console.log(triple()); // ???
```


---
# idiom `that`

solução muito usada... usar um closure do valor desejado

```javascript
const o = {
  n: 10,
  multiplicador: function multiplicador(fator) {
    const that = this; // dado a ser “enclausurado”
    return function () {
      return fator * that.n; // uso do dado de this do escopo acima
    };
  }
}

const double = o.multiplicador(2);
const triple = o.multiplicador(3);
console.log(double()); // 20
console.log(triple()); // 30
```


---
# notação arrow e this

felizmente, a notação arrow (ES6) impõe `lexycal scoping`

```javascript
const o = {
  n: 10,
  multiplicador: function multiplicador(fator) {
    return () => {
      return fator * this.n; // este this é do escopo acima
    };
  }
}

const double = o.multiplicador(2);
const triple = o.multiplicador(3);
console.log(double()); // 20
console.log(triple()); // 30
```

---
# funções retornadas por métodos

e qual valor é ligado ao `this` abaixo?

```javascript
const o = {
  n: 10,
  multiplicador: function multiplicador(fator) {
    return function () {
      return fator * this.n;
    };
  }
}
```

--
count: false
para testar, declare uma variável global...
```
n = 1000;
const double = o.multiplicador(2);
const triple = o.multiplicador(3);
console.log(double()); // 2000
console.log(triple()); // 3000
```


---
# bind

```javascript
function dobro(x) {
    return 2 * x;
}

const a = {x: 10, d: dobro.bind(null, this.x)};
 
a.x; // 10
a.d; // [Function: dobro]

a.d();      // 20
dobro(a.x); // 20
```

