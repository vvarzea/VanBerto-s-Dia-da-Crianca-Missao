# Estrutura de ficheiros — VanBerto's

O jogo passou a estar dividido em vários ficheiros para facilitar a manutenção,
usando módulos ES nativos do browser (sem build step — funciona tal e qual
em GitHub Pages).

## Ficheiros

- **`index.html`** — carrega `dia-crianca.js` com `type="module"`.
- **`dia-crianca.js`** — motor do jogo (Phaser, física, níveis, UI, save/load).
  Importa os dados dos 4 ficheiros abaixo.
- **`data-quiz.js`** — conteúdo educativo e perguntas:
  `HISTORY`, `QUIZ_TIPS`, `QUIZ_ARTICLE`, `QUIZ_BY_THEME`.
  *(Editar aqui quando quiseres mudar/acrescentar perguntas ou curiosidades.)*
- **`data-levels.js`** — definição dos níveis e paletas visuais:
  `THEMES`, `LEVELS`.
  *(Editar aqui para criar ou ajustar um nível.)*
- **`data-progression.js`** — sistemas de progressão:
  `MAP_REGIONS`, `ARTEFACTS`, `ARTEFACT_SETS`, `SET_REACTIONS`, `ACHIEVEMENTS_DEFS`.
  *(Editar aqui para mexer no mapa, álbum de artefactos ou conquistas — útil para a Fase 2/3.)*
- **`data-flavor.js`** — frases soltas sem estrutura de jogo:
  `PRAISE`, `PAUSE_TIPS`, `LEVEL_ENTRY_PHRASES`, `DYNAMIC_MSGS_CORRECT`, `DYNAMIC_MSGS_WRONG`.
- **`dia-crianca.css`** — estilos (sem alterações).
- **`vanberto_voar.png`** — imagem do mascote (sem alterações).

## Porquê módulos ES e não um script de build?

Publicas via GitHub Pages, que serve ficheiros estáticos diretamente — não há
nenhum passo de build no teu fluxo atual. Os módulos ES (`import`/`export`)
funcionam nativamente no browser sem precisar de nenhuma ferramenta extra:
publicas os ficheiros tal como estão e funciona.

## Como adicionar um novo ficheiro de dados no futuro

1. Cria `data-X.js` com `export const ALGO = [...]`.
2. No topo de `dia-crianca.js`, acrescenta:
   `import { ALGO } from "./data-X.js";`
3. Usa `ALGO` normalmente no resto do código — nada mais muda.

## Nota técnica importante

`dia-crianca.js` continua a ser uma única função grande (a mesma
`window.addEventListener("DOMContentLoaded", ...)` de sempre) — só os
**blocos de dados puros** (arrays/objetos sem lógica) foram extraídos.
A física, o Phaser, os event handlers e toda a lógica do jogo continuam
no mesmo sítio, exatamente como estavam. Isto foi deliberado: são a parte
mais interligada e arriscada de separar, por isso ficou para uma fase
futura, se um dia fizer sentido.

## Verificação feita

- Sintaxe válida em todos os 5 ficheiros JS (`node -c`).
- `dia-crianca.js` valida como módulo ES (`node --input-type=module -c`).
- Os imports resolvem corretamente (testado com Node em modo módulo).
- Comparação linha-a-linha confirmou: **zero conteúdo perdido, zero
  conteúdo duplicado** entre a versão original e a nova estrutura.
