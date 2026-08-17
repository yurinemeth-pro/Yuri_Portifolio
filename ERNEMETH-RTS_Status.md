---
projeto: ERNEMETH-RTS
tipo: status-de-desenvolvimento
data_atualizacao: 2026-08-03
engine: Unity 6 (2D Core template, com mistura 3D)
repositorio: GitHub (público, GPL-3.0)
---

# ERNEMETH-RTS — Status do Projeto

## Visão geral do jogo

RTS/Tower Defense espacial 2D (com elementos 2.5D/3D), inspirado visualmente em **Ender's Game**, com modelo de distribuição inspirado no **Mindustry**: código aberto no GitHub, venda na Steam, grátis no itch.io com opção de doação.

### Premissa narrativa (Fase 1 — "Primeiro Encontro")

- Ano de ancoragem histórica: **agosto de 1989**, referência ao sobrevoo real da Voyager 2 por Netuno.
- Telescópios detectam objeto anômalo se aproximando da órbita de Netuno.
- Jogador lança um **satélite não-tripulado** (interface retrô/arcaica de lançamento — inspirado em salas de controle de missão estilo Apollo). Viagem representada no mapa do Sistema Solar, não jogada em tempo real.
- Satélite escaneia o objeto ao chegar. **O próprio escaneamento é o gatilho do "ataque" alien** — não é hostilidade intencional, é resposta instintiva a estímulo tecnológico (analogia usada: formigas atraídas por corrente elétrica em fios, mastigam e morrem eletrocutadas, gerando mais feromônio de alarme — mesmo princípio da resposta alien).
- Satélite comprometido/destruído, mas envia dados parciais.
- Jogador envia **nave tripulada** (primeira jogabilidade de controle direto) para recuperar/decodificar os dados restantes.
- Chegada da nave revela a escala real do "inimigo" — momento de impacto visual ("colossal").
- Missão é **essencialmente suicida**: sem armas (nem satélite nem nave têm motivo narrativo pra ter armamento ainda). Objetivo é extrair e transmitir dados antes do fim.
- Curiosidade real incorporada à narrativa: cápsula de dados enviada à Terra na velocidade da **Parker Solar Probe** (~692.000 km/h, objeto mais rápido já construído pela humanidade).
- Dados chegam à Terra, provam ameaça real → gancho pra Fase 2 (tecnologia militar espacial começa a evoluir, gradualmente ofensiva/defensiva).

### Lore dos inimigos (enxame alien)

- Estrutura social: **Rainhas** (múltiplas, produzem e compartilham trilha de feromônio) → **Enxames** (subdivididos, não uma nuvem única a maior parte do tempo) → **Unidades individuais** com papéis (coletora, defensora, atacante-leve, tanque).
- Unidades reagem a um **campo de feromônio compartilhado**, não a um alvo fixo — comportamento tipo formigueiro real (stigmergy), não Boids (Boids = coesão de grupo tipo cardume/bando; o que se quer é caos individual dentro de um direcionamento coletivo).
- Se a Rainha morre, unidades órfãs buscam Rainha viva mais próxima; sem uma por perto, morrem (referência: filme *Ender's Game*).
- Ataque: feixe de energia (referência visual: bobina de Tesla / cena do filme *Ender's Game*), disparado quando próximo o suficiente de um alvo emissor do "estímulo errado".
- Morte é **individual e unitária** (gera destroços/explosão), nunca ilusão de enxame — cada nave é um objeto real e destrutível.
- Movimento: precisa ser **erraticamente orgânico** (tipo formiga vasculhando), não suave/previsível. Boids foi descartado por gerar movimento coeso demais (estilo cardume), não o caos individual desejado.

### Inspirações de referência

- **Ender's Game** (filme): interfaces holográficas, ataques tipo bobina de Tesla, treinamento de pilotos na Battle Sphere.
- **Mindustry**: modelo de distribuição open source + comercial.
- **Project Hail Mary**: tom narrativo — "tiro no escuro", evolução tecnológica rápida por necessidade.
- **Sins of a Solar Empire**: zoom contínuo sem telas de carregamento, do mapa estratégico até o nível tático.
- **Kerbal Space Program**: sistema de time warp (aceleração de tempo com desaceleração automática perto de eventos importantes).
- **Auralux: Constellations** / solarsystemscope.com: referência visual pro 2.5D (planetas com volume/luz real, jogabilidade em plano).
- **0RBITALIS**: referência de simulador de lançamento de satélite (avaliado e descartado como modelo direto — ver decisões abaixo).

---

## Decisões de arquitetura já tomadas

1. **Engine**: Unity (migrando de GameMaker). Template 2D (Core), mas confirmado que aceita mistura de elementos 3D sem problema (abordagem 2.5D).
2. **Licença**: GPL-3.0 (mesma do Mindustry) — protege o código-fonte de fechamento por terceiros, não impede venda de builds compilados nem distribuição gratuita. Arte/assets podem ter copyright separado.
3. **Repositório**: público, único, dedicado só ao jogo (separado do portfólio pessoal). Git LFS configurado desde o commit inicial (tipos rastreados: psd, png, jpg, mp3, wav, fbx, tga, exr, mp4).
4. **Estrutura de pastas fixada** (`Assets/`):
   ```
   Assets/
   ├── Scripts/
   │   ├── Solar/       → GameClock, PlanetData, OrbitalBody, SolarSystemSettings
   │   ├── Camera/       → CameraZoom
   │   └── Enemies/      → EnemyUnit, EnemySpawner
   ├── Data/
   │   ├── Planets/      → arquivos PlanetData (Earth, Mars, Jupiter, Saturn, Uranus, Neptune — falta Mercúrio, Vênus e o Sol)
   │   └── Settings/      → SolarSystemSettings (MainSettings)
   ├── Prefabs/          → EnemyUnit prefab
   └── Scenes/            → SolarSystem_Test e outras
   ```
5. **Migração do GameMaker**: decisão de **não** trazer o projeto antigo como histórico de commits (engines incompatíveis, sem continuidade real de versionamento). Código antigo serve só como referência de lógica, traduzido/reformulado no Unity, não copiado.
6. **Abertura do jogo**: sem menu tradicional separado — considerado incorporar o "menu" dentro do próprio painel retrô de lançamento do satélite, pra não quebrar imersão.
7. **Sistema Solar como fundação**: decidido construir o Sistema Solar funcional (relógio, órbitas, câmera de zoom) **antes** de qualquer sistema de gameplay (combate, interface de lançamento), porque a interface de lançamento vai depender de mostrar posições sobre esse sistema.
8. **Escala dupla (distância x tamanho)**: distância orbital usa fator de compressão único e ajustável (`unitsPerAU`, atualmente testando valores como 100); tamanho visual dos planetas (`visualDiameter`) é **independente** e ajustável por planeta — decisão consciente de não usar escala real 1:1 (planetas ficariam menores que 1 pixel se a distância fosse real). Foi cogitado Terra = 10 como base de proporção, com leve compressão adicional só no Sol (que seria 109x a Terra em escala real, desequilibrando a cena).

---

## Sistemas já implementados e funcionando

### 1. Sistema de enxame inimigo (`EnemyUnit.cs` + `EnemySpawner.cs`)

- **Status**: funcional, testado com **35.000–40.000 unidades simultâneas** antes de travar (sem otimização de performance ainda — Object Pooling e particionamento espacial ficam pra depois, há bastante margem).
- Movimento final aprovado pelo usuário: baseado em direção "desejada" (atração a um `stimulus` + comportamento de patrulha/órbita quando perto) somada a uma **decisão errática em intervalos irregulares** (não periódica), sobrescrevendo a direção diretamente (não como força acumulada) para simular decisão abrupta tipo formiga.
- Histórico de tentativas descartadas, documentado para não repetir erros:
  - Somar forças (atração + jitter + espasmo) com atrito fixo → gerava **curva rosácea** (dois movimentos periódicos se somando, sem dissipação real de energia).
  - Atrito sem compensar `Time.deltaTime` → movimento ficou dependente de frame rate, quase imperceptível (força de atração e perda por atrito quase se anulavam).
  - `Vector2.Lerp` da velocidade + Perlin noise → movimento suave demais, "bolinha de gude caindo num funil" (Perlin noise é ruído contínuo/correlacionado, Lerp é filtro passa-baixa — o oposto de errático).
  - Solução final: `erraticOffset` recalculado só em `Time.time >= nextDecisionTime`, com `nextDecisionTime` sorteado em intervalo irregular (`Random.Range(0.15f, 0.8f)`), sobrescrevendo a direção (não somando a uma força suavizada).
- `EnemyUnit` é Prefab (`Assets/Prefabs`). `EnemySpawner` instancia N cópias espalhadas (`Random.insideUnitCircle * spawnRadius`) e atribui o mesmo `stimulus` a todas.
- **Pendência de design ainda não resolvida tecnicamente**: campo de feromônio compartilhado (hoje o `stimulus` é um `Transform` fixo único, não um grid de feromônio real); lógica de Rainha e orfandade; separação entre unidades (existia no protótipo de força, foi removida na versão de steering — avaliar se precisa voltar quando houver múltiplas unidades reais em vez do teste atual).

### 2. Sistema Solar (`GameClock.cs`, `PlanetData.cs`, `OrbitalBody.cs`, `SolarSystemSettings.cs`, `CameraZoom.cs`)

- **Status**: funcional para Mars, Earth, Jupiter, Saturn, Uranus, Neptune. **Faltam**: Mercúrio, Vênus, e principalmente **o Sol** (ainda não criado — precisa de `PlanetData` com `orbitalRadiusAU = 0` e uma luz de verdade, Point Light ou Directional Light).
- `GameClock`: relógio de jogo com `DateTime` real, iniciando em 25/08/1989 (data configurável), com `timeScale` para aceleração (time warp simples, ainda **não** tem a desaceleração automática perto de eventos que o KSP tem — isso é pendência futura).
  - Usa `double` para `secondsElapsedSimTime` (não `float`) para evitar perda de precisão em sessões longas de jogo com time warp alto.
- `PlanetData` (ScriptableObject): dados por planeta — `orbitalRadiusAU`, `orbitalPeriodDays`, `startingAngleDegrees`, `visualDiameter`. Valores reais usados até agora:

  | Planeta | Orbital Radius AU | Orbital Period Days |
  |---|---|---|
  | Mercúrio | *(não criado ainda)* 0.39 | 88 |
  | Vênus | *(não criado ainda)* 0.72 | 225 |
  | Earth | 1.0 | 365.25 |
  | Mars | 1.52 | 687 |
  | Jupiter | 5.2 | 4331 |
  | Saturn | 9.58 | 10747 |
  | Uranus | 19.2 | 30589 |
  | Neptune | 30.05 | 59800 |

  Diâmetros reais relativos (Terra = 1), para referência ao decidir `visualDiameter`: Mercúrio 0.38 · Vênus 0.95 · Marte 0.53 · Júpiter 10.97 · Saturno 9.14 · Urano 3.98 · Netuno 3.86 · **Sol 109.3**. Discussão em aberto: usar Terra = 10 como base, decidir se o Sol usa a proporção real (ficaria ~1093) ou uma compressão adicional só nele.

- `SolarSystemSettings` (ScriptableObject único, `MainSettings`, em `Assets/Data/Settings`): centraliza `unitsPerAU`, compartilhado por todos os `OrbitalBody` — permite testar escalas de distância diferentes (ex: 100, 150, 200) mudando um único valor, sem editar planeta por planeta. Valor em teste no momento: **100**.
- `OrbitalBody`: por planeta, calcula posição orbital a partir do `GameClock` + `PlanetData` + `SolarSystemSettings` (órbita circular simplificada, sem excentricidade real ainda — fora de escopo por ora). Também desenha a própria linha de órbita (`LineRenderer`, círculo gerado matematicamente, 100 segmentos) **no mesmo objeto**, não em objeto separado (simplificação pedida pelo usuário).
  - Aplica `transform.localScale` a partir de `data.visualDiameter` para o tamanho visual do planeta (independente da distância orbital).
- `CameraZoom`: zoom por scroll do mouse, ajustando `orthographicSize` da câmera ortográfica.

### Bugs encontrados e resolvidos (documentados para não repetir)

| Sintoma | Causa real | Correção |
|---|---|---|
| `Can't add script behaviour... needs to derive from MonoBehaviour` | Nome da classe no arquivo não batia com nome do arquivo, ou faltava `: MonoBehaviour` | Garantir `public class NomeExato : MonoBehaviour` idêntico ao nome do arquivo `.cs` |
| Unity criou subpasta duplicada (`ERNEMETH-RTS/ERNEMETH/`) ao criar projeto | Campo "Project Name" do Unity Hub criou pasta extra dentro do repo já clonado | Mover conteúdo pra pasta raiz do repo, deletar subpasta vazia, reabrir projeto pelo caminho corrigido via "Add project from disk" |
| Objetos criados/configurados durante o modo Play sumiram ao parar | Unity descarta qualquer mudança feita na Hierarchy durante o Play Mode — não é bug, é comportamento padrão | Regra fixada: sempre configurar/criar objetos com o jogo **parado** (fora do modo Play) |
| Zoom da câmera não fazia nada, erro no Console (`InvalidOperationException`) | Projeto configurado para o novo Input System, mas script usava `Input.GetAxis` (API antiga) | Edit → Project Settings → Player → Other Settings → **Active Input Handling** → mudar para **Both** |
| Órbita desenhada "grudava" e girava junto com o planeta | `LineRenderer.useWorldSpace = false` fazia os pontos serem relativos à posição do objeto (que se move) | Trocar para `useWorldSpace = true` — pontos passam a ser coordenadas absolutas do mundo |
| Planetas maiores viraram "rosquinhas"/anéis grossos ao redor de si mesmos | `LineRenderer.startWidth/endWidth` é multiplicado pela `transform.localScale` do objeto onde está — como o mesmo objeto agora tinha escala grande (`visualDiameter`), a linha da órbita também escalava | **Resolvido definitivamente por**: `transform.localScale = new Vector3(data.visualDiameter, data.visualDiameter, 1f);` (ver nota abaixo) |
| Tudo aparecia "pixelado/8-bit" na aba Game | Não era bug real — o controle **Scale** da aba Game do Editor estava em `3.4x`, esticando a imagem renderizada em resolução menor | Resetar o Scale da aba Game para `1x`. Não afeta builds finais, é só visualização do Editor |
| Zoom da câmera "travava" sem grande efeito mesmo com valores enormes (`zoomSpeed` e `maxZoom` na casa dos milhões/trilhões) | Zoom estava implementado de forma **linear** (`orthographicSize -= scrollInput * zoomSpeed`) — não funciona bem quando a cena tem escalas muito diferentes (perto de um planeta vs. sistema inteiro, ~300x de diferença) | Trocar para zoom **multiplicativo/exponencial** (`orthographicSize *= fator`), que se adapta naturalmente a diferentes escalas. `maxZoom` recalibrado para bater com a escala real da cena (~4000, considerando Netuno a ~3000 unidades com `unitsPerAU = 100`) |
| Planetas apareciam todos do mesmo tamanho na Scene View, mesmo com `visualDiameter` diferentes | `transform.localScale` só é aplicado dentro do `Start()`, que só roda em modo Play — Scene View (modo Edit) não executa scripts, mostra o objeto "cru" | Não é bug — comportamento esperado. Resolvido apenas mentalmente (checar sempre em modo Play); `[ExecuteInEditMode]` é opção futura de conveniência, não aplicada ainda |

**Nota importante sobre a correção final das "rosquinhas"**: a correção definitiva que funcionou foi trocar a linha de escala para:
```csharp
transform.localScale = new Vector3(data.visualDiameter, data.visualDiameter, 1f);
```
(em vez de `Vector3.one * data.visualDiameter`, que também escalava o eixo Z e afetava a espessura da linha de forma mais imprevisível). A tentativa anterior de compensar dividindo a largura da linha (`0.05f / data.visualDiameter`) foi **substituída** por esta abordagem mais direta — vale confirmar no início do próximo chat se essa é de fato a versão final em uso no `OrbitalBody.cs`, e se a divisão de largura ainda está ou não presente no código (para não haver dupla compensação).

---

## Estado exato dos scripts (para colar no próximo chat se necessário)

> Recomendo, no início da próxima sessão, copiar o conteúdo atual e completo de `OrbitalBody.cs`, `PlanetData.cs`, `GameClock.cs`, `SolarSystemSettings.cs`, `CameraZoom.cs`, `EnemyUnit.cs` e `EnemySpawner.cs` direto do editor (Visual Studio/VS Code), já que passaram por várias iterações e o histórico de chat tem versões intermediárias que **não** refletem o estado final. Este documento resume decisões e causas de bugs, não é um changelog linha-a-linha do código.

---

## Próximos passos (em ordem sugerida, não decidida ainda)

1. **Confirmar e fechar** a correção das "rosquinhas" (verificar código atual de `OrbitalBody.cs`, remover qualquer compensação de largura de linha redundante).
2. **Criar o Sol**: `PlanetData` com `orbitalRadiusAU = 0`, adicionar iluminação real (Point Light ou Directional Light) — importante para validar o efeito 2.5D (planetas iluminados de um lado só) mencionado como requisito original.
3. **Decidir a proporção final de `visualDiameter`** para todos os planetas (Terra = 10 como base proposta, decidir compressão ou não do Sol).
4. **Criar Mercúrio e Vênus** (únicos planetas reais faltando nos dados).
5. Considerar implementar `[ExecuteInEditMode]` no `OrbitalBody` para visualizar escala/posição corretamente também fora do modo Play (conveniência, não bloqueante).
6. Depois do Sistema Solar fechado: interface de lançamento do satélite (painel retrô estilo controle de missão Apollo) — reaproveita o `SatelliteTravel.cs` já criado (nota: há um warning pendente nesse script — campo `elapsedTime` declarado mas nunca usado — resolver ao retomar esse arquivo).
7. Mais adiante: campo de feromônio real (grid, não `Transform` fixo), sistema de Rainha, spatial partitioning para separação entre unidades em maior escala, Object Pooling para otimizar o spawn de inimigos.

---

## Como retomar em outro chat

Sugestão de mensagem inicial para o próximo chat:

> "Estou continuando o desenvolvimento do ERNEMETH-RTS. Anexei o documento de status do projeto. Vamos retomar a partir de [X — escolher um item da lista de Próximos Passos]."

Anexar este arquivo markdown já deve dar contexto suficiente sobre decisões tomadas, arquitetura e histórico de bugs resolvidos, sem precisar reexplicar tudo do zero.
