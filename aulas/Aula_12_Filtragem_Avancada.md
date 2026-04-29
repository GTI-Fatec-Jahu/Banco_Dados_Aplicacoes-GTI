# Aula 12 — Filtragem Avançada: WHERE e ORDER BY

**Disciplina:** Banco de Dados e Aplicações (IBD951)  
**Professor:** Ronan Adriel Zenatti · ronan.zenatti@cps.sp.gov.br  
**Fatec Jahu — 1º Semestre/2026**

---

## 🎯 Objetivos da Aula

Ao final desta aula você deverá ser capaz de aplicar filtros complexos com `WHERE`; usar operadores de comparação, lógicos e especiais (`BETWEEN`, `IN`, `LIKE`, `IS NULL`); e ordenar resultados com `ORDER BY`.

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
SELECT * FROM produto
WHERE preco > 50 AND estoque > 0;

-- OR: pelo menos uma condição deve ser verdadeira
SELECT * FROM pedidos
WHERE status = 'CANCELADO' OR status = 'DEVOLVIDO';

-- NOT: inverte a condição
SELECT * FROM clientes
WHERE NOT nome LIKE 'A%';
```

Quando combinar `AND` e `OR`, use parênteses para garantir a precedência correta — `AND` tem precedência sobre `OR`, o que pode gerar resultados inesperados.

---

## 3. Operadores Especiais

O SQL oferece operadores que tornam filtros comuns mais elegantes e legíveis.

```sql
-- BETWEEN: intervalo inclusivo (equivalente a >= e <=)
SELECT * FROM produto
WHERE preco BETWEEN 50 AND 200;

SELECT * FROM pedidos
WHERE data_pedido BETWEEN '2026-01-01' AND '2026-06-30';

-- IN: lista de valores permitidos (mais limpo que múltiplos OR)
SELECT * FROM pedidos
WHERE status IN ('CONFIRMADO', 'EM_TRANSITO', 'ENTREGUE');

-- LIKE: busca por padrão em texto
--   % = zero ou mais caracteres
--   _ = exatamente um caractere
SELECT * FROM clientes WHERE nome LIKE 'Ana%';       -- começa com Ana
SELECT * FROM clientes WHERE email LIKE '%@gmail%';  -- contém @gmail
SELECT * FROM produtos WHERE nome LIKE '_ouse%';     -- segundo char: 'o', ex: Mouse

-- IS NULL / IS NOT NULL: verificar valores nulos
SELECT * FROM clientes WHERE email IS NULL;           -- sem e-mail cadastrado
SELECT * FROM matriculas WHERE nota_final IS NOT NULL; -- nota já lançada
```

---

## 4. ORDER BY — Ordenando Resultados

```sql
-- Ordenação crescente (padrão, pode omitir ASC)
SELECT nome, preco FROM produto ORDER BY preco ASC;

-- Ordenação decrescente
SELECT nome, preco FROM produto ORDER BY preco DESC;

-- Múltiplos critérios de ordenação
SELECT nome, email FROM clientes
ORDER BY nome ASC;  -- Ordem alfabética por nome
```

---

## 5. Exemplo Completo: Relatório de Produtos

```sql
SELECT
    nome            AS "Produto",
    preco           AS "Preço (R$)",
    estoque         AS "Qtd. em Estoque",
    preco * estoque AS "Valor Total em Estoque"
FROM produtos
WHERE preco BETWEEN 50 AND 500
  AND estoque > 0
  AND nome NOT LIKE '%Usado%'
ORDER BY preco DESC
LIMIT 10;
```

---

## 📝 Resumo

O `WHERE` é o motor dos filtros. Operadores de comparação filtram por valor. `AND`/`OR`/`NOT` combinam condições. `BETWEEN` simplifica intervalos, `IN` substitui múltiplos `OR`, `LIKE` permite buscas por padrão e `IS NULL` trata valores ausentes. O `ORDER BY` garante que os resultados cheguem na ordem correta para relatórios e listagens.

---

## 🔗 Navegação

⬅️ [Aula 11 — Consultas Básicas DQL](Aula_11_Consultas_Basicas_DQL.md) · ➡️ [Aula 13 — Agregação de Dados](Aula_13_Agregacao_Dados.md)

---

*Fatec Jahu · IBD951 · Prof. Ronan Adriel Zenatti · 2026*
