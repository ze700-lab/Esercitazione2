# Git-tutorial
Introduzione a git

## PortableGit
Scaricare l'ultima versione di PortableGit: https://github.com/git-for-windows/git/releases/
![](PortableGit.png)
Eseguire il file exe scaricato.
![](exe.png)
Aprire il programma `git-bash` nella cartella di PortableGit appena creata.
![](git-bash.png)

## Creazione progetto
Creare una cartella per il progetto sul Desktop chiamata `Esercitazione2`.
Su git-bash eseguire
```
cd  $HOME/path/to/Esercitazione2
```
dove `path/to/` rappresenta il percorso della cartella `Esercitazione2`.

Creare un file `README.md` in Blocco note, che sarà il readme file del repository Git, scrivete Esercitazione2 e aggiungete il vostro nome e classe .
Salvare il file.

### Creazione repository su Github
- Aprire il sito github.com
- Effettuare il login
- Cliccare su `New` ![](newRepo.png)
- Creare un nuovo repository `Esercitazione2`, prestando attenzione a non creare un Readme di default ![](repo.png)

### Configurazione del repository locale e sincronizzazione
Da `git-bash` eseguire i seguenti comandi:
```
git init  # Inizializza il repository locale
git add README.md  # Inserimento del file README.md nell'area di staging
git commit -m "first commit"  # Creazione del primo commit, che serve a sincronizzare il repository locale con lo stage
git branch -M main  # Creazione del branch main, da usare come default
git remote add origin git@github.com:<username>/Esercitazione2.git  # Connessione del repository remoto al repository locale
git push -u origin main  # Sincronizzazione del repository remoto con quello locale
```

# Sezione 2
## Branching con git
Nella directory di lavoro dell’esempio precedente la digitazione del comando git branch visualizza inizialmente solo il branch principale main:
```
giorgio@localhost:~/workspace/ExpenseManager> git branch

* main
```
Dopo la creazione di un nuovo branch per l’attività di documentazione del progetto il comando git branch visualizza entrambi i branch indicando con il simbolo «*» quello attualmente selezionato:
```
giorgio@localhost:~/workspace/ExpenseManager> git branch Documentazione
giorgio@localhost:~/workspace/ExpenseManager> git branch

 Documentazione
* main
```
L’attività di sviluppo viene effettuata sul branch principale aggiungendo/modificando file di codice ed effettuando il commit delle aggiunte/modifiche apportate:
## Aggiunta di nuovi file al repository
1. Creare un nuovo file `main.cpp` nel workspace
2. Implementare un programma che, dati in input 10 numeri positivi, trovi il massimo e lo visualizzi a video
3. Aggiungere il nuovo file nell'area di staging
4. Sincronizzare l'area di staging con il repository locale (usando un messaggio di commit congruo)
5. Sincronizzare il repository remoto con quello locale
```
giorgio@localhost:~/workspace/ExpenseManager> ls
main.cpp README.md
giorgio@localhost:~/workspace/ExpenseManager> git add main.*
giorgio@localhost:~/workspace/ExpenseManager> git commit -m "Aggiunta classe Main"
[main 4c53517] Aggiunta classe Main
 2 files changed, 462 insertions(+)
 create mode 100644 main.cpp
 ```

 L’attività di documentazione viene effettuata sul branch dedicato aggiungendo/modificando file di testo ed effettuando il commit delle aggiunte/modifiche apportate; nel branch dedicato alla documentazione non sono visibili le aggiunte/modifiche apportate nel branch principale:
 ```
giorgio@localhost:~/workspace/ExpenseManager> git checkout documentazione
Switched to branch 'documentazione'
giorgio@localhost:~/workspace/ExpenseManager> git branch

* documentazione
 main

giorgio@localhost:~/workspace/ExpenseManager> ls
main.h LICENSE README
giorgio@localhost:~/workspace/ExpenseManager> git add README
giorgio@localhost:~/workspace/ExpenseManager> git commit -m "Aggiunta file README"
[Documentazione f4f025d] Aggiunta file README
 1 file changed, 5 insertions(+)
 create mode 100644 README
  ```
Nel branch principale è possibile effettuare un’operazione di merge con il branch dedicato alla documentazione successivamente alla quale sono visibili i file aggiunti in entrambi i flussi di lavoro:
 ```
giorgio@localhost:~/workspace/ExpenseManager> git checkout main
Switched to branch 'main'
Your branch is ahead of 'origin/main' by 1 commits.
 (use "git push" to publish your local commits)
 giorgio@localhost:~/workspace/ExpenseManager> git merge Documentazione -m
"Merge documentazione"
Merge made by the 'recursive' strategy.
 README | 5 +++++
 1 file changed, 5 insertions(+)
 create mode 100644 README
giorgio@localhost:~/workspace/ExpenseManager> ls
main.cpp Main.h README
```

Il comando git log visualizza l’operazione di merging come un’operazione di commit e include nel proprio output le operazioni di commit effettuate sul branch dedicato alla documentazione:
 ```
giorgio@localhost:~/workspace/ExpenseManager> git log

commit d2fe76497089669f7af1904bad001e2107355e46 (HEAD -> main)
Merge: 4c53517 f4f025d
Author: Giorgio Meini <...@...>
Date: Wed Jun 30 23:44:05 2021 +0200

 Merge documentazione

commit f4f025dac954479f162020c229ad0b89448fd485 (Documentazione)
Author: Giorgio Meini <...@...>
Date: Wed Jun 30 23:19:21 2021 +0200

 Aggiunta file README

 commit 4c53517a54f88d00da1d50d5339d86d687acc006
Author: Giorgio Meini <...@...>
Date: Wed Jun 30 23:15:04 2021 +0200

 Aggiunta classe Progetto

commit 92665c63a7125cd0e6475a16a613cfb630011005
Author: Giorgio Meini <...@...>
Date: Wed Jun 30 14:46:12 2021 +0200

 Classi base del progetto

commit bb546b6af951f65425641cca9ef02aa3b1f2fe54
Author: giorgio-meini <86645920+giorgio-meini@users.noreply.github.com>
Date: Tue Jun 29 07:35:04 2021 +0200

 Initial commit
```
