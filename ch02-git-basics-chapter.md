# Git Grundlagen {#ch02-git-basics-chapter}

Wenn Sie nur ein Kapitel durcharbeiten können/wollen, um mit Git zu
beginnen, dann ist dieses hier das richtige. Dieses Kapitel behandelt
alle grundlegenden Befehle, die Sie benötigen, um die überwiegende
Anzahl der Aufgaben zu erledigen, die Sie irgendwann einmal mit Git
erledigen werden. Am Ende des Kapitels sollten Sie in der Lage sein, ein
neues Repository anzulegen und zu konfigurieren, Dateien zu versionieren
bzw. aus der Versionsverwaltung zu entfernen, Dateien in die
Staging-Area hinzuzufügen und schließlich einen Commit durchzuführen.
Außerdem wird gezeigt, wie Sie Git so konfigurieren können, dass es
bestimmte Dateien und Dateimuster ignoriert, wie Sie Fehler schnell und
einfach rückgängig machen, wie Sie die Historie eines Projekts
durchsuchen und Änderungen zwischen Commits nachschlagen, und wie Sie
von einem Remote-Repository Daten herunter- bzw. dort hochladen können.

## Ein Git-Repository anlegen {#_getting_a_repo}

Sie haben zwei Möglichkeiten, ein Git-Repository auf Ihrem Rechner
anzulegen.

1.  Sie können ein lokales Verzeichnis, das sich derzeit nicht unter
    Versionskontrolle befindet, in ein Git-Repository verwandeln, oder

2.  Sie können ein bestehendes Git-Repository von einem anderen Ort aus
    *klonen*.

In beiden Fällen erhalten Sie ein einsatzbereites Git-Repository auf
Ihrem lokalen Rechner.

### Ein Repository in einem bestehenden Verzeichnis einrichten {#_ein_repository_in_einem_bestehenden_verzeichnis_einrichten}

Wenn Sie ein Projektverzeichnis haben, das sich derzeit nicht unter
Versionskontrolle befindet, und Sie mit der Kontrolle über Git beginnen
möchten, müssen Sie zunächst in das Verzeichnis dieses Projekts
wechseln. Wenn Sie dies noch nie getan haben, sieht es je nachdem,
welches System Sie verwenden, etwas anders aus:

für Linux:

``` console
$ cd /home/user/my_project
```

für macOS:

``` console
$ cd /Users/user/my_project
```

für Windows:

``` console
$ cd C:/Users/user/my_project
```

Führen Sie dort folgenden Befehl aus:

``` console
$ git init
```

Der Befehl erzeugt ein Unterverzeichnis `.git`, in dem alle relevanten
Git-Repository-Daten enthalten sind, also ein Git-Repository
Grundgerüst. Zu diesem Zeitpunkt werden noch keine Dateien in Git
versioniert. In Kapitel 10, [Git
Interna](ch10-git-internals.xml#ch10-git-internals), finden Sie weitere
Informationen, welche Dateien im `.git` Verzeichnis enthalten sind und
was ihre Aufgabe ist.[]{.indexterm primary="Git Befehle"
secondary="init"}

Wenn Sie bereits existierende Dateien versionieren möchten (und es sich
nicht nur um ein leeres Verzeichnis handelt), dann sollten Sie den
aktuellen Stand in einem initialen Commit starten. Mit dem Befehl
`git add` legen Sie fest, welche Dateien versioniert werden sollen und
mit dem Befehl `git commit` erzeugen Sie einen neuen Commit:

``` console
$ git add *.c
$ git add LICENSE
$ git commit -m 'Initial project version'
```

Wir werden gleich noch einmal genauer auf diese Befehle eingehen. Im
Moment ist nur wichtig, dass Sie verstehen, dass Sie jetzt ein
Git-Repository erzeugt und einen ersten Commit angelegt haben.

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
Serverfestplatte beschädigt wird, können Sie nahezu jeden der Klone auf
irgendeinem Client verwenden, um den Server wieder in den Zustand
zurückzusetzen, in dem er sich zum Zeitpunkt des Klonens befand. (Sie
werden vielleicht einige serverseitige Hooks und dergleichen verlieren,
aber alle versionierten Daten wären vorhanden -- siehe Kapitel 4, [Git
auf dem Server](ch04-git-on-the-server.md#_getting_git_on_a_server),
für weitere Details.)

Sie können ein Repository mit dem Befehl `git clone [url]`
klonen.[]{.indexterm primary="Git Befehle" secondary="clone"} Um
beispielsweise das Repository der verlinkbaren Git-Bibliothek `libgit2`
zu klonen, führen Sie den folgenden Befehl aus:

``` console
$ git clone https://github.com/libgit2/libgit2
```

Git legt dann ein Verzeichnis `libgit2` an, initialisiert in diesem ein
`.git` Verzeichnis, lädt alle Daten des Repositorys herunter und checkt
eine Arbeitskopie der aktuellsten Version aus. Wenn Sie in das neue
`libgit2` Verzeichnis wechseln, finden Sie dort die Projektdateien und
können gleich damit arbeiten.

Wenn Sie das Repository in ein Verzeichnis mit einem anderen Namen als
`libgit2` klonen möchten, können Sie das wie folgt durchführen:

``` console
$ git clone https://github.com/libgit2/libgit2 mylibgit
```

Dieser Befehl tut das Gleiche wie der vorhergehende, aber das
Zielverzeichnis lautet diesmal `mylibgit`.

Git unterstützt eine Reihe unterschiedlicher Übertragungsprotokolle. Das
vorhergehende Beispiel verwendet das `https://` Protokoll, aber Ihnen
können auch die Angaben `git://` oder `user@server:path/to/repo.git`
begegnen, welches das SSH-Transfer-Protokoll verwendet. Kapitel 4, [Git
auf dem Server](ch04-git-on-the-server.md#_getting_git_on_a_server),
stellt alle verfügbaren Optionen vor, die der Server für den Zugriff auf
Ihr Git-Repository hat und die Vor- und Nachteile der einzelnen
Optionen, die man einrichten kann.

## Änderungen nachverfolgen und im Repository speichern {#_änderungen_nachverfolgen_und_im_repository_speichern}

An dieser Stelle sollten Sie ein *originalgetreues* Git-Repository auf
Ihrem lokalen Computer und eine Checkout- oder Arbeitskopie aller seiner
Dateien vor sich haben. Normalerweise werden Sie damit beginnen wollen,
Änderungen vorzunehmen und Schnappschüsse dieser Änderungen in Ihr
Repository zu übertragen, wenn das Projekt so weit fortgeschritten ist,
dass Sie es sichern möchten.

Denken Sie daran, dass sich jede Datei in Ihrem Arbeitsverzeichnis in
einem von zwei Zuständen befinden kann: *tracked* oder *untracked* --
Änderungen an der Datei werden verfolgt (engl. *tracked*) oder eben
nicht (engl. *untracked*). Tracked Dateien sind Dateien, die im letzten
Snapshot enthalten sind. Genauso wie alle neuen Dateien in der
Staging-Area. Sie können entweder unverändert, modifiziert oder für den
nächsten Commit vorgemerkt (staged) sein. Kurz gesagt, nachverfolgte
Dateien sind Dateien, die Git kennt.

Alle anderen Dateien in Ihrem Arbeitsverzeichnis dagegen, sind nicht
versioniert: das sind all diejenigen Dateien, die nicht schon im letzten
Schnappschuss enthalten waren und die sich nicht in der Staging-Area
befinden. Wenn Sie ein Repository zum ersten Mal klonen, sind alle
Dateien versioniert und unverändert. Nach dem Klonen wurden sie ja
ausgecheckt und bis dahin haben Sie auch noch nichts an ihnen verändert.

Sobald Sie anfangen, versionierte Dateien zu bearbeiten, erkennt Git
diese als modifiziert, weil sie sich im Vergleich zum letzten Commit
verändert haben. Die geänderten Dateien können Sie dann für den nächsten
Commit vormerken und schließlich alle Änderungen, die sich in der
Staging-Area befinden, einchecken/committen. Danach wiederholt sich der
Zyklus.

![Der Status Ihrer Dateien im Überblick](images/lifecycle.png)

### Zustand von Dateien prüfen {#_checking_status}

Das wichtigste Hilfsmittel, um den Zustand zu überprüfen, in dem sich
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

Dieser Zustand wird auch als sauberes Arbeitsverzeichnis (engl. clean
working directory) bezeichnet. Mit anderen Worten, es gibt keine
Dateien, die unter Versionsverwaltung stehen und seit dem letzten Commit
geändert wurden -- andernfalls würden sie hier aufgelistet werden.
Außerdem teilt Ihnen der Befehl mit, auf welchem Branch Sie gerade
arbeiten und informiert Sie darüber, dass dieser sich im Vergleich zum
Branch auf dem Server nicht verändert hat. Momentan ist dieser Zweig
immer `master`, was der Vorgabe entspricht; Sie müssen sich jetzt nicht
darum kümmern. Wir werden im Kapitel [Git
Branching](ch03-git-branching.md#ch03-git-branching) auf Branches
detailliert eingehen.

::: note
GitHub änderte Mitte 2020 den Standard-Branch-Namen von `master` in
`main`, und andere Git-Hosts folgten diesem Beispiel. Daher werden Sie
möglicherweise feststellen, dass der Standard-Branch-Name in einigen neu
erstellten Repositories `main` und nicht `master` ist. Außerdem kann der
Standard-Branch-Name geändert werden (wie Sie in
[ch01-getting-started.md](ch01-getting-started.md#_new_default_branch)
gesehen haben), sodass Sie möglicherweise einen anderen Namen für den
Standard-Branch vorfinden.

Git selbst verwendet jedoch immer noch `master` als Standard, also
werden wir auch es im gesamten Buch verwenden.
:::

Nehmen wir einmal an, Sie fügen eine neue Datei mit dem Namen `README`
zu Ihrem Projekt hinzu. Wenn die Datei zuvor nicht existiert hat und Sie
jetzt `git status` ausführen, zeigt Git die bisher nicht versionierte
Datei wie folgt an:

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
sind Dateien, die bisher noch nicht versioniert sind. Dort wird jetzt
auch die Datei `README` angezeigt. Mit anderen Worten, die Datei README
wird in diesem Bereich gelistet, weil sie im letzten Schnappschuss nicht
enthalten war und noch nicht gestaged wurde. Git nimmt eine solche Datei
nicht automatisch in die Versionsverwaltung auf, sondern Sie müssen Git
dazu ausdrücklich auffordern. Ansonsten würden generierte Binärdateien
oder andere Dateien, die Sie nicht in Ihrem Repository haben möchten,
automatisch hinzugefügt werden. Das möchte man in den meisten Fällen
vermeiden. Jetzt wollen wir aber Änderungen an der Datei `README`
verfolgen und fügen sie deshalb zur Versionsverwaltung hinzu.

### Neue Dateien zur Versionsverwaltung hinzufügen {#_tracking_files}

Um eine neue Datei zu versionieren, können Sie den Befehl `git add`
verwenden.[]{.indexterm primary="Git Befehle" secondary="add"} Für Ihre
neue `README` Datei, können Sie folgendes ausführen:

``` console
$ git add README
```

Wenn Sie erneut den Befehl `git status` ausführen, werden Sie sehen,
dass sich Ihre `README` Datei jetzt unter Versionsverwaltung befindet
und für den nächsten Commit vorgemerkt ist:

``` console
$ git status
On branch master
Your branch is up-to-date with 'origin/master'.
Changes to be committed:
  (use "git restore --staged <file>..." to unstage)

    new file:   README
```

Sie können erkennen, dass die Datei für den nächsten Commit vorgemerkt
ist, weil sie unter der Rubrik „Changes to be committed" aufgelistet
ist. Wenn Sie jetzt einen Commit anlegen, wird der Schnappschuss den
Zustand der Datei festhalten, den sie zum Zeitpunkt des Befehls
`git add` hat. Sie erinnern sich vielleicht daran, wie Sie vorhin
`git init` und anschließend `git add <files>` ausgeführt haben. Mit
diesen Befehlen haben Sie die Dateien in Ihrem Verzeichnis zur
Versionsverwaltung hinzugefügt.[]{.indexterm primary="Git Befehle"
secondary="init"}[]{.indexterm primary="Git Befehle" secondary="add"}
Der Befehl `git add` akzeptiert einen Pfadnamen einer Datei oder eines
Verzeichnisses. Wenn Sie ein Verzeichnis angeben, fügt `git add` alle
Dateien in diesem Verzeichnis und allen Unterverzeichnissen rekursiv
hinzu.

### Geänderte Dateien zur Staging-Area hinzufügen {#_geänderte_dateien_zur_staging_area_hinzufügen}

Lassen Sie uns jetzt eine bereits versionierte Datei ändern. Wenn Sie
zum Beispiel eine bereits unter Versionsverwaltung stehende Datei mit
dem Dateinamen `CONTRIBUTING.md` ändern und danach den Befehl
`git status` erneut ausführen, erhalten Sie in etwa folgende Ausgabe:

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

Die Datei `CONTRIBUTING.md` erscheint im Abschnitt „Changes not staged
for commit". Das bedeutet, dass eine versionierte Datei im
Arbeitsverzeichnis verändert worden ist, aber noch nicht für den Commit
vorgemerkt wurde. Um sie vorzumerken, führen Sie den Befehl `git add`
aus. Der Befehl `git add` wird zu vielen verschiedenen Zwecken
eingesetzt. Man verwendet ihn, um neue Dateien zur Versionsverwaltung
hinzuzufügen, Dateien für einen Commit vorzumerken und verschiedene
andere Dinge -- beispielsweise einen Konflikt aus einem Merge als
aufgelöst zu kennzeichnen. Leider wird der Befehl `git add` oft
missverstanden. Viele assoziieren damit, dass damit Dateien zum Projekt
hinzugefügt werden. Wie Sie aber gerade gelernt haben, wird der Befehl
auch noch für viele andere Dinge eingesetzt. Wenn Sie den Befehl
`git add` einsetzen, sollten Sie das eher so sehen, dass Sie damit einen
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

Beide Dateien sind nun für den nächsten Commit vorgemerkt. Nehmen wir
an, Sie wollen jetzt aber noch eine weitere Änderung an der Datei
`CONTRIBUTING.md` vornehmen, bevor Sie den Commit tatsächlich starten.
Sie öffnen die Datei erneut, ändern sie entsprechend ab und eigentlich
wären Sie ja jetzt bereit den Commit durchzuführen. Allerdings lassen
Sie uns vorher noch einmal den Befehl `git status` ausführen:

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
stattdessen die gegenwärtige Version im Commit haben möchten, müssen Sie
erneut `git add` ausführen, um die Datei der Staging-Area hinzuzufügen:

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

Die Ausgabe von `git status` ist sehr umfassend und auch ziemlich
wortreich. Git hat auch ein Kurzformat, mit dem Sie Ihre Änderungen
kompakter sehen können. Wenn Sie `git status -s` oder
`git status --short` ausführen, erhalten Sie eine kürzere Darstellung
des Befehls:

``` console
$ git status -s
 M README
MM Rakefile
A  lib/git.rb
M  lib/simplegit.rb
?? LICENSE.txt
```

Neue Dateien, die nicht versioniert werden, haben `??` neben sich, neue
Dateien, die der Staging-Area hinzugefügt wurden, haben ein `A`,
geänderte Dateien haben ein `M` usw. Es gibt zwei Spalten für die
Ausgabe -- die linke Spalte zeigt den Status der Staging-Area und die
rechte Spalte den Status des Verzeichnisbaums. So ist beispielsweise in
der Bildschirmausgabe oben die Datei `README` im Arbeitsverzeichnis
geändert, aber noch nicht staged, während die Datei `lib/simplegit.rb`
geändert und staged ist. Das `Rakefile` wurde modifiziert, staged und
dann wieder modifiziert, so dass es Änderungen an ihm gibt, die sowohl
staged als auch unstaged sind.

### Ignorieren von Dateien {#_ignoring}

Häufig gibt es eine Reihe von Dateien, die Git nicht automatisch
hinzufügen oder schon gar nicht als „nicht versioniert" (eng. untracked)
anzeigen soll. Dazu gehören in der Regel automatisch generierte Dateien,
wie Log-Dateien oder Dateien, die von Ihrem Build-System erzeugt werden.
In solchen Fällen können Sie die Datei `.gitignore` erstellen, die eine
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
Codegenerierung sein könnten. Die zweite Zeile weist Git an, alle
Dateien zu ignorieren, deren Name mit einer Tilde (`~`) enden, was von
vielen Texteditoren wie Emacs zum Markieren temporärer Dateien verwendet
wird. Sie können auch ein Verzeichnis „log", „tmp" oder „pid"
hinzufügen, eine automatisch generierte Dokumentation usw. Es ist im
Allgemeinen eine gute Idee, die `.gitignore` Datei für Ihr neues
Repository einzurichten, noch bevor Sie loslegen. So können Sie nicht
versehentlich Dateien committen, die Sie wirklich nicht in Ihrem
Git-Repository haben möchten.

Die Richtlinien für Vergleichsmuster, die Sie in der Datei `.gitignore`
eingeben können, lauten wie folgt:

-   Leerzeilen oder Zeilen, die mit `#` beginnen, werden ignoriert.

-   Standard-Platzhalter-Zeichen funktionieren und werden rekursiv im
    gesamten Verzeichnisbaum angewendet.

-   Sie können Vergleichsmuster mit einem Schrägstrich (`/`)
    **beginnen**, um die Rekursivität zu verhindern.

-   Sie können Vergleichsmuster mit einem Schrägstrich (`/`)
    **beenden**, um ein Verzeichnis anzugeben.

-   Sie können ein Vergleichsmuster verbieten, indem Sie es mit einem
    Ausrufezeichen (`!`) beginnen.

Platzhalter-Zeichen sind wie einfache, reguläre Ausdrücke, die von der
Shell genutzt werden. Ein Sternchen (`*`) entspricht null oder mehr
Zeichen; `[abc]` entspricht jedem Zeichen innerhalb der eckigen Klammern
(in diesem Fall a, b oder c); ein Fragezeichen (`?`) entspricht einem
einzelnen Zeichen und eckige Klammern, die durch einen Bindestrich
(`[0-9]`) getrennte Zeichen einschließen, passen zu jedem Zeichen
dazwischen (in diesem Fall von 0 bis 9). Sie können auch zwei Sterne
verwenden, um verschachtelte Verzeichnisse abzugleichen; `a/**/z` würde
zu `a/z`, `a/b/z`, `a/b/c/z` usw. passen.

Hier ist eine weitere `.gitignore` Beispieldatei:

    # ignore all .a files
    *.a

    # but do track lib.a, even though you're ignoring .a files above
    !lib.a

    # only ignore the TODO file in the current directory, not subdir/TODO
    /TODO

    # ignore all files in any directory named build
    build/

    # ignore doc/notes.txt, but not doc/server/arch.txt
    doc/*.txt

    # ignore all .pdf files in the doc/ directory and any of its subdirectories
    doc/**/*.pdf

::: tip
GitHub unterhält eine ziemlich umfassende Liste guter `.gitignore`
Beispiel-Dateien für Dutzende von Projekten und Sprachen auf
<https://github.com/github/gitignore>, falls Sie einen Ansatzpunkt für
Ihr Projekt suchen.
:::

::: note
Im einfachsten Fall kann ein Repository eine einzelne `.gitignore` Datei
in seinem Root-Verzeichnis haben, die rekursiv für das gesamte
Repository gilt. Es ist aber auch möglich, weitere `.gitignore` Dateien
in Unterverzeichnissen anzulegen. Die Regeln dieser verschachtelten
`.gitignore` Dateien gelten nur für die in dem Verzeichnis (und
unterhalb) liegenden Dateien. Das Linux-Kernel-Source-Repository hat
beispielsweise 206 `.gitignore` Dateien.

Es würde den Rahmen dieses Buches sprengen, detaillierter auf den
Einsatz mehrerer `.gitignore` Dateien einzugehen; siehe die Manpage
`man gitignore` für weitere Informationen.
:::

### Überprüfen der Staged- und Unstaged-Änderungen {#_git_diff_staged}

Wenn der Befehl `git status` für Sie zu vage ist -- Sie wollen genau
wissen, **was** Sie geändert haben, nicht nur welche Dateien geändert
wurden -- können Sie den Befehl `git diff` verwenden.[]{.indexterm
primary="Git Befehle" secondary="diff"} Wir werden `git diff` später
ausführlicher behandeln, aber Sie werden es wahrscheinlich am häufigsten
verwenden, um diese beiden Fragen zu beantworten: Was hat sich geändert,
ist aber noch nicht zum Commit vorgemerkt (engl. staged)? Und was haben
Sie zum Commit vorgemerkt und können es demnächst committen? Der Befehl
`git status` beantwortet diese Fragen ganz allgemein, indem er die
Dateinamen auflistet; `git diff` zeigt Ihnen aber genau die
hinzugefügten und entfernten Zeilen -- sozusagen den Patch.

Nehmen wir an, Sie bearbeiten und merken die Datei `README` zum Commit
vor (engl. stage) und bearbeiten dann die Datei `CONTRIBUTING.md`, ohne
sie zu „stagen". Wenn Sie den Befehl `git status` ausführen, sehen Sie
erneut so etwas wie das hier:

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

Um die Änderungen zu sehen, die Sie noch nicht zum Commit vorgemerkt
haben, geben Sie den Befehl `git diff`, ohne weitere Argumente, ein:

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

Dieses Kommando vergleicht, was sich in Ihrem Arbeitsverzeichnis
befindet, mit dem, was sich in Ihrer Staging-Area befindet. Das Ergebnis
gibt Ihnen an, welche Änderungen Sie vorgenommen haben, die noch nicht
„gestaged" sind.

Wenn Sie wissen wollen, was Sie zum Commit vorgemerkt haben, das in
Ihren nächsten Commit einfließt, können Sie `git diff --staged`
verwenden. Dieser Befehl vergleicht Ihre zum Commit vorgemerkten
Änderungen mit Ihrem letzten Commit:

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

Es ist wichtig zu wissen, dass `git diff` von sich aus nicht alle
Änderungen seit Ihrem letzten Commit anzeigt -- nur die Änderungen, die
noch „unstaged" sind. Wenn Sie alle Ihre Änderungen bereits „gestaged"
haben, wird `git diff` Ihnen keine Antwort geben.

Ein weiteres Beispiel: wenn Sie die Datei `CONTRIBUTING.md` zum Commit
vormerken und dann wieder bearbeiten, können Sie mit `git diff` die
Änderungen in der „staged"-Datei und die „unstaged"-Änderungen sehen.
Wenn Sie folgendes gemacht haben

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

können Sie jetzt mit `git diff` sehen, was noch nicht zum Commit
vorgemerkt ist

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

und `git diff --cached` zeigt Ihnen, was Sie bisher zum Commit
vorgemerkt haben (`--staged` und `--cached` sind Synonyme, sie bewirken
das Gleiche):

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
Git Diff mit einem externen Tool
:::

Wir werden den Befehl `git diff` im weiteren Verlauf des Buches auf
vielfältige Weise verwenden. Es gibt eine weitere Methode, diese Diffs
zu betrachten, wenn Sie lieber ein graphisches oder externes
Diff-Viewing-Programm bevorzugen. Wenn Sie `git difftool` anstelle von
`git diff` verwenden, können Sie alle diese Unterschiede in einer
Software wie emerge, vimdiff und viele andere (einschließlich
kommerzieller Produkte) anzeigen lassen. Führen Sie den Befehl
`git difftool --tool-help` aus, um zu sehen, was auf Ihrem System
verfügbar ist.
:::

### Die Änderungen committen {#_committing_changes}

Nachdem Ihre Staging-Area nun so eingerichtet ist, wie Sie es wünschen,
können Sie Ihre Änderungen committen. Denken Sie daran, dass alles, was
noch nicht zum Commit vorgemerkt ist -- alle Dateien, die Sie erstellt
oder geändert haben und für die Sie seit Ihrer Bearbeitung nicht mehr
`git add` ausgeführt haben -- nicht in diesen Commit einfließen werden.
Sie bleiben aber als geänderte Dateien auf Ihrer Festplatte erhalten. In
diesem Beispiel nehmen wir an, dass Sie beim letzten Mal, als Sie
`git status` ausgeführt haben, gesehen haben, dass alles zum Commit
vorgemerkt wurde und bereit sind, Ihre Änderungen zu
committen.[]{.indexterm primary="Git Befehle" secondary="status"} Am
einfachsten ist es, wenn Sie `git commit` eingeben:[]{.indexterm
primary="Git Befehle" secondary="commit"}

``` console
$ git commit
```

Dadurch wird der Editor Ihrer Wahl gestartet.

::: note
Das wird durch die Umgebungsvariable `EDITOR` Ihrer Shell festgelegt --
normalerweise Vim oder Emacs. Sie können den Editor aber auch mit dem
Befehl `git config --global core.editor` beliebig konfigurieren, wie Sie
es in Kapitel 1, [Erste
Schritte](ch01-getting-started.md#ch01-getting-started) gesehen
haben.[]{.indexterm primary="Editor"
secondary="ändern der Voreinstellung"}[]{.indexterm
primary="Git Befehle" secondary="config"}
:::

Der Editor zeigt den folgenden Text an (dieses Beispiel ist eine
Vim-Ansicht):

    # Please enter the commit message for your changes. Lines starting
    # with '#' will be ignored, and an empty message aborts the commit.
    # On branch master
    # Your branch is up-to-date with 'origin/master'.
    #
    # Changes to be committed:
    #   new file:   README
    #   modified:   CONTRIBUTING.md
    #
    ~
    ~
    ~
    ".git/COMMIT_EDITMSG" 9L, 283C

Sie können erkennen, dass die Standard-Commit-Meldung die neueste
Ausgabe des auskommentierten Befehls `git status` und eine leere Zeile
darüber enthält. Sie können diese Kommentare entfernen und Ihre
Commit-Nachricht eingeben oder Sie können sie dort stehen lassen, damit
Sie sich merken können, was Sie committen.

::: note
Für eine noch bessere Gedächtnisstütze über das, was Sie geändert haben,
können Sie die Option `-v` an `git commit` übergeben. Dadurch wird auch
die Differenz Ihrer Änderung in den Editor geschrieben, so dass Sie
genau sehen können, welche Änderungen Sie committen.
:::

Wenn Sie den Editor verlassen, erstellt Git Ihren Commit mit dieser
Commit-Nachricht (mit den Kommentaren und ausgeblendetem Diff).

Alternativ können Sie Ihre Commit-Nachricht auch inline mit dem Befehl
`commit -m` eingeben. Das Flag `-m` ermöglicht die direkte Eingabe eines
Kommentartextes:

``` console
$ git commit -m "Story 182: fix benchmarks for speed"
[master 463dc4f] Story 182: fix benchmarks for speed
 2 files changed, 2 insertions(+)
 create mode 100644 README
```

Jetzt haben Sie Ihren ersten Commit erstellt! Sie können sehen, dass der
Commit eine Nachricht über sich selbst ausgegeben hat: in welchen Branch
Sie committet haben (`master`), welche SHA-1-Prüfsumme der Commit hat
(`463dc4f`), wie viele Dateien geändert wurden und Statistiken über
hinzugefügte und entfernte Zeilen im Commit.

Denken Sie daran, dass der Commit den Snapshot aufzeichnet, den Sie in
Ihrer Staging-Area eingerichtet haben. Alles, was von Ihnen nicht zum
Commit vorgemerkt wurde, liegt immer noch als modifiziert da. Sie können
einen weiteren Commit durchführen, um es zu Ihrer Historie hinzuzufügen.
Jedes Mal, wenn Sie einen Commit ausführen, zeichnen Sie einen
Schnappschuss Ihres Projekts auf, auf den Sie zurückgreifen oder mit
einem späteren Zeitpunkt vergleichen können.

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

Beachten Sie, dass Sie in diesem Fall `git add` nicht für die Datei
`CONTRIBUTING.md` ausführen müssen, bevor Sie committen. Das liegt
daran, dass das `-a`-Flag alle geänderten Dateien einschließt. Das ist
bequem, aber seien Sie vorsichtig. Manchmal führt dieses Flag dazu, dass
Sie ungewollte Änderungen vornehmen.

### Dateien löschen {#_removing_files}

[]{.indexterm primary="Dateien" secondary="entfernen"} Um eine Datei aus
Git zu entfernen, müssen Sie sie aus der Versionsverwaltung entfernen
(genauer gesagt, aus Ihrem Staging-Bereich löschen) und dann committen.
Der Befehl `git rm` erledigt das und entfernt die Datei auch aus Ihrem
Arbeitsverzeichnis, so dass Sie sie beim nächsten Mal nicht mehr als
„untracked"-Datei sehen.

Wenn Sie die Datei einfach aus Ihrem Arbeitsverzeichnis entfernen,
erscheint sie unter dem „Changes not staged for commit"-Bereich (das ist
die *unstaged*-Area) Ihrer `git status` Ausgabe:

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

Wenn Sie dann `git rm` ausführen, wird die Entfernung der Datei zum
Commit vorgemerkt:

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

Wenn Sie das nächste Mal einen Commit ausführen, ist die Datei weg und
ist nicht mehr versioniert (engl. tracked). Wenn Sie die Datei geändert
oder bereits zur Staging-Area hinzugefügt haben, müssen Sie das
Entfernen mit der Option `-f` erzwingen. Hierbei handelt es sich um eine
Sicherheitsfunktion, die ein versehentliches Entfernen von Dateien
verhindert, die noch nicht in einem Snapshot aufgezeichnet wurden und
die nicht von Git wiederhergestellt werden können.

Eine weitere nützliche Sache, die Sie möglicherweise nutzen möchten,
ist, die Datei in Ihrem Verzeichnisbaum zu behalten, sie aber aus Ihrer
Staging-Area zu entfernen. Mit anderen Worten, Sie können die Datei auf
Ihrer Festplatte behalten, aber nicht mehr von Git
protokollieren/versionieren lassen.

Das ist besonders dann nützlich, wenn Sie vergessen haben, etwas zu
Ihrer Datei `.gitignore` hinzuzufügen und dies versehentlich „gestaged"
haben (eine große Logdatei z.B. oder eine Reihe von .a-kompilierten
Dateien). Das erreichen Sie mit der Option `--cached`:

``` console
$ git rm --cached README
```

Sie können Dateien, Verzeichnisse und Platzhalter-Zeichen an den Befehl
`git rm` übergeben. Das bedeutet, dass Sie folgende Möglichkeit haben:

``` console
$ git rm log/\*.log
```

Beachten Sie den Backslash (`\`) vor dem `*`. Der ist notwendig, weil
Git zusätzlich zur Dateinamen-Erweiterung Ihrer Shell eine eigene
Dateinamen-Erweiterung vornimmt. Mit dem Befehl oben werden alle Dateien
entfernt, die die Erweiterung `.log` im Verzeichnis `log/` haben. Oder
Sie können Folgendes ausführen:

``` console
$ git rm \*~
```

Dieses Kommando entfernt alle Dateien, deren Name mit einer `~` endet.

### Dateien verschieben {#_git_mv}

[]{.indexterm primary="Dateien" secondary="verschieben"} Im Gegensatz zu
vielen anderen VCS-Systemen verfolgt (engl. track) Git das Verschieben
von Dateien nicht ausdrücklich. Wenn Sie eine Datei in Git umbenennen,
werden keine Metadaten in Git gespeichert, die dem System mitteilen,
dass Sie die Datei umbenannt haben. Allerdings ist Git ziemlich clever,
das im Nachhinein herauszufinden -- wir werden uns etwas später mit der
Erkennung von Datei-Verschiebungen befassen.

Daher ist es etwas verwirrend, dass Git einen Befehl `mv` vorweisen
kann. Wenn Sie eine Datei in Git umbenennen möchten, können Sie
beispielsweise Folgendes ausführen:

``` console
$ git mv file_from file_to
```

Das funktioniert gut. Tatsache ist, wenn Sie so einen Befehl ausführen
und sich den Status ansehen, werden Sie sehen, dass Git es für eine
umbenannte Datei hält:

``` console
$ git mv README.md README
$ git status
On branch master
Your branch is up-to-date with 'origin/master'.
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

    renamed:    README.md -> README
```

Unabhängig davon, ist dieser Befehl zu dem Folgenden gleichwertig:

``` console
$ mv README.md README
$ git rm README.md
$ git add README
```

Git erkennt, dass es sich um eine umbenannte Datei handelt, so dass es
egal ist, ob Sie eine Datei auf diese Weise oder mit dem Befehl `mv`
umbenennen. Der alleinige, reale Unterschied ist, dass `git mv` ein
einziger Befehl ist statt deren drei -- es ist eine Komfortfunktion.
Wichtiger ist, dass Sie jedes beliebige Tool verwenden können, um eine
Datei umzubenennen und das `add`/`rm` später aufrufen können, bevor Sie
committen.

## Anzeigen der Commit-Historie {#_viewing_history}

Nachdem Sie mehrere Commits erstellt haben, oder wenn Sie ein Repository
mit einer bestehenden Commit-Historie geklont haben, werden Sie
wahrscheinlich zurückschauen wollen, um zu erfahren, was geschehen ist.
Das wichtigste und mächtigste Werkzeug dafür ist der Befehl `git log`.

Diese Beispiele verwenden ein sehr einfaches Projekt namens „simplegit".
Um das Projekt zu erstellen, führen Sie diesen Befehl aus:

``` console
$ git clone https://github.com/schacon/simplegit-progit
```

Wenn Sie `git log` in diesem Projekt aufrufen, sollten Sie eine Ausgabe
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

Standardmäßig, d.h. ohne Argumente, listet `git log` die in diesem
Repository vorgenommenen Commits in umgekehrter chronologischer
Reihenfolge auf, d.h. die neuesten Commits werden als Erstes angezeigt.
Wie Sie sehen können, listet dieser Befehl jeden Commit mit seiner
SHA-1-Prüfsumme, dem Namen und der E-Mail-Adresse des Autors, dem
Erstellungs-Datum und der Commit-Beschreibung auf.

Eine Vielzahl und Vielfalt von Optionen für den Befehl `git log` stehen
zur Verfügung, um Ihnen exakt das anzuzeigen, wonach Sie suchen. Hier
zeigen wir Ihnen einige der gängigsten.

Eine der hilfreichsten Optionen ist `-p` oder `--patch`. Sie zeigt den
Unterschied (die *patch*-Ausgabe) an, der bei jedem Commit eingefügt
wird. Sie können auch die Anzahl der anzuzeigenden Protokolleinträge
begrenzen, z.B. mit `-2` nur die letzten beiden Einträge darstellen.

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

 end
-
-if $0 == __FILE__
-  git = SimpleGit.new
-  puts git.show
-end
```

Diese Option zeigt die gleichen Informationen an, jedoch mit dem
Unterschied, dass sie direkt hinter jedem Eintrag stehen. Diese Funktion
ist sehr hilfreich für die Code-Überprüfung oder zum schnellen
Durchsuchen der Vorgänge während einer Reihe von Commits, die ein
Teammitglied hinzugefügt hat. Sie können auch eine Reihe von Optionen
zur Verdichtung mit `git log` verwenden. Wenn Sie beispielsweise einige
gekürzte Statistiken für jede Übertragung sehen möchten, können Sie die
Option `--stat` verwenden:

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

Wie Sie sehen können, gibt die Option `--stat` unter jedem
Commit-Eintrag eine Liste der geänderten Dateien aus. Wie viele Dateien
geändert wurden und wie viele Zeilen in diesen Dateien hinzugefügt und
entfernt wurden. Sie enthält auch eine Zusammenfassung am Ende des
Berichts.

Eine weitere wirklich nützliche Option ist `--pretty`. Diese Option
ändert das Format der Log-Ausgabe in ein anderes als das
Standard-Format. Ihnen stehen einige vorgefertigte Optionswerte zur
Verfügung. Der Wert `oneline` für diese Option gibt jeden Commit in
einer einzigen Zeile aus, was besonders nützlich ist, wenn Sie sich
viele Commits ansehen. Darüber hinaus zeigen die Werte `short`, `full`
und `fuller` die Ausgabe im etwa gleichen Format, allerdings mit mehr
oder weniger Informationen an:

``` console
$ git log --pretty=oneline
ca82a6dff817ec66f44342007202690a93763949 Change version number
085bb3bcb608e1e8451d4b2432f8ecbe6306e7e7 Remove unnecessary test
a11bef06a3f659402fe7563abf99ad00de2209e6 Initial commit
```

Der interessanteste Wert ist `format`, mit dem Sie Ihr eigenes
Log-Ausgabeformat festlegen können. Dieses Verfahren ist besonders
nützlich, wenn Sie Ausgaben für das maschinelle Parsen generieren -- da
Sie das Format explizit angeben, wissen Sie, dass es sich mit Updates
von Git nicht ändert:[]{.indexterm primary="Log formatieren"}

``` console
$ git log --pretty=format:"%h - %an, %ar : %s"
ca82a6d - Scott Chacon, 6 years ago : Change version number
085bb3b - Scott Chacon, 6 years ago : Remove unnecessary test
a11bef0 - Scott Chacon, 6 years ago : Initial commit
```

[Wichtige Spezifikatoren für ](#pretty_format) listet einige der
nützlichsten Spezifikatoren auf, die `format` bietet.

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

Sie fragen sich vielleicht, worin der Unterschied zwischen *Autor* und
*Committer* besteht. Der Autor ist die Person, die das Werk ursprünglich
geschrieben hat, während der Committer die Person ist, die das Werk
zuletzt bearbeitet hat. Wenn Sie also einen Patch an ein Projekt senden
und eines der Core-Mitglieder den Patch einbindet, erhalten Sie beide
die Anerkennung -- Sie als Autor und das Core-Mitglied als Committer.
Wir werden diese Unterscheidung näher in Kapitel 5, [Verteiltes
Git](ch05-distributed-git.xml#ch05-distributed-git) erläutern.

Die Optionswerte `oneline` und `format` sind vor allem bei einer anderen
`log` Option mit der Bezeichnung `--graph` hilfreich. Diese Option fügt
ein schönes kleines ASCII-Diagramm hinzu, das Ihren Branch und den
Merge-Verlauf zeigt:

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

Dieser Ausgabetyp wird immer interessanter, wenn wir im nächsten Kapitel
über das Branching und Merging sprechen.

Das sind nur einige einfache Optionen zur Ausgabe-Formatierung von
`git log` -- es gibt noch viele mehr. [Allgemeine Optionen für
](#log_options) listet die bisher von uns behandelten Optionen auf,
sowie einige andere gängige Format-Optionen, die sinnvoll sein können,
um die Ausgabe des log-Befehls zu ändern.

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

Zusätzlich zu den Optionen für die Ausgabe-Formatierung bietet `git log`
eine Reihe nützlicher einschränkender Optionen, d.h. Optionen, mit denen
Sie nur eine Teilmenge von Commits anzeigen können. Sie haben eine
solche Option bereits gesehen -- die Option `-2`, die nur die letzten
beiden Commits anzeigt. In Wahrheit können Sie `-<n>` verwenden, wobei
`n` eine beliebige ganze Zahl ist, um die letzten `n` Commits
anzuzeigen. In der Praxis werden Sie das kaum verwenden, da Git
standardmäßig alle Ausgaben über einen Pager leitet, so dass Sie jeweils
nur eine Seite der Log-Ausgabe sehen.

Die zeitbeschränkenden Optionen wie `--since` und `--until` sind sehr
nützlich. Dieser Befehl ruft z.B. die Liste der in den letzten beiden
Wochen durchgeführten Commits ab:

``` console
$ git log --since=2.weeks
```

Dieser Befehl funktioniert mit vielen Formaten. Sie können ein
bestimmtes Datum wie `"2008-01-15"` angeben, oder ein relatives Datum
wie `"vor 2 Jahren 1 Tag 3 Minuten"`.

Sie können die Liste auch nach Commits filtern, die bestimmten
Suchkriterien entsprechen. Mit der Option `--author` können Sie nach
einem bestimmten Autor filtern und mit der Option `--grep` können Sie
nach Schlüsselwörtern in den Übertragungsmeldungen suchen.

::: note
Sie können mehr als eine Instanz der Suchkriterien `--author` und
`--grep` angeben, was die Commit-Ausgabe auf Commits beschränkt, die
*jedem* der `--author` Muster und *jedem* der `--grep` Muster
entsprechen; durch Hinzufügen der Option `--all-match` wird die Ausgabe
jedoch weiter auf diejenigen Commits beschränkt, die *allen* `--grep`
Mustern entsprechen.
:::

Ein weiterer wirklich hilfreicher Filter ist die Option `-S`
(umgangssprachlich als Git's „Pickel"-Option bezeichnet), die eine
Zeichenkette übernimmt und nur die Commits anzeigt, die die Anzahl der
Vorkommen dieses Strings geändert haben. Wenn Sie beispielsweise das
letzte Commit suchen möchten, das einen Verweis auf eine bestimmte
Funktion hinzugefügt oder entfernt hat, können Sie Folgendes aufrufen:

``` console
$ git log -S function_name
```

Zuletzt eine wirklich nützliche Option, die Sie als Filter an `git log`
übergeben können, den Pfad. Wenn Sie ein Verzeichnis oder einen
Dateinamen angeben, können Sie die Log-Ausgabe auf Commits beschränken,
die eine Änderung an diesen Dateien vorgenommen haben. Das ist immer die
letzte Option und wird in der Regel durch Doppelstriche (`--`)
eingeleitet, um Pfade von den Optionen zu trennen.

``` console
$ git log -- path/to/file
```

In [Optionen zum Anpassen der Ausgabe von ](#limit_options) werden wir
Ihnen diese und einige andere gängige Optionen als Referenz auflisten.

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

Wenn Sie zum Beispiel sehen möchten, welche der Commits die Testdateien
in der Git-Quellcode-Historie ändern, die von Junio Hamano im Monat
Oktober 2008 committet wurden und keine Merge-Commits sind, können Sie
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

Von den fast 40.000 Commits in der Git-Quellcode-Historie zeigt dieser
Befehl die 6 Commits an, die diesen Kriterien entsprechen.

::: tip
::: title
Die Anzeige von Merge-Commits unterdrücken
:::

Abhängig von dem in Ihrem Repository verwendeten Workflow ist es
möglich, dass ein beträchtlicher Prozentsatz der Commits in Ihrer
Log-Historie nur Merge-Commits sind, die in der Regel nicht sehr
informativ sind. Um zu vermeiden, dass die Anzeige von Merge-Commits
Ihren Log-Verlauf überflutet, fügen Sie einfach die Log-Option
`--no-merges` hinzu.
:::

## Ungewollte Änderungen rückgängig machen {#_undoing}

Zu jeder Zeit können Sie eine Änderung rückgängig machen. Hier werden
wir einige grundlegende Werkzeuge besprechen, die zum Widerrufen von
gemachten Änderungen dienen. Seien Sie vorsichtig, denn man kann nicht
immer alle diese Annullierungen rückgängig machen. Das ist einer der
wenigen Bereiche in Git, in denen Sie etwas Arbeit verlieren könnten,
wenn Sie etwas falsch machen.

Eines der häufigsten Undos tritt auf, wenn Sie zu früh committen und
möglicherweise vergessen haben, einige Dateien hinzuzufügen, oder wenn
Sie Ihre Commit-Nachricht durcheinander bringen. Wenn Sie den Commit
erneut ausführen möchten, nehmen Sie die zusätzlichen Änderungen vor,
die Sie vergessen haben, stellen diese bereit (engl. stage) und
committen diese erneut mit der Option `--amend`:

``` console
$ git commit --amend
```

Dieser Befehl übernimmt Ihre Staging-Area und verwendet sie für den
Commit. Wenn Sie seit Ihrem letzten Commit keine Änderungen vorgenommen
haben (z.B. Sie führen diesen Befehl unmittelbar nach Ihrem vorherigen
Commit aus), dann sieht Ihr Snapshot genau gleich aus; Sie ändern nur
Ihre Commit-Nachricht.

Der gleiche Commit-Message-Editor wird aufgerufen, enthält aber bereits
die Nachricht Ihres vorherigen Commits. Sie können die Nachricht wie
gewohnt bearbeiten, aber sie überschreibt den vorherigen Commit.

Wenn Sie zum Beispiel die Änderungen in einer Datei, die Sie zu dieser
Übertragung hinzufügen wollten, vergessen haben, können Sie etwas
Ähnliches durchführen:

``` console
$ git commit -m 'Initial commit'
$ git add forgotten_file
$ git commit --amend
```

Sie erhalten am Ende einen einzigen Commit -- der zweite Commit ersetzt
die Ergebnisse des ersten.

::: note
Es ist wichtig zu verstehen, dass, wenn Sie Ihren letzten Commit ändern,
Sie ihn weniger reparieren, als ihn komplett durch einen neuen,
verbesserten Commit *ersetzen*. Der alte Commit wird aus dem Weg geräumt
und der neue Commit an seine Stelle gesetzt. Tatsächlich ist es so, als
ob der letzte Commit nie stattgefunden hätte und er nicht mehr in Ihrem
Repository-Verlauf auftaucht.

Der naheliegendste Nutzen für die Änderung von Commits besteht darin,
kleine Verbesserungen an Ihrem letzten Commit vorzunehmen, ohne Ihren
Repository-Verlauf mit Commit-Nachrichten der Form „Ups, vergessen, eine
Datei hinzuzufügen" oder „Verdammt, einen Tippfehler im letzten Commit
behoben" zu überladen.
:::

::: note
Ändern Sie nur lokale Commits, die noch nicht gepusht wurden. Das Ändern
zuvor übertragener Commits und das forcierte pushen des Branches
verursacht Probleme bei ihren Mitstreitern. Weitere Informationen
darüber, was dabei passiert und wie Sie es wieder gerade ziehen können,
wenn Sie sich auf der Empfängerseite befinden, finden Sie unter
[ch03-git-branching.md](ch03-git-branching.md#_rebase_peril).
:::

### Eine Datei aus der Staging-Area entfernen {#_unstaging}

Die nächsten beiden Abschnitte erläutern, wie Sie mit Ihrer Staging-Area
und den Änderungen des Arbeitsverzeichnisses arbeiten. Der angenehme
Nebeneffekt ist, dass der Befehl, mit dem Sie den Zustand dieser beiden
Bereiche bestimmen, Sie auch daran erinnert, wie Sie Änderungen an ihnen
rückgängig machen können. Nehmen wir zum Beispiel an, Sie haben zwei
Dateien geändert und möchten sie als zwei separate Änderungen
übertragen, aber Sie geben versehentlich `git add *` ein und stellen sie
dann beide in der Staging-Area bereit. Wie können Sie eine der beiden
aus der Staging-Area entfernen? Der Befehl `git status` meldet:

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
`git reset HEAD <file>…​` verwenden soll, um die Staging-Area zu
entleeren. Lassen Sie uns also diesem Rat folgen und die Datei
`CONTRIBUTING.md` aus der Staging-Area entfernen:

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

Der Befehl klingt etwas merkwürdig, aber er funktioniert. Die Datei
`CONTRIBUTING.md` wurde modifiziert, aber erneut im Status unstaged.

::: note
Es ist wahr, dass `git reset` ein riskanter Befehl sein kann, besonders,
wenn Sie das `--hard` Flag mitgeben. In dem oben beschriebenen Szenario
wird die Datei in Ihrem Arbeitsverzeichnis jedoch nicht angetastet, so
dass er relativ sicher ist.
:::

Im Moment ist dieser Aufruf alles, was Sie über den Befehl `git reset`
wissen müssen. Wir werden viel ausführlicher darauf eingehen, was
`reset` bewirkt und wie man es beherrscht, um wirklich interessante
Aufgaben zu erledigen, siehe Kapitel 7 [Git
Reset](ch07-git-tools.xml#_git_reset).

### Änderung in einer modifizierten Datei zurücknehmen {#_änderung_in_einer_modifizierten_datei_zurücknehmen}

Was ist, wenn Sie feststellen, dass Sie Ihre Änderungen an der Datei
`CONTRIBUTING.md` nicht behalten wollen? Wie können Sie sie leicht
wieder ändern -- sie wieder so zurücksetzen, wie sie beim letzten Commit
ausgesehen hat (oder anfänglich geklont wurde, oder wie auch immer Sie
sie in Ihr Arbeitsverzeichnis bekommen haben)? Glücklicherweise sagt
Ihnen `git status` auch, wie Sie das machen können. Im letzten Beispiel
sieht die Unstaged-Area so aus:

``` console
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

    modified:   CONTRIBUTING.md
```

Sie erklärt Ihnen ziemlich eindeutig, wie Sie die von Ihnen
vorgenommenen Änderungen verwerfen können. Lassen Sie uns machen, was da
steht:

``` console
$ git checkout -- CONTRIBUTING.md
$ git status
On branch master
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

    renamed:    README.md -> README
```

Sie können erkennen, dass die Änderungen rückgängig gemacht wurden.

::: important
Es ist sehr wichtig zu begreifen, dass `git checkout — <file>` ein
riskanter Befehl ist. Alle lokalen Änderungen, die Sie an dieser Datei
vorgenommen haben, sind verloren -- Git hat diese Datei einfach durch
die zuletzt committete oder gestagten Version ersetzt. Verwenden Sie
diesen Befehl nur, wenn Sie sich absolut sicher sind, dass Sie diese
nicht gespeicherten lokalen Änderungen nicht benötigen.
:::

Wenn Sie die Änderungen, die Sie an dieser Datei gemacht haben,
beibehalten möchten, sie aber vorerst aus dem Weg räumen möchten,
sollten wir das Stashing und Branching in Kapitel 3 -- [Git
Branching](ch03-git-branching.md#ch03-git-branching) durchgehen; das
sind im Allgemeinen die besseren Methoden, um das zu erledigen.

Denken Sie daran, dass alles, was in Git *committet* wird, fast immer
wiederhergestellt werden kann. Sogar Commits, die auf gelöschten
Branches lagen oder Commits, die mit einem `--amend` Commit
überschrieben wurden, können wiederhergestellt werden (siehe Kapitel 10
[Daten-Rettung](ch10-git-internals.xml#_data_recovery) für das
Wiederherstellen der Daten). Allerdings wird alles, was Sie verloren
haben und das nie committet wurde, wahrscheinlich nie wieder gesehen
werden.

### Änderungen Rückgängigmachen mit git restore {#undoing_git_restore}

Git Version 2.23.0 führte einen neuen Befehl ein: `git restore`. Es ist
im Grunde eine Alternative zu `git reset`, die wir gerade behandelt
haben. Ab Git Version 2.23.0 verwendet Git für viele Rückgängig-Vorgänge
`git restore` anstelle von `git reset`.

Lassen Sie uns unsere Schritte zurückverfolgen und die Dinge mit
`git restore` anstelle von `git reset` rückgängig machen.

#### Unstagen einer staged Datei mit git restore {#_unstagen_einer_staged_datei_mit_git_restore}

Die nächsten beiden Abschnitte zeigen, wie Sie an Änderungen in Ihrem
Staging-Bereich und im Arbeitsverzeichnisses mit `git restore` arbeiten.
Das Schöne daran ist, dass der Befehl, mit dem Sie den Status dieser
beiden Bereiche bestimmen, Ihnen auch zeigt, wie Sie Änderungen an ihnen
rückgängig machen können. Angenommen, Sie haben zwei Dateien geändert
und möchten sie als zwei separate Änderungen festschreiben. Sie geben
jedoch versehentlich `git add *` ein und stellen beide bereit. Wie
können sie einen der beiden wieder unstagen? Der Befehl `git status`
zeigt es ihnen:

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
`git restore --staged <file> …​` zum unstagen. Verwenden wir diesen Rat,
um die Datei `CONTRIBUTING.md` zu unstagen:

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

Die Datei `CONTRIBUTING.md` ist geändert aber wieder unstaged.

#### Rückgängig machen einer geänderten Datei mit git restore {#_rückgängig_machen_einer_geänderten_datei_mit_git_restore}

Was ist, wenn Sie feststellen, dass Sie Ihre Änderungen an der Datei
`CONTRIBUTING.md` nicht beibehalten möchten? Wie können Sie sie einfach
rückgängig machen --- sprich, sie so zurücksetzen, wie sie aussah, als
Sie sie zuletzt committet haben (oder ursprünglich geklont haben oder
wie auch immer Sie es in Ihr Arbeitsverzeichnis aufgenommen haben)?
Glücklicherweise sagt Ihnen `git status` wiederum, wie das geht. In der
letzten Beispielausgabe sieht der unstaged Bereich folgendermaßen aus:

``` console
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
    modified:   CONTRIBUTING.md
```

Es zeigt Ihnen ziemlich explizit, wie Sie die vorgenommenen Änderungen
verwerfen können. Lassen Sie uns das tun:

``` console
$ git restore CONTRIBUTING.md
$ git status
On branch master
Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
    renamed:    README.md -> README
```

::: important
Es ist wichtig zu verstehen, dass `git restore <file>` ein gefährlicher
Befehl ist. Alle lokalen Änderungen, die Sie an dieser Datei vorgenommen
haben, sind weg. Git hat diese Datei durch die zuletzt committete oder
gestagte Version ersetzt. Verwenden Sie diesen Befehl nur, wenn Sie sich
absolut sicher sind, dass Sie diese nicht gespeicherten lokalen
Änderungen nicht benötigen.
:::

## Mit Remotes arbeiten {#_remote_repos}

Um an jedem Git-Projekt mitarbeiten zu können, müssen Sie wissen, wie
Sie Ihre Remote-Repositorys verwalten können. Remote-Repositorys sind
Versionen Ihres Projekts, die im Internet oder im Netzwerk irgendwo
gehostet werden. Sie können mehrere einrichten, von denen jedes in der
Regel entweder schreibgeschützt oder beschreibbar für Sie ist. Die
Zusammenarbeit mit anderen erfordert die Verwaltung dieser
Remote-Repositorys sowie das Pushing und Pulling von Daten zu und von
den Repositorys, wenn Sie Ihre Arbeit teilen möchten. Die Verwaltung von
Remote-Repositorys umfasst das Wissen, wie man entfernte Repositorys
hinzufügt, nicht mehr gültige Remotes entfernt, verschiedene
Remote-Branches verwaltet, sie als versioniert oder nicht versioniert
definiert und vieles mehr. In diesem Abschnitt werden wir einige dieser
Remote-Management-Fertigkeiten erörtern.

::: note
::: title
Remote-Repositorys können auch auf Ihrem lokalen Rechner liegen.
:::

Es ist durchaus möglich, dass Sie mit einem „entfernten" Repository
arbeiten können, das sich tatsächlich auf demselben Host befindet auf
dem Sie gerade arbeiten. Das Wort „remote" bedeutet nicht unbedingt,
dass sich das Repository an einem anderen Ort im Netzwerk oder Internet
befindet, sondern nur, dass es an einem anderen Ort liegt. Die Arbeit
mit einem solchen entfernten Repository würde immer noch alle üblichen
Push-, Pull- und Fetch-Operationen einschließen, wie bei jedem anderen
Remote-Repository.
:::

### Auflisten der Remotes {#_auflisten_der_remotes}

Um zu sehen, welche Remote-Server Sie konfiguriert haben, können Sie den
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

Sie können zusätzlich auch `-v` angeben, das Ihnen die URLs anzeigt, die
Git für den Kurznamen gespeichert hat, der beim Lesen und Schreiben auf
diesem Remote verwendet werden soll:

``` console
$ git remote -v
origin  https://github.com/schacon/ticgit (fetch)
origin  https://github.com/schacon/ticgit (push)
```

Wenn Sie mehr als einen Remote haben, listet der Befehl sie alle auf.
Ein Repository mit mehreren Remotes für die Arbeit mit mehreren
Beteiligten könnte beispielsweise so aussehen.

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

Das bedeutet, dass wir Beiträge von jedem dieser Benutzer ziemlich
einfach abrufen können. Möglicherweise haben wir zusätzlich die
Erlaubnis, auf einen oder mehrere von diesen zu pushen, obwohl wir das
hier nicht erkennen können.

Beachten Sie, dass diese Remotes eine Vielzahl von Protokollen
verwenden; wir werden mehr darüber erfahren, wenn wir [Git auf einem
Server
installieren](ch04-git-on-the-server.md#_getting_git_on_a_server).

### Hinzufügen von Remote-Repositorys {#_hinzufügen_von_remote_repositorys}

Wir haben bereits erwähnt und einige Beispiele gezeigt, wie der Befehl
`git clone` stillschweigend den Remote `origin` für Sie hinzufügt. So
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

Jetzt können Sie die Zeichenfolge `pb` auf der Kommandozeile anstelle
der gesamten URL verwenden. Wenn Sie beispielsweise alle Informationen
abrufen möchten, die Paul hat, die aber noch nicht in Ihrem Repository
enthalten sind, können Sie `git fetch pb` ausführen:

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
können ihn in eine Ihrer Branches einbinden oder an dieser Stelle in
einen lokalen Branch wechseln (engl. checkout), wenn Sie ihn inspizieren
möchten. Wir werden in [Git
Branching](ch03-git-branching.md#ch03-git-branching) detailliert
beschreiben, was Branches sind und wie man sie nutzt.

### Fetchen und Pullen von Ihren Remotes {#_fetching_and_pulling}

Wie Sie gerade gesehen haben, können Sie Daten aus Ihren
Remote-Projekten abrufen:[]{.indexterm primary="Git Befehle"
secondary="fetch"}

``` console
$ git fetch <remote>
```

Der Befehl geht an das Remote-Projekt und zieht (engl. pull) alle Daten
von diesem Remote-Projekt runter, die Sie noch nicht haben. Danach
sollten Sie Referenzen auf alle Branches von diesem Remote haben, die
Sie jederzeit einbinden oder inspizieren können.

Wenn Sie ein Repository klonen, fügt der Befehl dieses entfernte
Repository automatisch unter dem Namen „origin" hinzu. So holt
`git fetch origin` alle neuen Inhalte, die seit dem Klonen (oder dem
letzten Abholen) auf diesen Server verschoben wurden. Es ist jedoch
wichtig zu beachten, dass der Befehl `git fetch` nur die Daten in Ihr
lokales Repository herunterlädt -- er mischt (engl. merged) sie nicht
automatisch mit Ihrer Arbeit zusammen oder ändert das, woran Sie gerade
arbeiten. Sie müssen das Ganze manuell mit Ihrer Arbeit zusammenführen,
wenn Sie fertig sind.

Wenn Ihr aktueller Branch so eingerichtet ist, dass er einen entfernten
Branch verfolgt (engl. tracking), können Sie den Befehl `git pull`
verwenden, um diesen entfernten Branch automatisch zu holen und dann mit
Ihrem aktuellen Branch zusammenzuführen (siehe den nächsten Abschnitt
und [Git Branching](ch03-git-branching.md#ch03-git-branching) für
weitere Informationen).[]{.indexterm primary="Git Befehle"
secondary="pull"} Das könnte ein einfacherer oder komfortablerer
Workflow für Sie sein. Standardmäßig richtet der Befehl `git clone`
Ihren lokalen `master` Branch automatisch so ein, dass er den entfernten
`master` Branch (oder wie auch immer der Standard-Branch genannt wird)
auf dem Server versioniert von dem Sie ihn geklont haben. Wenn Sie
`git pull` ausführen, werden normalerweise Daten von dem Server
abgerufen, von dem Sie ursprünglich geklont haben, und es wird
automatisch versucht in den Code zu mergen, an dem Sie gerade arbeiten.

::: note
Ab der Version 2.27 von Git wird `git pull` eine Warnung ausgeben, wenn
die Variable `pull.rebase` nicht gesetzt ist. Git wird Sie so lange
warnen, bis Sie die Variable setzen.

Falls Sie das Standardverhalten (möglichst ein fast-forward, ansonsten
einen Merge-Commit erstellen) von Git beibehalten wollen:
`git config --global pull.rebase "false"`

Wenn Sie mit dem Pullen einen Rebase machen wollen:
`git config --global pull.rebase "true"`
:::

### Zu Ihren Remotes Pushen {#_pushing_remotes}

Wenn Sie Ihr Projekt an einem bestimmten Punkt haben, den Sie teilen
möchten, müssen Sie es zum Upstream verschieben (engl. pushen). Der
Befehl dafür ist einfach: `git push <remote> <branch>`.[]{.indexterm
primary="Git Befehle" secondary="push"} Wenn Sie Ihren `master` Branch
auf Ihren `origin` Server verschieben möchten (nochmals, das Klonen
richtet im Regelfall beide dieser Namen automatisch für Sie ein), dann
können Sie diesen Befehl auch nutzen, um alle Commits, die Sie
durchgeführt haben, auf den Server zu übertragen:

``` console
$ git push origin master
```

Dieser Befehl funktioniert allerdings nur, wenn Sie von einem Server
geklont haben, auf den Sie Schreibzugriff haben und wenn in der
Zwischenzeit noch niemand anderes gepusht hat. Wenn Sie und ein anderer
Benutzer gleichzeitig klonen und Sie beide Upstream pushen wollen, Sie
aber etwas später nach Upstream pushen, dann wird Ihr Push zu Recht
abgelehnt. Sie müssen zuerst dessen Bearbeitung abholen und in Ihre
einbinden, bevor Sie pushen können. Siehe Kapitel 3 [Git
Branching](ch03-git-branching.md#ch03-git-branching) mit
ausführlicheren Details zum Pushen auf Remote-Server.

### Inspizieren eines Remotes {#_inspecting_remote}

Wenn Sie mehr Informationen über einen bestimmten Remote sehen möchten,
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

Er listet die URL für das Remote-Repository sowie die Informationen zu
den Tracking-Branchen auf. Der Befehl teilt Ihnen hilfreich mit, dass,
wenn Sie sich im Master-Zweig befinden und `git pull` ausführen, der
`master` Branch des remotes nach dem abrufen (engl. fetched) automatisch
mit dem lokalen Branch gemerged wird. Er listet auch alle
Remote-Referenzen auf, die er abgerufen hat.

Das ist nur ein einfaches Beispiel, auf das Sie vermutlich treffen
werden. Wenn Sie Git hingegen intensiver verwenden, können Sie viel mehr
Informationen aus `git remote show` herauslesen:

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

Dieser Befehl zeigt an, zu welchem Zweig automatisch gepusht wird, wenn
Sie `git push` ausführen, während Sie sich in bestimmten Branches
befinden. Er zeigt Ihnen auch, welche entfernten Branches auf dem Server
sind, die Sie noch nicht haben, welche entfernten Branches Sie haben,
die aber vom Server entfernt wurden und die lokalen Branches, die
automatisch mit Ihrem Remote-Tracking-Branch zusammengeführt (gemergt)
werden können, wenn Sie `git pull` ausführen.

### Umbenennen und Entfernen von Remotes {#_umbenennen_und_entfernen_von_remotes}

Sie können `git remote rename` ausführen, um den Kurznamen eines Remotes
zu ändern.[]{.indexterm primary="Git Befehle" secondary="remote"} Wenn
Sie beispielsweise `pb` in `paul` umbenennen möchten, können Sie dieses
mit dem Befehl `git remote rename` machen:

``` console
$ git remote rename pb paul
$ git remote
origin
paul
```

Es ist zu beachten, dass dadurch auch alle Ihre
Remote-Tracking-Branchnamen geändert werden. Was früher mit `pb/master`
angesprochen wurde, ist jetzt `paul/master`.

Wenn Sie einen Remote aus irgendwelchen Gründen entfernen möchten -- Sie
haben den Server verschoben oder verwenden einen bestimmten Mirror nicht
länger oder ein Beitragender ist nicht mehr dabei -- dann können Sie
entweder `git remote remove` oder `git remote rm` verwenden:

``` console
$ git remote remove paul
$ git remote
origin
```

Sobald Sie die Referenz auf einen Remote auf diese Weise gelöscht haben,
werden auch alle mit diesem Remote verbundenen Remote-Tracking-Branches
und Konfigurationseinstellungen gelöscht.

## Taggen {#_git_tagging}

[]{.indexterm primary="Tags"} Wie die meisten VCSs hat Git die
Möglichkeit, bestimmte Punkte in der Historie eines Repositorys als
wichtig zu markieren. Normalerweise verwenden Leute diese
Funktionalität, um Releases zu markieren (`v1.0`, `v2.0` usw). In diesem
Abschnitt erfahren Sie, wie Sie bestehende Tags auflisten, Tags
erstellen und löschen können sowie was die unterschiedlichen Tag-Typen
sind.

### Ihre Tags auflisten {#_ihre_tags_auflisten}

Die Auflistung der vorhandenen Tags in Git ist unkompliziert. Geben Sie
einfach `git tag` (mit optionalem `-l` oder `--list`) ein:[]{.indexterm
primary="Git Befehle" secondary="tag"}

``` console
$ git tag
v1.0
v2.0
```

Dieser Befehl listet die Tags in alphabetischer Reihenfolge auf. Die
Reihenfolge, in der sie angezeigt werden, hat keine wirkliche Bedeutung.

Sie können auch nach Tags suchen, die einer bestimmten Zeichenfolge
entsprechen. Das Git-Source-Repo zum Beispiel enthält mehr als 500 Tags.
Wenn Sie nur daran interessiert sind, sich die 1.8.5-Serie anzusehen,
können Sie Folgendes ausführen:

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
Das Auflisten von Tag-Wildcards erfordert die Option `-l` oder `--list`
:::

Wenn Sie lediglich die gesamte Liste der Tags wünschen, geht die
Ausführung des Befehls `git tag` implizit davon aus, dass Sie eine
Auflistung haben wollen und gibt sie aus; die Verwendung von `-l` oder
`--list` ist in diesem Fall optional.

Wenn Sie jedoch ein Platzhaltermuster angeben, das mit den Tag-Namen
übereinstimmt, ist die Verwendung von `-l` oder `--list` obligatorisch.
:::

### Erstellen von Tags {#_erstellen_von_tags}

Git unterstützt zwei Arten von Tags: *lightweight* (d.h.
nicht-annotiert) und *annotated*.

Ein nicht-annotiertes Tag ist sehr ähnlich eines Branches, der sich
nicht ändert -- es ist nur ein Zeiger auf einen bestimmten Commit.

Annotierte Tags werden dagegen als vollständige Objekte in der
Git-Datenbank gespeichert. Sie werden mit einer Prüfsumme versehen,
enthalten den Tagger-Namen, die E-Mail-Adresse und das Datum, haben eine
Tagging-Meldung und können mit GNU Privacy Guard (GPG) signiert und
überprüft werden. Es wird allgemein empfohlen, dass Sie annotierte Tags
erstellen, damit Sie all diese Informationen erhalten können; aber wenn
Sie ein temporäres Tag wünschen oder aus irgendwelchen Gründen die
anderen Informationen nicht speichern wollen, sind auch nicht-annotierte
Tags möglich.

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

Ein `-m` spezifiziert eine Tagging-Meldung, die mit dem Tag gespeichert
wird. Wenn Sie keine Meldung für ein annotiertes Tag angeben, startet
Git Ihren Editor, damit Sie diese eingeben können.

Sie können die Tag-Daten zusammen mit dem Commit einsehen, der mit dem
Befehl `git show` getaggt wurde:

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

Es werden die Tagger-Informationen, das Datum, an dem der Commit getaggt
wurde, und die Annotationsmeldung angezeigt, gefolgt von den
Commit-Informationen.

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

Wenn Sie diesmal `git show` auf dem Tag ausführen, sehen Sie keine
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

Sie können auch Commits markieren, wenn Sie sich bereits an einem
anderen Punkt befinden. Angenommen, Ihr Commit-Verlauf sieht so aus:

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

Nehmen wir an, Sie haben vergessen, das Projekt mit v1.2 zu markieren,
das bei dem Commit von „Update rakefile" vorlag. Sie können ihn
nachträglich hinzufügen. Um diesen Commit zu markieren, geben Sie am
Ende des Befehls die Commit-Prüfsumme (oder einen Teil davon) an:

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

Normalerweise überträgt der Befehl `git push` keine Tags an den
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

Wenn Sie eine Menge Tags haben, die Sie auf einmal pushen wollen, können
Sie auch die Option `--tags` mit dem Befehl `git push` verwenden.
Dadurch werden alle Ihre Tags auf den Remote-Server übertragen, die sich
noch nicht auf dem Server befinden.

``` console
$ git push origin --tags
Counting objects: 1, done.
Writing objects: 100% (1/1), 160 bytes | 0 bytes/s, done.
Total 1 (delta 0), reused 0 (delta 0)
To git@github.com:schacon/simplegit.git
 * [new tag]         v1.4 -> v1.4
 * [new tag]         v1.4-lw -> v1.4-lw
```

Wenn jetzt jemand anderes aus Ihrem Repository klont oder pullt, erhält
er auch alle Ihre Tags.

::: note
::: title
`git push` pusht beide Arten von Tags
:::

`git push <remote> --tags` wird sowohl Lightweight- als auch
Annotated-Tags pushen. Es gibt zur Zeit keine Möglichkeit, nur
Lightweight-Tags zu pushen, aber wenn Sie
`git push <remote> --follow-tags` verwenden, werden nur annotierte Tags
an den Remote gepusht.
:::

### Tags löschen {#_tags_löschen}

Um einen Tag aus dem lokalen Repository zu löschen, verwenden Sie
`git tag -d <tagname>`. Wir könnten beispielsweise den leichtgewichtigen
Tag wie folgt entfernen:

``` console
$ git tag -d v1.4-lw
Deleted tag 'v1.4-lw' (was e7d5add)
```

Beachten Sie, dass dadurch das Tag nicht von Remote-Servern entfernt
wird. Es gibt zwei gängige Varianten, um ein Tag von einem entfernten
Server zu löschen.

Die erste Möglichkeit ist `git push <remote> :refs/tags/<tagname>`:

``` console
$ git push origin :refs/tags/v1.4-lw
To /git@github.com:schacon/simplegit.git
 - [deleted]         v1.4-lw
```

Die Lösung, das oben Gezeigte zu interpretieren, besteht darin, es als
Nullwert zu lesen, bevor der Doppelpunkt auf den Remote-Tag-Namen
verschoben wird, wodurch es effektiv gelöscht wird.

Der zweite, intuitivere Weg, ein Remote-Tag zu löschen, ist mit:

``` console
$ git push origin --delete <tagname>
```

### Tags auschecken {#_tags_auschecken}

Wenn Sie die Dateiversion anzeigen möchten, auf die ein bestimmter Tag
zeigt, können Sie `git checkout` auf dieses Tag durchführen, obwohl dies
Ihr Repository in den Zustand „detached HEAD" (dt. losgelöst) versetzt,
was einige negative Nebenwirkungen hat:

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
einen Commit erstellen, bleibt der Tag gleich, aber Ihr neuer Commit
gehört zu keinem Branch und ist unzugänglich, außer mit dem genauen
Commit-Hash. Wenn Sie also Änderungen vornehmen müssen -- z.B. wenn Sie
einen Fehler in einer älteren Version beheben -- sollten Sie im
Regelfall einen Branch erstellen:

``` console
$ git checkout -b version2 v2.0.0
Switched to a new branch 'version2'
```

Wenn Sie das tun und einen Commit erstellen, wird sich Ihr Zweig
`version2` leicht von Ihrem Tag `v2.0.0` unterscheiden, da er mit Ihren
neuen Änderungen fortschreitet, seien Sie also vorsichtig.

## Git Aliases {#_git_aliases}

[]{.indexterm primary="Aliasnamen"} Bevor wir dieses Kapitel über Basic
Git abschließen, gibt es noch einen kurzen Tipp, der Ihre Arbeit mit Git
einfacher, leichter und verständlicher machen kann: Aliase. Der Klarheit
halber werden wir sie nirgendwo anders in diesem Buch verwenden, aber
wenn Sie Git in Zukunft regelmäßig verwenden, dann sind Aliase etwas,
das Sie kennen sollten.

Git schließt nicht automatisch auf Ihren Befehl, wenn Sie ihn nur
teilweise eingeben. Wenn Sie nicht den gesamten Text von jedem der
Git-Befehle eingeben möchten, können Sie mit Hilfe von `git config`
einfach ein Alias für jeden Befehl einrichten.[]{.indexterm
primary="Git Befehle" secondary="config"} Hier sind ein paar Beispiele,
die Sie einrichten sollten:

``` console
$ git config --global alias.co checkout
$ git config --global alias.br branch
$ git config --global alias.ci commit
$ git config --global alias.st status
```

Das bedeutet, dass Sie z.B. anstelle von `git commit` einfach `git ci`
eingeben können. Wenn Sie Git nun weiter verwenden, werden Sie
vermutlich auch andere Befehle häufig verwenden; scheuen Sie sich nicht,
neue Aliase zu erstellen.

Diese Technik kann auch sehr nützlich sein, um Befehle zu erstellen, von
denen Sie glauben, dass sie vorhanden sein sollten. Um beispielsweise
ein Usability-Problem zu beheben, auf das Sie beim Entfernen einer Datei
aus der Staging-Area stoßen, können Sie Git Ihren eigenen Unstage-Alias
hinzufügen:

``` console
$ git config --global alias.unstage 'reset HEAD --'
```

Dadurch sind die folgenden beiden Befehle gleichwertig:

``` console
$ git unstage fileA
$ git reset HEAD -- fileA
```

Das erscheint etwas klarer. Es ist auch üblich, einen `last` (dt.
letzten) Befehl hinzuzufügen, so wie hier:

``` console
$ git config --global alias.last 'log -1 HEAD'
```

Auf diese Weise können Sie den letzten Commit leicht auffinden:

``` console
$ git last
commit 66938dae3329c7aebe598c2246a8e6af90d04646
Author: Josh Goebel <dreamer3@example.com>
Date:   Tue Aug 26 19:48:51 2008 +0800

    Test for current head

    Signed-off-by: Scott Chacon <schacon@example.com>
```

Wie Sie feststellen können, ersetzt Git einfach den neuen Befehl durch
den Alias, für den Sie ihn verwenden. Vielleicht möchten Sie jedoch eher
einen externen Befehl als einen Git-Subbefehl ausführen. In diesem Fall
starten Sie den Befehl mit einem Ausrufezeichen (`!`). Das ist
hilfreich, wenn Sie Ihre eigenen Tools schreiben, die mit einem
Git-Repository arbeiten. Wir können dies demonstrieren, indem wir
`git visual` mit `gitk` aliasieren:

``` console
$ git config --global alias.visual '!gitk'
```

## Zusammenfassung {#_zusammenfassung}

Sie sollten jetzt in der Lage sein, die wichtigsten Git-Befehle
einsetzen zu können. Folgendes sollte Ihnen jetzt gelingen: Erzeugen
oder Klonen eines Repositorys, Änderungen vorzunehmen und zur
Staging-Area hinzuzufügen, Commits anzulegen und die Historie aller
Commits in einem Repository zu durchsuchen. Als nächstes werden wir uns
mit der „Killer-Funktion" von Git befassen: dem Branching-Modell.
