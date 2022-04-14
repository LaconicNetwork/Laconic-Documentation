# Laconic-Documentation

This repository is the source for the site https://docs.laconic.com, detailing the usage of the [Laconic Network](https://laconic.com).

## Local editing

The site is built with [Hugo](https://github.com/gohugoio/hugo/). To edit locally, first [install Hugo](https://gohugo.io/getting-started/installing/), and then start Hugo in server mode in the root of your checkout of this repository.

This site is currently built with `hugo_extended_0.92.0`.

```
▶ hugo server -D

Started building sites ...
.
.
Serving pages from memory
Web Server is available at http://localhost:1313/ (bind address 127.0.0.1)
Press Ctrl+C to stop
```
The `-D` flag allows you to see draft content.

The deployment script is:

`hugo --minify`

If you notice differences in the styling between your local copy and the preview built on Fleek, try adding `--minify` to your local server command.

## Fleek hosting

The site, including all per-branch preview deployments, is hosted on IPFS via Fleek. Cloudflare IPFS gateway is set up as a part of the Fleek build, including TLS procurement. 

All Fleek deployment URLs can be accessed from the branches here: https://github.com/LaconicNetwork/Laconic-Documentation/branches

## Adding content

In general, to create a new page, open a terminal, cd to the repository's root directory, and type

```
hugo new content/<sectionName>/<newFile>.md
```

- Replace `<sectionName>` by the name of the directory right below `content/` that you want to add a page to.
- Replace `<newFile>` by the name of the file to create. In general, there are no rules for that name but it should relate to the content.

Example:

```
hugo new content/faq/watcher.md
```

would create a new file `watcher.md` in the FAQ section. The initial content is derived from a template file in `archetypes/` with the same name as the section (e.g., `faq.md` is the template for docuemnts created in the `faq` section.)

Then edit the new file as needed. If you have a local Hugo server running, it reflects all changes almost instantly.

### Adding FAQ entries

New FAQ entries contain this front matter section:

```
---
question:
weight: 999
---
```

- Add the question to the `question` field, without surrounding quotes.
- The `weight` entry specifies the sequence of FAQ entries. In other words, FAQ entries are sorted by weight in ascending order. Typically you would set this to the count of existing FAQ files (including the new one). If you want to move that item to a higher position, you would need to change the weights of all entries that shall appear after the new one. 
- Write the answer below the front matter. You can use Markdown formatting here.

Example:
```
---
question: Pepsi or Coke?
weight: 8
---
It depends.
```

### Adding glossary entries

Glossary entries come with this initial front matter:

```
---
title: A title
description: A new entry to the glossary
tags:
  - fundamental
---
```

- The `title` field contains the glossary term
- The `description` field contains a *short*  description
- The `tags` field is a list of tags (currently not evaluated but might be used for filtering the glossary when it gets too long for one page)
- Below the front matter, write a detailed explanation of the term. You can use Markdown formatting here.

Example:

```
---
title: diphthong
description: A phonetics term referring to an unsegmentable sound
tags:
  - fundamental
---

A phonetics term referring to an unsegmentable sound, like the OI in oil. It’s been described as the "[the best word ever](https://www.theatlantic.com/technology/archive/2012/09/here-it-is-the-best-word-ever/262348/)", and while that may or may not be true, the combination of dip, which can refer to a clueless individual, and thong, which is a style of underwear, creates auditory splendor. (Source: [dictionary.com](https://www.dictionary.com/e/s/funny-words/#diphthong))

```

### Cross-references 

To add cross-references, for example, between FAQ or glossary entries, the Hugo-safe way is to use the [ref and relref shortcodes](https://gohugo.io/content-management/shortcodes/#ref-and-relref).

Example: 

```
{{< glossary "ethereum" "Ethereum" >}}
{{< glossary "glossary/ethereum" "Ethereum" >}}
```

`relref` returns the relative URL (without the host part; for example, "`/glossary/ethereum`"). If the document name cannot be resolved uniquely (in this case, to "`contents/glossary/ethereum.md`", Hugo generates an error message. Using simple Markdown links is possible but circumvents the URL resolution and validation.
