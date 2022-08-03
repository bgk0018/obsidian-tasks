---
tags: [moc, {{title}}]
---

# {{title}}

## Content
```dataview
TABLE WHERE contains(tags, lower("{{title}}")) AND contains(tags, "content")
```

## Authors
```dataview
TABLE WHERE contains(tags, lower("{{title}}")) AND contains(tags, "author")
```