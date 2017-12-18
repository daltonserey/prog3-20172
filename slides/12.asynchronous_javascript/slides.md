class: center, middle
# Programação Assíncrona
### ©2017 Dalton Serey, Programação 3, UFCG
.center[![Right-aligned image](https://i.stack.imgur.com/Mmww2.png)]

---
# conceitos a cobrir

- modelo de concorrência
   - _request-response_ X _event-driven_
   - _non blocking i/o_
   - _callbacks_

- modelo de execução de JavaScript
   - engine X runtime
   - _call stack_ X _call queue_ (ou _message queue_)
   - _event loop_

---
# exercício 1

Escreva código JavaScript que crie um contador simples em uma
página html. O contador deve atender aos seguintes requisitos:

- inicialmente, o contador deve estar “parado”;
- deve se iniciar quando o botão `start` for pressionado;
- a contagem deve ser atualizada de 1 em 1s;
- deve parar quando o botão `stop` for pressionado;
- o botão `stop` deve ser desabilitado.ref[✱] quando a contagem estiver parada; 
- os botões `start` e `reset` devem ser desabilitados quando a contagem estiver em
  andamento; 
- faça uma versão usando apenas `setTimeout`;
- faça outra usando `setInterval` e `clearInterval`;

Use este [código base](exercicio1) para começar.

.footnote[.ref[✱] dica: use `element.disabled = true` para desabilitar
um elemento do DOM]
---
# exercício 2

Escreva um construtor que encapsule o funcionamento do contador
em um objeto, de forma que vários contadores possam ser criados e
usados em uma mesma página e controlados separadamente. Em
particular, faça com que ao serem criados, possamos definir o
intervalo entre os eventos de contagem. O html fornecido
exemplifica como o contador será usado. 

Use este [código base](exercicio2) para começar.
