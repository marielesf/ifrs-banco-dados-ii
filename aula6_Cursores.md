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
#### Cursores exercicios: 

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
