🧪 Testing
Jest (test unitari per JavaScript)

describe, test, expect, toBe, toEqual
Esecuzione test: npm test o npx jest
Playwright (test end-to-end/browser automation)

Automazione di browser (Chromium, Firefox, WebKit)
Test di interfaccia: click, input, assertions
Esecuzione: npx playwright test


🧰 2. Comandi Git da usare in VS Code

🔁 Comandi base da terminale (integrato in VS Code)
git init                 # Inizializza repo locale
git clone <url>          # Clona repo da GitHub
git status               # Mostra modifiche
git add .                # Aggiunge tutti i file tracciati
git commit -m "msg"      # Commit con messaggio
git push origin main     # Pusha su GitHub
git pull origin main     # Prende aggiornamenti da GitHub
git log                  # Mostra cronologia
git remote remove origin # Rimuove il vecchio remote (se esiste)

🌿 Gestione branch
git branch nome-branch        # Crea branch
git checkout nome-branch      # Cambia branch
git checkout -b nuovo-branch  # Crea e cambia branch
git merge nome-branch         # Unisce branch in quello attuale
git branch -d nome-branch     # Elimina branch locale già mergiato
git branch -D nome-branch     # Elimina branch locale forzatamente
git push origin --delete nome-branch # Elimina branch remoto

🔀 Flusso di lavoro tipico per feature branch
# 1. Aggiorna la main locale
git checkout main
git pull origin main

# 2. Crea e passa a un nuovo branch per la feature
git checkout -b feature/nome-feature

# 3. Lavora, aggiungi e committa le modifiche
git add .
git commit -m "Implementa nome-feature"

# 4. Pusha il branch su GitHub
git push origin feature/nome-feature

# 5. (Su GitHub) Apri una Pull Request verso main

# 6. Dopo il merge della PR, elimina il branch locale e remoto
git checkout main
git pull origin main
git branch -d feature/nome-feature
git push origin --delete feature/nome-feature

🔧 Altri utili
git remote add origin  https://github.com/<tuo-username>/<nome-repo> # Setta l'origine della repo su cui pushare  
git remote -v                 # Vedi remote repo
git fetch                     # Recupera aggiornamenti (senza merge)
git diff                      # Differenze tra file modificati
git stash                     # Salva modifiche temporaneamente (non committate)
git stash pop                 # Recupera modifiche stashed
git rebase main               # Rebase del branch corrente su main
git cherry-pick <hash>        # Applica un singolo commit da un altro branch

🛠️  Risoluzione conflitti merge
# Dopo un merge/rebase con conflitti:
# 1. Modifica i file per risolvere i conflitti
# 2. Aggiungi i file risolti
git add <file>
# 3. Completa il merge/rebase
git commit   # (se merge)
git rebase --continue  # (se rebase)

🔧 Altri utili
git remote add origin  https://github.com/<tuo-username>/<nome-repo> #setta l'origine della repo su cui pushare  
git remote -v                 # Vedi remote repo
git fetch                     # Recupera aggiornamenti (senza merge)
git diff                      # Differenze tra file modificati


🔄 CI/CD - GitHub Actions
Concetti base:
Workflow YAML file (.github/workflows/ci.yml)
Trigger: push, pull_request
Job e Step

Esempio minimo:

name: CI
on: [push]
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - name: Install deps
        run: npm install
      - name: Run tests
        run: npm test

Esempi slide:

BUILD

name: build-test-deploy
on: push
jobs:
 build:
   runs-on: ubuntu-latest
   steps:
     - name: checkout repo
       uses: actions/checkout@v3
     - name: use node.js
       uses: actions/setup-node@v3
       with:
         node-version: '22.x'
     - run: npm install
     - run: npm run build
    
TEST

name: build-test-deploy
on: push
jobs:
[...]
 test:
   needs: build
   runs-on: ubuntu-latest
   steps:
     - name: checkout repo
       uses: actions/checkout@v3
     - name: use node.js
       uses: actions/setup-node@v3
       with:
         node-version: '18.x'
     - run: npm install
     - run: npm test

DEPLOY

deploy:
   needs: test
   permissions:
     contents: write
     pages: write
     id-token: write
   environment:
     name: production
     url: ${{ steps.deployment.outputs.page_url }}
   runs-on: ubuntu-latest

steps:
    - name: checkout repo
      uses: actions/checkout@v3 with:
        token: ${{ secrets.GITHUB_TOKEN }}

    - name: use node.js
      uses: actions/setup-node@v3
      with:
        node-version: '18.x'

    - name: configure github pages
      uses: actions/configure-pages@v3
      with:
        static_site_generator: next

    - run: npm install
    - run: npm run build

    - name: upload artifacts
      uses: actions/upload-pages-artifact@v3 with:
      path: "./out"

    - name: deploy
      id: deployment
      uses: actions/deploy-pages@v4

CHECKOUT CON AUTENTICAZIONE
