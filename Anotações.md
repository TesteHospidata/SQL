# SQL

## Comandos Basicos

### SELECT
> Serve para selecionar colunas de tabelas
```mysql
select coluna_1, coluna_2, coluna_3
from schema_1.tabela_1
```
>1) Comando usado para selecionar colunas de tabelas
>2) Quando selecionar mais de uma coluna, elas devem ser separadas por vírgula sem conter vírgula antes do comando FROM
>3) Pode-se utilizar o asterisco (*) para selecionar todas as colunas da tabela

### DISTINCT
> Serve para remover linhas duplicadas e mostrar apenas linhas distintas Muito usado na etapa de exploração das bases 
```mysql
select distinct coluna_1, coluna_2, coluna_3
from schema_1.tabela_1
```
>1) Comando usado para remover linhas duplicadas e mostrar apenas linhas distintas
>2) Muito utilizado na etapa de exploração dos dados
>3) Caso mais de uma coluna seja selecionada, o comando SELECT DISTINCT irá retornar todas as combinações distintas.

### WHERE
> Serve para filtrar linhas de acordo com uma condição

```mysql
select coluna_1, coluna_2, coluna_3
from schema_1.tabela_1
where condição_x=true
```
>1) Comando utilizado para filtrar linhas de acordo com uma condição
>2) No PostgreSQL são utilizadas aspas simples para delimitar strings
>3) string = sequência de caracteres = texto
>4) Pode-se combinar mais de uma condição utilizando os operadores lógicos
>5) No PostgreSQL as datas são escritas no formato 'YYYY-MM-DD' ou 'YYYYMMDD'

### ORDER BY
> Serve para ordenar a seleção de acordo com uma regra definida pelo usuário
```mysql
select coluna_1, coluna_2, coluna_3
from schema_1.tabela_1
```

>1) Comando utilizado para ordenar a seleção de acordo com uma regra definida
>2) Por padrão o comando ordena na ordem crescente. Para mudar para a ordem decrescente usar o comando DESC
>3) No caso de strings a ordenação será seguirá a ordem alfabetica

### LIMIT
> Serve para limitar o nº de linhas da consulta. Muito utilizado na etapa de exploração dos dados
```mysql
select coluna_1, coluna_2, coluna_3
from schema_1.tabela_1
```
>1) Comando utilizado para limitar o nº de linhas da consulta.
>2) Muito utilizado na etapa de exploração dos dados
>3) Muito utilizado em conjunto com o comando ORDER BY quando o que importa são os TOP N. Ex: "N pagamentos mais recentes", "N produtos mais caros"

## Operadores

### Aritimédicos
> Servem para executar operações matemáticas Muito utilizados para criar colunas calculadas
```mysql
-- + ADIÇÃO
-- - SUBTRAÇÃO
-- * MULTIPLICAÇÃO
-- / DIVISÃO
-- ^ EXPNENSIAL
-- % MODÚLO
-- || ADIÇÃO DE STINGS --> não é um operador aritmético
```
>1) Servem para executar operações matemáticas
>2) Muito utilizado para criar colunas calculadas
>3) Alias (pseudônimos) são muito utilizados para dar nome as colunas calculadas.
>4) Para criar pseudônimos que contém espaços no nome são utilizadas aspas duplas
>5) No caso de strings o operador de adição (||) irá concatenar as strings
>6) Utilize o Guia de comandos para consultar os operadores utilizados no SQL

### Comparação
>Servem para comparar dois valores retornando TRUE ou FALSE Muito utilizado em conjunto com a função WHERE para filtrar linhas de uma seleção
```mysql
-- = IGUAL
-- > MAIOR QUE
-- < MENOR QUE
-- >= MAIOR OU IGUAL
-- <= MENOR OU IGUAL
-- <> / != DIFERENTE
```
>1) Servem para comparar dois valores retornando TRUE ou FALSE
>2) Muito utilizado em conjunto com a função WHERE para filtrar linhas de uma eleção
>3) Utilizados para criar colunas Flag que retornem TRUE ou FALSE
>4) Utilize o Guia de comandos para consultar os operadores utilizados no SQL

### Logicos
>Usados para unir expressões simples em uma composta
```mysql
-- AND
-- OR
-- NOT
-- BETWEEN
-- IN
-- LIKE
-- ILIKE
-- IS NULL
```
>1) Usados para unir expressões simples em uma composta
>2) AND: Verifica se duas comparações são simultaneamente verdadeiras
>3) OR: Verifica se uma ou outra comparação é verdadeiras
>4) BETWEEN: Verifica quais valores estão dentro do range definido
>5) IN: Funciona como multiplos ORs
>6) LIKE e ILIKE: Comparam textos e são sempre utilizados em conjunto com o perador %, que funciona como um coringa, indicando que qualquer texto pode parecer no lugar do campo
>7) ILIKE ignora se o campo tem letras maiúsculas ou minúsculas na comparação
>8) IS NULL: Verifica se o campo é nulo
>9) Utilize o Guia de comandos para consultar os operadores utilizados no SQL

## Agregação

### Agregadas
>Servem para executar operações aritmética nos registros de uma coluna 

```mysql
-- COUNT()
-- SUM()
-- MIN()
-- MAX()
-- AVG()
```
>1) Servem para executar operações aritmética nos registros de uma coluna
>2) Funções agregadas não computam células vazias (NULL) como zero
>3) Na função COUNT() pode-se utilizar o asterisco (*) para contar os registros
>4) COUNT(DISTINCT ) irá contar apenas os valores exclusivos

### Group by
>Serve para agrupar registros semelhantes de uma coluna Normalmente utilizado em conjunto com as Funções de agregação
```mysql
select state, count(*) as contagem
from sales.customers
group by state
order by contagem desc
```
>1) Serve para agrupar registros semelhantes de uma coluna, 
>2) Normalmente utilizado em conjunto com as Funções de agregação
>3) Pode-se referenciar a coluna a ser agrupada pela sua posição ordinal ex: GROUP BY 1,2,3 irá agrupar pelas 3 primeiras colunas da tabela) 
>4) O GROUP BY sozinho funciona como um DISTINCT, eliminando linhas duplicadas

### Having
```mysql
select 
    state, 
    count(*)
from sales.customers
group by state
having count(*) > 100
```
>1) Tem a mesma função do WHERE mas pode ser usado para filtrar os resultados as funções agregadas enquanto o WHERE possui essa limitação
>2) A função HAVING também pode filtrar colunas não agregadas

## Joins

>Servem para combinar colunas de uma ou mais tabelas

```mysql
select t1.cpf, t1.name, t2.state
from temp_tables.tabela_1 as t1 
left join temp_tables.tabela_2 as t2
	on t1.cpf = t2.cpf
```
>1) Servem para combinar colunas de uma ou mais tabelas
>2) Pode-se chamar todas as colunas com o asterisco (*), mas não é recomendado
>3) É uma boa prática criar aliases para nomear as tabelas utilizadas 
>4) O JOIN sozinho funciona como INNER JOIN

## Union
>Serve para unir tabelas com o mesmo tipo de dados
```mysql
select * from sales.products
union all
select * from temp_tables.products_2
```
>1) Union une 2 tabelas com o os mesmos tipo de dados, sem duplicatas
>2) Union All une as tabelas independente de dados repitidos (prefirivel uso do union all pois o union consome muito processamente por comparar todas as linhas das tabelas)

## Subquery

>Servem para consultar dados de outras consultas.
```mysql
-- Subquery no WHERE
-- Subquery com WITH
-- Subquery no FROM
-- Subquery no SELECT
```

### Subquery no WHERE
```mysql
select *
from sales.products
where price = (select min(price) from sales.products)
```

### Subquery com WITH
```mysql
with alguma_tabela as (
select
	professional_status,
	(current_date - birth_date)/365 as idade
from sales.customers
)

select
	professional_status,
	avg(idade) as idade_media
from alguma_tabela
group by professional_status
```
### Subquery no FROM
```mysql
select
	professional_status,
	avg(idade) as idade_media
from (
		select
			professional_status,
			(current_date - birth_date)/365 as idade
		from sales.customers
	 ) as alguma_tabela
group by professional_status
```
### Subquery no SELECT
```mysql
select
	fun.visit_id,
	fun.visit_page_date,
	sto.store_name,
	(
		select count(*)
		from sales.funnel as fun2
		where fun2.visit_page_date <= fun.visit_page_date
			and fun2.store_id = fun.store_id
	) as visitas_acumuladas
from sales.funnel as fun
left join sales.stores as sto
	on fun.store_id = sto.store_id
order by sto.store_name, fun.visit_page_date
```
>1)Servem para consultar dados de outras consultas.
>2) Para que as subqueries no WHERE e no SELECT funcionem, elas devem retornar penas um único valor
>3) Não é recomendado utilizar subqueries diretamente dentro do FROM pois isso ificulta a legibilidade da query. 

## Tratamento de dados

### conversão de dados
> Serve para converter os tipos de dados
```mysql
-- Operador ::
-- CAST
```

```mysql
select '2021-10-01'::date - '2021-02-01'::date
select cast('2021-10-01' as date) - cast('2021-02-01' as date)
```

>1) O operador :: e o CAST() são funções utilizadas para converter um dado para  unidade desejada. 
>2) O operador :: é mais "clean", porém, em algumas ocasiões não funciona, sendo ecessário utilizar a função CAST()
>3) Use o Guia de comandos para consultar a lista de unidades mais utilizadas

### Geral
>-- CASE WHEN <br>
>-- COALESCE()

```mysql
with faixa_de_renda as (
	select
		income,
		case
			when income < 5000 then '0-5000'
			when income >= 5000 and income < 10000 then '5000-10000'
			when income >= 10000 and income < 15000 then '10000-15000'
			else '15000+'
			end as faixa_renda
	from sales.customers
)

select faixa_renda, count(*)
from faixa_de_renda
group by faixa_renda
```
```mysql
-- Opção 1
select
	*,
	case
		when population is not null then population
		else (select avg(population) from temp_tables.regions)
		end as populacao_ajustada

from temp_tables.regions

-- Opção 2
select
	*,
	coalesce(population, (select avg(population) from temp_tables.regions)) as populacao_ajustada
	
from temp_tables.regions
```

>1) CASE WHEN é o comando utilizado para criar respostas específicas para diferentes condições e é muito utilizado para fazer agrupamento de dados
>2) COALESCE é o comando utilizado para preencher campos nulos com o primeiro valor não nulo de uma sequência de valores.

### Tratamento de texto
```mysql
-- LOWER()
-- UPPER()
-- TRIM()
-- REPLACE()
```

```mysql
select 'São Paulo' = 'SÃO PAULO'
select upper('São Paulo') = 'SÃO PAULO'


select 'São Paulo' = 'são paulo'
select lower('São Paulo') = 'são paulo'


select 'SÃO PAULO     ' = 'SÃO PAULO'
select trim('SÃO PAULO     ') = 'SÃO PAULO'


select 'SAO PAULO' = 'SÃO PAULO'
select replace('SAO PAULO', 'SAO', 'SÃO') = 'SÃO PAULO'
```

>1) LOWER() é utilizado para transformar todo texto em letras minúsculas
>2) UPPER() é utilizado para transformar todo texto em letras maiúsculas
>3) TRIM() é utilizado para remover os espaços das extremidades de um texto
>4) REPLACE() é utilizado para substituir uma string por outra string

### Datas
>-- INTERVAL<br>
>-- DATE_TRUNC<br>
>-- EXTRACT<br>
>-- DATEDIFF<br>

```mysql
select current_date + 10
select (current_date + interval '10 weeks')::date
select (current_date + interval '10 months')::date
select current_date + interval '10 hours'
```
```mysql
select
	date_trunc('month', visit_page_date)::date as visit_page_month,
	count(*)
from sales.funnel
group by visit_page_month
order by visit_page_month desc
```
```mysql
select
	extract('dow' from visit_page_date) as dia_da_semana,
	count(*)
from sales.funnel
group by dia_da_semana
order by dia_da_semana
```
```mysql
select (current_date - '2018-06-01')
select (current_date - '2018-06-01')/7
select (current_date - '2018-06-01')/30
select (current_date - '2018-06-01')/365

select datediff('weeks', '2018-06-01', current_date)
```
>(1) O comando INTERVAL é utilizado para somar datas na unidade desejada. Caso a unidade não seja informada, o SQL irá entender que a soma foi feita em dias.
>(2) O comando DATE_TRUNC é utilizado para truncar uma data no início do período
>(3) O comando EXTRACT é utilizado para extrair unidades de uma data/timestamp
>(4) O cálculo da diferença entre datas com o operador de subtração (-) retorna valores em dias. Para calcular a diferença entre datas em outra unidade é necessário fazer uma transformação de unidades (ou criar uma função para isso)
>(5) Utilize o Guia de comandos para consultar as unidades de data e hora utilizadas no SQL

### Funções
>Servem para criar comandos personalizados de scripts usados recorrentemente.
```mysql
create function datediff(unidade varchar, data_inicial date, data_final date)
returns integer
language sql

as

$$

	select
		case
			when unidade in ('d', 'day', 'days') then (data_final - data_inicial)
			when unidade in ('w', 'week', 'weeks') then (data_final - data_inicial)/7
			when unidade in ('m', 'month', 'months') then (data_final - data_inicial)/30
			when unidade in ('y', 'year', 'years') then (data_final - data_inicial)/365
			end as diferenca

$$
--drop function datediff
```
>1) Para criar funções, utiliza-se o comando CREATE FUNCTION
>2) Para que o comando funcione é obrigatório informar (a) quais as unidades dos INPUTS (b) quais as unidades dos OUTPUTS e (c) em qual linguagem a função será escrita
>3) O script da função deve estar delimitado por $$
>4) Para deletar uma função utiliza-se o comando DROP FUNCTION

## Creaição de tabelas
>-- Criação de tabela a partir de uma query <br>
>-- Criação de tabela a partir do zero<br>
>-- Deleção de tabelas 

### Tabela apartir de uma query
```mysql
select
	customer_id,
	datediff('years', birth_date, current_date) idade_cliente
	into temp_tables.customers_age
from sales.customers

select *
from temp_tables.customers_age
```

### Tabela do zero
```mysql
select distinct professional_status
from sales.customers

create table temp_tables.profissoes (
	professional_status varchar,
	status_profissional varchar
)

insert into temp_tables.profissoes
(professional_status, status_profissional)

values
('freelancer', 'freelancer'),
('retired', 'aposentado(a)'),
('clt', 'clt'),
('self_employed', 'autônomo(a)'),
('other', 'outro'),
('businessman', 'empresário(a)'),
('civil_servant', 'funcionário público(a)'),
('student', 'estudante')

select * from temp_tables.profissoes    
```
>1) Para criar tabelas a partir de uma query basta escrever a query normalmente e colocar o comando INTO antes do FROM informando o schema e o nome da tabela a ser criada
>2) Para criar tabelas a partir do zero é necessário <br>(a) criar uma tabela vazia com o comando CREATE TABLE <br>(b) Informar que valores serão inseridos com o comando INSERT INTO seguido do nome das colunas <br>(c) Inserir os valores manualmente em forma de lista após o comando VALUES
>3) Para deletar uma tabela utiliza-se o comando DROP TABLE


### Deletar tabela
```mysql
drop table temp_tables.profissoes
```

## Linhas
>-- Inserção de linhas
>-- Atualização de linhas
>-- Deleção de linhas

### Inserção
```mysql
create table temp_tables.profissoes (
	professional_status varchar,
	status_profissional varchar
);

insert into temp_tables.profissoes
(professional_status, status_profissional)

values
('freelancer', 'freelancer'),
('retired', 'aposentado(a)'),
('clt', 'clt'),
('self_employed', 'autônomo(a)'),
('other', 'outro'),
('businessman', 'empresário(a)'),
('civil_servant', 'funcionário público(a)'),
('student', 'estudante')

select * from temp_tables.profissoes

insert into temp_tables.profissoes
(professional_status, status_profissional)

values
('unemployed', 'desempregado(a)'),
('trainee', 'estagiario(a)');
```
### Atualização
```mysql
update temp_tables.profissoes
set professional_status = 'intern'
where status_profissional = 'estagiario(a)';

select * from temp_tables.profissoes
```

### Deletação
```mysql
delete from temp_tables.profissoes
where status_profissional = 'desempregado(a)'
or status_profissional = 'estagiario(a)';

select * from temp_tables.profissoes
```
>1) Para inserir linhas em uma tabela é necessário <br>(a) Informar que valores serão inseridos com o comando INSERT INTO seguido do nome da tabela e nome das colunas <br>(b) Inserir os valores manualmente em forma de lista após o comando VALUES
>2) Para atualizar as linhas de uma tabela é necessário (a) Informar qual tabela será atualizada com o comando UPDATE <br>(b) Informar qual o novo valor com o comando SET <br>(c) Delimitar qual linha será modificada utilizando o filtro WHERE
>3) Para deletar linhas de uma tabela é necessário <br>(a) Informar de qual tabela as linhas serão deletadas com o comando DELETE FROM <br>(b) Delimitar qual linha será deletada utilizando o filtro WHERE

## Colunas
>-- Inserção de colunas<br>
>-- Alteração de colunas<br>
>-- Deleção de colunas

### Inserção
```mysql
alter table sales.customers
add customer_age int
```

### Autalização
#### Mudar tipo
```mysql
alter table sales.customers
alter column customer_age type varchar
```
#### Renomear
```mysql
alter table sales.customers
rename column customer_age to age
```
### Deleção
```mysql
alter table sales.customers
drop column age
```

>(1) Para fazer qualquer modificação nas colunas de uma tabela utiliza-se o comando ALTER TABLE seguido do nome da tabela
>(2) Para adicionar colunas utiliza-se o comando ADD seguido do nome da coluna e da unidade da nova coluna
>(3) Para mudar o tipo de unidade de uma coluna utiliza-se o comando ALTER COLUMN 
>(4) Para renomear uma coluna utiliza-se o comando RENAME COLUMN
>(5) Para deletar uma coluna utiliza-se o comando DROP COLUMN
