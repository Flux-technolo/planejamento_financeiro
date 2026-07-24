# NOW Labs — Design System (código)

Fonte de verdade *em prosa* das decisões (paleta, tipografia, benchmark, rationale): [`../docs/15-Sistema-de-Design.md`](../docs/15-Sistema-de-Design.md).

Este diretório é a fonte de verdade *em código*, sincronizada com o projeto "NOW Labs Design System" no Claude Design via `/design-sync`.

- `tokens.css` — cores (claro/escuro) e famílias tipográficas como custom properties. Nada de cor/fonte deve ser hardcoded em componentes; sempre referenciar um token daqui.
- `fonts/` — arquivos IBM Plex Serif/Sans/Mono (subset latin) usados pelos tokens.
- `components/*.html` — um preview por componente, cada um começando com um marcador `<!-- @dsCard group="..." -->` que o Claude Design usa para montar o card no painel.

Ao evoluir qualquer token ou componente, atualizar aqui primeiro e depois rodar `/design-sync` de novo para republicar.
