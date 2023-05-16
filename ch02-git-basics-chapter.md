# Git Grundlagen {#ch02-git-basics-chapter}
## Ein Git-Repository anlegen {#_getting_a_repo}
### Ein Repository in einem bestehenden Verzeichnis einrichten {#_ein_repository_in_einem_bestehenden_verzeichnis_einrichten}
``` console
$ cd /home/user/my_project
```
``` console
$ cd /Users/user/my_project
```
``` console
$ cd C:/Users/user/my_project
```
``` console
$ git init
```
was ihre Aufgabe ist.[]{.indexterm primary="Git Befehle"
secondary="init"}
``` console
$ git add *.c
$ git add LICENSE
$ git commit -m 'Initial project version'
```
### Ein existierendes Repository klonen {#_git_cloning}

Wenn Sie eine Kopie eines existierenden Git-Repositorys anlegen wollen
-- um beispielsweise an einem Projekt mitzuarbeiten -- können Sie den
Befehl `git clone` verwenden. Wenn Sie bereits Erfahrung mit einem
anderen VCS-System, wie Subversion, gesammelt haben, fällt Ihnen
bestimmt sofort auf, dass der Befehl „clone" und nicht etwa „checkout"
lautet. Das ist ein wichtiger Unterschied, den Sie unbedingt verstehen
sollten. Anstatt nur eine einfache Arbeitskopie vom Projekt zu erzeugen,
lädt Git nahezu alle Daten, die der Server bereithält, auf den lokalen
Rechner. Jede Version von jeder Datei der Projekt-Historie wird
automatisch heruntergeladen, wenn Sie `git clone` ausführen. Wenn Ihre
aber alle versionierten Daten wären vorhanden -- siehe Kapitel 4, [Git
auf dem Server](ch04-git-on-the-server.md#_getting_git_on_a_server),
klonen.[]{.indexterm primary="Git Befehle" secondary="clone"} Um
beispielsweise das Repository der verlinkbaren Git-Bibliothek `libgit2`
zu klonen, führen Sie den folgenden Befehl aus:
``` console
$ git clone https://github.com/libgit2/libgit2
```
``` console
$ git clone https://github.com/libgit2/libgit2 mylibgit
```
auf dem Server](ch04-git-on-the-server.md#_getting_git_on_a_server),
## Änderungen nachverfolgen und im Repository speichern {#_änderungen_nachverfolgen_und_im_repository_speichern}
einem von zwei Zuständen befinden kann: *tracked* oder *untracked* --
![Der Status Ihrer Dateien im Überblick](images/lifecycle.png)
### Zustand von Dateien prüfen {#_checking_status}
Ihre Dateien gerade befinden, ist der Befehl `git status`.[]{.indexterm
primary="Git Befehle" secondary="status"} Wenn Sie diesen Befehl
unmittelbar nach dem Klonen eines Repositorys ausführen, sollte er in
etwa folgende Ausgabe liefern:

``` console
$ git status
On branch master
Your branch is up-to-date with 'origin/master'.
nothing to commit, working tree clean
```
geändert wurden -- andernfalls würden sie hier aufgelistet werden.
Branching](ch03-git-branching.md#ch03-git-branching) auf Branches
::: note
[ch01-getting-started.md](ch01-getting-started.md#_new_default_branch)
:::
``` console
$ echo 'My Project' > README
$ git status
On branch master
Your branch is up-to-date with 'origin/master'.
Untracked files:
  (use "git add <file>..." to include in what will be committed)
    README
nothing added to commit but untracked files present (use "git add" to track)
```
Alle Dateien, die im Abschnitt „Untracked files" aufgelistet werden,
### Neue Dateien zur Versionsverwaltung hinzufügen {#_tracking_files}
verwenden.[]{.indexterm primary="Git Befehle" secondary="add"} Für Ihre
neue `README` Datei, können Sie folgendes ausführen:
``` console
$ git add README
```
``` console
$ git status
On branch master
Your branch is up-to-date with 'origin/master'.
Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
    new file:   README
```
ist, weil sie unter der Rubrik „Changes to be committed" aufgelistet
Versionsverwaltung hinzugefügt.[]{.indexterm primary="Git Befehle"
secondary="init"}[]{.indexterm primary="Git Befehle" secondary="add"}
Der Befehl `git add` akzeptiert einen Pfadnamen einer Datei oder eines
Verzeichnisses. Wenn Sie ein Verzeichnis angeben, fügt `git add` alle
Dateien in diesem Verzeichnis und allen Unterverzeichnissen rekursiv
hinzu.
### Geänderte Dateien zur Staging-Area hinzufügen {#_geänderte_dateien_zur_staging_area_hinzufügen}
``` console
$ git status
On branch master
Your branch is up-to-date with 'origin/master'.
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)
    new file:   README
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)
    modified:   CONTRIBUTING.md
```
for commit". Das bedeutet, dass eine versionierte Datei im
andere Dinge -- beispielsweise einen Konflikt aus einem Merge als
bestimmten Inhalt für den nächsten Commit vormerken.[]{.indexterm
primary="Git Befehle" secondary="add"} Lassen Sie uns nun mit `git add`
die Datei `CONTRIBUTING.md` zur Staging-Area hinzufügen und danach das
Ergebnis mit `git status` kontrollieren:

``` console
$ git add CONTRIBUTING.md
$ git status
On branch master
Your branch is up-to-date with 'origin/master'.
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

    new file:   README
    modified:   CONTRIBUTING.md
```
``` console
$ vim CONTRIBUTING.md
$ git status
On branch master
Your branch is up-to-date with 'origin/master'.
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

    new file:   README
    modified:   CONTRIBUTING.md

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

    modified:   CONTRIBUTING.md
```

Was zum Kuckuck ...​? Jetzt wird die Datei `CONTRIBUTING.md` sowohl in
der Staging-Area als auch als geändert aufgelistet. Wie ist das möglich?
Die Erklärung dafür ist, dass Git eine Datei in exakt dem Zustand für
den Commit vormerkt, in dem sie sich befindet, wenn Sie den Befehl
`git add` ausführen. Wenn Sie den Commit jetzt anlegen, wird die Version
der Datei `CONTRIBUTING.md` denjenigen Inhalt haben, den sie hatte, als
Sie `git add` zuletzt ausgeführt haben und nicht denjenigen, den sie in
dem Moment hat, wenn Sie den Befehl `git commit` ausführen. Wenn Sie
``` console
$ git add CONTRIBUTING.md
$ git status
On branch master
Your branch is up-to-date with 'origin/master'.
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)
    new file:   README
    modified:   CONTRIBUTING.md
```
### Kompakter Status {#_kompakter_status}
``` console
$ git status -s
 M README
MM Rakefile
A  lib/git.rb
M  lib/simplegit.rb
?? LICENSE.txt
```
Ausgabe -- die linke Spalte zeigt den Status der Staging-Area und die
### Ignorieren von Dateien {#_ignoring}
hinzufügen oder schon gar nicht als „nicht versioniert" (eng. untracked)
Liste mit Vergleichsmustern enthält.[]{.indexterm
primary="ignorieren von Dateien"}[]{.indexterm primary="Dateien"
secondary="ignorieren"} Hier ist eine `.gitignore` Beispieldatei:

``` console
$ cat .gitignore
*.[oa]
*~
```

Die erste Zeile weist Git an, alle Dateien zu ignorieren, die auf „.o"
oder „.a" enden -- Objekt- und Archivdateien, die das Ergebnis der
wird. Sie können auch ein Verzeichnis „log", „tmp" oder „pid"
::: tip
:::
::: note
:::
### Überprüfen der Staged- und Unstaged-Änderungen {#_git_diff_staged}
Wenn der Befehl `git status` für Sie zu vage ist -- Sie wollen genau
wurden -- können Sie den Befehl `git diff` verwenden.[]{.indexterm
primary="Git Befehle" secondary="diff"} Wir werden `git diff` später
ausführlicher behandeln, aber Sie werden es wahrscheinlich am häufigsten
verwenden, um diese beiden Fragen zu beantworten: Was hat sich geändert,
ist aber noch nicht zum Commit vorgemerkt (engl. staged)? Und was haben
Sie zum Commit vorgemerkt und können es demnächst committen? Der Befehl
`git status` beantwortet diese Fragen ganz allgemein, indem er die
Dateinamen auflistet; `git diff` zeigt Ihnen aber genau die
hinzugefügten und entfernten Zeilen -- sozusagen den Patch.
sie zu „stagen". Wenn Sie den Befehl `git status` ausführen, sehen Sie
``` console
$ git status
On branch master
Your branch is up-to-date with 'origin/master'.
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)
    modified:   README
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)
    modified:   CONTRIBUTING.md
```
``` console
$ git diff
diff --git a/CONTRIBUTING.md b/CONTRIBUTING.md
index 8ebb991..643e24f 100644
--- a/CONTRIBUTING.md
+++ b/CONTRIBUTING.md
@@ -65,7 +65,8 @@ branch directly, things can get messy.
 Please include a nice description of your changes when you submit your PR;
 if we have to read the whole diff to figure out why you're contributing
 in the first place, you're less likely to get feedback and have your change
-merged in.
+merged in. Also, split your changes into comprehensive chunks if your patch is
+longer than a dozen lines.

 If you are starting to work on a particular area, feel free to submit a PR
 that highlights your work in progress (and note in the PR title that it's
```
„gestaged" sind.
``` console
$ git diff --staged
diff --git a/README b/README
new file mode 100644
index 0000000..03902a1
--- /dev/null
+++ b/README
@@ -0,0 +1 @@
+My Project
```
Änderungen seit Ihrem letzten Commit anzeigt -- nur die Änderungen, die
noch „unstaged" sind. Wenn Sie alle Ihre Änderungen bereits „gestaged"
Änderungen in der „staged"-Datei und die „unstaged"-Änderungen sehen.
``` console
$ git add CONTRIBUTING.md
$ echo '# test line' >> CONTRIBUTING.md
$ git status
On branch master
Your branch is up-to-date with 'origin/master'.
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)
    modified:   CONTRIBUTING.md
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)
    modified:   CONTRIBUTING.md
```
``` console
$ git diff
diff --git a/CONTRIBUTING.md b/CONTRIBUTING.md
index 643e24f..87f08c8 100644
--- a/CONTRIBUTING.md
+++ b/CONTRIBUTING.md
@@ -119,3 +119,4 @@ at the
 ## Starter Projects
 See our [projects list](https://github.com/libgit2/libgit2/blob/development/PROJECTS.md).
+# test line
```
``` console
$ git diff --cached
diff --git a/CONTRIBUTING.md b/CONTRIBUTING.md
index 8ebb991..643e24f 100644
--- a/CONTRIBUTING.md
+++ b/CONTRIBUTING.md
@@ -65,7 +65,8 @@ branch directly, things can get messy.
 Please include a nice description of your changes when you submit your PR;
 if we have to read the whole diff to figure out why you're contributing
 in the first place, you're less likely to get feedback and have your change
-merged in.
+merged in. Also, split your changes into comprehensive chunks if your patch is
+longer than a dozen lines.

 If you are starting to work on a particular area, feel free to submit a PR
 that highlights your work in progress (and note in the PR title that it's
```

::: note
::: title
:::
:::
### Die Änderungen committen {#_committing_changes}
noch nicht zum Commit vorgemerkt ist -- alle Dateien, die Sie erstellt
`git add` ausgeführt haben -- nicht in diesen Commit einfließen werden.
committen.[]{.indexterm primary="Git Befehle" secondary="status"} Am
einfachsten ist es, wenn Sie `git commit` eingeben:[]{.indexterm
primary="Git Befehle" secondary="commit"}
``` console
$ git commit
```
::: note
Das wird durch die Umgebungsvariable `EDITOR` Ihrer Shell festgelegt --
Schritte](ch01-getting-started.md#ch01-getting-started) gesehen
haben.[]{.indexterm primary="Editor"
secondary="ändern der Voreinstellung"}[]{.indexterm
primary="Git Befehle" secondary="config"}
:::
::: note
:::
``` console
$ git commit -m "Story 182: fix benchmarks for speed"
[master 463dc4f] Story 182: fix benchmarks for speed
 2 files changed, 2 insertions(+)
 create mode 100644 README
```
### Die Staging-Area überspringen {#_die_staging_area_überspringen}

[]{.indexterm primary="Staging-Area" secondary="überspringen"} Obwohl es
außerordentlich nützlich sein kann, Commits so zu erstellen, wie Sie es
wünschen, ist die Staging-Area manchmal etwas komplexer, als Sie es für
Ihren Workflow benötigen. Wenn Sie die Staging-Area überspringen
möchten, bietet Git eine einfache Kurzform. Durch das Hinzufügen der
Option `-a` zum Befehl `git commit` wird jede Datei, die bereits vor dem
Commit versioniert war, automatisch von Git zum Commit vorgemerkt (engl.
staged), so dass Sie den Teil `git add` überspringen können:

``` console
$ git status
On branch master
Your branch is up-to-date with 'origin/master'.
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

    modified:   CONTRIBUTING.md

no changes added to commit (use "git add" and/or "git commit -a")
$ git commit -a -m 'Add new benchmarks'
[master 83e38c7] Add new benchmarks
 1 file changed, 5 insertions(+), 0 deletions(-)
```
### Dateien löschen {#_removing_files}
[]{.indexterm primary="Dateien" secondary="entfernen"} Um eine Datei aus
Git zu entfernen, müssen Sie sie aus der Versionsverwaltung entfernen
(genauer gesagt, aus Ihrem Staging-Bereich löschen) und dann committen.
Der Befehl `git rm` erledigt das und entfernt die Datei auch aus Ihrem
Arbeitsverzeichnis, so dass Sie sie beim nächsten Mal nicht mehr als
„untracked"-Datei sehen.
erscheint sie unter dem „Changes not staged for commit"-Bereich (das ist
``` console
$ rm PROJECTS.md
$ git status
On branch master
Your branch is up-to-date with 'origin/master'.
Changes not staged for commit:
  (use "git add/rm <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)
        deleted:    PROJECTS.md
no changes added to commit (use "git add" and/or "git commit -a")
```
``` console
$ git rm PROJECTS.md
rm 'PROJECTS.md'
$ git status
On branch master
Your branch is up-to-date with 'origin/master'.
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)
    deleted:    PROJECTS.md
```
Ihrer Datei `.gitignore` hinzuzufügen und dies versehentlich „gestaged"
``` console
$ git rm --cached README
```
``` console
$ git rm log/\*.log
```
``` console
$ git rm \*~
```
### Dateien verschieben {#_git_mv}
[]{.indexterm primary="Dateien" secondary="verschieben"} Im Gegensatz zu
vielen anderen VCS-Systemen verfolgt (engl. track) Git das Verschieben
von Dateien nicht ausdrücklich. Wenn Sie eine Datei in Git umbenennen,
werden keine Metadaten in Git gespeichert, die dem System mitteilen,
dass Sie die Datei umbenannt haben. Allerdings ist Git ziemlich clever,
das im Nachhinein herauszufinden -- wir werden uns etwas später mit der
``` console
$ git mv file_from file_to
```
``` console
$ git mv README.md README
$ git status
On branch master
Your branch is up-to-date with 'origin/master'.
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)
    renamed:    README.md -> README
```
``` console
$ mv README.md README
$ git rm README.md
$ git add README
```
einziger Befehl ist statt deren drei -- es ist eine Komfortfunktion.
## Anzeigen der Commit-Historie {#_viewing_history}
Diese Beispiele verwenden ein sehr einfaches Projekt namens „simplegit".
``` console
$ git clone https://github.com/schacon/simplegit-progit
```
erhalten, die ungefähr so aussieht:[]{.indexterm primary="Git Befehle"
secondary="log"}
``` console
$ git log
commit ca82a6dff817ec66f44342007202690a93763949
Author: Scott Chacon <schacon@gee-mail.com>
Date:   Mon Mar 17 21:52:11 2008 -0700
    Change version number
commit 085bb3bcb608e1e8451d4b2432f8ecbe6306e7e7
Author: Scott Chacon <schacon@gee-mail.com>
Date:   Sat Mar 15 16:40:33 2008 -0700
    Remove unnecessary test
commit a11bef06a3f659402fe7563abf99ad00de2209e6
Author: Scott Chacon <schacon@gee-mail.com>
Date:   Sat Mar 15 10:31:28 2008 -0700
    Initial commit
```
``` console
$ git log -p -2
commit ca82a6dff817ec66f44342007202690a93763949
Author: Scott Chacon <schacon@gee-mail.com>
Date:   Mon Mar 17 21:52:11 2008 -0700

    Change version number

diff --git a/Rakefile b/Rakefile
index a874b73..8f94139 100644
--- a/Rakefile
+++ b/Rakefile
@@ -5,7 +5,7 @@ require 'rake/gempackagetask'
 spec = Gem::Specification.new do |s|
     s.platform  =   Gem::Platform::RUBY
     s.name      =   "simplegit"
-    s.version   =   "0.1.0"
+    s.version   =   "0.1.1"
     s.author    =   "Scott Chacon"
     s.email     =   "schacon@gee-mail.com"
     s.summary   =   "A simple gem for using Git in Ruby code."

commit 085bb3bcb608e1e8451d4b2432f8ecbe6306e7e7
Author: Scott Chacon <schacon@gee-mail.com>
Date:   Sat Mar 15 16:40:33 2008 -0700

    Remove unnecessary test

diff --git a/lib/simplegit.rb b/lib/simplegit.rb
index a0a60ae..47c6340 100644
--- a/lib/simplegit.rb
+++ b/lib/simplegit.rb
@@ -18,8 +18,3 @@ class SimpleGit

 end
-
-if $0 == __FILE__
-  git = SimpleGit.new
-  puts git.show
-end
```
``` console
$ git log --stat
commit ca82a6dff817ec66f44342007202690a93763949
Author: Scott Chacon <schacon@gee-mail.com>
Date:   Mon Mar 17 21:52:11 2008 -0700
    Change version number
 Rakefile | 2 +-
 1 file changed, 1 insertion(+), 1 deletion(-)
commit 085bb3bcb608e1e8451d4b2432f8ecbe6306e7e7
Author: Scott Chacon <schacon@gee-mail.com>
Date:   Sat Mar 15 16:40:33 2008 -0700
    Remove unnecessary test
 lib/simplegit.rb | 5 -----
 1 file changed, 5 deletions(-)
commit a11bef06a3f659402fe7563abf99ad00de2209e6
Author: Scott Chacon <schacon@gee-mail.com>
Date:   Sat Mar 15 10:31:28 2008 -0700
    Initial commit
 README           |  6 ++++++
 Rakefile         | 23 +++++++++++++++++++++++
 lib/simplegit.rb | 25 +++++++++++++++++++++++++
 3 files changed, 54 insertions(+)
```
``` console
$ git log --pretty=oneline
ca82a6dff817ec66f44342007202690a93763949 Change version number
085bb3bcb608e1e8451d4b2432f8ecbe6306e7e7 Remove unnecessary test
a11bef06a3f659402fe7563abf99ad00de2209e6 Initial commit
```
nützlich, wenn Sie Ausgaben für das maschinelle Parsen generieren -- da
von Git nicht ändert:[]{.indexterm primary="Log formatieren"}
``` console
$ git log --pretty=format:"%h - %an, %ar : %s"
ca82a6d - Scott Chacon, 6 years ago : Change version number
085bb3b - Scott Chacon, 6 years ago : Remove unnecessary test
a11bef0 - Scott Chacon, 6 years ago : Initial commit
```
+-------------+--------------------------------------------------------+
| S           | Beschreibung der Ausgabe                               |
| pezifikator |                                                        |
+=============+========================================================+
| `%H`        | Commit-Hash                                            |
+-------------+--------------------------------------------------------+
| `%h`        | gekürzter Commit-Hash                                  |
+-------------+--------------------------------------------------------+
| `%T`        | Hash-Baum                                              |
+-------------+--------------------------------------------------------+
| `%t`        | gekürzter Hash-Baum                                    |
+-------------+--------------------------------------------------------+
| `%P`        | Eltern-Hashes                                          |
+-------------+--------------------------------------------------------+
| `%p`        | gekürzte Eltern-Hashes                                 |
+-------------+--------------------------------------------------------+
| `%an`       | Name des Autors                                        |
+-------------+--------------------------------------------------------+
| `%ae`       | E-Mail-Adresse des Autors                              |
+-------------+--------------------------------------------------------+
| `%ad`       | Erstellungs-Datum des Autors (Format berücksichtigt    |
|             | \--date=option)                                        |
+-------------+--------------------------------------------------------+
| `%ar`       | relatives Erstellungs-Datum des Autors                 |
+-------------+--------------------------------------------------------+
| `%cn`       | Name des Committers                                    |
+-------------+--------------------------------------------------------+
| `%ce`       | E-Mail-Adresse des Committers                          |
+-------------+--------------------------------------------------------+
| `%cd`       | Erstellungs-Datum des Committers                       |
+-------------+--------------------------------------------------------+
| `%cr`       | relatives Erstellungs-Datum des Committers             |
+-------------+--------------------------------------------------------+
| `%s`        | Thema                                                  |
+-------------+--------------------------------------------------------+

: Wichtige Spezifikatoren für `git log --pretty=format`
die Anerkennung -- Sie als Autor und das Core-Mitglied als Committer.
Wir werden diese Unterscheidung näher in Kapitel 5, [Verteiltes
``` console
$ git log --pretty=format:"%h %s" --graph
* 2d3acf9 Ignore errors from SIGCHLD on trap
*  5e3ee11 Merge branch 'master' of https://github.com/dustin/grit
|\
| * 420eac9 Add method for getting the current branch
* | 30e367c Timeout code and tests
* | 5a09431 Add timeout protection to grit
* | e1193f8 Support for heads with slashes in them
|/
* d6016bc Require time for xmlschema
*  11d191e Merge branch 'defunkt' into local
```
`git log` -- es gibt noch viele mehr. [Allgemeine Optionen für
+-------------+--------------------------------------------------------+
| Option      | Beschreibung                                           |
+=============+========================================================+
| `-p`        | Zeigt den Patch an, der mit den jeweiligen Commits     |
|             | eingefügt wurde.                                       |
+-------------+--------------------------------------------------------+
| `--stat`    | Anzeige der Statistiken für Dateien, die in den        |
|             | einzelnen Commits geändert wurden.                     |
+-------------+--------------------------------------------------------+
| `-          | Anzeige nur der geänderten/eingefügten/gelöschten      |
| -shortstat` | Zeile des Befehls \--stat.                             |
+-------------+--------------------------------------------------------+
| `-          | Listet die Dateien auf, die nach den                   |
| -name-only` | Commit-Informationen geändert wurden.                  |
+-------------+--------------------------------------------------------+
| `--n        | Listet die Dateien auf, die von hinzugefügten,         |
| ame-status` | geänderten oder gelöschten Informationen betroffen     |
|             | sind.                                                  |
+-------------+--------------------------------------------------------+
| `--abb      | Zeigt nur die ersten paar Zeichen der SHA-1-Prüfsumme  |
| rev-commit` | an, nicht aber alle 40.                                |
+-------------+--------------------------------------------------------+
| `--rel      | Zeigt das Datum in einem relativen Format an (z.B.     |
| ative-date` | „vor 2 Wochen"), anstatt das volle Datumsformat zu     |
|             | verwenden.                                             |
+-------------+--------------------------------------------------------+
| `--graph`   | Zeigt ein ASCII-Diagramm des Branches an und verbindet |
|             | die Historie mit der Log-Ausgabe.                      |
+-------------+--------------------------------------------------------+
| `--pretty`  | Zeigt Commits in einem anderen Format an. Zu den       |
|             | Optionswerten gehören oneline, short, full, fuller und |
|             | format (womit Sie Ihr eigenes Format angeben können).  |
+-------------+--------------------------------------------------------+
| `--oneline` | Kurzform für die gleichzeitige Verwendung von          |
|             | `--pretty=oneline` und `--abbrev-commit`.              |
+-------------+--------------------------------------------------------+

: Allgemeine Optionen für `git log`

### Einschränken der Log-Ausgabe {#_einschränken_der_log_ausgabe}
solche Option bereits gesehen -- die Option `-2`, die nur die letzten
``` console
$ git log --since=2.weeks
```
::: note
:::
(umgangssprachlich als Git's „Pickel"-Option bezeichnet), die eine
``` console
$ git log -S function_name
```
``` console
$ git log -- path/to/file
```
+----------------------+-----------------------------------------------+
| Option               | Beschreibung                                  |
+======================+===============================================+
| `-<n>`               | Zeigt nur die letzten n Commits an            |
+----------------------+-----------------------------------------------+
| `--since`, `--after` | Begrenzt die angezeigten Commits auf die, die |
|                      | nach dem angegebenen Datum gemacht wurden.    |
+----------------------+-----------------------------------------------+
| `--until`,           | Begrenzt die angezeigten Commits auf die, die |
| `--before`           | vor dem angegebenen Datum gemacht wurden.     |
+----------------------+-----------------------------------------------+
| `--author`           | Zeigt nur Commits an, bei denen der           |
|                      | Autoren-Eintrag mit der angegebenen           |
|                      | Zeichenkette übereinstimmt.                   |
+----------------------+-----------------------------------------------+
| `--committer`        | Zeigt nur Commits an, bei denen der           |
|                      | Committer-Eintrag mit der angegebenen         |
|                      | Zeichenkette übereinstimmt.                   |
+----------------------+-----------------------------------------------+
| `--grep`             | Zeigt nur Commits an, deren                   |
|                      | Commit-Beschreibung die Zeichenkette enthält  |
+----------------------+-----------------------------------------------+
| `-S`                 | Zeigt nur Commits an, die solchen Code        |
|                      | hinzufügen oder entfernen, der mit der        |
|                      | Zeichenkette übereinstimmt                    |
+----------------------+-----------------------------------------------+

: Optionen zum Anpassen der Ausgabe von `git log`
in etwa folgendes aufrufen:[]{.indexterm primary="Log filtern"}

``` console
$ git log --pretty="%h - %s" --author='Junio C Hamano' --since="2008-10-01" \
   --before="2008-11-01" --no-merges -- t/
5610e3b - Fix testcase failure when extended attributes are in use
acd3b9e - Enhance hold_lock_file_for_{update,append}() API
f563754 - demonstrate breakage of detached checkout with symbolic link HEAD
d1a43f2 - reset --hard/read-tree --reset -u: remove unmerged new paths
51a94af - Fix "checkout --track -b newbranch" on detached HEAD
b0ad11e - pull: allow "git pull origin $something:$current_branch" into an unborn branch
```
::: tip
::: title
:::
:::
## Ungewollte Änderungen rückgängig machen {#_undoing}
``` console
$ git commit --amend
```
``` console
$ git commit -m 'Initial commit'
$ git add forgotten_file
$ git commit --amend
```
Sie erhalten am Ende einen einzigen Commit -- der zweite Commit ersetzt
::: note
Datei hinzuzufügen" oder „Verdammt, einen Tippfehler im letzten Commit
behoben" zu überladen.
:::
::: note
[ch03-git-branching.md](ch03-git-branching.md#_rebase_peril).
:::
### Eine Datei aus der Staging-Area entfernen {#_unstaging}
``` console
$ git add *
$ git status
On branch master
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)
    renamed:    README.md -> README
    modified:   CONTRIBUTING.md
```
Direkt unter dem Text „Changes to be committed", steht, dass man
``` console
$ git reset HEAD CONTRIBUTING.md
Unstaged changes after reset:
M   CONTRIBUTING.md
$ git status
On branch master
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)
    renamed:    README.md -> README
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)
    modified:   CONTRIBUTING.md
```
::: note
:::
### Änderung in einer modifizierten Datei zurücknehmen {#_änderung_in_einer_modifizierten_datei_zurücknehmen}
wieder ändern -- sie wieder so zurücksetzen, wie sie beim letzten Commit
``` console
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)
    modified:   CONTRIBUTING.md
```
``` console
$ git checkout -- CONTRIBUTING.md
$ git status
On branch master
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)
    renamed:    README.md -> README
```
::: important
vorgenommen haben, sind verloren -- Git hat diese Datei einfach durch
die zuletzt committete oder gestagten Version ersetzt. Verwenden Sie
diesen Befehl nur, wenn Sie sich absolut sicher sind, dass Sie diese
nicht gespeicherten lokalen Änderungen nicht benötigen.
:::
sollten wir das Stashing und Branching in Kapitel 3 -- [Git
Branching](ch03-git-branching.md#ch03-git-branching) durchgehen; das
### Änderungen Rückgängigmachen mit git restore {#undoing_git_restore}
#### Unstagen einer staged Datei mit git restore {#_unstagen_einer_staged_datei_mit_git_restore}
``` console
$ git add *
$ git status
On branch master
Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
    modified:   CONTRIBUTING.md
    renamed:    README.md -> README
```

Direkt unter dem Text „Changes to be committed" steht
``` console
$ git restore --staged CONTRIBUTING.md
$ git status
On branch master
Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
    renamed:    README.md -> README
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
    modified:   CONTRIBUTING.md
```
#### Rückgängig machen einer geänderten Datei mit git restore {#_rückgängig_machen_einer_geänderten_datei_mit_git_restore}
rückgängig machen --- sprich, sie so zurücksetzen, wie sie aussah, als
Sie sie zuletzt committet haben (oder ursprünglich geklont haben oder
wie auch immer Sie es in Ihr Arbeitsverzeichnis aufgenommen haben)?
``` console
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
    modified:   CONTRIBUTING.md
```
``` console
$ git restore CONTRIBUTING.md
$ git status
On branch master
Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
    renamed:    README.md -> README
```
::: important
:::
## Mit Remotes arbeiten {#_remote_repos}
::: note
::: title
:::
Es ist durchaus möglich, dass Sie mit einem „entfernten" Repository
dem Sie gerade arbeiten. Das Wort „remote" bedeutet nicht unbedingt,
:::
### Auflisten der Remotes {#_auflisten_der_remotes}
Befehl `git remote` aufrufen.[]{.indexterm primary="Git Befehle"
secondary="remote"} Er listet die Kurznamen der einzelnen von Ihnen
festgelegten Remote-Handles auf. Wenn Sie Ihr Repository geklont haben,
sollten Sie zumindest `origin` sehen -- das ist der Standardname, den
Git dem Server gibt, von dem Sie geklont haben:

``` console
$ git clone https://github.com/schacon/ticgit
Cloning into 'ticgit'...
remote: Reusing existing pack: 1857, done.
remote: Total 1857 (delta 0), reused 0 (delta 0)
Receiving objects: 100% (1857/1857), 374.35 KiB | 268.00 KiB/s, done.
Resolving deltas: 100% (772/772), done.
Checking connectivity... done.
$ cd ticgit
$ git remote
origin
```
``` console
$ git remote -v
origin  https://github.com/schacon/ticgit (fetch)
origin  https://github.com/schacon/ticgit (push)
```
``` console
$ cd grit
$ git remote -v
bakkdoor  https://github.com/bakkdoor/grit (fetch)
bakkdoor  https://github.com/bakkdoor/grit (push)
cho45     https://github.com/cho45/grit (fetch)
cho45     https://github.com/cho45/grit (push)
defunkt   https://github.com/defunkt/grit (fetch)
defunkt   https://github.com/defunkt/grit (push)
koke      git://github.com/koke/grit.git (fetch)
koke      git://github.com/koke/grit.git (push)
origin    git@github.com:mojombo/grit.git (fetch)
origin    git@github.com:mojombo/grit.git (push)
```
installieren](ch04-git-on-the-server.md#_getting_git_on_a_server).
### Hinzufügen von Remote-Repositorys {#_hinzufügen_von_remote_repositorys}
können Sie explizit einen neuen Remote hinzufügen.[]{.indexterm
primary="Git Befehle" secondary="remote"} Um ein neues
Remote-Git-Repository als Kurzname hinzuzufügen, auf das Sie leicht
verweisen können, führen Sie `git remote add <shortname> <url>` aus:

``` console
$ git remote
origin
$ git remote add pb https://github.com/paulboone/ticgit
$ git remote -v
origin  https://github.com/schacon/ticgit (fetch)
origin  https://github.com/schacon/ticgit (push)
pb  https://github.com/paulboone/ticgit (fetch)
pb  https://github.com/paulboone/ticgit (push)
```
``` console
$ git fetch pb
remote: Counting objects: 43, done.
remote: Compressing objects: 100% (36/36), done.
remote: Total 43 (delta 10), reused 31 (delta 5)
Unpacking objects: 100% (43/43), done.
From https://github.com/paulboone/ticgit
 * [new branch]      master     -> pb/master
 * [new branch]      ticgit     -> pb/ticgit
```

Pauls `master` Branch ist nun lokal als `pb/master` erreichbar -- Sie
Branching](ch03-git-branching.md#ch03-git-branching) detailliert
### Fetchen und Pullen von Ihren Remotes {#_fetching_and_pulling}
Remote-Projekten abrufen:[]{.indexterm primary="Git Befehle"
secondary="fetch"}
``` console
$ git fetch <remote>
```
Repository automatisch unter dem Namen „origin" hinzu. So holt
lokales Repository herunterlädt -- er mischt (engl. merged) sie nicht
und [Git Branching](ch03-git-branching.md#ch03-git-branching) für
weitere Informationen).[]{.indexterm primary="Git Befehle"
secondary="pull"} Das könnte ein einfacherer oder komfortablerer
::: note
:::
### Zu Ihren Remotes Pushen {#_pushing_remotes}
Befehl dafür ist einfach: `git push <remote> <branch>`.[]{.indexterm
primary="Git Befehle" secondary="push"} Wenn Sie Ihren `master` Branch
auf Ihren `origin` Server verschieben möchten (nochmals, das Klonen
richtet im Regelfall beide dieser Namen automatisch für Sie ein), dann
können Sie diesen Befehl auch nutzen, um alle Commits, die Sie
``` console
$ git push origin master
```
Branching](ch03-git-branching.md#ch03-git-branching) mit
### Inspizieren eines Remotes {#_inspecting_remote}
können Sie den Befehl `git remote show <remote>` verwenden.[]{.indexterm
primary="Git Befehle" secondary="remote"} Wenn Sie diesen Befehl mit
einem spezifischen Kurznamen ausführen, wie z.B. `origin`, erhalten Sie
eine ähnliche Meldung:

``` console
$ git remote show origin
* remote origin
  Fetch URL: https://github.com/schacon/ticgit
  Push  URL: https://github.com/schacon/ticgit
  HEAD branch: master
  Remote branches:
    master                               tracked
    dev-branch                           tracked
  Local branch configured for 'git pull':
    master merges with remote master
  Local ref configured for 'git push':
    master pushes to master (up to date)
```
``` console
$ git remote show origin
* remote origin
  URL: https://github.com/my-org/complex-project
  Fetch URL: https://github.com/my-org/complex-project
  Push  URL: https://github.com/my-org/complex-project
  HEAD branch: master
  Remote branches:
    master                           tracked
    dev-branch                       tracked
    markdown-strip                   tracked
    issue-43                         new (next fetch will store in remotes/origin)
    issue-45                         new (next fetch will store in remotes/origin)
    refs/remotes/origin/issue-11     stale (use 'git remote prune' to remove)
  Local branches configured for 'git pull':
    dev-branch merges with remote dev-branch
    master     merges with remote master
  Local refs configured for 'git push':
    dev-branch                     pushes to dev-branch                     (up to date)
    markdown-strip                 pushes to markdown-strip                 (up to date)
    master                         pushes to master                         (up to date)
```
### Umbenennen und Entfernen von Remotes {#_umbenennen_und_entfernen_von_remotes}
zu ändern.[]{.indexterm primary="Git Befehle" secondary="remote"} Wenn
Sie beispielsweise `pb` in `paul` umbenennen möchten, können Sie dieses
mit dem Befehl `git remote rename` machen:
``` console
$ git remote rename pb paul
$ git remote
origin
paul
```
Wenn Sie einen Remote aus irgendwelchen Gründen entfernen möchten -- Sie
länger oder ein Beitragender ist nicht mehr dabei -- dann können Sie
``` console
$ git remote remove paul
$ git remote
origin
```
## Taggen {#_git_tagging}
[]{.indexterm primary="Tags"} Wie die meisten VCSs hat Git die
Möglichkeit, bestimmte Punkte in der Historie eines Repositorys als
wichtig zu markieren. Normalerweise verwenden Leute diese
### Ihre Tags auflisten {#_ihre_tags_auflisten}
einfach `git tag` (mit optionalem `-l` oder `--list`) ein:[]{.indexterm
primary="Git Befehle" secondary="tag"}
``` console
$ git tag
v1.0
v2.0
```
``` console
$ git tag -l "v1.8.5*"
v1.8.5
v1.8.5-rc0
v1.8.5-rc1
v1.8.5-rc2
v1.8.5-rc3
v1.8.5.1
v1.8.5.2
v1.8.5.3
v1.8.5.4
v1.8.5.5
```

::: note
::: title
:::
:::
### Erstellen von Tags {#_erstellen_von_tags}
nicht ändert -- es ist nur ein Zeiger auf einen bestimmten Commit.
### Annotated Tags {#_annotated_tags}
[]{.indexterm primary="Tags" secondary="annotiert"} Das Erstellen eines
annotierten Tags in Git ist einfach. Der einfachste Weg ist die Eingabe
von `-a`, wenn Sie den Befehl `tag` ausführen:[]{.indexterm
primary="Git Befehle" secondary="tag"}
``` console
$ git tag -a v1.4 -m "my version 1.4"
$ git tag
v0.1
v1.3
v1.4
```
``` console
$ git show v1.4
tag v1.4
Tagger: Ben Straub <ben@straub.cc>
Date:   Sat May 3 20:19:12 2014 -0700
my version 1.4
commit ca82a6dff817ec66f44342007202690a93763949
Author: Scott Chacon <schacon@gee-mail.com>
Date:   Mon Mar 17 21:52:11 2008 -0700
    Change version number
```
### Lightweight Tags {#_lightweight_tags}

[]{.indexterm primary="Tags" secondary="lightweight"} Eine weitere
Möglichkeit, Commits zu markieren, ist ein leichtgewichtiger,
nicht-annotierter Tag. Das ist im Grunde genommen die in einer Datei
gespeicherte Commit-Prüfsumme -- es werden keine weiteren Informationen
gespeichert. Um einen leichtgewichtigen Tag zu erstellen, geben Sie
keine der Optionen `-a`, `-s` oder `-m` an, sondern nur einen Tag-Namen:

``` console
$ git tag v1.4-lw
$ git tag
v0.1
v1.3
v1.4
v1.4-lw
v1.5
```
zusätzlichen Tag-Informationen.[]{.indexterm primary="Git Befehle"
secondary="show"} Der Befehl zeigt nur den Commit an:
``` console
$ git show v1.4-lw
commit ca82a6dff817ec66f44342007202690a93763949
Author: Scott Chacon <schacon@gee-mail.com>
Date:   Mon Mar 17 21:52:11 2008 -0700
    Change version number
```
### Nachträgliches Tagging {#_nachträgliches_tagging}
``` console
$ git log --pretty=oneline
15027957951b64cf874c3557a0f3547bd83b3ff6 Merge branch 'experiment'
a6b4c97498bd301d84096da251c98a07c7723e65 Create write support
0d52aaab4479697da7686c15f77a3d64d9165190 One more thing
6d52a271eda8725415634dd79daabbc4d9b6008e Merge branch 'experiment'
0b7434d86859cc7b8c3d5e1dddfed66ff742fcbc Add commit function
4682c3261057305bdd616e23b64b0857d832627b Add todo file
166ae0c4d3f420721acbb115cc33848dfcc2121a Create write support
9fceb02d0ae598e95dc970b74767f19372d61af8 Update rakefile
964f16d36dfccde844893cac5b347e7b3d44abbc Commit the todo
8a5cbc430f1a9c3d00faaeffd07798508422908a Update readme
```
das bei dem Commit von „Update rakefile" vorlag. Sie können ihn
``` console
$ git tag -a v1.2 9fceb02
```
Sie sehen, dass Sie den Commit markiert haben:[]{.indexterm
primary="Git Befehle" secondary="tag"}
``` console
$ git tag
v0.1
v1.2
v1.3
v1.4
v1.4-lw
v1.5
$ git show v1.2
tag v1.2
Tagger: Scott Chacon <schacon@gee-mail.com>
Date:   Mon Feb 9 15:32:16 2009 -0800
version 1.2
commit 9fceb02d0ae598e95dc970b74767f19372d61af8
Author: Magnus Chacon <mchacon@gee-mail.com>
Date:   Sun Apr 27 20:43:35 2008 -0700
    Update rakefile
...
```
### Tags freigeben {#_sharing_tags}
Remote-Server.[]{.indexterm primary="Git Befehle" secondary="push"} Sie
müssen Tags explizit auf einen freigegebenen Server verschieben, nachdem
Sie sie erstellt haben. Dieser Prozess funktioniert genauso wie das
Freigeben von Remote-Branches -- Sie müssen dazu
`git push origin <tagname>` ausführen.

``` console
$ git push origin v1.5
Counting objects: 14, done.
Delta compression using up to 8 threads.
Compressing objects: 100% (12/12), done.
Writing objects: 100% (14/14), 2.05 KiB | 0 bytes/s, done.
Total 14 (delta 3), reused 0 (delta 0)
To git@github.com:schacon/simplegit.git
 * [new tag]         v1.5 -> v1.5
```
``` console
$ git push origin --tags
Counting objects: 1, done.
Writing objects: 100% (1/1), 160 bytes | 0 bytes/s, done.
Total 1 (delta 0), reused 0 (delta 0)
To git@github.com:schacon/simplegit.git
 * [new tag]         v1.4 -> v1.4
 * [new tag]         v1.4-lw -> v1.4-lw
```
::: note
::: title
:::
:::
### Tags löschen {#_tags_löschen}
``` console
$ git tag -d v1.4-lw
Deleted tag 'v1.4-lw' (was e7d5add)
```
``` console
$ git push origin :refs/tags/v1.4-lw
To /git@github.com:schacon/simplegit.git
 - [deleted]         v1.4-lw
```
``` console
$ git push origin --delete <tagname>
```
### Tags auschecken {#_tags_auschecken}
Ihr Repository in den Zustand „detached HEAD" (dt. losgelöst) versetzt,
``` console
$ git checkout v2.0.0
Note: switching to 'v2.0.0'.
You are in 'detached HEAD' state. You can look around, make experimental
changes and commit them, and you can discard any commits you make in this
state without impacting any branches by performing another checkout.
If you want to create a new branch to retain commits you create, you may
do so (now or later) by using -c with the switch command. Example:
  git switch -c <new-branch-name>
Or undo this operation with:
  git switch -
Turn off this advice by setting config variable advice.detachedHead to false
HEAD is now at 99ada87... Merge pull request #89 from schacon/appendix-final
$ git checkout v2.0-beta-0.1
Previous HEAD position was 99ada87... Merge pull request #89 from schacon/appendix-final
HEAD is now at df3f601... Add atlas.json and cover image
```
Wenn Sie im Zustand „losgelöster HEAD" Änderungen vornehmen und dann
Commit-Hash. Wenn Sie also Änderungen vornehmen müssen -- z.B. wenn Sie
einen Fehler in einer älteren Version beheben -- sollten Sie im
Regelfall einen Branch erstellen:
``` console
$ git checkout -b version2 v2.0.0
Switched to a new branch 'version2'
```
## Git Aliases {#_git_aliases}
[]{.indexterm primary="Aliasnamen"} Bevor wir dieses Kapitel über Basic
Git abschließen, gibt es noch einen kurzen Tipp, der Ihre Arbeit mit Git
einfacher, leichter und verständlicher machen kann: Aliase. Der Klarheit
halber werden wir sie nirgendwo anders in diesem Buch verwenden, aber
wenn Sie Git in Zukunft regelmäßig verwenden, dann sind Aliase etwas,
das Sie kennen sollten.
einfach ein Alias für jeden Befehl einrichten.[]{.indexterm
primary="Git Befehle" secondary="config"} Hier sind ein paar Beispiele,
die Sie einrichten sollten:
``` console
$ git config --global alias.co checkout
$ git config --global alias.br branch
$ git config --global alias.ci commit
$ git config --global alias.st status
```
``` console
$ git config --global alias.unstage 'reset HEAD --'
```
``` console
$ git unstage fileA
$ git reset HEAD -- fileA
```
``` console
$ git config --global alias.last 'log -1 HEAD'
```
``` console
$ git last
commit 66938dae3329c7aebe598c2246a8e6af90d04646
Author: Josh Goebel <dreamer3@example.com>
Date:   Tue Aug 26 19:48:51 2008 +0800
    Test for current head
    Signed-off-by: Scott Chacon <schacon@example.com>
```
``` console
$ git config --global alias.visual '!gitk'
```
## Zusammenfassung {#_zusammenfassung}
mit der „Killer-Funktion" von Git befassen: dem Branching-Modell.