# Aula 09 — Avaliação Bimestral P1

**Disciplina:** Banco de Dados e Aplicações (IBD951)  
**Professor:** Ronan Adriel Zenatti · ronan.zenatti@cps.sp.gov.br  
**Fatec Jahu — 1º Semestre/2026**

---

## 📋 Sobre esta Avaliação

Esta avaliação cobre todo o conteúdo do primeiro bloco da disciplina: modelagem conceitual, modelo lógico relacional, normalização e implementação com DDL. Você deve ler o enunciado com atenção, identificar as entidades, os relacionamentos e as regras de negócio, e entregar um script SQL completo e funcional.

**Valor:** 3,0 pontos (30% da nota final)  
**Entrega:** Google Classroom — atividade "Avaliação P1"  
**Formato:** um único arquivo `.sql` nomeado como `NomeCompleto_P1.sql`  
**Exemplo:** `MariaOliveira_P1.sql`

> O arquivo deve executar do início ao fim sem erros. Use comentários SQL para identificar cada tabela criada e para registrar decisões de modelagem que julgar importante justificar.

---

## 🐾 Enunciado — Pet Shop Patas & Cia

Você foi até o pet shop "Patas & Cia" conversar com a dona, a Simone. Ela quer informatizar o negócio e foi explicando como as coisas funcionam por lá:

*"Aqui a gente atende os pets dos clientes. Cada cliente tem nome, CPF e telefone. Um cliente pode ter mais de um pet — tem gente que traz cachorro, gato e passarinho — então preciso guardar os pets separado. De cada pet eu quero saber o nome, a espécie e a data de nascimento.*

*Meus funcionários são de dois tipos: os veterinários e os tosadores. Todo funcionário tem nome, CPF e data de contratação. Mas o veterinário tem o número do CRMV dele, e o tosador tem a especialidade — se é só banho, banho e tosa, tosa artística, esse tipo de coisa.*

*Quando o pet chega aqui, a gente abre um agendamento. O agendamento tem a data e hora marcada, o pet que vai ser atendido e um status — pode ser Agendado, Em Atendimento, Concluído ou Cancelado. Agora, tem um detalhe: eu preciso saber quem foi que registrou o agendamento no sistema e quem vai de fato atender o pet. Às vezes a Joana abre o agendamento, mas quem vai atender é o Dr. Fábio. São dois funcionários diferentes na mesma ficha.*

*Por último, cada agendamento pode ter vários serviços — banho, tosa, consulta, vacina, o que for. Cada serviço tem um nome e um preço padrão, mas no agendamento eu guardo quanto foi cobrado de verdade, porque às vezes dou desconto pra cliente que é frequente."*

---

## 📝 O que entregar

Escreva o script SQL completo que cria o banco de dados `patas_cia` e todas as suas tabelas. Seu arquivo deve conter, nesta ordem:

1. Remoção do banco anterior e criação do banco novo
2. Garantia de que o script rode mesmo que o banco ou tabelas já existam
3. Criação de todas as tabelas, na ordem correta, com PKs, FKs e demais constraints adequadas ao contexto descrito

Além dos campos mencionados pela Simone, complemente o banco com outros campos que julgar pertinentes para o negócio, com base nos conhecimentos adquiridos no curso de Gestão da Tecnologia da Informação. Esses campos adicionais serão considerados nos critérios de modelagem e de tipos e tamanhos.

Não é necessário inserir dados nem escrever consultas SELECT.

---

## 📊 Critérios de Avaliação

| Critério | Pontos |
|---|---|
| Criação do banco | 0,30 |
| Modelagem | 0,90 |
| Chaves | 0,40 |
| Constraints | 0,40 |
| Convenções | 0,50 |
| Tipos e tamanhos | 0,50 |
| **Total** | **3,00** |

**Descontos automáticos:**
- Nome Incompleto ou fora da formatação solicitada: -0,10
- Arquivo que não executa sem erros do início ao fim: -0,50

---

## 🔗 Navegação

⬅️ [Aula 08 — Restrições de Integridade](Aula_08_Restricoes_Integridade.md) · ➡️ [Aula 10 — SQL DML](Aula_10_SQL_DML.md)

---

*Fatec Jahu · IBD951 · Prof. Ronan Adriel Zenatti · 2026*
