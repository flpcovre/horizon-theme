# Horizon Theme — Mapeamento VS Code → Orca

Fonte da verdade: repositório oficial [`alexandernanberg/horizon-theme-vscode`](https://github.com/alexandernanberg/horizon-theme-vscode) (v1.0.1):
- Dark: `themes/horizon.json` + paleta semântica `src/dark/globals.json`
- Light: `themes/horizon-bright.json` + paleta semântica `src/bright/globals.json`
- Tokens públicos do Orca: `docs/reference/plugin-theme-tokens.md` (`stablyai/orca`)

Legenda de origem na coluna **Decisão / Origem**:
- **Direta**: valor hex idêntico ao JSON compilado do Horizon.
- **Composta (opaca)**: cor translúcida do Horizon pré-composta sobre o fundo correspondente porque o validador de contraste do Orca exige texto/superfície opaco (`alpha = 1`).
- **Ajuste L (contraste)**: luminosidade ajustada mantendo o matiz (H) exato do Horizon para atender WCAG ≥ 4.5:1 (UI), ≥ 3.0:1 (Git/sintaxe) ou ≥ 2.0:1 (anéis).
- **Derivada da paleta**: derivada de um token semântico existente em `globals.json` (nenhum matiz novo inventado).
- **Inferida**: cor sem equivalente direto no JSON do Horizon, derivada de forma documentada da paleta.

---

## 1. Tokens de Interface (App Theme)

| Chave VS Code (Horizon) | Token Orca | Dark (Horizon) | Light (Horizon Bright) | Decisão / Origem |
| --- | --- | --- | --- | --- |
| `editor.background`, `titleBar.activeBackground`, `statusBar.background` (`ui.background`) | `--background` | `#1C1E26` | `#FDF0ED` | **Direta** |
| `editor.background` | `--editor-surface` | `#1C1E26` | `#FDF0ED` | **Direta** |
| `foreground` (`ui.lightText` / `ui.darkText`) | `--foreground` | `#D5D8DA` | `#06060C` | **Direta** (contraste 11.61:1 / 18.15:1) |
| `editorWidget.background`, `walkThrough.embeddedEditorBackground` (`ui.backgroundAlt`) | `--card` | `#232530` | `#FADAD1` | **Direta** (superfície elevada padrão do Horizon) |
| `foreground` | `--card-foreground` | `#D5D8DA` | `#06060C` | **Direta** (10.77:1 / 15.41:1) |
| `dropdown.background`, `editorWidget.background`, `breadcrumbPicker.background`, `peekViewResult.background` (`ui.backgroundAlt`) | `--popover` | `#232530` | `#FADAD1` | **Direta** (menus, popovers e diálogos) |
| `foreground` | `--popover-foreground` | `#D5D8DA` | `#06060C` | **Direta** |
| `extensionButton.prominentBackground`, `activityBarBadge.background` (`ui.secondaryAccentAlt`) | `--primary` | `#E95378` | `#E84A72` | **Direta** (botão primário icônico do Horizon; `button.background` vai para `--secondary`) |
| `activityBarBadge.foreground` (`ui.darkText`) | `--primary-foreground` | `#06060C` | `#06060C` | **Direta** (5.75:1 Dark, 5.44:1 Light) |
| `button.background`, `badge.background` (`ui.accent`) | `--secondary` | `#2E303E` | `#F9CBBE` | **Direta** (botões secundários / neutros) |
| `button.foreground`, `badge.foreground`, `foreground` | `--secondary-foreground` | `#D5D8DA` | `#06060C` | **Direta** |
| `ui.backgroundAlt` | `--muted` | `#232530` | `#FADAD1` | **Derivada da paleta** (preenchimentos sutis) |
| `sideBar.foreground` / `statusBar.foreground` (`#D5D8DA80` Dark; `#06060CB3` Light) | `--muted-foreground` | `#8D8F93` | `#504C4F` | Dark: **Composta + Ajuste L** (`#D5D8DA80` sobre `#1C1E26` = `#797B80`, CR 3.61 sobre `#232530` e 3.49 sobre `--chat-canvas` `#25272F` → ΔL +7.6% mantendo H=220°, CR 4.58 sobre `--chat-canvas`). Light: **Composta (opaca)** (`#06060CB3` sobre `#FDF0ED` = `#504C4F`, CR 6.44 sobre `#FADAD1`). |
| `list.hoverBackground`, `list.activeSelectionBackground` (base `ui.accent`) | `--accent` | `#2E303E` | `#F9CBBE` | **Direta (opaca)** (Orca exige `--accent` opaco para par de contraste) |
| `list.hoverForeground`, `list.activeSelectionForeground` | `--accent-foreground` | `#D5D8DA` | `#06060C` | **Direta** |
| `editorIndentGuide.activeBackground` (`ui.accent` Dark) / `editorGroup.border` (`#1A1C231A` Light) | `--border` | `#2E303E` | `#1A1C231A` | Dark: **Derivada da paleta** (`editorGroup.border: #1A1C23` é mais escuro que `#1C1E26` e invisível sem sombras no Orca; `#2E303E` é a divisória estrutural do Horizon). Light: **Direta** (`#1A1C231A`, alpha 0.10 ≥ 0.05). |
| `input.background` (`ui.accent` Dark) / `editorGroup.border` (Light) | `--input` | `#2E303E` | `#1A1C2326` | Dark: **Direta** (`ui.accent`). Light: **Derivada da paleta** (`ui.border` `#1A1C23` em 15% para borda de input legível). |
| `editorCursor.foreground`, `tab.activeBorder`, `progressBar.background` (`ui.secondaryAccentAlt`) | `--ring` | `#E95378` | `#E84A72` | **Direta** (`focusBorder` `#1A1C23` / `#1A1C231A` falha no mínimo 2:1 do Orca e o Orca usa `--ring` como `editorCursor.foreground` no Monaco, que no Horizon é exatamente `#E95378` / `#E84A72`). |
| `sideBar.background`, `activityBar.background` | `--sidebar` | `#1C1E26` | `#FDF0ED` | **Direta** (sidebar direita e navegação de Settings, com borda explícita) |
| `activityBar.foreground` / `sideBarSectionHeader.foreground` (`#D5D8DAB3` Dark) / `sideBar.foreground` (`#06060CB3` Light) | `--sidebar-foreground` | `#A0A2A7` | `#504C4F` | **Composta (opaca)** (`#D5D8DAB3` sobre `#1C1E26` = `#A0A2A7`, CR 6.49:1; `#06060CB3` sobre `#FDF0ED` = `#504C4F`, CR 7.61:1 — preserva o visual suave da sidebar no Horizon). |
| `activityBarBadge.background` | `--sidebar-primary` | `#E95378` | `#E84A72` | **Direta** |
| `activityBarBadge.foreground` | `--sidebar-primary-foreground` | `#06060C` | `#06060C` | **Direta** |
| `list.hoverBackground` / `list.activeSelectionBackground` (`ui.accent`) | `--sidebar-accent` | `#2E303E` | `#F9CBBE` | **Direta (opaca)** |
| `list.activeSelectionForeground` | `--sidebar-accent-foreground` | `#D5D8DA` | `#06060C` | **Direta** |
| `ui.accent` (Dark) / `editorGroup.border` (Light) | `--sidebar-border` | `#2E303E` | `#1A1C231A` | **Derivada da paleta** / **Direta** |
| `editorCursor.foreground` / `ui.secondaryAccentAlt` | `--sidebar-ring` | `#E95378` | `#E84A72` | **Direta** |
| `ui.backgroundAlt` (elevação de sidebar esquerda sem borda no Orca) | `--worktree-sidebar` | `#232530` | `#FADAD1` | **Derivada da paleta** (no Orca não há linha entre a sidebar esquerda e o canvas; usar `ui.backgroundAlt` cria o degrau visual autêntico da paleta Horizon). |
| `foreground` / `sideBarSectionHeader.foreground` | `--worktree-sidebar-foreground` | `#D5D8DA` | `#06060C` | **Direta** (garante alto contraste inclusive sobre linha ativa `#2E303E` / `#F9CBBE80`). |
| `list.hoverBackground` (`ui.accent`) | `--worktree-sidebar-accent` | `#2E303E` | `#F9CBBE` | **Direta (opaca)** |
| `list.hoverForeground` | `--worktree-sidebar-accent-foreground` | `#D5D8DA` | `#06060C` | **Direta** |
| `ui.accent` (Dark) / `editorGroup.border` (Light) | `--worktree-sidebar-border` | `#2E303E` | `#1A1C231A` | **Derivada da paleta** / **Direta** |
| `ui.secondaryAccentAlt` | `--worktree-sidebar-ring` | `#E95378` | `#E84A72` | **Direta** |
| `list.activeSelectionBackground` (`#2E303E80` Dark, `#F9CBBE80` Light) | `--worktree-sidebar-active` | `#2E303ECC` | `#F9CBBECC` | **Derivada da paleta** (mesmo `ui.accent` em 80% de opacidade para destacar claramente o workspace ativo sobre `ui.backgroundAlt`). |
| `ui.accentAlt` (Dark) / `ui.border` (Light) | `--worktree-sidebar-active-border` | `#6C6F9366` | `#1A1C2326` | **Derivada da paleta** (contorno sutil do card ativo) |
| `badge.background` (`ui.accent`) | `--host-badge` | `#2E303E` | `#F9CBBE` | **Direta** |
| `badge.foreground` / `sideBar.foreground` | `--host-badge-foreground` | `#A0A2A7` | `#504C4F` | **Composta (opaca)** (CR 5.24:1 sobre `#2E303E`; CR 5.75:1 sobre `#F9CBBE`) |
| `editorGroupHeader.tabsBackground`, `titleBar.activeBackground` | `--tab-bar` | `#1C1E26` | `#FDF0ED` | **Direta** |
| `ui.backgroundAlt` (Dark) / `editor.background` (Light) | `--tab-active` | `#232530` | `#FDF0ED` | Dark: **Derivada da paleta** (leva a aba ativa um degrau acima de `#1C1E26`). Light: **Direta** (mantém aba ativa clara unida ao editor). |
| `foreground` | `--tab-active-foreground` | `#D5D8DA` | `#06060C` | **Direta** |
| `tab.activeBorder`, `panelTitle.activeBorder` | `--tab-active-indicator` | `#E95378` | `#E84A72` | **Direta** (filete rosa icônico da aba ativa e da ferramenta ativa na sidebar direita) |
| `tab.inactiveBackground` (Dark) / `ui.backgroundAlt` (Light) | `--tab-inactive` | `#1C1E26` | `#FADAD1` | Dark: **Direta**. Light: **Derivada da paleta** (recessão suave em pêssego nas abas inativas, alinhada à convenção light do Orca). |
| `sideBar.foreground` (ajustada Dark / composta Light) | `--tab-inactive-foreground` | `#8D8F93` | `#504C4F` | **Ajuste L / Composta** (garante ≥ 4.5:1 sobre `--tab-inactive`) |
| `ui.accent` (Dark) / `ui.border` (Light) | `--split-divider` | `#2E303E` | `#1A1C2333` | **Derivada da paleta** |
| `ui.accentAlt` (Dark) / `ui.secondaryAccentAlt` (Light) | `--split-divider-hover` | `#6C6F93` | `#E84A72` | **Direta** |
| `editor.selectionBackground` | `--editor-selection` | `#2E303EB3` | `#F9CBBE80` | **Direta** (mantida translúcida para mostrar find matches; CR 9.75:1 Dark, 15.86:1 Light) |
| `textLink.foreground` | `--text-link` | `#E95378` | `#D61B4B` | Dark: **Direta** (CR 4.73:1). Light: **Ajuste L** (`#E84A72` tem CR 3.34 sobre `#FDF0ED` → ΔL -12.6% mantendo H=344.8°, CR 4.58:1). |
| `ui.darkText` (Dark) / `ui.background` (Light) | `--theme-accent-foreground` | `#06060C` | `#FDF0ED` | **Direta** (5.75:1 sobre `#E95378` Dark; 4.58:1 sobre `#D61B4B` Light) |
| `ui.backgroundAlt` | `--chat-user-message` | `#232530` | `#FADAD1` | **Derivada da paleta** (bolha de mensagem do usuário) |
| `foreground` | `--chat-bold-foreground` | `#D5D8DA` | `#06060C` | **Direta** |
| `sideBar.foreground` (ajustada Dark / composta Light) | `--chat-muted-foreground` | `#8D8F93` | `#504C4F` | **Ajuste L / Composta** |
| `ui.backgroundAlt` | `--chat-inline-code` | `#232530` | `#FADAD1` | **Derivada da paleta** |
| `markup.inline.raw` (`syntax.apricot` / `syntax.thunderbird`) | `--chat-inline-code-foreground` | `#F09483` | `#BC2B14` | Dark: **Direta** (CR 6.66:1 sobre `#232530`). Light: **Ajuste L** (`#DC3318` tem CR 3.54 sobre `#FADAD1` → ΔL -7.1% mantendo H=8°, CR 4.58 sobre `#FADAD1`). |
| `walkThrough.embeddedEditorBackground` (`ui.backgroundAlt`) | `--chat-code-block` | `#232530` | `#FADAD1` | **Direta** |
| `selection.background` (`#6C6F9380` Dark) / `editor.selectionBackground` (`#F9CBBE80` Light) | `--chat-selection` | `#6C6F9366` | `#F9CBBE99` | **Derivada da paleta** (translúcida, testada sobre todas as 5 superfícies de chat para manter ≥ 4.5:1) |
| `foreground` | `--chat-selection-foreground` | `#D5D8DA` | `#06060C` | **Direta** |
| `ui.positive` (base de `diffEditor.insertedTextBackground`) | `--diff-added` | `#09F7A0` | `#059963` | Dark: **Direta**. Light: **Ajuste L** (`#07DA8C` gera `--chat-diff-added` CR 2.58 sobre `--chat-code-block` → ΔL -13.1% mantendo H=158°, CR 4.59). |
| `diffEditor.insertedTextBackground` | `--diff-added-background` | `#09F7A01A` | `#07DA8C1A` | **Direta** |
| `ui.positive` (20% opacidade) | `--diff-added-emphasis` | `#09F7A033` | `#07DA8C26` | **Derivada da paleta** (destaque inline de caracteres adicionados) |
| `ui.positive` (30% opacidade) | `--diff-added-gutter` | `#09F7A04D` | `#07DA8C42` | **Derivada da paleta** (gutter de linhas adicionadas) |
| `ui.negative` (base de `diffEditor.removedTextBackground`) | `--diff-removed` | `#F44260` | `#F33555` | Dark: **Ajuste L** (`#F43E5C` gera `--chat-diff-removed` CR 4.497 sobre `--chat-canvas` → ΔL +0.9% mantendo H=350°, CR 4.58). Light: **Ajuste L** (`#F43E5C` gera CR 4.43 sobre `--chat-code-block` → ΔL -1.8%, CR 4.60). |
| `diffEditor.removedTextBackground` | `--diff-removed-background` | `#F43E5C1A` | `#F43E5C1A` | **Direta** |
| `ui.negative` (20% opacidade) | `--diff-removed-emphasis` | `#F43E5C33` | `#F43E5C26` | **Derivada da paleta** |
| `ui.negative` (32% opacidade) | `--diff-removed-gutter` | `#F43E5C52` | `#F43E5C38` | **Derivada da paleta** |
| `gitDecoration.addedResourceForeground` (`#27D797B3` / `#1EB980B3`) | `--git-decoration-added` | `#24A075` | `#349B73` | Dark: **Composta (opaca)** (CR 5.03:1). Light: **Composta + Ajuste L** (`#60C9A0` CR 1.81 → ΔL -17.7%, CR 3.10:1). |
| `gitDecoration.modifiedResourceForeground` | `--git-decoration-modified` | `#FAB38E` | `#AF5427` | **Direta** (9.42:1 / 4.58:1) |
| `gitDecoration.deletedResourceForeground` | `--git-decoration-deleted` | `#F43E5C` | `#F43E5C` | **Direta** (4.51:1 / 3.32:1) |
| `ui.modified` (`#21BFC2` / `#1EAEAE`) | `--git-decoration-renamed` | `#21BFC2` | `#1A9999` | Dark: **Direta** (CR 7.45:1). Light: **Ajuste L** (`#1EAEAE` CR 2.44 → ΔL -4.7% mantendo H=180°, CR 3.11:1). |
| `gitDecoration.untrackedResourceForeground` | `--git-decoration-untracked` | `#27D797` | `#199D6D` | Dark: **Direta** (8.91:1). Light: **Ajuste L** (`#1EB980` CR 2.27 → ΔL -6.3%, CR 3.10:1). |
| `ansi.normal.blue` (`#26BBD9`) / `syntax.elm` (`#1D8991`) | `--git-decoration-copied` | `#26BBD9` | `#1D8991` | **Derivada da paleta** (CR 7.47:1 Dark / 3.74:1 Light) |
| `gitDecoration.ignoredResourceForeground` (`#D5D8DA4D` / `#06060C4D`) | `--git-decoration-ignored` | `#686B72` | `#948787` | **Composta + Ajuste L** (Dark `#54565C` CR 2.27 → ΔL +8.2%, CR 3.12:1; Light `#B2A9A9` CR 2.06 → ΔL -12.6%, CR 3.11:1) |
| `ui.secondaryAccentAlt`, `ansi.normal.*`, `syntax.lavender`/`amethyst` | `--chart-1` .. `--chart-5` | `#E95378`, `#26BBD9`, `#29D398`, `#FAB795`, `#B877DB` | `#E84A72`, `#1D8991`, `#1EB980`, `#F6661E`, `#8A31B9` | **Derivada da paleta** (séries harmônicas do Horizon) |

---

## 2. Mapeamento Terminal (`terminalThemes`)

O Horizon define `terminal.foreground`, `terminal.selectionBackground`, `terminalCursor.*` e os 12 ANSI cromáticos (`red`, `green`, `yellow`, `blue`, `magenta`, `cyan` + `bright*`), mas **não** define `terminal.ansiBlack`, `terminal.ansiWhite`, `terminal.ansiBrightBlack` nem `terminal.ansiBrightWhite` no JSON compilado. Estes 4 slots foram derivados diretamente de `globals.json` (marcados como **Derivada da paleta**):

| Chave VS Code (Horizon) | Chave Orca `terminal` | Dark (`horizon-dark`) | Light (`horizon-bright`) | Decisão / Origem |
| --- | --- | --- | --- | --- |
| `editor.background` | `background` | `#1C1E26` | `#FDF0ED` | **Direta** |
| `terminal.foreground` | `foreground` | `#D5D8DA` | `#06060C` | **Direta** |
| `editorCursor.foreground` / `terminalCursor.foreground` | `cursor` | `#E95378` | `#E84A72` | **Derivada da paleta** (cursor opaco visível; `terminalCursor.foreground` no Horizon é translúcido `#6C6F9380`/`#F9CEC3B3`, incompatível com hex 6-dígitos de terminal) |
| `terminalCursor.background` | `cursorAccent` | `#1C1E26` | `#FDF0ED` | **Derivada da paleta** (texto legível sob o cursor bloco) |
| `terminal.selectionBackground` (composta opaca) | `selectionBackground` | `#2E303E` | `#F9CBBE` | **Derivada da paleta** (`ui.accent` opaco — schema de terminal do Orca aceita apenas `#RRGGBB`) |
| `terminal.foreground` | `selectionForeground` | `#D5D8DA` | `#06060C` | **Direta** |
| *(omitido no Horizon)* | `black` | `#16161C` | `#06060C` | **Derivada da paleta** (`ui.shadow` / `ui.darkText`) |
| `terminal.ansiRed` | `red` | `#E95678` | `#DA103F` | Dark: **Direta**. Light: **Derivada da paleta** (`syntax.crimson`, pois o Horizon Bright copiou os ANSI pastéis do Dark que ficam apagados em fundo claro `#FDF0ED`; mantemos ANSI legíveis da paleta Bright `globals.json` — ver nota em `NOTES.md`). |
| `terminal.ansiGreen` | `green` | `#29D398` | `#1EB980` | Dark: **Direta**. Light: **Derivada da paleta** (`ui.warning` Bright) |
| `terminal.ansiYellow` | `yellow` | `#FAB795` | `#F6661E` | Dark: **Direta**. Light: **Derivada da paleta** (`syntax.tango` Bright) |
| `terminal.ansiBlue` | `blue` | `#26BBD9` | `#1D8991` | Dark: **Direta**. Light: **Derivada da paleta** (`syntax.elm` Bright) |
| `terminal.ansiMagenta` | `magenta` | `#EE64AC` | `#8A31B9` | Dark: **Direta**. Light: **Derivada da paleta** (`syntax.amethyst` Bright) |
| `terminal.ansiCyan` | `cyan` | `#59E1E3` | `#1EAEAE` | Dark: **Direta**. Light: **Derivada da paleta** (`ui.modified` Bright) |
| *(omitido no Horizon)* | `white` | `#D5D8DA` | `#FADAD1` | **Derivada da paleta** (`ui.lightText` / `ui.backgroundAlt`) |
| *(omitido no Horizon)* | `brightBlack` | `#6C6F93` | `#504C4F` | **Derivada da paleta** (`ui.accentAlt` Dark / `sideBar.foreground` composta Light) |
| `terminal.ansiBrightRed` | `brightRed` | `#EC6A88` | `#E84A72` | Dark: **Direta**. Light: **Derivada da paleta** (`ui.secondaryAccentAlt` Bright) |
| `terminal.ansiBrightGreen` | `brightGreen` | `#3FDAA4` | `#07DA8C` | Dark: **Direta**. Light: **Derivada da paleta** (`ui.positive` Bright) |
| `terminal.ansiBrightYellow` | `brightYellow` | `#FBC3A7` | `#F77D26` | Dark: **Direta**. Light: **Derivada da paleta** (`syntax.jaffa` Bright) |
| `terminal.ansiBrightBlue` | `brightBlue` | `#3FC4DE` | `#26BBD9` | Dark: **Direta**. Light: **Direta** (`ansi.normal.blue`) |
| `terminal.ansiBrightMagenta` | `brightMagenta` | `#F075B5` | `#E73665` | Dark: **Direta**. Light: **Derivada da paleta** (`ui.secondaryAccent` Bright) |
| `terminal.ansiBrightCyan` | `brightCyan` | `#6BE4E6` | `#25B0BC` | Dark: **Direta**. Light: **Derivada da paleta** (`syntax.turquoise`) |
| *(omitido no Horizon)* | `brightWhite` | `#FDF0ED` | `#333333` | **Derivada da paleta** (`ui.background` Bright / `syntax.black` Bright) |

---

## 3. Notas de `tokenColors` (Sintaxe)

- Todos os 39 blocos originais de `themes/horizon.json` e `themes/horizon-bright.json` são preservados com os mesmos nomes de `scope`.
- **Adaptações estruturais obrigatórias para o Orca / Monaco**:
  1. `"fontStyle": "normal"` → omitido (o schema do Orca aceita apenas `italic`, `bold`, `underline`, `strikethrough` ou `""`).
  2. Seletor descendente `"string.template meta.embedded"` → simplificado para `"meta.embedded"` (Orca ignora seletores com espaço).
  3. Escopos raiz exigidos pelo mapeamento Monaco do Orca (`MONACO_TOKEN_SCOPES`): adicionados `"string"` junto a `["string.quoted", "string.template"]`, `"punctuation"` junto a `"punctuation.separator"`, além de `"meta.decorator"` e `"invalid"` com cores da própria paleta do Horizon.
  4. Ajustes mínimos de luminosidade (mantendo o matiz) apenas nos tokens abaixo de 3.0:1 contra o fundo do editor (`comment` e `punctuation.definition.tag` no Dark; `comment`, `entity.name`/`entity.name.type`/`support`/`variable.language`, `string`/`entity.other.inherited-class`/pseudo/links e `markup.quote` no Light). Todos os deltas exatos são registrados em `NOTES.md`.
