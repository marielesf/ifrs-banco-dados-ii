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
3. Faça um procedimento que receba como parâmetro de entrada o tipo de um produto (código) e imprima:
Nome do Tipo de Produto: XXXX
Produtos desse tipo:
* Nome do Produto: XXXXX – Comercializado em:
- Número da Venda: XXX – Data da Venda: XX/XX/XXXX – Quantidade Vendida: XXXX
- Número da Venda: XXX – Data da Venda: XX/XX/XXXX – Quantidade Vendida: XXXX
...
* Nome do Produto: XXXXX – Comercializado em:
- Número da Venda: XXX – Data da Venda: XX/XX/XXXX – Quantidade Vendida: XXXX
- Número da Venda: XXX – Data da Venda: XX/XX/XXXX – Quantidade Vendida: XXXX
...

```
create or replace procedure dados_produto(CodTipoProd number) as
  
cursor c_produto is SELECT p.DescricaoProd, venda2.NumeroNF, venda2.DATAVENDA, i.QTDEITEM
    FROM produto p, venda2, ItemVenda i
    WHERE p.CodigoTipoProd = CodTipoProd AND
    p.NumeroProd = i.NumeroProd AND 
    i.NumeroNF = venda2.NumeroNF;

  var_cursorProd c_produto%rowtype;
  var_nomeTipoProduto produto.DESCRICAOPROD%type;
begin
select DescricaoTipoProd
 into var_nomeTipoProduto 
 from TipoProd p
 where p.CodigoTipoProd= CodTipoProd ; 
 dbms_output.put_line('Nome do Tipo de Produto: '||var_nomeTipoProduto);

 for var_cursorProd in c_produto loop 
   dbms_output.put_line('*Nome do Produto: ' || var_cursorProd.DescricaoProd ||' – Comercializado em: ');
   dbms_output.put_line('-Número da Venda: '||var_cursorProd.NumeroNF|| ' – Data da Venda:  '||var_cursorProd.DataVenda||' – Quantidade Vendida:'|| var_cursorProd.QtdeItem);
 end loop;
end; 

begin
 dados_produto(21);
end; 
```
4. Faça uma função que receba como parâmetro de entrada o nome de um empregado e retorne o
nome do departamento no qual ele está alocado – imprima essa informação no bloco chamador.
Dentro da função imprima:
Nomes dos demais empregados que trabalham com ele:
XXXXXX
XXXXXX
....
Vendas e datas das vendas que ele realizou:
Venda_XXXXX – Data_XX/XX/XXXX
Venda_XXXXX – Data_XX/XX/XXXX

```
CREATE OR REPLACE FUNCTION function_DepartamentoEmpregado(NOME IN VARCHAR2)
RETURN VARCHAR2 AS
var_nomeDEP VARCHAR2(30);

cursor demais_emp is 
SELECT e.NOMEEMP as outrosEmpregados
    FROM DEPARTAMENTO2  d INNER JOIN empregado2 e ON d.CODDEPT = e.CODDEPT
    WHERE e.NomeEmp = NOME ;

cursor emp_vendas is 
SELECT v.NumeroNF, v.DataVenda as NumeroNF, dataVenda
    FROM EMPREGADO2 E INNER JOIN Venda2 v ON E.CODEMP = v.CODEMP
    WHERE e.NomeEmp = NOME;

  var_cursorEmpDep demais_emp%rowtype;
  cursorempVendas emp_vendas%rowtype;

BEGIN
select d.NomeDepartamento into var_nomeDEP
from Departamento2 d, empregado2 e
where d.CODDEPT= e.CODDEPT and 
e.NomeEmp = NOME;

dbms_output.put_line('Nomes dos demais empregados que trabalham com ele: ');
 for var_cursorEmpDep in demais_emp loop 
   dbms_output.put_line(var_cursorEmpDep.outrosEmpregados);
 end loop;

dbms_output.put_line('Vendas e datas das vendas que ele realizou: ');
 for var_cursorempVendas in emp_vendas loop 
   dbms_output.put_line('Venda_'||var_cursorempVendas.NumeroNF||'–Data_'||var_cursorempVendas.dataVenda);
 end loop;

RETURN var_nomeDEP;
END;

begin
dbms_output.put_line(function_DepartamentoEmpregado('Patricia'));
end function_DepartamentoEmpregado;

select * from Departamento2; 
select * from empregado2; 
select * from venda2;
```
