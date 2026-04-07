<h2> Questão 1 </h2>
SELECT * FROM aluno 

<h2> Questão 2 </h2>
SELECT nome, curso FROM aluno

<h2> Questão 3 </h2>
SELECT * FROM aluno WHERE curso = 'Computacao'

<h2> Questão 4 </h2>
SELECT * FROM aluno WHERE cidade = 'Maringa'

<h2> Questão 5 </h2>
SELECT * FROM aluno ORDER BY nome

<h2> Questão 6 </h2>
SELECT * FROM aluno ORDER BY ano_ingresso

<h2> Questão 7 </h2>
SELECT * FROM aluno WHERE ano_ingresso >= 2022

<h2> Questão 8 </h2>
SELET * FROM aluno WHERE nome LIKE 'A%'

<h2> Questão 9 </h2>
SELECT * FROM aluno WHERE curso = 'Computacao' OR curso = 'Engenharia'

<h2> Questão 10 </h2>
SELECT * FROM disciplina WHERE carga_horaria >= 60 AND carga_horaria <= 80

<h2> Questão 11 </h2>
SELECT COUNT(id) FROM aluno 

<h2> Questão 12 </h2>
SELECT AVG(nota) FROM matricula

<h2> Questão 13 </h2>
SELECT MAX(nota) FROM matricula

<h2> Questão 14 </h2>
SELECT MIN(nota) FROM matricula

<h2> Questão 15 </h2>
SELECT SUM(carga_horaria) FROM disciplina
