---
tags: [daily]
---
[[{{date-1d: YYYY-MM-DD}}|<<< {{date-1d: YYYY-MM-DD}}]]  --  [[{{date+1d: YYYY-MM-DD}}|{{date+1d: YYYY-MM-DD}} >>>]]
# {{title}}
![[daily.jpg]]

![[Landing#Values]]
## Tasks
---

> [!ATTENTION]+ Carry Over
> ```tasks
> not done
> due before {{title}}
> short mode```

> [!TODO]+
> ```tasks
> not done
> happens on {{title}}
> short mode```

> [!DONE]-
> ```tasks
> done on {{title}}
> short mode```

## Journal
---



## Other Notes
---


**Created Today:**
```dataview
TABLE dateformat(file.cday, "yyyy-MM-dd") as "Created"
WHERE dateformat(file.cday, "yyyy-MM-dd") = this.file.name 
	AND file.path != this.file.path
	AND file.folder != "journal"
SORT file.cday desc 
```

**Modified Last Today:**
```dataview
TABLE dateformat(file.mday, "yyyy-MM-dd") as "Modified"
WHERE dateformat(file.mday, "yyyy-MM-dd") = this.file.name 
	AND file.path != this.file.path
	AND file.folder != "journal"
	AND file.cday != file.mday
SORT file.mday desc 
```