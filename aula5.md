#### Dia 31/08/2017

#### Revisão lista procedimentos 1: 
```
f) Receber como parâmetro o nome de um departamento e retornar a média 
salarial dos empregados deste departamento em um parâmetro de saída. 
Mostre o resultado no bloco chamador.

CREATE OR REPLACE PROCEDURE RETORNA_MEDIA(NOMEDEP IN VARCHAR2, MEDIA OUT NUMBER)
AS
BEGIN
  SELECT AVG(SAL)
  INTO MEDIA
  FROM EMPREGADO1 INNER JOIN DEPARTAMENTO1 
       ON EMPREGADO1.DEPNUM = DEPARTAMENTO1.DEPNUM
  WHERE DEPARTAMENTO1.NOME = NOMEDEP;
END; 

DECLARE
 MEDIA_VAR NUMBER;
BEGIN
 RETORNA_MEDIA('RH',MEDIA_VAR);
 DBMS_OUTPUT.PUT_LINE('A media salarial do departamento é: '|| MEDIA_VAR);
END; 
```

#### http://moodle.canoas.ifrs.edu.br/pluginfile.php/11752/mod_resource/content/2/exercicio_Blocos_Funcoes_2.pdf
#### RETURN
```
a) Faça uma função que receba como parâmetro o código de um projeto e retorne a
quantidade de alunos que participam desse projeto. 

CREATE OR REPLACE FUNCTION qtdEmpregadoProjeto(codProj number)
RETURN NUMBER AS
qtdEmp NUMBER;
BEGIN
select count(e.IdentEmp) into qtdEmp 
from projeto p, empregado e, trabalhano t
where codProj = p.ProjNum and 
p.ProjNum = t.ProjNum and
t.IdentEmp = e.IdentEmp;
RETURN qtdEmp;
END;

begin
dbms_output.put_line(qtdEmpregadoProjeto(1));
end;

select * from projeto;
select * from empregado;
select * from trabalhano;
```
