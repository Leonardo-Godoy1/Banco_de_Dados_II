# Lista 4 — Concorrência e Controle de Transações

## Questão 1
R: A proposta inicial é para garantir a previsibilidade e determinismo dos testes e dos resultados.

---

## Questão 2
R: Pois para que possamos garantir a consistência dos dados durante o experimento, eles devem já iniciar em estado válido, respeitando a ACID.

---

## Questão 3
R: Ela foi bloqueada até que a Transação 1 não termine.

---

## Questão 4
R: Porque elas estavam em um impasse, uma vez que ambas escrevem no mesmo registro. Para isso ocorre um bloqueio com o objetivo de evitar erros lógicos.

---

## Questão 5
R: Avisar qual registro a operação vai alterar, para que possa tratar nas outras sessões.

---

## Questão 6
R: Elas acessam registros distintos, assim, não ocorre impasses.

---

## Questão 7
R: Mostra que não há problemas em alterar linhas distintas em uma tabela simultaneamente.

---

## Questão 8
R: O objetivo principal é tentar realizar a leitura suja, ou seja, ler os valores incompletos de uma transação.

---

## Questão 9
R: O isolamento evita que ocorra a leitura suja, uma vez que proíbe que outras transações acessem os valores que a mesma está modificando até que ela realize um COMMIT.

---

## Questão 10
R: Depende do SGBD. No caso do MySQL ele permanece, já que nesse tipo de banco ele lê o valor de antes da transação começar, ignorando a segunda transação.

---

## Questão 11
R: Busca identificar o fenômeno da leitura não repetível.

---

## Questão 12
R: Operações concorrentes sobre o mesmo registro requerem maior controle porque, se feitas de forma indiscriminada, podem gerar inconsistências, já que alterações feitas numa transação podem não ser percebidas por outra.

---

## Questão 13
R: A atualização perdida.

---

## Questão 14
R: O congelamento, ou seja, a espera que a Transação 2 tem que fazer.

---

## Questão 15
R: Pois precisa garantir o isolamento e a consistência dos dados da transação inicial (I e C do ACID).

---

## Questão 16
R: Pois ambas dão indício que vão alterar o mesmo registro, assim ocorre um bloqueio para persistir o isolamento das transações.

---

## Questão 17
R: A principal diferença está na intenção de modificação dos valores.

---

## Questão 18
R: O saldo correto seria 700.

---

## Questão 19
R: Como a transação 2 foi feita sem utilizar o saldo gerado pela transação 1, o resultado da transação 1 foi perdido.

---

## Questão 20
R: Inserções em linhas diferentes nem sempre geram conflito direto pois elas estão operando sobre dados diferentes.

---

## Questão 21
R: Isso mostra que a concorrência quando não há disputa pelo mesmo registro não gera problemas de consistência e não precisa de bloqueios.

---

## Questão 22
R: O bloqueio causado por muito tempo em um sistema real pode causar gargalos no sistema, uma vez que pode “segurar” outras operações importantes e afins.

---

## Questão 23
R: Pois existe uma chance maior de ocorrer algum bloqueio ou inconsistência lógica, e isso deve ser evitado ao máximo para que não ocorram travamentos nos sistemas com ambientes concorrentes.

---

## Questão 24
R: Analisando se os dados finais das contas estão de acordo com as movimentações salvas no log de operações.

---

## Questão 25
R: A análise final é importante para verificar se ocorreu alguma inconsistência no banco a fim de corrigi-la.

---

## Questão 26
R: A concorrência é a capacidade do SGBD de permitir vários usuários ou operações simultâneas em um mesmo banco de dados, sendo separado em transações.

---

## Questão 27
R: O bloqueio é responsável por impedir uma transação de acessar um dado que está sendo alterado por outra, a fim de garantir que ela utilizará a versão mais atualizada do dado.

---

## Questão 28
R: Ao acessar registros diferentes em transações simultâneas, não há a necessidade de tratar impasses. Já em registros iguais, deve ocorrer um bloqueio para que não ocorram inconsistências no banco de dados.

---

## Questão 29
R: FOR UPDATE é importante porque ele sinaliza para outras sessões ou usuários qual registro será alterado, a fim de possibilitar o bloqueio e evitar problemas de concorrência.

---

## Questão 30
R: Significa que ela ficou bloqueada, aguardando outra transação terminar de alterar algum dado para que ela possa usar a versão mais atualizada.

---

## Questão 31
R: Atualização perdida é um problema de concorrência em que duas ou mais transações acessam o mesmo dado simultaneamente e fazem escritas nele. O resultado da última transação é o que será persistido no banco, enquanto as outras serão sobrescritas e perdidas.

---

## Questão 32
R: O papel do isolamento é impedir que um usuário veja as alterações e dados parciais gerados por outro enquanto elas não forem confirmadas. Isso impede, por exemplo, que um usuário leia um valor incorreto e que será corrigido por outro usuário por meio de um ROLLBACK.

---

## Questão 33
R: Se uma transação T está lendo um dado que está sendo utilizado por outra, existe o risco do dado ser alterado e T ficar com um valor desatualizado.

---

## Questão 34
R: Por conta do bloqueio, se uma transação for muito longa, outras transações que queiram operar sobre os mesmos dados ficarão esperando por muito tempo para serem liberadas.

---

## Questão 35
R: A concorrência, se feita sem controle, pode gerar problemas e ferir a consistência dos dados, deixando o banco menos consistente.

---

## Questão 36
R: Dois usuários num sistema de cinema estão tentando comprar o mesmo lugar numa mesma sessão.

---

## Questão 37
R: Operações simultâneas feitas sobre registros diferentes não geram conflitos.

---

## Questão 38
R: O banco de dados bloqueia transações que estão tentando operar sobre dados que já estão sendo utilizados por outra.

---

## Questão 39
R: Transações poderiam operar sobre dados desatualizados e gerar atualizações perdidas, gerando inconsistência de dados.

---

## Questão 40
R: A ordem de execução é importante para garantir que as transações estão ocorrendo no momento certo e utilizando os dados mais atualizados para aquele momento.

---

# Sessão 8

## Criação da tabela

```SQL
CREATE TABLE conta (
   id INT PRIMARY KEY,
   titular VARCHAR(100),
   saldo DECIMAL(10,2)
);
```

## Inserção de dados

```SQL
INSERT INTO conta VALUES
(1, 'Ana', 1000.00),
(2, 'Carlos', 1500.00);
```

## Teste 1 — Bloqueio explícito com FOR UPDATE

### Objetivo
Demonstrar o bloqueio de um registro.

### Sessão 1

```SQL
START TRANSACTION;

SELECT *
FROM conta
WHERE id = 1
FOR UPDATE;
```

### Resultado
- o registro da conta 1 fica bloqueado;
- nenhuma outra transação poderá alterá-lo até o COMMIT ou ROLLBACK.

### Sessão 2

```SQL
UPDATE conta
SET saldo = saldo - 100
WHERE id = 1;
```

### Resultado esperado
- a consulta ficará esperando;
- o banco identifica que o registro está bloqueado.

### Liberação do lock — Sessão 1

```SQL
COMMIT;
```

### Após isso
- a Sessão 2 continua automaticamente.

## Teste 2 — Espera entre transações

### Objetivo
Demonstrar por que algumas transações precisam esperar.

### Sessão 1

```SQL
START TRANSACTION;

UPDATE conta
SET saldo = saldo - 200
WHERE id = 1;
```

### Sessão 2

```SQL
UPDATE conta
SET saldo = saldo + 300
WHERE id = 1;
```

### Resultado
- a Sessão 2 ficará bloqueada;
- existe disputa pelo mesmo recurso.

## Teste 3 — Concorrência em registros diferentes

### Objetivo
Demonstrar concorrência sem conflito.

### Sessão 1

```SQL
UPDATE conta
SET saldo = saldo - 100
WHERE id = 1;
```

### Sessão 2

```SQL
UPDATE conta
SET saldo = saldo + 100
WHERE id = 2;
```

### Resultado
- ambas executam simultaneamente;
- não há conflito;
- os registros são independentes.

## Teste 4 — Atualização perdida (Lost Update)

### Objetivo
Mostrar o risco de sobrescrita de dados.

## Situação inicial

```SQL
SELECT *
FROM conta
WHERE id = 1;
```

### Saldo
```text
1000
```

## Sessão 1 lê o saldo

```SQL
SELECT saldo
FROM conta
WHERE id = 1;
```

### Resultado
```text
1000
```

## Sessão 2 lê o saldo

```SQL
SELECT saldo
FROM conta
WHERE id = 1;
```

### Resultado
```text
1000
```

## Sessão 1 atualiza

```SQL
UPDATE conta
SET saldo = 900
WHERE id = 1;
```

---

## Sessão 2 atualiza depois

```SQL
UPDATE conta
SET saldo = 800
WHERE id = 1;
```

### Problema
- a atualização da Sessão 1 foi sobrescrita;
- ocorreu atualização perdida.

## Como evitar atualização perdida

Utilizando:

```SQL
SELECT *
FROM conta
WHERE id = 1
FOR UPDATE;
```

### Ou
- níveis adequados de isolamento.

## Teste 5 — Verificação da consistência final

### Após as transações

```SQL
SELECT *
FROM conta;
```

### O banco deve manter
- saldos corretos;
- nenhuma atualização perdida;
- integridade dos dados preservada.

## Explicação Teórica

## O que é concorrência em banco de dados

Concorrência é a capacidade de várias transações acessarem o banco de dados simultaneamente.

Ela é fundamental em sistemas multiusuário, como:
- bancos;
- e-commerce;
- sistemas acadêmicos;
- aplicativos financeiros.

Sem controle de concorrência, vários usuários poderiam alterar os mesmos dados ao mesmo tempo, causando inconsistências.

## Como funcionam os locks

Locks são mecanismos de bloqueio usados pelo banco para controlar acesso simultâneo aos dados.

Quando uma transação modifica um registro:
- o banco aplica um bloqueio;
- outras transações precisam aguardar.

O comando:

```SQL
FOR UPDATE
```

cria um lock exclusivo sobre os registros selecionados.


## Por que algumas transações precisam esperar

A espera acontece quando duas transações tentam acessar simultaneamente o mesmo recurso para escrita.

### Exemplo
- uma transação altera o saldo da conta;
- outra tenta alterar o mesmo saldo;
- o banco bloqueia a segunda até a primeira finalizar.

Isso evita corrupção de dados.

## O que é atualização perdida

Atualização perdida ocorre quando:
- duas transações leem o mesmo valor;
- ambas calculam alterações;
- uma sobrescreve a alteração da outra.

### Resultado
- uma modificação desaparece;
- os dados ficam inconsistentes.

## Por que o isolamento é importante

O isolamento impede que transações interfiram incorretamente umas nas outras.

Ele garante:
- segurança nas operações;
- previsibilidade dos dados;
- consistência mesmo com muitos usuários simultâneos.

## Como o banco preserva a consistência

O banco utiliza:
- transações;
- locks;
- controle de concorrência;
- níveis de isolamento.

Esses mecanismos garantem que:
- operações sejam executadas corretamente;
- dados não sejam corrompidos;
- alterações simultâneas mantenham integridade.


## Conclusão

Em ambientes multiusuário, concorrência é inevitável.

Por isso, bancos de dados utilizam locks e isolamento para garantir consistência e segurança.

Os testes mostraram que:
- registros iguais geram disputa;
- registros diferentes podem ser processados simultaneamente;
- locks evitam conflitos;
- o FOR UPDATE protege registros críticos;
- o isolamento impede atualizações perdidas.

Dessa forma, o banco mantém a integridade dos dados mesmo sob acesso concorrente intenso.

## Questão 41

R: Concorrência, bloqueios e isolamento atuam juntos para garantir a integridade e a consistência dos dados em bancos de dados multiusuário. A concorrência permite que várias transações sejam executadas simultaneamente, aumentando o desempenho e a disponibilidade do sistema, porém isso pode gerar conflitos quando diferentes usuários tentam acessar ou alterar os mesmos dados ao mesmo tempo.

Para evitar inconsistências, o banco utiliza bloqueios (locks), que controlam o acesso aos registros, fazendo com que uma transação espere enquanto outra finaliza uma alteração sobre o mesmo recurso. O comando `FOR UPDATE`, por exemplo, cria um bloqueio exclusivo para impedir modificações concorrentes em um registro crítico.

Além disso, o isolamento define como uma transação enxerga as alterações realizadas por outras, evitando problemas como leitura de dados não confirmados, atualizações perdidas e sobrescrita de informações.

Dessa forma, mesmo com múltiplos usuários operando simultaneamente, o banco consegue manter os dados corretos, seguros e consistentes, garantindo confiabilidade nas operações.
