# T2 — Atividade Prática: SQL

**Disciplina:** Banco de Dados e Aplicações (IBD951)  
**Professor:** Ronan Adriel Zenatti · ronan.zenatti@cps.sp.gov.br  
**Fatec Jahu — 1º Semestre/2026**

---

## 📋 Descrição

Esta atividade avalia a competência técnica em manipulação de dados, integrando Joins, Filtros e Agregações. Você trabalhará sobre um banco de dados de uma livraria já estruturado.

---

## 🗄️ Banco de Dados: Livraria Cultural

O banco de dados da Livraria Cultural possui a seguinte estrutura:

```sql
-- Execute este script para criar e popular o banco de dados da atividade
CREATE DATABASE IF NOT EXISTS livraria_cultural;
USE livraria_cultural;

CREATE TABLE autor (
    id_autor    INT NOT NULL AUTO_INCREMENT,
    nome        VARCHAR(120) NOT NULL,
    pais_origem VARCHAR(60),
    CONSTRAINT pk_autor PRIMARY KEY (id_autor)
);

CREATE TABLE livro (
    id_livro    INT NOT NULL AUTO_INCREMENT,
    titulo      VARCHAR(200) NOT NULL,
    isbn        CHAR(13)     NOT NULL,
    ano         INT,
    preco       DECIMAL(8,2) NOT NULL,
    estoque     INT NOT NULL DEFAULT 0,
    id_autor    INT NOT NULL,
    CONSTRAINT pk_livro   PRIMARY KEY (id_livro),
    CONSTRAINT uq_isbn    UNIQUE (isbn),
    CONSTRAINT fk_livro_autor FOREIGN KEY (id_autor) REFERENCES autor(id_autor)
);

CREATE TABLE cliente (
    id_cliente  INT NOT NULL AUTO_INCREMENT,
    nome        VARCHAR(120) NOT NULL,
    email       VARCHAR(150),
    cidade      VARCHAR(80),
    CONSTRAINT pk_cliente PRIMARY KEY (id_cliente)
);

CREATE TABLE venda (
    id_venda    INT NOT NULL AUTO_INCREMENT,
    data_venda  DATE NOT NULL,
    id_cliente  INT NOT NULL,
    CONSTRAINT pk_venda PRIMARY KEY (id_venda),
    CONSTRAINT fk_venda_cliente FOREIGN KEY (id_cliente) REFERENCES cliente(id_cliente)
);

CREATE TABLE item_venda (
    id_venda        INT NOT NULL,
    id_livro        INT NOT NULL,
    quantidade      INT NOT NULL,
    preco_venda     DECIMAL(8,2) NOT NULL,
    CONSTRAINT pk_item_venda PRIMARY KEY (id_venda, id_livro),
    CONSTRAINT fk_iv_venda FOREIGN KEY (id_venda) REFERENCES venda(id_venda),
    CONSTRAINT fk_iv_livro FOREIGN KEY (id_livro) REFERENCES livro(id_livro)
);
```

---

## 📝 Questões

Escreva o comando SQL para cada uma das questões a seguir. Inclua o resultado esperado (ou um exemplo de resultado) como comentário SQL.

**Questão 1 — DML:** Insira 3 autores, 5 livros, 3 clientes e realize ao menos 2 vendas completas com seus itens. Respeite a ordem de inserção das dependências de FK.

**Questão 2 — DQL com filtro:** Liste todos os livros cujo preço esteja entre R$ 30,00 e R$ 80,00, ordenados pelo preço de forma crescente. Mostre o título, o preço e o estoque.

**Questão 3 — LIKE e IS NULL:** Liste os clientes cujo nome começa com a letra "M" ou que não têm e-mail cadastrado.

**Questão 4 — Agregação simples:** Mostre a quantidade de livros cadastrados por autor, incluindo o nome do autor. Ordene do autor com mais livros para o com menos.

**Questão 5 — GROUP BY com HAVING:** Liste os autores que têm mais de 1 livro cadastrado com preço acima de R$ 50,00.

**Questão 6 — INNER JOIN:** Liste as vendas realizadas, mostrando a data da venda, o nome do cliente, o título do livro vendido, a quantidade e o valor total de cada item (quantidade × preço).

**Questão 7 — LEFT JOIN:** Liste todos os clientes, exibindo o nome do cliente e a quantidade de vendas que realizou. Inclua os clientes que nunca compraram (resultado = 0).

**Questão 8 — Consulta complexa:** Apresente um relatório com o nome do autor, o título do livro e o total arrecadado com as vendas desse livro, considerando todos os itens vendidos. Ordene pelo total arrecadado de forma decrescente. Mostre apenas os livros que geraram mais de R$ 100,00 em vendas.

---

## 📊 Critérios de Avaliação

| Critério | Peso |
|---|---|
| DML correto com respeito às FKs (Questão 1) | 15% |
| Consultas com WHERE e operadores especiais (Questões 2 e 3) | 20% |
| Agregação com GROUP BY e HAVING (Questões 4 e 5) | 25% |
| JOINs (Questões 6 e 7) | 25% |
| Consulta complexa integrada (Questão 8) | 15% |

---

## 📅 Entrega

Arquivo `.sql` com todos os comandos, incluindo o script de criação do banco e os dados de teste (Questão 1). Entregue conforme orientação do professor.

---

*Fatec Jahu · IBD951 · Prof. Ronan Adriel Zenatti · 2026*
