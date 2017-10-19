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


