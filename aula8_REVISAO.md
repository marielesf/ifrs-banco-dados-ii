#### Exercicos de revisão prova: http://moodle.canoas.ifrs.edu.br/pluginfile.php/41164/mod_resource/content/1/Exercicios_Revisao_Procedimentos_Fun%C3%A7%C3%B5es_Cursores_VENDA.pdf

1. Crie um procedimento que receba o código de um empregado como parâmetro e gere um
relatório da seguinte forma:
Relatório de vendas do empregado <FULANO DE TAL>
---------------------------------------------------------------------------
Detalhamento das vendas:
* <NOTA FISCAL X> - Valor total da nota: R$ XXX,XX
* <NOTA FISCAL X> - Valor total da nota: R$ XXX,XX
...........
---------------------------------------------------------------------------
Total de vendas realizadas pelo empregado: R$ XXXXX,XX


```
create or replace procedure relatorioVenda(cod number) as 

 cursor c_detalheVendas is 
  SELECT v.NumeroNF, SUM(i.PrecoItem * i.QtdeItem) as soma
  FROM ItemVenda i, venda2 v
  WHERE v.CodEmp = cod and
v.NumeroNF = i.NumeroNF 
group by v.NumeroNF;

  var_totalNotas c_detalheVendas%rowtype;
  var_nomeEmp empregado2.NOMEEMP%type;
var_somatorioTotal number;
begin
 select NOMEEMP
 into var_nomeEmp 
 from empregado2
 where CODEMP = cod; 
 dbms_output.put_line('Relatório de vendas do empregado '|| var_nomeEmp);
 dbms_output.put_line('----------------------------- ');

 for var_totalNotas in c_detalheVendas loop 
   dbms_output.put_line('* ' || var_totalNotas.NumeroNF ||' - Valor total da nota: R$ '||var_totalNotas.soma);
 end loop;
 dbms_output.put_line('----------------------------- ');

SELECT SUM(i.PrecoItem * i.QtdeItem) 
into var_somatorioTotal 
  FROM ItemVenda i, venda2 v
  WHERE v.CodEmp = cod and
v.NumeroNF = i.NumeroNF; 
 dbms_output.put_line('Relatório de vendas do empregado '|| var_somatorioTotal );
end; 

begin
 relatorioVenda(1);
end; 
```


