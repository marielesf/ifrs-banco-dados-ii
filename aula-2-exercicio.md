### Aula 2 - exercicios
PDF do exercicio - http://moodle.canoas.ifrs.edu.br/pluginfile.php/10987/mod_resource/content/3/Exerc%C3%ADcios%20de%20Fixa%C3%A7%C3%A3o_Bloco.pdf

#### Exercício 1
```
declare
   value_1 number;
   value_2 number;
begin
   value_1 := :value_1;
   value_2 := :value_2;
   dbms_output.put_line(value_1 * value_2);
end;
```

#### Exercício 2
```
declare
   value_1 number;
   value_2 number;
   value_3 number;
   biggest_value number;
   lowest_value number;
   condition_1 boolean;
   condition_2 boolean;
begin
   value_1 := :value_1;
   value_2 := :value_2;
   value_3 := :value_3;
   
   condition_1 := (value_1 > value_2 and value_1 > value_3);
   if condition_1 then
      condition_2 := (value_2 > value_3);
      if condition_2 then
         biggest_value := value_1;
         lowest_value := value_3;
      else
         biggest_value := value_1;
         lowest_value := value_2;          
      end if; 
   
    else     
           condition_1 := (value_2 > value_1 and value_2 > value_3);
        if condition_1 then
           condition_2 := (value_1 > value_3);
           if condition_2 then
              biggest_value := value_2;
              lowest_value := value_3;
           else
              biggest_value := value_2;
              lowest_value := value_1;          
           end if;
        else
          condition_2 := (value_2 > value_1);
          if condition_2 then
             biggest_value := value_3;
             lowest_value := value_1;
          else
             biggest_value := value_3;
             lowest_value := value_2; 
          end if;
        end if; 
    end if;
        dbms_output.put_line('Maior: ' ||biggest_value);
        dbms_output.put_line('Menor: ' ||lowest_value);
end;
```
#### Exercício 3
```
create table MINHA_LOG (
  DT_LOG DATE, 
  DS_LOG VARCHAR2(4000)
);
```
