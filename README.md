# Marcus-Mori Context Repository

Persistent context storage for Marcus Tull's Mori (Morwen) assistant sessions.

## Purpose

This repo serves as the durable context layer while the Supabase KB writer is unavailable on the desktop surface. Files here mirror the exact pillar structure used by the Kindo/Supabase context system, so they transfer directly once the connection is restored.

## Structure

```
pillars/
  context_core.md       # Identity, voice, behavioral rules
  context_index.md      # Rolling session log, priorities, active state
  context_domain.md     # Domain knowledge, product architecture, technical reference
  operating_model.md    # SOPs, constraints, working rules
  relationship.md       # Who Marcus is, preferences, key people
  self_observations.md  # Reaction-based functional observations
kb/
  failure_log.md        # SOPs, incidents, FMEA table
  chronicle.md          # Running factual record (book material)
```

## Usage

- Mori writes updated pillar files here at session end (context push)
- Mori reads them at session start (context load)
- Once Supabase KB writer is reconnected, content migrates there and this repo becomes the backup/archive

## Migration Path

Each file maps 1:1 to a Supabase KB entry or OneDrive pillar file:
- `pillars/*` → Supabase pillar state service (update_pillar/read_pillar)
- `kb/*` → Supabase KB entries (read_kb_entry/write_kb_entry)
