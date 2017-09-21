#### Dia 14/09/2017
```
Exemplo 1:
CURSOR meu_cursor IS SELECT * FROM empregados
WHERE departamento_id = 20;
reg empregados%RowType;
BEGIN
open meu_cursor;
LOOP
FETCH meu_cursor INTO reg;
EXIT WHEN meu_cursor%NOTFOUND;
 dbms_output.put_line(reg.lastname);
END LOOP;
END;

Exemplo 2:
Cursor FOR LOOP
CURSOR c1 IS
SELECT nome_funcionario,nome_cargo,salário
FROM Funcionarios f
WHERE f.salário > 2000
cf c1%ROWTYPE; -- Declara uma variavel do tipo cursor
BEGIN
/* FOR record_name IN cursor_name */
/* For open,fetch e close implícito */
FOR cf IN c1 LOOP
DBMS_OUTPUT.PUT_LINE(cf.nome_funcionario||cf.nome_cargo||cf.salario);
END LOOP;
Ou
DECLARE
 bonus REAL;
BEGIN
 FOR funcionarios_rec IN (SELECT * FROM funcionarios) LOOP
 bonus := funcionarios_rec.salario*1.1;
 DBMS_OUTPUT.put_line(to_char(bonus));
 END LOOP;
 COMMIT;
END; 
```
#### Cursores exercicios simples: http://moodle.canoas.ifrs.edu.br/pluginfile.php/11760/mod_resource/content/1/exercicio_Blocos_Cursor_A.pdf 

a. Receba o nome de um projeto, imprima seu código (uma única vez) e apresente os nomes
dos empregados que trabalham nesse projeto.
```
V1:
declare
var_nomeProj Projeto.nome%Type;

CURSOR meu_cursor IS SELECT  distinct(p.ProjNum), e.nome  
from Projeto p, TRABALHANO t, EMPREGADO e
where p.Nome = var_nomeProj AND
t.ProjNum = p.ProjNum AND
t.IdentEmp = e.IdentEmp;
var_proj meu_cursor%RowType;
BEGIN
var_nomeProj := :var_nomeProj;
open meu_cursor;

FETCH meu_cursor INTO var_proj;
 dbms_output.put_line('Codigo do projeto: ' ||var_proj.ProjNum);
LOOP
EXIT WHEN meu_cursor%NOTFOUND;
 dbms_output.put_line(var_proj.nome);
FETCH meu_cursor INTO var_proj;
END LOOP;
END;

v2:
create or replace procedure empregados_projeto(nomeproj varchar2) as
  cursor dados_emp(codigo number) is 
    SELECT E.NOME
    FROM EMPREGADO E INNER JOIN TRABALHANO T ON E.IDENTEMP = T.IDENTEMP
    INNER JOIN PROJETO P ON T.PROJNUM = P.PROJNUM
    WHERE P.projnum = codigo;
  var_dados dados_emp%rowtype;
  var_codigo number;
begin
 select projnum
 into var_codigo
 from projeto
 where nome = nomeproj; 
 dbms_output.put_line('Código do projeto: '|| var_codigo);
 for var_dados in dados_emp(var_codigo) loop 
   dbms_output.put_line(var_dados.nome);
 end loop;
end; 

begin
 empregados_projeto('Banco de Dados');
end; 
```

b. Receba o código de um departamento e imprima todos os projetos vinculados a esse
departamento e todos os empregados vinculados a esse departamento (informações
separadas). 
```
create or replace procedure empregados_departamento(codDep number) as
  
cursor proj_departamento is SELECT p.nome as projNome
    FROM PROJETO p INNER JOIN DEPARTAMENTO d ON p.DepNum = d.DepNum
    WHERE p.DepNum = codDep;

cursor emp_departamento is SELECT e.nome as EmpNome
    FROM EMPREGADO E INNER JOIN TRABALHANO T ON E.IDENTEMP = T.IDENTEMP
    INNER JOIN PROJETO P ON T.PROJNUM = P.PROJNUM
    WHERE p.projnum = codDep;

  var_cursorProjDep proj_departamento%rowtype;
  var_cursorEmpDep emp_departamento%rowtype;

begin
 for var_cursorProjDep in proj_departamento loop 
   dbms_output.put_line(var_cursorProjDep.projNome);
 end loop;

 for var_cursorEmpDep in emp_departamento loop 
   dbms_output.put_line(var_cursorEmpDep.empNome);
 end loop;
end; 

begin
 empregados_departamento('2');
end; 
```

#### Cursores exercicios Complexos: http://moodle.canoas.ifrs.edu.br/pluginfile.php/18105/mod_resource/content/2/exercicio_Blocos_Cursor_Lista1.pdf

3- Faça um procedimento que receba como parâmetro o nome de um empregado e gere o seguinte
relatório:
<Código Empregado> – <Nome>
 trabalhou dos Seguintes projetos:
 <Nome do Projeto> - <HRS>
 <Nome do Projeto> - <HRS>
   
```
create or replace procedure empregados_proj_hrs(codemp number) as
cursor dadosEmp is SELECT p.nome, t.hrs 
    FROM Projeto p, trabalhano t
    WHERE t.IdentEmp = codemp and 
t.ProjNum = p.ProjNum;

var_cursor_dadosEmp dadosEmp%rowtype;
var_nomeEmp empregado.nome%type;
begin

select nome
 into var_nomeEmp 
 from EMPREGADO
 where IdentEmp = codemp ; 

dbms_output.put_line(codemp  ||' - ' || var_nomeEmp );
dbms_output.put_line('trabalhou dos Seguintes projetos:');
  for var_cursor_dadosEmp in dadosEmp loop 
   dbms_output.put_line(var_cursor_dadosEmp.nome ||' - '|| var_cursor_dadosEmp.hrs);
 end loop;
end; 

begin
 empregados_proj_hrs(1);
end; 

```

3. Faça um procedimento que receba como parâmetro o nome de um empregado 
e gere o seguinte relatório:
<Código Empregado> – <Nome>
 trabalhou dos Seguintes projetos:
 <Nome do Projeto> - <HRS>
 <Nome do Projeto> - <HRS>
 <Nome do Projeto> - <HRS>
 
 ```
CREATE OR REPLACE PROCEDURE EX_3(NOMEEMP VARCHAR2) AS
   VAR_COD NUMBER;
   cursor dados is 
     SELECT P.NOME, T.HRS
     FROM EMPREGADO1 E, TRABALHANO T, PROJETO P
     WHERE E.IDENTEMP = T.IDENTEMP AND T.PROJNUM = P.PROJNUM
       AND E.NOME = nomeemp;
   var_dados dados%rowtype;
BEGIN
   SELECT IDENTEMP
   INTO VAR_COD
   FROM EMPREGADO1
   WHERE NOME = NOMEEMP;
   DBMS_OUTPUT.PUT_LINE(VAR_COD || ' - ' || NOMEEMP);
   DBMS_OUTPUT.PUT_LINE('trabalhou nos seguintes projetos:');
   for var_dados in dados loop
     dbms_output.put_line(var_dados.nome || ' - '|| var_dados.HRS);
   end loop; 
END; 

BEGIN
 EX_3('Auria');
end;
   
   SELECT * FROM EMPREGADO1
   SELECT * FROM TRABALHANO
  ``` 
   
4. Faça uma função que receba como parâmetro o número de um departamento. 
Gere o seguinte relatório:
<Nome Departamento >
 Quantidade de projetos: <XXX>
 Funcionários alocados no departamento:
 <Nome Funcionario>
 <Nome Funcionario>
Retorne, a quantidade total de funcionários alocados. 
  
```
CREATE OR REPLACE FUNCTION EX_4(CODDEP NUMBER) RETURN NUMBER AS
  NOMEDEP DEPARTAMENTO1.NOME%TYPE; 
  QUANTIDADE NUMBER; 
  CURSOR DADOS_EMP IS
   SELECT E.NOME
   FROM EMPREGADO1 E INNER JOIN DEPARTAMENTO1 D ON E.DEPNUM = D.DEPNUM
   WHERE D.DEPNUM = CODDEP;
  VAR_DADOS DADOS_EMP%ROWTYPE; 
BEGIN
 SELECT NOME
 INTO NOMEDEP
 FROM DEPARTAMENTO1
 WHERE DEPNUM = CODDEP; 
 DBMS_OUTPUT.PUT_LINE(NOMEDEP); 
 SELECT COUNT(*)
 INTO QUANTIDADE
 FROM PROJETO
 WHERE DEPNUM = CODDEP; 
 DBMS_OUTPUT.PUT_LINE('Quantidade de projetos: '||QUANTIDADE); 
 DBMS_OUTPUT.PUT_LINE('Funcionários alocados no departamento:');
 QUANTIDADE := 0;
 OPEN DADOS_EMP;
 LOOP
  FETCH DADOS_EMP INTO VAR_DADOS;
  EXIT WHEN DADOS_EMP%NOTFOUND;
  DBMS_OUTPUT.PUT_LINE(VAR_DADOS.NOME);
  QUANTIDADE := QUANTIDADE +1; 
 END LOOP;
 CLOSE DADOS_EMP; 
 RETURN(QUANTIDADE); 
END; 
  
DECLARE
 QUANT NUMBER;
BEGIN
 QUANT := EX_4(1);
 DBMS_OUTPUT.PUT_LINE('Quantidade de funcionarios alocados: '||quant);
END; 
```
