# Horizon Theme for Orca

Porte de alta fidelidade do [Horizon Theme](https://github.com/alexandernanberg/horizon-theme-vscode) (Dark e Bright) para o [Orca](https://github.com/stablyai/orca), incluindo temas de app, sintaxe (`tokenColors`) e paletas de terminal.

## Instalação no Orca

```sh
orca plugin install https://github.com/flpcovre/horizon-theme
```

Depois de instalar, selecione **Horizon** (Dark) e **Horizon Bright** (Light) em **Settings → Appearance**.  
Para ver todas as cores do tema sem sobrescritas, mantenha **Left Sidebar Appearance** em **Default**.

## Temas incluídos

- **App themes**: `Horizon` (Dark) e `Horizon Bright` (Light)
- **Terminal themes**: `Horizon` (Dark) e `Horizon Bright` (Light)

## Desenvolvimento local

```sh
orca plugin validate .
orca plugin inspect .
orca plugin dev load .
```

## Documentação

- [MAPPING.md](MAPPING.md) — tabela completa VS Code → Orca com origem de cada cor.
- [NOTES.md](NOTES.md) — ajustes de contraste (com deltas exatos), tratamento de seletores de sintaxe e decisões técnicas.
