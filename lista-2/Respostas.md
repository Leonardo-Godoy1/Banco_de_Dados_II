## Questão 1 
``` SQL
SELECT * FROM aluno
```

## Questão 2 
```SQL
SELECT nome, curso FROM aluno
```

## Questão 3 
```SQL
SELECT * FROM aluno WHERE curso = 'Computacao'
```

## Questão 4
```SQL 
SELECT * FROM aluno WHERE cidade = 'Maringa'
```

## Questão 5
```SQL 
SELECT * FROM aluno ORDER BY nome
```

## Questão 6
```SQL
SELECT * FROM aluno ORDER BY ano_ingresso
```

## Questão 7
```SQL
SELECT * FROM aluno WHERE ano_ingresso >= 2022
```

## Questão 8
```SQL
SELET * FROM aluno WHERE nome LIKE 'A%'
```

## Questão 9
```SQL
SELECT * FROM aluno WHERE curso = 'Computacao' OR curso = 'Engenharia'
```

## Questão 10
```SQL
SELECT * FROM disciplina WHERE carga_horaria >= 60 AND carga_horaria <= 80
```

## Questão 11
```SQL
SELECT COUNT(id) FROM aluno
```

## Questão 12
```SQL
SELECT AVG(nota) FROM matricula
```

## Questão 13
```SQL
SELECT MAX(nota) FROM matricula
```

## Questão 14
```SQL
SELECT MIN(nota) FROM matricula
```

## Questão 15
```SQL
SELECT SUM(carga_horaria) FROM disciplina
```

## Questão 16
```SQL
SELECT curso, COUNT(*) FROM aluno GROUP BY(curso)
```

## Questão 17
```SQL
SELECT cidade, COUNT(*) FROM aluno GROUP BY(cidade)
```

## Questão 18
```SQL
SELECT situacao, AVG(nota) FROM matricula GROUP BY(situacao)
```

## Questão 19
```SQL
SELECT semestre, COUNT(*) FROM matricula GROUP BY(semestre)
```

## Questão 20
```SQL 
SELECT curso FROM aluno GROUP BY(CURSO) HAVING COUNT(*) > 1
```
