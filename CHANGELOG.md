# Changelog

All notable changes to Cardinal Network Analysis are documented here.

This project has no formal version tags or GitHub releases. Changes are tracked by commit on the `main` branch.

**Repository**: <https://github.com/Dicklesworthstone/cardinal_network_analysis>

---

## 2026-02-21 -- Licensing and Repository Presentation

### License

Added an MIT license with an OpenAI/Anthropic Rider (73 lines). The rider restricts all use by OpenAI, Anthropic, their affiliates, and anyone acting on their behalf, unless Jeffrey Emanuel grants express prior written permission. The rider takes precedence over the base MIT terms in any conflict.

- [`c763d53`](https://github.com/Dicklesworthstone/cardinal_network_analysis/commit/c763d53fa9ccb807e1d31909fe919d6ec4109952) -- chore: update license to MIT with OpenAI/Anthropic Rider

### Social Preview

Added a 1280x640 PNG social preview image (`gh_og_share_image.png`) for consistent Open Graph previews when the repository URL is shared on social media or chat platforms.

- [`6e500f4`](https://github.com/Dicklesworthstone/cardinal_network_analysis/commit/6e500f43cac042e6e3f16ec6e24bc4c67cae9d5f) -- chore: add GitHub social preview image (1280x640)

---

## 2025-05-09 -- Initial Creation

The entire application was created in a single session across four commits. There is no build step and no `package.json`; the application runs entirely in-browser.

### Network Simulation (Python via Pyodide)

A complete ecclesiastical network is generated at runtime inside the browser using Pyodide v0.24.0 and NetworkX. The simulation is deterministic (`random.seed(42)`) and produces a graph of 60 fictional cardinals.

**Cardinal attributes:**
- Name (randomly composed Italian first/last names)
- Ideology -- one of five categories: Liberal, Soft Liberal, Moderate, Soft Conservative, Conservative (plus a Non-voting category defined but not assigned during generation)
- Age (uniform random, 55--85)
- Country (sampled from 10 nations: Italy, France, Spain, Germany, USA, Brazil, Argentina, Nigeria, Philippines, Poland)
- The future Pope is biased toward Moderate ideology during generation

**Three relationship layers** mirror the Rizzo/Soda/Iorio methodology:
1. Official co-memberships -- 15 committees of 3--8 members each; all pairwise edges within each committee carry weight 1 (additive if the edge already exists)
2. Lines of episcopal consecration -- each cardinal has 0--2 consecrators; these edges carry weight 2 (additive)
3. Informal relationships -- 70% probability of connection between cardinals sharing a country, plus `2 * num_cardinals` additional random edges at weight 1

Weighted edges accumulate across all three layers when the same pair appears in multiple relationship types.

### Network Metrics

Five centrality metrics are computed on the generated graph, entirely in Python:

| Metric | Algorithm | Role in analysis |
|---|---|---|
| Eigenvector centrality | `nx.eigenvector_centrality(G, weight='weight')` | Primary predictor of papal election ("status") |
| Betweenness centrality | `nx.betweenness_centrality(G, weight='weight')` | Information brokerage / bridging factions |
| Degree centrality | `nx.degree_centrality(G)` | Raw connectivity count |
| Coalition building index | 40% eigenvector + 40% betweenness + 20% degree | Composite political capability score |
| Age-adjusted index | Coalition index multiplied by `1 - 0.2 * (age - min_age) / age_range` | Composite score with modest penalty for older cardinals |

Top-10 rankings are computed for eigenvector, betweenness, coalition, and age-adjusted metrics. The cardinal with the highest eigenvector centrality is automatically designated as the elected Pope.

### Interactive Visualization (D3.js)

A force-directed graph renders the full network in an SVG element:

- **Node sizing** -- radius is `5 + 20 * eigenvector_centrality`, making influential cardinals visually larger
- **Node coloring** -- ideology mapped to a blue-gray-red spectrum (blue = Liberal, gray = Moderate, red = Conservative)
- **Pope highlight** -- the elected Pope node receives a gold stroke (width 3) instead of the default white stroke
- **Edge width** -- proportional to `sqrt(weight) * 0.5`
- **Zoom and pan** -- full `d3.zoom` support with scale extent 0.1x to 8x
- **Drag interaction** -- nodes can be repositioned; the simulation restarts with `alphaTarget(0.3)` during drag and settles on release
- **Click-to-select** -- clicking a node opens a detailed profile panel; clicking empty space clears the selection
- **Force parameters** -- link distance 80, link strength 0.5, charge strength -300, collision radius `node.size * 1.5`

### User Interface (React + Tailwind CSS)

The single-file React component (`index.html`, 560 lines) provides a two-panel layout:

- **Left panel (2/3 width)** -- the SVG graph visualization
- **Right panel (1/3 width, scrollable)** -- contextual information:
  - When no node is selected: ideology legend with color swatches, top-10 rankings for eigenvector and betweenness centrality, and a methodology summary
  - When a node is selected: full name, ideology badge with color indicator, age, country, and all four metric scores (eigenvector, betweenness, coalition, age-adjusted) to four decimal places
- **Loading state** -- displayed while Pyodide initializes and NetworkX installs via `loadPackagesFromImports`

### External Dependencies (CDN, no build tooling)

| Dependency | Version | Purpose |
|---|---|---|
| Pyodide | v0.24.0 | In-browser Python runtime |
| NetworkX | (installed at Pyodide runtime) | Graph construction and centrality algorithms |
| D3.js | (ES module import) | Force-directed graph rendering |
| React | (ES module import) | UI component framework |

### Documentation

- Expanded the placeholder README (2 lines) into a comprehensive 130-line document covering the project overview, the Rizzo/Soda/Iorio methodology, mathematical definitions for all four metric families (eigenvector, betweenness, coalition, age-adjusted), visualization semantics, implications for papal elections, and technical stack notes.
  - [`735578f`](https://github.com/Dicklesworthstone/cardinal_network_analysis/commit/735578f0d47c9d27ce2b90b3e4169cff719d1002) -- expand README with full methodology and metric definitions
- Fixed the repository link in README, removing the trailing `/tree/main` path.
  - [`98fbee4`](https://github.com/Dicklesworthstone/cardinal_network_analysis/commit/98fbee4b08a41262dd3daca44714db7d8a039d05) -- fix repository URL in README

### Commits (chronological)

| Commit | Description |
|---|---|
| [`8548549`](https://github.com/Dicklesworthstone/cardinal_network_analysis/commit/8548549ddcbcd1e38b9a0da6d9976a7c6a5e847b) | Initial commit -- placeholder README |
| [`014346e`](https://github.com/Dicklesworthstone/cardinal_network_analysis/commit/014346edcc1e935db8310025a0231051cea94497) | Create index.html -- full application (560 lines) |
| [`735578f`](https://github.com/Dicklesworthstone/cardinal_network_analysis/commit/735578f0d47c9d27ce2b90b3e4169cff719d1002) | Expand README with methodology and metrics |
| [`98fbee4`](https://github.com/Dicklesworthstone/cardinal_network_analysis/commit/98fbee4b08a41262dd3daca44714db7d8a039d05) | Fix repository URL in README |
