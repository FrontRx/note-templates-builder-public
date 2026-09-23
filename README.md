# FrontRx Note Templates Builder Files

Data files read by the physician app's note template builder (`webapp-physician-next`, `PUBLIC_API_TEMPLATE_BUILDER_URL`).

- `builder-headers.json`: the header library, one entry per section a physician can add.
- `builder-templates.json`: ready-made templates grouped by specialty, each a list of sections in the same shape.

## Section fields

| field | meaning |
|---|---|
| `en`, `fr` | Section name in each language, Title Case. The builder upper-cases it when writing the heading. |
| `abbr_en`, `abbr_fr` | Clinical shorthand in each language (`HPI` / `HMA`, `PE` / `E/P`, `Meds` / `Rx`). Empty when the section has none. Casing is meaningful and must not be normalized. The builder offers these as the abbreviation options, so a shorthand added here shows up in the app without a code change. |
| `format` | Output format for the section (`BULLET POINTS`, `TELEGRAPHIC LINES`, `PROSE`, `NUMBERED LIST`), empty for the default. |
| `condition` | `ONLY IF ...` gate, empty when the section is always written. |
| `instruction_en`, `instruction_fr` | Instruction to the scribe for that section. |
| `example` | Example content shown in the preview. |
| `category` (headers) / `group` (template sections) | Grouping in the library. |

A template section that reuses a library header carries the same `abbr_en` / `abbr_fr` as the header, so a template shows the same shorthand as the library entry.

## Markdown the builder writes

Each section becomes one `###` heading followed by its instruction and example:

```
### HISTORY OF PRESENT ILLNESS <FR: Anamnèse> <ABBR: HPI> <FR-ABBR: HMA> <ONLY IF MENTIONED> <FORMAT: PROSE>
<instruction>
example
```

- Every tag is optional and is omitted when the field is empty.
- In the abbreviated header style, the heading and the `<FR: ...>` replacement carry the shorthand instead of the name (`### HPI <FR: HMA> <ABBR: HPI> <FR-ABBR: HMA>`). The `ABBR` / `FR-ABBR` tags are written in both styles; they are what recovers the full names when the template is reopened.
- A template-level instruction is written as a single `<...>` line before the first `###` heading.
