# Aula 1 — Seus primeiros 30 dias em automação de IA

**Curso:** Como montar um negócio de serviços de IA
**Duração estimada:** 35–38 minutos

---

## ABERTURA — o gancho (0:00–2:30)

Fala direto pra câmera, tom de quem já viu gente travar nesse ponto:

> Eu quero te fazer uma pergunta antes de qualquer coisa: quantos cursos, vídeos, threads e comunidades de automação de IA você já consumiu nos últimos meses? Não precisa responder em voz alta, mas pensa no número.
>
> Agora a segunda pergunta, a que realmente importa: quantas coisas você já **lançou**? Não "testou no seu próprio computador", não "mostrou pra sua namorada", não "deixou salvo numa pasta chamada 'projetos'". Quantas coisas que você construiu já foram vistas, usadas ou compradas por alguém que não é você?
>
> Se a resposta for zero, essa aula é pra você. E não é motivacional barata — é um plano de 30 dias, com critério objetivo em cada decisão, pra sair do modo "estudando pra sempre" e chegar no modo "tenho uma coisa funcionando que alguém pode ver".
>
> O filtro que vai atravessar esse curso inteiro cabe em quatro palavras: **projetos reais, receita real**. Não teoria, não hype. E é exatamente esse o filtro que vamos aplicar nos seus primeiros 30 dias.

---

## 1. O erro de entrada mais comum (2:30–6:30)

> Vamos nomear o problema com precisão, porque "procrastinação" é um nome preguiçoso pra isso.
>
> O erro mais comum de quem está entrando em automação de IA não é falta de conhecimento. É o oposto: é usar o consumo de conteúdo como um substituto confortável pra ação. Você assiste a um vídeo sobre n8n, entende, se sente produtivo. Assiste outro sobre agentes com Claude, entende, se sente produtivo de novo. Entra numa comunidade, lê 40 posts, se sente ainda mais por dentro. E no fim da semana, você sabe mais — mas ninguém no mundo sabe que você existe como alguém que constrói coisas.
>
> Isso tem um nome mais honesto: **é acúmulo de competência sem prova de competência**. E o mercado de serviços de IA, que é o que esse curso ensina a montar, não compra conhecimento acumulado. Compra prova.
>
> Pensa assim: se você fosse contratar alguém pra automatizar o atendimento da sua empresa, o que te convenceria mais rápido — a pessoa falando "eu estudei muito sobre isso" ou a pessoa te mostrando um vídeo de 60 segundos de um agente funcionando, com antes e depois? A resposta é óbvia, e é exatamente por isso que o critério dessa aula inteira é: **nada de estudar mais uma semana antes de lançar algo público**. Estudo sem prazo de lançamento vira consumo passivo disfarçado de produtividade.
>
> A boa notícia é que o oposto também é verdade: você não precisa saber tudo pra lançar a primeira coisa. Você precisa saber o suficiente pra resolver **um** problema específico, de um jeito que funcione de ponta a ponta. É isso que a Semana 1 desse plano força você a fazer — e vamos chegar lá.
>
> Antes, precisamos resolver uma pergunta que trava muita gente: lançar o quê, exatamente?

---

## 2. Critério para escolher o que construir primeiro (6:30–11:00)

> Essa é a pergunta que mais gera paralisia: "automação de IA" é um campo gigante. Agente de atendimento, geração de conteúdo, qualificação de leads, agendamento, RAG sobre documentos internos, automação financeira — tudo isso é "automação de IA". Se você tentar escolher pensando em "o que é mais impressionante" ou "o que está mais na moda", você vai travar de novo, só que travado numa decisão em vez de travado num vídeo.
>
> O critério que funciona na prática — e que aparece, em espírito, em praticamente todo programa sério de formação nesse mercado — não é sobre a ferramenta nem sobre o quão sofisticado é o agente. É sobre três perguntas objetivas que você faz sobre a **tarefa**, não sobre a tecnologia:
>
> **Pergunta 1: é repetitivo?** Se a tarefa acontece uma vez por ano, automatizar ela não vale o esforço — ninguém vai pagar por isso e você não vai aprender nada reaproveitável. Você quer uma tarefa que se repete todo dia, ou pelo menos toda semana.
>
> **Pergunta 2: existe uma regra clara pra decidir o que fazer?** Se a tarefa depende de julgamento humano extremamente subjetivo e inconsistente — tipo "decidir se vale a pena processar alguém" — ela é uma péssima primeira automação. Você quer uma tarefa onde, se você perguntasse pra três pessoas diferentes da empresa "o que eu faço nesse caso?", elas dariam respostas parecidas. Regra clara = automação viável.
>
> **Pergunta 3: consome tempo de um jeito que dói?** Não basta ser chato. Precisa ser uma tarefa que, se sumisse, alguém sentiria alívio real — porque hoje toma 30 minutos, 1 hora, 3 horas por dia de alguém. Dor mensurável em tempo é o que vira conversa de venda depois.
>
> Repare que nenhuma dessas perguntas é "qual ferramenta é mais avançada" ou "isso vai parecer impressionante no meu portfólio". É deliberado. O primeiro projeto tem que ser chato o suficiente pra ser real, e claro o suficiente pra você conseguir terminar em uma semana.
>
> Um exemplo concreto que caça as três caixas ao mesmo tempo — e não por acaso é o exemplo clássico de primeiro agente em quase todo programa de formação do mercado: um agente que lê e-mails de clientes chegando na caixa de suporte, busca nas políticas da empresa a resposta certa, e redige uma resposta pra um humano revisar antes de mandar. É repetitivo (chega todo dia), tem regra clara (a política da empresa já existe, você só está automatizando a busca e a redação) e consome tempo real (responder e-mail de suporte é, sabidamente, uma das tarefas mais odiadas em qualquer operação pequena).
>
> Guarda esse filtro de três perguntas. Você vai usar ele de novo daqui a duas semanas, quando for validar o primeiro caso real com um negócio de verdade.

---

## 3. Semana 1 — a peça de portfólio não-negociável (11:00–17:00)

> Aqui é onde a aula para de ser conceito e vira prazo. Semana 1, sete dias, uma entrega. Não é sugestão — é o compromisso mínimo desse curso.
>
> O formato que comprovadamente funciona pra isso é o de **desafio estruturado de 7 dias**: aulas curtas e sequenciais, um passo por dia, presumindo que você não sabe nada — sem pular passos, sem jargão. No fim dos sete dias, o resultado é um agente de atendimento ao cliente funcional: ele lê a mensagem que chega, busca numa base de políticas da empresa (usando um vector store simples — RAG básico — pra organizar e recuperar essa informação), e escreve um rascunho de resposta direto no Gmail, esperando aprovação humana antes de sair. A stack típica desse primeiro agente: entender a diferença entre usar o ChatGPT e usar a API da OpenAI, montar o fluxo no n8n Cloud, conectar OpenAI ao n8n, e integrar com o Gmail pra gerar rascunhos automáticos.
>
> Você não precisa seguir exatamente esse projeto, mas ele é um ótimo modelo de estrutura porque resolve um problema de design pedagógico difícil: quebra uma tarefa que parece grande — "construir um agente de IA" — em passos diários pequenos o suficiente pra não gerar overwhelm, mas que somados entregam algo real e demonstrável. Use esse mesmo princípio mesmo que você use outra stack: n8n, Make, Zapier com IA, ou até um script simples com a API da Anthropic ou da OpenAI. O que importa não é a ferramenta, é o resultado no dia 7.
>
> O que precisa estar de pé no fim da Semana 1, sem exceção:
>
> **Um.** Uma automação que roda de ponta a ponta sem você precisar ficar clicando em cada etapa manualmente. Trigger → processamento com IA → saída utilizável.
>
> **Dois.** Um caso de uso real, mesmo que fictício ou aplicado a um negócio de mentira — não pode ser "oi, tudo bem, teste 123". Tem que ser algo que, se você mostrasse pra um dono de pequena empresa, ele entenderia em 10 segundos o que aquilo resolve.
>
> **Três.** Prova visual de que funciona — print de tela, gravação de execução, ou o próprio agente rodando ao vivo. Isso vira matéria-prima do exercício desta aula, que é o vídeo de antes/depois. Guarda essa gravação, você vai precisar dela literalmente no exercício de hoje.
>
> Se no fim do dia 7 você tem isso rodando, você já está, sem exagero, na frente da maioria absoluta de gente que "estuda automação de IA" há meses. Porque você tem prova. Eles têm conhecimento.

---

## 4. Semanas 2–3 — validar com um caso real (17:00–24:00)

> A peça de portfólio da Semana 1 prova uma coisa: que você consegue construir. Ela não prova a segunda coisa, que é igualmente importante: que alguém, de carne e osso, com um negócio de verdade, tem esse problema e pagaria pra resolver.
>
> É aqui que entra um modelo de fases consagrado no mercado de agências de automação: **Aprender → Aplicar → Monetizar**. Você acabou de fazer a fase "Aprender" comprimida numa semana. Semanas 2 e 3 são a fase "Aplicar" — e a regra mais específica e mais útil dessa fase é numérica: **feche pelo menos três entregas de projeto de graça antes de sequer cogitar cobrar**. Não é caridade, é calibração — você está testando se o que você constrói sozinho, numa tela, sobrevive ao contato com um processo real de negócio, com dados reais, exceções reais e um cliente real com expectativas.
>
> Três projetos grátis em duas semanas é ambicioso mas não é loucura, porque a barra de "grátis" é mais baixa do que a de "pago": você está pedindo pra alguém te dar um problema real e 30 minutos de paciência, não pedindo cartão de crédito. Isso muda completamente quem topa.
>
> Como achar esses três? Duas rotas, que formam o funil de aquisição clássico de quem está começando nesse tipo de negócio:
>
> **Rota 1 — warm outreach (contato direto com quem você já conhece).** Antes de qualquer estratégia de prospecção fria, esgote sua rede. Dono de empresa que é seu vizinho, ex-colega que agora tem um negócio próprio, grupo de WhatsApp da família com alguém que tem uma clínica, uma loja, um escritório de contabilidade. A mensagem é simples e sem enrolação: "Eu tô aprendendo a automatizar processos com IA e queria testar num negócio de verdade, de graça. Você tem alguma tarefa chata e repetitiva que eu poderia tentar resolver essa semana?" Repare que essa frase já embute o filtro da Seção 2 — repetitivo, com regra clara, que consome tempo.
>
> **Rota 2 — plataformas de freelancer, mesmo sem reputação ainda.** Upwork é o canal clássico desse funil no mercado global. Pro público brasileiro, os equivalentes locais são a **Workana** (a maior da América Latina, com projetos em português e espanhol) e o **99freelas** — mesma lógica, menos concorrência global e sem barreira de idioma. A diferença nas primeiras semanas é que você não está competindo em preço — está oferecendo o primeiro projeto de graça ou a preço simbólico, exatamente pra construir prova social e portfólio, não pra faturar ainda.
>
> O objetivo dessas duas semanas não é "ganhar dinheiro". É sair de "eu construí uma coisa sozinho" para "eu construí uma coisa que resolveu um problema de alguém que não sou eu". Essa frase — dita por um cliente real, mesmo que informalmente, tipo "nossa, isso vai economizar um tempão" — vale mais nessa fase do que qualquer certificado.
>
> Uma calibração importante de expectativa: os primeiros projetos grátis não precisam — e realisticamente não vão — produzir um resultado polido e perfeito pro cliente. Eles existem de propósito pra aprendizado, não pra excelência. O ponto não é impressionar, é aprender onde o mundo real quebra as suas suposições — e coletar a segunda peça de prova que você vai precisar na Semana 4.

---

## 5. Semana 4 — empacotar e transformar em oferta (24:00–28:00)

> Você chega na Semana 4 com duas coisas na mão: a automação da Semana 1, que prova que você constrói; e pelo menos um, idealmente dois ou três, casos reais de aplicação das Semanas 2 e 3, que provam que o que você constrói resolve problema de gente de verdade.
>
> Essa semana não é sobre construir mais nada novo. É sobre **empacotar** o que já existe em algo que uma pessoa que não te conhece consiga entender em menos de um minuto.
>
> Empacotar, na prática, quer dizer três entregáveis simples:
>
> **Um resumo de uma frase do que você faz**, no formato "eu ajudo [tipo específico de negócio] a [resolver problema específico] usando automação com IA, sem precisar contratar mais gente." Não "eu faço automações com IA" — isso não diz nada pra ninguém. Precisa ser específico o bastante pra alguém pensar "ah, isso é literalmente o meu problema".
>
> **Um antes/depois de cada caso**, mesmo que seja simples: "antes, essa tarefa levava X minutos/horas por dia e dependia de uma pessoa; depois, leva Y segundos e roda sozinha, com revisão humana só quando necessário." Números concretos, mesmo estimados, valem muito mais que adjetivo.
>
> **Uma oferta inicial de preço baixo**, seguindo a lógica da fase "Monetizar": feche o primeiro cliente pago a um valor propositalmente baixo pra reduzir a barreira de decisão dele, e suba o preço à medida que o portfólio de casos reais cresce. Preço baixo no começo não é desvalorização — é o custo de aquisição da sua primeira prova paga.
>
> No fim da Semana 4, você não tem uma "agência". Você tem uma oferta testada, com prova, num preço que reduz o risco de dizer sim. Isso já é infinitamente mais do que 95% de quem começa esse caminho tem depois de um mês.

---

## 6. O que ignorar deliberadamente (28:00–31:30)

> Agora a parte que mais economiza tempo dessa aula inteira: o que você tem permissão explícita pra ignorar nos primeiros 30 dias. Não porque essas coisas não importem nunca, mas porque elas são, comprovadamente, os três jeitos mais comuns de transformar 30 dias de ação em 90 dias de procrastinação disfarçada.
>
> **Ignore a busca pela ferramenta perfeita.** N8n, Make, Zapier, um script direto com API — qualquer uma resolve um primeiro projeto bem escolhido. Trocar de ferramenta no meio do caminho porque "essa outra parece mais poderosa" é o jeito mais popular de nunca terminar nada. Escolha uma no dia 1 e não troque até pelo menos terminar o primeiro caso real.
>
> **Ignore construir marca antes de funcionar.** Logo, identidade visual, nome de agência rebuscado, site institucional bonito — tudo isso pode esperar. Nos primeiros 30 dias, sua "marca" é uma automação que funciona e um print de antes/depois. Ninguém contrata baseado em paleta de cores.
>
> **Ignore colecionar certificações antes de lançar.** Certificado de curso de automação, de prompt engineering, de "especialista em IA" — eles não substituem prova. Um agente funcionando publicamente vale mais, pra quem paga, do que qualquer selo. Se sobrar tempo depois do dia 30, aí sim certificação pode fazer sentido como reforço, nunca como pré-requisito.
>
> O padrão comum das três armadilhas é o mesmo da Seção 1: são formas socialmente aceitáveis de adiar a exposição pública. Ferramenta perfeita, marca bonita e certificado são todos "trabalho" que parece produtivo mas não te obriga a mostrar nada pra ninguém. Os 30 dias desse plano são desenhados pra não deixar essa saída disponível.

---

## 7. Ferramentas e modelos mentais citados

| Item | O que é | Onde entra no plano de 30 dias |
|---|---|---|
| Desafio estruturado de 7 dias | Formato de aprendizagem: aulas curtas e sequenciais, um passo por dia, terminando com um agente funcional | Modelo de estrutura pra Semana 1 |
| n8n Cloud | Plataforma de automação low-code | Ferramenta sugerida (não obrigatória) pra Semana 1 |
| Make / Zapier | Alternativas low-code de automação | Opções equivalentes pra Semana 1 |
| API da OpenAI / da Anthropic | Acesso programático aos modelos (diferente de usar o chat) | Motor de IA do primeiro agente; alternativa via script |
| Vector store / RAG básico | Técnica de organizar e recuperar informação (ex.: políticas da empresa) pra alimentar o agente | Técnica usada no agente de atendimento da Semana 1 |
| Gmail (integração) | Geração de rascunhos automáticos com revisão humana antes do envio | Saída do agente da Semana 1 |
| Roadmap Aprender → Aplicar → Monetizar | Estrutura em 3 fases: aprender, aplicar em projetos grátis, monetizar | Modelo mental central das Semanas 2–4 |
| Regra "3 entregas grátis antes de cobrar" | Validar com pelo menos 3 casos reais sem cobrar antes de precificar | Critério de saída das Semanas 2–3 |
| Warm outreach + plataformas freelance | Dois canais clássicos pra achar os primeiros casos/clientes (Upwork; no Brasil, Workana e 99freelas) | Tática de aquisição das Semanas 2–3 |
| Critério "repetitivo + regra clara + consome tempo" | Filtro de três perguntas pra escolher o primeiro projeto | Usado na Seção 2 pra escolher o que construir |

---

## EXERCÍCIO — passo a passo (31:30–34:00 na aula; prazo de execução: 7 dias)

> Antes de fechar, deixa eu te apresentar o exercício — porque essa aula não termina quando o vídeo acaba, ela termina quando você entregar três coisas nos próximos 7 dias. Vou mostrar cada uma rapidinho; o passo a passo completo fica no material de apoio.

**Objetivo do exercício:** produzir, até o fim da Semana 1, as três provas que você vai precisar pro resto do plano de 30 dias.

**Entrega 1 — automação funcional simples.**
1. Escolha uma tarefa aplicando o filtro da Seção 2: é repetitiva? tem regra clara? consome tempo real de alguém?
2. Escolha uma ferramenta e não troque até terminar (n8n, Make, Zapier com IA, ou script direto com API da Anthropic/OpenAI).
3. Monte o fluxo mínimo: gatilho → processamento com IA → saída utilizável (mensagem, e-mail, planilha atualizada, resposta gerada). Se quiser seguir o modelo clássico quase à risca, use um caso de atendimento ao cliente com resposta redigida por IA e revisão humana antes do envio.
4. Teste rodando do início ao fim pelo menos três vezes com entradas diferentes, incluindo um caso de borda (uma pergunta ou situação fora do padrão) — se quebrar, ajuste antes de seguir.

**Entrega 2 — vídeo de 60 a 90 segundos, antes/depois.**
1. Grave a tela mostrando como a tarefa era feita manualmente (ou descreva em texto/narração se for um processo que hoje nem existe formalizado) — 15 a 20 segundos.
2. Grave a automação rodando de ponta a ponta, do gatilho até o resultado final — 30 a 45 segundos.
3. Feche com uma frase de resultado objetivo: "isso levava X minutos e agora leva Y segundos" ou equivalente em esforço humano evitado.
4. Não precisa de edição sofisticada — celular ou gravação de tela simples resolve. O que importa é existir e ser mostrável.

**Entrega 3 — frase de posicionamento aplicada a um negócio real.**
1. Pegue a estrutura: "Eu ajudo [tipo específico de negócio] a [resolver problema específico] usando automação com IA."
2. Aplique essa frase a um negócio real que você conhece (não hipotético) — pode ser o mesmo negócio usado na automação, ou um alvo diferente que você já tem em mente para a Semana 2.
3. Teste a frase em voz alta com uma pessoa que não é da área de tecnologia. Se ela entender o que você faz em menos de 10 segundos, a frase está pronta. Se ela perguntar "mas o que isso faz exatamente?", refine.

Ao fim dos 7 dias, guarde as três entregas — elas viram, literalmente, sua munição de warm outreach na próxima etapa do plano.

---

## FECHAMENTO — recap e ponte (34:00–37:00)

> Vamos recapitular rápido. O erro mais comum de quem entra em automação de IA é confundir consumir conteúdo com fazer progresso — e o antídoto é um prazo de lançamento não-negociável. Você escolhe o primeiro projeto com três perguntas objetivas: é repetitivo, tem regra clara, consome tempo real. Na Semana 1 você constrói e lança uma peça de portfólio funcional, no formato de desafio de 7 dias — passo a passo, sem pular etapa. Nas Semanas 2 e 3, você valida isso com casos reais, mirando pelo menos três entregas grátis, usando warm outreach e plataformas de freelancer, seguindo a lógica Aprender → Aplicar → Monetizar. Na Semana 4, você empacota tudo isso numa frase de posicionamento, num antes/depois concreto e numa oferta de entrada com preço baixo. E o tempo todo, você ignora deliberadamente três armadilhas: ferramenta perfeita, marca antes de funcionar, e certificação antes de lançar.
>
> Se você seguir esse plano à risca, no dia 30 você não tem uma "agência de IA" — isso ainda vem depois. Mas você tem algo muito mais raro do que 95% de quem entra nesse mercado consegue construir num mês: uma automação real, provas de que ela resolveu problema de gente de verdade, e uma frase que qualquer pessoa entende em 10 segundos.
>
> E é exatamente aí que a próxima aula começa. Porque essa primeira automação que você vai construir — em n8n, Make, Zapier ou o que escolher — funciona, mas tem um teto. Quando os casos reais começarem a se acumular, você vai sentir as costuras do low-code: custo por execução, dificuldade de debugar, impossibilidade de versionar direito. Na próxima aula, a gente encara isso de frente: como saber a hora certa de migrar de low-code pra código de verdade, o modelo mental de 3 camadas que sustenta qualquer sistema de IA em produção, e o esqueleto mínimo de um backend que aguenta cliente de verdade. Até lá — bota a mão na massa nos próximos 7 dias. Essa aula só vale alguma coisa se você sair dela com uma automação rodando.
