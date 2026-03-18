# Atividades de Estudos 02 — Modelagem de Banco de Dados

**Disciplina:** Banco de Dados e Aplicações (IBD951)  
**Professor:** Ronan Adriel Zenatti · ronan.zenatti@cps.sp.gov.br  
**Fatec Jahu — 1º Semestre/2026**

---

## 📌 Instruções Gerais

Este arquivo contém **5 exercícios de modelagem** com dificuldade progressiva. Leia o enunciado de cada exercício com atenção, identifique as entidades, os atributos e os relacionamentos a partir do contexto descrito, e construa o modelo lógico relacional usando a ferramenta **dbdiagram.io**.

**Para cada exercício, você deve entregar:**

1. **O link público do diagrama** gerado no dbdiagram.io (`Share → Copy link`)
2. **Um arquivo `.txt`** contendo o script de criação do banco de dados, nomeado como `ExXX_NomeCompleto.txt` (exemplo: `Ex01_JoaoSilva.txt`)

---

## 🛠️ Referência Rápida: Sintaxe do dbdiagram.io

O dbdiagram.io usa uma linguagem própria chamada **DBML** para descrever tabelas e relacionamentos. Veja um mini-exemplo para se basear:

```
// Mini-exemplo: sistema de pedidos simples
// Acesse dbdiagram.io, clique em "Create your diagram" e cole o código abaixo

Table clientes {
  id_cliente INT [PK]   // chave primária com auto incremento
  nome VARCHAR
  email VARCHAR
  data_nascimento DATE
}

Table pedidos {
  id_pedido INT [PK]
  data_pedido DATETIME
  status VARCHAR
  cliente_id INT
}
Ref: pedidos.cliente_id < clientes.id_cliente
// Referências também podem ser declaradas fora das tabelas:
// Ref: pedidos.cliente_id > clientes.id_cliente   // N:1
// Ref: turmas.id_turma - alunos.turma_id           // 1:1
```

**Notação de relacionamentos no DBML:**

| Símbolo | Significado |
|---|---|
| `>` | Muitos para Um (N:1) — o lado esquerdo é o N |
| `<` | Um para Muitos (1:N) — o lado esquerdo é o 1 |
| `-` | Um para Um (1:1) |
| `<>` | Muitos para Muitos (N:M) — gera tabela intermediária |

---

## 📐 Convenções de Nomenclatura

Siga rigorosamente as convenções da Aula 04 em todos os exercícios:

| Elemento | Convenção | Exemplo |
|---|---|---|
| Nome da tabela | minúsculas, singular, snake_case | `produto`, `item_pedido` |
| Nome da coluna | minúsculas, snake_case | `data_nascimento`, `preco_unitario` |
| Chave primária | `id_` + nome da tabela | `id_produto`, `id_cliente` |
| Chave estrangeira | tabela que referencia + _id | `cliente_id`, `produto_id` |
| Comandos SQL no `.txt` | MAIÚSCULAS | `VARCHAR`, `INT` |

---

## ⚙️ O que analisar em cada enunciado

Ao ler o enunciado, faça as seguintes perguntas mentalmente:

1. **Quais são as "coisas" sobre as quais o sistema precisa guardar informações?** → Cada uma vira uma tabela
2. **Quais são as características de cada coisa?** → Viram colunas
3. **Como essas coisas se relacionam entre si?** → Definem as FKs e tabelas associativas
4. **Quantas instâncias de A se relacionam com quantas de B?** → Define a cardinalidade (1:1, 1:N, N:M)
5. **Existe alguma coisa que só faz sentido existir junto de outra?** → Entidade fraca / autorelacionamento

---
---

## 🟢 Exercício 01 — Cadastro de uma Barbearia `[Nível: Iniciante]`

### Enunciado

Marcos é dono de uma barbearia no centro de Jahu e quer informatizar seu negócio. Em uma conversa, ele explicou como funciona o dia a dia:

*"Meu trabalho gira em torno dos meus clientes e dos meus barbeiros. Cada cliente tem nome, telefone e data de nascimento. Cada barbeiro que trabalha aqui tem nome e especialidade — tem quem faz só corte, quem faz corte e barba, esse tipo de coisa.*

*O que eu preciso mesmo é controlar os agendamentos. Quando um cliente marca horário, eu anoto o dia e a hora, o barbeiro que vai atender e o serviço que ele vai fazer — que pode ser Corte, Barba, Corte + Barba ou Hidratação. O agendamento também tem um status: Agendado, Concluído ou Cancelado.*

*Os serviços têm preço e duração em minutos, e isso pode variar — um corte simples custa R$ 35,00 e leva 30 minutos, enquanto um corte + barba custa R$ 60,00 e leva 50 minutos."*

### O que você deve modelar

A partir do relato de Marcos, identifique as entidades, seus atributos e como elas se relacionam. Nem todos os campos foram citados explicitamente — use seu raciocínio para inferir o que é necessário em cada tabela.

### Campos parcialmente declarados pelo cliente

- Cliente: nome, telefone, data de nascimento *(e mais o que você julgar necessário)*
- Barbeiro: nome, especialidade *(e mais o que você julgar necessário)*
- Serviço: nome, preço, duração em minutos
- Agendamento: data e hora, status *(e mais o que você julgar necessário para ligar as entidades)*

### Requisitos obrigatórios

- [ ] Toda tabela deve ter uma chave primária
- [ ] Os relacionamentos devem estar representados com as FKs corretas
- [ ] O agendamento deve ligar cliente, barbeiro e serviço
- [ ] O script `.txt` deve conter `CREATE TABLE` para todas as tabelas na ordem correta

### 💭 Dica de raciocínio

Pense: um cliente pode marcar vários agendamentos? E um barbeiro, pode ter vários agendamentos? E um serviço, pode aparecer em vários agendamentos? Essas respostas definem as cardinalidades.

---
---

## 🟡 Exercício 02 — Sistema de uma Biblioteca Municipal `[Nível: Básico]`

### Enunciado

A Biblioteca Municipal de uma cidade está modernizando seu controle de acervo e empréstimos. A bibliotecária-chefe, Dona Vera, explicou o funcionamento:

*"Temos um acervo grande de livros. Cada livro tem título, ISBN, ano de publicação e número de páginas. Um livro pode ter mais de um autor — tem livro aqui com três, quatro autores — então isso precisa ficar registrado direitinho. De cada autor a gente guarda o nome e a nacionalidade.*

*Os nossos usuários se cadastram com nome completo, CPF e endereço. Cada usuário tem um número de matrícula único que ele usa para se identificar.*

*Quando alguém pega um livro emprestado, a gente registra a data que saiu e a data prevista para devolução. Quando o livro volta, a gente anota a data real da devolução. Se essa data for depois da data prevista, o usuário paga uma multa — mas a multa a gente calcula depois, não precisa guardar no banco.*

*Ah, e cada exemplar físico do livro tem um número de tombo próprio — um mesmo livro pode ter 3 exemplares aqui na biblioteca, e cada exemplar pode ser emprestado de forma independente."*

### O que você deve modelar

Preste atenção na diferença entre o **livro** (a obra, com título e ISBN) e o **exemplar** (o objeto físico com número de tombo). Essa distinção é fundamental para o modelo.

### Requisitos obrigatórios

- [ ] O relacionamento entre livros e autores deve ser modelado corretamente (um livro pode ter vários autores)
- [ ] O exemplar deve estar vinculado ao livro, mas ser independente para empréstimos
- [ ] O empréstimo deve estar vinculado ao exemplar e ao usuário
- [ ] Toda tabela deve ter PK; FKs nomeadas explicitamente no script `.txt`

### 💭 Dica de raciocínio

Um livro tem muitos autores, e um autor pode ter escrito muitos livros — isso é um relacionamento de que tipo? O que ele gera no modelo relacional?

---
---

## 🟠 Exercício 03 — Plataforma de Cursos Online `[Nível: Intermediário]`

### Enunciado

Uma edtech chamada **LearnFast** quer um banco de dados para gerenciar sua plataforma de cursos online. O CEO, Felipe, mandou um e-mail explicando o modelo de negócio:

> *Olá, precisamos de um banco de dados para a LearnFast. Aqui vai como funciona:*
>
> *Temos **instrutores** que criam **cursos**. Um instrutor pode criar vários cursos, mas cada curso tem apenas um instrutor responsável. De cada instrutor, precisamos guardar nome, e-mail, bio e área de especialidade.*
>
> *Cada curso tem título, descrição, preço, nível (Iniciante, Intermediário, Avançado) e carga horária total. Um curso é dividido em **módulos** — tem curso com 3 módulos, tem com 15. Cada módulo tem título, descrição e uma ordem dentro do curso (para saber qual vem primeiro, segundo, etc.). Dentro de cada módulo há **aulas**, e cada aula tem título, duração em minutos e um link para o vídeo.*
>
> *Os **alunos** se cadastram com nome, e-mail e data de nascimento. Um aluno pode se matricular em vários cursos e um curso pode ter vários alunos. Quando um aluno se matricula, a gente registra a data da matrícula e o preço que ele pagou (que pode ser diferente do preço atual por causa de promoções). Também controlamos o **progresso**: precisamos saber quais aulas cada aluno já assistiu e quando assistiu pela primeira vez.*
>
> *Por último, os alunos podem avaliar os cursos com uma nota de 1 a 5 e um comentário. Um aluno só pode avaliar um curso se estiver matriculado nele, e só pode avaliar uma vez.*

### O que você deve modelar

Este exercício introduz uma hierarquia de três níveis (curso → módulo → aula) e múltiplos relacionamentos N:M. Mapeie tudo com cuidado.

### Requisitos obrigatórios

- [ ] A hierarquia curso → módulo → aula deve estar clara no modelo
- [ ] O relacionamento de matrícula deve guardar o preço pago no momento
- [ ] O progresso por aula deve vincular aluno e aula
- [ ] A constraint de "um aluno avalia o mesmo curso apenas uma vez" deve estar representada (use `unique` na combinação de colunas)
- [ ] O script `.txt` deve incluir comentários indicando qual entidade cada `CREATE TABLE` representa

### 💭 Dica de raciocínio

Quantas tabelas você precisará criar no total? Faça a lista antes de começar a modelar. Dica: são mais do que as entidades que o CEO citou diretamente.

---
---

## 🔴 Exercício 04 — Sistema de Gestão de uma Oficina Mecânica `[Nível: Avançado]`

### Enunciado

Dona Célia é proprietária de uma oficina mecânica e quer um sistema completo para controlar clientes, veículos, ordens de serviço e estoque de peças. Em uma reunião de levantamento de requisitos, ela explicou:

*"Meus clientes podem trazer mais de um carro aqui — tem cliente que tem carro no nome dele, da esposa e do filho. Então não posso misturar cliente com veículo. De cada cliente eu preciso do CPF, nome, telefone e e-mail. Cada veículo tem placa, marca, modelo e ano de fabricação.*

*Quando um veículo chega, a gente abre uma **ordem de serviço**. Nela fica registrado o que o cliente relatou de problema, a data de entrada e a data de saída quando o serviço termina. A ordem também tem um status: Aberta, Em andamento, Aguardando peça, Concluída ou Cancelada.*

*Cada ordem tem os **serviços** que foram realizados — pode ser Troca de óleo, Alinhamento, Funilaria, Revisão geral, e por aí vai. Cada tipo de serviço tem um nome e um valor de mão de obra. Uma ordem pode ter vários serviços diferentes.*

*Além dos serviços, uma ordem pode usar **peças** do nosso estoque. Cada peça tem código, descrição, quantidade em estoque e preço de custo. Quando uma peça é usada numa ordem, a gente registra quantas unidades foram usadas e o preço de venda cobrado ao cliente naquele momento.*

*Os mecânicos que trabalham aqui têm nome, CPF e especialidade. Uma ordem de serviço pode ter mais de um mecânico trabalhando nela — e um mecânico pode trabalhar em várias ordens ao mesmo tempo.*

*Tem mais uma coisa importante: os mecânicos podem ser funcionários comuns ou supervisores. Um supervisor pode ser responsável por outros mecânicos. O supervisor de um mecânico também é um mecânico."*

### O que você deve modelar

Este exercício contém um **autorelacionamento** (supervisores que são mecânicos) e múltiplos relacionamentos N:M com atributos próprios. Identifique todos eles antes de começar.

### Requisitos obrigatórios

- [ ] O autorelacionamento de supervisão entre mecânicos deve estar explícito no modelo com FK para a própria tabela
- [ ] A tabela de uso de peças na ordem deve guardar o preço de venda no momento do uso
- [ ] A associação de mecânicos à ordem de serviço deve ser modelada como tabela associativa
- [ ] A placa do veículo é candidata a chave — analise se ela deve ser a PK ou se é melhor ter um `id_veiculo` com a placa como unique
- [ ] O script `.txt` deve criar as tabelas na ordem correta, respeitando todas as dependências de FK

### 💭 Dica de raciocínio

Conte os relacionamentos N:M deste exercício — cada um gera uma tabela associativa. Ao total, quantas tabelas você terá? Liste antes de modelar para não esquecer nenhuma. O autorelacionamento do mecânico aparece como uma FK dentro da própria tabela `mecanico` apontando para `mecanico.id_mecanico` — como representar isso no dbdiagram?

---
---

## ⚫ Exercício 05 — Sistema de Gestão de Campeonato de E-Sports `[Nível: Complexo]`

### Enunciado

Uma empresa de eventos chamada **ArenaGG** quer um sistema completo para gerenciar campeonatos de jogos eletrônicos. O gerente de produto, Rafael, enviou uma especificação detalhada:

*"Organizamos campeonatos de vários jogos — League of Legends, CS2, Valorant, esse tipo de coisa. Cada **jogo** tem nome, estúdio desenvolvedor, gênero e ano de lançamento.*

*Os campeonatos são o coração do negócio. Cada **campeonato** é de um único jogo, tem nome, data de início, data de término, premiação total em reais e um formato (Online, Presencial, Híbrido). Um campeonato é organizado em **fases** — fase de grupos, quartas, semifinal, final. Cada fase tem nome, data de início e data de fim.*

*Dentro de cada fase acontecem as **partidas**. Cada partida ocorre em uma data e hora específica, tem uma duração em minutos e um resultado. Cada partida é entre exatamente dois **times**.*

*Os **times** têm nome, tag (abreviação como 'LOUD', 'FURIA'), país de origem e data de fundação. Um time pode participar de vários campeonatos, e um campeonato tem vários times — a participação de um time em um campeonato deve ser registrada, assim como a colocação final que ele obteve.*

*Cada time tem **jogadores**. Um jogador tem nome, nickname, data de nascimento, nacionalidade e função (como Suporte, Carry, Midlaner no LoL ou Rifle, AWP, IGL no CS2). Um jogador pertence a um time por vez, mas ao longo da carreira pode ter passado por vários times — e um time ao longo da história teve vários jogadores. Portanto, precisamos guardar o histórico: quando o jogador entrou no time, quando saiu, e se ainda está ativo naquele time.*

*As partidas têm estatísticas por jogador. Para cada jogador em cada partida, registramos: kills, deaths, assists e uma pontuação de desempenho calculada pelo sistema.*

*Por último, cada campeonato tem **árbitros** que supervisionam as partidas. Um árbitro tem nome, certificação e país. Um árbitro pode supervisionar várias partidas e uma partida pode ter mais de um árbitro. Árbitros também têm hierarquia: um árbitro sênior pode ser responsável por árbitros juniores, e o árbitro responsável também é um árbitro."*

### O que você deve modelar

Este é o exercício mais completo da lista. Antes de qualquer coisa, liste todas as entidades, depois todos os relacionamentos, classificando cada um como 1:1, 1:N ou N:M. Somente depois comece a modelar no dbdiagram.

### Relacionamentos a identificar *(sem revelar o tipo — analise cada um)*

- Campeonato ↔ Jogo
- Campeonato ↔ Fase
- Fase ↔ Partida
- Partida ↔ Time *(atenção: toda partida envolve exatamente 2 times)*
- Campeonato ↔ Time *(com colocação final)*
- Time ↔ Jogador *(com histórico de entrada/saída)*
- Jogador ↔ Partida *(com estatísticas)*
- Partida ↔ Árbitro
- Árbitro ↔ Árbitro *(hierarquia de supervisão)*

### Requisitos obrigatórios

- [ ] O histórico de jogadores por time deve ser modelado como tabela associativa com atributos de data de entrada, data de saída e flag de ativo
- [ ] A participação de um time em um campeonato deve guardar a colocação final
- [ ] As estatísticas de jogador por partida devem estar em tabela própria
- [ ] O autorelacionamento de árbitros (sênior → júnior) deve estar explícito
- [ ] A restrição de que uma partida tem exatamente dois times deve ser resolvida com criatividade — pense em como representar os dois lados (time_mandante e time_visitante, por exemplo)
- [ ] O script `.txt` deve estar com comentários por seção e as tabelas criadas na ordem correta

### 💭 Dica de raciocínio

Quantas tabelas você precisa criar no total? Faça a lista completa antes de abrir o dbdiagram. Se chegar a menos de 12 tabelas, provavelmente esqueceu alguma tabela associativa. Se chegar a mais de 16, talvez esteja criando tabelas desnecessárias. Refaça o mapeamento de relacionamentos antes de começar.

---
---

## 📊 Critérios de Avaliação

Cada exercício será avaliado pelos seguintes critérios:

| Critério | Descrição |
|---|---|
| **Identificação das entidades** | Todas as entidades necessárias foram mapeadas corretamente |
| **Atributos e tipos** | Os campos estão presentes com tipos de dados adequados ao contexto |
| **Chaves primárias** | Toda tabela tem PK; a escolha da PK é justificável |
| **Relacionamentos e FKs** | As FKs estão corretas e refletem as cardinalidades do enunciado |
| **Tabelas associativas** | Relacionamentos N:M foram resolvidos com tabela intermediária |
| **Casos especiais** | Autorelacionamentos e atributos de relacionamento foram tratados |
| **Script `.txt`** | O SQL cria todas as tabelas na ordem correta e sem erros de FK |
| **Nomenclatura** | Segue as convenções definidas (minúsculas, snake_case, prefixo `id_`) |

---

## 🔗 Referências Úteis

- **dbdiagram.io** — ferramenta de modelagem: [https://dbdiagram.io](https://dbdiagram.io)
- **Documentação DBML** — sintaxe completa: [https://dbml.dbdiagram.io/docs](https://dbml.dbdiagram.io/docs)
- **Aula 03** — Relacionamentos e Cardinalidade: [Aula_03_Relacionamentos_Cardinalidade.md](../aulas/Aula_03_Relacionamentos_Cardinalidade.md)
- **Aula 04** — Modelo Lógico Relacional: [Aula_04_Modelo_Logico_Relacional.md](../aulas/Aula_04_Modelo_Logico_Relacional.md)
- **Aula 05** — Normalização: [Aula_05_Normalizacao.md](../aulas/Aula_05_Normalizacao.md)

Dúvidas? Entre em contato: [ronan.zenatti@cps.sp.gov.br](mailto:ronan.zenatti@cps.sp.gov.br)

---

*Fatec Jahu · IBD951 · Prof. Ronan Adriel Zenatti · 2026*
