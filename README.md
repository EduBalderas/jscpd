# jscpd

## META
- id: jscpd
- owner: EduBalderas
- visibility: public
- idioma: mixto
- estado: estable
- tipo: fork
- nivel: L4
  - L0=gobernanza/política
  - L1=marca/metodología
  - L2=flujo/arquitectura
  - L3=producto ejecutable
  - L4=herramienta
- familia: ninguna
- origen: upstream:https://github.com/kucherenko/jscpd
- experimento: no
- nacido_de: kucherenko/jscpd
- orquestado_por: N/A
- brief_desde: N/A

## OBJETIVO
Detectar código duplicado en código fuente (224 formatos) con un motor Rust y un binario autocontenido, e informar el resultado (HTML, JSON, SARIF, Markdown) para poder fallar CI por encima de un umbral.

## QUE_CONTIENE
- `rust/`: workspace Cargo (edition 2024, `rust-version` 1.97) con los crates `cpd` (binarios `jscpd` y `cpd`, versión 5.4.1), `cpd-core`, `cpd-tokenizer`, `cpd-similarity`, `cpd-finder`, `cpd-reporter`, `cpd-semantic` y `basta`. Todos tienen `publish = false` en este árbol. El empaquetado npm está en `rust/package.json` (`cpd`) y `rust/jscpd/package.json` (`jscpd`), más paquetes de binario por plataforma en `rust/npm/`.
- `FORMATS.md`: 224 formatos generados desde `rust/crates/cpd-tokenizer/src/formats.rs` (203 con asociación de fichero, 21 sin ella). `action.yml` describe el Action como «223+ languages».
- `docs/`: `rust.md`, `ai-ready.md`, `editors.md`, `api.md`, `ci-and-hooks.md`, `packages.md`, `basta-extending.md`.
- `fixtures/`: corpus y demos por función. También `examples/`, `benchmark/` (`benchmark/BENCHMARK.md`, `benchmark/benchmark.sh`) y `skills/` (`jscpd`, `dry-refactoring`, `compare-codebases`, `code-migration`, `codebase-refactoring`).
- `action.yml`: GitHub Action `jscpd-copy-paste-detector`. Distribución en el árbol: `Dockerfile`, `flake.nix` / `flake.lock`, `.pre-commit-hooks.yaml` y `pyproject.toml` (proyecto local `jscpd-repo` 5.4.1, dependencia `jscpd==5.4.1`, no es el paquete publicado).
- `README-v4.md`: página del motor TypeScript v4. `CITATION.cff`: software «jscpd: copy/paste detector for programming source code», versión 5.4.1, `date-released` 2026-10-09, autor Andrey Kucherenko. `LICENSE`: MIT, copyright 2013-2024 Andrey Kucherenko.

## CARACTERISTICAS
- Tokenización con las reglas de comentario y cadena de cada formato, y búsqueda de secuencias de tokens duplicadas con hash rodante Rabin-Karp (`CITATION.cff`, `docs/rust.md`).
- Clones exactos en la ejecución por defecto. Type-2 con `--ignore-identifiers`, `--ignore-literals` y `--ignore-annotations`. Type-3 con `--max-gap-lines` y `--similarity`. Type-4 semántico, marcado experimental, con `--semantic` (`rust/crates/cpd-semantic`).
- 16 reporters en `REPORTER_NAMES` (`rust/crates/cpd-reporter/src/reporter.rs`): `console`, `console-full`, `json`, `sarif`, `codeclimate`, `ai`, `xml`, `csv`, `edn`, `html`, `markdown`, `badge`, `openmetrics`, `xcode`, `threshold`, `silent`.
- Código muerto en el crate `basta`: ficheros, exports, símbolos e imports sin uso en JavaScript, TypeScript, Vue, Svelte, Astro y Python, y Rust vía diagnósticos de cargo. El mismo motor respalda `jscpd --dead-code`.
- Otras pasadas documentadas en `docs/rust.md`: `--complexity`, `--summary`, `--history`, `--health`, `--dashboard`, baseline de clones y ficheros cambiados según git.
- Servidor MCP (`--mcp`), language server (`--lsp`) y reporter `ai`.
- Binarios precompilados declarados como `optionalDependencies` 5.4.1: `jscpd-darwin-arm64`, `jscpd-darwin-x64`, `jscpd-linux-x64-gnu`, `jscpd-linux-arm64-gnu`, `jscpd-linux-x64-musl`, `jscpd-linux-arm64-musl`, `jscpd-windows-x64-msvc`, `jscpd-windows-arm64-msvc`.
- Descripción npm (`rust/jscpd/package.json`): copy/paste detector, 224 lenguajes, informes HTML/JSON/SARIF/Markdown, fallo de CI por umbral, motor Rust autocontenido. Homepage `https://jscpd.dev`. Descripción de este fork en GitHub: «Detector de código duplicado en archivos de código fuente.»

## FUERA_DE_ALCANCE
- El motor v4 (TypeScript, API de Node.js, almacenes LevelDB/Redis) no es este árbol: `README-v4.md` lo sitúa en la rama `master-v4` del upstream y en el dist-tag `jscpd@4` / `latest-4`.
- Clientes de editor para VS Code y Zed: `docs/editors.md` dice que ambos arrancan language servers solo desde extensiones y que esos clientes irán en un repositorio propio. En este árbol el LSP documentado es Neovim, Helix, Sublime Text, Emacs y JetBrains.
- `basta` no es un detector de código muerto para los 224 formatos; el alcance está en la descripción de `rust/crates/basta/Cargo.toml`.
- Este fork no es el repositorio de publicación: `repository` en los `package.json`, en `rust/Cargo.toml` y en `CITATION.cff` apunta a `https://github.com/kucherenko/jscpd`.
- `--semantic` y `--compare` figuran como experimentales en la documentación de este árbol.

## RELACIONES
- Parent del fork: https://github.com/kucherenko/jscpd (Andrey Kucherenko). Este repo: https://github.com/EduBalderas/jscpd, creado 2026-10-13, rama por defecto `master`, homepage `https://jscpd.dev`.
- A 2026-10-26 el fork va 0 commits por delante y 107 por detrás de `kucherenko/jscpd` `master` (compare API de GitHub). No tiene topics propios.
- npm declarado en el árbol: paquete `jscpd` (`rust/jscpd`) y paquete `cpd` (`rust/`), versión 5.4.1, `repository` `git+https://github.com/kucherenko/jscpd.git`.
- npm, crates.io, Homebrew, Nix y Docker (`ghcr.io/kucherenko/jscpd`) están descritos en `docs/packages.md`. PyPI (`pip install jscpd`) está en `docs/rust.md`. El Action de ejemplo es `kucherenko/jscpd@v5` (`docs/ci-and-hooks.md`). Los crates del workspace tienen `publish = false`.
- Financiación declarada en los `package.json`: https://opencollective.com/jscpd.
- Documentación: https://jscpd.dev. Cita: `CITATION.cff`.

## ETIQUETAS
- jscpd
- cpd
- copy-paste-detector
- duplicate
- duplicate-code
- code-duplication
- duplication
- code-quality
- static-analysis
- dry
- sarif
- ci
- cli
- rust
- code-clone-detection
- copy-paste-detection
- command-line-utilities
- development-tools

## TOPICS_GITHUB
Este fork no tiene topics (`topics: []`). Topics del upstream `kucherenko/jscpd`:
- copy-paste
- duplicates
- cpd
- code-quality
- clones-detection
- ai
- coding-agents
- skills
- mcp
- dry
- copy-paste-detection
- cli
- code-duplication
- developer-tools
- github-action
- linter
- rust
- sarif
- static-analysis
- typescript
