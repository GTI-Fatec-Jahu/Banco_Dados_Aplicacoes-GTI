# Aula 15 — Junções Externas: LEFT JOIN e RIGHT JOIN

**Disciplina:** Banco de Dados e Aplicações (IBD951)  
**Professor:** Ronan Adriel Zenatti · ronan.zenatti@cps.sp.gov.br  
**Fatec Jahu — 1º Semestre/2026**

---

## 🎯 Objetivos da Aula

Ao final desta aula você deverá ser capaz de compreender a diferença entre INNER e OUTER JOINs; aplicar `LEFT JOIN` e `RIGHT JOIN` em consultas reais; e identificar quando usar cada tipo.

---

## 1. O Problema do INNER JOIN

O `INNER JOIN` é excludente: ele só retorna linhas com correspondência nos dois lados. Mas e se você quiser saber quais clientes **nunca fizeram um pedido**? Ou quais produtos **nunca foram vendidos**? Para esses casos, precisamos de JOINs externos, que **preservam** todos os registros de um dos lados — mesmo sem correspondência do outro.

---

## 2. LEFT JOIN — Preserva o lado esquerdo

O `LEFT JOIN` retorna **todas** as linhas da tabela à esquerda (`FROM`), e as colunas da tabela à direita preenchidas com `NULL` quando não há correspondência.

[Three Venn diagrams side by side comparing INNER JOIN (only intersection), LEFT JOIN (full left circle highlighted plus intersection), and RIGHT JOIN (full right circle highlighted plus intersection). Each labeled in Portuguese. Educational style, blue color scheme, clean design.]

![Comparação entre tipos de JOIN](../imgs/Aula_15_img_01.png)

```sql
-- Todos os clientes, mesmo sem pedidos
SELECT
    c.nome,
    COUNT(p.id_pedido) AS total_pedidos
FROM clientes c
LEFT JOIN pedidos p ON c.id_cliente = p.cliente_id
GROUP BY c.id_cliente, c.nome
ORDER BY total_pedidos ASC;
-- Clientes sem pedidos aparecem com total_pedidos = 0
```

---

## 3. Identificar Registros sem Correspondência

Uma técnica poderosa é usar `LEFT JOIN` com `WHERE tabela_direita.coluna IS NULL` para encontrar registros que **não têm** correspondência — algo impossível com `INNER JOIN`.

```sql
-- Clientes que NUNCA fizeram pedido
SELECT c.nome, c.email
FROM clientes c
LEFT JOIN pedidos p ON c.id_cliente = p.cliente_id
WHERE p.id_pedido IS NULL;

-- Produtos que NUNCA foram vendidos
SELECT pr.nome, pr.preco, pr.estoque
FROM produtos pr
LEFT JOIN itens_pedidos ip ON pr.id_produto = ip.produto_id
WHERE ip.produto_id IS NULL;
```

---

## 4. RIGHT JOIN — Preserva o lado direito

O `RIGHT JOIN` é o espelho do `LEFT JOIN`: preserva todas as linhas da tabela à **direita**. Na prática, a maioria dos desenvolvedores prefere usar sempre `LEFT JOIN` (reordenando as tabelas se necessário), pois é mais fácil de ler da esquerda para a direita.

```sql
-- Equivalente: todos os pedidos, mesmo sem cliente cadastrado (teoricamente impossível com FK)
SELECT c.nome, p.id_pedido, p.data_pedido
FROM clientes c
RIGHT JOIN pedidos p ON c.id_cliente = p.cliente_id;

-- Reescrito com LEFT JOIN (mais legível para a maioria)
SELECT c.nome, p.id_pedido, p.data_pedido
FROM pedidos p
LEFT JOIN clientes c ON p.cliente_id = c.id_cliente;
```

---

## 5. Tabela Comparativa dos JOINs

| Tipo | O que retorna |
|---|---|
| `INNER JOIN` | Apenas linhas com correspondência nos dois lados |
| `LEFT JOIN` | Todas as linhas da esquerda + correspondências da direita (NULL se não houver) |
| `RIGHT JOIN` | Todas as linhas da direita + correspondências da esquerda (NULL se não houver) |
| `FULL OUTER JOIN` | Todas as linhas de ambos os lados (não disponível no MySQL/MariaDB — simular com UNION) |

---

## 📝 Resumo

Os JOINs externos (`LEFT` e `RIGHT`) diferem do `INNER JOIN` ao preservar todos os registros de um dos lados, preenchendo com `NULL` onde não há correspondência. O `LEFT JOIN` com `WHERE direita IS NULL` é uma técnica elegante para encontrar registros "órfãos" — clientes sem pedido, produtos sem venda, etc. Prefira sempre o `LEFT JOIN` por ser mais intuitivo na leitura da esquerda para a direita.

---

## 🔗 Navegação

⬅️ [Aula 14 — Inner Join](Aula_14_Inner_Join.md) · ➡️ [Aula 16 — Introdução ao PL/SQL](Aula_16_Introducao_PLSQL.md)

---

*Fatec Jahu · IBD951 · Prof. Ronan Adriel Zenatti · 2026*
