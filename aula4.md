
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
