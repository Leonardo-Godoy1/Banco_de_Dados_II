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

## Questão 21
```SQL 
SELECT aluno.nome, matricula.situacao
FROM aluno
INNER JOIN matricula
    ON aluno.id = matricula.id;
```

## Questão 22
```SQL 
SELECT aluno.nome, disciplina.nome
FROM aluno
INNER JOIN matricula
    ON aluno.id = matricula.aluno_id
INNER JOIN disciplina
    ON disciplina.id = matricula.disciplina_id;
```

## Questão 23
```SQL
SELECT aluno.nome, disciplina.nome, matricula.nota
FROM aluno
INNER JOIN matricula
    ON aluno.id = matricula.aluno_id
INNER JOIN disciplina
    ON disciplina.id = matricula.disciplina_id;
```

## Questão 24
```SQL
SELECT DISTINCT aluno.nome, aluno.curso, aluno.cidade, aluno.ano_ingresso
FROM aluno
INNER JOIN matricula
    ON aluno.id = matricula.aluno_id
INNER JOIN disciplina
    ON disciplina.id = matricula.disciplina_id
WHERE disciplina.departamento = 'Computacao';
```

## Questão 25
```SQL
SELECT DISTINCT aluno.nome
FROM aluno
INNER JOIN matricula
    ON aluno.id = matricula.aluno_id
WHERE situacao = 'Reprovado';
```

## Questão 26
```SQL
SELECT aluno.nome, disciplina.nome
FROM aluno
INNER JOIN matricula
    ON aluno.id = matricula.aluno_id
INNER JOIN disciplina
    ON disciplina.id = matricula.disciplina_id
WHERE aluno.curso = 'Computacao';
```

## Questão 27
```SQL
SELECT aluno.nome, AVG(matricula.nota) AS media
FROM aluno
INNER JOIN matricula
    ON aluno.id = matricula.aluno_id
GROUP BY aluno.nome;
```

## Questão 28
```SQL
SELECT aluno.nome, COUNT(*) AS total_disciplinas
FROM aluno
INNER JOIN matricula
    ON aluno.id = matricula.aluno_id
INNER JOIN disciplina
    ON disciplina.id = matricula.disciplina_id
GROUP BY aluno.nome;
```

## Questão 29
```SQL
SELECT aluno.nome, aluno.curso, aluno.cidade, aluno.ano_ingresso
FROM aluno
INNER JOIN matricula
    ON aluno.id = matricula.aluno_id
GROUP BY aluno.nome
HAVING AVG(matricula.nota) > 8;
```

## Questão 30
```SQL
SELECT disciplina.departamento, COUNT(*) AS total_matriculas
FROM disciplina
INNER JOIN matricula
    ON disciplina.id = matricula.disciplina_id
GROUP BY disciplina.departamento;
```
