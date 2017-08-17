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
...
```
