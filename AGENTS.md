# AGENTS.md

## Generated project

- Identity: `name=vue-calendar-day-week-month-open-source`; `component=calendar`; `edition=lite`; `target=vue3`.
- Public Builder generation creates a fresh baseline; it does not patch or merge this working tree.

## DayPilot sources

- Installed API shape: `node_modules/@daypilot/daypilot-lite-vue/daypilot-vue.min.d.ts`
  - Available after dependency install (`npm install`); this generated starter intentionally has no lockfile.
- Builder settings for this exact target: <https://builder.daypilot.org/api/v1/components/calendar/settings?edition=lite&target=vue3>
- Builder service entry point: <https://builder.daypilot.org/api/v1>. Read the current catalog revision there and follow its linked OpenAPI and Arazzo contracts for automation.
- Builder discovery covers only Builder-configurable capabilities; an absent setting does not mean the DayPilot product lacks that API.
- For APIs outside Builder's subset, check the installed typings above and the [official API reference](https://api.daypilot.org/).


## Edition boundary

- This is a Lite project. Do not silently add Pro packages or Pro-only APIs; propose an explicit edition change or a Lite-safe alternative.

## Project commands

- Install: `npm ci` (uses the included package-lock.json).
- Development: `npm run dev`.
- Build: `npm run build`.
- Lint: no `lint` script is provided.
- Test: this starter does not provide an executable test suite (no `test` script).
