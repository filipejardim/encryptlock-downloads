# EncryptLock — White Paper e Especificação Técnica (v0.5)

> Substitui a v0.2. Este documento explica o projeto e as suas regras.
> Marcas: confirmar no EUIPO se "EncryptLock", "LockKeeper" e o símbolo "LOCK" estão livres antes do lançamento público.
>
> **O que mudou na v0.5 (12 out 2026):** **reserva eterna dos LockKeepers** — 800.000 LOCK (2%) ficam fora dos 30 anos (que passam a 38.799.000 LOCK) e, depois do ano 30, o que resta da reserva cai para metade a cada 30 anos: 99,9% até ao ano 330 e o último LOCK cerca de 1.440 anos depois do início; o total continua 40.000.000. **Fees:** 75/25 nos primeiros 5 anos e depois 50/50 para sempre (antes, se a rede chegar ao marco final).
>
> **O que mudou na v0.4 (7 out 2026):** 1% da oferta máxima (**400.000 LOCK**) para o criador, público desde o primeiro bloco, como recompensa por criar a blockchain; a emissão dos 30 anos passa a 39.599.000 LOCK para o total continuar 40.000.000.
>
> **O que mudou na v0.3:** o EncryptLock passa a ser um projeto totalmente independente (sem regras especiais para nenhuma app); símbolo **LOCK**; fee paga pelo remetente; registo de contas; regra de aceitação definida para envios entre contas; teto de 5% liga-se aos 20 LockKeepers; alocação génese de 1.000 LOCK; moedas de batoteiros queimadas; fundo de desenvolvimento com chave múltipla; 4 LockKeepers no arranque; notas honestas sobre o que a rede consegue ou não garantir.

---

## 0. Decisões fechadas (resumo rápido)

| Tema | Decisão |
|---|---|
| Nome da moeda e da carteira | **EncryptLock** |
| Símbolo | **LOCK** |
| Nome do programa da rede (nó/validador) | **LockKeeper** |
| Independência | Projeto próprio. Nenhuma app tem regras, fees ou privilégios especiais na rede |
| Modelo | Sem empresa, sem licenças, custo zero. Lançamento justo e transparente |
| Oferta máxima | **40.000.000** LOCK, 8 casas decimais |
| Alocação génese | **401.000 LOCK**: 1.000 para os primeiros LockKeepers arrancarem a rede e **400.000 (1%) para o criador**, público |
| Emissão | Durante **30 anos** (360 meses), curva suave que cai para metade a cada 5 anos (38.799.000 LOCK). Depois, a **reserva eterna** de 800.000 LOCK: metade do que resta a cada 30 anos, até cerca de 1.440 anos. Orçamento distribuído bloco a bloco |
| Quem recebe as moedas novas | 100% para os LockKeepers (proporcional às moedas em garantia) |
| Mineração | **Não existe.** Proof of Stake |
| Fee de transferência | 0,1% (mínimo 2 cêntimos, **máximo 1 $**), paga pelo remetente |
| Fee de swap | **0,25%** |
| Divisão das fees | Fundo de desenvolvimento / LockKeepers: 75/25 nos primeiros 5 anos, depois 50/50 para sempre (antes, se a rede chegar ao marco final) |
| Fundo de desenvolvimento | **Uma carteira, a do fundador** (endereço público), protegida pelas proteções da app |
| Aceitação | **Todas** as transferências entre contas têm de ser aceites pelo recetor em **1 hora**, senão voltam automaticamente. Sem exceções (inclui QR e links) |
| Contas | Têm de ser **registadas** na rede antes de poderem receber (registo gratuito) |
| Tipos de conta | Pessoal e Empresa (mesma rede, mesma moeda) |
| App | Gratuita, open source, não-custodial |
| Regras de ouro | Imutáveis (secção 5.3). Correções de erros só por versões novas que cada LockKeeper escolhe instalar |
| Teto de poder | Nenhum LockKeeper com mais de 5%; liga-se automaticamente aos **20** LockKeepers |
| Batota | Moedas em garantia de quem faz batota são **queimadas** |
| Só computadores verdadeiros | **1 computador = 1 LockKeeper**, provado pelo chip de segurança de fábrica (TPM 2.0). Servidores na nuvem e máquinas virtuais não são aceites (secção 5) |
| Marco final | 10 milhões de carteiras ativas → todas as fees caem para metade, para sempre |
| Cofre Soberano | Conta para valores grandes com prova de reservas na rede (secção 7.4) |

---

## 1. Visão

**"A moeda onde não se perde dinheiro: nem por engano, nem por roubo, nem por morte, nem por computadores quânticos."**

Dinheiro digital rápido e barato, sem nome nem dados pessoais, pensado para pessoas normais. A diferença para as outras redes não é a velocidade (a Solana já é rápida), é a **proteção do utilizador**, gravada nas regras da própria rede.

---

## 2. O que nos diferencia

| | Bitcoin | Solana | Zcash | **EncryptLock** |
|---|---|---|---|---|
| Rápida e barata | ✗ | ✓ | ~ | ✓ |
| Transferência tem de ser aceite (1 h), senão volta | ✗ | ✗ | ✗ | ✓ |
| Impossível enviar para endereço errado ou inexistente | ✗ | ✗ | ✗ | ✓ |
| Anti-roubo: código de envio, "Não fui eu", PIN de pânico | ✗ | ✗ | ✗ | ✓ |
| Herança automática na rede | ✗ | ✗ | ✗ | ✓ |
| Prova de reservas na rede (Cofre Soberano) | ✗ | ✗ | ✗ | ✓ |
| Marco final: fees a metade para sempre | ✗ | ✗ | ✗ | ✓ |
| Pós-quântica desde o primeiro dia (contas e validadores) | Em discussão | Em preparação | Parcial | ✓ |
| Utilizadores e liquidez | Enorme | Enorme | Médio | Zero (ponto de partida) |

Contexto da pesquisa (outubro 2026):
- Pagamentos dentro de apps de mensagens **já existem** (Signal + MobileCoin, Telegram + TON/Gram). Não é isso que nos diferencia.
- Perdas por endereços falsos ("address poisoning") explodiram em 2026, com casos de dezenas de milhões. Estima-se que 3–4 milhões de bitcoins estejam perdidos para sempre. **A proteção contra perdas é o nosso espaço.**
- O Bitcoin enfrenta uma migração pós-quântica difícil e lenta.
- Nenhuma das moedas conhecidas nasceu com esta combinação completa de raiz.

---

## 3. Modelo de lançamento

- **Sem empresa, sem venda de moedas, sem leilões pagos, sem bolsa custodial.** Assim o projeto fica, em grande parte, fora das licenças do MiCA.
- O código é open source no GitHub.
- **Recompensa do criador, à vista de todos:** 1% da oferta máxima (400.000 LOCK) fica na conta do criador desde o primeiro bloco, como recompensa por criar a blockchain. Além disso, o criador corre os primeiros LockKeepers desde o primeiro dia e ganha moedas como qualquer LockKeeper. Isto é dito publicamente e vê-se no explorador.
- **Alocação génese pública de 401.000 LOCK:** 1.000 para o primeiro LockKeeper poder arrancar a rede e 400.000 (1%) para o criador. Nada mais existe no primeiro bloco.
- **Nunca** prometer que o preço vai subir.
- A carteira é **não-custodial**: as chaves ficam só no telemóvel do utilizador; ninguém tem acesso ao dinheiro de terceiros.
- **Nota honesta sobre o arranque:** enquanto o fundador for o único LockKeeper, recebe 100% da emissão (≈ 462.053 LOCK no mês 1; ≈ 5,21 milhões, 13% do total, se ficar sozinho um ano inteiro). Quanto mais cedo outras pessoas correrem LockKeepers, mais justa é a distribuição. Abrir a rede a terceiros é prioridade.
- Notas honestas: quem receber a parte do fundo de desenvolvimento fica identificável ao converter em euros e paga impostos sobre o que levantar.

---

## 4. Moeda e emissão

- **Oferta máxima: 40.000.000** LOCK, 8 casas decimais. Nenhuma função permite criar mais.
- **Emissão durante 30 anos (360 meses)**, com descida suave:
  - Fórmula: moedas do mês *n* = `a × r^(n−1)`, com `r = 0,5^(1/60)` (cai para metade a cada 60 meses).
  - `a` é calculado para que a soma dos 360 meses + alocação génese (401.000) + reserva eterna (800.000) = **exatamente 40.000.000**. Resultado: `a ≈ 452.718,2`.
  - Referência aproximada: mês 1 ≈ 452.718 · mês 60 ≈ 228.989 · mês 120 ≈ 114.495 · mês 240 ≈ 28.624 · mês 360 ≈ 7.156.
  - **Cálculo determinístico:** os 360 valores são calculados uma única vez e gravados no código como uma tabela de números inteiros (em unidades de 10⁻⁸ LOCK). O último mês absorve a diferença de arredondamento para a soma dar exatamente 40.000.000. Nada de contas com casas decimais dentro da rede (dariam resultados diferentes em computadores diferentes).
- **"Mês" = 30 dias contados pelo relógio da rede**, a partir do bloco génese. O orçamento de cada mês é distribuído **a cada bloco**, em partes proporcionais ao tempo decorrido.
- **100% da emissão vai para os LockKeepers**: de hora a hora é dividida por todos os que estiveram ligados, pelas horas ligados × moedas em garantia (estejam ou não no grupo que assina).
- **Teto de 5% por LockKeeper:** nenhum LockKeeper pode receber mais de 5% dos ganhos de cada hora (no grupo que assina, cada computador já vale 1 voto igual). Liga-se automaticamente quando houver **20 LockKeepers** ativos (com menos de 20 é matematicamente impossível todos ficarem em 5% ou menos). Quando o teto está ligado, a recompensa que caberia acima dos 5% é dividida pelos restantes LockKeepers na mesma hora — nada se perde nem fica por distribuir.
  - Nota honesta: a rede não consegue saber se dois LockKeepers pertencem à mesma pessoa. O teto limita cada LockKeeper, não cada dono.
- **Moedas de batoteiros são queimadas.** Por isso a oferta real pode ficar abaixo de 40.000.000, nunca acima.
- **Reserva eterna dos LockKeepers (800.000 LOCK, 2%)**, para quem segura a rede ter sempre moedas novas além das fees, sem nunca passar dos 40.000.000:
  - Não sai durante os 30 anos. A partir do ano 31, **o que resta cai para metade a cada 30 anos**, distribuído bloco a bloco como a emissão: anos 31–60 saem 400.000; 61–90, 200.000; 91–120, 100.000; e assim por diante.
  - **99,9% sai até ao ano 330.** Como a moeda tem 8 casas decimais, o último bocadinho (0,00000001 LOCK) sai no período que acaba **cerca de 1.440 anos depois do bloco génese** (por volta do ano 3466, se o início for em 2026).
  - Contas só com números inteiros, iguais em todos os computadores: o que resta depois de *k* períodos de 30 anos é `800.000 LOCK ÷ 2^k` (arredondado para baixo em unidades de 10⁻⁸); dentro de cada período sai a parte proporcional ao tempo decorrido.
  - Não depende do preço (um gatilho de preço pode ser manipulado com trocas entre contas da mesma pessoa e libertaria moedas no pior momento).
- Depois dos 30 anos, os LockKeepers vivem das fees e da reserva eterna.

---

## 5. A rede (LockKeeper)

- **LockKeeper** = o programa que qualquer pessoa pode descarregar e correr. Cada LockKeeper guarda os saldos de todos e os blocos recentes (os antigos apagam-se ao fim de cerca de 1 dia, para não encher o disco nem tornar o computador lento) e fala com os outros pela internet (como torrents; não há servidor central).
- **Consenso:** finalidade imediata (BFT). Alvo: 1–2 segundos por confirmação final.
- **Quem assina os blocos (decisão de 9 out 2026):** um grupo sorteado entre os LockKeepers ligados sem falhas há pelo menos 1 hora. Todos têm a mesma hipótese e, dentro do grupo, **cada computador vale 1 voto** (1 chip = 1 voto): a garantia conta para os ganhos, não para assinar. O grupo começa com **100** e cresce com a rede (1 lugar por cada 10 LockKeepers ligados) até **300**; mais do que isso exigiria blocos mais lentos. Troca gradual e contínua: a cada 6 minutos muda 1% dos lugares (~10% por hora), quem está há mais tempo sai primeiro, para todos terem a sua vez e a rede nunca parar. Quem do grupo deixa de assinar durante cerca de 1 minuto sai e é logo substituído por outro ligado de forma estável. Com menos LockKeepers ligados do que lugares, assinam todos. O sorteio usa o código do bloco, por isso todos os LockKeepers chegam ao mesmo grupo.
- **Validadores que fazem batota perdem as moedas em garantia** (queimadas).
- **Só computadores verdadeiros, 1 LockKeeper por computador.** O computador prova-o com o chip de segurança que traz de fábrica (TPM 2.0: AMD, Intel, Infineon, STMicro, Nuvoton, Microsoft Pluton), confirmado pelo certificado do fabricante e por um teste secreto que só o chip verdadeiro consegue responder. A rede guarda só um código tirado do chip (sem nome nem dados pessoais) e cada código só serve uma vez. Chips virtuais (servidores na nuvem, máquinas virtuais) são recusados. Quem quer 10 LockKeepers precisa de 10 computadores. Atualizações do Windows não mudam o chip.
- **Base técnica do protótipo:** CometBFT + Cosmos SDK, linguagem Go. Continua a ser uma blockchain própria e independente. Não reinventar criptografia.
- **Módulos do Cosmos desligados:** envio direto de moedas (contornaria a aceitação), votação/governação on-chain (não há votação), e o módulo de emissão padrão (substituído pelo nosso).
- **Arranque:** os computadores do fundador (computadores verdadeiros, cada um com o seu chip). A rede precisa de mais de 2/3 dos votos para funcionar: com 1 só LockKeeper, a rede para enquanto ele está desligado e continua quando liga (nada se perde); com 4, aguenta um desligado. Abrir a outras pessoas o mais cedo possível.

### 5.1 Criptografia
- **Contas dos utilizadores — assinaturas híbridas:** Ed25519 **e** ML-DSA-65 (norma pós-quântica do NIST, FIPS 204). Ambas obrigatórias em todas as transações. Usa-se ML-DSA-65 porque é a versão suportada de origem pelo Cosmos SDK v0.55 (não se reinventa criptografia).
- A chave pública híbrida é gravada **uma vez**, no registo da conta, e não repetida em cada transação (poupa espaço).
- Atenção técnica: uma assinatura ML-DSA-65 tem 3.309 bytes (Ed25519 tem 64). As transações ficam bastante maiores. Tem de ser medido na rede de teste.
- **Votos dos LockKeepers (consenso): pós-quânticos desde o primeiro dia.** O Cosmos SDK v0.55 / CometBFT v0.40 suportam chaves ML-DSA-65 para os validadores; a rede EncryptLock só aceita esse tipo de chave nos LockKeepers.

### 5.2 Privacidade
- **Sem nome nem dados pessoais:** cada conta é só um endereço `lock1…`; a app não envia dados a ninguém (sem telemetria).
- **Saldos e transferências são públicos** na rede, como na Bitcoin e na Ethereum. É isso que permite as proteções: aceitação em 1 hora, "Não fui eu", limites, herança e Cofre.
- **Sem modo privado (decisão do fundador, 15 out 2026):** as provas de conhecimento zero pós-quânticas ainda não estão maduras, e o EncryptLock não lança nada de que não tenha a certeza.

### 5.3 Regras de ouro e atualizações
- **Regras de ouro (nunca mudam):** oferta de 40.000.000; curva de emissão e reserva eterna; ninguém pode congelar ou retirar moedas de outra pessoa; aceitação em 1 hora com devolução automática; teto de 5%; marco final das fees.
- **Como são protegidas (nota honesta):** o código é aberto, por isso qualquer pessoa pode copiá-lo e lançar *outra* rede com regras diferentes — isso não se consegue impedir. O que protege as regras de ouro é que os LockKeepers da rede EncryptLock recusam instalar versões que as alterem, tal como acontece no Bitcoin. Cada versão oficial traz testes que falham se alguma regra de ouro for mudada.
- **Manutenção:** correções de erros e de segurança saem como versões novas do LockKeeper. **Cada LockKeeper decide se instala.** Ninguém é obrigado; não há votação nem dono (como no Bitcoin).

### 5.4 Marco final
- Quando a rede chegar a **10.000.000 de carteiras ativas**, todas as fees (transferência e swap) caem para **metade, para sempre**, e a divisão das fees passa logo a **50/50**, mesmo que ainda não tenham passado 5 anos (regra de ouro).
- **Carteira ativa** = saldo equivalente a mais de 100 $ (no arranque, valor fixo em moedas; a ajustar) **e** transações em pelo menos 3 meses diferentes, ligada a um aparelho verdadeiro.
- **Por definir antes da rede pública:** como a rede mede isto sozinha. "Aparelho verdadeiro" e "100 $" não se conseguem verificar dentro da rede sem uma entidade central. A regra tem de ser reescrita em termos que a rede consiga contar (ex.: contas registadas com saldo mínimo em LOCK e atividade em 3 meses).
  - **Forma proposta:** a rede conta, com os dados que já são públicos, as contas registadas com saldo mínimo em LOCK e transações em pelo menos 3 meses diferentes.
- Recompensa sem dinheiro: insígnia de "Fundador" visível na carteira para os primeiros utilizadores (opcional).

---

## 6. Contas e transferências

### 6.1 Registo de contas
- Uma conta só existe depois de ser **registada** na rede. O registo grava a chave pública híbrida (Ed25519 + ML-DSA).
- O registo é **gratuito** (uma conta nova tem 0 moedas e não podia pagar). Uma chave só pode ser registada uma vez. Na rede pública serão precisos limites contra registos em massa (a definir).
- **Só se pode enviar para contas registadas.**

### 6.2 Aceitação
- **Todas as transferências entre contas ficam pendentes até o recetor as aceitar.**
- Prazo: **1 hora**, contada pelo relógio da rede. Se não for aceite, o dinheiro volta automaticamente para quem enviou.
- **Sem exceções** entre contas: transferências normais, QR codes e links de pagamento, contas pessoais e de empresa.
- Enquanto está pendente, o remetente pode cancelar.
- **Movimentos da própria rede não são transferências entre contas** e seguem regras próprias: recompensas da emissão, divisão das fees, queima por batota, e (nas fases seguintes) execução de trocas no mercado, herança e saídas do Cofre Soberano.
- **Pedido de pagamento:** alguém pede um valor; o outro vê o pedido e aprova (útil para dividir contas, cobrar a um amigo).
- Notas: lojas online automáticas não funcionam com esta regra (aceite conscientemente).

### 6.3 Quem paga a fee
- **O remetente paga a fee no momento em que cria a transferência.**
- Aceitar, cancelar e registar conta **não têm fee**.
- Se a transferência expirar ou for cancelada, o valor volta por inteiro, mas **a fee não é devolvida** (impede que alguém encha a rede de transferências pendentes de graça).

### 6.4 Tipos de conta
- **Pessoal:** comprar, guardar, enviar, receber, swap.
- **Empresa:** igual, mais pedidos de pagamento com link/QR ligados a uma venda (com valor fixo, uso único e expiração). A aceitação continua a ser manual.
- Escolha feita na criação da conta: "Pessoal ou Empresa?".

---

## 7. Escudo de segurança

### 7.1 Nunca perder por engano
- Endereços com código de verificação: uma letra errada e a app nem deixa enviar.
- **Só se pode enviar para contas registadas na rede.**
- Antes de enviar a alguém novo, aparece o nome que essa pessoa escolheu ("Vais enviar para Ricardo M.?"). O nome é passado de forma encriptada entre as duas apps e **nunca fica na blockchain**.
- Moedas "lixo" de desconhecidos (o truque usado no address poisoning) não chegam a entrar: como todas as transferências têm de ser aceites, basta não aceitar. A app nunca sugere endereços a partir do histórico.
- **Contactos da carteira:** lista guardada só no telemóvel, numa pasta encriptada. "Contacto novo" = endereço para o qual nunca enviaste.

### 7.2 Hacker no telemóvel não consegue roubar
- Chave protegida pelo **chip de segurança do telemóvel**, só funciona com cara/impressão digital. Nota técnica: o chip do Android não suporta ML-DSA nem, em geral, Ed25519; por isso o chip guarda uma chave que encripta as chaves híbridas.
- **Código de envio separado** do PIN de abrir a app.
- **Alerta em todos os aparelhos** do dono a cada envio, com botão **"Não fui eu"** que congela a conta e cancela tudo o que está pendente.
- **Contacto novo ou valor grande:** confirmação extra num segundo aparelho.
- **Limite diário** definido pelo utilizador; para o aumentar, espera de 48 horas.
- **PIN de pânico:** um PIN alternativo abre uma carteira falsa com pouco dinheiro e envia alerta silencioso para o segundo aparelho.
- PIN de abertura, biometria e sessão que expira e força novo login.
- **Regras exatas na rede (implementadas na Fase 2):**
  - O segundo aparelho é outra conta híbrida ligada à conta; **só pode aprovar e congelar, nunca enviar**. Ligar o primeiro é imediato; mudar ou remover demora **7 dias**.
  - Aprovação extra para **contacto novo** (endereço que nunca aceitou uma transferência desta conta) ou **valor acima do limite escolhido**: o segundo aparelho tem **24 horas**; depois quem recebe tem 1 hora para aceitar. Sem aprovação, o valor volta.
  - **"Não fui eu"** (pela conta ou pelo segundo aparelho): congela a conta, devolve todas as transferências pendentes que ela enviou e anula mudanças pendentes que afrouxem a proteção. A conta continua a poder receber. Descongelar: as duas chaves juntas → logo; uma só → **7 dias**.
  - **Limite diário** (janela de 24 horas). Apertar (baixar o limite ou o valor de aprovação) vale logo; afrouxar só **48 horas** depois.

### 7.3 Nunca perder por morte ou perda do telemóvel
- **Herança automática:** se uma conta não mostrar sinal de vida durante X meses (definido pelo dono), as moedas passam para os herdeiros escolhidos. Como é um movimento da rede (secção 6.2), o herdeiro tem um prazo longo para reclamar (a definir), em vez de 1 hora.
- **Recuperação por pessoas de confiança** escolhidas pelo dono.
- **Regras exatas (decididas pelo fundador; implementadas na Fase 2). Ambas são opcionais.**
  - Herança: o dono escolhe os herdeiros (percentagens que somam 100%) e o tempo sem sinal de vida, **no mínimo 1 ano** (pode escolher mais). Sinal de vida = qualquer transação da conta ou o botão "Estou cá"; cancela logo um pedido de herança. Os herdeiros podem pedir **sem prazo**.
  - **Todos os herdeiros têm de confirmar** (cada um na sua carteira, com o seu PIN/biometria), dentro de **30 dias** a contar do primeiro; senão o pedido cai e pode recomeçar. O dono pode escolher que bastam menos (ex.: 2 de 3), para o caso de um herdeiro já não estar contactável.
  - Quando o primeiro herdeiro pede, a rede regista-o publicamente e a app do dono (e o segundo aparelho) mostra logo um alerta "Os teus herdeiros pediram a herança — se estás vivo, carrega em Estou cá", repetido durante a espera. Quando todos confirmam, **30 dias** de espera; só então as moedas passam para os herdeiros.
  - Recuperação: pelo menos **2 pessoas de confiança**, e pelo menos **2 têm de aprovar** a mesma conta nova (registada). Depois, **30 dias** de espera, em que o dono pode cancelar. Uma transação da conta não cancela uma recuperação (pode ser um ladrão); só o cancelamento explícito do dono.
  - **Leva tudo:** se a conta era de um LockKeeper, a rede devolve também a garantia (chega ao fim do tempo de espera normal da rede, 21 dias). A conta antiga fica **fechada**: tudo o que lhe chegar depois (garantia, trancas e ofertas que voltam, ganhos) segue sozinho para os herdeiros, nas mesmas percentagens, ou para a conta nova; e a conta antiga não pode assinar mais nada — quem ainda tiver as palavras dela não lhe pode mexer.
  - Mudar herdeiros ou pessoas de confiança demora **7 dias** (o primeiro plano vale logo) e o dono pode cancelar a mudança.
  - A passagem final das moedas é um movimento da rede (não precisa de ser aceite). Moedas em garantia de LockKeeper não passam automaticamente (a tratar mais tarde).

### 7.3.1 Abrir e recuperar a carteira (obrigatório na app)
- **24 palavras de recuperação** (seed phrase). As duas chaves da conta (Ed25519 e ML-DSA-65) saem destas 24 palavras: quem as tiver recupera sempre a carteira, mesmo perdendo o telemóvel.
- **No primeiro arranque**, a app pergunta: **"Criar carteira nova"** ou **"Já tenho as 24 palavras (recuperar)"**.
- Ao criar: mostra as 24 palavras uma vez e obriga a confirmar algumas, por ordem, antes de continuar.
- **A carteira fica sempre encriptada no telemóvel** com uma palavra-passe / código de desencriptação. Ao abrir, aparece sempre o ecrã **"Decrypt your wallet"** (traduzido para o idioma escolhido), além da biometria.
- Sem as 24 palavras e sem o telemóvel, ninguém (nem o fundador) consegue recuperar a carteira — é isso que a torna não-custodial.

### 7.4 Cofre Soberano (para valores grandes, disponível desde o início)
- **Prova de reservas:** qualquer pessoa confirma na rede que o dono tem "pelo menos X moedas" (pública, sem conhecimento zero).
- A prova deve cobrir um **período** (ex.: "teve pelo menos X durante todo o trimestre") e não só um instante, para impedir que alguém peça moedas emprestadas só no dia da auditoria.
  - **Forma proposta (mais simples e segura):** em vez de provar o passado, o dono **tranca** X moedas no Cofre até uma data (ex.: fim do trimestre). Moedas trancadas não podem sair antes da data, por isso a prova "tem pelo menos X" é automaticamente válida para todo o período. Provar o passado sobre todos os blocos de um trimestre seria muito mais pesado.
- **Várias chaves obrigatórias** (ex.: 3 de 5 pessoas/aparelhos aprovam qualquer saída).
- **Atraso obrigatório nas saídas** (ex.: 7 dias), cancelável. Depois do atraso, a saída segue a regra normal de aceitação em 1 hora.
- Pós-quântico e com herança, como o resto da rede.
- Para quem: fundos, empresas com tesouraria em cripto, grandes detentores.
- **Implementado (Fase 4):**
  - **Conta de equipa** M de N (ex.: 3 de 5), em que cada membro é uma conta híbrida.
  - **Cofre Soberano:** atraso nas saídas entre 7 e 365 dias; durante o atraso o cofre pode cancelar; depois quem recebe tem 1 hora para aceitar. Aumentar o atraso vale logo; reduzir (ou deixar de ser cofre) só depois do atraso atual; o "Não fui eu" anula uma redução pendente.
  - **Trancas:** trancar moedas até uma data (máx. 10 anos); ninguém as move antes; voltam sozinhas nessa data. **Prova de reservas:** consulta "esta conta tem pelo menos X trancados até à data D" — vale para todo o período. É **pública**.

---

## 8. Fees

Valores de referência em dólares. **Na rede de teste ficam fixos em LOCK** (mínimo 0,0002 LOCK, máximo 0,01 LOCK, só para testar). Ligar a fee a uma referência de preço em dólares é uma fase posterior com estudo próprio (uma referência de preço é um ponto central e pode ser manipulada).

### 8.1 Transferências
| Valor | Fee |
|---|---|
| 20 $ | 2 cêntimos |
| 100 $ | 10 cêntimos |
| 500 $ | 50 cêntimos |
| 1.000 $ ou mais | **1 $ (máximo)** |

Regra: 0,1% do valor, mínimo 2 cêntimos, máximo 1 $, paga pelo remetente. Para comparação: Bitcoin 1–5 $, Ethereum 0,25–20 $, Solana < 1 cêntimo.

### 8.2 Swap
**0,25%** (MetaMask 0,875%, Exodus ~0,5%+, Uniswap 0,3%).

### 8.3 Divisão
| Período | Fundo de desenvolvimento | LockKeepers |
|---|---|---|
| Anos 1–5 | 75% | 25% |
| Ano 6 em diante, para sempre | 50% | 50% |

- Se a rede chegar ao marco final (secção 5.4) antes dos 5 anos, passa logo a 50/50.

- O fundo de desenvolvimento é **uma carteira normal do EncryptLock, a do fundador** (decisão do fundador, 12 out 2026), com **endereço público** na rede (precedente: o fundo de desenvolvimento da Zcash). Como todas as contas, a chave é híbrida (Ed25519 + ML-DSA-65), por isso o fundo é pós-quântico, e usa as proteções da app: segundo aparelho, limite diário, "Não fui eu", herança e recuperação por pessoas de confiança. O endereço fica gravado no bloco génese e ninguém o pode mudar.
  - Recomendação para o lançamento: as 24 palavras em papel num sítio seguro fora de casa, segundo aparelho ligado, herança e pessoas de confiança escolhidas. Nunca as 24 palavras em email, fotos ou nuvem.
- As mesmas fees aplicam-se a todos. Nenhuma app tem desconto.
- "Ano" = 365 dias contados pelo relógio da rede, a partir do bloco génese.

---

## 9. Mercado e swap (dentro da rede)

- **Não-custodial:** a rede faz a troca; ninguém guarda o dinheiro dos utilizadores.
- **Livro de ordens** (para comprar é preciso alguém vender), não fórmula automática.
- **Execução em lotes** a cada 1–2 segundos, todas ao mesmo preço (sem vantagem para robôs ultrarrápidos).
- **Anti-manipulação:** limite de volume por conta e por aparelho a cada X minutos; tempo mínimo de vida de uma ordem; proibido negociar consigo próprio; travão automático se o preço/volume disparar em segundos.
- **Dependência:** um mercado precisa de pelo menos duas moedas. Enquanto só existir LOCK na rede, não há nada para trocar. O mercado só faz sentido depois de existir uma ponte para outra moeda (ex.: USDT).
- **Swap com outras cripto (USDT, ETH, …):** exige pontes entre redes, que são tecnicamente complexas e historicamente um alvo de ataques. Estudo próprio.
- **Fora do âmbito:** contratos com margem, short, alavancagem (derivados regulados).

---

## 10. Peças a construir (tudo grátis)

| Peça | O que é | Onde |
|---|---|---|
| LockKeeper | Programa da rede | GitHub + computadores do fundador (só computadores verdadeiros; sem servidores na nuvem) |
| Carteira EncryptLock | App Android (Flutter), não-custodial | Play Store (conta de programador já existente) |
| Explorador | Site com estatísticas públicas: moedas existentes e libertadas, nº de carteiras, nº de transações. **Nunca** mostra donos nem saldos | Cloudflare Pages / GitHub Pages |

Domínio próprio opcional (~10 €/ano); pode começar com endereço gratuito (ex.: encryptlock.pages.dev).

**Idiomas:** a carteira, o explorador e a carteira por linha de comandos têm **inglês como base** e são preparados desde o primeiro dia para traduções. Todos os textos que o utilizador vê ficam em ficheiros de tradução separados (nunca escritos diretamente no código); a app usa o idioma escolhido pelo utilizador. A lista de idiomas é definida pelo fundador antes da Fase 5. Acrescentar um idioma = acrescentar um ficheiro.

---

## 11. Ligação a outras apps e carteiras

- O EncryptLock é um projeto **independente**. Não depende de nenhuma app e nenhuma app tem regras especiais na rede.
- Qualquer app ou carteira (incluindo apps do mesmo autor) pode enviar e receber LOCK, usando o formato de endereço e de pedido de pagamento públicos da rede.
- **Troca segura de endereços:** formato aberto de "cartão de endereço" assinado, que qualquer app pode enviar e a carteira lê diretamente (sem copiar e colar). QR presencial também é aceite.

---

## 12. Enquadramento legal (resumo, não é aconselhamento jurídico)
- Sem empresa, sem venda de moedas e sem custódia, o projeto fica em grande parte fora das licenças do MiCA.
- Moedas totalmente anónimas estão a ser restringidas na UE (regras anti-branqueamento a partir de 2027) — o EncryptLock não tem modo anónimo: os saldos e as transferências são públicos, sem nome.
- Carteiras não-custodiais são aceites na Google Play sem licença.
- Quem converter moedas em euros paga impostos.
- Não incluído de propósito: compra com cartão/euros na app, pagamentos em Multibanco, derivados.

---

## 13. Ideias para mais tarde (não confirmadas)
- Pagamentos sem internet (NFC/Bluetooth) para valores pequenos.
- Compra protegida (dinheiro bloqueado até o comprador confirmar a entrega).
- Pagamentos recorrentes controlados pelo utilizador.
- Contas de família.
- Parceiro licenciado para compra com euros (quando a moeda for conhecida).

**Rejeitado:** relatório de IRS automático, prémios por falhas, subscrição/pagamento da app, empresa a operar bolsa, reserva de moedas a custo zero para o fundador, descontos de fee para apps específicas.

---

## 14. Fases

1. **Protótipo da rede:** 4 LockKeepers locais, oferta e emissão corretas, fees com mínimo/máximo e divisão por período, assinaturas híbridas, registo de contas, transferências pendentes com aceitação em 1 h e devolução automática, recusa de envio para contas não registadas. Carteira por linha de comandos.
2. **Escudo de segurança:** código de envio, "Não fui eu", limites, segundo aparelho, PIN de pânico, herança, recuperação.
3. ~~**Privacidade**~~ — retirada (decisão do fundador, 15 out 2026; ver secção 5.2).
4. **Cofre Soberano e marco final.**
5. **Carteira Android + explorador**, com traduções.
6. **Rede de teste pública** com outros LockKeepers.
7. **Lançamento.**
8. **Pontes para outras moedas + mercado/swap interno:** livro de ordens, lotes, anti-manipulação, fee 0,25%. (Passou para o fim porque um mercado precisa de pelo menos duas moedas — ver secção 9.)

---

