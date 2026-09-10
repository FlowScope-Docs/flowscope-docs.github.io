# FlowScope Coworker Guidebook

<p class="intro-lead">Internal guide for BRI coworkers using FlowScope — ETL lineage and CML job monitoring during incident response.</p>

**Audience:** Viewer, Editor, Admin  
**Language:** English (Indonesian translation later)  
**Site:** [https://flowscope-docs.github.io/](https://flowscope-docs.github.io/)

FlowScope helps you answer what feeds what, who owns a job, when a run was expected, and what happened on the latest execution. It does **not** start or schedule CML jobs for you.

---

## Start here by role

<div class="role-cards">
  <div class="role-card">
    <span class="role-tag">Viewer</span>
    <h3>Read Monitor &amp; lineage</h3>
    <p>Use Monitor, Chart, and Table. No edit, History, acknowledge, or Admin.</p>
    <ol>
      <li><a href="#/00-what-is-flowscope">What is FlowScope</a></li>
      <li><a href="#/01-getting-started">Getting started</a></li>
      <li><a href="#/02-roles-and-permissions">Roles</a></li>
      <li><a href="#/03-monitor">Monitor</a></li>
      <li><a href="#/04-chart-and-table">Chart and Table</a></li>
      <li><a href="#/13-faq-and-troubleshooting">FAQ</a></li>
    </ol>
    <a class="start-link" href="#/00-what-is-flowscope">Begin →</a>
  </div>
  <div class="role-card">
    <span class="role-tag">Editor</span>
    <h3>Author jobs &amp; lineage</h3>
    <p>Everything a Viewer can do, plus draft/save, History, acknowledge, and CML Telegram instrumentation.</p>
    <ol>
      <li>Viewer path first</li>
      <li><a href="#/05-editor-draft-and-history">Draft and History</a></li>
      <li><a href="#/06-editor-node-forms">Node forms</a> → <a href="#/07-editor-edges-and-lineage">Edges</a></li>
      <li><a href="#/09-editor-job-monitoring">Job monitoring</a> + <a href="#/incorporate-flow-report">Flow Report</a></li>
      <li><a href="#/10-editor-acknowledge">Acknowledge</a></li>
    </ol>
    <a class="start-link" href="#/00-what-is-flowscope">Begin →</a>
  </div>
  <div class="role-card">
    <span class="role-tag">Admin / Superadmin</span>
    <h3>Manage access</h3>
    <p>Everything an Editor can do, plus invite users, approve requests, and set roles.</p>
    <ol>
      <li>Editor path first</li>
      <li><a href="#/11-admin">Admin</a></li>
    </ol>
    <a class="start-link" href="#/11-admin">Go to Admin →</a>
  </div>
</div>

---

## Quick jumps

<div class="quick-jumps">
  <a href="#/03-monitor">Job failed or late → Monitor</a>
  <a href="#/09-editor-job-monitoring">Instrument a CML job</a>
  <a href="#/incorporate-flow-report">Incorporate Flow Report</a>
  <a href="#/13-faq-and-troubleshooting">Something looks wrong → FAQ</a>
  <a href="#/reference">Glossary &amp; deep links</a>
</div>

---

## More reference

- [Reference cheat sheet](reference.md) — chapter index, role map, glossary, deep-link params, timezone
- [Standards and use cases](12-standards-and-use-cases.md)
- [assets/](assets/README.md) — screenshot filenames (most still pending)

English is the source of truth. Screenshots are placeholders until PNGs are dropped into [`assets/`](assets/README.md).
