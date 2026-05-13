# Aula 12 — Filtragem Avançada: WHERE e ORDER BY

**Disciplina:** Banco de Dados e Aplicações (IBD951)  
**Professor:** Ronan Adriel Zenatti · ronan.zenatti@cps.sp.gov.br  
**Fatec Jahu — 1º Semestre/2026**

---

## 🎯 Objetivos da Aula

Ao final desta aula você deverá ser capaz de aplicar filtros complexos com `WHERE`; usar operadores de comparação, lógicos e especiais (`BETWEEN`, `IN`, `LIKE`, `IS NULL`); filtrar por datas usando funções nativas; tratar valores nulos com `COALESCE` e `IFNULL`; e ordenar resultados com `ORDER BY`.

---

## 1. A Cláusula WHERE

O `WHERE` é o filtro da consulta. Ele avalia uma condição para cada linha da tabela e retorna apenas as linhas em que a condição é verdadeira.

```sql
-- Operadores de comparação básicos
SELECT * FROM produtos WHERE preco > 100;              -- maior que
SELECT * FROM produtos WHERE estoque = 0;              -- igual a
SELECT * FROM clientes WHERE nome != 'João da Silva';  -- diferente de
SELECT * FROM produtos WHERE preco <= 50;              -- menor ou igual
```

---

## 2. Operadores Lógicos: AND, OR, NOT

```sql
-- AND: ambas as condições devem ser verdadeiras
SELECT * FROM produtos
WHERE preco > 50 AND estoque > 0;

-- OR: pelo menos uma condição deve ser verdadeira
SELECT * FROM pedidos
WHERE status = 'CANCELADO' OR status = 'DEVOLVIDO';

-- NOT: inverte a condição
SELECT * FROM clientes
WHERE NOT nome LIKE 'A%';
```

Quando combinar `AND` e `OR`, use parênteses para garantir a precedência correta. O `AND` tem precedência sobre o `OR`, o que pode gerar resultados inesperados sem os parênteses.

---

## 3. Operadores Especiais

O SQL oferece operadores que tornam filtros comuns mais elegantes e legíveis.

```sql
-- BETWEEN: intervalo inclusivo (equivalente a >= e <=)
SELECT * FROM produtos
WHERE preco BETWEEN 50 AND 200;

SELECT * FROM pedidos
WHERE data_pedido BETWEEN '2026-01-01' AND '2026-06-30';

-- IN: lista de valores permitidos (mais limpo que múltiplos OR)
SELECT * FROM pedidos
WHERE status IN ('CONFIRMADO', 'EM_TRANSITO', 'ENTREGUE');

-- LIKE: busca por padrão em texto
--   % = zero ou mais caracteres
--   _ = exatamente um caractere
SELECT * FROM clientes WHERE nome LIKE 'Ana%';        -- começa com Ana
SELECT * FROM clientes WHERE email LIKE '%@gmail%';   -- contém @gmail
SELECT * FROM produtos WHERE nome LIKE '_ouse%';      -- segundo char: 'o', ex: Mouse

-- IS NULL / IS NOT NULL: verificar valores nulos
SELECT * FROM clientes WHERE email IS NULL;            -- sem e-mail cadastrado
SELECT * FROM matriculas WHERE nota_final IS NOT NULL; -- nota já lançada
```

---

## 4. Filtragem por Datas

Datas exigem atenção especial. O MariaDB armazena datas no formato `AAAA-MM-DD`, mas é comum precisar filtrar por partes da data (mês, ano, dia da semana) ou calcular intervalos. Para isso existem funções nativas de data.

### 4.1 Extraindo Partes da Data

```sql
-- Pedidos feitos no ano de 2026
SELECT * FROM pedidos
WHERE YEAR(data_pedido) = 2026;

-- Pedidos do mês de março, independente do ano
SELECT * FROM pedidos
WHERE MONTH(data_pedido) = 3;

-- Clientes aniversariantes do mês atual
SELECT nome, data_nascimento FROM clientes
WHERE MONTH(data_nascimento) = MONTH(CURDATE());

-- Pedidos realizados hoje
SELECT * FROM pedidos
WHERE DATE(data_pedido) = CURDATE();
```

### 4.2 Calculando Diferença entre Datas

```sql
-- Clientes cadastrados há mais de 365 dias
SELECT nome, data_cadastro FROM clientes
WHERE DATEDIFF(CURDATE(), data_cadastro) > 365;

-- Produtos com validade vencida (data_validade anterior a hoje)
SELECT nome, data_validade FROM produtos
WHERE data_validade < CURDATE();

-- Pedidos com mais de 30 dias sem atualização
SELECT id_pedido, status, data_pedido FROM pedidos
WHERE DATEDIFF(CURDATE(), data_pedido) > 30
  AND status NOT IN ('ENTREGUE', 'CANCELADO');
```

### 4.3 Formatando Datas para Exibição

```sql
-- Exibir data no formato brasileiro (DD/MM/AAAA)
SELECT
    nome,
    DATE_FORMAT(data_nascimento, '%d/%m/%Y') AS "Data de Nascimento"
FROM clientes;

-- Exibir data e hora completa em português
SELECT
    id_pedido,
    DATE_FORMAT(data_pedido, '%d/%m/%Y às %H:%i') AS "Data do Pedido"
FROM pedidos
ORDER BY data_pedido DESC
LIMIT 10;
```

### 4.4 Filtrando por Intervalos Relativos

```sql
-- Pedidos dos últimos 7 dias
SELECT * FROM pedidos
WHERE data_pedido >= DATE_SUB(CURDATE(), INTERVAL 7 DAY);

-- Pedidos dos últimos 3 meses
SELECT * FROM pedidos
WHERE data_pedido >= DATE_SUB(CURDATE(), INTERVAL 3 MONTH);

-- Contratos que vencem nos próximos 30 dias
SELECT * FROM contratos
WHERE data_vencimento BETWEEN CURDATE()
                          AND DATE_ADD(CURDATE(), INTERVAL 30 DAY);
```

> Consulte a referência completa de funções de data e hora na seção 3 do arquivo
> [funcoes_mysql.md](funcoes_mysql.md), que cobre também `TIMESTAMPDIFF`,
> `EXTRACT`, `STR_TO_DATE`, `WEEK` e outras.

---

## 5. Tratamento de Valores Nulos em Filtros

Valores `NULL` têm comportamento especial em SQL: qualquer comparação com `NULL` usando `=` ou `!=` retorna `NULL` (não `TRUE` nem `FALSE`). Por isso existe o operador `IS NULL`.

Além dos filtros, é possível substituir `NULL` por um valor padrão na exibição, usando funções condicionais.

```sql
-- IFNULL: substitui NULL por um valor padrão
SELECT
    nome,
    IFNULL(telefone, 'Não informado') AS "Telefone"
FROM clientes;

-- COALESCE: retorna o primeiro valor não-NULL da lista
-- Útil quando há mais de uma coluna alternativa
SELECT
    nome,
    COALESCE(celular, telefone_fixo, 'Sem contato') AS "Contato"
FROM clientes;

-- Filtrar apenas registros com pelo menos um contato preenchido
SELECT * FROM clientes
WHERE COALESCE(celular, telefone_fixo) IS NOT NULL;
```

> Consulte as funções condicionais (`IF`, `IFNULL`, `COALESCE`, `NULLIF`, `CASE`)
> na seção 5 do arquivo [funcoes_mysql.md](funcoes_mysql.md).

---

## 6. ORDER BY — Ordenando Resultados

```sql
-- Ordenação crescente (padrão, pode omitir ASC)
SELECT nome, preco FROM produtos ORDER BY preco ASC;

-- Ordenação decrescente
SELECT nome, preco FROM produtos ORDER BY preco DESC;

-- Múltiplos critérios: por cidade alfabeticamente, depois por nome
SELECT nome, email, cidade FROM clientes
ORDER BY cidade ASC, nome ASC;

-- Ordenar por data mais recente primeiro
SELECT id_pedido, data_pedido, status FROM pedidos
ORDER BY data_pedido DESC;
```

---

## 7. Exemplo Completo: Relatório de Pedidos Recentes

```sql
SELECT
    p.id_pedido                                          AS "Pedido",
    CONCAT(c.nome, ' ', c.sobrenome)                    AS "Cliente",
    DATE_FORMAT(p.data_pedido, '%d/%m/%Y')              AS "Data",
    DATEDIFF(CURDATE(), p.data_pedido)                  AS "Dias Aguardando",
    IFNULL(p.observacao, 'Sem observação')              AS "Observação",
    p.status                                             AS "Status"
FROM pedidos p
JOIN clientes c ON c.id_cliente = p.id_cliente
WHERE p.status NOT IN ('ENTREGUE', 'CANCELADO')
  AND p.data_pedido >= DATE_SUB(CURDATE(), INTERVAL 90 DAY)
ORDER BY p.data_pedido ASC
LIMIT 20;
```

---

## 📚 Referência de Funções

Para aprofundar nos recursos usados nesta aula, consulte o material de apoio:

**[funcoes_mysql.md](funcoes_mysql.md)**
- Seção 3: Funções de Data e Hora (`NOW`, `CURDATE`, `DATE_FORMAT`, `DATEDIFF`, `DATE_ADD`, `DATE_SUB`, `TIMESTAMPDIFF`, `STR_TO_DATE` e outras)
- Seção 5: Funções Condicionais (`IFNULL`, `COALESCE`, `IF`, `NULLIF`, `CASE`)

---

## 📝 Resumo

O `WHERE` é o motor dos filtros. Operadores de comparação filtram por valor. `AND`/`OR`/`NOT` combinam condições. `BETWEEN` simplifica intervalos, `IN` substitui múltiplos `OR`, `LIKE` permite buscas por padrão e `IS NULL` trata valores ausentes. Funções de data como `YEAR`, `MONTH`, `DATEDIFF`, `DATE_FORMAT` e `DATE_SUB` tornam os filtros temporais muito mais expressivos. `IFNULL` e `COALESCE` substituem nulos por valores padrão sem precisar de estruturas condicionais complexas. O `ORDER BY` garante que os resultados cheguem na ordem correta.

---

## 🔗 Navegação

⬅️ [Aula 11 — Consultas Básicas DQL](Aula_11_Consultas_Basicas_DQL.md) · ➡️ [Aula 13 — Agregação de Dados](Aula_13_Agregacao_Dados.md)

---

*Fatec Jahu · IBD951 · Prof. Ronan Adriel Zenatti · 2026*
