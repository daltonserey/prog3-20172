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
v.reduce((a, e) => a + e, 1000); // 1061
v.reduce((a, e) => a * e); // 61
```
???
- a “rainha” das funções de alta ordem
- calcula um valor inteiramente novo a partir de um array
