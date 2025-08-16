# Replacing Tmux

## Writing a script to open many windows for a project
```bash
#!/usr/bin/env bash
# sessionName="MONS"
# sessionExists=$(tmux list-sessions | rg "$sessionName")

projects_dir="./projects"
client_dir="./projects/battle_monsters_web_client"
main_project_dir="./projects/battle_monsters"

kitty @ launch --cwd $client_dir --title client --type tab --keep-focus
kitty @ launch --cwd $main_project_dir --title backend --type tab --keep-focus
kitty @ launch --cwd "${main_project_dir}/game-db" --title schema --type tab --keep-focus
kitty @ launch --cwd "${projects_dir}/haskell-oauth-2-mock" --title auth --type tab --keep-focus

cd "$main_project_dir/"
kitten @ set-tab-title "git"
lazygit
```
