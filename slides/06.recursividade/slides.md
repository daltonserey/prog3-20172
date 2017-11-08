class: center, middle
# Recursividade
### ©2017 Dalton Serey, Programação 3, UFCG
.center[![Right-aligned image](https://i.stack.imgur.com/Mmww2.png)]
---
# recursividade

- consiste em especificar um conceito com base nele próprio

- é a forma funcional ou matemática de **repetição**

- é intrinsecamente relacionada à _indução matemática_

   - importante ferramenta de raciocínio matemático
   - recursividade ⇒ suporte a raciocínio indutivo

.right[<img src="drawing_hands.jpg" width="40%"><img>]
---
# funções recursivas

- ideia: especificar funções usando recursividade

- consiste em especificar uma função em termos dela própria

   - as aplicações recursivas são sempre em casos “_menores_”

- como toda forma de “repetição”, requer _critérios de parada_

   - os critérios de parada são conhecidos por _casos base_
   - são eles que evitam aplicações infinitas da função

---
# fibonacci

<div style="font-size: 120%;">
<script type="math/tex; mode=display">
    \textit{fatorial}(n) = \left\{\begin{aligned}
        &1&&: n = 0\\
        &n \times \textit{fatorial}(n - 1)&&: n > 0
    \end{aligned}
    \right.
</script>
</div>

---
# gcd

<div style="font-size: 120%;">
<script type="math/tex; mode=display">
    gcd(a, b) = \left\{\begin{aligned}
        &a&&: b = 0\\
        &gcd(b, a \text{ mod } b)&&: b > 0
    \end{aligned}
    \right.
</script>
</div>

---
# fibonacci

<div style="font-size: 120%;">
<script type="math/tex; mode=display">
    \textit{fib}(n) = \left\{\begin{aligned}
        &0&&: n = 0\\
        &1&&: n = 1\\
        &\textit{fib}(n - 1) + \textit{fib}(n - 2)&&: n > 1
    \end{aligned}
    \right.
</script>
</div>

---
# soma de naturais

<div style="font-size: 120%;">
<script type="math/tex; mode=display">
    a + b = \left\{\begin{aligned}
        &a&&: b = 0\\
        &\textit{inc}(a) + \textit{dec}(b)&&: b > 0
    \end{aligned}
    \right.
</script>
</div>

---
# recursividade em LPs

- todas as LPs modernas suportam recursividade

   - introduzidas por LISP em 1958

- requer uma boa implementação de invocação de funções

   - no mínimo, uma pilha de chamadas (_call stack_)
   - idealmente, tratamento especial para _tail calls_

- permitem que o código reflita especificações recursivas

  ```javascript
  function fatorial(n) {
        return n === 0 ? 1 : n * factorial(n - 1);
  }
  ```

---
# exercício: max

Escreva uma especificação **recursiva** de `max` de forma que:
```javascript
max() // undefined
max(2) // 2
max(2, 3) // 3
max(4, 1, 5, 3, 2) // 5
```

Escreva uma especificação **recursiva** de `search` de forma que:
```javascript
const v = [9, 7, 3, 11, 8, 6, 5, 8, 10, 4];
console.log(search(v, 9)); // 0
console.log(search(v, 8)); // 4
console.log(search(v, 4)); // 9
console.log(search(v, 20)); // -1
```
---
# exemplo: max

```javascript
function max(e0, e1, ...resto) {
  if (e0 === undefined) return undefined;
  if (e1 === undefined) return e0;

  return max( (e0 > e1 ? e0 : e1), ...resto );
};
```


---
# exemplo: search

```javascript
function search(vetor, valor) {
  if (vetor.length === 0) return -1;
  if (vetor[0] === valor) return 0;

  const i = search(vetor.slice(1), valor);
  return i < 0 ? -1 : i + 1;
}
```

---
# exemplo: soma ate primeiro maior

```javascript
function soma_ate_primeiro_maior(array, n) {
  if (array.length === 0) return 0;
  if (array[0] > n) return 0;
  
  return array[0] + soma_ate_primeiro_maior(array.slice(1), n);
}
```

---
# recursividade X eficiência

- invocar funções implica em usar a pilha
- para cada invocação ⇒ 
   - é criado um _stack frame_ e
   - são copiados os dados passados como argumentos

- recursividade ⇒ 
   - invocar _repetidamente_ a mesma função
   - com enorme potencial para ineficiência ☹ 
 
- como limitar os problemas? _tail calls_

---
# tail calls

- uma invocação de função é uma _tail call_ se é a última ação da função
- uma _tail call recursion_ é uma invocação recursiva que é uma _tail call_

   - muitas vezes são chamadas apenas de _tail calls_
   - outras vezes apenas de _tail recursion_
   - e, em português, de _recursão de cauda_

--
count: false
- quando ocorre, o _stack frame_ da função invocadora perde sua utilidade
   - se a invocação é a última operação, não tem mais utilidade
   - e o valor a retornar, será o mesmo da função invocada
   - ⇒ o frame pode ser descartado 👍 

- e, portanto, permitem economizar memória
---
# exemplos

  ```javascript
  function f(a, b) {
      const c = a - b;
      return g(a * c, b * c);    
  }

  function g(x, y) {
      const z = x ** 2;
      const w = y * x;
      return z + w;
  }

  const a = exemplo1(2, 3);
  ```


  ```javascript
  function exemplo() {
      ...
      return exemplo();    
  }

  const a = exemplo();
  ```
---
# tail call

- código recursivo, mas processo iterativo

---
# exemplo fibonacci

- código recursivo direto da spec
 
- código recursivo/iterativo, três variáveis de estado
