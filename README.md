# Linux Least Privilege Lab

I did this lab to practice one of the basics that actually matters: file permissions. I audited a project directory, found the misconfigurations, and fixed them with `chmod`. It was part of my Google Cybersecurity Certificate work.

## The task

Audit `/home/researcher2/projects`, find every permission that violates the principle of least privilege, and fix it.

## What I found

Running `ls -la` turned up three problems:

1. `project_k.txt` was world-writable (`-rw-rw-rw-`). Anyone on the system could edit it.
2. `.project_x.txt` was writable when policy said read-only (`-rw--w----`).
3. The `drafts/` directory let group members traverse it (`drwx--x---`) when they should not have access at all.

## What I did

Took write access away from everyone except the owner on the world-writable file:

```bash
chmod o-w /home/researcher2/projects/project_k.txt
# -rw-rw-r--
```

Locked the hidden file down to read-only for user and group, nothing for others:

```bash
chmod 440 /home/researcher2/projects/.project_x.txt
# -r--r-----
```

Restricted the directory to the owner only:

```bash
chmod 700 /home/researcher2/projects/drafts
# drwx------
```

A final `ls -la` confirmed everything matched policy.

## What this shows

I am comfortable in a Linux terminal and I understand how user, group, and other permissions work in practice, not just in theory. Least privilege is simple to explain and easy to get wrong. This is me getting it right, in both symbolic and numeric `chmod` modes.

## License

MIT License. See [LICENSE.md](LICENSE.md) for details.
