---
layout : page
title : Understanding SSH for GitHub
date : 2025-05-04
---

Like most people, I initially used HTTPS to access (push/pull) my repositories from my local machine to/from GitHub. Since I was on Windows, I used Git Bash to access (push/pull) my repositories. I had to enter my Github credentials (username and password) only once. After that I could freely access without any further authentication. The Github password was replaced with PAT (Personal Access Token) since 2021 but it was similar.

**Reason:** Credential Helpers work by default on Windows and macOS. When you install Git for Windows, the Git Credential Manager (GCM) is included and enabled by default. GCM stores your credentials securely in the Windows Credential Store. After you enter your credentials (username + PAT) once, GCM remembers them, so you aren’t prompted again for future pushes/pulls. Similarly Git on macOS uses the osxkeychain credential helper by default.

When I transitioned to Linux, I found this magic was no longer working. I had to enter my credentials each time.

I found that I had two options:

1. Still use PAT & HTTPS and manually configure Credential-Helper using `git config --global credential.helper cache` or `git config --global credential.helper store`.
2. Use SSH with public/private key pair

I just didn't want to use PATs, because I had to choose among so many granular permissions each time I created one on Github. So I went with the second option. I had also heard that using SSH was much faster and more secure than using HTTPS because it used a public/private key pair to authenticate the user. This meant that even if someone intercepted the data being transmitted, they would not be able to decrypt it without the private key. I just needed to set up SSH keys on my local machine and add them to my GitHub account. So here's how I did it:

1. **Generate SSH keys:** Run this in the terminal: `ssh-keygen -t ed25519 -C "your_email@example.com"` : Keep the passphrase empty orelse you'll have to enter it each time again!

2. **Add SSH keys to GitHub:** Copy the contents of the public key file (`~/.ssh/id_ed25519.pub`) and add it to your GitHub account under Settings > SSH and GPG keys > New SSH key.

3. **Test SSH connection:** Run this in the terminal: `ssh -T git@github.com`. If everything is set up correctly, you should see a message like "Hi username! You've successfully authenticated, but Github does not provide shell access."

4. **Start accessing through SSH:** Go to your old project and run `git remote -v`. You may see the https url. Change it to the ssh url of your remote github repo using `git remote set-url origin git@github.com:username/repo.git`. Now run `git push`. Similarly you can pull, clone, etc.

5. **OPTIONAL: Configure Git to use SSH:** Run this in the terminal: `git config --global url."git@github.com:".insteadOf "https://github.com/"`. This will tell Git to use SSH instead of HTTPS for all GitHub repositories. This is useful when you use a Github client like Github Desktop to directly pull repositories.
