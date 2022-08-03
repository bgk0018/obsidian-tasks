---
tags: [weekly]
---
# {{title}} Review
![[weekly 1.jpg]]
## Tasks

> [!Todo] Monday
> ```tasks
> happens on {{date+1d:YYYY-MM-DD}}
> short mode```

> [!Todo] Tuesday
> ```tasks
> happens on {{date+2d:YYYY-MM-DD}}
> short mode```

> [!Todo] Wednesday
> ```tasks
> happens on {{date+3d:YYYY-MM-DD}}
> short mode```

> [!Todo] Thursday
> ```tasks
> happens on {{date+4d:YYYY-MM-DD}}
> short mode```

> [!Todo] Friday
> ```tasks
> happens on {{date+5d:YYYY-MM-DD}}
> short mode```

> [!Todo] Saturday
> ```tasks
> happens on {{date+6d:YYYY-MM-DD}}
> short mode```

> [!Todo] Sunday
> ```tasks
> happens on {{date+7d:YYYY-MM-DD}}
> short mode```

---
## Notes From the Week

```dataview
LIST rows.file.link WHERE file.ctime >= date({{date:YYYY-MM-DD}}) AND file.ctime <= date({{date:YYYY-MM-DD}}) + dur(7days) GROUP BY file.folder
```
