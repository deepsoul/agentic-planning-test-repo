# Agentic Planning — Test Target Repo

Minimales **Ziel-Repository** für den End-to-End-Test des [Agentic Sprint Planning Boards](https://agentic-sprint-planning.vercel.app).

Hier landen Agent-Branches und PRs. GitHub-Webhooks synchronisieren PR- und Deploy-Status zurück auf die Ticket-Karten im Dashboard.

## Schnellstart (Pitch-Demo)

### 1. Vercel verbinden (einmalig)

1. [Vercel Dashboard](https://vercel.com/new) → **Import Git Repository**
2. Repo `deepsoul/agentic-planning-test-repo` auswählen
3. Framework: **Other** (statische `index.html`, kein Build nötig)
4. Deploy — fertig. Jeder Push/PR erzeugt eine **Preview-URL**.

> Vercel meldet Deployments an GitHub zurück → das Dashboard zeigt **PREVIEW** auf der Ticket-Karte.

### 2. MCP im Cursor (dieses Repo öffnen)

```bash
cp .cursor/mcp.json.example .cursor/mcp.json
```

In `.cursor/mcp.json`:

- `DASHBOARD_URL`: `https://agentic-sprint-planning.vercel.app`
- `MCP_API_TOKEN`: derselbe Wert wie im Dashboard (Vercel Env / `.env`)
- `args`-Pfad: absoluter Pfad zu `mein-agenten-board/mcp-server/src/index.ts`  
  _(oder `npx tsx` aus einem geklonten Dashboard-Repo)_

Cursor: **Reload Window**.

### 3. Dashboard konfigurieren

Im [Admin Control Deck](https://agentic-sprint-planning.vercel.app/admin):

| Feld | Wert |
|------|------|
| GitHub-Ziel-Repo | `deepsoul/agentic-planning-test-repo` |

Vercel Env (Dashboard-Projekt):

| Variable | Wert |
|----------|------|
| `GITHUB_WEBHOOK_SECRET` | Secret aus GitHub-Webhook |
| `GITHUB_DEFAULT_REPO` | `deepsoul/agentic-planning-test-repo` |

### 4. GitHub-Webhook (bereits angelegt?)

Repo → **Settings** → **Webhooks**:

- URL: `https://agentic-sprint-planning.vercel.app/api/webhooks/github`
- Events: **Pull requests**, **Deployments**
- Secret = `GITHUB_WEBHOOK_SECRET`

### 5. Demo durchspielen

1. Ticket **AGT-xxx** im Board agent-ready machen und in den Sprint ziehen.
2. In Cursor (dieses Repo): `/agentic-planning AGT-xxx`
3. Agent erstellt Branch `feature/agt-xxx-…`, ändert z. B. diese `index.html`, öffnet Draft-PR.
4. Im Board: Pipeline-Badge **PR DRAFT/OPEN**.
5. Vercel baut Preview → Badge **PREVIEW** (klickbar).
6. PR mergen → optional **PROD** nach Production-Deploy.

## Demo-Ticket AGT-101

Fertige DoR-Texte und Pitch-Skript liegen im Dashboard-Repo unter `docs/pitch-demo-AGT-101.md`.

Kurzbefehl in Cursor (dieses Repo geöffnet):

```text
/agentic-planning AGT-101
```

Der Agent ändert `index.html` → h1 **„Agentic Planning — Live Demo"** → Draft-PR → Pipeline-Badge erscheint im Board.

## Branch-Konvention

```
feature/{ticket-id-klein}-{slug}
```

Beispiel: Ticket `AGT-101` → `feature/agt-101-hello-pipeline-demo`

## Später: Firmen-Repo

Nur Konfiguration umziehen (kein Code-Change im Board):

1. Admin: `githubRepo` → z. B. `netfonds/vue-finfire`
2. Webhook im Firmen-Repo neu anlegen (gleiche URL + Secret)
3. MCP weiterhin im jeweiligen Ziel-Repo nutzen

## Dateien

| Datei | Zweck |
|-------|--------|
| `index.html` | Minimale Demo-Seite (Agent-Änderungsziel) |
| `vercel.json` | Statisches Deployment ohne Build |
| `.cursor/mcp.json.example` | MCP-Anbindung ans Dashboard |
