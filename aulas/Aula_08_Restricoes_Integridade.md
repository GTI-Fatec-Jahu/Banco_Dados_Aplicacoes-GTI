# Aula 08 — Restrições de Integridade (Constraints)

**Disciplina:** Banco de Dados e Aplicações (IBD951)  
**Professor:** Ronan Adriel Zenatti · ronan.zenatti@cps.sp.gov.br  
**Fatec Jahu — 1º Semestre/2026**

---

## 🎯 Objetivos da Aula

Ao final desta aula você deverá ser capaz de:
- Compreender o papel das constraints na integridade dos dados
- Aplicar corretamente `PRIMARY KEY`, `FOREIGN KEY`, `UNIQUE`, `NOT NULL` e `CHECK`
- Entender os diferentes tipos de integridade que o SGBD pode garantir

---

## 1. Por que Constraints?

Um banco de dados sem restrições de integridade é como uma cidade sem leis de trânsito: cada programa que acessa os dados pode inserir o que quiser, e o caos é questão de tempo. As **constraints** são regras definidas na própria estrutura do banco que o SGBD faz cumprir automaticamente, independentemente de qual aplicação está inserindo os dados.

[Visual illustration of a database table as a city with traffic rules: some gates labeled 'NOT NULL' stopping empty trucks, a unique checkpoint labeled 'UNIQUE' rejecting duplicate vehicles, a border control labeled 'FOREIGN KEY' verifying that destination exists. Fun metaphor, flat design style, colorful.]

![Constraints como regras de negócio no banco](../imgs/Aula_08_img_01.png)

Isso é fundamental: não podemos confiar apenas na aplicação para validar dados. O banco de dados é a última linha de defesa e deve garantir a consistência por si só.

---

## 2. Tipos de Integridade

Antes de detalhar cada constraint, vale entender os quatro tipos de integridade que elas protegem.

A **integridade de entidade** garante que cada linha de uma tabela seja unicamente identificável — implementada pela `PRIMARY KEY`. A **integridade referencial** garante que uma FK sempre aponte para um valor existente na tabela referenciada — implementada pela `FOREIGN KEY`. A **integridade de domínio** garante que cada coluna contenha apenas valores válidos para aquele campo — implementada por `NOT NULL`, `CHECK` e tipos de dados. E a **integridade definida pelo usuário** abrange regras de negócio específicas — implementada pela `UNIQUE`, `CHECK` e outras.

---

## 3. PRIMARY KEY (PK)

A `PRIMARY KEY` é a constraint mais fundamental. Ela combina automaticamente `NOT NULL` e `UNIQUE`, garantindo que cada linha seja identificável de forma única e que nenhuma linha "anônima" exista na tabela.

```sql
-- PK simples (uma coluna)
CREATE TABLE IF NOT EXISTS departamentos (
    id_departamento  BIGINT UNSIGNED  NOT NULL AUTO_INCREMENT,
    nome             VARCHAR(255)     NOT NULL,
    criado_em        DATETIME         NOT NULL DEFAULT CURRENT_TIMESTAMP,
    atualizado_em    DATETIME         NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
    deletado_em      DATETIME             NULL,
    CONSTRAINT pk_departamento PRIMARY KEY (id_departamento)
);

-- PK composta (duas ou mais colunas)
CREATE TABLE IF NOT EXISTS itens_pedidos (
    pedido_id   BIGINT UNSIGNED  NOT NULL,
    produto_id  BIGINT UNSIGNED  NOT NULL,
    quantidade  INT UNSIGNED     NOT NULL,
    criado_em   DATETIME         NOT NULL DEFAULT CURRENT_TIMESTAMP,
    atualizado_em DATETIME       NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
    deletado_em DATETIME             NULL,
    CONSTRAINT pk_item_pedido PRIMARY KEY (pedido_id, produto_id)
    -- Juntos, pedido_id + produto_id identificam unicamente cada item
);
```

---

## 4. FOREIGN KEY (FK)

A `FOREIGN KEY` implementa a integridade referencial: o valor na coluna FK deve existir como PK na tabela referenciada — ou ser `NULL` (caso a FK aceite nulos, o que indica que o relacionamento é opcional).

```sql
CREATE TABLE IF NOT EXISTS funcionarios (
    id_funcionario   BIGINT UNSIGNED  NOT NULL AUTO_INCREMENT,
    nome             VARCHAR(255)     NOT NULL,
    departamento_id  BIGINT UNSIGNED  NOT NULL,
    criado_em        DATETIME         NOT NULL DEFAULT CURRENT_TIMESTAMP,
    atualizado_em    DATETIME         NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
    deletado_em      DATETIME             NULL,
    CONSTRAINT pk_funcionario  PRIMARY KEY (id_funcionario),
    CONSTRAINT fk_func_depto   FOREIGN KEY (departamento_id)
        REFERENCES departamentos (id_departamento)
        ON DELETE RESTRICT
        ON UPDATE CASCADE
);
```

As **ações referenciais** `ON DELETE` e `ON UPDATE` definem o que acontece quando o registro referenciado é excluído ou atualizado. `RESTRICT` (ou `NO ACTION`) bloqueia a operação se houver registros dependentes. `CASCADE` propaga automaticamente a exclusão ou atualização para os registros filhos. `SET NULL` define as FKs como `NULL` nos registros filhos (só funciona se a coluna aceitar nulos). `SET DEFAULT` define um valor padrão.

---

## 5. UNIQUE

A constraint `UNIQUE` garante que dois registros não possam ter o mesmo valor em uma coluna — diferente da PK, porém, ela aceita `NULL` (e em muitos SGBDs, múltiplos `NULL` são permitidos, pois NULL não é igual a NULL).

```sql
CREATE TABLE IF NOT EXISTS clientes (
    id_cliente    BIGINT UNSIGNED  NOT NULL AUTO_INCREMENT,
    nome          VARCHAR(255)     NOT NULL,
    cpf           CHAR(11)         NOT NULL,
    email         VARCHAR(255)         NULL,
    criado_em     DATETIME         NOT NULL DEFAULT CURRENT_TIMESTAMP,
    atualizado_em DATETIME         NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
    deletado_em   DATETIME             NULL,
    CONSTRAINT pk_cliente   PRIMARY KEY (id_cliente),
    CONSTRAINT uq_cpf       UNIQUE (cpf),
    CONSTRAINT uq_email     UNIQUE (email)
);
```

`UNIQUE` também pode ser aplicado a combinações de colunas. Por exemplo, em uma tabela de matrículas, podemos garantir que um aluno não se matricule duas vezes na mesma disciplina no mesmo semestre:

```sql
CONSTRAINT uq_matricula UNIQUE (id_aluno, id_disciplina, semestre)
```

---

## 6. NOT NULL

O `NOT NULL` é o mais simples e o mais esquecido. Ele garante que uma coluna sempre terá um valor — nunca `NULL`. Deve ser aplicado a todas as colunas que são obrigatórias no negócio.

```sql
nome    VARCHAR(100)  NOT NULL,  -- Nome é sempre obrigatório
email   VARCHAR(150),            -- Email é opcional (pode ser NULL)
```

---

## 7. CHECK

A constraint `CHECK` permite definir uma condição booleana que cada linha deve satisfazer. É a forma de implementar regras de domínio diretamente no banco.

```sql
CREATE TABLE IF NOT EXISTS produtos (
    id_produto    BIGINT UNSIGNED  NOT NULL AUTO_INCREMENT,
    nome          VARCHAR(255)     NOT NULL,
    preco         DECIMAL(10, 2)   NOT NULL,
    estoque       INT              NOT NULL DEFAULT 0,
    criado_em     DATETIME         NOT NULL DEFAULT CURRENT_TIMESTAMP,
    atualizado_em DATETIME         NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
    deletado_em   DATETIME             NULL,
    CONSTRAINT pk_produto   PRIMARY KEY (id_produto),
    CONSTRAINT chk_preco    CHECK (preco > 0),
    CONSTRAINT chk_estoque  CHECK (estoque >= 0)
);
```

---

## 8. DEFAULT

O `DEFAULT` não é tecnicamente uma constraint de integridade, mas complementa as demais ao definir um valor automático para uma coluna quando nenhum valor é informado no `INSERT`. Isso evita `NULL` indesejados em colunas que têm um valor padrão razoável.

```sql
status        VARCHAR(20)  NOT NULL DEFAULT 'ativo',
criado_em     DATETIME     NOT NULL DEFAULT CURRENT_TIMESTAMP,
ativo         TINYINT(1)   NOT NULL DEFAULT 1
```

---

## 9. Exemplo Completo

```sql
CREATE TABLE IF NOT EXISTS matriculas (
    id_matricula    BIGINT UNSIGNED  NOT NULL AUTO_INCREMENT,
    aluno_id        BIGINT UNSIGNED  NOT NULL,
    disciplina_id   BIGINT UNSIGNED  NOT NULL,
    semestre        CHAR(6)          NOT NULL,
    nota_final      DECIMAL(4, 2)        NULL,
    situacao        VARCHAR(20)      NOT NULL DEFAULT 'cursando',
    data_matricula  DATE             NOT NULL DEFAULT (CURRENT_DATE),
    criado_em       DATETIME         NOT NULL DEFAULT CURRENT_TIMESTAMP,
    atualizado_em   DATETIME         NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
    deletado_em     DATETIME             NULL,

    CONSTRAINT pk_matricula  PRIMARY KEY (id_matricula),
    CONSTRAINT uq_mat        UNIQUE (aluno_id, disciplina_id, semestre),
    CONSTRAINT fk_mat_aluno  FOREIGN KEY (aluno_id)
                                 REFERENCES alunos (id_aluno) ON DELETE RESTRICT,
    CONSTRAINT fk_mat_disc   FOREIGN KEY (disciplina_id)
                                 REFERENCES disciplinas (id_disciplina) ON DELETE RESTRICT,
    CONSTRAINT chk_nota      CHECK (nota_final BETWEEN 0 AND 10),
    CONSTRAINT chk_situacao  CHECK (situacao IN ('cursando', 'aprovado', 'reprovado', 'trancado'))
);
```

---

## 📝 Resumo

As constraints são as guardiãs da integridade do banco de dados. `PRIMARY KEY` garante unicidade e identificabilidade. `FOREIGN KEY` garante que referências sejam válidas. `UNIQUE` impede duplicatas em colunas não-PK. `NOT NULL` garante que campos obrigatórios sempre tenham valor. `CHECK` valida regras de domínio. Usadas em conjunto, elas tornam o banco de dados autodefensivo contra dados inválidos ou inconsistentes.

---

## 🔗 Navegação

⬅️ [Aula 07 — SQL DDL](Aula_07_SQL_DDL.md) · ➡️ [Aula 09 — Avaliação Bimestral P1](Aula_09_Avaliacao_P1.md)

---

*Fatec Jahu · IBD951 · Prof. Ronan Adriel Zenatti · 2026*
