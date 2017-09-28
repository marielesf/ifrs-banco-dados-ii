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
 dbms_output.put_line('Detalhamento das vendas: ');

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

2. Crie uma função que receba um código de produto, retorne a quantidade total de itens vendidos
deste produto e imprima um relatório da seguinte forma:
O produto de nome <Nome do produto> foi vendido:
* Pelo empregado de nome <FULANO DE TAL>, no dia XX/XX/XXXX: XX unidades
* Pelo empregado de nome <FULANO DE TAL>, no dia XX/XX/XXXX: XX unidades
O empregado que vendeu mais unidades deste produto é o <FULANO DE TAL>
No bloco chamador imprima:
* Ao todo, já foram vendidas: XX unidades deste produto. 
 
```
CREATE OR REPLACE FUNCTION qtdToalItenVendido(codprod in number)
RETURN NUMBER AS
Var_qtdTotalVendida NUMBER;
BEGIN
select sum(QtdeItem) into Var_qtdTotalVendida
from ItemVenda i, Produto p 
where codprod = p.NumeroProd AND
p.NumeroProd =i.NumeroProd;
return Var_qtdTotalVendida ;
END qtdToalItenVendido;
---------
create or replace procedure empregados_projeto(codprod in number) as
  cursor c_dadosEmp is 
    SELECT e.NomeEmp, v.DataVenda, sum(i.QtdeItem) as qtdVendida 
from empregado2 e INNER JOIN venda2 v ON e.CodEmp = v.CodEmp
INNER JOIN ItemVenda i ON v.NumeroNF = i.NumeroNF
INNER JOIN Produto p ON i.NumeroProd = p.NumeroProd
    WHERE p.NumeroProd = codprod group by e.NomeEmp, v.DataVenda;

var_relatorio c_dadosEmp%rowtype;
var_nomeProduto Produto.DESCRICAOPROD%type;
var_qtdToalItenVendido number;
var_NomeEmp empregado2.NomeEmp%type;
var_qtdSoma number;
begin
 select DESCRICAOPROD
 into var_nomeProduto
 from produto p
 where codprod = p.NumeroProd; 
 dbms_output.put_line('O produto de nome '|| var_nomeProduto ||' foi vendido: ');

 for var_relatorio in c_dadosEmp loop 
   dbms_output.put_line('* Pelo empregado de nome '||var_relatorio.NomeEmp||', no dia '||var_relatorio.DataVenda|| ': '|| var_relatorio.qtdVendida||' unidades');
 end loop;

select e.NomeEmp, sum(i.QtdeItem) into var_NomeEmp, var_qtdSoma  
 from empregado2 e inner join venda2 v on e.CodEmp = v.CodEmp
 inner join ItemVenda i  on v.NumeroNF = i.NumeroNF  
 where i.NumeroProd = codprod 
 group by e.NomeEmp
 having sum(i.QtdeItem) = (select max(sum(QtdeItem)) from empregado2 e inner join venda2 v on e.CodEmp = v.CodEmp
 inner join ItemVenda i  on v.NumeroNF = i.NumeroNF  
 where i.NumeroProd = 10 GROUP BY e.NomeEmp);

 dbms_output.put_line('O empregado que vendeu mais unidades deste produto é o '||var_NomeEmp);
end; 
-----------------
begin
 empregados_projeto(10);
 dbms_output.put_line('Ao todo, já foram vendidas: '||qtdToalItenVendido(10) ||' unidades deste produto.');
end; 
 
```
