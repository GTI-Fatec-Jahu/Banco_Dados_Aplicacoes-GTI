# T1 — Atividade Avaliativa: Modelagem Conceitual e Lógica

**Disciplina:** Banco de Dados e Aplicações (IBD951)  
**Professor:** Ronan Adriel Zenatti · ronan.zenatti@cps.sp.gov.br  
**Fatec Jahu — 1º Semestre/2026**

---

## 📋 Descrição

Esta atividade avalia os conhecimentos de modelagem conceitual e lógica, cobrindo o conteúdo das aulas 2 a 5. Você receberá um enunciado de negócio e deverá produzir os modelos conceitual e lógico completos.

---

## 📝 Enunciado: Sistema de Agendamento de Clínica Médica

Uma clínica médica deseja um sistema para gerenciar seus atendimentos. Com base nas informações a seguir, crie o modelo conceitual e o modelo lógico relacional.

**Sobre a clínica:** A clínica possui vários médicos, cada um com especialidade (clínico geral, pediatra, ortopedista, etc.) e CRM. Um médico pode atender em mais de um consultório, e um consultório pode ser usado por mais de um médico, mas nunca ao mesmo tempo.

**Sobre os pacientes:** Cada paciente é identificado pelo CPF e possui nome, data de nascimento e telefone. Um paciente pode ter vários telefones cadastrados.

**Sobre as consultas:** Um paciente pode agendar consultas com médicos. Cada consulta tem data, horário e status (Agendada, Realizada, Cancelada). Quando a consulta é realizada, o médico registra o diagnóstico e a prescrição.

**Sobre os convênios:** A clínica aceita vários convênios. Um paciente pode ter vários convênios. Uma consulta pode ser coberta por apenas um convênio (ou nenhum, se for particular).

---

## 📌 O que deve ser entregue

**Parte 1 — Modelo Conceitual (Diagrama ER):** Identifique todas as entidades e seus atributos, incluindo os identificadores. Defina todos os relacionamentos com suas cardinalidades mínima e máxima. Indique entidades fracas e atributos multivalorados onde houver.

**Parte 2 — Modelo Lógico Relacional:** Converta o modelo conceitual para tabelas relacionais, aplicando corretamente as regras de mapeamento. Indique claramente as PKs e FKs em cada tabela.

**Parte 3 — Verificação das Formas Normais:** Para cada tabela do modelo lógico, verifique e justifique se ela está na 1FN, 2FN e 3FN. Se identificar violações, corrija-as e explique o que foi feito.

---

## 📊 Critérios de Avaliação

| Critério | Peso |
|---|---|
| Identificação correta de entidades e atributos | 25% |
| Relacionamentos com cardinalidades corretas | 25% |
| Mapeamento para o modelo lógico | 25% |
| Aplicação das formas normais | 25% |

---

## 📅 Entrega

A entregar em aula conforme orientação do professor. Pode ser feito à mão (para o diagrama ER) ou usando ferramentas como BRModelo, MySQL Workbench ou Draw.io.

---

*Fatec Jahu · IBD951 · Prof. Ronan Adriel Zenatti · 2026*
