Dia 31/08/2017

Revisão lista procedimentos 1: 

f) Receber como parâmetro o nome de um departamento e retornar a média 
salarial dos empregados deste departamento em um parâmetro de saída. 
Mostre o resultado no bloco chamador.

CREATE OR REPLACE PROCEDURE RETORNA_MEDIA(NOMEDEP IN VARCHAR2, MEDIA OUT NUMBER)
AS
BEGIN
  SELECT AVG(SAL)
  INTO MEDIA
  FROM EMPREGADO1 INNER JOIN DEPARTAMENTO1 
       ON EMPREGADO1.DEPNUM = DEPARTAMENTO1.DEPNUM
  WHERE DEPARTAMENTO1.NOME = NOMEDEP;
END; 

DECLARE
 MEDIA_VAR NUMBER;
BEGIN
 RETORNA_MEDIA('RH',MEDIA_VAR);
 DBMS_OUTPUT.PUT_LINE('A media salarial do departamento é: '|| MEDIA_VAR);
END; 
