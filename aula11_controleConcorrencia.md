#### http://moodle.canoas.ifrs.edu.br/pluginfile.php/12229/mod_resource/content/3/Exercicios_concorrencia_Completo.pdf
''''
1
CREATE TABLE t(
    cod int NOT NULL PRIMARY KEY,
    nome varchar(20));
    
insert into t values(3,'tres');

select * from t;

DDL - commmit automatico

2
insert into t values(3,'tres');
 SELECT * FROM t;
 commit;
 SELECT * FROM t;

3
T1
 insert into t values(4,'quatro');
 SELECT * FROM t;
 COMMIT;
T2
 INSERT INTO t values(5,'cinco');
 SELECT * FROM t;

4 - Entram em conflito -> necessario fazer um commmit ou rollback -> Alterar o id
5 - Entram em conflito -> necessario fazer um commmit ou rollback
6 - CREATE TABLE t2(
    a1 int NOT NULL PRIMARY KEY,
    a2 int,
    a3 int);

Erro de SQL: ORA-00060: conflito detectado ao aguardar recurso
00060. 00000 -  "deadlock detected while waiting for resource"
*Cause:    Transactions deadlocked one another while waiting for resources.
*Action:   Look at the trace file to see the transactions and resources
           involved. Retry if necessary.


7 - Erro de SQL: ORA-00060: conflito detectado ao aguardar recurso
00060. 00000 -  "deadlock detected while waiting for resource"
*Cause:    Transactions deadlocked one another while waiting for resources.
*Action:   Look at the trace file to see the transactions and resources
           involved. Retry if necessary.
''''
