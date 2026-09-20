# SIGA — Atividade de Padrões Criacionais (código final)

**Técnicas de Programação II (TP2) · Aula 6** — CST em Desenvolvimento de Software Multiplataforma · Fatec de Porto Ferreira

Este é o **código final** da atividade prática da Aula 6. Ele continha, de forma **proposital**, três problemas de design que foram resolvidos aplicando, em conjunto, os padrões **Abstract Factory**, **Builder** e **Singleton**. O programa compilava e executava — o problema não era o funcionamento, e sim a coerência, a legibilidade e o controle da criação de objetos.

## Estrutura do projeto

```
siga-criacionais/
└── src/
    └── siga/
        ├── Conexao.java              (interface — produto; pronta)
        ├── Comando.java              (interface — produto; pronta)
        ├── ObjetosAcessoDados.java   (implementações MySQL e PostgreSQL; prontas)
        ├── FabricaBanco.java         (interface — Abstract Factory)
        ├── FabricaMySQL.java         (fábrica concreta — fornecedor MySQL)
        ├── FabricaPostgreSQL.java    (fábrica concreta — fornecedor PostgreSQL)
        ├── ConsultaBuilder.java      (Builder da consulta, substitui o método telescópico)
        ├── AcessoDados.java          (Singleton; ponto único de acesso ao banco)
        └── Main.java                 (demonstra a solução em execução)
```

## Como compilar e executar

Pré-requisito: JDK 17 ou superior (`java -version` para verificar).

```bash
# 1. Compilar (a saída vai para a pasta "bin")
javac -d bin src/siga/*.java

# 2. Executar
java -cp bin siga.Main
```

## Os problemas propositais

| Local | Problema | Padrão que resolve |
|---|---|---|
| `AcessoDados.conectar` | Cria conexão e comando por `if` e `new` separados, sem garantir que sejam do mesmo fornecedor (dá para misturar MySQL e PostgreSQL). | **Abstract Factory** |
| `AcessoDados.montarConsulta` | Método com muitos parâmetros opcionais (construtor telescópico), ilegível e propenso a erro de ordem. | **Builder** |
| `AcessoDados` | Nada garante um único ponto de acesso ao banco no sistema. | **Singleton** |

## Como cada problema foi resolvido

Pra mistura de fornecedores, criei a interface `FabricaBanco` e as fábricas `FabricaMySQL` e `FabricaPostgreSQL`, cada uma só conhecendo as classes do seu próprio fornecedor. O `conectar` agora recebe uma fábrica pronta em vez de decidir com `if/else`.

Pro construtor telescópico, criei o `ConsultaBuilder`, com um método nomeado pra cada parâmetro opcional (`comFiltro`, `comLimite` etc.), encadeáveis, e um `construir()` no final. O método antigo ficou comentado em `AcessoDados.java`.

Pra instância não controlada, o `AcessoDados` virou Singleton: construtor privado, atributo estático e o `obterInstancia()` como único jeito de conseguir o objeto.

Diagrama de classes da solução: [`diagrama-uml.md`](diagrama-uml.md).

## Tarefas realizadas

Siga as etapas da ficha de atividade prática:

1. **Analisar** o código inicial e identificar a possibilidade de misturar fornecedores e o método de consulta telescópico.
2. **Abstract Factory:** criar uma fábrica abstrata (por exemplo, `FabricaBanco`) com `FabricaMySQL` e `FabricaPostgreSQL`, cada uma produzindo uma `Conexao` e um `Comando` **do mesmo fornecedor**. O `AcessoDados` passa a receber uma fábrica e criar a família coerente a partir dela.
3. **Builder:** criar um `ConsultaBuilder` com métodos nomeados e encadeáveis para os parâmetros opcionais (`comFiltro`, `comOrdenacao`, `comLimite`, etc.) e um `construir()` que devolve a consulta. Substitui o método telescópico.
4. **Singleton:** transformar o `AcessoDados` em um Singleton, com construtor privado, instância estática e método de acesso.
5. **Desenhar** o diagrama de classes da solução (fábrica de banco, produtos, builder e acesso), evidenciando os três padrões.

## Critério de sucesso

Ao final: (a) é **impossível** combinar uma conexão de um fornecedor com um comando de outro; (b) a montagem da consulta está **legível**, com passos nomeados; e (c) existe **um único** ponto de acesso ao banco, obtido de forma controlada.

## Padrão de entrega

Conforme a ficha de atividade prática: identificadores em português, um arquivo `.java` por classe pública, código formatado, entrega no repositório Git com README e commits descritivos. O uso de IA para gerar o código é proibido nesta atividade (ver seção 5.3 da ficha).
