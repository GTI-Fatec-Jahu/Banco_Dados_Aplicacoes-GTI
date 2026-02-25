# Aula 20 — Encerramento do Semestre: Feedback e Novas Tecnologias

**Disciplina:** Banco de Dados e Aplicações (IBD951)  
**Professor:** Ronan Adriel Zenatti · ronan.zenatti@cps.sp.gov.br  
**Fatec Jahu — 1º Semestre/2026**

---

## 🎯 Objetivos da Aula

Esta aula de encerramento tem como objetivo celebrar o aprendizado do semestre, contextualizar o que estudamos dentro do panorama atual da área e mostrar os caminhos de evolução profissional em banco de dados.

---

## 1. O que Construímos Juntos

Ao longo deste semestre passamos por uma jornada completa: começamos entendendo por que bancos de dados existem, aprendemos a modelar o mundo real em diagramas ER, transformamos esses modelos em tabelas relacionais bem normalizadas, implementamos fisicamente usando DDL, e aprendemos a consultar e manipular dados com SQL. Isso representa o ciclo completo de um projeto de banco de dados.

```mermaid
flowchart LR
    A["🌍 Negócio
e Requisitos"] --> B["✏️ MER
Modelagem
Conceitual"]
    B --> C["📐 Lógico
Tabelas PK FK"]
    C --> D["🏅 Normalização
1FN 2FN 3FN"]
    D --> E["💾 DDL
CREATE TABLE
Constraints"]
    E --> F["📊 DML + DQL
INSERT SELECT
JOINs Agregações"]
    F --> G["🎓 Você aqui!"]
```

---

## 2. Para Onde Ir Daqui: NoSQL

O modelo relacional domina o mercado há décadas e continuará sendo relevante. Mas o volume e a variedade de dados da era atual levaram ao surgimento dos bancos **NoSQL (Not Only SQL)**, que oferecem modelos alternativos para necessidades específicas.

[Infographic showing four NoSQL database types as colorful tiles: Document Databases with a paper icon (MongoDB), Key-Value stores with a key icon (Redis), Column-Family with a grid icon (Cassandra), Graph Databases with a network icon (Neo4j). Each with use case examples. Flat design, vibrant colors.]

![Tipos de bancos NoSQL](../imgs/Aula_20_img_01.png)

Os bancos de **documentos** (como o MongoDB) armazenam dados em formato JSON-like, ideais para dados semi-estruturados. Os bancos **chave-valor** (como o Redis) são extremamente rápidos para operações simples — muito usados para cache. Os bancos de **famílias de colunas** (como o Cassandra) escalam horizontalmente de forma impressionante para grandes volumes. E os bancos de **grafos** (como o Neo4j) são perfeitos para dados fortemente conectados, como redes sociais.

A boa notícia: quem domina bem o modelo relacional aprende NoSQL com muito mais facilidade, pois entende os trade-offs e sabe quando cada abordagem faz sentido.

---

## 3. Banco de Dados na Nuvem

A tendência atual é o banco de dados como serviço (**DBaaS — Database as a Service**). Em vez de instalar e administrar um servidor de banco de dados, você usa serviços gerenciados na nuvem: o provedor cuida de backups, replicação, escalabilidade e atualizações.

Exemplos: Amazon RDS (suporta MySQL, PostgreSQL, Oracle e outros), Google Cloud SQL, Azure SQL Database e PlanetScale. Esse tema é aprofundado na disciplina de Computação em Nuvem.

---

## 4. Caminhos de Evolução na Carreira

Quem quer se especializar em banco de dados pode seguir diferentes direções: o **DBA (Database Administrator)** cuida da instalação, configuração, performance e segurança dos SGBDs. O **Engenheiro de Dados** projeta pipelines de dados e trabalha com grandes volumes em ambientes de Data Warehouse e Data Lake. O **Analista de BI** transforma dados em informações estratégicas com ferramentas de visualização. E qualquer desenvolvedor back-end precisa de SQL como habilidade fundamental.

---

## 🎓 Parabéns pelo Semestre!

Você completou a disciplina de Banco de Dados e Aplicações. O conhecimento que adquiriu aqui é a base de toda aplicação que manipula dados — ou seja, praticamente toda aplicação do mundo real. Use bem!

---

## 🔗 Navegação

⬅️ [Aula 19 — Avaliação Substitutiva](Aula_19_Avaliacao_Substitutiva.md)

---

*Fatec Jahu · IBD951 · Prof. Ronan Adriel Zenatti · 2026*
