class: center, middle

### `Algorithms + Data Structures = Programs`

Niklaus Wirth, 1976
---
class: center, middle
# Tipos de Dados JavaScript
### ©2017 Dalton Serey, Programação 3, UFCG
.center[![Right-aligned image](https://i.stack.imgur.com/Mmww2.png)]

---
# originalmente para **não programadores**

- em Python, Java, etc, operações sobre tipos diferentes ⇒ Erro

- em JS a semântica tenta capturar “intenção”

- antes de produzir Erro, JavaScript tenta fazer **coerção**

   - uma das maiores fontes de insatisfação com JS


---
# valores têm tipo, variáveis não

- tipo: conjunto de valores + operações suportadas

- todo valor armazenado no _heap_ pertence a um tipo

- variáveis, contudo, não têm tipos

   - ⇒ não há verificação estática de tipos nas operações

   - e verificação

- valores têm tipo, mas mesmo assim há pouca verificação dinâmica

   - poucas operações geram erros de _runtime_
   - muitas produzem _valores especiais_ e falham silenciosamente

- nada impede mudar o tipo dos valores ligados às variáveis 

- muita **coerção** é usada para ajustar operandos às operações

   - coerção `===` conversão implícita de tipo 🤔

---
# dois tipos de tipos: primitivos e objetos

<img src="./js_data_types.jpg" style="width:100%;">
<small>ES6 inclui ainda _Symbol_ (tipo primitivo)</small>
???
**primitivos**: imutáveis, equivalentes Object, autoboxing

**objetos**: base pra construção de estruturas de dados

---
# number

- **único tipo numérico** (ponto flutuante, 64 bits, IEEE 754)

- literais convencionais

   - decimais: `1`, `250`, `123.45`, `2.05`
   - notação científica: `1.23e-21`, `1.2e+55`
   - binários: `0b10011`, `0b10`
   - octais: `015`, `026` ou `0o15`, `0o26`
   - hexadecimais: `0xff`, `0x15a4`

- valores especiais

   - `NaN` (“**N**ot **a** **N**umber”)
   - `+Infinity`, `-Infinity`
   - `-0`
   - `Number.EPSILON` (igual a 2<sup>-52</sup>; novo em ES6)

- via autoboxing, têm acesso à API de Number

---
# string

- literais com aspas simples e duplas: `'ok'` e `"ok"`

- literais com _backticks_ (&#96;...&#96;) para _multi-line_ e _template
  literals_

  ```javascript
  const texto = `primeira linha
  segunda linha
  terceira linha`;

  peso = 40.5;
  idade = 11;
  msg = `Idade = ${idade}, Peso = ${peso}`;
  ```

- caracteres de escape convencionais

  ```code
             \'         \"         \\         \n         \r         \t
  ```

- acesso à API de String (via autoboxing)

   - [JavaScript String Reference](https://www.w3schools.com/jsref/jsref_obj_string.asp)
   - [JavaScript String Methods](https://www.w3schools.com/js/js_string_methods.asp)
---
# boolean

- valores `true` e `false`

- outros valores são promovidos por coerção a valores `truthy`

- falsy e truthy values

   - falsy: null, undefined, false, +0, -0, NaN e ""

   - truthy: qualquer coisa que não seja falsy

- operadores `||` e `&&` (estilo python, não Java ou C++ ou C)

   - melhor compreendidos como *seletores*

---
# null e undefined

- não valores: null e undefined

- cada um corresponde a um tipo e seu único valor

- felizmente, null é palavra reservada

- infelizmente, undefined NÃO é palavra reservada, é um
  identificador apenas!!!

- undefined não é undeclared!

- referenciar variável inexistente/não declarada é Erro! `ReferenceError` 

   - problema aqui: a mensagem de JS é `x is not defined` e dá a
     impressão de que é o mesmo que dizer que x é undefined...
     mas não é! o ideal seria que fosse `x is not declared` ou
     algo do tipo... menos confusão => melhor compreensão

   - quer ver piorar? use `typeof x` no caso acima; o resultado
     será `undefined`, mesmo que x esteja não declarada!!! isso
     só aumenta a confusão; veja que se você declarar uma
     variável e não a atribuir, aí sim, o resultado é undefined
     corretamente; :-(

   - contudo, `typeof x` funcionar mesmo com `x` não declarado é 
     desejável e útil; principalmente, em um ambiente em que
     vários scripts compartilham um escopo global; isso muda um
     pouco de figura com ES6... e módulos;

- NaN

   - NaN é uma realidade na programação JS

   - NaN é um number !?

   - como é o resultado de algumas operações, é comum precisar
     testar se o resultado é NaN...

     - infelizmente, `r == NaN` ou mesmo `r === NaN` não
       funcionam!!!

     - porque na verdade, `NaN == NaN` e `NaN === NaN` são ambos
       falsos!!! O NaN é o **único valor** em JavaScript que não é
       igual a si próprio!

     - então, como testar se o resultado é NaN? que tal a função
       global `isNaN()`? Não funciona... tem um bug histórico!
       Ela foi feita por alguém que levou ao pé da letra o nome
       da função e do acrônimo NaN e verifica apenas se o
       argumento passado não é um número válido... logo, retorna
       `true` mesmo quando o valor não é NaN, mas o valor não é
       um número; assim, isNaN("teste") é true, mesmo sem que a
       string seja um NaN...

     - e o que fazer? felizmente, em ES6 foi feita uma versão
       correta da função e colocada em `Number.isNaN()`; essa é
       confiável; você também pode fazer a sua (um polyfill)
       baseado em `typeof n == "number" && window.isNaN(n)` ou
       aproveitar a peculiaridade exclusiva do valor NaN e
       escrever `return n !== n`.

     - aqueles de vocês que já programam e usaram isNaN em algum
       lugar de seus projetos, corram pra corrigir esses bugs...
       ainda que não tenham se manifestado, são bugs!

# zeros

- `+0` e `-0`


---
# o operador `typeof`

- o operador faz inspeção do tipo do valor

- retorna o nome (string) de um dos tipos existentes:

   - `number`
   - `string`
   - `booolean`
   - `undefined`
   - `symbol`
   - `object`
   - `function` (?!)
   - mas não `null`... por bug, `typeof null === 'object'` :-D

- ao ser aplicado a uma variável, o valor é que é inspecionado

colocar só depois de passarmos por Objects
---
# natives
---
# conversões de tipos: _casting_ e _coercion_

- casting não é mencionado na spec, mas existe

- coerção é uma _feature_, não um _bug_

- coerção é conversão implícita de tipos

   - quando é explícita é chamada de casting

   - em LPs estáticas é tipicamente feita em tempo de compilação

- em JS, há coerção em todo lugar!

   - expressões condicionais de modo geral
   - condições no if, no for, no while e no do..while
   - indexação de arrays
   - concatenação
   - expressões ternárias

- difícil e polêmica, mas vetor de concisão, elegância e legibilidade

   - a coerção tenta capturar a flexibilidade da linguagem humana

- coerção é a criação de um novo valor, de um outro tipo, para
  substituir outro, para permitir certa operação

- 3 + '4'

- 3 * '4'

- 3 * '4' + '2'

- 3 + '4' * '2'

- (3 + '4') * '2'

- problemas

  ```
  > a = "100"
  > b = "20"
  > a / b

  ```
---
# curiosidades

NaN é o único valor que não é igual a ele mesmo.

```
NaN !== NaN
typeof NaN === 'number'
```

Logo, você não pode testar se um resultado é NaN com o operador
de igualdade. Pra isso, use `isNaN()`

```
isNaN(0/0) === true
```

Curiosamente, `isNaN("abc") === true`. Isso ocorre, porque
JavaScript faz coerção ao fazer o teste. Felizmente, em ES6 foi
adicionada uma nova função de teste a `Number`. Assim,
`Number.isNaN("abc") === false`.

---
# leituras

- https://developer.mozilla.org/en-US/docs/Web/JavaScript/Data_structures
