class: center, middle
# Brevíssimo histórico de JavaScript
### ©2017 Dalton Serey, Programação 3, UFCG
.center[![Right-aligned image](https://i.stack.imgur.com/Mmww2.png)]
---
# JavaScript foi criada...

- em 1995, pela Netscape

- em plena web 1.0

- em meio à primeira guerra dos browsers

- no ano em que a Sun Microsystems lançou Java e Applets

- no ano em que a Microsoft lançou o Internet Explorer

---
# o projeto inicial era scheme no browser

- deveria atender prioritariamente a web designers

   - sem deixar de atender programadores

- também deveria ser interpretada e muito leve

- Brendan Eich foi chamado a implementar Scheme no browser

   - inspirado nos vários lisps em aplicações

--
count: false

<br>
contudo, devido à pressão de marketing...

  - os executivos exigiram: *“tem que parecer com Java”*

---
# o resultado foi JavaScript
por fora,

   - léxico e sintaxe inspiradas em **Java**

<br>
por dentro, 

   - funções inspiradas em **Scheme** e **Lisp**

   - um modelo de objetos inspirado em **Self**

<br>
_buzzwords_:

   - alto nível, multi-paradigma, interpretada, dinâmica,
com tipagem dinâmica (*untyped*, pros teóricos) e baseada
em objetos

---
# histórico de versões
1995: Netscape lança Mocha/LiveScript/JavaScript

1996: MicroSoft lança JScript no IE 3.0

1997: A Ecma cria ECMAScript

1998: ECMAScript 2

1999: **ECMAScript 3** 👍

2007: <span style="text-decoration: line-through;">ECMAScript 4 e ECMAScript 3.1</span> 👎

2009: **ECMAScript 5**

2015: **ECMAScript 6** ([ES6 ou ES2015](https://www.ecma-international.org/ecma-262/6.0/))

<br>
até este momento, [nenhuma implementação de ES6 está 100%](https://kangax.github.io/compat-table/es6/).
---
# lições

1. O nome e a aparência (parte da sintaxe) de JavaScript enganam
programadores vindos de Java, C, Python, etc. JavaScript é
**muito** mais fortemente relacionada a <span style="color:
green">**Scheme**</span>, <span style="color:
blue">**Lisp**</span> e a <span style="color:
orange">**Self**</span> do que a Java.

2. JavaScript é <span style="color:red">**multi-paradigma**</span>. Ela foi projetada para acomodar
diferentes estilos e paradigmas de programação. Isso permite que
desenvolvedores reproduzam e se limitem ao estilo que trazem de
outras linguagens.

    - é impensável que alguém que tenha escrito um sistema de
      tamanho significativo em C, C++ ou Java não compreenda a
      linguagem em detalhes; contudo, para JavaScript isso é
      perfeitamente possível

      - é exatamente porque se pode usar JS sem entendê-la que
        não há estímulos e não se encontra quem a entenda bem;

3. JavaScript acumula erros de _design_ que lhe dão má fama entre
os que não estudam bem a linguagem. JavaScript, contudo, reúne um
conjunto de conceitos que a fazem uma **excelente linguagem**.

---
# leituras indicadas

- https://auth0.com/blog/a-brief-history-of-javascript/
- http://www.crockford.com/javascript/javascript.html
