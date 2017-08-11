### Aula - 2 (10/08/2017)
#### Exemplo 1:

```
declare
	teste boolean;
begin
	teste := true;
	if teste then
		dbms_output.put_line('maior que minimo');   
	end if;
end;
```

#### Exemplo if
```
if horas > 40 then
   extra := horas – 40;
else
   extra := 0;
end if;
```

#### Exemplo while
```
while cont < 10 loop
      cont := cont + 1;
      dbms_output.put_line('Contador:' || cont);
end loop;
```

#### Exemplo for
```
for i in 1..10 loop
    dbms_output.put_line('Contador:' || cont);
end loop;
```

#### Obs.:
Da pra usar o comando exit pra sair do loop.

http://dontpad.com/happy
