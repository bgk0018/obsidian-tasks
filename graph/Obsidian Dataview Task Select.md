**Warning** you can do a **ton** with Dataview. Be ready to go down a rabbit hole here.

[Repository](https://github.com/blacksmithgu/obsidian-dataview)
[Documentation](https://blacksmithgu.github.io/obsidian-dataview/)

**Example:**

List all Tasks in my Journal folder using the SQL-like Syntax
```dataview
TASK FROM "journal"
```

List all incomplete tasks using **Javascript**!
```dataviewjs
dv.taskList(dv.pages().file.tasks.where(t => !t.completed));
```