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
