# Contexto do Projeto — Portfólio Pessoal (Yuri Nemeth)

> Documento de contexto para acompanhar o desenvolvimento do portfólio. Pode ser colado no Obsidian ou versionado junto ao repositório (ex: raiz do projeto).

**Última atualização:** 03 de setembro de 2026

---

## 1. Escolha de design

- **Estética:** HUD minimalista de nave espacial — fundo quase preto, tipografia monoespaçada, paleta reduzida a 4 cores.
- **Referências visuais explícitas:** interface de *Ender's Game*, *Star Wars Battlefront II* (PS4), site "Death Clock" do Vsauce.
- **Paleta de cores** (definida em `:root`):
  - `--bg: #05070a` — fundo
  - `--fg: #d7e6e2` — texto principal
  - `--accent: #6de3c9` — cor de destaque
  - `--dim: #3a4a48` — texto secundário / grid de fundo

**Fluxo de entrada (intro):**
1. Tela preta, "Hello, world!" com cursor piscando
2. Subtítulo troca sozinho para "Type ENTER to enter:" após 3s
3. Usuário apaga "Hello, world!" (Backspace) e digita "ENTER"
4. Tela vira a cor de destaque (`#6de3c9`), texto muda para "YURI"
5. Animação de String Art (retrato formado por 5500 linhas)
6. Ao terminar, o texto completa sozinho "_NEMETH" (efeito de digitação automática)
7. Pausa de 2s, depois efeito de "portal" (círculo revelador via `mask-image`) com leve zoom, revelando a página inicial

**Página inicial (`.home`) — layout full-screen (atualizado hoje):** a home deixou de ser uma coluna central de largura máxima 900px para virar um *shell* de duas áreas ocupando a tela inteira: uma coluna fixa (`sticky`) à esquerda com foto + identidade, e uma área principal rolável à direita com bio, linhas horizontais de timeline e a seção de projeto em destaque. Ver seção 6 (diário de bordo) para o histórico da mudança.

**Mobile:** input escondido (`<input>` invisível, focado no primeiro toque) capturando o teclado virtual — necessário porque toque não gera `keydown` utilizável para letras. Abaixo de 860px, o shell empilha (coluna vira linha no topo, área principal ganha largura total).

**Elementos decorativos pendentes** (mencionados, aguardando priorização): constelações, xadrez, contrabaixo elétrico, óculos, fórmulas matemáticas, ligações químicas, referência ao TCC "Conecta" (estilo mockup de protótipo/tela de cadastro).

**Ideia registrada para o futuro — barra de rolagem personalizada:** hoje a barra foi apenas recolorida (fina, `--dim`/`--accent`, sem branco). A ideia de longo prazo é substituir o thumb por um cubo mágico (vídeo ou canvas 2D/3D) que "se resolve" conforme o usuário rola até o fim da página — plausível em 2D mostrando só 3 faces do cubo. Ainda não implementado; marcado com `TODO` no CSS do `index.html`.

---

## 2. Foco em matemática visual

**Regra de design definida explicitamente:** nenhuma animação deve ser decorativa — cada uma precisa estar ancorada em um conceito matemático real ou em dados reais do projeto/jogo.

Aplicações até agora:

| Conceito | Onde é usado |
|---|---|
| Trigonometria (`cos`/`sin`) | Posição dos 400 pinos da string art num círculo |
| Curva exponencial (`easeInExpo`) | Velocidade de desenho das 5500 linhas — acelera com `Math.pow(2, 10t − 10)` |
| Teorema de Pitágoras (`Math.hypot`) | Raio exato do círculo revelador do portal, cobrindo a tela inteira em qualquer proporção |
| Curvas distintas por efeito | Portal usa `t³` (acelerado); fade do texto usa `t` puro (linear) — decisão consciente após identificar que uma curva só deixava o fade abrupto |
| Interpolação linear (lerp) | Slider de comparação de fotos — posição do arrasto (`t` de 0 a 1) revela a imagem e calcula uma estimativa de tempo/crescimento *(em implementação)* |
| Dado real × constante real | A estimativa do slider cruza a data de entrada na Obramax (já presente em `experienceData`) com a taxa média de crescimento de cabelo humano (~1,25cm/mês) |

---

## 3. Linguagens principais

- **HTML + CSS + JavaScript puro** — escolha deliberada, sem framework, para consolidar fundamentos antes de aumentar complexidade.
- **Git** — aprendizado iniciado pelo GitHub Desktop (interface visual); migração para o terminal está planejada como próximo passo natural.
- **C# (Unity)** — usado no projeto paralelo ERNEMETH-RTS (ver seção 5.1), mantido em repositório próprio, mas citado aqui porque agora tem uma seção dedicada dentro do portfólio.

**Planejadas, ainda não implementadas:**
- **Python** — camada de processamento de dados (ex: script que analisa commits e gera `.json` consumido pelo JS), rodando via GitHub Actions.
- **SQL** — alinhado ao objetivo de carreira em Banco de Dados; entraria como seção futura ("Laboratório") com queries reais ou mini playground no navegador.

---

## 4. Estrutura do projeto e arquivos

Repositório: `Yuri_Portifolio` — hospedado via GitHub Pages em `https://[usuário].github.io/Yuri_Portifolio/`

| Arquivo | Função |
|---|---|
| `index.html` | Arquivo único com HTML, CSS (`<style>`) e JS (`<script>`) — toda a intro + a home |
| `pins.txt` | 5500 pares de índices de pinos (0–399) usados para desenhar a string art |
| `foto-antes.jpg` | Foto de perfil — cabelo mais curto (entrada na Obramax) |
| `foto-agora.jpg` | Foto de perfil — cabelo atual, mais comprido |
| `CONTEXTO-PORTFOLIO.md` *(este arquivo)* | Documento de contexto — decisões, estrutura, histórico resumido |

> `index.html` precisa manter exatamente esse nome — é convenção universal de servidores web: é o arquivo carregado por padrão quando nenhum nome é especificado na URL.

---

## 5. Pontos principais sobre mim relacionados ao projeto

- Yuri Brandão Nemeth (Yusef), 21 anos, Praia Grande – SP.
- Atualmente Vendedor Júnior de Marcenaria na Obramax (promovido por mérito, começou como Operador de Caixa) — buscando vaga interna de Estagiário em TI.
- Formação técnica: ETEC Técnico em Informática para Internet (TCC "Conecta", premiado como melhor da área); FATEC ADS (4 semestres concluídos, matrícula pausada).
- Em andamento: Mate Academy — Engenharia de QA e Análise de Dados (sequencial).
- Concluído: IBM SkillsBuild — Getting Started with Data (2025).
- Experiência prévia em TI: infraestrutura (GS Tecnologia, Credlar) e análise de custos (Clínica Sion).
- Interesses pessoais que alimentam a identidade visual do site: matemática, xadrez, astronomia, engenharia aeroespacial, música (baixo elétrico, banda com apresentações públicas).
- Projeto paralelo em destaque: **ERNEMETH-RTS**, jogo de estratégia espacial em Unity, ancorado no sobrevoo real da Voyager 2 por Netuno (1989), sob licença GPL-3.0, modelo de distribuição inspirado em *Mindustry* — agora tem seção própria dentro da home do portfólio (ver 5.1).
- Metodologia declarada para entrevistas: uso de IA (Claude) como multiplicador de produtividade no desenvolvimento, mantendo domínio técnico das decisões de arquitetura e design.
- Objetivo imediato: currículo enviado ao diretor da Obramax; portfólio sendo avaliado em paralelo.

### 5.1 ERNEMETH-RTS — resumo usado na seção "Projeto em Destaque"

O projeto tem seu próprio documento de status (`ERNEMETH-RTS_Status.md`, mantido à parte, no repositório do jogo). O texto abaixo é a versão resumida que foi incorporada ao `index.html` — qualquer atualização de fundo no jogo deve, em algum momento, refletir aqui também:

- **Premissa:** RTS/Tower Defense espacial 2D (2.5D), Unity, ancorado em agosto de 1989 (sobrevoo real da Voyager 2 por Netuno). Um satélite não-tripulado escaneia um objeto anômalo; o próprio escaneamento — não hostilidade — dispara a reação de um enxame alienígena.
- **Lore/IA dos inimigos:** comportamento de formigueiro real (stigmergy, campo de feromônio compartilhado), decisão consciente contra o padrão clássico de Boids (que gera coesão de cardume, não o caos individual desejado).
- **Sistema Solar:** já funcional para Mars, Earth, Jupiter, Saturn, Uranus e Neptune — órbitas, escalas (`unitsPerAU`, `visualDiameter`) e câmera de zoom exponencial, todos com dados reais. Construído *antes* de qualquer sistema de jogabilidade, por decisão de arquitetura.
- **Licença e distribuição:** GPL-3.0, mesmo modelo do Mindustry (código aberto no GitHub, venda na Steam, grátis no itch.io com doação).
- **Estado do link:** a seção no portfólio tem um botão "Ver repositório" apontando para `#` como placeholder — trocar pela URL real do repositório assim que o projeto estiver pronto para divulgação pública.

---

## 6. Diário de bordo

> Cada entrada resume o que mudou e por quê — útil para retomar o contexto rapidamente numa próxima sessão. Novas entradas devem ser adicionadas no topo.

### Última entrega concluída — Layout full-screen + correção do portal + seção ERNEMETH-RTS
- **O quê:**
  1. **Bug do portal corrigido:** `.content` não tinha `z-index` explícito. Quando o JS aplicava `transform: scale(...)` durante `goHome()`, isso criava um novo *stacking context* para `.content` — e sem `z-index`, `.home` (que tem `z-index: 1`) passava a vencer essa disputa e aparecia por cima do overlay mascarado, revelando a página inteira de uma vez em vez de só através do furo crescente. Corrigido com `z-index: 10` fixo em `.content`.
  2. **Layout redesenhado para tela cheia:** a antiga coluna central (max-width 900px, tudo empilhado verticalmente) virou um *shell* de duas áreas: `.corner-profile` (foto + nome, fixa/`sticky` à esquerda) e `.home-main` (bio + timelines + projeto, área rolável à direita). Abaixo de 860px, empilha automaticamente (photo vira linha no topo).
  3. **Timelines viraram faixas horizontais estilo Netflix:** `.timeline` (coluna vertical com espinha) virou `.timeline-row` (flex horizontal, `overflow-x: auto`), com cards de largura fixa (270px). Rolagem da rodinha do mouse é convertida para rolagem horizontal via JS, mas só quando o cursor está sobre a faixa — fora dela, a rolagem normal da página continua.
  4. **Barra de rolagem recolorida:** branca (padrão do navegador) → fina, cor `--dim` com hover `--accent`, sem invadir visualmente o design (`scrollbar-width`/`scrollbar-color` para Firefox, `::-webkit-scrollbar-*` para Chrome/Edge/Safari). Ideia do cubo mágico documentada como `TODO` no CSS, para implementação futura.
  5. **Nova seção "Projeto em Destaque":** card dedicado ao ERNEMETH-RTS, com resumo da premissa narrativa, decisão de arquitetura (Sistema Solar antes da jogabilidade), lore do enxame inimigo e tags de tecnologia — conteúdo puxado do documento de status separado do jogo (ver seção 5.1).
- **Por quê:** o print enviado mostrava três problemas concretos: transição do portal revelando tudo de uma vez ao invés de progressivamente, tela mal aproveitada (design pensado primeiro pro celular), e barra de rolagem branca quebrando a estética HUD.
- **Estado:** `index.html` atualizado e entregue como arquivo completo, pronto para substituir o antigo e commitar. Falta trocar o link placeholder (`href="#"`) do botão "Ver repositório" pela URL real assim que o repo do jogo estiver pronto pra divulgação.

### Entrega anterior — Timeline de carreira em duas colunas
- **O quê:** separação da timeline única em duas trilhas paralelas ("Experiência Profissional" e "Formação"), com o item de intercâmbio em Boston duplicado nas duas (é profissional e educacional ao mesmo tempo), inclusão da formação IBM SkillsBuild, e mecanismo de expansão ao passar o mouse (ou tocar, no celular), revelando um campo `details` por item.
- **Por quê:** ficou mais claro separar as duas naturezas de experiência, e havia a necessidade de contar a história de cada item sem poluir a visualização inicial — a expansão sob demanda resolve isso.
- **Estado:** superada pela reestruturação em faixas horizontais desta sessão, mas a lógica de expansão por item (`.expanded`, campo `details`) foi mantida.

### Em andamento — Slider de comparação de fotos
- **O quê:** substituição da foto de perfil estática por um slider interativo "antes/depois" (cabelo curto → cabelo comprido), usando `clip-path` controlado por arrasto, com legenda calculada dinamicamente via interpolação linear.
- **Por quê:** aproveitar a própria mudança de visual ao longo do tempo na empresa como material para uma peça matematicamente fundamentada, em vez de puramente decorativa.
- **Estado:** CSS já implementado (agora com a foto reduzida para 96px, no canto); HTML (imagens + divisor + legenda) e JavaScript (lógica de arrasto e cálculo) ainda pendentes — finalizar numa próxima sessão.

### Pendente de priorização — Elementos decorativos
- Lista aguardando decisão sobre quais construir primeiro: constelações, xadrez, contrabaixo elétrico, óculos, fórmulas matemáticas, ligações químicas, referência ao TCC "Conecta".

### Pendente — Cubo mágico como barra de rolagem
- Ideia registrada (ver seção 1) para substituir o thumb da barra de rolagem por um cubo mágico que se resolve conforme a página avança. Depende de material de vídeo/render 2D ou 3D ainda não providenciado.

---

*Documento gerado a partir da conversa de desenvolvimento do portfólio.*
