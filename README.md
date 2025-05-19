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
git clone <url>         # Clona repo da GitHub
git status              # Mostra modifiche
git add .               # Aggiunge tutti i file tracciati
git commit -m "msg"     # Commit con messaggio
git push origin main    # Pusha su GitHub
git pull origin main    # Prende aggiornamenti da GitHub
git log                 # Mostra cronologia

🌿 Gestione branch
git branch nome-branch        # Crea branch
git checkout nome-branch      # Cambia branch
git checkout -b nuovo-branch  # Crea e cambia branch
git merge nome-branch         # Unisce branch in quello attuale

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
