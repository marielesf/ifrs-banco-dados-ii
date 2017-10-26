#### Exemplo:

create table temp_table
(id_trigger number primary key, msg varchar(100))

create sequence trigger_seq;

select * from temp_table;

CREATE OR REPLACE TRIGGER trigger_type_row
AFTER
 UPDATE
ON EMPREGADO
FOR EACH ROW
BEGIN
 INSERT INTO temp_table VALUES(trigger_seq.nextval,'After Row trigger');
END; 

update empregado 
set sal= sal *1.1;

select * from temp_table;

CREATE OR REPLACE TRIGGER trigger_type_statement
AFTER
 UPDATE
ON empregado
BEGIN
 INSERT INTO temp_table VALUES(trigger_seq.nextval,'After Statement trigger');
END; 

select * from temp_table;

update empregado 
set sal= sal *1.1;

select * from temp_table;

#### Exercicos de triggers:  
http://moodle.canoas.ifrs.edu.br/pluginfile.php/18450/mod_resource/content/2/Exercicios_Gatilhos_Lista1.pdf

1. Faça um gatilho que, após inserir uma tupla na tabela trabalhano, imprime a seguinte informação:
O empregado de nome TAL está alocado ao projeto TAL em tantas HORAS. 

```
create or replace TRIGGER DADOS_PROJ_EMP AFTER INSERT
ON TRABALHANO  FOR EACH ROW
DECLARE
  NOMEEMP VARCHAR2(30);
  NOMEPROJ VARCHAR2(30);
BEGIN
 SELECT E.NOME,P.NOME
 INTO NOMEEMP, NOMEPROJ
 FROM EMPREGADO1 E, PROJETO P
 WHERE E.IDENTEMP = :NEW.IDENTEMP
 AND P.PROJNUM = :NEW.PROJNUM;  

 DBMS_OUTPUT.PUT_LINE('O empregado de nome '|| NOMEEMP|| ' está alocado ao projeto '|| NOMEPROJ ||
' em '|| :NEW.HRS ||' horas.');
END;



CREATE OR REPLACE TRIGGER trigger_inserirTrabalhano
AFTER
 INSERT
ON TRABALHANO
FOR EACH ROW
DECLARE
 nomeEmp varchar2(30);
 nomeProj varchar2(30);

BEGIN
select E.NOME, p.nome
into nomeEmp, nomeProj
from empregado e, projeto p
where e.idemtEmp =  :NEW.identEMP
AND p.projnum = :NEW.projnum;

DBMS_OUTPUT.PUT_LINE('O empregado de nome' || nomeEmp||' está alocado ao projeto' ||projnum||' em ' || :NEW.HRS||'Horas.' );

 INSERT INTO trigger_inserirTrabalhano VALUES(trigger_seq.nextval,'Linha inserida em TRABALHANO - trigger_inserirTrabalhano');
END; 

insert into trabalhano values(2,2,200);
```

2. Faça um gatilho que, antes de inserir uma tupla na tabela trabalhano, verifica se as horas a serem
trabalhadas pelo empregado não são superiores a 200 horas. Se for, não permita a inserção. 
```
CREATE OR REPLACE TRIGGER trigger_verificarTrabalhano
before INSERT
ON TRABALHANO
FOR EACH ROW

BEGIN
IF (:new.HRS > 200 ) THEN
  raise_application_error(-20005, 'Erro de trabalho');
END IF;
END; 

insert into trabalhano values(2,2,100);
drop trigger TRIGGER_INSERIRTRABALHANO
```
3. Faça um gatilho que não permita a inserção de dependentes que não sejam filhos dos empregados.
(FILHO ou FILHA).
```
CREATE OR REPLACE TRIGGER trigger_controlDependentes
before
 INSERT
ON DEPENDENTE
FOR EACH ROW

BEGIN
IF (:NEW.PARENTESCO='filha') THEN
   DBMS_OUTPUT.PUT_LINE('Filha');
ELSIF (:NEW.PARENTESCO='filho') THEN
   DBMS_OUTPUT.PUT_LINE('Filho');
ELSE
  raise_application_error(-20005, 'PARENTESCO INVALIDO');
END IF;
END; 

insert into DEPENDENTE values('santa','F', sysdate, 'MAE',3);
select * from DEPENDENTE;
```

4. Faça um gatilho que não permita a inserção de empregados com idade menor do que 16 anos. 
```
CREATE OR REPLACE TRIGGER trigger_cDepEmpregados
before
 INSERT
ON EMPREGADO
FOR EACH ROW
Declare
v_idade number;

BEGIN
if (trunc(months_between(sysdate, :NEW.DATANASC)/12) < 16) THEN
  raise_application_error(-20005, 'Empregado menor de idade');
END IF;
END; 

insert into Empregado values('9', 'teste trigger', 3500, 'rua das bromélias', 'M', to_date('11/06/2000','dd/mm/yyyy'), 1);
select * from empregado;
```

4. Faça um gatilho que não permita a inserção de empregados com idade menor do que 16 anos. 
```
CREATE OR REPLACE TRIGGER trigger_cDepEmpregados
before
 INSERT
ON EMPREGADO
FOR EACH ROW
Declare
v_idade number;

BEGIN
if (trunc(months_between(sysdate, :NEW.DATANASC)/12) < 16) THEN
  raise_application_error(-20005, 'Empregado menor de idade');
END IF;
END; 

insert into Empregado values('9', 'teste trigger', 3500, 'rua das bromélias', 'M', to_date('11/06/2000','dd/mm/yyyy'), 1);
select * from empregado;
```

5. Faça uma visão que apresente os dados dos empregados (nome, sexo, data de nascimento, nome do
departamento) e os dados dos projetos nos quais eles trabalham (nome do projeto, quantidade de horas
trabalhadas).
```
CREATE OR REPLACE  view view_DadosEmpregados as 
SELECT E.nome AS NOMEeMP, E.sexo AS SEXOeMP, E.DATANASC, D.NOME AS DEPARTAMENTO, P.NOME AS nomeProjeto, T.HRS AS quantidadeHoras
FROM EMPREGADO E, PROJETO P, DEPARTAMENTO D, TRABALHANO T
WHERE E.IDENTEMP = T.IDENTEMP AND
T.PROJNUM = P.PROJNUM AND
P.DEPNUM = D.DEPNUM AND
D.DEPNUM = E.DEPNUM;
```

6. Faça um gatilho que insira informações nas tabelas a partir da visão criada no exercício 5. Lembre-se:
somente insira informações caso os dados ainda não tenham sido cadastrados. 
```

```
