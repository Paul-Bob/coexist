# coexist

An agent skill that writes PR descriptions, GitHub issues and Linear issues for the two readers who now share them. People get a short part on top. Agents get their context in a section folded underneath, titled **For AI agents**.

Descriptions written by agents tend to be long mechanism write-ups. The reviewer has to dig for the outcome, and the next agent gets paragraphs where it needs paths, commands and the reasons behind a choice. `coexist` splits the two:

```markdown
CSV exports no longer drop rows after a date filter is applied. The export
read the filtered page size instead of the full result count.

A new request spec exports 1,200 filtered rows and gets all of them.

Fixes #482

<details>
<summary>For AI agents</summary>

**Where**

- `app/services/csv_export.rb:41`: now counts the unpaginated scope.

**Verify**

- `bin/rspec spec/requests/csv_export_spec.rb`

</details>
```

It works for new descriptions and for rewriting existing ones. A rewrite keeps every fact from the old body, moving each into one of the two parts.

## Install

```bash
npx skills add Paul-Bob/coexist
```

Or copy it by hand:

```bash
git clone https://github.com/Paul-Bob/coexist
cp -r coexist/skills/coexist ~/.claude/skills/
```

## Use

Run `/coexist`, or ask in plain words:

- "write the PR description"
- "file an issue for this bug"
- "clean up the description of #123"
- "rewrite ENG-123 in the coexist format"

## What it handles

| Situation                         | What the skill does                                                                        |
| --------------------------------- | ------------------------------------------------------------------------------------------ |
| GitHub PRs and issues             | Folds the agent part into `<details>`, after the template's own sections                   |
| Linear issues                     | Uses `>>> For AI agents`, the form Linear stores and returns (it also accepts `+++`)        |
| `Fixes #123` and closing keywords | Keeps the closing line as written and leaves keywords out of the fold, so nothing else closes |
| Linear issues with images         | Edits with partial updates, since Linear signs image URLs per read and they expire         |
| Customer-filed issues             | Leaves them as their author wrote them                                                     |

## Works better with

Both are optional. The skill carries a short fallback for each.

- [`ce-noslop`](https://github.com/EveryInc/compound-engineering-plugin/tree/main/skills/ce-noslop) from Every's compound-engineering plugin, for taking AI writing tells out of the human part.
- [`writing-for-agents`](https://github.com/mattpocock/skills/tree/main/skills/productivity/writing-for-agents) by Matt Pocock, for writing the agent part.

## Related

Some project skills already fold agent context, as one rule among many:

- [skyf0xx/hedgehog `pr-writing`](https://github.com/skyf0xx/hedgehog/blob/master/src/skills/pr-writing/SKILL.md) folds deep reasoning under `<details>`.
- [Taketo-Yoda/uv-sbom `issue`](https://github.com/Taketo-Yoda/uv-sbom/blob/develop/.claude/skills/issue/SKILL.md) adds a collapsed implementation spec for agents to GitHub issues.
- [PostHog `writing-pr-descriptions`](https://github.com/PostHog/posthog/blob/master/.agents/skills/writing-pr-descriptions/SKILL.md) puts the effect first and collapses transcripts.

`coexist` makes the split the whole skill, and covers Linear and rewriting existing descriptions.

## License

MIT
