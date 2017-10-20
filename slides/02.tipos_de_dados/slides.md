class: center, middle
# Tipos de Dados em JavaScript
### ©2017 Dalton Serey, Programação 3, UFCG
.center[![Right-aligned image](https://i.stack.imgur.com/Mmww2.png)]

---
class: center, middle

# Algorithms + Data Structures = Programs

Niklaus Wirth, 1976
---
# tipos de dados e sistema de tipos

- TD = conjunto de valores + conjunto de operações

   - valores que podem ser substituídos uns por outros


###  sistema de tipos _(type system)_

- conjunto de regras para associar tipos a valores

   - sistema formal para evitar estados indesejáveis em programas

   - anotações de tipo são fórmulas; programas são provas

- quando o programa não atende às restrições do sistema de tipos

   - o programa não compila (verificação estática)

   - o programa dá runtime error (verificação dinâmica)

---
# o TS de JS é para **não programadores**

- em Java, Python, etc

   - operações com operandos de tipos não suportados ⇒ .red[Erro]

- em JS a semântica tenta capturar “intenção”

   - os operandos sofrem _coerção_ pra de adequar à operação

   - além disso, as operações minimizam casos de erro

- coerções em JavaScript são ao mesmo tempo

   - uma poderosa _feature_ que facilita a codificação

   - e uma das maiores causas de frustração

---
# dois “tipos de tipos”: primitivos e objetos

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

   - este é um dos grandes problemas com coerção

- valores _falsy_ e _truthy_ — enorme fonte de problemas 👀

   - _falsy_: `false`, `null`, `undefined`, `+0`, `-0`, `NaN` e `""` 😠

   - _truthy_: qualquer valor que não seja _falsy_

- atentar pra (excelente) semântica de `||` e `&&`

   - ao estilo de python e bash, não de Java, C ou C++ ou C

   - melhor compreendidos como *seletores*

      - `&&` pode ser usado como uma guarda
      - `||` pode ser pensado como seletor de default

---
# null e undefined

- JS tem dois “_não valores_”: `null` e `undefined`

- cada um pertence a seu próprio tipo e é seu único valor

   - detalhe: `null` é palavra reservada, mas `undefined` não!

- não confundir `undefined` com _undeclared_!

- referenciar variável inexistente/não declarada é Erro! `ReferenceError` 

???

O problema aqui é que a mensagem de JS é `x is not defined` e dá
a impressão de que é o mesmo que dizer que x é undefined...  mas
não é! o ideal seria que fosse `x is not declared` ou algo do
tipo... menos confusão => melhor compreensão.

Ainda pode piorar. Use `typeof x` no caso acima; o resultado será
`undefined`, mesmo que x esteja não declarada!!! isso só aumenta
a confusão; veja que se você declarar uma variável e não a
atribuir, aí sim, o resultado é `undefined` corretamente. :-(

Contudo, `typeof x` funcionar mesmo com `x` não declarado é
desejável e útil; principalmente, em um ambiente em que vários
scripts compartilham um escopo global; isso muda um pouco de
figura com ES6... e módulos.

---
# object

- em JS, qualquer valor que não seja um primitivo é um objeto

- objetos são **coleções de propriedades**

- cada propriedade consiste em um _nome_ e um _valor_

   - cada _nome_ é uma _string_

   - e cada _valor_ é qualquer valor, incluindo objetos

- objetos podem ser ligados a outros por **herança prototipal**

   - pode ser melhor entendida como _delegação_

- diferente de outras LPs, JS tem **literais pra objetos**

- todos os objetos têm um único tipo: `object`

---
# exemplo de literal de um objeto simples

```javascript
const jackson = {
   nome: 'josé gomes filho',
   nascimento: {
      dia: 31,
      mes: 7,
      ano: 1919
   }
}
```

---
# objetos nativos (ou _built-in_)

- pra cada tipo primitivo, um objeto associado

   - `number` ⇔  `Number`
   - `string` ⇔  `String`
   - `boolean` ⇔  `Boolean`

- os objetos acima podem ser usados como:

   - funções de casting
   - ou como _construtores_

- dois outros objetos nativos merecem destaque

   - **`Array`**: objeto que “simula” arrays e listas
   - **`Function`**: objeto especial que pode ser “executado”
   - tão importantes que têm léxico, sintaxe e semântica próprias

---
# exemplo de literal de um outro objeto

```javascript
const jackson = {
   nome: 'josé gomes filho',
   nascimento: {
      dia: 31,
      mes: 7,
      ano: 1919
   },
   instrumentos: ['pandeiro', 'bateria', 'voz'],
   prim_nome: function () {
      return this.nome.split(' ')[0];
   }
}
```

Atente também, para os literais de _Array_ e _Function_.

**Quem precisa de classes e de `new` com literais pra
objetos?!**
---
# `Array`

- em aparência, semelhantes a listas em python

   - também lembram ArrayLists em Java
   - internamente, contudo, são bem diferentes

- arrays JS são objetos simples com...

   - sintaxe própria (similar à de python)
   - índices e valores simulados via properties
   - `typeof` resulta em `object`

```javascript
const a = ['a', 'b', 'c'];
console.log(Object.keys(a)); // ["0", "1", "2"]

delete a[1];
console.log(a); // ["a", empty × 1, "c"]

a.length = 2000;
console.log(a); // ["a", empty × 1, "c", empty × 1997]
```

---
# `Function`

