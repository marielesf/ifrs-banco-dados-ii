#### Aula 3 - Exercícios
PDF do exercício - 
http://moodle.canoas.ifrs.edu.br/pluginfile.php/11079/mod_resource/content/3/exercicio_Blocos.pdf

#### Exercício 1 
```
declare 
  nome_dependente EMPREGADO.NOME%type;
begin
  select empregado.nome
  into nome_dependente
  from empregado, dependente
  where empregado.identemp = dependente.identemp
        and dependente.nome = 'Marco';
  dbms_output.put_line('Empregado: ' || nome_dependente);
end;
```
//a. Apresentar o nome do empregado que possui como dependente a pessoa de nome Luana. 

declare
var_nome empregado.nome%type;
var_dep empregado.nome%type;
begin
var_dep := :var_dep;
select empregado.nome 
into var_nome
from empregado inner join dependente
on empregado.IdentEmp = dependente.IdentEmp 
where dependente.nome = var_dep;
dbms_output.put_line('Empregado: ' || var_nome);
end;

//b. Informar o sexo do dependente mais velho do empregado de nome João
declare
var_sexo dependente.sexo%type;
begin
select dependente.sexo
into var_sexo 
from empregado inner join dependente
on empregado.IdentEmp = dependente.IdentEmp 
where empregado.nome = 'Auria'
and dependente.DATANASC = (select min(dependente.DataNasc) 
from dependente, empregado
where empregado.IdentEmp = dependente.IdentEmp and 
empregado.nome = 'Auria');
dbms_output.put_line('Sexo da criança: ' || var_sexo);
end;


//c. Informar se o empregado de nome José possui salário maior do que R$ 1.000,00 (utilizar
IF)
declare
var_salEmp empregado.sal%type;
begin
select sal
into var_salEmp
from empregado  
where nome = 'Maria';
if(var_salEmp > 1.000) then
dbms_output.put_line('Salario maior: '||var_salEmp);
else
dbms_output.put_line('Salario Menor:  '||var_salEmp);
end if;
end;

//d. Buscar a idade do funcionário de nome Maria e apresentar o fatorial dessa idade.
declare
var_dataNascimento empregado.DataNasc%type;
var_idade number(3);
fatorial_Idade number;
begin
fatorial_Idade := 1;
select DataNasc
into var_dataNascimento
from empregado  
where nome = 'Maria';
var_idade := (SYSDATE - var_dataNascimento) /365;
dbms_output.put_line('Idade:' || var_idade);
for i in 1..var_idade loop
fatorial_Idade := fatorial_Idade * i;
end loop;
    dbms_output.put_line('fatorial_Idade:' || fatorial_Idade);
end;



__________________________


CREATE OR REPLACE PROCEDURE Qtd_emp_departamento
    (codigo_departamento IN number)
AS
    quantidade_empregados number;
BEGIN
    select count(identEmp)
    into quantidade_empregados
    from empregado
    where empregado.depnum = codigo_departamento;
    
    dbms_output.put_line(quantidade_empregados);
    
END Qtd_emp_departamento;

begin
  Qtd_emp_departamento(1);
end;


//a. Receber como parâmetro o código de um departamento e informar quantos empregados
estão lotados neste departamento.
CREATE OR REPLACE PROCEDURE Nr_emp
(depart IN Varchar2)
AS
qtd_depart Varchar2(10);
BEGIN
select count(identEmp) into qtd_depart 
from empregado
where  empregado.DepNum  = depart;
dbms_output.put_line('depart:' || depart);
dbms_output.put_line('qtd_depart :' || qtd_depart );
END Nr_emp;

begin
Nr_emp(1);
end;

//b. Receber como parâmetro o código de um empregado e escrever, por extenso, o mês de
nascimento deste empregado, bem como a estação do ano na qual ele nasceu.
CREATE OR REPLACE PROCEDURE calc_signo
(cod_emp IN number)
AS
mes_emp varchar2(10);
nr_mes number(2);
BEGIN
select TO_char(DataNasc,'MONTH') into mes_emp 
from empregado
where  empregado.IdentEmp = cod_emp;
dbms_output.put_line('Codigo empregado:' || cod_emp);
dbms_output.put_line('Mes de nascimento:' || mes_emp);
select TO_char(DataNasc,'MM') into nr_mes 
from empregado
where  empregado.IdentEmp = cod_emp;

if(nr_mes > 1 AND nr_mes <= 3) then
dbms_output.put_line('VERAO');
end if;
if(nr_mes > 4 AND  nr_mes <= 6) then
dbms_output.put_line('OUTONO');
end if;
if(nr_mes > 7 AND  nr_mes <= 9) then
dbms_output.put_line('INVERNO');
end if;
if(nr_mes > 10 AND  nr_mes <= 12) then
dbms_output.put_line('PRIMAVERA');
end if;
END calc_signo;

begin
calc_signo(1);
end;

