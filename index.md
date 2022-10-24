------
# Organizing Your Code Projects with Git

Chapter 12 knjizice 'Beyond the Basic Stuff with Python'
https://inventwithpython.com/beyond/chapter12.html  
---
Git = najpopularniji version control (onda mercurial i subversion (aka svn)).
Umesto da pravis kopije za svoje projekte kako napreduju, bolje ti je da learn how to set up files for code projects and use Git to track their changes - nema veze iako radis sam i na manjem projekticu.
Version control sluzi i da tim sto radi na projektu se sinhronizuje (kad jedan komituje promenu, ostali mogu da je pull-uju; za kolaboraciju ima i malo naprednijih ficera ala branching and merging).

###  /1/ ~ Terminologija ~

*Commit* = *Snapshot* = neka verzija projekta na koju mozes da roll-back-ujes (commit moze i kao glagol).
*Repo* = *Reposetory* = Git folder sa project fajlovima (uobicajeno jedan repo po projektu).
*Working directory* = project folder = folder sa svim kodom, dokumentacion, testovima i drugim fajlovima vezanim za projekat. The files in the working directory are collectively called the working copy.
\* *Promene se komituju, ostali mogu da promene pull-uju, i u kolaboraciji jos postoje neki termini naprednijih ficera (branching i merging).*

###  /2/ ~ New Python Project ~

Python projects follow conventions for folder names and hierarchies.
Typically, the root of the project folder contains a src folder for the .py source code files, a tests folder for unit tests, and a docs folder for any documentation + Other files contain project information and tool configuration: README.md for general information, .coveragerc for code coverage configuration, LICENSE.txt for the project’s software license, and so on.  

*NTS: Za cookie-cuter boilerplate pokazuje u knjizi i neki tul za generisanje ful strukture projekta na pocetku da je sve automacki (preskacem, za kasnije koriscenje git-a nebitno, i ide-i i github isto nesto od tog bojlerplejta mogu da generisu pa nebitno; inace sam sama napravila ovu najprostiju strukturu foldera i samo README.md file)*

###  /3/ ~ Instaliranje Git-a ~

1/ Prover da nije vec instaliran u cmd-u sa: "git --version"
2/ Za Win skini instaler sa https://git-scm.com/download > instaliraj
3/ Konfigurisi korisnicko ime i mail u cmd (ili iz Git Bash) sa:
git config --global user.name "Ime Prezime"
git config --global user.email kototamopeva597@gmail.com
4/ mozes da proveris sta iskonfigurisano sa: git config --list
5/ mozes (za sad prekacem) da instaliras neki Gui za koriscenje git-a (neki pobrojani na https://git-scm.com/download/gui/windows, medju njima i kornjaca sto nekad koristih)

### /4/  ~ Git Workflow ~

\- Git je distribuirani version control system tako da se sve storuje lokalno u folderu ".git" - sa njima moze da se radi i offline ne mora da bude commit na neki server (dakle razlikuje local i remote reposetori, sto naravno relevantno za kolaboraciju ili ako hoces remote backup (obicno u neki privat repozitorijum na github-u)).

**_\- Tipican workflow:_**

**1/** Kreiras Git repo sa: git init 
Tacnije: kreiranje Git repoa na lokalu (=  pravljenje ".git" foldera) koraci su:
u cmd cd-ujes u folder sa projektom > cukas `git init` > stvorio se prazan Git repo u ".git" novostvorenom folderu (sam novostvoreni folder nije prazan ima git stvarcice, al je generalno sakriven folder vidis ga kad kazes u folder setinzima da hoces da vidis skreivene fajlove i foldere)

**1.5/** Ako hoces da pogledas promene u odnos na prethodni commit (ako ovo nije prvi commit za projekat):
u cmd komandu: `git diff` > sa - i + ti pokazuje modifikacije sta je izbrisano ili dodato u fajlu u odnosu na prethodni commit
\- Posto je ovo malo tesko citljivo, ima i vizualni tool ako za neki odredjeni fajl hoces lepse da pogledas promenu: za win skines https://winmerge.org/ > instaliras > kazes Gitu da ga koristi sa `git config diff.tool winmerge` > upotrebis ga sa `git difftool <filename sa putanjom ako nije u root-u tipa scr\\filename >`
\* Ako hoces uvek ovaj visualni diferens da koristis bez da te uvek pita jesi siguran, ranuj `git config --global difftool.prompt false`
\* Takodje gui programcici za koriscenje gita tipa kornjaca ove "diference" ficere vec imaju obicno.
**1.6/** Ako imas neke unit testove, treba da ih ranujes pre komitovanja. Idealno, all your tests should pass (and if they don’t pass, mention this in the commit message).

**2/** Status svih fajlova mozes da vidis ako ucukas `git status`>
Fajlove prvo dodajes u listu za sledeci commit tako sto u cmd cukas:
`git add imeFajla.txt` (ovo za svaki fajl pojedinacno) 
ILI 
za djuture na foru za sve fajlove:  `git add .`  (bukvalno fajlove i u svim podfolderima, ne foldere kao takve) 
ILI 
`git add \*.py` (doda sve .py fajlove u working directory-ju i pod-folderima) 
\* Obrnuti proces = unstaging = skidanje sa liste za sledeci komit = `git restore --staged <filename>`  

**3/** Te fajlove sledece komitujes sa: 
`git commit -m "neka poruka o komitu"`
Posle toga normalno menjas kod/radis.
Ako neces sve fajlove stavljene u listu za sledeci komit, nogo samo odredjene, mozes sa `git commit –m "komit poruka" file1.py file2.py`
Takodje mozes jednom komandom korake 2/ 3/ (`git add .` +  `git commit -m "neka poruka o komitu"`): 
`git commit –am "komit poruka"`
Ako hoces da modifikujes komit poruku, mozes sa : `git commit --amend -m "Ispravljena komit poruka"

**3.5/** ako ti vec dodat repo i na gitHubu, komitujes i tamo sa:
`git push`
a ako nije na gitHub-u a hoces da bacis tamo => see below za rad sa gitHubom (za samo backup moze github privatni repo da bude kul)

-Svi trakovani fajlovi mogu biti u jednom od 3 stanja:
	- committed state = unmodified state = clean state = radna verzija je identicna sa repo\`s most recent commit.
	- modified state
	- staged state = modifikovan i markiran da bude ukljucen u sledeci komit (kaze se: "file is staged or in the staging area". The staging area is also known as the index or cache.)
- Da saznas u kom su stanju ti fajlovi u repozitorijumu, ranujes: `git status` i ta komanda ti i izlista i par sledecih logicnih kamandi: kad hoces da dodas u za_komit_listu/komitujes.
* help za komande sa: `git help <imeKomande>`

### /5/  ~ Dodavanje Fajlova u Ignor Listu ~

Dok radis na projektu, you might want to exclude certain files from version control completely so you don’t accidentally track them. These include:
    -Temporary files in the project folder.
    -The .pyc, .pyo, and .pyd files that the Python interpreter generates when it runs .py programs.
    -The .tox, htmlcov, and other folders that various software development tools generate.
    -Any other compiled or generated files that could be regenerated (because the repo is for source files, not the products created from source files).
    -Source code files that contain database passwords, authentication tokens, credit card numbers, or other sensitive information.
\* konkretno kako se ovo radi vraca me na tul za generisanje kompletne strukture projekta, a mislim da sam nasla laksi seljacki nacin:
`.git\info\exclude` file sam editovala da izgleda:
```
# git ls-files --others --exclude-from=.git/info/exclude
# Lines that start with '#' are comments.
# For a project mostly in C, the following would be a good set of
# exclude patterns (uncomment them if you want to use them):
# *.[oa]
# *~

Firefox.lnk
```
Tj.: samo sam dodala ime ovog fajla (u ovom slucaju firefox shortkata) sto hocu da Git ignorise.

### /6/  ~ Pisanje/Brisanje Fajlova iz Git Repo ~

- Pisanje:
U cmd cd-ujes gde hoces da se pojavi fajl i cukas:
```bash
echo "TekstTestFajla" > napraviPaBrisiMe.txt`
git add napraviPaBrisiMe.txt
git commit -m "Dodavanje fajla za testiranje pisanja brisanja fajlova u repozitorijumu"
```
\* doduse mogla si doticni fajl i samo seljacki da napravis nije moralo u cmd sa echo

\- Brisanje (mozes da brises samo ful komitovane fajlove ne u nekom medjustanju fajlove):
```bash
git rm napraviPaBrisiMe.txt
git commit -m "Deleting napraviPaBrisiMe.txt from the repo to finish the deletion test."
```
=> stvarno izbrisan fajl i vise se ne trakuje ali postoji u istorijatu repozitorijuma i moze da se povrati (see below).

### /7/  ~ Reimenovanje i Pomeranje Fajlova u Git Repo  ~
(ista "mv" komanda)

- Reimenovanje README.md u README.txt:
```bash
git mv README.md README.txt
git commit -m "Testing the renaming of files in Git."
```

- Kreiranje novog fodera (svakako se ne komituje doklegod prazan folder): 
```bash
mkdir novi_foldercic
```

- Pomeranje fajla u "novi_foldercic" folder:
```bash
git mv README.txt novi_foldercic/README.txt
git commit -m "Pomeranje fajlova po folderima u git repo testing"
```

- Reimenovanje i pomeranje fajla po folderima u isto vreme:
```bash
git mv novi_foldercic/README.txt README.md
git commit -m "Moving the README file back to its original place and name."
```

### /8/   ~ Gledanje Istorijat Loga ~

- CD-ovan u project folder cukas:
```bash
git log
```
=> izlista od poslednjeg do prvog komita sa komentima (ako previse redova skrati ga pa sa enter otkriva red po red), i za kvit moras da cikas "q".
Tu za svaki komit vidis commit hash, a 40-character string of hexadecimal digits.
Za kompresovanu verziju cikas:
```bash
git log --oneline"
```
ili jos kompresovanije, npr samo zadnja 3 komita:
```bash
git log --oneline –n 3
```

### /9/   ~ Gledanje istorijat verzije fajla ~

CD-ovan u repo cukas:
`git show <hash>:<filename>
npr:
`git show 1af4cba9c70cf21baddc42ba34c213b2e54db32a:README.md`
ili skracen hash isto sljaka (prvih 7 cifara)
`git show 1af4cba:README.md`

Ali: GUI Git tools will provide a more convenient interface for examining the repo log than the command line Git tool provides.

### /10/   ~ Rolling Back ~

-Tacna komanda ce da zavisi od stanja fajlova u working copy. Version control systemi samo dodaju info (zato je moguce rekoverovati cak i fajlove obrisane iz git repo). Rolling back takodje se belezi kao "nova promena".
O raznim rollbacks ima detaljnije na https://github.blog/2015-06-08-how-to-undo-almost-anything-with-git/ .

*Rollback scenario 1:* Undo na poslednji komit = Vrati na prethodnu komitovanu verziju, sta sam od tad radila nije ni stavljeno u red za komit, ni komitovano, i samo hocu da ne postoje te promene od zadnjeg komita, samo da se vratim na zadnju komitovanu verziju:
`git restore <filename>`
npr:
`git restore README.md`
ili za sve djuture 
`git checkout .`

*Rollback scenario 2:* Hoces da "undo" promene zadnja 3 komita (i vratis se na cetvrti komit unazad):
```bash
git revert -n HEAD~3..HEAD
git add .
git commit -m "Start-over anduovanjem zadnja 3 komita." 
```

*Rollback scenario 3:* Hocu da proverim kako je izgledao odredjeni fajl posle odredjenog komita, i onda da vratim samo taj fajl na stanje posle tog odredjenog komita:
`git show hash_comita:ime_fajla.txt`  
=> prikaze mi kako posle tog komita izgledao taj fajl, sad kad ga vidim, ako hocu na to da ga revertujem i taj revert komitujem:
``` bash
git checkout hash_comita -- ime_fajla.txt
git add ime_fajla.txt
git commit -m "Rolbek odredjenog fajla na stanje posle odredjenog komita"
```

*Rollback scenario 4:* Slucajno sam komitovala nesto senzitivno tipa fajl sa paswordima, i hocu da ga ful izbrisem i iz istorije:
-Actually removing this information from your repo so it’s unrecoverable is tricky but possible. The exact steps are beyond the scope of this book, but you can use either the git filter-branch command or, preferably, the BFG Repo-Cleaner tool. You can read about both at https://help.github.com/en/articles/removing-sensitive-data-from-a-repository.
-The easiest preventative measure for this problem is to have a secrets.txt, confidential.py, or similarly named file where you place sensitive, private information, and add it to .gitignore so you’ll never accidentally commit it to the repo. Your program can read this file for the sensitive info instead of having the sensitive info directly in its source code.

### /11/   ~ Koriscenje GitHub-a ~
*\*NTS: sa zardjalim OSom moze da ne uspe korak 3 (problemi sa ssl/tls kanalom, i tesko za ne suportovan OS opravis)*

Mnogi online sajtovi mogu da houstuju klonove repo-a online za dzabe - GitHub je najpopularniji. Klon je i kul backup (nikako nista osetljivo na dzabe sajtu naravno).

Za koriscenje gitHuba koraci:
0. Ako nemas otvori free account na https://github.com (imam: kototamopeva597@gmail.com; AmfarKodTaii; fbstxfxqry2h6dw AND mmmb9410@gmail.com (Mimi B jshdfhsjkdhf54654wer) github username: hjhjhjkh pass: jshdfhsjkdhf54654wer ) > Login
1. New repository (gore desno meni) > dodas ime, opis, izaberes public/private (privatni na hub-u nema wiki strane osim ako platis, privatni ne moz ni srcom ni sa online da nadjes, samo da git-kloniras ako vec znas lokaciju repoa), ostale preskocis ako ces da importujes svoj repo postojeci > klik "Create repository". (sad je taj repo spravljen na githubu i moze da se vidi na `https://github.com/<username>/<repo_name>` (npr https://github.com/AmfarKodTaii/firstRepo.git))
2. Da dodas GitHub kao remote repo corresponding to your local repo, lokalno u cmd cukas:
`git remote add origin https://github.com/<github_username>/<repo_name>`
(npr: `git remote add origin https://github.com/AmfarKodTaii/firstRepo.git` )
3. Push any commits you’ve made on your local repo to the remote repo (i opet nece se pojaviti prazni folderi):
```bash 
git push -u origin master
```
odnosno,
After this first push, you can push all future commits from your local repo by running 2 commands: 
`git pull` (= jel ima neka promena na central repozitorijumi koju lokalno nisi imao pa mozda treba nesto da se mrdzuje ili resolvuje ako u konfliktu (ako nema konflikta samo novina na centralnom, samo ce da skine novinu, a ako konlikt, pokazace ti u samom fajlu u konliktu neke kerefeke ce da stavi oko kolfliktnog dela u lokalnom fajlu))
`git push` (= cak i posle par propustenih lokalnih commit-a sve pushuje i propustene)

=> 
Sad moze bilo ko da se skine ceo folder sa `https://github.com/<github_username>/<repo_name>.git`  >Code > Download zip (skinuti zip nema ni prazne foldere ni .git nevidljivi) (private repo ne moz da se skine ovako, cak ni neko sa gitHub nalogom ulogovan, samo vlasnik repoa moze).
ili
Ako neko hoce da skine projekat (i sa .git folderom, al to ne ukljucuje i wiki strane, issues, pull requests i ostalo gitHub specific) cuka (i za private repo sljaka): 
`git clone https://github.com/<github_username>/<repo_name>.git` (npr: `git clone https://github.com/AmfarKodTaii/firstRepo.git`)
ili wiki koj napravis za taj projekat: 
`git clone https://github.com/AmfarKodTaii/firstRepo.wiki.git`

```
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
                             ~ THE END ~
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
```