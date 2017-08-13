### Aula 2 - exercicios
PDF do exercicio - http://moodle.canoas.ifrs.edu.br/pluginfile.php/10987/mod_resource/content/3/Exerc%C3%ADcios%20de%20Fixa%C3%A7%C3%A3o_Bloco.pdf

#### Exercício 1
```
/*
1) Objetivo: bloco anônimo e tratamento de variáveis
Elabore um  PL/SQL que solicite a entrada de 2 valores. (utilize a opção : para entrada dedados nas variáveis Ex.: v_num1 := :v_num1;). Faça e exiba a multiplicação desses valores
(valor1 * valor2), bem como os 2 valores originais utilizados para realizar o cálculo. Utilize o pacote DBMS_OUTPUT.PUT_LINE para exibir o conteúdo. (Obs.: Para solicitar a leitura de um
valor, utilize, por exemplo: v_numero := :v_numero – isso fará com que o SQLPLUS solicite queo valor seja digitado; não esqueça, também, que se utiliza o || para concatenar valores em uma
string)
*/

declare
numero1 number;
numero2 number;
begin

numero1 := :numero1;
numero2 := :numero2;

dbms_output.put_line('valor 1: ' || numero1 );
dbms_output.put_line('Valor 2: '|| numero2); 
dbms_output.put_line('Multiplicacao: ' ||numero1 * numero2);

end;
----------------------------------
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
#### Exercício 4
```
declare
    username varchar2(90);
begin
    username := :username;
    insert into minha_log (dt_log, ds_log) values (SYSDATE, 'Eu ' || username || ' serei um excelente desenvolvedor PL/SQL ');
end;
```
#### Exercício 5
```
declare
    value_1 number;
    value_2 number;
begin
    value_1 := :value_1;
    value_2 := :value_2;
    insert into minha_log (dt_log, ds_log) values (SYSDATE, TO_CHAR(value_1/value_2));
end;
    
```
#### Exercício 6
```
declare
    dividendo number(1);
    divisor number(1);
begin
    dividendo := :dividendo;
    divisor := :divisor;
    if (divisor > 0) then
        insert into minha_log (dt_log, ds_log) values (SYSDATE, 'O valor da divisão é ' ||dividendo/divisor);
    end if;
end;
```
#### Exercício 7
```
declare
    v_nota1 number(02);
    v_nota2 number(02);
    v_nome varchar2(30);
    v_texto varchar2(300);
begin
    v_nome := 'Yuri Lopes';
    v_nota1 := '9';
    v_nota2 := '7';
    v_texto := 'O aluno ' || v_nome || ' tem a nota média de de : ' || (v_nota1+v_nota2)/2;
    dbms_output.put_line(v_texto);
end;
```
#### Exercício 8
```
null != number
```
#### Exercício 9
```
declare
    segunda number;
    terca number;
    quarta number;
    quinta number;
    sexta number;
    sabado number;
    domingo number;
    media_semanal number;
    cidade varchar2(50);
    mensagem varchar2(500);
begin
    
    cidade := :cidade;
    segunda := :segunda;
    terca := :terca;
    quarta := :quarta;
    quinta := :quinta;
    sexta := :sexta;
    sabado := :sabado;
    domingo := :domingo;
    
    media_semanal := (segunda + terca + quarta + quinta + sexta + sabado + domingo ) / 7;
    
    mensagem := 'Na cidade ' || cidade || ' durante os ultimos 7 dias, a temperatura média registrada foi de ' || media_semanal || '°C';
    insert into minha_log(dt_log, ds_log) values (SYSDATE, mensagem);
    dbms_output.put_line(mensagem);
    
end;
```
