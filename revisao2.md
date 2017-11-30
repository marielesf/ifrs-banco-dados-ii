Otimização: http://moodle.canoas.ifrs.edu.br/pluginfile.php/19394/mod_resource/content/2/exercicio_otimiza%C3%A7%C3%A3o_1_Corre%C3%A7%C3%A3o.pdf

select DISTINCT  c.nome
from clientes c, locacoes l 
where l.ID_CLIENTE = c.ID_CLIENTE;

1. Qual o tempo de execução? (para verificar o tempo exato, execute as
consultas no SQL Developer)
2. Qual o plano de execução?
3. Foram utilizados índices? Se sim, quais? Se não, poderia ser criado algum
índice para otimizar a consulta?
4. Faça a consulta em álgebra relacional equivalente e monte a árvore
algébrica para as consultas que não utilizam funções de agregação.
