# fp-cowork-plugins

Claude Cowork plugins for the Farmer Power team.

## fp-wiki

Asks the Farmer Power wiki questions. The wiki is the private repository
`farmerpower-ai/fp-wiki`; this plugin reads it through GitHub's read-only connector and answers
the Open Knowledge Format way: it starts at `wiki/index.md`, follows the concept indexes, the page
fields and the links, cites every page it used, and writes nothing.

Use it with:

```
/fp-wiki:ask <question>
```

It also answers when you ask Claude about the wiki in plain words ("what does the wiki say
about ..."), but the command is the way to be sure it reads the wiki and nothing else.

### What this repository holds

No secret. The plugin names the variable `FP_WIKI_GITHUB_TOKEN`; its value is the read-only token
you receive from the wiki owner, and it is never written in this repository.

| File | What it is |
| --- | --- |
| `.claude-plugin/marketplace.json` | Lists the plugins of this repository |
| `plugins/fp-wiki/.claude-plugin/plugin.json` | The plugin's name and version |
| `plugins/fp-wiki/.mcp.json` | The GitHub connector, read-only, repositories only |
| `plugins/fp-wiki/skills/fp-wiki-query/SKILL.md` | How to read the wiki and answer |
| `plugins/fp-wiki/commands/ask.md` | The `/fp-wiki:ask` command |

### Install

The install steps for an individual Cowork account are written here after the first test install.

### The token

The wiki owner creates one fine-grained GitHub token: resource owner `farmerpower-ai`, repository
`fp-wiki` only, permission Contents read-only, with an expiry date. When it expires, the owner
sends a new one and each member replaces the value once.
