# Anotações

## Tipos de dados

###### Varchar
>Campos de caracteres com tamanho variavel, campo VARCHAR(50), que rebece o nome "Lucas" ocupara o apenas 5 Bytes.

###### Char
>Campo de carecteres de tamanho fixo, um campo CHAR(15), ocupara 15 Bytes mesmo que contenha apenas a letra "M"

###### Int
>Campo de numeros inteiros com até 10 digitos

###### Float
>Campo com numeros de ponto flutuante, pode se definir a precisam de um float ao se declarar a coluna Ex: FLOAT(10,3), neste caso a coluna armazenario 10 digitos no total sendo 3 após a virgula

###### ENUM
>Campo com valores predifinidos (Apenas no MySql) 

## Criação de tabelas

###### Chave primaria
> **PRIMARY KEY** logo a pós o topo do campo, normalmente INT <br>
> **AUTO_INCREMENT** incremente um campo do em 1 a cada novo registro<br>
> **NOT NULL** obriga o preenchimento da coluna<br>
> **UNIQUE** cada valor desta cluna deve ser unico (utilizado para criar referencias 1-1)

## Normalização
> 1. Todo campo vetorizado se torna uma tabela
> 2. Todo campo multivalorado se torna outro tabela quando divisivel
> 3. Todo tabela necessita de pelo menos um campo que identifique todo o registro como sendo unico (PRIMARY KEY).

## FOREIGN KEY 
> **1-1 FK** na tabela mais fraca, a depender da regra de negocio<br>
> **1-N FK** na tabela N

## Conceitos
> **PROJEÇÂO** -> Tudo que é exibido na tela<br>
> **SELEÇÂO** -> Subconjonto do conjunto total de registros filtro pela clausula WHERE<br>
> **JUNÇÂO** -> É a união de 2 tabelas para criar uma progeção, atraves de seleções<BR> 
> **DML** -> DATA MANIPULATION LANGUEGE, toda manipulação de registro na tabela Ex: INSERT, UPDATE, DELETE<br>
> **DDL** -> DATA DEFINITTION LANGUEGE, é a definição dos tipos de dados na tabela como INT, CHAR, etc...<br>
> **DCL** -> DATA CONTROL LANGUEGE<br>
> **TCL** -> TRANSACTION CONTROL LANGUEGE

## Comandos basicos

###### Criar uma base
> CREATE DATABASE [nome_da_base];
```mysql
CREATE DATABASE baseteste;
```

###### Conectar a base
> USE [nome_da_base];
```mysql
USE baseteste;
```

###### Criar tabale
>CREATE TABLE [nome_tabela] ([colunas]);
```mysql
CREATE TABLE clientes (
    ID_CLIENTE INT ,
    Nome VARCHAR(30),
    Sexo CHAR(1),
    Email VARCHAR(30),
    Cpf INT(11),
    Telefone VARCHAR(20),
    Endereco VARCHAR(100),
    Renda_mensal FLOAT(9.2)
);
```
> Uma boa pratica é criar as chaves primarias e estrageiras através de CONSTRAIT 

###### Criar constraint
> ALTER TABLE [tabela] ADD CONSTRAINT [nome_constrait] [regra] (coluna)
```mysql
-- Criando PRIMARY KEY
ALTER TABLE clientes ADD CONSTRAINT PK_CLIENTE 
    PRIMARY KEY (ID_CLIENTE);
-- Tornando auto incrementada
ALTER TABLE clientes MODIFY ID_CLIENTE INT AUTO_INCREMENT;
DESC clientes;
```
```mysql
-- Chave Estrangeira
ALTER TABLE clientes ADD CONSTRAINT FK_CLIENTE_VENDEDOR
    FOREIGN KEY (ID_VENDEDOR) REFERENCES VENDEDORES(IDVENDEDOR);
DESC clientes;
```

###### Inserir registros/tulpas]
> INSERT INTO [tabela] VALUES ([dado_1], [dado_2]...); (Ocultando as colunas, os dados devem ser informandos na mesma ordem das colunas no banco)<br>
> INSERT INTO [tabela] ([coluna_1], [coluna_2]...) VALUES ([dado_1],[dado_2]...); (especificando as colunas)

```mysql
INSERT INTO clientes (Nome, Sexo, Endereco, Telefone, Cpf) VALUES ('Lilian', 'F', 'Alameda verde, 110', '(51) 9 8728-3423', 1234567890);
```

## Dicionario de dados

###### Exibir todas a bases do banco
> SHOW DATABASES
```mysql
SHOW DATABASES;
```

###### Acessar dicionario de dados
>USE information_schema
```mysql
USE information_schema;
```

###### Exibir lista de tabelas
> SHOW TABLES
```mysql
SHOW TABLES;
```

###### Exibir as infromações da tabela
>DESC [tabela];
```mysql
DESC TABLE_CONSTRAINTS;
```

###### Filtrar conntraint's
```mysql
SELECT T.CONSTRAINT_NAME AS NOME, CONSTRAINT_SCHEMA AS BASE, T.TABLE_NAME AS TABELA, T.CONSTRAINT_TYPE AS TIPO
       FROM TABLE_CONSTRAINTS AS T
WHERE CONSTRAINT_SCHEMA = 'COMERCIO';
```

## Projeção de dados

###### Selecionar dados simples
> SELECT [colona_1], [coluna_2]... FROM [tabela];<br>
> SELECT * FROM [tabela] (exibe todas as colunas da tabela)
```mysql
SELECT Nome, Cpf, Sexo FROM clientes;
SELECT * FROM clientes;
```

###### Dar apelido a coluna
> SELECT [colona_1] AS [apelido], [coluna_2]... FROM [tabela];
```mysql
SELECT Nome AS Cliente, Cpf, Sexo FROM clientes;
```

###### Exibir data/hora do sistema
>SELECT NOW(); (pode ser integrado a uma query)
```mysql
SELECT Nome AS Cliente, Cpf, Sexo, NOW() AS Data_Hora FROM clientes;
```

###### Exibir apenas dados ou combinações de distintos
>DISTINCT
```mysql
SELECT DISTINCT Nome AS Cliente, Cpf, Sexo, NOW() AS Data_Hora FROM clientes;
```

## Seleção de dados

###### Fitro de dados estritamente igual
>WHERE [coluna] = [dado_pesquisado]
```mysql
SELECT Nome, Cpf, Endereco FROM clientes
WHERE Sexo = 'F'
```

###### Filtro de dados aproximado 
>WHERE [coluna] LIKE [dado_pesquisado]
```mysql
SELECT Nome, Cpf, Endereco FROM clientes
WHERE Endereco LIKE '%meda%'
```
>  "%" É um operador informa ao banco que podera considerar qualquer coisa antes ou após ele.

###### Condicionais
> AND operador logico, que torna o registro apenas se ambas as condições forem atendidas
```mysql
SELECT Nome, Cpf, Endereco FROM clientes
    WHERE Endereco LIKE '%meda%'
    AND Sexo = 'F'
```

>  OR operador logico, que torna o registro apenas se ao menos uma das condições for atendida
```mysql
SELECT Nome, Cpf, Endereco FROM clientes
    WHERE Endereco LIKE '%meda%'
    OR Sexo = 'M'
```

###### Contar numero de registros retornados em uma seleção
> COUNT([coluna]) (ao passar uma coluna ele ira ignorar os campos com valor null, passando * contara o numero de linhas total)
```mysql
SELECT Sexo, COUNT(*) FROM clientes; -- Este comando retornara erro pois a coluna "Sexo" retornara mutiplas linha e o COUNT apenas uma
```

###### Agrupar dados 
> GROUP BY [coluna] (agrupa todos os dados da coluna, e retorna a primeira linha encontrada)
```mysql
SELECT Sexo, COUNT(*) FROM clientes
    GROUP BY Sexo;
```

###### Ordedando dados
> ORDER BY [coluna] (a coluna tambem pode ser passada pelo seu index)
```mysql
SELECT * FROM clientes
    ORDER BY Sexo;
```
> Podemos usar ASC e DESC para ordenard e forma ascendente e descendete respectivamente;

###### Valor maximo de uma coluna
> MAX([coluna])
```mysql
SELECT MAX(Renda_mensal) FROM clientes;
```

###### Valor minimo de uma coluna
> MIN([coluna])
```mysql
SELECT MIN(Renda_mensal) FROM clientes;
```

###### A media dos valor de uma coluna
> AVG([coluna])
```mysql
SELECT AVG(Renda_mensal) FROM clientes;
```

###### Limitar numero de casas decimais
>TRUCATE([dado],[numero_de_casas])
```mysql
SELECT TRUNCATE(AVG(Renda_mensal),2) FROM clientes;
```

###### Somar todos os valores de uma coluna
> SUM([coluna])
```mysql
SELECT TRUNCATE(SUM(Renda_mensal),2) AS 'Soma Renda Total' FROM clientes;
```

### Dicas de Performançe

> Utiliziando operador OR sempre testar primeiro o campo com mais registros que atendão a condição de VERDADEIRO<br>
> Utiliziando operador AND sempre testar primeiro o campo com mais registros que atendão a condição de FALSO

###### Filtrando valor nulo
> IS NULL trás apenas os valores nulos
```mysql
SELECT Nome, Cpf, Endereco FROM clientes
    WHERE Email IS NULL
```
> IS NOT NULL trás apenas os valores que NÃO são nulos
```mysql
SELECT Nome, Cpf, Endereco FROM clientes
    WHERE Email IS NOT NULL
```

#### Atualizando dados
> UPDATE [tabela] SET [coluna] = [novo_dado] (sempre usar WHERE)
```mysql
UPDATE clientes SET sexo = 'M'
    WHERE nome = 'Lilian'
```

###### Deletando dados
>DELETE excluir registros da tabela (SEMPRE USAR **WHERE**)
```mysql
DELETE FROM clientes
    WHERE Nome = 'Lilian'
```
>Uma boa pratica para cofirmar os dados que deseja excluir é realizar um SELECT com as mesmas clausulas de seleção para confirmar os dados que serão deletados

#### Alterar tabela

###### CHANGE
> ALTER TABLE [tabela] CHANGE [nome_coluna] [novo_nome] [tipo] [propriedades]
```mysql
ALTER TABLE clientes CHANGE Cpf CPF varchar(11) NOT NULL;
DESC clientes;
```

###### MODIFY
> ALTER TABLE [tabela] CHANGE [nome_coluna] [tipo] [propriedades]
```mysql
ALTER TABLE clientes MODIFY CPF varchar(15) NOT NULL;
DESC clientes;
```

###### Adicionar coluna
>ALTER TABLE [tabela] ADD [nova_coluna] [tipo] [propriedade]
```mysql
ALTER TABLE clientes ADD Renda_mensal FLOAT(10.2);
DESC clientes;
```

###### Excluir coluna
>ALTER TABLE [tabela] DROP COLLUM [nova_coluna]
```mysql
ALTER TABLE clientes DROP COLUMN Estado_civil;
DESC clientes;
```

###### Adicionar coluna em uma posição especifica
>ALTER TABLE [tabela] ADD [nova_coluna] [tipo] [propriedade] AFTER [coluna_referencia] (a nova coluna sera inserida imediatamente após a coluna de referencia, para inserir na primeira posição usamos **FIRST**)
```mysql
ALTER TABLE clientes ADD Estado_civil varchar(20) NOT NULL 
    AFTER Email;
DESC clientes;
```

###### Tratando dados nulos
> IFNULL([coluna], [output_predefinido]) verifica se a dados nulos na coluna
```mysql
SELECT *, IFNULL(Email, 'Email não a cadastrado') AS Email FROM clientes
```

###### Criando view
> CREATE VIEW [nome_da_view] AS [select_referenica]
```mysql
CREATE VIEW clintes_mulheres AS 
    SELECT * FROM clientes
    WHERE Sexo = 'F';
```

###### Consultar view
> SELECT [campos] FROM [view]
```mysql
SELECT * FROM clintes_mulheres;
```

###### Deletar view
> DROP VIEW [view]
```mysql
DROP VIEW clintes_mulheres;
```
> Nãoo é posivel fazer o perações de DELETE e INSERT em VIEW's com JOIN em sua criação, Mas é possivel realizar UPDATE

## Procedures

###### Alterar o delimiter
> Acessar o mysql >> usar o comando status >> demitador atual em 'Using Delimiter: ' >> alterar de demitador com delimiter [novo_delimitador]
```mysql
DELIMITER $
```
> Ao encerrar a conecxão o delimitador volta a ser ';'

###### Criar procedure
> CREATE PROCEDURE [nome]([parametros])<br>
> BEGIN<br>
> [bloco_de_codigo]<br>
> END<br>
> [novo_delimitador]
```mysql
CREATE PROCEDURE nome_clientes()
BEGIN 
    SELECT Nome FROM clientes;
end
$
```
```mysql
-- Usando parametros
CREATE PROCEDURE nome_clientes(n1 INT, n2 INT)
BEGIN 
    SELECT n1 + n2 ,Nome FROM clientes;
end
$
```

###### Invocar procedure
> CALL [procedure]
```mysql
CALL nome_clientes();
CALL nome_clientes(10, 12);
```

###### Deletar procedure
> DROP PROCEDURE [procedure]
```mysql
DROP PROCEDURE nome_clientes;
```

## Trigger
> CREATE TRIGGER [nome]<br>
> AFTER/BEFORE [gatilho] (INSERT/UPDATE/DELETE)<br>
> FOR EACH ROW<br>
> BEGIN<br>
> [bloco_de_comando]<br>
> END<br>
> $


```mysql
CREATE TRIGGER Log_cliente
    AFTER UPDATE ON clientes
FOR EACH ROW 
BEGIN
    INSERT INTO BKP_CLIENTE VALUES (NULL ,OLD.Nome, NEW.Nome, NOW());    
END
$;
```
> Para descobrir o usuario que fez a alteração usar SELECTE CURRENT_USER();

## CURSOR
```mysql
CREATE PROCEDURE INSEREDADOS()
BEGIN
		DECLARE FIM INT DEFAULT 0; -- (variavel de controle)
		DECLARE VAR1, VAR2, VAR3, VTOTAL, VMEDIA INT; -- (variaveis para manipular os dados)<br>
		DECLARE VNOME VARCHAR(50);

		DECLARE REG CURSOR FOR( -- (decalra o cursor e pega os dados da tabela)
			SELECT NOME, JAN, FEV, MAR FROM VENDEDORES 
		);

		DECLARE CONTINUE HANDLER FOR NOT FOUND SET FIM = 1; -- (declara um "robo" que ira tranforma a varivel de controle 'FIM' em TRUE ao terminar de ler os registros)

		OPEN REG; -- (envia os registor para memoria RAM)

		REPEAT -- (inicia um loop)

			FETCH REG INTO VNOME, VAR1, VAR2, VAR3; -- (o proximo registro joga nas variaves)
			IF NOT FIM THEN -- (testa se ainda a registros)

				SET VTOTAL = VAR1 + VAR2 + VAR3;
				SET VMEDIA = VTOTAL / 3; -- (realiza os calculos)

				INSERT INTO VEND_TOTAL VALUES(VNOME,VAR1,VAR2,VAR3,VTOTAL,VMEDIA); -- (insere os dados na nova tabela)

			END IF; -- (encerra o condifional)

		UNTIL FIM END REPEAT; -- (Encerra o loop quando os registros acabam)

		CLOSE REG; -- (limpa os registros da memoria ram)
END
$
```
