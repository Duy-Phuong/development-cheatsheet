```bash
❯ cd ~/.ssh
❯ tree
.
├── config
├── id_rsa
├── id_rsa.ppk
├── id_rsa.pub
├── id_rsa_other
├── id_rsa_other.pub
├── known_hosts
└── known_hosts.old


❯ cd ..


❯ ls

.config/starship.toml
.ssh
.tigrc
.gitignore
```

## Add new repository

```bash
…or create a new repository on the command line
echo "# development-cheatsheet" >> README.md
git init
git add README.md
git commit -m "first commit"
git branch -M main
git remote add origin git@github-superman:Duy-Phuong/development-cheatsheet.git
git push -u origin main


…or push an existing repository from the command line


git remote add origin git@github-superman:Duy-Phuong/development-cheatsheet.git
git branch -M main
git push -u origin main
```