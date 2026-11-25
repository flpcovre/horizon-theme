# Horizon Theme — Notas Técnicas, Ajustes e Decisões

## 1. Fonte da verdade e variantes encontradas (Fase 1)

Clonado diretamente de `https://github.com/alexandernanberg/horizon-theme-vscode` (v1.0.1).

Arquivos encontrados em `themes/` e `src/`:
- **Dark** (`src/dark/globals.json`, `src/dark/template.json`): `horizon.json`, `horizon-italic.json`, `horizon-bold.json`
- **Bright / Light** (`src/bright/globals.json`, `src/bright/template.json`): `horizon-bright.json`, `horizon-bright-italic.json`, `horizon-bright-bold.json`
- **`semanticTokenColors`**: ausente em todos os JSONs (apenas `"semanticHighlighting": true`, sem mapa de cores semânticas).

### Diferenças encontradas vs. âncoras de terceiros
- **Light `foreground`**: o JSON real usa `#06060C` (`ui.darkText`) para UI e `#333333` (`syntax.black`) para pontuação/operadores — não `#403C3D`.
- **Dark `comment`**: o JSON real usa `#BBBBBB4D` (`syntax.gray` em 30% de opacidade) — `#6C6F93` é `ui.accentAlt` (usado em seleções, scrollbars, rulers e cursor de terminal).
- **Dark `tokenColors`**: quase todos os tokens de sintaxe usam sufixo alfa `E6` (90% opacidade), que o Monaco do Orca compõe automaticamente sobre `--editor-surface`.

### Par entregue
Como o Orca seleciona 1 tema Dark e 1 tema Light por slot, entregamos o par canônico **Horizon** (`base: "dark"`) e **Horizon Bright** (`base: "light"`), além dos respectivos `terminalThemes`. Ambas as variantes base já preservam itálicos em `comment`, `markup.quote`, `markup.italic`, `variable.language`, `variable.parameter` e negrito em `markup.bold`. As variantes `-italic` e `-bold` do repositório original alteram apenas `fontStyle` de 3–5 escopos (`keyword`, `storage`, `entity.other.attribute-name`, `entity.name.tag`, `punctuation.definition.tag`) mantendo exatamente as mesmas cores.

---

## 2. Ajustes de contraste (apenas luminosidade, matiz preservado)

Todos os 61 pares/regras de contraste do Orca (`src/renderer/src/app-shell/appearance/plugin-theme-contrast-rules.ts` + `plugin-theme-tokens.ts`), os 5 anéis (`RINGS`, ≥ 2.0:1), as 7 bordas (`BORDERS`, alpha ≥ 0.05) e todas as 34 regras de `tokenColors` com `foreground` (≥ 3.0:1 sobre `--editor-surface`) passaram com **0 falhas**.

### Horizon Dark (`themes/horizon.json`, fundo `#1C1E26`)

| Token / Escopo | Origem Horizon | Composto sobre fundo | Ajustado | ΔL (H fixo) | Contraste (antes → depois) | Motivo |
| --- | --- | --- | --- | --- | --- | --- |
| `--muted-foreground`, `--tab-inactive-foreground`, `--chat-muted-foreground` | `#D5D8DA80` | `#797B80` (H=220.0°) | `#8D8F93` | +7.6% | 3.49 → 4.58 (sobre `--chat-canvas` `#25272F`) | Exige ≥ 4.5:1 opaco em `--background`, `--muted`, `--tab-inactive` e `--chat-canvas`. |
| `--diff-removed` | `#F43E5C` | `#F43E5C` (H=350.1°) | `#F44260` | +0.9% | 4.497 → 4.58 (`--chat-diff-removed` sobre `--diff-removed-background`) | Limite exato de 4.5:1 em chat diff sobre `--chat-canvas`. |
| `--git-decoration-ignored` | `#D5D8DA4D` | `#54565C` (H=223.7°) | `#686B72` | +8.2% | 2.27 → 3.12 (sobre `--sidebar` `#1C1E26`) | Git decorations exigem ≥ 3.0:1 opaco sobre `--sidebar`. |
| `tokenColors: comment` | `#BBBBBB4D` | `#4C4D53` (H=228.0°) | `#696B72` | +11.7% | 1.98 → 3.12 (sobre `#1C1E26`) | Monaco descarta `tokenColors` < 3.0:1. |
| `tokenColors: punctuation.definition.tag` | `#E95678B3` | `#AC4560` (H=344.6°) | `#B04762` | +1.0% | 2.99 → 3.11 (sobre `#1C1E26`) | Ficava em 2.99:1 (0.01 abaixo do corte do Monaco). |

*(Nota: `--sidebar-foreground` `#A0A2A7`, `--host-badge-foreground` `#A0A2A7` e `--git-decoration-added` `#24A075` são composições opacas exatas de `#D5D8DAB3` e `#27D797B3` sobre `#1C1E26`, sem alteração de cor visual.)*

### Horizon Bright (`themes/horizon-bright.json`, fundo `#FDF0ED`)

| Token / Escopo | Origem Horizon | Composto sobre fundo | Ajustado | ΔL (H fixo) | Contraste (antes → depois) | Motivo |
| --- | --- | --- | --- | --- | --- | --- |
| `--text-link` | `#E84A72` | `#E84A72` (H=344.8°) | `#D61B4B` | -12.6% | 3.34 → 4.58 (sobre `#FDF0ED`) | Exige ≥ 4.5:1 sobre `--editor-surface` (e `--chat-link` ≥ 4.5:1 sobre `--chat-user-message`/`--chat-inline-code`). |
| `--chat-inline-code-foreground` | `#DC3318` | `#DC3318` (H=8.3°) | `#BC2B14` | -7.1% | 3.54 → 4.58 (sobre `#FADAD1`) | Exige ≥ 4.5:1 sobre `--chat-inline-code` `#FADAD1`. |
| `--diff-added` | `#07DA8C` | `#07DA8C` (H=157.8°) | `#059963` | -13.1% | 2.58 → 4.59 (`--chat-diff-added` sobre `--chat-code-block`) | Garante contraste ≥ 4.5:1 nas linhas de diff do chat. |
| `--diff-removed` | `#F43E5C` | `#F43E5C` (H=350.1°) | `#F33555` | -1.8% | 4.43 → 4.60 (`--chat-diff-removed` sobre `--chat-code-block`) | Garante contraste ≥ 4.5:1 nas linhas de diff do chat. |
| `--git-decoration-added` | `#1EB980B3` | `#60C9A0` (H=156.6°) | `#349B73` | -17.7% | 1.81 → 3.10 (sobre `#FDF0ED`) | Git decorations exigem ≥ 3.0:1 opaco sobre `--sidebar`. |
| `--git-decoration-renamed` | `#1EAEAE` | `#1EAEAE` (H=180.0°) | `#1A9999` | -4.7% | 2.44 → 3.11 (sobre `#FDF0ED`) | Exige ≥ 3.0:1 sobre `--sidebar`. |
| `--git-decoration-untracked` | `#1EB980` | `#1EB980` (H=157.9°) | `#199D6D` | -6.3% | 2.27 → 3.10 (sobre `#FDF0ED`) | Exige ≥ 3.0:1 sobre `--sidebar`. |
| `--git-decoration-ignored` | `#06060C4D` | `#B2A9A9` (H=1.8°) | `#948787` | -12.6% | 2.06 → 3.11 (sobre `#FDF0ED`) | Exige ≥ 3.0:1 sobre `--sidebar`. |
| `tokenColors: comment` | `#33333380` | `#989190` (H=11.2°) | `#8F8887` | -3.3% | 2.78 → 3.12 (sobre `#FDF0ED`) | Monaco descarta `tokenColors` < 3.0:1. |
| `tokenColors: entity.name`, `entity.name.type`, `storage.type.cs`, `support`, `variable.language` | `#F77D26` | `#F77D26` (H=25.0°) | `#E36308` | -9.7% | 2.38 → 3.12 (sobre `#FDF0ED`) | Monaco descarta `tokenColors` < 3.0:1. |
| `tokenColors: string*`, `entity.other.inherited-class`, `pseudo-element/class`, `markup.underline.link` | `#F6661E` | `#F6661E` (H=20.0°) | `#F0570A` | -5.1% | 2.76 → 3.11 (sobre `#FDF0ED`) | Monaco descarta `tokenColors` < 3.0:1. |
| `tokenColors: markup.quote` | `#F6661EB3` | `#F88F5C` (H=19.7°) | `#F1560A` | -17.3% | 2.08 → 3.10 (sobre `#FDF0ED`) | Monaco descarta `tokenColors` < 3.0:1. |

---

## 3. Regras de `tokenColors` adaptadas para o Orca

1. **Seletor descendente único do Horizon**:
   - Original: `"string.template meta.embedded"` (`#BBBBBB` Dark / `#333333` Light)
   - Ação: convertido para `"meta.embedded"` (escopo final mais específico), pois o Orca ignora seletores com espaço. Mantém expressões interpoladas (`${...}`) em cor neutra; tokens internos mais específicos (`variable`, `keyword`, `string`) sobrescrevem normalmente.
2. **`"fontStyle": "normal"`**:
   - Original: presente em `entity.name.tag`, `entity.other.attribute-name`, `keyword`, `storage`, `punctuation.definition.tag`.
   - Ação: propriedade `fontStyle` omitida nesses 5 casos porque o validador estrito do Orca aceita apenas `italic`, `bold`, `underline`, `strikethrough` ou `""`.
3. **Escopos adicionados para cobertura total do Monaco (`MONACO_TOKEN_SCOPES`)**:
   - `"string"` adicionado junto a `["string.quoted", "string.template"]`: sem o escopo raiz `"string"`, o mapeamento do Monaco do Orca deixaria todas as strings de TS/JS/JSON/HTML/YAML/SQL/Python no fallback padrão do VS Code.
   - `"punctuation"` adicionado junto a `"punctuation.separator"`: garante que o token `delimiter` do Monaco use `#BBBBBB` / `#333333`.
   - `"meta.decorator"` adicionado junto a `["entity.name.function", "support.function"]`: mapeia `annotation` (decorators em TypeScript/Python/Rust) para `#25B0BCE6` / `#1D8991`.
   - `"invalid"` adicionado com `ui.negative` (`#F43E5C`): cobre tokens inválidos no editor.
   - Verificação automatizada: **42/42** escopos de `MONACO_TOKEN_SCOPES` cobertos (0 fallbacks).

---

## 4. Notas sobre `terminalThemes`

- **Horizon Dark (`terminal-themes/horizon-dark.json`)**: usa os 12 códigos ANSI cromáticos exatos do Horizon (`red`..`cyan` e `brightRed`..`brightCyan`). Os 4 slots omitidos no JSON do Horizon (`black`, `white`, `brightBlack`, `brightWhite`) foram derivados diretamente de `src/dark/globals.json` (`#16161C`, `#D5D8DA`, `#6C6F93`, `#FDF0ED`), e `selectionBackground`/`cursor` foram feitos opacos (`#2E303E`/`#E95378`) conforme exigido pelo formato `#RRGGBB` do terminal no Orca.
- **Horizon Bright (`terminal-themes/horizon-bright.json`)**: no repositório original, `horizon-bright.json` reutiliza literalmente os ANSI pastéis do Dark (`#FAB795`, `#59E1E3`, etc.), que são praticamente ilegíveis sobre `#FDF0ED`. Para preservar a identidade Horizon Bright com legibilidade real no terminal, os slots ANSI usam as cores saturadas oficiais de `src/bright/globals.json` (`#DA103F`, `#1EB980`, `#F6661E`, `#1D8991`, `#8A31B9`, `#1EAEAE`, etc.).

---

## 5. Validação e limitações conhecidas

- **`orca plugin validate`**: `ok` (exit code 0, zero erros, zero warnings).
- **`orca plugin inspect`**: 2 app themes (`Horizon`, `Horizon Bright`) + 2 terminal themes (`Horizon`, `Horizon Bright`), todos abaixo de 7 KiB (limite 16 KiB).
- **`orca plugin dev load`**: carregado com sucesso em Orca (`local.horizon-theme 1.0.0 idle data, dev`), zero linhas de erro em `orca plugin logs`.
- **Verificação de contraste**: executada via script Node.js (Python não estava instalado neste ambiente Windows/WSL), replicando integralmente `rgba-contrast.ts`, `plugin-theme-contrast-rules.ts`, `plugin-theme-tokens.ts` e `plugin-monaco-theme.ts` do Orca — 61/61 regras de UI e 34/34 `tokenColors` aprovados em ambos os temas.
- **Limitações de plataforma do Orca (fora do escopo de tokens públicos)**:
  - Não há token separado para `statusBar.debuggingBackground`, `scrollbarSlider.*`, `editorIndentGuide.*` ou `editorBracketMatch.*`.
  - Diálogos de confiança/aprovação do Orca sempre usam as cores próprias do Orca.
  - Para ver 100% da paleta Horizon na sidebar esquerda, mantenha **Left Sidebar Appearance** em **Default** (as opções **Match terminal** e **Tinted** sobrescrevem a linha ativa/badges).
  - A inspeção visual lado a lado das amostras de linguagem no Orca vs. VS Code depende da sessão gráfica do app desktop do usuário (o plugin já está carregado em dev mode e recarrega automaticamente ao salvar qualquer ajuste).
