# Aula 7 — Frota de agentes sempre ativos (always-on agent fleets)

**Curso:** Como montar um negócio de serviços de IA
**Bloco:** Fundamentos técnicos / mão na massa
**Duração estimada:** 50-55 minutos
**Formato:** roteiro corrido, pronto pra narrar — pausas de bloco marcadas com `[BLOCO]`

---

## Abertura — Gancho (≈5 min)

Deixa eu abrir com uma ideia que resume o assunto de hoje inteiro: **uma frota de agentes, na plataforma certa, consegue rodar uma operação de verdade.** Não um hobby, não uma demo — uma operação de verdade, com faturamento, clientes e prazos. Essa é a tese que vem separando, na prática, duas ligas completamente diferentes de quem trabalha com IA:

- **Liga 1 — automação de tarefa.** Você monta um workflow, ele dispara, faz uma coisa, termina. Amanhã você reexplica tudo de novo. É o modelo que 90% dos "agentes de IA" vendidos hoje ainda seguem.
- **Liga 2 — frota de agentes sempre ativos.** Agentes que não dormem entre conversas, que lembram o que você já ensinou, que evoluem sozinhos as próprias skills, e que se auto-avaliam contra um padrão de qualidade que você define uma vez. Eles não ficam passivos esperando um prompt: eles ficam de plantão, agem quando o evento certo acontece, e não esquecem.

A tese da aula de hoje é simples: **o próximo salto de valor em serviços de IA não é fazer mais automações — é transformar automações soltas numa frota que acumula conhecimento.** E pra fazer isso direito, você precisa entender a anatomia de um agente always-on peça por peça. É isso que vamos desmontar agora, usando a arquitetura que as plataformas de agentes mais modernas vêm consolidando — porque esse vocabulário vai virar linguagem comum na área nos próximos anos.

`[BLOCO]`

---

## Bloco 2 — O que muda quando o agente ganha um ambiente próprio (≈8 min)

Vamos tirar o marketing da frente e falar da arquitetura real.

Boa parte da comunidade de automação hoje roda agentes numa máquina física em casa ou no escritório — um mini PC ligado 24 horas, rodando um script que quebra quando a luz cai ou quando alguém desliga sem querer. As plataformas de agentes modernas resolvem isso de raiz: **cada sessão do agente roda no seu próprio ambiente de computação isolado, na nuvem** — não um contêiner compartilhado, mas um ambiente dedicado por sessão, com execução de código em tempo real, navegador próprio e acesso à web.

E sobre o motor por trás: essas plataformas rodam sobre **modelos de fronteira, como o Claude Opus**, geralmente com alguma flexibilidade de escolha de modelo — mas com os modelos mais capazes servindo de espinha dorsal das execuções complexas.

Por que isso é uma decisão de arquitetura, e não um detalhe técnico qualquer? Porque muda o que o agente pode fazer sem você. Um agente que só existe dentro de uma janela de chat depende de você manter a aba aberta e reagir. Um agente que roda num ambiente isolado na nuvem pode:

- Ficar de plantão esperando um gatilho (uma mensagem no Slack, um evento agendado) sem custar nada enquanto está ocioso.
- Rodar por horas sem travar sua máquina.
- Ter navegador próprio pra acessar sistemas que exigem login, sem depender do seu Chrome aberto.
- Escalar em paralelo — dez agentes rodando ao mesmo tempo, cada um no seu próprio ambiente, sem brigar por recurso.

O contraste que resume tudo é este: **pensa na diferença entre um chatbot e um funcionário.** Um chatbot responde perguntas. Um funcionário aprende o seu negócio, constrói habilidades ao longo do tempo, lembra o que funcionou e melhora a cada semana.

Guarda essa frase, porque ela é literalmente o critério que separa "ferramenta de IA" de "frota de agentes always-on" — e é o fio condutor do checklist que a gente monta no fim da aula.

`[BLOCO]`

---

## Bloco 3 — Anatomia de um agente always-on (≈12 min)

Aqui é o coração técnico da aula. Um agente always-on de verdade não é um prompt bonito. Ele tem pelo menos cinco peças que se encaixam, e vou te dar o nome de cada uma usando o vocabulário que as plataformas de agentes vêm padronizando.

### 3.1 — Agente (a persona)

A camada mais visível é o **Agente**: uma entidade com nome, papel e comportamento próprio. Na prática, quando você cria um agente numa plataforma dessas, você define um **system prompt customizado** (o papel e a personalidade dele), **integrações específicas** (só as ferramentas que aquele agente precisa, não tudo), **limite de orçamento** (um teto de gasto por agente) e **qual modelo o alimenta**.

O ponto prático aqui: **um agente, um job.** A recomendação de quem opera isso em produção é não criar um "agente faz-tudo" — e sim especialistas: um de pesquisa, um de outreach, um de conteúdo, cada um com seu próprio orçamento e integrações. Isso é o oposto do instinto de quem está começando com IA, que tende a montar um super-prompt genérico. A frota funciona porque cada peça é estreita e profunda, não porque uma peça é onisciente.

### 3.2 — Skills (workflows aprendidos)

**Skills** são fluxos de trabalho codificados que o agente aprende observando como você trabalha. Você completa uma tarefa uma vez, o sistema identifica o padrão por trás dela, e transforma isso numa skill reutilizável que pode ser disparada de novo depois.

E aqui tem um detalhe que separa isso de um "prompt salvo": cada skill pode rodar **seu próprio script em Python e guardar suas próprias chaves de API** — ou seja, uma skill pode chamar uma API interna, transformar dado, ou plugar uma ferramenta que a plataforma não suporta nativamente, sem esperar suporte oficial. Skill não é macro de clique. É um pedaço de capacidade nova que o agente ganha e mantém.

E skills são compartilháveis: já existem coleções públicas de skills prontas, publicadas como arquivos que qualquer um pode importar e adaptar — de gerador de brand book a visualização de dados e simulação de operação de negócio. Isso significa que sua frota não precisa aprender tudo do zero: parte do repertório dela pode vir pronto da comunidade, e você adapta.

### 3.3 — Memórias (contexto pessoal que compõe)

**Memórias** são o contexto pessoal que o agente guarda sobre você e suas preferências — sua voz de marca, sua formatação preferida, as ferramentas que você gosta de usar. A ideia central é que tudo isso **compõe**: cada sessão pode gerar novas memórias, revisáveis manualmente ou aceitas automaticamente, e com o tempo cada agente fica mais afiado no seu jeito específico de trabalhar.

Isso resolve a dor que qualquer um que já usou um chatbot genérico conhece: passar 20 minutos ensinando contexto de novo toda manhã, porque o histórico não persiste de um jeito útil. Memória, aqui, não é "o chat lembra da conversa anterior" — é "o agente carrega uma camada de preferência que atravessa qualquer conversa nova".

### 3.4 — Rubrics (LLM-as-judge)

Essa é a peça que menos gente monta na prática, e é provavelmente a mais importante pra confiar num agente sem supervisionar cada saída dele. **Rubrics** são avaliações do tipo LLM-as-judge: você define critérios de qualidade, e o próprio sistema avalia a saída dele contra esse padrão — funciona como um A/B testing embutido pros fluxos do agente.

Pensa assim: sem rubric, um agente always-on é uma aposta — ele roda sozinho e você só descobre que errou quando o erro já causou dano. Com rubric, o próprio agente aplica um teste de qualidade antes de te entregar (ou antes de agir), e você calibra esse teste, não cada execução individual.

### 3.5 — Thread Context e Library (memória de trabalho e acervo)

Toda tarefa vive numa **Thread** — pensa nela como uma conversa específica de projeto, onde você acompanha a execução em tempo real. A recomendação prática é "empilhar workflows, não prompts": em vez de escrever um prompt monstro, você quebra a tarefa complexa em threads separadas, e deixa a saída de uma alimentar a próxima — a memória persistente carrega o contexto adiante.

E tudo que sai dessas threads vai pra uma **Library** — um acervo pesquisável de tudo que os agentes já produziram. Com o tempo, essa Library vira uma base de conhecimento real da empresa, construída a partir do trabalho de verdade que já rolou, não de documentação escrita à parte.

Junta essas cinco peças — Agente, Skills, Memórias, Rubrics, Thread/Library — e você tem um **motor de inteligência composta**: um sistema em que ensinar uma coisa pra um agente pode valer pra frota inteira. É essa composição — não qualquer peça isolada — que separa "automação" de "frota always-on".

`[BLOCO]`

---

## Bloco 4 — O loop de execução: a camada de navegador (≈8 min)

Toda essa anatomia bonita de nada serve se o agente não conseguir *agir* no mundo real — e "mundo real", pra maioria das tarefas de negócio, significa navegar na web: entrar em painéis, preencher formulário, baixar relatório, confirmar reserva. Esse é o pedaço mais chato de construir do zero — e é exatamente por isso que até as plataformas grandes preferem **comprar essa camada pronta em vez de construir**.

O raciocínio é direto: construir essa camada internamente significa montar frotas de navegadores, lidar com barreiras de autenticação, observabilidade, e gerenciar infraestrutura global — tudo coisa que o time teria que manter pra sempre. Ferramentas especializadas resolvem isso como serviço: a **Browserbase**, por exemplo, oferece infraestrutura de sessões de navegador na nuvem, e o **Stagehand**, o SDK de código aberto dela, traduz uma intenção em linguagem natural ("clica em exportar relatório") numa interação confiável de navegador, dentro do próprio loop de execução do agente.

Duas características técnicas fazem esse tipo de camada funcionar em produção, não só em demo:

- **Seletores que se auto-curam quando a página muda** — o site redesenha o botão e o agente não quebra.
- **Ações em cache que pulam chamadas redundantes de LLM** em páginas parecidas — ou seja, o agente fica mais rápido e mais barato com o uso.

E essa camada resolve três problemas que, do contrário, você teria que carregar sozinho:

1. **Acesso confiável à web.** Via identidade de agente, cada execução pode ganhar uma credencial verificada criptograficamente usando o **Web Bot Auth** — um padrão aberto adotado por players como Cloudflare e Stytch. Sites reconhecem o agente como legítimo, então o usuário termina a tarefa em vez de esbarrar num muro de bloqueio.
2. **Capacidade de pico que o usuário nunca sente.** Quando o tráfego aumenta, a infraestrutura escala pra milhares de sessões simultâneas de navegador automaticamente.
3. **A "view" de navegador ao vivo como momento central de UX** — você literalmente vê o agente navegando em tempo real. Isso não é enfeite: é o que dá confiança pro usuário deixar o agente agir sozinho, porque ele pode auditar qualquer execução com os próprios olhos.

E pra onde isso caminha: autenticação persistente (o usuário loga uma vez, e os agentes seguem a partir daí) e bibliotecas de skills de navegação com receitas prontas pra tarefas comuns — buscar voos, painéis financeiros, fluxos de reserva.

A lição prática pra quem monta serviço de IA: **não reinvente a camada de navegador**. Se seu agente precisa "clicar em coisas" na internet pra produzir valor, o gargalo não é o prompt — é infraestrutura de sessão de navegador, autenticação e resiliência a mudança de layout. Isso é exatamente o problema que ferramentas como Browserbase/Stagehand (e equivalentes) resolvem, e é o mesmo tipo de decisão de "comprar a manutenção em vez de construir" que qualquer agência pequena deveria replicar em vez de tentar montar isso do zero.

`[BLOCO]`

---

## Bloco 5 — Estudo de caso: o agente de faturamento no Slack (≈8 min)

Agora vamos ver as cinco peças da anatomia e o loop de execução funcionando juntos, num caso concreto — o tipo de caso que qualquer prestador de serviço reconhece na própria operação.

Imagina uma firma pequena de consultoria, com uma operação de faturamento (billing) típica. Antes do agente, manter o billing em dia significava: abrir a base no Airtable pra checar o status de cada contrato, calcular manualmente a taxa certa pra cada entrega concluída, e depois montar os itens de linha (line items) no QuickBooks à mão. Trabalho repetitivo, sensível a erro, e que ninguém quer fazer na sexta à tarde.

Com um agente always-on vivendo dentro de um canal privado do Slack, o fluxo vira: **o operador digita no Slack o que aconteceu** — "fechamos a entrega X do cliente Y" — **e o trabalho é feito**: o agente confere o status na base do Airtable, calcula a taxa correta com base no tipo de contrato, e monta os itens de linha no QuickBooks sozinho.

Vamos mapear isso na anatomia que acabamos de aprender, pra fixar o conceito:

- **Agente (persona):** um "agente de finanças" com system prompt focado em billing, com acesso restrito a duas integrações (Airtable e QuickBooks) — não um agente genérico.
- **Gatilho:** mensagem no canal privado do Slack — não é um cron job, é reativo a um evento de negócio real (uma entrega concluída).
- **Skill:** o fluxo "checar status → calcular taxa → montar line item" é exatamente o tipo de padrão repetitivo que vira skill codificada, reaproveitável em cada entrega nova.
- **Memória:** as regras de precificação por tipo de contrato, uma vez ensinadas, não precisam ser reexplicadas a cada mensagem.
- **Rubric:** um critério de qualidade plausível aqui seria "o valor calculado bate com o contrato original" ou "o item de linha segue o padrão de nomenclatura do QuickBooks" — o tipo exato de checagem que rubrics em LLM-as-judge servem pra automatizar.
- **Thread/Library:** cada mês de billing vira uma thread, e o histórico de faturamento processado fica pesquisável.

O motivo desse tipo de caso ser tão bom como referência não é o volume (é uma tarefa de escritório pequeno) — é que ele é **o exemplo mais nítido e replicável** de como uma tarefa administrativa chata, que qualquer prestador de serviço de IA reconhece na própria operação (faturar clientes), vira um agente always-on de verdade, sem precisar ser uma megacorporação.

E o mesmo padrão escala: uma empresa de software estabelecida colocou um agente no Slack que responde perguntas analíticas da equipe o dia inteiro — navega um catálogo de dados com dezenas de milhares de entradas, escreve SQL, valida os resultados contra benchmarks conhecidos, e entrega a análise pronta em poucos minutos, economizando **centenas de horas por semana** do time de dados. Dois tamanhos de operação completamente diferentes, a mesma arquitetura de agente always-on por trás.

`[BLOCO]`

---

## Bloco 6 — Síntese: checklist de 6 perguntas pra diagnosticar um agente always-on de verdade (≈6 min)

Tudo isso vira prático com um checklist — a destilação didática das peças que vimos nas Seções 3 e 4. Use isso pra separar "eu automatizei uma tarefa" de "eu tenho um agente always-on".

| # | Pergunta | O que responde | Se a resposta for "não" |
|---|----------|-----------------|--------------------------|
| 1 | **Gatilho:** o agente age sem alguém abrir uma janela de chat e escrever um prompt novo? | Existe um evento (mensagem, agenda, webhook) que dispara a execução sozinho | É um chatbot reativo, não um agente always-on |
| 2 | **Persona:** o agente tem papel, escopo de ferramentas e orçamento próprios — e não é um "faz-tudo" genérico? | Existe um system prompt e integrações restritas a um job específico | É um assistente genérico, difícil de confiar em produção |
| 3 | **Skill:** existe um fluxo de trabalho codificado e reutilizável, ou cada execução é reinventada na hora? | O padrão de trabalho foi capturado como algo reaproveitável | Cada execução custa o mesmo esforço de ensinar de novo |
| 4 | **Memória:** o que foi ensinado numa sessão sobrevive e informa a próxima, sem reexplicação manual? | Preferências e contexto compõem ao longo do tempo | Você está sempre pagando o "custo de re-onboarding" |
| 5 | **Rubric:** existe um critério de qualidade que o próprio sistema (ou um processo definido) usa pra validar a saída antes de você confiar nela? | Há uma verificação de qualidade programada, não só sua revisão manual | Você é o único controle de qualidade — não escala |
| 6 | **Thread/Library:** o resultado do trabalho fica registrado e pesquisável, formando acervo, ou se perde na conversa? | Existe um repositório de saídas que vira base de conhecimento | Conhecimento não composto — cada tarefa começa do zero de novo |

Se você respondeu "sim" nas seis, o que você tem na mão é uma frota always-on de verdade. Se ficou em três ou quatro "sim", você tem uma automação boa — só que ainda não compõe sozinha, e ainda depende fortemente de você pra escalar.

`[BLOCO]`

---

## Exercício — aplique o checklist numa tarefa sua (10-15 min, pode ser feito após a aula)

**Objetivo:** pegar uma tarefa recorrente da sua própria operação (ou de um cliente) e desenhar as seis peças antes de programar qualquer coisa.

**Passo 1 — Escolha a tarefa.** Pense numa tarefa que você (ou alguém da sua equipe) faz toda semana, que segue um padrão razoavelmente estável, e que dói o suficiente pra valer a pena resolver. Exemplos: responder um tipo específico de pergunta de cliente, gerar um relatório semanal, qualificar leads que chegam por formulário, ou — como no estudo de caso — processar faturamento. Escreva essa tarefa em uma frase.

**Passo 2 — Defina o Gatilho.** Escreva exatamente o evento que vai disparar o agente, sem que você precise abrir um chat e digitar manualmente. Pode ser: uma mensagem chegando num canal do Slack/WhatsApp, um horário fixo (ex.: toda segunda 8h), ou um evento de sistema (um formulário preenchido, um card movido no board). Seja específico: "quando uma mensagem chegar no canal #financeiro contendo a palavra 'fechado'" é um gatilho de verdade; "quando eu lembrar" não é.

**Passo 3 — Escreva o Prompt/Persona.** Redija, em texto corrido, o system prompt desse agente: qual é o papel dele, o que ele pode e não pode fazer, quais ferramentas/integrações ele acessa, e (se fizer sentido) um teto de orçamento por execução. Uma frase de teste: se você mostrasse esse prompt pra alguém novo na sua equipe, essa pessoa saberia exatamente o que o agente faz e o que está fora do escopo dele?

**Passo 4 — Nomeie a Skill.** Descreva o fluxo de passos que esse agente executa toda vez (o "como"), como se fosse uma receita: passo 1, passo 2, passo 3. Essa é a skill. Se você não consegue descrever em passos repetíveis, a tarefa provavelmente ainda não está madura o bastante pra virar agente — volta pro passo 1 e escolhe algo mais estruturado.

**Passo 5 — Defina a Memória.** Liste 3 a 5 coisas que esse agente precisa "lembrar" entre execuções pra não custar seu tempo toda vez de novo: preferências de formatação, regras de negócio específicas (tipo tabela de preço), nomes e exceções recorrentes de clientes.

**Passo 6 — Escreva a Rubrica.** Defina de 2 a 4 critérios objetivos que decidem se a saída desse agente está boa o suficiente pra você confiar sem revisar linha por linha. Exemplos de critério: "o valor bate com o contrato original", "o tom está alinhado com a marca", "todos os campos obrigatórios foram preenchidos". Se você não consegue escrever um critério objetivo, é sinal de que ainda não dá pra automatizar a validação — e por ora um humano segue no loop.

**Entregável do exercício:** um documento curto (meia página) com as seis peças preenchidas — Tarefa, Gatilho, Persona/Prompt, Skill, Memória, Rubrica — pronto pra virar a especificação de configuração do seu primeiro agente always-on, em qualquer stack que você já usa (Claude Code, n8n, uma plataforma de agentes na nuvem, etc. — o framework de anatomia vale independente da ferramenta).

`[BLOCO]`

---

## Fechamento (≈2 min)

Resumindo o que vimos hoje: a virada de "automação de tarefa" pra "frota de agentes always-on" não é sobre ter um modelo mais esperto — é sobre montar as cinco peças que fazem um agente compor conhecimento ao longo do tempo: persona com escopo definido, skills codificadas, memórias que persistem, rubrics que validam sozinhas, e um acervo de thread/library que vira base de conhecimento viva. E a mesma arquitetura serve do agente de dados interno de uma empresa grande, que economiza centenas de horas por semana, ao agente de billing de um único operador rodando dentro de um canal do Slack.

A régua que você leva desta aula é o checklist de seis perguntas: gatilho, persona, skill, memória, rubrica, thread/library. Aplique nele antes de vender qualquer coisa como "agente" — porque é exatamente essa distinção que separa quem cobra por automação pontual de quem cobra por uma frota que se paga sozinha com o tempo.

Na próxima aula a gente sobe de nível de conversa: depois de saber *construir* uma frota de agentes always-on, o tópico 8 encara o outro lado do negócio — como vender isso pra empresas grandes, o que uma Fortune 500 realmente compra em IA hoje (não é tecnologia, é resultado), e os frameworks de venda B2B — MEDDIC e Challenger Sale — aplicados especificamente a projetos de IA. Até lá.
