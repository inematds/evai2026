# Aula 8 — Serviços de IA para empresas enterprise

**Curso:** Como montar um negócio de serviços de IA
**Bloco:** Técnico — esta é a última aula do bloco (tópico 8 de 8). Depois dela o curso muda de registro: entra o bloco de visão estratégica do mercado.
**Duração estimada:** 48-52 minutos

---

## ABERTURA / GANCHO (0:00 – 3:00)

Fala direto pra câmera.

Toda essa jornada até aqui te ensinou a construir. Agente que funciona, automação que roda, evals que provam que não quebra, um workspace que parece uma agência de verdade. O que ninguém te contou ainda é o seguinte: nada disso vale nada se você não sabe vender pra quem tem orçamento de sete dígitos.

E aqui vai uma verdade que o mercado esconde: quem se senta em reunião com as maiores empresas do planeta — farmacêuticas, bancos, seguradoras gigantes — muitas vezes não veio de Big Four nem de big tech. Veio de fora, de uma consultoria pequena, de uma carreira técnica comum. Essa pessoa não chegou lá porque tinha o discurso mais bonito de IA. Ela chegou lá porque sabia navegar a burocracia, os atores e a linguagem que uma conta enterprise fala. É uma habilidade aprendível — e é exatamente o que a gente vai destrinchar agora.

Essa é a aula de hoje. Não é sobre ferramenta, não é sobre prompt, não é sobre agente. É sobre a parte que decide se o seu trabalho técnico vira contrato de seis dígitos ou fica andando de projeto de dois mil reais em dois mil reais pra sempre. É a última aula do bloco técnico do curso — depois dela a gente muda de marcha e entra no bloco de visão estratégica do mercado. Mas antes de sair do "mão na massa", falta a parte que separa quem entrega bem de quem fatura bem: vender pra empresa grande.

---

## 1. O que Fortune 500 compra em IA agora — e o que ignora (3:00 – 8:00)

Primeira correção de rota mental, e ela é brutal: empresa grande não compra tecnologia. Comprador enterprise de verdade — o cara que assina o cheque — não quer saber se você usou Claude, GPT, um agente com RAG ou um pombo-correio treinado. Ele quer saber uma coisa: **isso reduz um número que eu sou cobrado por reduzir?**

Isso muda tudo na forma de se posicionar. Se você chega falando "eu construo agentes de IA com tal stack", você está falando a língua errada pro comprador certo. Se você chega falando "eu reduzo o tempo médio de abertura de sinistro de 9 dias pra 36 horas, e isso impacta diretamente a taxa de retenção de clientes que o VP de operações reporta pro board", agora você está no jogo dele.

E aqui está o que Fortune 500 ignora, deliberadamente, na sua cara: demonstração bonita sem prova de produção. Ignora "estamos explorando IA de forma inovadora". Ignora ferramenta genérica sem verticalização — eles já têm dez fornecedores empurrando "copilot pra tudo". O que faz o comprador enterprise parar de rolar o slide e prestar atenção é isto: **um resultado de negócio específico, com métrica, ligado a um problema que já está no radar de alguém com orçamento.**

Pensa no exemplo que vamos usar a aula inteira: uma seguradora de porte médio-grande, chama ela de Grupo Assist Seguros. Ninguém lá acorda pensando "preciso de um agente de IA". Alguém lá acorda pensando "meu índice de leakage de fraude em sinistros está 2 pontos percentuais acima da média do setor, e isso é uma linha no relatório trimestral que me deixa mal na foto". Se a sua proposta fala a língua de "agente conversacional multimodal com function calling", você perdeu essa pessoa no primeiro parágrafo. Se ela fala "redução de leakage de fraude e tempo médio de triagem", você já tem a atenção.

Isso não é opinião isolada — é o consenso de qualquer metodologia de venda B2B madura, e é exatamente o pano de fundo que os dois frameworks que vamos ver daqui a pouco (MEDDIC e Challenger Sale) foram desenhados pra resolver: vender resultado de negócio amarrado a métrica que já importa pra alguém com poder de decisão, nunca tecnologia pela tecnologia.

---

## 2. Os atores da decisão de compra enterprise (8:00 – 13:00)

Segunda correção de rota: numa venda pequena, você fala com uma pessoa. Numa venda enterprise, você está entrando numa sala com gente que quer coisas diferentes e às vezes conflitantes entre si. Se você não souber quem é quem, sua proposta perfeita morre na mesa de alguém que nunca nem te respondeu.

Os papéis clássicos, e o que cada um realmente quer:

- **Economic buyer (comprador econômico)** — a pessoa que pode dizer sim quando todo mundo diz não, e dizer não quando todo mundo diz sim. No Grupo Assist Seguros, é o CFO ou o VP de Operações. Ele não quer saber de arquitetura. Ele quer ROI, prazo de payback e risco.
- **Technical buyer (comprador técnico)** — normalmente o Head de TI ou de Dados. Ele não aprova o orçamento, mas pode vetar tudo se achar que a solução é insegura, não escala ou não integra com o que já existe. Ele fala de LGPD, de arquitetura, de quem vai manter aquilo depois que você for embora.
- **User buyer (comprador usuário)** — quem vai usar a ferramenta no dia a dia. No nosso exemplo, é o gerente de sinistros e a equipe de triagem. Se essa pessoa não confiar na ferramenta ou achar que ela ameaça o trabalho dela, ela sabota silenciosamente a adoção — e isso mata o projeto meses depois da assinatura, quando ninguém mais está prestando atenção.
- **Procurement (compras)** — o time que existe pra reduzir risco de contrato e espremer preço. Não é seu inimigo, mas também não está do seu lado — o trabalho dele é literalmente questionar tudo que você propõe.
- **Jurídico/compliance** — em setores regulados (o seu exemplo de seguros é um deles, assim como saúde e finanças), esse ator pode travar o projeto inteiro por causa de dado sensível, LGPD, ou cláusula de responsabilidade civil sobre decisão automatizada.

O erro mais caro que agência pequena comete: fechar o discurso todo com o technical buyer — porque é quem te entende, quem fala a mesma língua que você — e nunca chegar no economic buyer. Você sai da reunião achando que "foi ótimo", e três meses depois descobre que ninguém com orçamento sequer sabia que a conversa existia.

Esse mapeamento de papéis (economic/technical/user buyer, procurement, jurídico) é vocabulário padrão de venda B2B complexa, usado por praticamente toda metodologia séria de vendas enterprise — é o pano de fundo em cima do qual o MEDDIC constrói o "E" de Economic Buyer.

---

## 3. A conversa de procurement na prática (13:00 – 18:00)

Aqui é onde agência pequena costuma travar de vez — não porque a solução seja ruim, mas porque procurement pergunta cinco coisas que ninguém preparou resposta.

**RFP (Request for Proposal).** Empresa grande frequentemente formaliza a compra num documento estruturado, com seções obrigatórias, prazo de resposta e critério de pontuação. Se você nunca respondeu um RFP, o primeiro choque é perceber que ele não pergunta "o que sua IA faz" — pergunta coisa como "descreva seu processo de gestão de incidentes", "qual seu SLA de suporte", "como vocês tratam dado pessoal em trânsito e em repouso". Você precisa ter essas respostas prontas antes de precisar delas.

**SOC 2.** É um relatório de auditoria (Tipo I ou Tipo II) sobre controles de segurança, disponibilidade e confidencialidade de uma empresa que processa dado de terceiros. Procurement de empresa grande vai perguntar se você tem. Se você é uma agência de três pessoas e não tem SOC 2, isso não te desqualifica automaticamente — mas você precisa ter uma resposta madura: quais controles de segurança você tem hoje, qual seu plano pra certificação, e como você isola o dado do cliente enquanto isso. "Ainda não temos, mas aqui está nosso protocolo de segurança e nosso plano de 12 meses" é uma resposta que sobrevive. Silêncio ou "não sei o que é isso" mata o negócio na hora.

**Referências.** Empresa grande quase nunca compra de agência sem case anterior verificável — de preferência em setor parecido. Isso significa que sua primeira conta enterprise de verdade é sempre a mais difícil de fechar, e é por isso que a seção 6 desta aula (nicho e prova social) importa tanto.

**SLA (Service Level Agreement).** Tempo de resposta a incidente, uptime garantido, penalidade contratual se você não cumprir. Empresa pequena promete "resolvo rápido". Empresa grande exige número: "tempo de resposta a incidente crítico: 4 horas úteis. Uptime mensal: 99,5%. Abaixo disso, crédito contratual de X%."

A regra de ouro aqui: você não precisa ter tudo isso perfeito pra entrar na conversa. Você precisa **não ser pego de surpresa** quando perguntarem, e ter uma resposta honesta e madura pra cada um desses pontos, mesmo que a resposta seja "ainda não, e aqui está o plano".

---

## 4. Framework MEDDIC aplicado a projetos de IA (18:00 – 26:00)

Aqui muda o registro da aula: de "como a compra enterprise funciona" pra "como você qualifica e conduz a venda dentro disso". E aqui entra o primeiro framework de venda B2B consagrado que vamos aplicar: o **MEDDIC**, criado nos anos 1990 dentro de uma grande empresa de software americana por executivos de vendas que analisaram, negócio a negócio, por que contratos fechavam ou morriam. Virou, com o tempo, MEDDPICC (as versões mais recentes adicionam Paper Process e Competition), mas o núcleo de seis letras é o que você precisa dominar primeiro.

A lógica do MEDDIC é simples: ele não é um script de venda, é um **filtro de qualificação**. Ele te diz, projeto por projeto, se aquele negócio tem chance real de fechar ou se você está perdendo tempo com gente que nunca vai assinar. Isso é ouro pra agência pequena, porque o recurso mais escasso que você tem não é dinheiro, é tempo de founder gasto em proposta que não vai pra frente.

Vamos aplicar cada letra ao caso do Grupo Assist Seguros — o projeto de triagem de sinistros com IA.

| Letra | O que significa | Aplicado ao Grupo Assist Seguros |
|---|---|---|
| **M — Metrics** | As medidas quantificáveis de valor que sua solução entrega. Precisa ser número, não adjetivo. | Reduzir tempo médio de triagem de sinistro de 9 dias pra 36 horas. Reduzir leakage de fraude em 1,5 ponto percentual em 12 meses. |
| **E — Economic Buyer** | Quem tem autoridade final — pode dizer sim quando todo mundo diz não. | VP de Operações ou CFO do Grupo Assist. Se você nunca falou com essa pessoa, o negócio ainda não está qualificado de verdade. |
| **D — Decision Criteria** | Os critérios formais (e informais) que a organização usa pra decidir entre fornecedores. | Segurança de dado (LGPD, dado de saúde/sinistro), integração com o sistema de sinistros já existente, tempo de implementação, referência em seguradora similar. |
| **D — Decision Process** | A sequência real de etapas e aprovações até a assinatura. | Piloto aprovado pelo Head de TI → validação jurídica de tratamento de dado → aprovação orçamentária do CFO → assinatura formal via procurement. |
| **I — Identify Pain** | A dor identificada, indicada pelo cliente e implicada (você mostrou a ele o custo real de não agir). | Não é só "sinistro demora" — é "cada dia a mais de triagem custa retenção de cliente e alimenta reclamação regulatória". Você precisa levar o cliente a admitir isso, não só observar de fora. |
| **C — Champion** | Alguém de dentro da organização com poder de influência e interesse pessoal em você ganhar — que briga por você quando você não está na sala. | O gerente de sinistros que está cansado de apagar incêndio manualmente e vê a ferramenta como alívio pro próprio time, não como ameaça. |

O ponto prático de MEDDIC: se você não consegue preencher pelo menos quatro dessas seis linhas com informação real (não achismo), você não tem um negócio qualificado — você tem uma conversa. E conversa não paga conta.

---

## 5. Challenger Sale aplicado a vender IA (26:00 – 33:00)

Segundo framework consagrado: a metodologia **Challenger Sale**, nascida de uma pesquisa ampla com milhares de vendedores B2B, publicada no livro *The Challenger Sale* e mantida hoje como metodologia comercial estabelecida. A pesquisa original mapeou os vendedores em cinco perfis: o **Hard Worker** (21% dos vendedores — vai além, mas em vendas complexas isso sozinho não basta), o **Relationship Builder** (21% — cliente pede ele pelo nome, mas constrói consenso demais e desafia de menos), o **Lone Wolf** (18% — confia no próprio instinto, ignora processo), o **Problem Solver/Reactive** (14% — resolve problema depois que ele aparece, é reativo) e o **Challenger** — o perfil que a pesquisa mostra ser desproporcionalmente mais comum entre os que batem meta em venda complexa.

O que um Challenger faz de diferente não é ser mais agressivo. É isto: em vez de responder exatamente o que o cliente pediu, ele **reformula o problema** antes de responder. A metodologia chama isso de **Commercial Teaching** — ensinar o cliente a enxergar o próprio negócio de um jeito que ele ainda não tinha considerado, criando o que o framework chama de **tensão construtiva**: o desconforto produtivo de perceber que o jeito atual de operar tem um custo que ninguém tinha nomeado.

Aplica isso à venda de IA, com o mesmo exemplo. O Grupo Assist Seguros te chama e diz: "queremos um chatbot pra atendimento ao segurado." Pedido literal: construir um chatbot. Resposta de vendedor **não-Challenger**: cotar um chatbot.

Resposta Challenger: "Antes de falar de chatbot — vocês sabem que o custo real não está no atendimento, está no tempo de triagem do sinistro depois que o segurado liga? Um chatbot bonito na ponta de entrada não resolve nada se o gargalo continua nos 9 dias de análise manual atrás dele. O problema que vocês estão me trazendo é sintoma. O problema real é outro, e é maior — e mais caro de ignorar do que vocês pensam." Isso é reframe: você não respondeu o pedido, você redefiniu o problema, e só depois disso apresentou uma solução — que agora é maior, mais estratégica, e vale mais caro do que "fazer um chatbot".

Três coisas importam pra isso funcionar e não soar arrogante:
1. **Teach** — você só pode ensinar se realmente entender o negócio do cliente melhor do que ele esperava. Isso exige pesquisa antes da reunião, não improviso na hora.
2. **Tailor** — a mesma mensagem muda de forma dependendo de quem está ouvindo. Pro CFO, o reframe é sobre custo e retenção. Pro Head de TI, é sobre risco de integração. Pro gerente de sinistros, é sobre carga de trabalho.
3. **Take Control** — depois de ensinar, você precisa conduzir a conversa até uma decisão, inclusive falando de dinheiro e prazo sem medo. Challenger não é só ter o insight, é não recuar quando chega a hora de pedir a assinatura.

O diálogo do chatbot acima é uma ilustração didática — o que importa é o padrão: ensinar, adaptar a mensagem por ator, e conduzir até a decisão.

---

## 6. Como agência pequena vence firma 10x maior (33:00 – 38:00)

Você não vai vencer Accenture ou Deloitte em orçamento, em quantidade de gente, em marca. Não tenta. Existem três eixos onde agência pequena genuinamente ganha, e eles não são consolo — são vantagem estrutural real:

**Nicho.** Firma grande vende "transformação digital com IA" pra qualquer setor. Você vende "triagem de sinistro com IA pra seguradoras de médio porte". Quando o comprador em procurement lê sua proposta ao lado da proposta genérica da consultoria gigante, a sua parece escrita especificamente pro problema dele — porque foi. Isso pesa mais do que logo grande em decisão técnica.

**Velocidade.** Firma grande tem processo de venda de seis a doze meses e implementação em fases que se arrastam por trimestres, com camadas de gerência entre quem decide e quem executa. Você entrega um piloto funcional em três semanas. Num mundo onde o board cobra resultado no próximo trimestre, isso é argumento comercial, não só técnico.

**Prova social específica.** Não adianta ter case de "fizemos IA pra uma empresa". Adianta ter "reduzimos leakage de fraude em X% numa seguradora de porte parecido, em Y semanas". Case genérico não convence procurement. Case específico, com número, no mesmo setor, é a coisa mais próxima de "prova" que existe numa venda B2B.

A armadilha a evitar: tentar parecer maior do que é. Não minta sobre tamanho de equipe, não infle case. O jogo de agência pequena vencendo firma grande não é fingir ser grande — é ser inegavelmente melhor no específico que o cliente precisa agora.

---

## 7. Estrutura de proposta e precificação enterprise (38:00 – 44:00)

Última peça antes do exercício: como transformar tudo isso em documento e número.

**Estrutura da proposta**, nessa ordem — e a ordem importa, porque ela espelha como o economic buyer lê:
1. **O problema reformulado** (seu reframe Challenger, não o pedido literal do cliente)
2. **O resultado de negócio prometido**, em métrica (seu M do MEDDIC)
3. **Como você chega lá** — abordagem técnica, mas resumida, sem jargão de stack
4. **Prova** — case específico e comparável
5. **Estrutura de entrega e prazo**
6. **Segurança e conformidade** — sua resposta madura ao que procurement vai perguntar (seção 3)
7. **Investimento**, em duas fases
8. **Próximo passo claro** — não "fico à disposição", e sim uma data e uma ação específica

**Precificação ancorada em métrica, não em hora.** Cobrar por hora convida o comprador a comparar sua hora com a hora de qualquer outro fornecedor — é a pior unidade de negociação que existe. Cobrar ancorado no resultado ("valor economizado", "receita protegida", "risco reduzido") muda a conversa de "quanto custa" pra "quanto vale".

**Preço em duas fases**, o modelo mais robusto pra vender IA enterprise sem virar refém de escopo infinito:

- **Fase 1 — Diagnóstico pago.** Um engajamento curto (2 a 4 semanas), pago, de escopo fechado: mapear o processo atual, validar a dor real (seu "Identify Pain"), estimar o ganho potencial em número, e — crucial — construir um protótipo mínimo que prova viabilidade técnica. Isso resolve dois problemas de uma vez: você recebe pra fazer o trabalho que normalmente daria de graça numa proposta comercial, e o cliente reduz o risco de assinar um contrato enorme às cegas.
- **Fase 2 — Implementação, ancorada no resultado do diagnóstico.** Agora o preço não é mais estimado, é calculado em cima do número validado na Fase 1: "reduzir leakage de fraude em 1,5 ponto percentual representa R$ X de impacto anual — nosso preço de implementação é uma fração conhecida e defensável desse valor", com um componente fixo (cobre seu custo e risco de entrega) e, quando fizer sentido, um componente variável ligado à métrica atingida.

Esse modelo em duas fases faz o trabalho de procurement por você: ele naturalmente incorpora RFP, prova de conceito e negociação de preço numa sequência que a empresa grande já está acostumada a seguir — só que com você no controle da narrativa, porque foi você quem definiu a métrica na Fase 1.

---

## 8. Exercício: one-pager de proposta usando MEDDIC + mapa de atores + reframe + preço em 2 fases (44:00 – 52:00)

Agora a parte prática. Escolha um cliente real ou hipotético de porte médio-grande (não precisa ser Fortune 500 de verdade — pode ser a maior empresa do seu mercado local, ou uma que você gostaria de fechar). O entregável é **um one-pager** — uma página, não um documento de vinte. Empresa grande decide rápido quando o documento é curto e denso, e trava quando é longo e genérico.

Passo a passo:

**Passo 1 — Escreva o pedido literal do cliente em uma frase.**
Exemplo: "Eles pediram um chatbot de atendimento ao cliente."

**Passo 2 — Reformule (reframe Challenger).**
Em duas ou três frases, reescreva qual é o problema real por trás do pedido — o que você descobriria perguntando "por quê" três vezes. Não é o que eles pediram, é o que está custando dinheiro/risco/tempo de verdade.

**Passo 3 — Preencha as seis linhas do MEDDIC**, uma frase cada, sem enrolação:
- Metrics: qual número muda, de quanto pra quanto
- Economic Buyer: nome do cargo (ou nome real, se souber)
- Decision Criteria: 2-3 critérios que essa empresa provavelmente usa pra escolher fornecedor
- Decision Process: as etapas até a assinatura, na ordem
- Identify Pain: a dor real, na palavra do cliente e na sua leitura mais funda dela
- Champion: quem, de dentro, ganharia pessoalmente se isso for aprovado

**Passo 4 — Mapeie os atores em uma linha cada.**
Economic buyer / technical buyer / user buyer / procurement / jurídico — o que cada um quer ouvir, em uma frase.

**Passo 5 — Escreva o preço em duas fases.**
Fase 1: escopo e prazo do diagnóstico pago, e valor cobrado.
Fase 2: como o preço da implementação será calculado a partir do resultado da Fase 1 — não um número fixo genérico, e sim a lógica do cálculo.

**Passo 6 — Monte tudo em uma página só.**
Título com o nome do cliente e o resultado prometido (não o nome da tecnologia). Seções curtas na ordem da seção 7 desta aula. Termine com uma data e uma ação concreta pro próximo passo — nunca "fico à disposição".

**Critério de qualidade do exercício:** se alguém de fora, lendo só o one-pager, não conseguir dizer em 30 segundos qual número essa proposta promete mudar, refaça o Passo 1 e 2 — o reframe não ficou afiado o suficiente.

---

## FECHAMENTO — ponte pro próximo bloco (52:00 – fim)

Essa foi a última aula do bloco técnico. Você já sabe, agora, construir o sistema, testar antes de entregar, rodar uma operação inteira sozinho, escalar frota de agente, e — com a aula de hoje — vender isso pra quem tem orçamento grande de verdade, sem se atropelar na burocracia de procurement e sem entrar na sala sem saber quem manda no quê.

O que vem agora muda de natureza. As próximas aulas não são mais tutorial — são visão de mercado: o que separa quem sobrevive de quem é engolido quando a poeira da hype baixar, os erros de precificação que estão matando agência agora, o caminho de contrato pra produto, a tese de quem vai vender a própria agência daqui a alguns anos, e a pergunta mais incômoda de todas: o que realmente importa em IA agora, descontando o hype. Você tem a ferramenta. A partir da próxima aula, você constrói o discernimento pra saber onde apontar ela.
