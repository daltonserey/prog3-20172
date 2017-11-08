class: center, middle
# Funções Puras
### ©2017 Dalton Serey, Programação 3, UFCG
.center[![Right-aligned image](https://i.stack.imgur.com/Mmww2.png)]
---
# função em matemática

- mapeamento de elementos de dois conjuntos

   - _domínio_: conjunto de “partida”
   - _contradomínio_: conjunto de “chegada”
   - _imagem_ é o subconjunto do contradomínio que é parte da relação

- um elemento no domínio ↦ um único elemento no contradomínio

???
- _parcial_ pq pode não ser definida pra todo elemento do domínio

   - é dita _total_ se for definida pra todo elemento do domínio
   - _domínio de definição_: subconjunto do domínio em que está definida

---
# notação

<br>
.center[<span style="font-size: 200%">$f : D → C$</span>]

<br>
.center[<span style="font-size: 200%">$d ↦ c$</span>]

<br>
.center[<span style="font-size: 200%">$f(d)$
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
$f(d)\downarrow$&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;$f(d)\uparrow$</span>]

---
class: center, middle
# _funções matemáticas_

# X

# _funções em computação_
---
# funções matemáticas

- são especificadas em linguagem matemática

   - linguagem de natureza _declarativa_ (expressam _o quê_)
   - foco da especificação: propriedades da _relação_ entre $x$ e $f(x)$
   - métodos efetivos de cálculo em segundo plano

???
A especificação permite testar se um par _x, y_ pertence à função.
--
count: false
- são, por definição, _puras_

   - cada $d \in D$ é relacionado a um único valor em $C$
   - não têm _nenhum_ outro propósito/efeito

--
count: false
- _podem ser compostas_ (são _composable_) com segurança

   - considere $f : A → B$ &nbsp;e&nbsp; $g : B → C$
   - então podemos definir $h : A → C$, com $a \mapsto c$
      - onde $c = g(b)$, tal que $b = f(a)$, ou simplesmente, $c = g(f(a))$

---
# exemplos
<div style="font-size: 140%;">
    <script type="math/tex; mode=display">
        \begin{aligned}
        f(x) = (x - 1)(x - 2) && g(x) = 3x^2 + \sin{x} 
        \end{aligned}
    </script>
</div>

--
count: false
<div style="font-size: 140%;">
    <script type="math/tex; mode=display">
        h(x) = \begin{aligned}
        &g(x) + 1\over{f(x)} && x \not\in \{1, 2\}
        \end{aligned}
    </script>
</div>

--
count: false
<div style="font-size: 140%;">
    <script type="math/tex; mode=display">
        i(x) = \left\{\begin{aligned}
        &f(x) + 1&&: x \leq 0\\
        &g(x) - 1&&: x > 0
        \end{aligned}
        \right.
    </script>
</div>

---
# funções nem sempre são métodos efetivos

compare as especificações das funções abaixo
<div style="font-size: 130%;">
    <script type="math/tex; mode=display">
        \begin{aligned}
        f(x) = (x - 1)(x - 2) &&\text{tal que } x \in \mathbb{R}\\
        \end{aligned}
    </script>
</div>

.center[e]

<div style="font-size: 130%;">
    <script type="math/tex; mode=display">
        \begin{aligned}
        raiz(x) = y &&\text{tal que } y \ge 0 \text{ e } y^2 = x\\
        \end{aligned}
    </script>
</div>

--
count: false
- a de $f(x)$ diz quais operações fazer para partir de $x$ e chegar em $f(x)$

- a de $raiz(x)$ apenas especifica a função...

   - mas sem dar um _método efetivo_ para calcular $raiz(x)$ a partir de $x$

---
# funções em programação

- são escritas em linguagens de programação

   - de natureza mais _imperativa_ que _declarativa_ (expressam _o como_)
   - foco da especificação: _método efetivo_ de cálculo de $f(x)$ a partir de $x$
   - propriedades da relação em segundo plano

--
count: false

- podem ser puras ou não

   - podem ser criadas para computar valores 👍 
   - mas também para produzir _efeitos colaterais_ (ECs) 🤔

--
count: false
- são fáceis de compor, mas há dificuldades...

   - ECs podem conflitar, se propagar e produzir efeitos difíceis de prever
   - caracterização imperativa ⇒ dificulta raciocínio sobre composições

---
# funções em LPs são métodos efetivos

- funções em LPs são **sempre** métodos efetivos

- e esperamos que sejam _algoritmos_ (métodos efetivos finitos)

```javascript
const f = (x) => (x - 1) * (x - 2);
const g = (x) => 3 * x ** 2 + Math.sin(x);
const h = (x) => (g(x) + 1) / f(x);
const i = (x) => x <= 0 ? f(x) + 1 : g(x) - 1;
```

- e $raiz(x)$ pode ser escrita como função JS?

   - claro, mas não apenas a partir da especificação matemática
   - para isso, precisamos de um _método efetivo_

---
# raiz quadrada pelo método de Newton

especificação matemática da função $raiz(x)$
<script type="math/tex; mode=display">
   \begin{aligned}
     raiz(x) = y &&\text{tal que } y \ge 0 \text{ e } y^2 = x\\
   \end{aligned}
</script>

<br>
especificação JavaScript da função $raiz(x)$

```javascript
const epsilon = 0.001;

var raiz = function (x) {
    const media = (a, b) => (a + b) / 2;
    const proximo = (v) => media(v, x / v);
    const eh_aceitavel = (v) => Math.abs(v * v - x) / x < epsilon;
    const itere = (v) => eh_aceitavel(v) ? v : itere(proximo(v));
    return itere(1.0);
};
```

---
# funções puras (em computação)

1. um valor de saída para o cada conjunto de valores de entrada

   - a saída é determinada **exclusivamente** pelos valores de entrada
   - a mesma saída é produzida, se aplicada às mesmas entradas

2. não têm efeitos colaterais

   - nenhum dado fora da função sofre alteração
   - não faz ou depende de entrada e saída (I/O), incluindo...
      - arquivos em meios externos
      - dispositivos de i/o: teclado, tela, etc
      - conexões de rede ou de bancos de dados

???
   - logo, mapeiam _funcionalmente_, um domínio a um contra-domínio
   - obs: _exceptions_ também podem ser consideradas efeitos colaterais

### transparência referencial
   - significado: o valor a que a expressão de invocação se refere ou a
     expressão em si podem ser usadas de forma intercambiável

  ```javascript const a = Math.sqrt(25) + 10;
  ```
  é idêntica em termos de significado a
  ```javascript
  const a = 25.0 + 10;
  ```
   - é a base para avaliação por substituição

---
# funções puras: benefícios
- são melhores para raciocinar ⇒ reduzem chances de _bugs_

- não dependem de estado ⇒ são mais fáceis de compor

- são “idempotentes”: podem ser invocadas múltiplas vezes
   - não têm efeitos colaterais, nem cumulativos
   - e, portanto, mais fáceis de testar e de manter

- funções puras permitem
   - otimização por _memoization_ (≈ _caching_)
   - avaliação preguiçosa (_lazy_)
   - execução em paralelo, com réplicas, livres de
      - _race conditions_
      - _starvation_
      - e _deadlocks_

---
# lições importantes

1. funções puras têm diversas vantagens sobre funções impuras;
entre elas: são mais fáceis de compreender, de testar e demanter;
além disso, são mais apropriadas para paralelizar e otimizar;

2. sempre que possível, prefira escrever funções puras; no
mínimo, projete suas funções tendo clareza sobre se são ou não
são puras;

3. procure isolar os aspectos com efeitos colaterais de seus
programas em funções específicas para isso; lembre-se que tais
funções são mais difíceis de testar, depurar, etc;

---
# leituras indicadas

- [Funções parciais](https://en.wikipedia.org/wiki/Partial_function)
- [Métodos efetivos](https://en.wikipedia.org/wiki/Effective_method)
- [Indução matemática](https://en.wikipedia.org/wiki/Mathematical_induction)
- [Recursão](https://en.wikipedia.org/wiki/Recursion)
- [Funções puras](https://en.wikipedia.org/wiki/Pure_function)
