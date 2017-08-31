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

CREATE OR REPLACE FUNCTION qtdEmpregadoProjeto(codProj in number)
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
end qtdEmpregadoProjeto;

select * from projeto;
select * from empregado;
select * from trabalhano;
```

```
b) Faça uma função que receba o nome de um dependente e retorne o nome do empregado
do qual ele depende. 
CREATE OR REPLACE FUNCTION returnNomeEmp(nomeDep in varchar2)
RETURN varchar2 AS
var_nomeEmp varchar2(20);
BEGIN
select e.nome into var_nomeEmp 
from empregado e inner join DEPENDENTE d
on e.IdentEmp = d.IdentEmp
where d.nome = nomeDep;
RETURN var_nomeEmp;
END;

begin
dbms_output.put_line(returnNomeEmp('Laura'));
end;

select * from empregado;
select * from DEPENDENTE;
```
```
c. Faça uma função que receba o nome de um projeto e retorne a quantidade 
total de horas trabalhadas nesse projeto. 

CREATE OR REPLACE FUNCTION RETORNA_SOMA(NOMEPROJ IN VARCHAR2)
RETURN NUMBER AS
TOTAL_HRS NUMBER;
BEGIN
 SELECT SUM(HRS)
 INTO TOTAL_HRS
 FROM PROJETO, TRABALHANO
 WHERE PROJETO.PROJNUM = TRABALHANO.PROJNUM
 AND upper(PROJETO.NOME) = upper(NOMEPROJ);
 RETURN TOTAL_HRS;
END; 

DECLARE
 TOTAL_HRS NUMBER;
BEGIN
 TOTAL_HRS := RETORNA_SOMA('Banco de Dados');
 dbms_output.put_line(total_hrs);
end; 
```
#### http://moodle.canoas.ifrs.edu.br/pluginfile.php/11656/mod_resource/content/3/exercicio_Blocos_Procedimentos-Funcoes.pdf

a. Receber como parâmetro dados para a inserção de uma tupla na tabela empregado. Não
receber o valor para identemp – essa informações será obtida a partir de consulta à base
de dados e receber o nome do departamento, ao invés do código. Proceda com a inserção
da informação dentro do procedimento. 

```
CREATE OR REPLACE FUNCTION consultarDep(nomeDep in varchar)
RETURN NUMBER AS
codDep NUMBER;
BEGIN
select DepNum into codDep 
from departamento
where nomeDep = departamento.nome;
return codDep;
END consultarDep;

begin
dbms_output.put_line(consultarDep('RH'));
end;
----------------------------------------
CREATE OR REPLACE FUNCTION maxIdentEmp
RETURN NUMBER AS
var_IdentEmp NUMBER;
BEGIN
select max(IdentEmp)+1 into var_IdentEmp 
from empregado;
return var_IdentEmp;
END maxIdentEmp;

begin
dbms_output.put_line(maxIdentEmp());
end;
-----------------
CREATE OR REPLACE PROCEDURE Process_insert_emp
    (Nome empregado.nome%type, Sal empregado.Sal%type, Ende empregado.ENDERECO%type, Sexo empregado.sexo%type, DataNasc empregado.DataNasc%type, nomeDep DEPARTAMENTO.nome%type)
AS
var_depNum empregado.depNum%type;
var_IdentEmp empregado.IdentEmp%type;

BEGIN
var_depNum := consultarDep(nomeDep);
var_IdentEmp := maxIdentEmp;

insert into EMPREGADO (IdentEmp, Nome, Sal, ENDERECO, Sexo, DataNasc, DepNum) values (var_IdentEmp,Nome,Sal,Ende,Sexo,DataNasc,var_depNum );
    dbms_output.put_line('Inserido com sucesso');
   
END Process_insert_emp;
----------------------------------------
begin
  Process_insert_emp('Estafania', 80000,'Rua das bromelias n:123', 'F', Sysdate, 'RH');
end;

select * from empregado;
```

b. Faça uma função que receba como parâmetro o código de um empregado e o percentual
de aumento que receberá. Proceda com a atualização do salário e retorne o valor alterado.
```
CREATE OR REPLACE FUNCTION updateEmp(codEmp in number, percentualAumento in number)
RETURN NUMBER AS
newSalario NUMBER;
salarioEmp number;

BEGIN

select Sal into salarioEmp 
from empregado
where codEmp = empregado.IdentEmp;
newSalario := salarioEmp +  ((salarioEmp * percentualAumento)/100); 
UPDATE empregado set sal = newSalario where codEmp = IdentEmp;
return newSalario;
END updateEmp;

begin
dbms_output.put_line(updateEmp( 2, 10));
end;

select * from empregado

```
