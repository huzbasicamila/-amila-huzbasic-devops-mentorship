# Git and github

## Osnovne naredbe

* git init- komanda koja kreira "skriveni" folder ".git" koji sadrži modifikaciju konfiguracije koja omogućava gitu da radi.

* git status- pregled liste fajlova unutar direktorija koji se nalaze i ne nalaze unutar version controla

* git add <file/directory name > - komanda za dodavanje određenog fajla iz direktorija unutar version controla 

* git add . - komanda koja dodaje sve fajlove unutar version controla te će one biti spremne za commit .

*  git config --global- potrebno je postaviti osnovne informacije prije izvršavanja komande commit. 

### Komande za dodavanje username-a i email-a : 

git config --global user.name "Your Name"

git config --global user.email mail@example.com

### Komande za brisanje username-a i email-a 

git config --global --remove-section user.name

git config --global --remove-section user.email

* git commit -m "Initial commit" - komanda koja kreira novi commit sa pratećom porukom. Komanda commit cuva dosadasnje promjene unutar fajlove, te se uvijek mozemo vratiti na stanje do commita. 

* git clone https://github.com/username/projectname.git - komanda za kloniranje nekog projekta 

### SSH for Git

* Komanda za generisanje ssh kljuceva

> ssh-keygen

* Komanda za izlistavanje ssh kljuceva

> ls -al ~/.ssh

Potrebno je kopirati content iz id_*.pub fajla i paste-ati ga unutar GitHuba, GitLaba i slično. 

### GIT LOG KOMANDA

* git log- komanda koja će ispisati sve commitove koji su urađeni do sada zajedno sa autorom, emailom, datumom i commit id-em. 

* git shortlog - komanda koja grupira logove po autoru i prikazuje samo poruke commitova

* git log --after '3 days ago' - primjer filtiraranja logova

* git log --stat - komanda koja pokazuje fajlove nad kojima je izvršena promjena prilikom commita

* git show (id commita) - komanda koja polazuje kontent commita sa navedenim id-em

### Rad sa remote repo

* git push [remote-name] --delete [branch-name] - brisanje grane sa remote repozitorija

* git fetch- komadnda koja dohvaca promjene sa remote repoa, ne primjenjuje ih na radni direktorij. Nakon izvršavanja "git fetch" možete pregledati dohvaćene promjene i usporediti ih s trenutnim stanjem prije nego što odlučite primijeniti promjene na trenutni radni direktorij.

* git pull- radi git fetch i git merge odjednom. Ova naredba je pogodna za situacije kada želite brzo ažurirati svoj lokalni repozitorij s najnovijim promjenama s udaljenog repozitorija i odmah ih primijeniti na svoj rad.

* git branch nova-grana - komanda za kreiranje nove grane

* git branch -b nova-grana - komanda koja kreira i prebacuje se na novu granu

* git checkout nova-grana - komanda koja prebacuje na drugu granu

* git reset <filePath> - komanda za unstageane fajla koji se nalazi u version controlu

* git reset --hard idnunber- brisanje promjena koje su napravljene commitom navedenog id-a

* git merge <branch> - merganje trenutne grane sa nekom drugom granom

* git stash - cuva izmjene koje nisu spremne za commit kako bi mogli preci na drugu granu. 
_
* git revert <id_commit> - kreira novi komit koji brise sve promjene napravljene za odredenim commitom






