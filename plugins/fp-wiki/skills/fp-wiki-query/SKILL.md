---
name: fp-wiki-query
description: 'Answer a question from the Farmer Power wiki (GitHub repository farmerpower-ai/fp-wiki), reading it through its indexes, page fields and links, with a link to every page used. Read-only. Use when the user says "ask the wiki", "what does the wiki say about ...", "look it up in the wiki", or asks about Farmer Power: the QC analyzer, the cloud platform, the AI platform, farmers, bags, grading, messages, factories, costs.'
---

# Ask the Farmer Power wiki

The Farmer Power wiki is the private GitHub repository `farmerpower-ai/fp-wiki`, branch `main`.
Read it with the GitHub connector of this plugin (`fp-wiki-github`), using its file-reading tool
(`get_file_contents`) with owner `farmerpower-ai`, repo `fp-wiki`, ref `main`. The connector is
read-only. Never try to write, comment, open an issue or a pull request.

## How the wiki is built

The wiki uses the Open Knowledge Format (OKF): each page is a field block in YAML between two `---`
lines, followed by a Markdown text.

- `wiki/index.md` is the entry point. It lists the two owner pages, then the product map: groups,
  items, and under each item the concepts that belong to it.
- `wiki/core-business.md` and `wiki/product-map.md` are the owner pages. They say what the company
  does and where every product fits.
- Each concept is a folder `wiki/<concept>/` with an `index.md` listing its pages, one per view.
- A view page is `wiki/<concept>/<view>.md` or `<view>-<part>.md`. The views are `business`,
  `integration`, `data`, `operations`, `hardware` and `agronomy`.
- A page's fields: `title`, `description` (one sentence), `view`, `belongs_to` (the product map item),
  `status` (`draft` means the owner has not reviewed it yet; no field means `stable`), and `sources`
  (the copies of the documents under `raw/` the page was written from).
- `raw/<date>-<name>/` holds copies of the requirement documents. A page links a requirement id such
  as `FR-30` to its heading in one of these copies.

## Read

Read only through the indexes, fields and links, in this order, and only what the question needs:

1. `wiki/index.md`. Choose the concepts from the product map and the concept descriptions.
2. The `index.md` of each concept chosen.
3. The view pages the question needs, chosen by their `view` and their `description`.
4. Any page those pages link to, including a copy under `raw/` when the answer depends on the exact
   words of a requirement.

Never search the repository by file name or guess a path. Never list folders to find pages. If the
indexes do not lead to an answer, say so.

## Rules for the answer

- Answer in plain text, in the user's language.
- Link every page used by its path in the repository, for example
  `wiki/graded-bag/business.md`.
- Several requirement documents use the same ids (two documents each have an `FR-30` and a `J1`).
  Always name the project with the id: "the cloud platform's FR-30", "the AI platform's J1".
- When a page says a rule comes from one project and another page says something different from
  another project, give both and say which project says which. Do not pick one.
- Keep what the pages say about status: "planned", "not built yet", "a target not yet measured".
- When a page used has `status: draft`, say once at the end that it has not been reviewed yet.
- When the pages read do not answer the question, say so. Do not answer from general knowledge.
- End the answer with the list of pages read, in the order they were read.

## When the connector fails

If the GitHub connector is missing or refuses access, say that the wiki could not be read and that the
read-only token (`FP_WIKI_GITHUB_TOKEN`) may be missing or expired. Do not answer from memory.
