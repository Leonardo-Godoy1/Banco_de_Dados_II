# Aplicando conceitos de transações em banco de dados sequencial

## 1. Atividade prática

#### Etapa 1. Criar o banco de teste

```sql
DROP TABLE IF EXISTS contas;

CREATE TABLE contas (
    id INT PRIMARY KEY,
    titular VARCHAR(100),
    saldo DECIMAL(10,2)
);

INSERT INTO contas (id, titular, saldo) VALUES
(1, 'Ana', 1000.00),
(2, 'Bruno', 500.00),
(3, 'Carlos', 300.00),
(4, 'Daniela', 800.00);

SELECT * FROM contas;
```

### **Pergunta 1**  
### Qual é o objetivo da tabela `contas` neste cenário prático?

R: Guardar as informações relevantes d uma conta bancária


### **Pergunta 2**  
### Quais são os saldos iniciais de cada titular antes da execução das transações?

R: Ana -> 1000.00;
   Bruno -> 500.00;
   Carlos -> 300.00;
   Daniela -> 800.00.


---

#### Etapa 2. Testar COMMIT

Execute:

```sql
START TRANSACTION;
UPDATE contas
SET saldo = saldo - 100
WHERE id = 1;
UPDATE contas
SET saldo = saldo + 100
WHERE id = 2;
COMMIT;
```

Depois:

```sql
SELECT * FROM contas;
```

### **Pergunta 3**  
### O que aconteceu com os saldos após o `COMMIT`?

R: Os saldos foram atualizados para:
   Ana -> 900.00;
   Bruno -> 600.00;
   Carlos -> 300.00;
   Daniela -> 800.00.


### **Pergunta 4**  
### Por que as duas instruções `UPDATE` devem fazer parte da mesma transação?

R: Neste caso se trata de uma transferência bancária, ou seja, deve tirar de uma e passar para outra. Desse modo atualizando
duas contas distintas para respeitar de maneira correta a regra de negócio.

---

#### Etapa 3. Testar ROLLBACK

Execute:

```sql
START TRANSACTION;
UPDATE contas
SET saldo = saldo - 50
WHERE id = 2;
UPDATE contas
SET saldo = saldo + 50
WHERE id = 3;
ROLLBACK;
```

Depois:

```sql
SELECT * FROM contas;
```

### **Pergunta 5**  
### Por que os valores não foram alterados ao final?

R: Pois houve um "Rollback" e, desse modo, não foram salvas as respostas.

### **Pergunta 6**  
### Em quais situações reais o uso de `ROLLBACK` seria essencial?

R: Se realizou uma operação equivocada ou houve algum problema na hora da persistência dos dados.

---

#### Etapa 4. Testar erro lógico antes da confirmação

Execute:

```sql
START TRANSACTION;
UPDATE contas
SET saldo = saldo - 2000
WHERE id = 3;
SELECT * FROM contas WHERE id = 3;
ROLLBACK;
```

Depois:

```sql
SELECT * FROM contas WHERE id = 3;
```

### **Pergunta 7**  
### Por que a transação foi desfeita neste caso?

R: Pois desrespeitou a regra de negócio.

### **Pergunta 8**  
### Qual problema de integridade poderia ocorrer se essa transação fosse confirmada?

R: A conta iria ter saldo negativo.

---

#### Etapa 5. Testar múltiplas operações na mesma transação

Execute:

```sql
START TRANSACTION;
UPDATE contas
SET saldo = saldo - 100
WHERE id = 4;
UPDATE contas
SET saldo = saldo + 60
WHERE id = 1;
UPDATE contas
SET saldo = saldo + 40
WHERE id = 2;
COMMIT;
```

Depois:

```sql
SELECT * FROM contas;
```

### **Pergunta 9**  
### Qual conta foi debitada e quais contas foram creditadas?

R: A conta com id 4, pertencente à Daniela foi debitada, e as contas com ids 1 e 2 respectivamente pertencentes a Ana e ao Bruno foram c 

### **Pergunta 10**  
### Por que esse conjunto de operações também deve ser tratado como uma única transação?

R: Neste caso se trata de uma transferência bancária para duas contas, ou seja, deve tirar de uma e passar para as outras. Desse modo atualizando
duas contas distintas para respeitar de maneira correta a regra de negócio.

---

#### Etapa 6. Testar leitura durante transação

Execute:

```sql
START TRANSACTION;
UPDATE contas
SET saldo = saldo - 150
WHERE id = 1;
```

Sem executar `COMMIT` ainda, em outra sessão rode:

```sql
SELECT * FROM contas WHERE id = 1;
```

Depois volte para a primeira sessão e execute:

```sql
ROLLBACK;
```

### **Pergunta 11**  
### Qual era o objetivo de observar o valor da conta em outra sessão antes do `COMMIT`?

R: Verificar que ele não alterou já que ele ainda não realizou o "commit".

### **Pergunta 12**  
### Como esse teste se relaciona com o conceito de isolamento?

R: Com o conceito de isolamento, os dados só são atualizados para verificação após o "commit", uma vez que,
os dados alterados só podem ser vistos ou alterados por outras operações/sessões após a persistência.

---

#### Etapa 7. Testar lock com duas sessões

Abra duas conexões.

### Sessão 1

```sql
START TRANSACTION;
SELECT * FROM contas WHERE id = 1 FOR UPDATE;
UPDATE contas
SET saldo = saldo - 200
WHERE id = 1;
```

Não execute `COMMIT` ainda.

### Sessão 2

```sql
START TRANSACTION;
UPDATE contas
SET saldo = saldo + 300
WHERE id = 1;
```

**Observe**  
A segunda sessão pode ficar bloqueada esperando a primeira terminar.

Agora volte para a Sessão 1 e faça:

```sql
COMMIT;
```

Depois finalize a Sessão 2.

### **Pergunta 13**  
### O que aconteceu com a segunda transação?

R: Foi bloqueada até que a primeira transação tenha sido concluída ("commitada").

### **Pergunta 14**  
### Por que ela precisou esperar?

R: Pois ambas estavam operando sobre o mesmo registro, desse modo, a segunda teve que esperar a primeira concluir
para que operasse em cima do valor mais atualizado.

### **Pergunta 15**  
### Qual a função do `FOR UPDATE`?

R: Avisar qual registro a operação vai alterar, para que possa tratar nas outras sessões.

---

#### Etapa 8. Testar concorrência em registros diferentes

Abra duas conexões.

### Sessão 1

```sql
START TRANSACTION;
UPDATE contas
SET saldo = saldo - 50
WHERE id = 1;
```

### Sessão 2

```sql
START TRANSACTION;
UPDATE contas
SET saldo = saldo + 70
WHERE id = 4;
```

Depois execute `COMMIT` em ambas.

### **Pergunta 16**  
### Por que nesse caso as transações tendem a não disputar o mesmo recurso?

R: Pois elas alteram registros distintos.

### **Pergunta 17**  
### O que esse teste mostra sobre concorrência em linhas diferentes da tabela?

R: Mostra que não há problemas em alterar simultaneamente linhas distintas da tabela.

---

#### Etapa 9. Criar tabela de movimentações

Execute:

```sql
DROP TABLE IF EXISTS movimentacoes;

CREATE TABLE movimentacoes (
    id INT AUTO_INCREMENT PRIMARY KEY,
    conta_origem INT,
    conta_destino INT,
    valor DECIMAL(10,2),
    data_movimentacao TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

### **Pergunta 18**  
### Qual é a importância de registrar movimentações além de atualizar os saldos?

R: Com esse histórico, mantém um controle mais rigoroso das transações, desse modo, reforçando a segurança.

---

#### Etapa 10. Transferência com registro em histórico

Execute:

```sql
START TRANSACTION;
UPDATE contas
SET saldo = saldo - 120
WHERE id = 2;
UPDATE contas
SET saldo = saldo + 120
WHERE id = 3;
INSERT INTO movimentacoes (conta_origem, conta_destino, valor)
VALUES (2, 3, 120.00);
COMMIT;
```

Depois:

```sql
SELECT * FROM contas;
SELECT * FROM movimentacoes;
```

### **Pergunta 19**  
### Por que o `INSERT` na tabela `movimentacoes` deve estar na mesma transação dos `UPDATE`s?

R: Porque o histórico faz parte da transação, uma vez que, registra umaoperação realizada.

### **Pergunta 20**  
### O que poderia acontecer se o histórico fosse gravado, mas os saldos não fossem atualizados, ou vice-versa?

R: Aconteceria inconsistências dos dados.

---

#### Etapa 11. Simular falha antes do registro da movimentação

Execute:

```sql
START TRANSACTION;
UPDATE contas
SET saldo = saldo - 80
WHERE id = 1;
UPDATE contas
SET saldo = saldo + 80
WHERE id = 4;
ROLLBACK;
```

Depois:

```sql
SELECT * FROM contas;
SELECT * FROM movimentacoes;
```

### **Pergunta 21**  
### O que o `ROLLBACK` garantiu nesse cenário?

R: Garantiu que não fossem registradas operações sem o histórico das mesmas.

### **Pergunta 22**  
### Como esse teste demonstra a propriedade de atomicidade?

R: A propriedade da atomicidade diz que uma transação não pode ser feita pela metade: ou é realizada corretamente até o fim, ou é descartada. Esse teste demonstra a propriedade ao não "commitar" uma transação feita de forma incompleta (por não atualizar a tabela de movimentações).

---

#### Etapa 12. Consultar estado final

Execute:

```sql
SELECT * FROM contas;
SELECT * FROM movimentacoes;
```

### **Pergunta 23**  
### Como verificar se o banco permaneceu consistente após todas as operações realizadas?

R: É possível comparar as informações de transferências presentes no histórico com os saldos finais para verificar se os dados estão coesos.

# Exercícios sobre Transações e Propriedades ACID

### Questão 24
### Por que a consistência do banco depende não apenas dos comandos SQL, mas também da forma como eles são agrupados em transações?

R: Porque a forma como são agrupadas ajuda a garantir que as transações são completas e que estão sendo realizadas na ordem correta.

---

### Questão 25
### Explique o que é uma transação em banco de dados.

R: Uma transação é uma unidade lógica de processamento de banco de dados que deve ser executada na íntegra ou cancelada totalmente.

---

### Questão 26
### Descreva a diferença entre COMMIT e ROLLBACK.

R: O COMMIT é a confirmação da transação, responsável por gravar as alterações feitas no disco. É utilizado quando a transação é bem sucedida. Por sua vez, o ROLLBACK é responsável por voltar os dados ao seu estado anterior ao da transação, cancelando-a. É utilizado quando a transação não é bem sucedida.

---

### Questão 27
### Explique por que uma transferência bancária deve ser tratada como transação.

R: Uma transferência bancária deve ser tratada como transação pois ela não pode ser executada pela metade — ou é executada corretamente até o final ou é totalmente cancelada.

---

### Questão 28
### O que pode acontecer se duas transações alterarem o mesmo dado ao mesmo tempo sem controle de concorrência?

R: Pode acontecer de uma das transações utilizar um dado desatualizado (por ter sido alterado na outra transação) ou uma transação pode escrever por cima do resultado da outra, deixando-a inútil.

---

### Questão 29
### Qual a relação entre transações e as propriedades ACID?

R: As propriedades ACID são utilizadas pelas transações a fim de manter a consistência dos dados e ajudar no controle de concorrência, tornando o banco mais seguro.

---

### Questão 30
### Explique o significado da propriedade de atomicidade no contexto de uma operação bancária.

R: A propriedade da atomicidade diz que uma transação não pode ser executada pela metade, ou seja, ou ela é executada corretamente até o fim, ou é completamente revertida. No contexto de uma operação bancária, o significado dela é que uma operação (como uma transferência, por exemplo) só pode ser finalizada quando todos os passos forem executados corretamente. No caso de algum erro ou violação de regra de negócio, todo o procedimento é revertido, a fim de não causar prejuízos nas contas.

---

### Questão 31
### Explique o que significa dizer que uma transação preserva a consistência do banco de dados.

R: A transação não pode quebrar a consistência do banco de dados, ou seja, ela deve sair de um estado matematicamente válido para outro também válido.

---

### Questão 32
### Descreva o papel do isolamento em ambientes com múltiplos usuários acessando o mesmo banco.

R: O papel do isolamento é impedir que um usuário veja as alterações e dados parciais gerados por outro enquanto elas não forem confirmadas. Isso impede, por exemplo, que um usuário leia um valor incorreto e que será corrigido por outro usuário por meio de um ROLLBACK.

---

### Questão 33
### Explique a importância da durabilidade após a execução de um COMMIT.

R: A durabilidade é responsável por garantir que os dados alterados na transação permanecerão no banco mesmo em caso de falha energética ou catastrófica.

---

### Questão 34
### O que é controle de concorrência e por que ele é necessário?

R: Controle de concorrência é o mecanismo do banco de dados que garante que múltiplos usuários possam acessar e modificar dados simultaneamente sem corromper as informações. É necessário para garantir o isolamento de transações concorrentes.

---

### Questão 35
### Explique a função do lock em transações concorrentes.

R: O lock é responsável por impedir uma transação de acessar um dado que está sendo alterado por outra, a fim de garantir que ela utilizará a versão mais atualizada do dado.

---

### Questão 36
### Descreva um exemplo prático em que o FOR UPDATE seja necessário.

R: Um usuário numa sessão do banco deseja informar aos usuários em outras sessões que ele vai alterar os dados da conta de id = 1.

---

### Questão 37
### O que é uma atualização perdida (lost update)?

R: Atualização perdida é um problema de concorrência em que duas ou mais transações acessam o mesmo dado simultaneamente e fazem escritas nele. O resultado da última transação é o que será persistido no banco, enquanto as outras serão sobrescritas e perdidas.

---

### Questão 38
### Explique por que nem toda leitura concorrente gera problema, mas algumas atualizações simultâneas sim.

R: Leituras de dados diferentes não causam problemas no banco, somente leituras do mesmo dado podem causar inconsistências.

---

### Questão 39
### Qual é a importância de registrar operações em uma tabela de histórico dentro da mesma transação?

R: É importante pois ajuda a manter a integridade e a segurança dos dados, além de auxiliar na recuperação em caso de erro.

---

### Questão 40
### Em um sistema acadêmico, cite um exemplo de operação que deveria ser tratada como transação.

R: Lançar as notas de um aluno no sistema.

---

### Questão 41
### Em um sistema de estoque, cite um exemplo de falha que poderia justificar o uso de ROLLBACK.

R: A remoção de um item numa quantidade maior do que a que estava disponível no estoque.

---

### Questão 42
### Como o processamento de transações contribui para a confiabilidade de sistemas de informação?

R: O processamento de transações garante maior consistência dos dados e permite a recuperação em casos de erro, auxiliando a manter os dados do sistema íntegros.

---

### Questão 43
### Considerando todos os experimentos realizados, explique de forma integrada como atomicidade, consistência, isolamento e durabilidade atuam em conjunto no processamento de transações.

R: As quatro propriedades atuam em conjunto a fim de garantir a integridade das transações: a atomicidade garante que a transação será realizada até o fim ou totalmente revertida, sem resultados parciais que podem sujar o banco; a consistência garante que o banco nunca estará num estado matematicamente inválido; o isolamento garante que uma transação não utilizará dados parciais gerados em outra; a durabilidade garante que os dados serão persistidos no banco sem riscos de serem perdidos.

---

## Desafio

### Questão 44
### Adapte o exemplo bancário para um sistema de matrícula em disciplinas, em que uma transação deva:
- verificar vaga disponível
- reduzir a quantidade de vagas
- registrar a matrícula do aluno

### Explique por que essas operações devem ocorrer na mesma transação.

```SQL
DROP TABLE IF EXISTS disciplina;
DROP TABLE IF EXISTS matricula;

CREATE TABLE disciplina (
    id INT AUTO_INCREMENT PRIMARY KEY,
    nome VARCHAR(100),
    vagas_disponiveis INT
);

CREATE TABLE matricula (
    id INT AUTO_INCREMENT PRIMARY KEY,
    aluno VARCHAR(100),
    id_disciplina INT
);

INSERT INTO disciplina (id, nome, vagas_disponiveis) VALUES
(1, 'Banco de Dados II', 20),
(2, 'Projeto e Análise de Algoritmos', 15),
(3, 'Sistemas Operacionais', 10);

START TRANSACTION;

UPDATE disciplina
SET vagas_disponiveis = vagas_disponiveis - 1
WHERE id = 1;

INSERT INTO matricula (id, aluno, id_disciplina) VALUES
(1, 'Caio César', 1);

COMMIT;
```

R: Essas operações devem ocorrer na mesma transação porque fazem parte do mesmo processo: a matrícula só pode ser realizada se houver vagas disponíveis e a quantidade de vagas na disciplina deve ser decrementada. Caso contrário, o banco ficaria com dados inconsistentes ou parciais.

---

### Questão 45
### Adapte o exemplo para um sistema de estoque e vendas, explicando quais operações devem ser agrupadas para evitar inconsistências.

R: As operações de checagem de itens disponíveis, remoção de itens do estoque e registro da venda devem ser agrupadas juntas a fim de evitar inconsistências.
