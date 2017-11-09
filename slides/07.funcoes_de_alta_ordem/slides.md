class: center, middle
# Funções de Alta Ordem
### ©2017 Dalton Serey, Programação 3, UFCG
.center[![Right-aligned image](https://i.stack.imgur.com/Mmww2.png)]
---
# programas = algoritmos + dados

mas qual é a linha que separa o que é dado e o que é algoritmo?
 
funções JavaScript são _dados_ ou são _algoritmos_?

--
count: false
- sabemos que funções expressam _procedimentos_ 

- ao compará-las como funções matemáticas, percebemos que
   - são mais especificação de cálculos que definições abstratas
   - modelam, portanto, mais **algoritmos** que especificações

--
count: false
- sabemos, por outro lado, que são objetos

- e por serem _cidadãs de primeira classe_, podem ser...
   - atribuídas a variáveis
   - armazenadas em arrays e objetos
   - passadas e recebidas como parâmetros
   - calculadas e criadas dinamicamente
   - são, portanto, sem nenhuma dúvida **dados**

???

Agora podemos usar “procedimentos” como dados. Isso nos faz
reduzir ou começar a questionar a separação de Wirth de que
falamos no início do curso de que algoritmos + dados = programas.
Agora os próprios algoritmos parecem ser dados.
---
# algoritmos vistos como dados

- base para a generalização de algoritmos

- a ideia é identificar generalidades e “fatorar”

   - se é geral ⇒ fatoramos e escrevemos em um “meta algoritmo”
   - não é geral ⇒ colocamos em dados e passamos como argumentos

- ideia mais fácil de _demonstrar_ que explicar...
---
# funções de alta ordem

são funções que:

   - recebem funções como parâmetros e/ou
   - retornar funções como resultados

<br>
as 3 funções _símbolo_ da programação funcional

   - _map_
   - _filter_ e
   - _reduce_

---
# exercícios

escreva as funções especificadas abaixo

```javascript
vezes_escalar([1, 2, 3, 4], 3); // [3, 6, 9, 12]

incremente([1, 2, 3, 4]); // [2, 3, 4, 5]

positivos([1, 2, -3, -4, 5, -2]) // [true, true, false, false, true, false]

quadrados([1, 2, 3, 4, 5]) // [1, 4, 9, 16, 25]
```

observe a similaridade entre as implementações
---
# map

função de alta ordem que produz um novo array contendo o
resultado da aplicação da função recebida a cada um dos elementos
do array original

- parâmetros: um array e uma função
- map aplicará a função a cada um dos elementos
- e produzirá um **novo array** com os elementos “mapeados”

```javascript
const v = [4, 11, 8, 6, 7, 10, 9, 6];
v.map(e => 2 * e); // [8, 22, 16, 12, 14, 20, 18, 12]
```
---
# exercícios

escreva as funções especificadas abaixo

```javascript
maiores_que_2([1, 2, 3, 4]); // [3, 4]

menores_que_2([1, 2, 3, 4]); // [1, 2]

so_positivos([1, 2, -3, -4, 5, -2]) // [1, 2, 5]

so_pares([1, 2, 3, 4, 5]) // [2, 4]
```

---
# filter

função de alta ordem que produz um novo array contendo os
elementos do array original que passam no _predicado_ recebido
como parâmetro

- um predicado é uma função booleana
- parâmetros: um array e um predicado
- filter aplicará o predicado a cada elemento do array
- e produzirá um **novo array** contendo os elementos “filtrados”

```javascript
const v = [4, 11, 8, 6, 7, 10, 9, 6];
v.filter(e => e % 2 === 1); // [11, 7, 9]
```
---
# exercícios

escreva as funções especificadas abaixo

```javascript
soma([1, 2, 3, 4]); // 10

produto([1, 2, 3, 4]); // 24

soma_positivos([1, 2, -3, -4, 5, -2]) // 8

soma_pares([1, 2, 3, 4, 5]) // 6
```


---
# reduce ♕

função de alta ordem que **produz um valor** através da aplicação de
um _redutor_ a um _acumulador_ e a cada um dos valores do array

- parâmetros: um array, um _redutor_ e, opcionalmente, um valor inicial
   - o array: contém os dados originais
   - o _redutor_: uma função que “reduz” dois valores (_e_ e _a_) a um só
      - parâmetros: _e_, um elemento e _a_, o _acumulador_
      - deve retornar o valor do acumulador para a próxima iteração
   - o valor inicial é apenas o valor do qual iniciar a operação
      - se não houver, deve ser usado o primeiro elemento
      - se o array for vazio, o resultado é o valor inicial
      - `TypeError` se for invocado para um array vazio e sem valor inicial

```javascript
const v = [4, 11, 8, 6, 7, 10, 9, 6];
v.reduce((a, e) => a + e); // 61
v.reduce((a, e) => a + e, 100); // 161
v.reduce((a, e) => a * e); // 7983360
```
???
- a “rainha” das funções de alta ordem
- calcula um valor inteiramente novo a partir de um array

---
# exemplo de funcionamento do reduce

```javascript
const v = [4, 11, 8, 6, 7, 10, 9, 6];
v.reduce((a, e) => a + e, 100);
```

- vejamos passo a passo as invocações do redutor
- observe que o redutor é: `(a, e) => a + e`


---
## funcionamento do reduce 
```javascript
(a, e) => a + e
```

<script type="math/tex; mode=display">
    \begin{aligned}
    & \underline{4}, 11, 8, 6, 7, 10, 9, 6\\
    \\
    &a = 100,\ e = 4\ \ && ⇒ 104\\
    \end{aligned}
</script>

---
count: false
## funcionamento do reduce 
```javascript
(a, e) => a + e
```
<script type="math/tex; mode=display">
    \begin{aligned}
    & 4, \underline{11}, 8, 6, 7, 10, 9, 6\\
    \\
    &a = 100,\ e = 4 && ⇒ 104\\
    &a = 104,\ e = 11 && ⇒ 115\\
    \end{aligned}
</script>

---
count: false
## funcionamento do reduce 
```javascript
(a, e) => a + e
```
<script type="math/tex; mode=display">
    \begin{aligned}
    & 4, 11, \underline{8}, 6, 7, 10, 9, 6\\
    \\
    &a = 100,\ e = 4 && ⇒ 104\\
    &a = 104,\ e = 11 && ⇒ 115\\
    &a = 115,\ e = 8 && ⇒ 123\\
    \end{aligned}
</script>

---
count: false
## funcionamento do reduce 
```javascript
(a, e) => a + e
```
<script type="math/tex; mode=display">
    \begin{aligned}
    & 4, 11, 8, \underline{6}, 7, 10, 9, 6\\
    \\
    &a = 100,\ e = 4 && ⇒ 104\\
    &a = 104,\ e = 11 && ⇒ 115\\
    &a = 115,\ e = 8 && ⇒ 123\\
    &a = 123,\ e = 6 && ⇒ 129\\
    \end{aligned}
</script>

---
count: false
## funcionamento do reduce 
```javascript
(a, e) => a + e
```
<script type="math/tex; mode=display">
    \begin{aligned}
    & 4, 11, 8, 6, \underline{7}, 10, 9, 6\\
    \\
    &a = 100,\ e = 4 && ⇒ 104\\
    &a = 104,\ e = 11 && ⇒ 115\\
    &a = 115,\ e = 8 && ⇒ 123\\
    &a = 123,\ e = 6 && ⇒ 129\\
    &a = 129,\ e = 7 && ⇒ 136\\
    \end{aligned}
</script>

---
count: false
## funcionamento do reduce 
```javascript
(a, e) => a + e
```
<script type="math/tex; mode=display">
    \begin{aligned}
    & 4, 11, 8, 6, 7, \underline{10}, 9, 6\\
    \\
    &a = 100,\ e = 4 && ⇒ 104\\
    &a = 104,\ e = 11 && ⇒ 115\\
    &a = 115,\ e = 8 && ⇒ 123\\
    &a = 123,\ e = 6 && ⇒ 129\\
    &a = 129,\ e = 7 && ⇒ 136\\
    &a = 136,\ e = 10 && ⇒ 146\\
    \end{aligned}
</script>

---
count: false
## funcionamento do reduce 
```javascript
(a, e) => a + e
```
<script type="math/tex; mode=display">
    \begin{aligned}
    & 4, 11, 8, 6, 7, 10, \underline{9}, 6\\
    \\
    &a = 100,\ e = 4 && ⇒ 104\\
    &a = 104,\ e = 11 && ⇒ 115\\
    &a = 115,\ e = 8 && ⇒ 123\\
    &a = 123,\ e = 6 && ⇒ 129\\
    &a = 129,\ e = 7 && ⇒ 136\\
    &a = 136,\ e = 10 && ⇒ 146\\
    &a = 146,\ e = 9 && ⇒ 155\\
    \end{aligned}
</script>

---
count: false
## funcionamento do reduce 
```javascript
(a, e) => a + e
```
<script type="math/tex; mode=display">
    \require{color}
    \begin{aligned}
    & 4, 11, 8, 6, 7, 10, 9, \underline{6}\\
    \\
    &a = 100,\ e = 4 && ⇒ 104\\
    &a = 104,\ e = 11 && ⇒ 115\\
    &a = 115,\ e = 8 && ⇒ 123\\
    &a = 123,\ e = 6 && ⇒ 129\\
    &a = 129,\ e = 7 && ⇒ 136\\
    &a = 136,\ e = 10 && ⇒ 146\\
    &a = 146,\ e = 9 && ⇒ 155\\
    &a = 155,\ e = 6 && ⇒ \colorbox{lightgreen}{161}\\
    \end{aligned}
</script>

---
# map via reduce

mapear cada elemento $e$ de v para $e + 1$

```javascript
v.reduce((a, e) => {
  a.push(e + 1);
  return a;
}, []);
```

--
count: false

versão genérica e aplicação

```javascript
const map = function (v, f) {
  return v.reduce((a, e) => {
    a.push(f(e));
    return a;
  }, []);
}

map(v, e => e + 1);
```

---
# filter via reduce

filtrar elementos $e$ de v tais que $e < 10$

```javascript
v.reduce((a, e) => {
  if (e < 10) {
    a.push(e);
  }
  return a;
}, []);
```

--
count: false

versão genérica e aplicação

```javascript
const filter = function (v, p) {
  return v.reduce((a, e) => {
    if (p(e)) {
        a.push(e);
    }
    return a;
  }, []);
};

filter(v, e => e < 5);
```

---
# foreach via reduce

imprimir dobro de cada elemento $e$ de v

```javascript
v.reduce((a, e) => {
  console.log(`n = ${e}, 2n = ${2 * e}`);
})
```

--
count: false

versão genérica e aplicação

```javascript
const foreach = function (v, f) {
  v.reduce((a, e) => {
    f(e);
  });
}

foreach(v, e => {console.log(`n = ${e}, 2n = ${2 * e}`)});
```

---
# combinando map, filter e reduce

- na prática, frequentemente precisamos manipular arrays

- pra isso, usamos laços explícitos

   - que envolvem _mutação_ (mudança de valores de variáveis)
   - tipicamente acumuladores, contadores e/ou índices

- map, filter e reduce oferecem uma alternativa

   - há quem defenda que todos devem ser escritos assim
   - com esse estilo, evita-se a mutação de dados
   - e obtém-se código mais declarativo
      - expressa mais “_o quê_” mais que “_o como_” deve ser feito

---
# exemplo 1

“_escreva um laço que produza a soma das potências de 2 dos
  elementos de um array `vetor` que sejam menores que 5_”

--
count: false
 ```javascript
const vetor = [4, 5, 10, 11, 7, 1, 32, 17, 9, 2, 12];

let valor = 0;
for (i = 0; i < vetor.length; i++) {
  if (vetor[i] < 5) {
    valor += Math.pow(2, vetor[i]);
  }
}
```

- como produzir o mesmo efeito com map, filter e reduce?

---
# exemplo 1: com map, filter e reduce

“_escreva um laço que produza a soma das potências de 2 dos
elementos de um array `vetor` que sejam menores que 5_”
```javascript
const vetor = [4, 5, 10, 11, 7, 1, 32, 17, 9, 2, 12];

const valor = vetor.filter(e => e < 5)
                   .map(e => Math.pow(2, e))
                   .reduce((a, e) => e + a);
```

--
count: false

observe os seguintes aspectos:

- o código é _declarativo_, quase a própria especificação
- é explícito nas etapas de **transformação de dados**, mas...
   - não requer variáveis temporárias (ou de apoio)
   - não faz mutação de dados
   - e **não usa laços**
- essas características dão excelente legibilidade ao código
   - a depender apenas do entendimento de map, filter e reduce

---
# compare as soluções lado a lado

```javascript
const valor = vetor.filter(e => e < 5)
                   .map(e => Math.pow(2, e))
                   .reduce((a, e) => e + a);
```

```javascript
let valor = 0;
for (i = 0; i < vetor.length; i++) {
  if (vetor[i] < 5) {
    valor += Math.pow(2, vetor[i]);
  }
}
```

- qual código expressa melhor a especificação?
- qual evidencia com mais clareza a composição dos padrões?
   - observe que ambos combinam os três elementos da spec 
- qual parece mais sujeito à introdução de bugs?


---
# compare as soluções lado a lado

```javascript
const valor = vetor.filter(e => e < 5)
                   .map(e => Math.pow(2, e))
                   .reduce((a, e) => e + a);
```

```javascript
let valor = 0;
for (e of vetor) {
  if (e < 5) {
    valor += Math.pow(2, e);
  }
}
```

- qual código expressa melhor a especificação?
- qual evidencia com mais clareza a composição dos padrões?
   - observe que ambos combinam os três elementos da spec 
- qual parece mais sujeito à introdução de bugs?
- se você insiste em usar o for, ao menos prefira o `for..of`


---
# map, filter e reduce: terceirize laços

observe que

- estas 3 funções permitem **eliminar laços** de seu código

- os laços são “terceirizados” para map, filter e reduce

- força o programador a encaixar seus laços nos padrões...

   - ... ou a produzi-los como uma combinação deles

- que o _pattern_ estimula a pensar em um pipeline de dados

   - ao invés de escrevermos:

---
# sobre a importância da sintaxe/notação

a notação matemática de composição de funções inverte a ordem de aplicação

```javascript
estagio1 = filter(vetor, e => e < 5)
estagio2 = map(estagio1, e => Math.pow(2, e))
v = reduce(estagio2, (a, e) => e + a)
```

```javascript
v = reduce(map(filter(vetor, e => e < 5), e => Math.pow(2, e)), (a, e) => e + a)
```
--
count: false

a notação OO põe o valor à frente, o que facilita a leitura/escrita
```javascript
estagio1 = vetor.filter(e => e < 5)
estagio2 = estagio1.map(e => Math.pow(2, e))
v = estagio2.reduce((a, e) => e + a);
```

```javascript
v = vetor.filter(e => e < 5).map(e => Math.pow(2, e)).reduce((a, e) => e + a);
```

---
count: false
# sobre a importância da sintaxe/notação

a notação matemática de composição de funções inverte a ordem de aplicação

```javascript
estagio1 = filter(vetor, e => e < 5)
estagio2 = map(estagio1, e => Math.pow(2, e))
v = reduce(estagio2, (a, e) => e + a)
```

```javascript
v = reduce(map(filter(vetor, e => e < 5), e => Math.pow(2, e)), (a, e) => e + a)
```

a notação OO põe o valor à frente, o que facilita a leitura/escrita
```javascript
estagio1 = vetor.filter(e => e < 5)
estagio2 = estagio1.map(e => Math.pow(2, e))
v = estagio2.reduce((a, e) => e + a);
```

```javascript
v = vetor.filter(e => e < 5)
         .map(e => Math.pow(2, e))
         .reduce((a, e) => e + a);
```

---
# lições importantes

- map, filter e reduce oferecem alternativas funcionais às
  construções imperativas de repetição

- o foreach é uma variação inspirada em map, filter e reduce; ao
  contrário das três, contudo, seu propósito é imperativo
  (permitir fazer algo com cada elemento), apesar de em estilo
  funcional;

- as três funções são simbólicas de funções de alta ordem; as
  três recebem funções como parâmetros; nenhuma, contudo, retorna
  funções que é outra possibilidade de funções de alta ordem;

- o código resultante do uso das três funções é tipicamente
  bastante legível; em geral, adota-se um estilo de composição
  que lembra _pipelines_ ou _chains_, em que o foco é aplicar
  sequências de transformações aos dados, com base nas três
  funções; 
