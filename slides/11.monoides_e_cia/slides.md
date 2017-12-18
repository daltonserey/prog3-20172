class: center, middle
# Monóides, semigrupos e magmas
### ©2017 Dalton Serey, Programação 3, UFCG
.center[![Right-aligned image](https://i.stack.imgur.com/Mmww2.png)]

---
# patterns funcionais

- em programação funcional, não há _design patterns_.red[<sup>1</sup>] no sentido GoF

   - embora vários deles possam ser úteis em JavaScript

.footnote[.red[<sup>1</sup>] _Design pattern_: solução reutilizável para um problema recorrente.]

--
count: false

- mas há padrões que podemos chamar de _patterns funcionais_

   - são soluções que podem ser reutilizadas
   - para problemas recorrentes com estruturas semelhantes

- neste módulo, veremos um desses padrões: _monóides_ (e semelhantes)

--
count: false
- antes, contudo, é conveniente revermos/definirmos dois conceitos:
   - assinaturas e aridade de funções

---
# assinatura

a _assinatura_ de uma função é definida…

- pelo tipo e ordem dos argumentos (de entrada)
- e pelo tipo do valor do resultado (de saída)

---
# notação para assinatua

por simplicidade, usarei a notação matemática para assinaturas, na forma:

.center[$\langle$_domínio_$\rangle \rightarrow \langle$_contradomínio_$\rangle$]

onde
- _domínio_ é o produto cartesiano dos tipos de entrada
- e _contradomínio_ é o tipo de saída da função

--
count: false
<br>
são exemplos de assinaturas:
> $\mathbb{Z} \rightarrow \mathbb{N}$<br>
> $\mathbb{R} \times \mathbb{N} \rightarrow \mathbb{R}$<br>
> `string` $\times$ `number` $\rightarrow$ `string`

---
# aridade 
a _aridade_ é o número de argumentos de entrada de uma função

uma função é dita…
- _nulária_ (ou 0-ária) se não tem argumentos
- _unária_ se tem um únco argumento
- _binária_ se tem dois argumentos ou
- _n-ária_ se tem $n$ argumentos (tipicamente, pra $n > 2$)

veja que a aridade pode ser vista como parte da assinatura

- ou, no mínimo, podemos dizer que é função dela
- às vezes, contudo, é conveniente poder distingui-la

---
# compatibilidade de funções

a _compatibilidade_ de duas funções depende de suas assinaturas

só podemos compor a função $f_1$ com $f_2$ se:

- o tipo de saída de $f_2$ for um dos tipos de entrada de $f_1$

--
count: false
<br>
por exemplo, só podemos escrever $f(g(…), h(…))$ se

.center[$\begin{aligned}
  &  g : … \rightarrow \mathbb{D}
  && h : … \rightarrow \mathbb{E}
  && f : \mathbb{D} \times \mathbb{E} \rightarrow … 
  \end{aligned}$]
  
- ou seja, se as saídas de $g$ e $h$ são dos tipos de entrada de $f$

.footnote[note que acima, usamos .red[$…$] para indicar um tipo qualquer]

---
# mais assinaturas ⇒  menos compatibilidade

em um conjunto de funções, quanto mais assinaturas, menos compatibilidade

<script type="math/tex; mode=display">
  \begin{aligned}
    & f : \mathbb{D} \rightarrow \mathbb{C} &&
    g : \mathbb{A} \times \mathbb{B} \rightarrow \mathbb{D} && \\
    & h : \mathbb{C} \times \mathbb{B} \rightarrow \mathbb{A} &&
    k : \mathbb{A} \times \mathbb{D} \times \mathbb{B}
    \rightarrow \mathbb{B}  \\
    \\

     & f(g(d)) &&
     h(f(g(d)), k(a, d, k(a_2, d_2, b))) \\
     & h(f(d), f(d_2)) &&
     h(f(g(a, b)), k(h(c, b), d, b_2)) \\
  \end{aligned}
</script>

--
count: false
<br>
quais composições acima são válidas/inválidas? 
- a dificuldade vem, em parte, da diversidade de tipos e assinaturas
- verificar as aplicações = verificar compatibilidade das funções
- porque nem todas as combinações são possíveis...

---
# menos é mais

claramente, uniformizar assinaturas maximiza compatibilidade

é isso que o padrão _monóide_.ref[✱] procura promover

.footnote[.ref[✱] De fato, o mesmo se aplica a _semigrupos_ e a _magmas_. Mais adiante, esclarecemos as diferenças.]

--
count: false
a ideia é partir de duas ideias muito simples:
- restringir as operações a atuar sobre um único tipo
- e usar uma assinatura padronizada

---
# menos assinaturas ⇒ mais compatibilidade

simplificar e padronizar assinaturas aumenta as chances

- de que as funções produzam valores válidos para todo contexto
- de que mais combinações de funções sejam válidas
- e de que a verificação se reduza a checar a aridade

--
count: false
e, talvez, ainda mais importante:

- facilita a leitura e a escrita das aplicações
- permite focar na semântica (efeito) das funções
- permite focar em identificar combinações _úteis_
   - já que, válidas, todas são

--
count: false
sabendo dos porquês e _praquês_, passemos à definição formal de _monóide_

---
# monóide

um _monóide_ é uma operação $f$ definida sobre um tipo $\mathbb{D}$ que…
  

   - é _binária_ e _fechada_ sobre $\mathbb{D}$, ou seja, tem assinatura
     $\mathbb{D} \times \mathbb{D} \rightarrow \mathbb{D}$

   - é _associativa_

   - e _tem elemento neutro_

--
count: false
em que
- _ser associativa_ significa que

  $f(f(a, b), c) = f(a, f(b, c))$ para todos $a, b, c \in \mathbb{D}$

- _ter elemento neutro_ significa que existe algum $\mathcal{N} \in \mathbb{D}$ tal que

  $f(a, \mathcal{N}) = f(\mathcal{N}, a) = a$, para todo $a \in \mathbb{D}$


---
# observações
- formalmente, o monóide é a estrutura $\langle \mathbb{D}, f\rangle$
  - ou seja, o monóide é o par $\langle$tipo, operação$\rangle$

- na prática, dizemos que $f$ é um monóide, quando $\mathbb{D}$ é subentendido

- as três condições são chamadas de _axiomas ou leis do monóide_

---
# exemplo 1: adição de inteiros

- a estrutura $\langle \mathbb{Z}, +\rangle$ é um monóide

   - o tipo base são os inteiros $\mathbb{Z}$
   - a operação $+$ é a soma de inteiros
   - é associativa: $a + (b + c) = (a + b) + c$, pra quaisquer $a, b$ e $c$
   - tem elemento neutro: $0$, pois $a + 0 = 0 + a = a$, pra qualquer $a$

---
# exemplo 2: multiplicação de inteiros

- a estrutura $\langle \mathbb{Z}, \times\rangle$ é um monóide

   - o tipo base são os inteiros $\mathbb{Z}$
   - a operação $\times$ é a multiplicação de inteiros
   - é associativa: $a \times (b \times c) = (a \times b) \times c$, pra quaisquer $a, b$ e $c$
   - tem elemento neutro: $1$, pois $a \times 1 = 1 \times a = a$, pra qualquer $a$

---
# exemplo 3: concatenação de strings

- a estrutura $\langle$`string`, `+`$\rangle$, em JavaScript, é um monóide

   - o tipo base é `string` de JavaScript
   - a operação `+` é a concatenação de strings
   - é associativa: `s1 + (s2 + s3) === (s1 + s2) + s3`, pra todos `s1`, `s2` e `s3`
   - tem elemento neutro: `''`, pois `s + '' === '' + s === s`, pra todo `s`


---
class: center, middle
# para quê servem as leis.ref[✱] do monóide?

.left[.footnote[.ref[✱] ou _axiomas_]]
---
# pra que binária e fechada?

função binária e fechada = _sempre_ produz **um valor** a partir de **outros dois**

⇒ nos permite encadear sequências de $n \geq 2$ valores de $\mathbb{D}$

- $\langle a, b, c\rangle$ podem ser combinados por $f(f(a, b), c)$
- $\langle a, b, c, d\rangle$ podem ser combinados por $f(f(f(a, b), c), d)$
- e assim por diante,
- como se a função se aplicasse a qualquer número $n > 2$ de valores

--
count: false
<br>
ao criarmos um monóide, portanto, definimos uma função binária e fechada
- mas, na prática, ganhamos uma forma de aplicar a função
- a um número arbitrário de $n > 2$ argumentos

---
# pra que associativa?

função associativa = a **ordem de aplicação** em sequências é **irrelevante**

--
count:
⇒ nos permite usar notação $n$-ária de forma não ambígua

- $f(a, b, c)$ ao invés de $f(f(a, b), c) = f(a, f(b, c))$
- $f(a, b, c, d)$ ao invés de $f(f(f(a, b), c), d) = f(a, f(b, f(c, d)))$
- e assim por diante

--
count: false
⇒ nos permite fazer as aplicações na ordem mais conveniente

- sequencial: $f(f(f(f(a, b), c), d), e)$ ou $f(a, f(b, f(c, f(d, e))))$
- recursiva incremental: $f(f(a, b, c, d), e)$
- em blocos e/ou em paralelo: $f(f(a, b), f(c, d, e))$
- graças à associatividade, o resultado é **sempre** o mesmo $f(a, b, c, d, e)$

--
count: false
ou seja, partimos de uma função binária, fechada e associativa, mas
- ganhamos uma função $n$-ária (com $n \geq 2$)
- que pode ser resolvida iterativamente, recursivamente ou em paralelo

---
# monóides e reduce

certamente, você já percebeu como tudo isto lembra a função [`reduce`](/07.funcoes_de_alta_ordem/#12)

de fato, ela faz _exatamente_ isso: estende uma função binária para $n$-ária
```
[x, y, z, …].reduce((a, e) => op(a, e))
```
--
count: false
veja que podemos dizer que seu efeito é:
- aplicar a função recebida como argumento
- aos $n$ elementos do `array` de forma iterativa (de dois em dois)

observe que: a operação recebida por `reduce` precisa ser
  _binária_ e _fechada_
   - deve receber dois valores do mesmo tipo: $\langle$_a_, _e_$\rangle$
   - e retornar um terceiro valor do mesmo tipo que combina _a_ e _e_

--
count: false
conclusão: para usar `reduce` com segurança, defina _monóides_

---
# pra que ter elemento neutro?

ainda falta entender a última lei dos monóides: ter elemento neutro

função com elemento neutro = tem um **valor de partida**.ref[✱] $\mathcal{N} \in \mathbb{D}$

.footnote[.ref[✱] o equivalente ao que o zero é para a soma]

--
count: false
⇒ nos permite estender a função para que seja _nulária_ e _binária_

- ou seja, permite definir que $f() = \mathcal{N}$
- e ainda que $f(e) = f(\mathcal{N}, e) = f(e, \mathcal{N}) = e$

--
count: false
⇒ em termos de `reduce`, ter elemento neutro nos permite
- reduzir arrays vazios: `[].reduce(…)` $\mapsto \mathcal{N}$ 
- e arrays com um único elemento: `[e].reduce(…)` $\mapsto$ `e`

---
# compatibilidade de monóides

monóides definidos sobre um mesmo tipo são, naturalmente, compatíveis

logo, diferentes operações podem ser _livremente combinadas_

- dado que todas têm o mesmo tipo de entrada e de saída
- ⇒ é fácil identificar composições válidas: todas são!

--
count: false
por exemplo, sabendo que $\times$ e $+$ são monóides sobre $\mathbb{Z}$, podemos escrever

- $e \times ((a + b) \times (c + d))$ ou $e + ((a \times b) + (c \times d))$
- sem maiores preocupações com tipos e assinaturas.ref[✱]

é excelente motivo para definir vários monóides sobre um mesmo tipo

.footnote[.ref[✱] muito embora a semântica possa diferir: $a
\times (b + c) \neq a + (b \times c)$]

---
class: center, middle
.right[ _é preferível ter 100 funções que operam sobre uma estrutura<br>
de dados do que 10 funções sobre 10 estruturas de dados_<br>
<span style="font-size: small;">(Alan Perllis, Epigrams on Programming)</span>]



---
# em resumo, monóides permitem...

definir uma função binária e fechada sobre um tipo que

- pode ser aplicada a um número arbitrário de argumentos
- e que tem elevado grau de compatibilidade com outras funções

--
count: false
há quem diga que Legos e Tetris foram inspirados em monóides 😁

- para que possam ser combinadas mais facilmente…
- …as peças devem ser mais simples e não mais complexas

---
class: center, middle
# exemplos

---
# exemplo 4: subtração de inteiros

$\langle \mathbb{Z}, -\rangle$ **não** é um monóide:
- não é associativa
- e não tem elemento neutro

logo, não podemos executar as operações em qualquer ordem

- porque $(a - b) - c \neq a - (b - c)$
- nem podemos agrupar ou paralelizar a execução

expressões sem parênteses têm uma ordem implícita de execução

> $a - b - c - d - e = (((a - b) - c) - d) - e$

---
# exemplo 5: concatenação de strings

- $\langle$`string`, `+`$\rangle$, em JavaScript, é um monóide

   - o tipo base é `string` de JavaScript
   - a operação `+` é a concatenação de strings
   - é associativa: `s1 + (s2 + s3) === (s1 + s2) + s3`, pra todos `s1`, `s2` e `s3`
   - tem elemento neutro: `''`, pois `s + '' === '' + s === s`, pra todo `s`

- $\langle$`string`, `+`$\rangle$ é um monóide ⇒  podemos escrever

   ```
   'a' + 'bc' === 'abc'; // por definição
   'a' + 'bc' + 'de' === 'abcde'; // por extensão
   ('a'+'bc') + 'de' === 'a' + ('bc'+'de'); // parênteses são opcionais
   ['a', 'bc', 'de'].reduce((s1, s2) => s1 + s2) === 'abcde';
   ['a', 'bc', 'de', 'x'].reduce((s1, s2) => s1 + s2) === 'abcdex';
   ['a'].reduce((s1, s2) => s1 + s2) === 'a';
   [].reduce((s1, s2) => s1 + s2, '') === ''; // Ok!
   // [].reduce((s1, s2) => s1 + s2) === ''; // TypeError, qual o elemento neutro?
   ```


---
# exemplo 6: max sobre numbers

```
const max = (a, b) => Math.max(a, b);
```
a estrutura $\langle$`number`, `max`$\rangle$, em JavaScript, é um monóide

- o tipo base é `number` de JavaScript
- é associativa e tem elemento neutro `-Infinity`; para números `a`, `b` e `c`
     ```
     max(a, max(b, c)) === max(max(a, b), c);
     max(-Infinity, a) === max(a, -Infinity) === a;
     ```

portanto, podemos escrever
```
max(1, 4) === 4; // por definição
max(1, 4, 3) === 4; // por extensão da definição
max(max(1, 3), max(5, 4)) === max(max(max(1, 3), 5), 4);
[1, 3, 5, 4].reduce(max) === 5;
[3].reduce(max) == 3;
[].reduce(max, -Infinity) === -Infinity;
// [].reduce(max); // TypeError, qual o elemento neutro?
```

---
# exemplo 7: and sobre booleans

a estrutura $\langle$`boolean`, `&&`$\rangle$, em JavaScript, é um monóide

- o tipo base é `boolean` de JavaScript
- a operação `&&` é a conjunção lógica
- é associativa e tem elemento neutro `true`; para booleans `a`, `b` e `c`
  ```
  a && (b && c) === (a && b) && c;
  true && a === a && true === a;
  ```

portanto, podemos escrever

```
true && false === false; // por definição
true && true && true === true; // por extensão da definição
[true, true, false, true].reduce((a, b) => a && b) === false;
[true].reduce((a, b) => a && b) === true;
[].reduce((a, b) => a && b, true) === true;
// [].reduce((a, b) => a && b); // TypeError, qual o elemento neutro?
```

---
# exemplo 8: or sobre booleans

a estrutura $\langle$`boolean`, `||`$\rangle$, em JavaScript, é um monóide

- o tipo base é `boolean` de JavaScript
- a operação `||` é a disjunção lógica
- é associativa e tem elemento neutro `false`; para booleans `a`, `b` e `c`
  ```
  a || (b || c) === (a || b) || c;
  false || a === a || false === a;
  ```

portanto, podemos escrever

```
true || false === true; // por definição
true || true || true === true; // por extensão da definição
[true, true, false, true].reduce((a, b) => a || b) === true;
[true].reduce((a, b) => a || b) === true;
[].reduce((a, b) => a || b, false) === false;
// [].reduce((a, b) => a || b); // TypeError, qual o elemento neutro?
```

---
# monóides, semigrupos e magmas

em algumas situações parte das leis podem não se aplicar

ainda assim, os tipos e operações podem ser úteis

- se a operação é binária e fechada, dizemos que é um _magma_

- e se a operação é binária, fechada e associativa, é um _semigrupo_

observe que todo monóide é também um _semigrupo_ e um _magma_

- mas que o contrário não é verdade
- $\langle \mathbb{Z}, -\rangle$ é um _magma_ (não é associativa,
  nem tem elemento neutro)
- $\langle \mathbb{N}^{+}, +\rangle$ é um _semigrupo_ (não tem
  elemento neutro)
- $\langle$`intersecao`, `Array`$\rangle$ é um _semigrupo_

na prática, contudo, o uso dos termos _magma_ e _semigrupo_ é mais raro

---
# hierarquia monóides, magmas e semigrupos

incluir figura

---
# monóide livre

o _monóide livre_ sobre um tipo $\mathbb{D}$ é o monóide $\langle \mathbb{D}^{*}, \centerdot\rangle$, em que:

- $\mathbb{D}^{*}$ é o conjunto de listas de elementos de $\mathbb{D}$
- e $\centerdot$ é a operação de concatenação de listas

--
count: false
é a ideia de que _qualquer tipo_ dispõe de pelo menos um monóide

- basta para isso, considerar _listas dos elementos_
- e usar a _concatencação de listas_ como a operação

observe que concatenação de listas é um monóide
- é binária e fechada
- é associativa: $a \centerdot (b \centerdot c) = (a \centerdot b) \centerdot c$
- e tem elemento neutro (a lista vazia): $\langle
     \rangle \centerdot a = a \centerdot \langle \rangle= a$

---
# por que _livre_?

listas (elementos de $\mathbb{D}^{*}$) podem ser vistos como _operações adiadas_

--
count: false
melhor ainda, a operação em si pode ser definida a posteriori

- pode ser _livremente_ escolhida e aplicada sobre as listas
- esse é o motivo pelo qual é chamado de _monóide livre_

uma vez escolhida a operação, podemos usar _reduce_ para aplicá-la
- e, assim, _reduzir_ a lista para um único valor
- ou, formalmente, para _reduzir_ um valor de $\mathbb{D}^{*}$ a um valor de $\mathbb{D}$

---
# exemplos

com o _monóide livre_ $\langle \mathbb{Z}^{*}, \centerdot \rangle$ sobre os inteiros $\mathbb{Z}$ podemos escrever:

   - $\langle 4\rangle \centerdot \langle 3\rangle = \langle 4, 3\rangle$
   - $\langle 4, 3\rangle \centerdot \langle 7\rangle = \langle 4, 3, 7\rangle$
   - $\langle 4, 3\rangle \centerdot \langle \rangle = \langle 4, 3\rangle$
   - $\langle \rangle \centerdot \langle 4, 3\rangle = \langle 4, 3\rangle$
   - $\langle 3, 1, 2\rangle \centerdot \langle 4, 3\rangle \centerdot \langle 5\rangle = \langle 3, 1, 2, 4, 3, 5\rangle$

--
count: false
cada valor expressa _operações adiadas_ que podem ser executadas depois

   - $reduce(\langle 4, 3\rangle, +) = 7$
   - $reduce(\langle 4, 3\rangle, \times) = 12$
   - $reduce(\langle 4, 3, 7\rangle, +) = 14$
   - $reduce(\langle 3, 1, 2, 4, 3, 5\rangle, +) = 18$
   - $reduce(\langle 3, 1, 2, 4, 3, 5\rangle, \times) = 360$

---
# exemplo 9: concatenação de arrays

.footnote[.ref[✱] a operação `equals` sobre `arrays` não é parte do runtime JavaScript]

concatenação de arrays em JavaScript $\langle$`Array`, `concat`$\rangle$ é um monóide

   - `concat` é associativa e tem elemento neutro: para arrays `a`, `b` e `c`.ref[✱]
     ```
     equals(a.concat(b.concat(c)), (a.concat(b)).concat(c));
     equals([].concat(a), a.concat([]));
     ```

isso nos permite escrever

   ```
   [1, 4].concat([5]); // => [1, 4, 5] por definição
   [1, 4].concat([5], [7]) // => [1, 4, 5, 7] por extensão da definição
   const concat = (a, b) => a.concat(b);
   [[1, 4], [5], [7]].reduce(concat); // => [1, 4, 5, 7]
   [[1, 4], [5], [7], [3, 8]].reduce(concat); // => [1, 4, 5, 7, 3, 8]
   [[1, 3]].reduce(concat); //  => [1, 3] versão unária
   [[]].reduce(concat, []); // => [] versão unária 
   [].reduce(concat, []); // => [] versão nulária
   // [].reduce(concat); // TypeError, qual o elemento neutro?
   ```

---
# monóides não matemáticos

construir exemplos com:

- registros de logs
- registros de livros em uma biblioteca
- registros de vendas


---
# monóides e unix text utilities

- aspectos em comum
   - tipo de dado simples: texto
   - “_assinatura_” simples: entrada e saída de texto
   - sem estado envolvido
   - permite pipelines: _chaining_
 
- a ideia é um _stream_ de dados sendo transformado
