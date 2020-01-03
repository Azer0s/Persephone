---
name: Instruction request
about: Propose a new instruction for Persephone
title: ''
labels: enhancement, investigation required
assignees: ''

---

**Is your feature request related to a problem? Please describe.**
A clear and concise description of what the problem is. Ex. I'm always frustrated when [...]

**Describe the solution you'd like**
A clear and concise description of what you want to happen.

**Describe alternatives you've considered**
A clear and concise description of any alternative solutions or features you've considered.

### Code

Persephone code using this instruction could look like this:

```coffeescript
v_uint8 foo
...
```

### Stack

When used, the stack changes like so:

```
+----- ~ ~ ~ -----+
|      ~ ~ ~      |
+----- ~ ~ ~ -----+
|     0x5 (u32)   |
+-----------------+
|     0x8 (u32)   |
+-----------------+
```
