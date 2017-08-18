### Aula 3 - Exercícios procedimentos
PDF do Exercício - 
http://moodle.canoas.ifrs.edu.br/pluginfile.php/11221/mod_resource/content/3/exercicio_Blocos_Procedimentos_PARTE1.pdf

### Exercício 1
```
CREATE OR REPLACE PROCEDURE Qtd_emp_departamento
    (codigo_departamento IN number)
AS
    quantidade_empregados number;
BEGIN
    select count(*)
    into quantidade_empregados
    from empregado
    where empregado.depnum = codigo_departamento;
    
    dbms_output.put_line(quantidade_empregados);
    
END Qtd_emp_departamento;

begin
  Qtd_emp_departamento(1);
end;
```

### Exercício 2

```
CREATE OR REPLACE PROCEDURE mes_nascimento_estacao
    (codigo_empregado IN number)
AS
    mes_nascimento varchar2(20);
    estacao varchar2(20);
BEGIN
    select TO_char(empregado.datanasc,'month')
    into mes_nascimento
    from empregado
    where empregado.identemp = codigo_empregado;
    
    dbms_output.put_line(mes_nascimento);
    
END mes_nascimento_estacao;

begin
  mes_nascimento_estacao(1);
end;

```
_____________________________________________________________
### Exercício 1
A. Receber como parâmetro o código de um departamento e informar quantos empregados estão lotados neste departamento.
```
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
```
#### Exercício 2
B. Receber como parâmetro o código de um empregado e escrever, por extenso, o mês de
nascimento deste empregado, bem como a estação do ano na qual ele nasceu.
```
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
```
