# Privacidade no EncryptLock — estudo (Fase 3)

12 de outubro de 2026 · Estudo antes de programar, como diz o white paper (secção 5.2 e Fase 3).

## Onde estamos hoje

- Sem nome nem dados pessoais: cada conta é só um endereço `lock1…`.
- Mas os **saldos e as transferências são públicos** na blockchain (quem sabe o teu endereço vê o teu saldo). É assim em quase todas as blockchains.
- Objetivo do white paper: esconder **valor, quem envia e quem recebe**, com **chave de visualização** (o dono mostra o seu histórico a quem quiser) e esconder o IP de quem envia.

## O problema principal

As proteções que tornam o EncryptLock diferente precisam que a rede **veja** as contas:

| Proteção | Porque precisa de ver |
|---|---|
| Transferência aceite em 1 h, senão volta | a rede tem de saber a quem devolver |
| "Não fui eu" (congelar) | a rede tem de saber que conta está congelada |
| Limite diário, segundo aparelho | a rede tem de somar o que a conta enviou hoje |
| Herança por inatividade | a rede tem de saber há quanto tempo a conta não dá sinal de vida |
| Cofre Soberano | a rede tem de reter as saídas da conta |

Com privacidade total ("privada por defeito"), a rede deixa de ver as contas e **estas proteções deixam de funcionar**. Nenhuma blockchain resolveu as duas coisas juntas — é o maior risco técnico do projeto, como já estava escrito.

## O que encontrei que resolve parte do problema

**A regra de 1 hora consegue funcionar mesmo às escondidas.** Cada "nota" privada (um bocado de dinheiro escondido) leva lá dentro duas chaves e uma hora-limite:
- até à hora-limite, só quem recebe a pode usar (= aceitar);
- depois da hora-limite, só quem enviou a pode usar (= volta para trás).

A prova matemática garante isto sem a rede saber quem é quem. As outras proteções (congelar, limites, herança, Cofre) não têm solução conhecida às escondidas.

## Recomendação: "Modo privado" opcional

- As contas normais continuam como estão, **com todas as proteções**.
- Cada pessoa pode passar LOCK para o **modo privado** (valores e pessoas escondidos) e voltar quando quiser.
- No modo privado: 1 hora para aceitar (sim), chave de visualização (sim); congelar, limites, herança e Cofre (não) — a app avisa disto antes de entrar.
- Quem quer o máximo de proteção fica nas contas normais; quem quer privacidade escolhe o modo privado.

Alternativa ("privada por defeito", como diz hoje o white paper): toda a gente perde as proteções. **Não recomendo**, porque a proteção do dinheiro é a razão de ser do EncryptLock.

## A decisão que é tua: computadores quânticos

O EncryptLock promete estar **pronto para computadores quânticos**. As provas de privacidade usadas pelas moedas privadas conhecidas (curvas elípticas) **não são** — um computador quântico grande poderia falsificá-las e criar moedas escondidas.

| | A — Provas rápidas (curvas elípticas) | B — Provas pós-quânticas (só hashes, "STARK") |
|---|---|---|
| Pronto para computadores quânticos | Não (só a parte privada) | Sim |
| Tamanho de cada transferência privada | pequeno (~1 KB) | grande (~50–200 KB) |
| Rapidez | rápido no telemóvel | mais lento (segundos no telemóvel) |
| Bibliotecas em Go (a linguagem da rede) | sim (gnark, auditada) | poucas; quase tudo em Rust |
| Trabalho | menor | bastante maior |

Em qualquer das duas, uso uma **"catraca"** (ideia da Zcash, ZIP 209): a rede conta quantos LOCK entraram no modo privado e **nunca deixa sair mais do que entrou**. Assim, mesmo que alguém falsificasse provas, não conseguia criar LOCK no resto da rede — o estrago ficava limitado ao modo privado.

**Recomendo B** (pós-quântico), para cumprir a promessa do white paper, mesmo sendo mais pesado. A fica como plano se B se revelar demasiado lento nos telemóveis.

## Plano por passos (depois da tua decisão)

1. **3a — Modo privado na rede:** notas escondidas, "catraca", regra de 1 hora dentro das notas; testes, incluindo tentativas de batota.
2. **3b — App:** botão "Passar para o modo privado" / "Voltar", saldo privado, aviso das proteções que não existem lá.
3. **3c — Chave de visualização:** mostrar o histórico privado a quem o dono escolher.
4. **3d — Esconder o IP:** a app envia as transferências por um caminho que não revela o IP (passo por passo, sem servidores pagos).
5. **3e — Contagem anónima de carteiras ativas** (marco dos 10 milhões) e **prova de reservas privada** do Cofre — usam as mesmas provas.

Honestidade: é a fase mais longa do projeto (meses, não dias). Erros em provas de privacidade não se veem a olho, por isso cada passo leva testes de ataque próprios e a "catraca" desde o primeiro dia.

---

## Anexo técnico (para quem programar)

- Modelo de notas tipo Zcash Sapling/Orchard: nota = (valor, chave pública do recetor, chave de devolução, hora-limite, aleatório); compromisso numa árvore de Merkle; gastar = revelar o "nullifier" e provar pertença à árvore.
- Regra de 1 h no circuito: entrada pública `hora_do_bloco`; se `hora_do_bloco ≤ limite` exige assinatura da chave do recetor, senão da chave de devolução.
- Catraca: `privado_total = entradas − saídas` guardado em claro; a saída do modo privado falha se ficasse negativo.
- Opção A: gnark (Go, PLONK sobre BN254, configuração universal) — integra direto no módulo Cosmos SDK.
- Opção B: STARK com hash amigável (Poseidon2/Rescue sobre corpo de 64 bits); provador na app (Rust via FFI, ex.: Plonky3/Winterfell) e verificador na rede (Go nativo ou chamada FFI determinística). Verificação tem de ser determinística e com custo de gás fixo.
- Referências de desenho: Zcash (Sapling/Orchard, ZIP 209), Namada (MASP, sobre CometBFT), Penumbra (CometBFT). Não copiar código sem verificar licenças.
- Privacidade de rede: Dandelion++ exige mudar o mempool do CometBFT; primeiro passo mais simples é a app enviar por retransmissores.

## Decisões (15 out 2026)

- Fundador escolheu **B (pós-quântico)** e o **Modo privado opcional**.
- **Tudo em Go, sem Rust:** o núcleo da app no telemóvel já é Go (gomobile), por isso o provador (telemóvel) e o verificador (rede) usam **o mesmo código Go** — não há risco de os dois lados calcularem de forma diferente, e não é preciso instalar outra linguagem.
- Peças: **gnark-crypto v0.22.0** (ConsenSys, Apache-2.0): corpo **KoalaBear** (31 bits, o mesmo do Plonky3), hash **Poseidon2** (largura 16), FFT e extensões do corpo.
- **Feito (3a-1, 3a-2a):** módulo `x/private` com árvore de notas em **Poseidon2** (cada nó = 8 elementos KoalaBear = 32 bytes; selos fora do corpo recusados), nullifiers, catraca, verificador "recusa tudo".
- **Próximo (3a-2b):** o sistema de provas em si (STARK = AIR + FRI sobre KoalaBear com Poseidon2). Avaliar primeiro reutilizar código Go já existente e auditado (gnark-crypto: fft/extensions/vortex; prover da Linea, também Go/Apache-2.0) em vez de escrever o FRI do zero; definir a nota (valor, chave do recetor, chave de devolução, hora-limite, aleatório) e o circuito com a regra de 1 h; medir tempo no telemóvel.

### 3a-2b — que código de provas usar (15 out 2026, avaliação)

- **gnark-crypto** não tem FRI para KoalaBear; o pacote `vortex` diz "não pronto para produção". Serve para as peças (corpo, Poseidon2, FFT), não para o sistema completo.
- **Provador da Linea** (`prover/` em github.com/LFDT-Lineth/lineth-monorepo, antes Consensys/linea-monorepo; Apache-2.0; Go): sistema "wizard/arcane" sobre KoalaBear + Poseidon2 + Vortex (compromisso por hashes — pós-quântico), com compiladores para restrições, permutações, lookups e Poseidon2. Em produção na rede Linea. **Atenção:** a Linea embrulha a prova final em PLONK/BN254 para a Ethereum — essa parte NÃO é pós-quântica e não a usamos; ficamos só com a prova interna. Feito para servidores grandes; não testado em telemóveis; módulo Go separado e muito grande.
- **Fazer o nosso STARK pequeno** (AIR + FRI sobre as peças da gnark-crypto): leve e à medida do telemóvel, mas é criptografia nossa → obriga a auditoria externa antes do lançamento.

**Plano 3a-2b:** (1) protótipo com o wizard da Linea para o circuito mínimo — "esta nota está na árvore (32 níveis Poseidon2) e este nullifier é dela"; (2) medir tamanho da prova, tempo de verificação no PC e tempo de prova no telemóvel do fundador; (3) se for pesado demais no telemóvel, passar ao STARK próprio. Só depois: nota completa (valor, chaves, hora-limite) e regra de 1 h.

**Teste de compilação do provador da Linea (15 out, pasta à parte, projeto intocado):** versão `prover@v0.0.0-20261015123321-7952d16daaf5` (caminho Go ainda `github.com/consensys/linea-monorepo/prover`, sem versões com etiqueta).
- ✅ Aceita a nossa gnark-crypto **v0.22.0** (embora peça uma versão de desenvolvimento mais antiga).
- ✅ Sem código C (CGO_ENABLED=0) — `wizard`, `compiler` (Arcane) e `compiler/vortex`.
- ✅ **Android arm64** compila (programa de teste com 13 MB).
- ❌ **Windows não compila:** `prover/utils/arena/vector_arena.go` usa `syscall.Mmap` (só Linux/Mac). Os LockKeepers e a app do PC são Windows → precisa de uma pequena cópia corrigida desse ficheiro (`replace` no go.mod com versão Windows usando memória normal) ou de confirmar que o verificador não usa o `arena`. Arranque do próximo passo: resolver isto, depois o circuito mínimo "nota na árvore + nullifier" e as medições.

### ⚠️ Descoberta (sessão seguinte a 15 out): a Linea NÃO serve como está

- Montagem real encontrada (circuito gnark com `poseidon2_koalabear.CompressPoseidon2` → `plonkinternal.PlonkCheck` → `compiler.Arcane` → `vortex.Compile(…, false /*KoalaBear*/)`), mas **as provas da Linea não são de conhecimento zero**: não há máscaras nem linhas aleatórias no Vortex (procurado em `protocol/compiler/vortex`, `crypto/vortex`, `protocol/wizard`); o Vortex abre colunas e envia combinações das linhas em claro. Na Linea os dados são públicos de propósito; para nós isso **revelaria valor e dono das notas**.
- Acrescentar o disfarce (máscaras) ao Vortex = criptografia nossa → mesmo risco/auditoria que o STARK próprio.
- **Opções a reavaliar no próximo passo** (todas pós-quânticas, só hashes):
  1. **Plonky3** (Rust, Polygon; KoalaBear + Poseidon2 — as mesmas peças da nossa árvore) com o modo de conhecimento zero (PCS com máscaras). Volta a precisar de Rust (FFI na app e na rede, como o CosmWasm faz com o wasmvm). Confirmar que o modo ZK está pronto e auditado.
  2. STARK próprio em Go com máscaras (AIR + FRI sobre gnark-crypto) → auditoria externa obrigatória.
  3. Outras com ZK de origem: Ligero/Ligetron, RISC Zero (só a parte STARK; a última camada Groth16 não é pós-quântica).
- A árvore Poseidon2/KoalaBear do `x/private` continua válida para as opções 1 e 2 (verificar que o Poseidon2 do Plonky3 usa os mesmos parâmetros: o da gnark-crypto usa 21 rondas parciais).

### Plonky3 com conhecimento zero — estado (pesquisa)

- Existe o modo `HidingFriPcs` / `MerkleTreeHidingMmcs` (ZK). **Sem auditoria publicada**: a auditoria da Least Authority (nov 2024) foi feita ao código sem esse modo e recomendou nova auditoria quando fosse acrescentado; não encontrei nenhuma.
- Problemas recentes: bloqueio em execução paralela (set 2026, correção no PR #2368); traços curtos podiam ser recuperados de uma prova — há um PR (#2100) a exigir altura mínima do traço. Ou seja: em uso, mas ainda a ser corrigido.
- Fontes: https://leastauthority.com/wp-content/uploads/2024/11/Updated_071124_Polygon_Plonky3_Final_Audit_Report.pdf · https://hackmd.io/@vba/plonky3-zero-knowledge-deadlock · https://github.com/Plonky3/Plonky3/pull/2368 · https://github.com/argumentcomputer/multi-stark/issues/89

**Conclusão:** hoje não encontrei nenhum sistema de provas pós-quântico, com conhecimento zero **e** auditado, pronto a usar. **Recomendação do Claude:** (1) usar Plonky3 com o modo ZK (o mais usado e mais escrutinado; mesmas peças KoalaBear + Poseidon2), versão fixa e recente, traço sempre acima do mínimo; (2) o **modo privado só é ligado na rede depois de uma auditoria externa** ao nosso circuito e ao modo ZK do Plonky3; (3) a moeda pode ser lançada sem o modo privado ligado — a catraca e o "verificador recusa tudo" já protegem. Pergunta ao fundador: aceita lançar primeiro sem o modo privado e ligá-lo depois da auditoria?

### Primeira medição com Plonky3 0.8.0 (modo ZK), no PC do fundador (AMD)

Teste à parte (pasta temporária, projeto intocado): permutações Poseidon2 em KoalaBear (largura 16) provadas com `HidingFriPcs` + `MerkleTreeHidingMmcs`, parâmetros `FriParameters::new_benchmark_zk` (100 consultas). As duas correções de segurança do modo ZK (#2100, #2368) entraram antes da 0.8.0 (11 out 2026).

| Permutações | Provar | Verificar | Tamanho da prova |
|---|---|---|---|
| 64 | **recusado** pelo próprio Plonky3 ("HidingBudgetExceeded": traço curto demais para esconder em segurança — a proteção do PR #2100 a funcionar) | — | — |
| 1.024 | 90 ms | 3 ms | 275 KB |
| 8.192 | 648 ms | 4 ms | 388 KB |

Leitura: o nosso circuito (caminho na árvore de 32 níveis + selo + nullifier ≈ 40 permutações) fica no tamanho mínimo de 1.024 linhas → **≈ 0,1 s no PC**, telemóvel previsivelmente ~0,5–1 s; verificação na rede ~3 ms (ótimo). **Ponto fraco: ~275 KB por transferência privada** — afinar depois (mais "blowup" = menos consultas, hash Poseidon2 em vez de Keccak nas árvores da prova, juntar várias transferências).

Ferramentas instaladas: Rust 1.99.1 (`%USERPROFILE%\.cargo\bin`, alvo `x86_64-pc-windows-gnu`, sem Visual Studio); o `dlltool` que faltava é o `llvm-dlltool.exe` do NDK Android copiado para `%USERPROFILE%\.cargo\tools\dlltool.exe` (pôr essa pasta no PATH ao compilar).

**Próximo:** circuito verdadeiro (AIR) da nota — caminho na árvore com o MESMO Poseidon2 do `x/private` (confirmar que as constantes do Plonky3 e da gnark-crypto coincidem; senão a árvore passa a usar as do Plonky3), nullifier, regra de 1 h; depois ligar ao Go (biblioteca Rust chamada pela rede e pela app).
