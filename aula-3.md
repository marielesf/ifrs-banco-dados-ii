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
#### Exercício 1
A. Apresentar o nome do empregado que possui como dependente a pessoa de nome Luana. 
```
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
```
#### Exercício 2
B. Informar o sexo do dependente mais velho do empregado de nome João
```
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
```
#### Exercício 3
C. Informar se o empregado de nome José possui salário maior do que R$ 1.000,00 (utilizar IF)
```
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
```
#### Exercício 4
D. Buscar a idade do funcionário de nome Maria e apresentar o fatorial dessa idade.
```
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
```
