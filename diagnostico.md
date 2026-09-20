## A mistura de elementos de variações diferentes

Em `AcessoDados.conectar` o método conectar compila e funciona, mas não garante coerência. Em vez de usar if/else espalhado, a melhor maneira é usar uma fabrica; em vez de chamar o `new comandoMySQL()` direto, chama-se a fábrica primeiro, eliminando o problema ao rodar.

## Construtor telescópico

Também em `AcessoDados` no método `montarConsulta`, quando vai chamar os parametros apenas colocando um no lado do outro com vírgula, não fica claro quando chega nos parametros inteiros, sua identificação. Como `limite`, `offset` e `timeoutSegundos` são inteiros, é fácil trocar a ordem sem que o compilador perceba já que é tudo int. A solução será colocar um builder, que quando chamar na main coloca a identificação e o parametro entre parênteses no lado.