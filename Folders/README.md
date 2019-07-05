# Folder Structure
Mostly taken from [@sindresorhus](https://github.com/sindresorhus/ama/issues/557):

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

## Prevent Spotlight from indexing files in `code`.

Tip from [@roelvangils](https://twitter.com/roelvangils/status/1113074439976075264):

> Does your Mac becomes slow/unresponsive (fans kicking in etc.) when you `npm install` a huge project with a million tiny dependencies? I learned that adding an empty `.metadata_never_index` in /node_modules *beforehand* will prevent Spotlight from indexing all that crap.

As an alternate, there's a command line fix from this [yarn issue](https://github.com/yarnpkg/yarn/issues/6453#issuecomment-476048618):

```bash
find . -type d -name "node_modules" -exec touch "{}/.metadata_never_index" \;
```

And as a bash alias:

```bash
alias fix-spotlight='find . -type d -name "node_modules" -exec touch "{}/.metadata_never_index" \;'
```

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
[includeIf "gitdir:~/code/oss/"]
	path = .gitconfig-oss
[includeIf "gitdir:~/code/private/"]
	path = .gitconfig-private
[includeIf "gitdir:~/code/todo/"]
	path = .gitconfig-todo
[includeIf "gitdir:~/code/work/"]
	path = .gitconfig-work
```

Then I add two additional sibling files to my profile root directory, next to my .gitconfig.

For my private & open source work I have my non-work values in a .gitconfig-private file.

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
├───.gitconfig-deprecated
├───.gitconfig-private
├───.gitconfig-oss
├───.gitconfig-todo
└───.gitconfig-work
```

This approach could be replicated to any number of patterns you choose. If you have specific accounts you use for client work you just add more entries to the conditional block. There have been scenarios where I would need to use an identity given by the client so that would require another conditional entry and .gitconfig-work-acme-archery config file.

I did find that with the nested directory approach the last entry wins so you have to put more specific values at the end.

This effectly translates an if or switch statement in my profile to explicit .gitconfig entries and config files. I'm not sure if I'll stick with it but I'll test drive it for now.
