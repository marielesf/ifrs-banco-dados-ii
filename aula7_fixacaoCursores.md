#### Exercícios de Fixação – PL/SQL: http://moodle.canoas.ifrs.edu.br/pluginfile.php/11813/mod_resource/content/3/exercicio_Cursores_B.pdf

select * from CLIENTES;
insert into CLIENTES(id_cliente, nome, endereco, TELEPHONE, data_nasc, rg) 
values (1, 'Marcos', 'Rua bromelias','5199999-8888', to_date('22/05/1995','dd/mm/yyyy'), '0001112223' );
insert into CLIENTES(id_cliente, nome, endereco, TELEPHONE, data_nasc, rg) 
values (2, 'Joana', 'Rua rosas','5199999-7777', to_date('22/05/2001','dd/mm/yyyy'), '0001112224' );

select * from Categorias;
insert into Categorias (id_categoria, nome_categoria) values (10, 'ACAO');
insert into Categorias (id_categoria, nome_categoria) values (11, 'Suspense');
insert into Categorias (id_categoria, nome_categoria) values (12, 'Animação');

select * from Filmes;
insert into Filmes(id_filme,nome, id_categoria) values (10, 'KILL BILL',10);
insert into Filmes(id_filme,nome, id_categoria) values (11, 'Monstros SA',12);
insert into Filmes(id_filme,nome, id_categoria) values (12, 'Alice',12);

select * from Status; // 1 – locada, 2- disponível 
insert into Status (id_status, nome_status) values (1, 'locada');
insert into Status (id_status, nome_status) values (2, 'disponivel');

select * from Fitas;
insert into Fitas (id_fita, id_filme,id_status) values (100, 10, 2);
insert into Fitas (id_fita, id_filme,id_status) values (101, 10, 1);
insert into Fitas (id_fita, id_filme,id_status) values (102, 11, 2);
insert into Fitas (id_fita, id_filme,id_status) values (103, 11, 2);

select * from Locacoes;
insert into Locacoes (id_locacao, id_fita, id_cliente,data_locacao, data_devolucao)
values (5, 100, 1, to_date('22/05/2017','dd/mm/yyyy'), to_date('24/05/2017','dd/mm/yyyy'));
insert into Locacoes (id_locacao, id_fita, id_cliente,data_locacao, data_devolucao)
values (15, 101, 2, to_date('21/09/2017','dd/mm/yyyy'), to_date('28/09/2017','dd/mm/yyyy'));
insert into Locacoes (id_locacao, id_fita, id_cliente,data_locacao, data_devolucao)
values (20, 102, 1, to_date('07/08/2017','dd/mm/yyyy'), to_date('16/08/2017','dd/mm/yyyy'));


```

```
