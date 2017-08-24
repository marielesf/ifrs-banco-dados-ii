
#### Exemplo
```
create or replace procedure teste_exception(cod number) as nomep varchar(30);
var_exc exception;
begin
if(cod >  3) then
raise var_exc;
else
select nome into nomep
from projeto
where projnum = cod;

dbms_output.put_line(nomep);
end if;
exception
when var_exc then
dbms_output.put_line('Valor maior que o permitido');
--raise_application_error(-200005, 'Valor maior que o permitido');
end;

begin
teste_exception(1);
dbms_output.put_line('finalizado com sucesso');
end;
```

#### 1
```
//Receber como parâmetro o nome de um projeto e informar, na forma de um relatório:
Nome do projeto: XXXXXXXXXXXXXXXXX
Código do projeto: XXXXXXX
Nome do departamento responsável: XXXXXX
Nome do empregado mais velho que trabalha no projeto: XXXXXXX

select * from PROJETO;

create or replace procedure dados_projeto(nomeproj varchar2) as nomep projeto%rowtype;
var_exc exception;
proj projeto%rowtype;

begin
select * into nomep 
from projeto
where nome = nomeproj;

dbms_output.put_line('Nome do projeto: ' || nomep.NOME);
dbms_output.put_line('Código do projeto: ' || nomep.projnum);

exception
when no_data_found then
dbms_output.put_line('Nome nao localizado');
RAISE_APPLICATION_ERROR(-20005, 'Valor maior que o permitido');
end;

begin
dados_projeto('fgfgfg');
dbms_output.put_line('finalizado com sucesso');
end;

begin
dados_projeto('Logica de Programacao');
dbms_output.put_line('finalizado com sucesso');
end;
```

#### 1 - parte 2
```

select * from PROJETO;
select * from empregado;
select * from DEPARTAMENTO;
select * from trabalhano;

create or replace procedure dados_projeto(nomeproj varchar2) as 
var_PROJNUM projeto.ProjNum %type; 
var_ProjNOMe projeto.nome%type; 
var_DepNOME departamento.nome%type; 
var_empNome empregado.nome%type;

begin
dbms_output.put_line('Projeto informado:  ' || nomeproj );

select p.PROJNUM, p.NOME, d.NOME, e.Nome 
into var_PROJNUM, var_ProjNOMe, var_DepNOME, var_empNome

from projeto p, trabalhano t, empregado e, departamento d
where p.nome = nomeproj  and 
p.ProjNum = t.ProjNum and 
t.IdentEmp = e.IdentEmp and 
e.DepNum = d.DepNum and 
e.DATANASC = (select min(e.DATANASC) 
from projeto p, trabalhano t, empregado e
where p.nome = nomeproj and 
p.ProjNum = t.ProjNum and 
t.IdentEmp = e.IdentEmp);

dbms_output.put_line('Nome do projeto: ' || var_ProjNOMe);
dbms_output.put_line('Código do projeto: ' || var_PROJNUM);
dbms_output.put_line('Nome do departamento responsável: ' || var_DepNOME);
dbms_output.put_line('Nome do empregado mais velho que trabalha no projeto: ' || var_empNome);

exception
when no_data_found then
--dbms_output.put_line('Nome nao localizado');
RAISE_APPLICATION_ERROR(-20005, 'errrrrrrrrrrrrrrrrrooooooooooooo');
end;

begin
dados_projeto('fgfgfg');
dbms_output.put_line('finalizado com sucesso');
end;

begin
dados_projeto('Banco de Dados');
dbms_output.put_line('finalizado com sucesso');
end;

begin
dados_projeto('Logica de Programacao');
dbms_output.put_line('finalizado com sucesso');
end;
```

//Receba como parâmetro o nome de um departamento e retorne, na forma de um relatório:
Código do Departamento: XXX
Nome do Departamento: XXXXXXXXXX
Nome do Gerente: XXXXXXX
O Departamento XXXXXX possui XXX empregados lotados nele e XXX projetos sob sua
responsabilidade

select * from PROJETO;
select * from empregado;
select * from DEPARTAMENTO;
select * from trabalhano;

CREATE OR REPLACE PROCEDURE relatorio_departamento(nomedep VARCHAR2) AS
 var_codDepartamento departamento.DEPNUM%type; ;
 var_nomeDepartamento departamento.nome%type; 
 var_nomeGerente departamento.IdentEmpGer%type; 
 var_countEmp number;
 var_countProjeto number;

begin 
 select d.DEPNUM, d.NOME, e.Nome
into var_codDepartamento , var_nomeDepartamento, var_nomeGerente  
from departamento d inner join empregado e
on d.IdentEmpGer = e.IdentEmp 
where 'RH'= d.nome;

dbms_output.put_line('Código do Departamento: ' || var_codDepartamento );
dbms_output.put_line('Nome do Departamento: ' || var_nomeDepartamento);
dbms_output.put_line('Não localizei o projeto');


 exception
   when empreg then
     dbms_output.put_line('Não localizei o empregado');
   when projet then
     dbms_output.put_line('Não localizei o projeto');
END; 




### Patricia
Exercício 4
```
CREATE OR REPLACE PROCEDURE INSERE_TRABALHANO(NOMEE VARCHAR2, NOMEP VARCHAR2, HORAS NUMBER) AS
 V_PROJ NUMBER;
 V_EMP NUMBER;
 empreg exception;
 projet exception;
 contae number;
 contap number;
BEGIN
 
 SELECT count(*)
 INTO contae
 FROM EMPREGADO1
 WHERE NOME = NOMEE;

 if contae = 0 then
  raise empreg;
 end if; 

 SELECT IDENTEMP
 INTO V_EMP
 FROM EMPREGADO1
 WHERE NOME = NOMEE;

 SELECT count(*)
 INTO contap
 FROM PROJETO
 WHERE NOME = NOMEP; 

 if contap = 0 then
  raise projet;
 end if;


 SELECT PROJNUM
 INTO V_PROJ
 FROM PROJETO
 WHERE NOME = NOMEP; 

 INSERT INTO TRABALHANO VALUES(V_EMP, V_PROJ, HORAS);

 exception
   when empreg then
     dbms_output.put_line('Não localizei o empregado');
   when projet then
     dbms_output.put_line('Não localizei o projeto');
END; 
```
