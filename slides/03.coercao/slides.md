class: center, middle
# Coerção em JavaScript
### ©2017 Dalton Serey, Programação 3, UFCG
.center[![Right-aligned image](https://i.stack.imgur.com/Mmww2.png)]

---
# conversões de tipo

- situação: os operandos são de tipos “_não esperados_” pela operação

   - você leu um valor do usuário, da rede, do DB
   - outro valor foi calculado
   - ou o valor é de um subtipo específico

--
count: false
- solução: conversão de tipo

   - explicitamente ⇒ _casting_
   - implicitamente ⇒ _coercing_

---
# implícito vs explícito

- explícito = declarado, sem reservas

   - usando as funções nativas `Number`, `String`, `Boolean`

   - cuidado: sem o operador `new`!

   - com o `new` não é coerção é instanciação de um objeto

- implícito = não declarado, subentendido

--
count: false
<br>
Parte do preconceito com coerção vem de entender mal isso...

- _implícito_ ≠ obscuro, imprevisível, inesperado

- atenção: muitos textos dizem exatamente isso

- coerção é totalmente previsível em JS!

---
# quase toda linguagem faz coerção

- potências de 2 em Java: https://repl.it/My0w
- potências de 2 em Python: https://repl.it/My0v
--
count: false
- e em JavaScript? https://repl.it/My2Q

---
# coerção é _feature_ em JavaScript

- é algo _intencionalmente_ projetado na linguagem

   - sem dúvida tem problemas, contudo...

   - também traz benefícios

--
count: false
- há duas posturas possíveis

   - não entendê-la, evitá-la e reclamar a cada surpresa

   - aceitá-la como parte da LP, aprendê-la e usá-la

- só lembre: coerção é usada em várias partes de JS

   - expressões lógicas (if, while)

   - indexação

---
# coerção em JavaScript

- ocorre sempre que os operandos não se ajustam à operação

- pode mudar o tipo de um ou dos dois operandos

- *sempre* resulta em um tipo primitivo simples

   - `object`s nunca são produzidos

   - valores _boxed_ (Number, String e Boolean) nunca são produzidos

   - `null` ou `undefined` nunca são produzidos

   - só `number`, `string` ou `boolean`

---
# o tipo resultante depende do operador

- `*` prefere números

- `+` prefere strings que números 😠

- `==` prefere números que qualquer outra coisa

- e `===` não faz coerção (e pode ser a solução)

---
# leituras indicadas

- [Abstract Equality Comparison (ES6 spec)](https://www.ecma-international.org/ecma-262/6.0/#sec-abstract-equality-comparison)
- [The Addition Operator (ES6 spec)](https://www.ecma-international.org/ecma-262/6.0/#sec-addition-operator-plus)
- [Equality comparisons and sameness](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Equality_comparisons_and_sameness)
- Capítulo 3 do You Don't Know JavaScript, Types and Grammar
- [Fixing Coercion](https://davidwalsh.name/fixing-coercion)
