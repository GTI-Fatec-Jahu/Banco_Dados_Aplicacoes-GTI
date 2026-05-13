# Aula 11 — Consultas Básicas (DQL): SELECT

**Disciplina:** Banco de Dados e Aplicações (IBD951)  
**Professor:** Ronan Adriel Zenatti · ronan.zenatti@cps.sp.gov.br  
**Fatec Jahu — 1º Semestre/2026**

---

## 🎯 Objetivos da Aula

Ao final desta aula você deverá ser capaz de escrever consultas básicas com `SELECT`; utilizar projeção de colunas, alias e expressões; aplicar funções de string e formatação em consultas; e compreender a estrutura e a ordem de execução de uma consulta SQL.

---

## 1. O Comando SELECT

O `SELECT` é o comando mais poderoso e mais usado da SQL. Ele recupera dados armazenados nas tabelas e nos permite escolher exatamente quais colunas ver, quais linhas filtrar, como ordenar os resultados e muito mais.

```mermaid
flowchart LR
    SELECT["SELECT
(quais colunas?)"] --> FROM["FROM
(qual tabela?)"]
    FROM --> WHERE["WHERE
(quais linhas?)"]
    WHERE --> ORDER["ORDER BY
(qual ordem?)"]
```

---

## 2. Estrutura Básica

```sql
-- Selecionar todas as colunas de uma tabela
SELECT * FROM clientes;

-- Projeção: selecionar apenas as colunas desejadas
SELECT nome, email FROM clientes;

-- Alias: renomear colunas no resultado
SELECT
    nome             AS "Nome do Cliente",
    email            AS "E-mail",
    data_nascimento  AS "Data de Nascimento"
FROM clientes;
```

O asterisco `*` seleciona todas as colunas, mas em sistemas reais prefira especificar as colunas que você precisa — isso melhora o desempenho e a legibilidade.

---

## 3. Expressões e Colunas Calculadas

O `SELECT` pode calcular valores diretamente na consulta, sem precisar armazená-los no banco:

```sql
SELECT
    nome,
    preco                                    AS "Preço Original",
    preco * 0.9                              AS "Preço com 10% de Desconto",
    preco * 1.1                              AS "Preço com 10% de Acréscimo",
    CONCAT('R$ ', FORMAT(preco, 2, 'pt_BR')) AS "Preço Formatado"
FROM produtos;
```

---

## 4. Funções de String em Consultas

Funções de string permitem formatar, combinar e transformar texto diretamente no resultado da consulta, sem alterar os dados armazenados.

### 4.1 CONCAT e CONCAT_WS — Combinando Colunas

```sql
-- Montar o nome completo do cliente a partir de duas colunas
SELECT
    CONCAT(nome, ' ', sobrenome)              AS "Nome Completo",
    CONCAT('(', ddd, ') ', telefone)          AS "Telefone Formatado"
FROM clientes;

-- CONCAT_WS usa um separador e ignora valores NULL automaticamente
-- Útil quando parte do endereço pode estar ausente
SELECT
    CONCAT_WS(', ', logradouro, numero, complemento, bairro) AS "Endereço Completo"
FROM enderecos;
```

> `CONCAT_WS` é preferível a `CONCAT` quando alguma coluna pode ser `NULL`,
> pois ele omite o separador ao redor do valor nulo em vez de anular
> todo o resultado.

### 4.2 UPPER, LOWER e FORMAT — Padronização e Formatação

```sql
-- Padronizar exibição de status em maiúsculas
SELECT
    id_pedido,
    UPPER(status)                        AS "Status",
    FORMAT(valor_total, 2, 'pt_BR')      AS "Valor Total"
FROM pedidos;

-- Gerar e-mail institucional a partir do nome do aluno
SELECT
    LOWER(CONCAT(nome, '.', sobrenome, '@fatec.sp.gov.br')) AS "E-mail Institucional"
FROM alunos;
```

### 4.3 SUBSTRING e LEFT/RIGHT — Extraindo Partes do Texto

```sql
-- Exibir apenas os 3 primeiros caracteres do código de produto
SELECT
    LEFT(codigo, 3)                   AS "Categoria",
    nome,
    preco
FROM produtos;

-- Extrair o domínio do e-mail (tudo após o @)
SELECT
    email,
    SUBSTRING(email, INSTR(email, '@') + 1) AS "Domínio"
FROM clientes;
```

### 4.4 TRIM e REPLACE — Limpeza de Dados

```sql
-- Remover espaços extras que podem ter sido digitados por engano
SELECT TRIM(nome) AS nome_limpo FROM clientes;

-- Substituir ponto por hífen no CPF para exibição
SELECT REPLACE(cpf, '.', '-') AS cpf_formatado FROM clientes;
```

> Consulte a referência completa de funções de string na seção 1 do arquivo
> [funcoes_mysql.md](funcoes_mysql.md).

---

## 5. DISTINCT — Eliminando Duplicatas

O `DISTINCT` remove linhas duplicadas do resultado:

```sql
-- Quais status de pedido distintos existem?
SELECT DISTINCT status FROM pedidos;

-- Quais domínios de e-mail os clientes usam?
SELECT DISTINCT SUBSTRING(email, INSTR(email, '@') + 1) AS dominio
FROM clientes
ORDER BY dominio;
```

---

## 6. LIMIT — Controlando a Quantidade de Linhas

```sql
-- Retornar apenas os 5 primeiros produtos
SELECT * FROM produtos LIMIT 5;

-- Paginação: pular os primeiros 10 e mostrar os próximos 5
SELECT * FROM produtos LIMIT 5 OFFSET 10;
```

---

## 7. Ordem de Execução da Query

A ordem em que **escrevemos** a query não é a ordem em que o banco a **executa**. Internamente, a execução segue esta sequência:

```mermaid
flowchart LR
    F["1. FROM
Identifica as tabelas"] --> W["2. WHERE
Filtra as linhas"]
    W --> G["3. GROUP BY
Agrega os grupos"]
    G --> H["4. HAVING
Filtra os grupos"]
    H --> S["5. SELECT
Projeta as colunas"]
    S --> O["6. ORDER BY
Ordena o resultado"]
    O --> L["7. LIMIT
Limita as linhas"]
```

Entender essa ordem ajuda a evitar erros comuns. Por exemplo: tentar usar um alias definido no `SELECT` dentro do `WHERE` não funciona porque o `WHERE` é executado antes do `SELECT`.

---

## 8. Exemplo Completo

```sql
SELECT
    CONCAT(c.nome, ' ', c.sobrenome)         AS "Cliente",
    UPPER(c.cidade)                           AS "Cidade",
    CONCAT('(', c.ddd, ') ', c.telefone)     AS "Contato",
    FORMAT(SUM(p.valor_total), 2, 'pt_BR')   AS "Total Gasto (R$)"
FROM clientes c
JOIN pedidos p ON p.id_cliente = c.id_cliente
GROUP BY c.id_cliente, c.nome, c.sobrenome, c.cidade, c.ddd, c.telefone
ORDER BY SUM(p.valor_total) DESC
LIMIT 10;
```

---

## 📚 Referência de Funções

Para conhecer todas as funções de string (`CONCAT`, `REPLACE`, `LPAD`, `GROUP_CONCAT` e outras), numéricas e de conversão disponíveis no MariaDB, consulte o material de apoio:

**[funcoes_mysql.md](funcoes_mysql.md)** — seções 1 (String), 2 (Numéricas) e 6 (Conversão de Tipo)

---

## 📝 Resumo

O `SELECT` é a base da recuperação de dados. A projeção de colunas melhora desempenho e clareza. Funções de string como `CONCAT`, `UPPER`, `TRIM` e `FORMAT` enriquecem o resultado sem alterar os dados armazenados. `DISTINCT` filtra duplicatas e `LIMIT` controla o volume de resultados. Compreender a ordem de execução interna da query é fundamental para escrever SQL correto e eficiente.

---

## 🔗 Navegação

⬅️ [Aula 10 — SQL DML](Aula_10_SQL_DML.md) · ➡️ [Aula 12 — Filtragem Avançada](Aula_12_Filtragem_Avancada.md)

---

*Fatec Jahu · IBD951 · Prof. Ronan Adriel Zenatti · 2026*
