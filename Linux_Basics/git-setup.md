# Git Setup

Hello, folks.

Before this, I have set up `git` on my **Parrot OS**. Now I'm going to redo and document it here. This time, I'll be doing it on my **Manjaro XFCE**.

---

## Git & Github Terms

Let's get familiar with the terms first.

When I first set it up on my Parrot OS, I was an absolute beginner at git. I had to read and memorize the *lingo*  first to get comfortable on the long run. 

So here's a quick reference:

| Term                  | Definition |
|-----------------------|------------|
| **Repository (repo)** | Project folder tracking files and all changes. Local = computer, Remote = GitHub. |
| **Commit**            | Snapshot of your project at a specific time, with a message describing changes. |
| **Branch**            | Separate timeline for development; default branch is `main`. |
| **Clone**             | Copy a remote repo from GitHub to your local machine. |
| **Fork**              | Personal copy of someone else's GitHub repo for experimentation. |
| **Pull**              | Get the latest changes from a remote repo to your local machine. |
| **Push**              | Send your local commits to the remote GitHub repo. |
| **Merge**             | Combine changes from one branch into another. |
| **Pull Request (PR)** | Request on GitHub to merge a branch into another branch, with review. |
| **Staging Area (Index)** | Temporary area to prepare files before committing (`git add`). |
| **.gitignore**        | File specifying which files/folders Git should ignore. |
| **HEAD**              | Pointer to your current branch and commit. |
| **Remote**            | Reference to a repository on GitHub (default name: `origin`). |
| **Fetch**             | Download changes from a remote repo without merging them. |
| **Conflict**          | Occurs when changes clash in the same file; requires manual resolution. |
| **Tag**               | Label for a specific commit, often used for versioning (e.g., `v1.0`). |
| **Actions (GitHub)**  | Automations on GitHub that run tasks like testing or deployment on push. |

You can access a more descriptive info on the [References](https://github.com/gamalkevin/linux-hub/tree/main/References) folder.

---

## Preconfiguring `git`

I have `git` installed already. Let's check the version:
```
[kev@Randrake ~]$ git --version
git version 2.50.1
```

Then, configure my global identity, which will appear in commits:
```
[kev@Randrake ~]$ git config --global user.name "gamalkevin"
[kev@Randrake ~]$ git config --global user.email "gamal.kevin@gmail.com"
```

Verifying the config:
```
[kev@Randrake ~]$ git config --list
user.name=gamalkevin
user.email=gamal.kevin@gmail.com
```

Also, since I'll be using git mainly to document things using [**Markdown**](https://www.markdownguide.org/cheat-sheet/), I set the editor to [**Ghostwriter**](https://ghostwriter.kde.org/documentation/):
```
[kev@Randrake ~]$ git config --global core.editor ghostwriter
```

### Setting up SSH key
For Manjaro, I decided to not reuse the same key from the Parrot OS' git, so I generate a new one (and name it accordingly). 
```
[kev@Randrake ~]$ ssh-keygen -t ed25519 -C "gamal.kevin@gmail.com" -f ~/.ssh/id_ed25519_manjarokey

Generating public/private ed25519 key pair.
Created directory '/home/kev/.ssh'.
Enter passphrase for "/home/kev/.ssh/id_ed25519_manjarokey" (empty for no passphrase): 
Enter same passphrase again: 
Your identification has been saved in /home/kev/.ssh/id_ed25519_manjarokey
Your public key has been saved in /home/kev/.ssh/id_ed25519_manjarokey.pub
The key fingerprint is:
********************************
The key's randomart image is:
********************************
```
Followed by adding it to the agent:
```
[kev@Randrake ~]$ ssh-add ~/.ssh/id_ed25519_manjarokey
Identity added: /home/kev/.ssh/id_ed25519_manjarokey (gamal.kevin@gmail.com)
```
Reveal the public key to copy and add it to Github:
```
[kev@Randrake ~]$ cat ~/.ssh/id_ed25519_manjarokey.pub
```
Check the connection after adding the key to Github:
```
[kev@Randrake ~]$ ssh -T git@github.com
The authenticity of host 'github.com (20.205.243.166)' can't be established.
ED25519 key fingerprint is SHA256:*****************.
This key is not known by any other names.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added 'github.com' (ED25519) to the list of known hosts.
Hi gamalkevin! You've successfully authenticated, but GitHub does not provide shell access.
```
**Note:**
As you might have noticed, I named my key with a personal identifier `manjarokey`. I have multiple Linux systems installed in various places (work, home, portable HDDs), so I decided to give unique names to each SSH key.

But when you test the connection, you will get an error:
```
$ ssh -T git@github.com git@github.com: Permission denied (publickey).
```

The fix is simple: We need to add a config in `~/.ssh/config` so `git` will know which key to use. For example:
```
Host github.com
  HostName github.com
  User git
  IdentityFile ~/.ssh/id_ed25519_manjarokey
```

Setting successful; now moving on to...

---

## Setting up `git`

### On GitHub
First, let's make a **repository**.

#### Create a repository on GitHub
1. Go to **GitHub Profile** → 
	- Go to **Repositories** tab → 
	- click New repository.
	- Give it a name (example: `linux-hub`).
	- Decide if it’s public or private.
	- I skip creating README.md for now
2. Click Create repository.

After that we can see instructions for connecting an existing repo or creating a new one locally.

### Local git
To keep things organized, I put all my projects in a designated folder: 
```
mkdir ~/Projects
cd ~/Projects
```
Clone the Github repo: 
```
git clone git@github.com:gamalkevin/linux-hub.git
```
Git will automatically create a folder called `linux-hub` with all the repo contents.

To access the repo folder, simply `cd`:
```
cd ~/Projects/linux-hub
```

We've succesfully set up git on our local machine, and it's ready to go. If you're still interested, you can continue on reading on how I prepopulate the folders for categorizing future docum

---

## Prepopulating the repo
Since I'm going to be using this repo as my **Linux Knowledge Hub**, I figure it's better to setup the folders from now.

However, git doesn't support pushing empty folders. So I'll be creating placeholder README.md (*which themselves are using a same template*) in every folder category. 

Here's a little script that I use:
```
# Go to the repo folder first
cd ~/Projects/linux-hub

# Define folder names
folders=("Linux_Basics" "System_Maintenance" "Networking" "Security" "Development" "Troubleshooting" "Automation" "Advanced" "References")

# Loop through folders and create README.md with template content
for folder in "${folders[@]}"; do
    mkdir -p "$folder"
    cat > "$folder/README.md" <<EOL
# ${folder//_/ }

This folder contains guides and resources related to **${folder//_/ }**.

## Guides

- [Guide 1](./Guide1.md) – Short description of what this guide covers.
- [Guide 2](./Guide2.md) – Short description of what this guide covers.
- [Guide 3](./Guide3.md) – Short description of what this guide covers.

## Notes
- All guides are in Markdown format for easy reading.  
- Scripts or commands are included with comments where applicable.  
- Feel free to add your own notes or improvements.
EOL
done
```
What this script do:
- `mkdir -p` creates the folder (and any parent if missing).
- `touch` creates an empty `README.md` inside each folder.
- All `README.md` use the same template, for uniformity.

---

## Notes
- When cloning, you will notice that I used SSH link to clone (*Repo main page: Code > SSH*) 
	* While it does require extra initial steps on setup, it will be way smoother as it automatically uses the SSH key. 
	* This way, I don't need to repeatedly enter credentials unlike the `https` mode that is token/password-based.

---

My git repo is finally fully ready to use. To read more about **`git` usage** (most used commands, how-to), go to [Git Usage](./git-usage.md).

*Thanks for reading!*