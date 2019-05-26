# Folder Structure
Mostly taken from https://github.com/sindresorhus/ama/issues/557:

```
.
├── Applications
├── Desktop
├── Documents
├── Downloads
├── Movies
├── Music
├── Pictures
├── code
│   ├── deprecated
│   ├── forks
│   ├── oss
│   │   ├── project1
│   │   ├── project2
│   │   └── …
│   ├── private
│   ├── todo
│   ├── work
│   └── scratchpad.md
```

* **deprecated:** Unmaintained projects are moved here to reduce noise in other directories.
* **forks:** Repos I fork are placed here.
* **oss:** My open source projects.
* **private:** Private projects like my macOS apps.
* **todo:** Projects I started but never finished enough to publish.
* **work:** Projects for my day job.
* **scratchpad.md:** Random ideas and todos.

## Conditional Includes For Git Config
*Mainly taken from [Eric William's blog](https://www.motowilliams.com/conditional-includes-for-git-config).*

Removing the following section in my .gitconfig

```bash
[user]
name = Firstname Lastname
email = user@example.com
```

with this conditional

```bash
[includeIf "gitdir:~/code/"]
  path = .gitconfig-personal
[includeIf "gitdir:~/code/work/"]
  path = .gitconfig-work
```

Then I add two additional sibling files to my profile root directory, next to my .gitconfig.

For my personal & open source work I have my non-work values in a .gitconfig-personal file.

```bash
[user]
name = Jon Doe
email = jon@doe.com
```

For my source code work related to my.gitconfig-work

```bash
[user]
name = Jon Doe
email = jon@work.com
```

So that I end up with these files in my profile root.

```bash
C:\users\jon\
├───.gitconfig
├───.gitconfig-personal
└───.gitconfig-work
```

This approach could be replicated to any number of patterns you choose. If you have specific accounts you use for client work you just add more entries to the conditional block. There have been scenarios where I would need to use an identity given by the client so that would require another conditional entry and .gitconfig-work-acme-archery config file.

I did find that with the nested directory approach the last entry wins so you have to put more specific values at the end.

This effectly translates an if or switch statement in my profile to explicit .gitconfig entries and config files. I'm not sure if I'll stick with it but I'll test drive it for now.
