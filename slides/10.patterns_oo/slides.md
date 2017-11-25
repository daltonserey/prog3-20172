class: center, middle
# Patterns OO em JavaScript
### ©2017 Dalton Serey, Programação 3, UFCG
.center[![Right-aligned image](https://i.stack.imgur.com/Mmww2.png)]
---
# número racional

qualquer número que pode ser expresso na forma

<script type="math/tex; mode=display">
  \frac{p}{q}, \text{ em que } p, q \in \mathbb{Z} \text { e } q \neq 0
</script>

---
# operações: igualdade

igualdade

<script type="math/tex; mode=display">
  \begin{aligned}
  & \frac{p}{q} = \frac{r}{s} && \text{ se e somente se } && ps = qr
  \end{aligned}
</script>

---
# operações: simplificação (ou redução)

- se $gcd(p, q)$ é i gual a 1, dizemos que $p/q$ é _irredutível_

   - e que $p / q$ é a forma simplificada/reduzida do número

- se $gcd(p, q) > 1$, dizemos que $p/q$ é _redutível_

   - e que a forma simplificada/reduzida do número é dada por

<script type="math/tex; mode=display">
  \begin{aligned}
  & \frac{p \div d}{q \div d} && \text{ onde $d = gcd(p, q)$}
  \end{aligned}
</script>


---
# operações: soma e subtração

<br>

<script type="math/tex; mode=display">
  \frac{p}{q} + \frac{r}{s} = \frac{ps + qr}{qs} 
</script>

<br>

<script type="math/tex; mode=display">
  \frac{p}{q} - \frac{r}{s} = \frac{ps - qr}{qs} 
</script>

---
# operações: produto e divisão

<br>

<script type="math/tex; mode=display">
  \frac{p}{q} \times \frac{r}{s} = \frac{pr}{qs}
</script>

<br>

<script type="math/tex; mode=display">
  \begin{aligned}
  & \frac{p}{q} \div \frac{r}{s} = \frac{ps}{qr} && \text{ com $r \neq 0$}
  \end{aligned}
</script>

---
class: center, middle
#implementação
<span style="font-size: 300px">1</span>
#funcional pura
---
# representação de racionais

podemos representar racionais como objetos em JavaScript

```
const r1 = { num: 1, den: 2 };
const r2 = { num: 1, den: 4 };
const r3 = { num: 2, den: 3 };
const r4 = { num: 3, den: 5 };
```

<br>
--
count: false
dessa forma, contudo, expomos detalhes de implementação para o usuário ☹ 

---
# representação abstrata de racionais

para representar de forma abstrata, usamos uma função construtora

```
function racional(num, den) {
    return {num: num, den: den};
}
```

--
count: false
<br>
embora pareça fazer o mesmo, permite expressar racionais assim
```
const r1 = racional(1, 2);
const r2 = racional(1, 4);
const r3 = racional(2, 3);
const r4 = racional(3, 5);
```

<br>
assim, o usuário é exposto a detalhes da implementação 👍 

---
# um pouco de robustez

vamos garantir que usos “abusivos” sejam corrigidos

```
function racional(num, den) {
    return { num: Math.truc(num), den: Math.trunc(den) };
}
```

--
count: false
agora o usuário não consegue criar racionais impróprios 
```
const r1 = racional(1, 2);
const meio = racional(1.5, 2.2); # igual a r1
```

.footnote[mais adiante, veremos como tornar _properties_ imutáveis]

---
# igualdade

```
function igual(r1, r2) {
    return r1.num * r2.den == r1.den * r2.num;
}
```

<br>
agora podemos fazer
```
const r1 = racional(1, 2);
const r2 = racional(1, 3);

igual(r1, r2);                          // false
igual(r1, racional(3, 6));              // true
igual(r2, racional(2, 6));              // true
igual(racional(2, 5), racional(6, 15)); // true
```

---
# simplificação

```
function simpl(r) {
    const gcd = (a, b) => b === 0 ? a : gcd(b, a % b);

    const dc = gcd(r.num, r.den);
    return racional(r.num / dc, r.den / dc);
}
```

<br>
agora podemos escrever
```
const r = racional(4, 8); // { num: 4, den: 8 }
const s = simpl(r);    // { num: 1, den: 2 }
igual(r, s);             // true
```

---
# operações aritméticas

```
function soma(r1, r2) {
    const n = r1.num * r2.den + r2.num * r1.den;
    const d = r1.den * r2.den;
    return simpl(racional(n, d));
}

function sub(r1, r2) {
    const n = r1.num * r2.den - r2.num * r1.den;
    const d = r1.den * r2.den;
    return simpl(racional(n, d));
}

function mult(r1, r2) {
    const n = r1.num * r2.num;
    const d = r1.den * r2.den;
    return simpl(racional(n, d));
}

function div(r1, r2) {
    const n = r1.num * r2.den;
    const d = r1.den * r2.num;
    return simpl(racional(n, d));
}
```

---
# toString()

é sempre bom dispor de uma forma _string_ apropriada
```
function toString(r) {
  return `${r.num}/${r.den}`;
}
```

assim
```
const r1 = simpl(racional(5, 10));
toString(r1);                        // '1/2'
console.log(`r1 = ${toString(r1)}`); // r1 = 1/2
```

---
# aplicação

vejamos como é usar a _biblioteca_ que fizemos

<script type="math/tex; mode=display">
    x = [(r1 + r2) \cdot (r3 - r4)] \div [r3 \cdot (r1 - r4)]
</script>

<script type="math/tex; mode=display">
    s = r1 + r2 + r3 + r4 + r5
</script>

--
count: false
```
const x = div(mult(soma(r1, r2), sub(r3, r4)), mult(r3, sub(r1, r4)));
```

--
count: false
```
const s = soma(soma(soma(soma(r1, r2), r3), r4), r5);
```
ou
--
count: false
```
const s = soma(r1, soma(r2, soma(r3, soma(r4, r5))));
```

---
# código completo

solução funcional

https://repl.it/@daltonserey/racionaisfuncjs

---
# qualidades da solução 👍 

1. é fácil de entender

   - por serem funções puras, todas as funções sempre que
     invocadas produzem novos valores, sem depender ou alterar de
     dados já presentes na memória; a operação de todas as
     funções depende apenas dos parâmetros e do código;

2. pode ser usada parcialmente

   - algumas das funções dependem de outras (por exemplo, `soma`
     depende de `simpl` e de `racional`); contudo, não há
     dependências entre todas elas; logo, em contextos em que
     apenas algumas sejam necessárias, pode-se excluir as que não
     forem necessárias;

3. usa espaço de funções forma eficiente

   - cada função só é criada uma única vez, sem importar a
     quantidade de números racionais que seja necessário
     manipular;

---
# problemas da solução 👎

1. não usa espaço de objetos de forma eficiente

   - a cada invocação as funções produzem novos valores (algumas
     mais de um); logo, uma única expressão pode produzir
     inúmeros valores intermediários que são, ao final do
     processo, apenas _lixo_ na memória;

2. as funções ocupam o escopo global e não formam uma unidade

   - por serem declaradas como funções simples, as funções ocupam
     e poluem o escopo do código cliente; além disso, as funções
     não estão agrupadas em uma única entidade;

3. a legibilidade do código “cliente” é discutível

   - por usar notação funcional clássica, a ordem sintática das
     funções é inversa à ordem de aplicação; o código, portanto,
     não reflete bem a ordem de execução das operações,
     dificultando escrita e leitura;

---
class: center, middle
#implementação
<span style="font-size: 300px">2</span>
#OO ingênua

---
# ideia central

criar uma versão OO a partir da versão funcional

- transformando as funções em métodos
- colocando-as como properties dos objetos

como podemos transformar uma função em método?

- eliminamos um dos objetos recebidos como parâmetro
- e usamos `this` para nos referirmos ao objeto
- isto permite usar a notação OO na invocação

---
# igualdade

transformemos a função `igual` que originalmente é...

```
function igual(r1, r2) {
    return r1.num * r2.den == r1.den * r2.num;
}
```

--
count: false

troquemos `r1` por `this`...
```
function igual(r2) {
    return this.num * r2.den == this.den * r2.num;
}
```

agora podemos fazer
```
const r1 = racional(1, 2);
const r2 = racional(1, 3);

r1.igual(r2); // ao invés de igual(r1, r2)
r1.igual(racional(3, 6));
r1.igual(racional(2, 6)); 
```

---
# simplificação

a função `simpl`
```
function simpl(r) {
    const gcd = (a, b) => b === 0 ? a : gcd(b, a % b);

    const dc = gcd(r.num, r.den);
    return racional(r.num / dc, r.den / dc);
}
```

pode ser transformada no método `simpl`
```
function simpl() {
    const gcd = (a, b) => b === 0 ? a : gcd(b, a % b);

    const dc = gcd(this.num, this.den);
    return racional(this.num / dc, this.den / dc);
}
```

---
# operações aritméticas

e o mesmo pode ser feito com as demais funções...
```
function soma(r2) {
    const n = this.num * r2.den + r2.num * this.den;
    const d = this.den * r2.den;
    return racional(n, d).simpl();
}
```

```
function toString() {
  return `${this.num}/${this.den}`;
}
```
...

---
# o construtor servirá como _container_

```
function racional(num, den = 1) {

    function toString() { … }
    function simpl() { … }
    function igual(r2) { … }
    function soma(r2) { … }
    function mult(r2) { … }

    return {
        num: Math.trunc(num),
        den: Math.trunc(den),
        igual, simpl, soma, mult, toString
    }
}
```

---
# código completo

solução OO ingênua

https://repl.it/@daltonserey/racionaisoo1js

---
# características importantes

- no _repl_ os racionais são pouco legíveis
   - todo racional tem seus próprios `num` e `den`
   - mas tem também `igual`, `toString`, `simpl`, `soma`...

- do ponto de vista de uso de memória é pior que a versão 1
   - o mesmo número de objetos intermediários é criado
   - e cada objeto tem uma **cópia de todas as funções**
```
r1.soma === r2.soma; // false (?!)
r1.soma === r3.soma; // false (?!)
r1.soma === r4.soma; // false (?!)
r1.soma === r5.soma; // false (?!)
r2.soma === r3.soma; // false (?!)
r2.soma === r4.soma; // false (?!)
r2.soma === r5.soma; // false (?!)
r3.soma === r4.soma; // false (?!)
r3.soma === r5.soma; // false (?!)
r4.soma === r5.soma; // false (?!)
```

---
# é uma boa solução?

1. legibilidade e organização? _excelente_

   - código cliente simples e fácil de ler/escrever
   - código de implementação também é simples e fácil de ler
   - métodos ainda são _puramente funcionais_
      - lembre que o cliente deve usar a notação oo
      - logo, `this` pode ser visto como parâmetro dos métodos

2. eficiência? _discutível_

   - depende de sua aplicação; pergunte-se:
      - quantos objetos serão instanciados?
      - qual o tamanho dos objetos? (inclua nisso as funções)

.center[<div style="font-size: 100%;">
<table width="100%">
 <tr>
   <td> </td>
   <td> objetos pequenos </td>
   <td> objetos grandes </td>
 </tr>
 <tr>
   <td> poucas instâncias </td>
   <td align="center"> 👍  </td>
   <td align="center"> 🤔 </td>
 </tr>
 <tr>
   <td> muitas instâncias </td>
   <td align="center"> 🤔 </td>
   <td align="center"> 👎 </td>
 </tr>
</table>
</div>]


---
class: center, middle
#implementação
<span style="font-size: 300px">3</span>
#OO com protótipo

---
# ideia central

usar um protótipo para “fatorar” os objetos

- todas os métodos serão colocados no protótipo
- logo, todos os objetos compartilharão essas funções

---
# 1 construtor, 3 propósitos

nosso construtor já acumula dois propósitos

- a de construtor efetivamente (ele cria novos objetos)
- e a de container (ele empacota todo o código de racionais)

daremos a ele mais um

- a de criar e vincular o protótipo aos novos objetos

---
# criando um protótipo: tentativa 1

podemos criar um protótipo assim no construtor
```
function racional(num, den = 1) {
  … 
  const proto_racional = {igual, simpl, soma, mult, toString};
  novo = { num: Math.trunc(num), den: Math.trunc(den) };
  Object.setPrototypeOf(novo, proto_racional);
  return novo;
}
```
--
count: false
funciona, mas… 🤔 

- criamos um novo protótipo para a cada invocação 

---
# criando um protótipo: tentativa 2

podemos tentar criá-lo fora do construtor
```
const proto_racional = {igual, simpl, soma, mult, toString};
function racional(num, den = 1) {
  … 
  novo = { num: Math.trunc(num), den: Math.trunc(den) };
  Object.setPrototypeOf(novo, proto_racional);
  return novo;
}
```
--
count: false
não funciona 👎 

- os métodos não estão no escopo global

---
# criando um protótipo: tentativa 3

podemos tentar combinar as duas abordagens
```
let proto_racional;
function racional(num, den = 1) {
  proto_racional = {igual, simpl, soma, mult, toString};
  … 
  novo = { num: Math.trunc(num), den: Math.trunc(den) };
  Object.setPrototypeOf(novo, proto_racional);
  return novo;
}
```
--
count: false
funciona, mas… 🤔 

- poluímos o escopo global com `proto_racional`

---
# criando um protótipo: tentativa 4

mas… lembre que funções são objetos, logo… 
```
function racional(num, den = 1) {
  racional.prototipo = racional.prototipo || {igual, simpl, soma, mult, toString};
  … 
  novo = { num: Math.trunc(num), den: Math.trunc(den) };
  Object.setPrototypeOf(novo, prototipo);
  return novo;
}
```
--
count: false
funciona! 👍 

- sem poluir o escopo global
- um protótipo é criado apenas na primeira invocação
- nas demais invocações, o protótipo é reusado 👏 👏 👏 

---
# código completo

solução OO com protótipo

https://repl.it/@daltonserey/racionaisoo2js

---
# características importantes

- o código da implementação continua simples de ler e entender

- o código cliente permanece idêntico ao da versão anterior

- usa memória de forma eficiente (em parte)
   - métodos são compartilhados pelos objetos
   - mas objetos intermediários ainda são criados

- embora seja OO, a solução não deixa de ser _funcional_

   - os racionais são imutáveis por construção
   - cada método é uma função pura (`this` é parâmetro implícito)

- problema: o construtor acumula propósitos demais

   - é construtor propriamente dito
   - é container da implementação (código provedor)
   - e é controlador do protótipo

---
class: center, middle
#implementação
<span style="font-size: 300px">4</span>
# módulo racionais

---
# ideia central

criar um módulo `racionais` que

- sirva de container do código
- e de controlador do protótipo

o construtor deve ser uma função a mais do módulo

- isso simplifica o construtor
- e o coloca no mesmo nível hierárquico que os métodos

---
# código cliente do módulo

a inspiração é o conceito de módulo de python

relembre como funciona um `import` em python
```python
from math import cos as coseno
```
--
count: false
podemos escrever algo semelhante em JavaScript
```
const coseno = math().cos;
```
--
count: false
ou para importar o construtor `racional` do módulo `racionais`
```
const rac = racionais().racional;
```
--
count: false
veja que no escopo local, chamamos `racional` de `rac`

---
# módulo racionais

```
function racionais() {

  function toString() { … }
  function simpl() { … }
  function igual(r2) { … }
  function soma(r2) { … }
  function mult(r2) { … }
  function racional(num, den = 1) {
    const novo = Object.create(racionais.prototipo);
    novo.num = Math.trunc(num);
    novo.den = Math.trunc(den);
  }

  racionais.prototipo = racionais.prototipo || {toString, mult, soma, igual, simpl};
  return { racional };
}
```
- métodos e construtor no mesmo nível hierárquico
- o objeto retornado dá acesso à parte pública módulo
- o que não está no objeto é privado
- logo, ainda podemos melhorar
   - o protótipo pode ser armazenado via _closure_

---
count: false
# módulo racionais

```
function racionais() {

  function toString() { … }
  function simpl() { … }
  function igual(r2) { … }
  function soma(r2) { … }
  function mult(r2) { … }
  function racional(num, den = 1) {
*   const novo = Object.create(prototipo);
    novo.num = Math.trunc(num);
    novo.den = Math.trunc(den);
  }

* const prototipo = {toString, mult, soma, igual, simpl};
  return { racional };
}
```
- métodos e construtor no mesmo nível hierárquico
- o objeto retornado dá acesso à parte pública módulo
- o que não está no objeto é privado
- logo, ainda podemos melhorar
   - o protótipo pode ser armazenado via _closure_ 👆

---
# código completo

solução OO em um módulo

https://repl.it/@daltonserey/racionaisoo3js

---
# características importantes

- sobre o código
   - código provedor permanece simples de ler e entender
   - código cliente, requer uma linha a mais: o import
   - dá mais flexibilidade no nome a ser usado localmente

- continua sendo OO e funcional
   - mas as funções acumulam menos propósitos
   - construtor é apenas construtor
   - o módulo exerce as funções que cabem a um módulo
      - container do código provedor
      - controle e configuração (provê o protótipo via _closures_)

---
# exercício

Implemente em JavaScript o conceito de polinômios de uma
variável ao estilo do que fizemos nestes slides.

1. Construa uma versão puramente funcional.

2. Em seguida, produza uma versão OO ingênua.

3. Em seguida, crie uma versão OO que use um protótipo criado
diretamente pelo construtor.

4. Finalmente, crie um módulo OO `polinomios` que permita
importar de forma discricionária o construtor.
