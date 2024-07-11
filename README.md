# obsidian-base
A template repository for Obsidian based repos.

## Rules
For structure of the Obsidian repo, one should follow these rules:
- If repo is **attachment heavy**, each directory should have a separate attachment folder:
```shell
.
├── first-dir
│   └── attachment
└── second-dir
    └── attachment
```

- If repo is not attachment heavy or by no means the organization of attachments are useful, then:
```shell
.
├── attachment
├── first-dir
└── second-dir
```

- Don't make a note too large (max 3-4 page), instead try to break down the concepts into smaller notes, but with stronger graph relations (it is a knowledge base).
