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
