# Erste Schritte {#ch01-getting-started}

In diesem Kapitel wird es darum gehen, wie Sie mit Git beginnen können.
Wir starten mit einer Erläuterung des Hintergrunds zu
Versionskontroll-Systemen, gehen dann weiter zu dem Thema, wie Sie Git
auf Ihrem System zum Laufen bringen und schließlich wie Sie es
einrichten können, sodass Sie in der Lage sind, die ersten Schritte mit
Git zu tun. Am Ende dieses Kapitels sollten Sie verstehen, wozu Git gut
ist und weshalb man es verwenden sollte und Sie sollten in der Lage
sein, mit Git loslegen zu können.

## Was ist Versionsverwaltung? {#_was_ist_versionsverwaltung}

[]{.indexterm primary="Versionsverwaltung"} Was ist
„Versionsverwaltung", und warum sollten Sie sich dafür interessieren?
Versionsverwaltung ist ein System, welches die Änderungen an einer oder
einer Reihe von Dateien über die Zeit hinweg protokolliert, sodass man
später auf eine bestimmte Version zurückgreifen kann. Die Dateien, die
in den Beispielen in diesem Buch unter Versionsverwaltung gestellt
werden, enthalten Quelltext von Software, tatsächlich kann in der Praxis
nahezu jede Art von Datei per Versionsverwaltung nachverfolgt werden.

Als Grafik- oder Webdesigner möchte man zum Beispiel in der Lage sein,
jede Version eines Bildes oder Layouts nachverfolgen zu können. Als
solcher wäre es deshalb ratsam, ein Versionsverwaltungssystem (engl.
Version Control System, VCS) einzusetzen. Ein solches System erlaubt es,
einzelne Dateien oder auch ein ganzes Projekt in einen früheren Zustand
zurückzuversetzen, nachzuvollziehen, wer zuletzt welche Änderungen
vorgenommen hat, die möglicherweise Probleme verursachen, herauszufinden
wer eine Änderung ursprünglich vorgenommen hat und viele weitere Dinge.
Ein Versionsverwaltungssystem bietet allgemein die Möglichkeit,
jederzeit zu einem vorherigen, funktionierenden Zustand zurückzukehren,
auch wenn man einmal Mist gebaut oder aus irgendeinem Grund Dateien
verloren hat. All diese Vorteile erhält man für einen nur sehr geringen,
zusätzlichen Aufwand.

### Lokale Versionsverwaltung {#_lokale_versionsverwaltung}

[]{.indexterm primary="Versionsverwaltung" secondary="lokal"} Viele
Menschen betreiben Versionsverwaltung, indem sie einfach all ihre
Dateien in ein separates Verzeichnis kopieren (die Schlaueren darunter
verwenden ein Verzeichnis mit Zeitstempel im Namen). Diese
Vorgehensweise ist sehr weit verbreitet und wird gern verwendet, weil
sie so einfach ist. Aber sie ist eben auch unglaublich fehleranfällig.
Man arbeitet sehr leicht im falschen Verzeichnis, bearbeitet damit die
falschen Dateien oder überschreibt Dateien, die man eigentlich nicht
überschreiben wollte.

Aus diesem Grund, haben Programmierer bereits vor langer Zeit, lokale
Versionsverwaltungssysteme entwickelt, die alle Änderungen an allen
relevanten Dateien in einer Datenbank verwalten.

<figure>
<img src="images/local.png" alt="Local version control diagram" />
<figcaption>Lokale Versionsverwaltung</figcaption>
</figure>

Eines der populäreren Versionsverwaltungssysteme war RCS, welches auch
heute noch mit vielen Computern ausgeliefert wird.
[RCS](https://www.gnu.org/software/rcs/) arbeitet nach dem Prinzip, dass
für jede Änderung ein Patch (ein Patch umfasst alle Änderungen an einer
oder mehreren Dateien) in einem speziellen Format auf der Festplatte
gespeichert wird. Um eine bestimmte Version einer Datei
wiederherzustellen, wendet es alle Patches bis zur gewünschten Version
an und rekonstruiert damit die Datei in der gewünschten Version.

### Zentrale Versionsverwaltung {#_zentrale_versionsverwaltung}

[]{.indexterm primary="Versionsverwaltung" secondary="zentral"} Ein
weiteres großes Problem, mit dem sich viele Leute dann konfrontiert
sahen, bestand in der Zusammenarbeit mit anderen Entwicklern auf anderen
Systemen. Um dieses Problem zu lösen, wurden zentralisierte
Versionsverwaltungssysteme entwickelt (engl. Centralized Version Control
System, CVCS). Diese Systeme, wozu beispielsweise CVS, Subversion und
Perforce gehören, basieren auf einem zentralen Server, der alle
versionierten Dateien verwaltet. Die Clients können die Dateien von
diesem zentralen Ort abholen und auf ihren PC übertragen. Den Vorgang
des Abholens nennt man Auschecken (engl. to check out). []{.indexterm
primary="CVS"}[]{.indexterm primary="Subversion"}[]{.indexterm
primary="Perforce"} Diese Art von System war über viele Jahre hinweg der
Standard für Versionsverwaltungssysteme.

<figure>
<img src="images/centralized.png"
alt="Centralized version control diagram" />
<figcaption>Zentrale Versionsverwaltung</figcaption>
</figure>

Dieser Ansatz hat viele Vorteile, besonders gegenüber lokalen
Versionsverwaltungssystemen. Zum Beispiel weiß jeder mehr oder weniger
genau darüber Bescheid, was andere an einem Projekt Beteiligte gerade
tun. Administratoren haben die Möglichkeit, detailliert festzulegen, wer
was tun kann. Und es ist sehr viel einfacher, ein CVCS zu administrieren
als lokale Datenbanken auf jedem einzelnen Anwenderrechner zu verwalten.

Allerdings hat dieser Aufbau auch einige erhebliche Nachteile. Der
offensichtlichste Nachteil ist das Risiko eines Systemausfalls bei
Ausfall einer einzelnen Komponente, nämlich dann, wenn der
zentralisierte Server ausfällt. Wenn dieser Server nur für eine Stunde
nicht verfügbar ist, dann kann in dieser einen Stunde niemand in
irgendeiner Form mit anderen zusammenarbeiten oder Dateien, an denen
gerade gearbeitet wird, versioniert abspeichern. Wenn die auf dem
zentralen Server eingesetzte Festplatte beschädigt wird und keine
Sicherheitskopien erstellt wurden, dann sind all diese Daten
unwiederbringlich verloren -- die komplette Historie des Projektes,
abgesehen natürlich von dem jeweiligen Zustand, den Mitarbeiter gerade
zufällig auf ihrem Rechner noch vorliegen haben. Lokale
Versionskontrollsysteme haben natürlich dasselbe Problem: Wenn man die
Historie eines Projektes an einer einzigen, zentralen Stelle verwaltet,
riskiert man, sie vollständig zu verlieren, wenn irgendetwas an dieser
zentralen Stelle ernsthaft schief läuft.

### Verteilte Versionsverwaltung {#_verteilte_versionsverwaltung}

[]{.indexterm primary="Versionsverwaltung" secondary="verteilt"} Und an
dieser Stelle kommen verteilte Versionsverwaltungssysteme (engl.
Distributed Version Control System, DVCS) ins Spiel. In einem DVCS (wie
z.B. Git, Mercurial, Bazaar oder Darcs) erhalten Anwender nicht einfach
nur den jeweils letzten Zustand des Projektes von einem Server: Sie
erhalten stattdessen eine vollständige Kopie des Repositorys. Auf diese
Weise kann, wenn ein Server beschädigt wird, jedes beliebige Repository
von jedem beliebigen Anwenderrechner zurückkopiert werden und der Server
so wiederhergestellt werden. Jede Kopie, ein sogenannter Klon (engl.
clone), ist ein vollständiges Backup der gesamten Projektdaten.

<figure>
<img src="images/distributed.png"
alt="Distributed version control diagram" />
<figcaption>Verteilte Versionsverwaltung</figcaption>
</figure>

Darüber hinaus können derartige Systeme hervorragend mit verschiedenen
externen Repositorys, sogenannten Remote-Repositorys, umgehen, sodass
man mit verschiedenen Gruppen von Leuten simultan auf verschiedene Art
und Weise an einem Projekt zusammenarbeiten kann. Damit ist es möglich,
verschiedene Arten von Arbeitsabläufen zu erstellen und anzuwenden,
welche mit zentralisierten Systemen nicht möglich wären. Dazu gehören
zum Beispiel hierarchische Arbeitsabläufe.

## Kurzer Überblick über die Historie von Git {#_kurzer_überblick_über_die_historie_von_git}

Wie viele großartige Dinge im Leben, entstand Git aus einer Prise
kreativem Chaos und hitziger Diskussion.

Der Linux-Kernel ist ein Open-Source-Software-Projekt von erheblichem
Umfang.[]{.indexterm primary="Linux"} Während der frühen Jahre der
Linux-Kernel Entwicklung (1991 - 2002) wurden Änderungen an diesem, in
Form von Patches und archivierten Dateien, herumgereicht. 2002 begann
man dann, ein proprietäres DVCS mit dem Namen Bitkeeper zu
verwenden.[]{.indexterm primary="BitKeeper"}

2005 ging die Beziehung zwischen der Community, die den Linux-Kernel
entwickelte, und des kommerziell ausgerichteten Unternehmens, das
BitKeeper entwickelte, in die Brüche. Die zuvor ausgesprochene
Erlaubnis, BitKeeper kostenlos zu verwenden, wurde widerrufen. Dies war
für die Linux-Entwickler-Community (und besonders für Linus Torvalds,
dem Erfinder von Linux) der Auslöser dafür, ein eigenes Tool zu
entwickeln, das auf den Erfahrungen mit BitKeeper basierte.[]{.indexterm
primary="Linus Torvalds"} Die Ziele des neuen Systems waren unter
anderem:

-   Geschwindigkeit

-   Einfaches Design

-   Gute Unterstützung von nicht-linearer Entwicklung (tausende
    parallele Entwicklungszweige)

-   Vollständig dezentrale Struktur

-   Fähigkeit, große Projekte, wie den Linux Kernel, effektiv zu
    verwalten (Geschwindigkeit und Datenumfang)

Seit seiner Geburt 2005 entwickelte sich Git kontinuierlich weiter und
reifte zu einem System heran, das einfach zu bedienen ist, die
ursprünglichen Ziele dabei aber weiter beibehält. Es ist unglaublich
schnell, äußerst effizient, wenn es um große Projekte geht, und es hat
ein fantastisches Branching-Konzept für nicht-lineare Entwicklung (siehe
Kapitel 3 [Git Branching](ch03-git-branching.md#ch03-git-branching)).

## Was ist Git? {#what_is_git_section}

Also, was ist Git in Kürze? Das ist ein wichtiger Teil, den es zu
beachten gilt, denn wenn Sie verstehen, was Git und das grundlegenden
Konzept seiner Arbeitsweise ist, dann wird die effektive Nutzung von Git
für Sie wahrscheinlich viel einfacher sein. Wenn Sie Git erlernen,
versuchen Sie, Ihren Kopf von den Dingen zu befreien, die Sie über
andere VCSs wissen, wie CVS, Subversion oder Perforce -- das wird Ihnen
helfen, unangenehme Missverständnisse bei der Verwendung des Tools zu
vermeiden. Auch wenn die Benutzeroberfläche von Git diesen anderen VCSs
sehr ähnlich ist, speichert und betrachtet Git Informationen auf eine
ganz andere Weise, und das Verständnis dieser Unterschiede hilft Ihnen
Unklarheiten bei der Verwendung zu vermeiden.[]{.indexterm
primary="Subversion"}[]{.indexterm primary="Perforce"}

### Snapshots statt Unterschiede {#_snapshots_statt_unterschiede}

Der Hauptunterschied zwischen Git und anderen
Versionsverwaltungssystemen (insbesondere auch Subversion und
vergleichbaren Systemen) besteht in der Art und Weise wie Git die Daten
betrachtet. Konzeptionell speichern die meisten anderen Systeme
Informationen als eine Liste von dateibasierten Änderungen. Diese
Systeme (CVS, Subversion, Perforce, Bazaar usw.) betrachten die
Informationen, die sie verwalten, als eine Reihe von Dateien an denen im
Laufe der Zeit Änderungen vorgenommen werden (dies wird allgemein als
*deltabasierte* Versionskontrolle bezeichnet).

<figure>
<img src="images/deltas.png"
alt="Storing data as changes to a base version of each file" />
<figcaption>Speichern von Daten als Änderung an einzelnen Dateien auf
Basis einer Ursprungsdatei</figcaption>
</figure>

Git arbeitet nicht auf diese Art und Weise. Stattdessen betrachtet Git
seine Daten eher wie eine Reihe von Schnappschüssen eines
Mini-Dateisystems. Git macht jedes Mal, wenn Sie den Status Ihres
Projekts committen, das heißt den gegenwärtigen Status Ihres Projekts
als eine Version in Git speichern, ein Abbild von all Ihren Dateien wie
sie gerade aussehen und speichert einen Verweis in diesem Schnappschuss.
Um dies möglichst effizient und schnell tun zu können, kopiert Git
unveränderte Dateien nicht, sondern legt lediglich eine Verknüpfung zu
der vorherigen Version der Datei an. Git betrachtet seine Daten eher wie
einen **Stapel von Schnappschüssen**.

<figure>
<img src="images/snapshots.png"
alt="Git stores data as snapshots of the project over time" />
<figcaption>Speichern der Daten-Historie eines Projekts in Form von
Schnappschüsse</figcaption>
</figure>

Das ist ein wichtiger Unterschied zwischen Git und praktisch allen
anderen Versionsverwaltungssystemen. In Git wurden daher fast alle
Aspekte der Versionsverwaltung neu überdacht, die in anderen Systemen
mehr oder weniger von ihrer jeweiligen Vorgängergeneration übernommen
worden waren. Git arbeitet im Großen und Ganzen eher wie ein mit einigen
unglaublich mächtigen Werkzeugen ausgerüstetes Mini-Dateisystem, statt
nur als Versionsverwaltungssystem. Auf einige der Vorteile, die es mit
sich bringt, Daten in dieser Weise zu verwalten, werden wir in Kapitel
3, [Git Branching](ch03-git-branching.md#ch03-git-branching), eingehen,
wenn wir das Git Branching-Konzept kennenlernen.

### Fast jede Funktion arbeitet lokal {#_fast_jede_funktion_arbeitet_lokal}

Die meisten Aktionen in Git benötigen nur lokale Dateien und Ressourcen,
um ausgeführt zu werden -- im Allgemeinen werden keine Informationen von
einem anderen Computer in Ihrem Netzwerk benötigt. Wenn Sie mit einem
CVCS vertraut sind, bei dem die meisten Operationen durch Overhead eine
Netzwerk-Latenz haben, dann wird diese Eigenschaft von Git Sie glauben
lassen, dass Git von „Gottes Segen" mit übernatürlichen Kräften bedacht
wurde. Die allermeisten Operationen können nahezu ohne jede Verzögerung
ausgeführt werden, da die vollständige Historie eines Projekts bereits
auf dem jeweiligen Rechner verfügbar ist.

Um beispielsweise die Historie des Projekts zu durchsuchen, braucht Git
sie nicht von einem externen Server zu holen -- es liest diese einfach
aus der lokalen Datenbank. Das heißt, die vollständige Projekthistorie
ist ohne jede Verzögerung verfügbar. Wenn man sich die Änderungen einer
aktuellen Version einer Datei im Vergleich zu vor einem Monat anschauen
möchte, dann kann Git den Stand von vor einem Monat in der lokalen
Historie nachschlagen und einen lokalen Vergleich zur vorliegenden Datei
durchführen. Für diesen Anwendungsfall benötigt Git keinen externen
Server, weder um Dateien dort nachzuschlagen, noch um Unterschiede dort
bestimmen zu lassen.

Das bedeutet natürlich außerdem, dass es fast nichts gibt, was man nicht
tun kann, nur weil man gerade offline ist oder keinen Zugriff auf ein
VPN hat. Wenn man also gerade im Flugzeug oder Zug ein wenig arbeiten
will, kann man problemlos seine Arbeit einchecken und dann, wenn man
wieder mit einem Netzwerk verbunden ist, die Dateien auf einen Server
hochladen. Wenn man zu Hause sitzt, aber der VPN-Client gerade mal
wieder nicht funktioniert, kann man immer noch arbeiten. Bei vielen
anderen Systemen wäre dies unmöglich oder äußerst kompliziert
umzusetzen. In Perforce können Sie beispielsweise nicht viel tun, wenn
Sie nicht mit dem Server verbunden sind; in Subversion und CVS können
Sie Dateien bearbeiten, aber Sie können keine Änderungen zu Ihren Daten
übertragen (weil die Datenbank offline ist). Das scheint keine große
Sache zu sein, aber Sie werden überrascht sein, was für einen großen
Unterschied es machen kann.

### Git stellt Integrität sicher {#_git_stellt_integrität_sicher}

Von allen zu speichernden Daten berechnet Git Prüfsummen (engl.
checksum) und speichert diese als Referenz zusammen mit den Daten ab.
Das macht es unmöglich, dass sich Inhalte von Dateien oder
Verzeichnissen ändern, ohne dass Git das mitbekommt. Git basiert auf
dieser Funktionalität und sie ist ein integraler Teil der
Git-Philosophie. Man kann Informationen deshalb z.B. nicht während der
Übermittlung verlieren oder unwissentlich beschädigte Dateien verwenden,
ohne dass Git in der Lage wäre, dies festzustellen.

Der Mechanismus, den Git verwendet, um diese Prüfsummen zu erstellen,
heißt SHA-1-Hash.[]{.indexterm primary="SHA-1"} Eine solche Prüfsumme
ist eine 40-Zeichen lange Zeichenkette, die aus hexadezimalen Zeichen
(0-9 und a-f) besteht und wird von Git aus den Inhalten einer Datei oder
Verzeichnisstruktur berechnet. Ein SHA-1-Hash sieht wie folgt aus:

    24b9da6552252987aa493b52f8696cd6d3b00373

Diese Hashes begegnen einem überall bei der Arbeit, weil sie so
ausgiebig von Git genutzt werden. Tatsächlich speichert Git alles in
seiner Datenbank nicht nach Dateinamen, sondern nach dem Hash-Wert
seines Inhalts.

### Git fügt im Regelfall nur Daten hinzu {#_git_fügt_im_regelfall_nur_daten_hinzu}

Wenn Sie Aktionen in Git durchführen, werden fast alle von ihnen nur
Daten zur Git-Datenbank *hinzufügen*. Deshalb ist es sehr schwer, das
System dazu zu bewegen, irgendetwas zu tun, das nicht wieder rückgängig
zu machen ist, oder dazu, Daten in irgendeiner Form zu löschen. Wie in
jedem anderen VCS, kann man in Git Daten verlieren oder durcheinander
bringen, solange man sie noch nicht eingecheckt hat. Aber sobald man
einen Schnappschuss in Git eingecheckt hat, ist es sehr schwierig, diese
Daten wieder zu verlieren, insbesondere wenn man regelmäßig seine lokale
Datenbank auf ein anderes Repository hochlädt.

Unter anderem deshalb macht es so viel Spaß mit Git zu arbeiten. Man
weiß genau, man kann ein wenig experimentieren, ohne befürchten zu
müssen, irgendetwas zu zerstören oder durcheinander zu bringen. Wer sich
genauer dafür interessiert, wie Git Daten speichert und wie man Daten,
die scheinbar verloren sind, wiederherstellen kann, dem wird das Kapitel
2, [Ungewollte Änderungen rückgängig
machen]( ch02-git-basics-chapter.md#_undoing), ans Herz gelegt.

### Die drei Zustände {#_die_drei_zustände}

Jetzt heißt es aufgepasst! Es folgt die wichtigste Information, die man
sich merken muss, wenn man Git erlernen und dabei Fallstricke vermeiden
will. Git definiert drei Hauptzustände, in denen sich eine Datei
befinden kann: committet (engl. *committed*), geändert (engl.
*modified*) und für Commit vorgemerkt (engl. *staged*).

-   **Modified** bedeutet, dass eine Datei geändert, aber noch nicht in
    die lokale Datenbank eingecheckt wurde.

-   **Staged** bedeutet, dass eine geänderte Datei in ihrem
    gegenwärtigen Zustand für den nächsten Commit vorgemerkt ist.

-   **Committed** bedeutet, dass die Daten sicher in der lokalen
    Datenbank gespeichert sind.

Das führt uns zu den drei Hauptbereichen eines Git-Projekts: dem
Verzeichnisbaum, der sogenannten Staging-Area und dem Git-Verzeichnis.

<figure>
<img src="images/areas.png"
alt="Working tree, staging area, and Git directory." />
<figcaption>Verzeichnisbaum, Staging-Area und
Git-Verzeichnis</figcaption>
</figure>

Der Verzeichnisbaum ist ein einzelner Abschnitt einer Version des
Projekts. Diese Dateien werden aus der komprimierten Datenbank im
Git-Verzeichnis geholt und auf der Festplatte so abgelegt, damit Sie sie
verwenden oder ändern können.

Die Staging-Area ist in der Regel eine Datei, die sich in Ihrem
Git-Verzeichnis befindet und Informationen darüber speichert, was in
Ihren nächsten Commit einfließen soll. Der technische Name im
Git-Sprachgebrauch ist „Index", aber der Ausdruck „Staging-Area"
funktioniert genauso gut.

Im Git-Verzeichnis werden die Metadaten und die Objektdatenbank für Ihr
Projekt gespeichert. Das ist der wichtigste Teil von Git, dieser Teil
wird kopiert, wenn man ein Repository von einem anderen Rechner *klont*.

Der grundlegende Git-Arbeitsablauf sieht in etwa so aus:

1.  Sie ändern Dateien in Ihrem Verzeichnisbaum.

2.  Sie stellen selektiv Änderungen bereit, die Sie bei Ihrem nächsten
    Commit berücksichtigen möchten, wodurch nur diese Änderungen in den
    Staging-Bereich aufgenommen werden.

3.  Sie führen einen Commit aus, der die Dateien so übernimmt, wie sie
    sich in der Staging-Area befinden und diesen Snapshot dauerhaft in
    Ihrem Git-Verzeichnis speichert.

Wenn sich eine bestimmte Version einer Datei im Git-Verzeichnis
befindet, wird sie als *committed* betrachtet. Wenn sie geändert und in
die Staging-Area hinzugefügt wurde, gilt sie als für den Commit
*vorgemerkt* (engl. *staged*). Und wenn sie geändert, aber noch nicht
zur Staging-Area hinzugefügt wurde, gilt sie als *geändert* (engl.
*modified*). Im Kapitel 2, [Git
Grundlagen]( ch02-git-basics-chapter.md#ch02-git-basics-chapter), werden
diese drei Zustände noch näher erläutert und wie man diese sinnvoll
einsetzen kann oder alternativ, wie man den Zwischenschritt der
Staging-Area überspringen kann.

## Die Kommandozeile {#_die_kommandozeile}

Es gibt viele verschiedene Möglichkeiten Git einzusetzen. Auf der einen
Seite gibt es die Werkzeuge, die per Kommandozeile bedient werden und
auf der anderen, die vielen grafischen Benutzeroberflächen (engl.
graphical user interface, GUI), die sich im Leistungsumfang
unterscheiden. In diesem Buch verwenden wir die Kommandozeile. In der
Kommandozeile können nämlich wirklich **alle** vorhandenen Git Befehle
ausgeführt werden. Bei den meisten grafischen Oberflächen ist dies nicht
möglich, da aus Gründen der Einfachheit nur ein Teil der
Git-Funktionalitäten zur Verfügung gestellt werden. Wenn Sie sich in der
Kommandozeilenversion von Git auskennen, finden Sie sich wahrscheinlich
auch in einer GUI-Version relativ schnell zurecht, aber andersherum muss
das nicht unbedingt zutreffen. Außerdem ist die Wahl der grafischen
Oberfläche eher Geschmackssache, wohingegen die Kommandozeilenversion
auf jedem Rechner installiert und verfügbar ist.

In diesem Buch gehen wir deshalb davon aus, dass Sie wissen, wie man bei
einem Mac ein Terminal öffnet, oder wie man unter Windows die
Eingabeaufforderung oder die Powershell öffnet. Sollten Sie jetzt nur
Bahnhof verstehen, sollten Sie an dieser Stelle abbrechen und sich
schlau machen, was ein Terminal bzw. eine Eingabeaufforderung ist und
wie man diese bedient. Nur so ist sichergestellt, dass Sie unseren
Beispielen und Ausführungen im weiteren Verlauf des Buches folgen
können.

## Git installieren {#_git_installieren}

Bevor Sie mit Git loslegen können, muss es natürlich zuerst installiert
werden. Auch wenn es bereits vorhanden ist, ist es vermutlich eine gute
Idee, auf die neueste Version zu aktualisieren. Sie können es entweder
als Paket oder über ein anderes Installationsprogramm installieren oder
den Quellcode herunterladen und selbst kompilieren.

::: note
Dieses Buch wurde auf Basis der Git-Version **2** geschrieben. Da Git
hervorragend darin ist, die Abwärtskompatibilität aufrechtzuerhalten,
sollte jede neuere Version problemlos funktionieren. Auch wenn die
meisten Befehle, die wir anwenden werden, auch in älteren Versionen
funktionieren, kann es doch sein, dass die Befehlsausgabe oder das
Verhalten leicht anders ist.
:::

### Installation unter Linux {#_installation_unter_linux}

[]{.indexterm primary="Linux" secondary="Installation"} Wenn Sie die
grundlegenden Git-Tools unter Linux über ein Installationsprogramm
installieren möchten, können Sie das in der Regel über das
Paketverwaltungstool der Distribution tun. Wenn Sie mit Fedora (oder
einer anderen eng damit verbundenen RPM-basierten Distribution, wie RHEL
oder CentOS) arbeiten, können Sie `dnf` verwenden:

``` console
$ sudo dnf install git-all
```

Auf einem Debian-basierten System, wie Ubuntu, steht `apt` zur
Verfügung:

``` console
$ sudo apt install git-all
```

Auf der Git-Homepage <https://git-scm.com/download/linux> findet man
weitere Möglichkeiten und Optionen, wie man Git unter einem
Unix-basierten Betriebssystem installieren kann.

### Installation unter macOS {#_installation_unter_macos}

[]{.indexterm primary="macOS" secondary="Installation"} Es gibt mehrere
Möglichkeiten, Git auf einem Mac zu installieren. Am einfachsten ist es
wahrscheinlich, die Xcode Command Line Tools zu
installieren.[]{.indexterm primary="Xcode"} Bei Mavericks (10.9) oder
neueren Versionen kann man dazu einfach `git` im Terminal eingeben.

``` console
$ git --version
```

Wenn Git noch nicht installiert ist, erscheint eine Abfrage, ob man es
installieren möchte.

Wenn man eine sehr aktuelle Version einsetzen möchte, kann man Git auch
über ein Installationsprogramm installieren. Auf der Git-Website
<https://git-scm.com/download/mac> findet man die jeweils aktuellste
Version und kann sie von dort herunterladen.

<figure>
<img src="images/git-osx-installer.png" alt="Git macOS installer" />
<figcaption>Git macOS Installationsprogramm</figcaption>
</figure>

### Installation unter Windows {#_installation_unter_windows}

Auch für Windows gibt es einige, wenige Möglichkeiten zur Installation
von Git.[]{.indexterm primary="Windows" secondary="Installation"} Eine
offizielle Windows-Version findet man direkt auf der Git-Homepage. Gehen
Sie dazu auf <https://git-scm.com/download/win> und der Download sollte
dann automatisch starten. Man sollte dabei beachten, dass es sich
hierbei um das Projekt „Git for Windows" handelt, welches unabhängig von
Git selbst ist. Weitere Informationen hierzu finden Sie unter
<https://msysgit.github.io/>.

Um eine automatisierte Installation zu erhalten, können Sie das [Git
Chocolatey Paket](https://chocolatey.org/packages/git) verwenden.
Beachten Sie, dass das Chocolatey-Paket von der Community gepflegt wird.

### Aus dem Quellcode installieren {#_aus_dem_quellcode_installieren}

Viele Leute kompilieren Git auch auf ihrem eigenen Rechner, weil sie
damit die jeweils aktuellste Version erhalten. Die vorbereiteten Pakete
hinken meist ein wenig hinterher. Heutzutage ist dies nicht mehr ganz so
schlimm, da Git einen gewissen Reifegrad erfahren hat.

Wenn Sie Git aus dem Quellcode installieren möchten, benötigen Sie die
folgenden Bibliotheken, von denen Git abhängt: autotools, curl, zlib,
openssl, expat und libiconv. Wenn Sie sich beispielsweise auf einem
System befinden, das Paketverwaltungen, wie `dnf` (Fedora) oder
`apt-get` (ein Debian-basiertes System) hat, können Sie mit einem dieser
Befehle die minimalen Abhängigkeiten für die Kompilierung und
Installation der Git-Binärdateien installieren:

``` console
$ sudo dnf install dh-autoreconf curl-devel expat-devel gettext-devel \
  openssl-devel perl-devel zlib-devel
$ sudo apt-get install dh-autoreconf libcurl4-gnutls-dev libexpat1-dev \
  gettext libz-dev libssl-dev
```

Um die Dokumentation in verschiedenen Formaten (doc, html, info) zu
erstellen, sind weitere Abhängigkeiten notwendig:

``` console
$ sudo dnf install asciidoc xmlto docbook2X
$ sudo apt-get install asciidoc xmlto docbook2x
```

::: note
Benutzer von RHEL und RHEL-Derivaten wie CentOS und Scientific Linux
müssen das [EPEL-Repository
aktivieren](https://docs.fedoraproject.org/en-US/epel/#how_can_i_use_these_extra_packages),
um das Paket `docbook2X` herunterzuladen.
:::

Wenn Sie eine Debian-basierte Distribution (Debian, Ubuntu oder deren
Derivate) verwenden, benötigen Sie auch das Paket `install-info`:

``` console
$ sudo apt-get install install-info
```

Wenn Sie eine RPM-basierte Distribution (Fedora, RHEL oder deren
Derivate) verwenden, benötigen Sie auch das Paket `getopt` (welches auf
einer Debian-basierten Distribution bereits installiert ist):

``` console
$ sudo dnf install getopt
```

Wenn Sie Fedora- oder RHEL-Derivate verwenden, müssen Sie wegen der
unterschiedlichen Paketnamen zusätzlich einen Symlink erstellen, indem
Sie folgenden Befehl ausführen:

``` console
$ sudo ln -s /usr/bin/db2x_docbook2texi /usr/bin/docbook2x-texi
```

aufgrund von binären Namensunterschieden.

Wenn Sie alle notwendigen Abhängigkeiten installiert haben, können Sie
sich als nächstes die jeweils aktuellste Version als Tarball von
verschiedenen Stellen herunterladen. Man findet die Quellen auf der
Kernel.org-Website unter <https://www.kernel.org/pub/software/scm/git>,
oder einen Mirror auf der GitHub-Website unter
<https://github.com/git/git/releases>. Auf der GitHub-Seite ist es
einfacher herauszufinden, welches die jeweils aktuellste Version ist.
Auf kernel.org dagegen werden auch Signaturen zur Verifikation des
Downloads der jeweiligen Pakete zur Verfügung gestellt.

Nachdem man sich so die Quellen beschafft hat, kann man Git kompilieren
und installieren:

``` console
$ tar -zxf git-2.8.0.tar.gz
$ cd git-2.8.0
$ make configure
$ ./configure --prefix=/usr
$ make all doc info
$ sudo make install install-doc install-html install-info
```

Jetzt, nachdem Git installiert ist, kann man sich Git-Updates auch per
Git beschaffen:

``` console
$ git clone https://git.kernel.org/pub/scm/git/git.git
```

## Git Basis-Konfiguration {#_first_time}

Nachdem Git jetzt auf Ihrem System installiert ist, sollten Sie Ihre
Git-Konfiguration noch anpassen. Dies muss nur einmalig auf dem
jeweiligen System durchgeführt werden. Die Konfiguration bleibt
bestehen, wenn Sie Git auf eine neuere Version aktualisieren. Die
Git-Konfiguration kann auch jederzeit geändert werden, indem die
nachfolgenden Befehle einfach noch einmal ausgeführt werden.

Git umfasst das Werkzeug `git config`, welches die Möglichkeit bietet,
Konfigurationswerte zu verändern. Auf diese Weise können Sie anpassen,
wie Git aussieht und arbeitet.[]{.indexterm primary="Git Befehle"
secondary="config"} Die Konfiguration ist an drei verschiedenen Orten
gespeichert:

1.  Die Datei `[path]/etc/gitconfig`: enthält Werte, die für jeden
    Benutzer auf dem System und alle seine Repositorys gelten. Wenn Sie
    die Option `--system` an `git config` übergeben, liest und schreibt
    sie spezifisch aus dieser Datei. Da es sich um eine
    Systemkonfigurationsdatei handelt, benötigen Sie Administrator- oder
    Superuser-Rechte, um Änderungen daran vorzunehmen.

2.  Die Datei `~/.gitconfig` oder `~/.config/git/config`: enthält Werte,
    die für Sie, den Benutzer, persönlich bestimmt sind. Sie können Git
    dazu bringen, diese Datei gezielt zu lesen und zu schreiben, indem
    Sie die Option `--global` übergeben, und dies betrifft *alle* der
    Repositorys, mit denen Sie auf Ihrem System arbeiten.

3.  Die Datei `config` im Git-Verzeichnis (also `.git/config`) des
    jeweiligen Repositorys, das Sie gerade verwenden: Sie können Git mit
    der Option `--local` zwingen, aus dieser Datei zu lesen und in sie
    zu schreiben, das ist in der Regel die Standardoption. (Es dürfte
    Sie nicht überraschen, dass Sie sich irgendwo in einem
    Git-Repository befinden müssen, damit diese Option ordnungsgemäß
    funktioniert.)

        Jede Ebene überschreibt Werte der vorherigen Ebene, so dass Werte in `.git/config` diejenigen in `[path]/etc/gitconfig` aushebeln.

Auf Windows-Systemen sucht Git nach der Datei `.gitconfig` im `$HOME`
Verzeichnis (normalerweise zeigt `$HOME` bei den meisten Systemen auf
`C:\Users\$USER`). Git schaut immer nach `[path]/etc/gitconfig`, auch
wenn die sich relativ zu dem MSys-Wurzelverzeichnis befindet, dem
Verzeichnis in das Sie Git installiert haben. Wenn Sie eine Version 2.x
oder neuer von Git für Windows verwenden, gibt es auch eine
Konfigurationsdatei auf Systemebene unter
`C:\Dokumente und Einstellungen\Alle Benutzer\Anwendungsdaten\Git\config`
unter Windows XP und unter `C:\ProgramData\Git\config` unter Windows
Vista und neuer. Diese Konfigurationsdatei kann nur von
`git config -f <file>` als Admin geändert werden.

Sie können sich alle Ihre Einstellungen ansehen sehen, wo sie herkommen:

``` console
$ git config --list --show-origin
```

### Ihre Identität {#_ihre_identität}

Nachdem Sie Git installiert haben, sollten Sie als allererstes Ihren
Namen und Ihre E-Mail-Adresse in Git konfigurieren. Das ist insofern
wichtig, da jeder Git-Commit diese Informationen verwendet und sie
unveränderlich in die Commits eingearbeitet werden, die Sie erstellen:

``` console
$ git config --global user.name "John Doe"
$ git config --global user.email johndoe@example.com
```

Wie schon erwähnt, brauchen Sie diese Konfiguration nur einmal
vorzunehmen, wenn Sie die Option `--global` verwenden. Auf Grund der
oben erwähnten Prioritäten gilt das dann für alle Ihre Projekte. Wenn
Sie für ein spezielles Projekt einen anderen Namen oder eine andere
E-Mail-Adresse verwenden möchten, können Sie den Befehl ohne die Option
`--global` innerhalb des Projektes ausführen.

Viele der grafischen Oberflächen helfen einem bei diesem Schritt, wenn
Sie sie zum ersten Mal ausführen.

### Ihr Editor {#_editor}

Jetzt, da Ihre Identität eingerichtet ist, können Sie den
Standard-Texteditor konfigurieren, der verwendet wird, wenn Git Sie zum
Eingeben einer Nachricht auffordert. Normalerweise verwendet Git den
Standard-Texteditor des jeweiligen Systems.

Wenn Sie einen anderen Texteditor, z.B. Emacs, verwenden wollen, können
Sie das wie folgt festlegen:

``` console
$ git config --global core.editor emacs
```

Wenn Sie auf einem Windows-System einen anderen Texteditor verwenden
möchten, müssen Sie den vollständigen Pfad zu seiner ausführbaren Datei
angeben. Dies kann, je nachdem, wie Ihr Editor eingebunden ist,
unterschiedlich sein.

Im Falle von Notepad++, einem beliebten Programmiereditor, sollten Sie
wahrscheinlich die 32-Bit-Version verwenden, da die 64-Bit-Version zum
Zeitpunkt der Erstellung nicht alle Plug-Ins unterstützt. Beim Einsatz
auf einem 32-Bit-Windows-System oder einem 64-Bit-Editor auf einem
64-Bit-System geben Sie etwa Folgendes ein:

``` console
$ git config --global core.editor "'C:/Program Files/Notepad++/notepad++.exe' -multiInst -notabbar -nosession -noPlugin"
```

::: note
Vim, Emacs und Notepad++ sind beliebte Texteditoren, die von Entwicklern
häufig auf Unix-basierten Systemen wie Linux und macOS oder einem
Windows-System verwendet werden. Wenn Sie einen anderen Editor oder eine
32-Bit-Version verwenden, finden Sie in
[C-git-commands.xml](C-git-commands.xml#ch_core_editor) spezielle
Anweisungen, wie Sie Ihren bevorzugten Editor mit Git einrichten können.
:::

::: warning
Wenn Sie Git nicht so konfigurieren, dass es Ihren Texteditor verwendet
und Sie keine Ahnung davon haben, wie man Vim oder Emacs benutzt, werden
Sie ein wenig überfordert sein, wenn diese zum ersten Mal von Git
gestartet werden. Ein Beispiel: auf einem Windows-System kann es eine
vorzeitig beendete Git-Operation während einer von Git angestoßenen
Verarbeitung sein.
:::

### Der standardmäßige Branch-Name {#_new_default_branch}

In der Voreinstellung wird Git einen Branch mit dem Namen *master*
erzeugen, wenn Sie ein neues Repository mit `git init` erstellen. Ab der
Git-Version 2.28 können Sie einen abweichenden Namen für den initialen
Branch festlegen.

So konfigurieren Sie *main* als Vorgabe für den Branch-Namen:

``` console
$ git config --global init.defaultBranch main
```

### Einstellungen überprüfen {#_einstellungen_überprüfen}

Wenn Sie Ihre Konfigurationseinstellungen überprüfen möchten, können Sie
mit dem Befehl `git config --list` alle Einstellungen auflisten, die Git
derzeit finden kann:

``` console
$ git config --list
user.name=John Doe
user.email=johndoe@example.com
color.status=auto
color.branch=auto
color.interactive=auto
color.diff=auto
...
```

Manche Parameter werden möglicherweise mehrfach aufgelistet, weil Git
denselben Parameter in verschiedenen Dateien (z.B.
`[path]/etc/gitconfig` und `~/.gitconfig`) gefunden hat. In diesem Fall
verwendet Git dann den jeweils zuletzt aufgelisteten Wert.

Außerdem können Sie mit dem Befehl `git config <key>` prüfen, welchen
Wert Git für einen bestimmten Parameter verwendet:[]{.indexterm
primary="Git Befehle" secondary="config"}

``` console
$ git config user.name
John Doe
```

::: note
Da Git möglicherweise den gleichen Wert der Konfigurationsvariablen aus
mehr als einer Datei liest, ist es möglich, dass Sie einen unerwarteten
Wert für einen dieser Werte haben und nicht wissen, warum. In solchen
Fällen können Sie Git nach dem *Ursprung* (engl. origin) für diesen Wert
fragen, und es wird Ihnen sagen, mit welcher Konfigurationsdatei der
Wert letztendlich festgelegt wurde:

``` console
$ git config --show-origin rerere.autoUpdate
file:/home/johndoe/.gitconfig   false
```
:::

## Hilfe finden {#_git_help}

Falls Sie einmal Hilfe bei der Anwendung von Git benötigen, gibt es drei
Möglichkeiten, die entsprechende Seite aus der Dokumentation (engl.
manpage) für jeden Git-Befehl anzuzeigen:

``` console
$ git help <verb>
$ git <verb> --help
$ man git-<verb>
```

Beispielweise erhalten Sie die Hilfeseite für den Befehl `git config`
folgendermaßen:[]{.indexterm primary="Git Befehle" secondary="help"}

``` console
$ git help config
```

Diese Befehle sind nützlich, weil Sie sich die Hilfe jederzeit anzeigen
lassen können, auch wenn Sie einmal offline sind. Wenn die Hilfeseiten
und dieses Buch nicht ausreichen und Sie persönliche Hilfe brauchen,
können Sie einen der Kanäle `#git`, `#github` oder `#gitlab` auf dem
Libera-Chat-IRC-Server probieren, der unter <https://libera.chat/> zu
finden ist. Diese Kanäle sind in der Regel sehr gut besucht.
Normalerweise findet sich unter den vielen Anwendern, die oft sehr viel
Erfahrung mit Git haben, irgendjemand, der Ihnen weiterhelfen
kann.[]{.indexterm primary="IRC-Chat"}

Wenn Sie nicht die vollständige Manpage-Hilfe benötigen, sondern nur
eine kurze Beschreibung der verfügbaren Optionen für einen Git-Befehl,
können Sie auch in den kompakteren „Hilfeseiten" mit der Option `-h`
nachschauen, wie in:

``` console
$ git add -h
usage: git add [<options>] [--] <pathspec>...

    -n, --dry-run               dry run
    -v, --verbose               be verbose

    -i, --interactive           interactive picking
    -p, --patch                 select hunks interactively
    -e, --edit                  edit current diff and apply
    -f, --force                 allow adding otherwise ignored files
    -u, --update                update tracked files
    --renormalize               renormalize EOL of tracked files (implies -u)
    -N, --intent-to-add         record only the fact that the path will be added later
    -A, --all                   add changes from all tracked and untracked files
    --ignore-removal            ignore paths removed in the working tree (same as --no-all)
    --refresh                   don't add, only refresh the index
    --ignore-errors             just skip files which cannot be added because of errors
    --ignore-missing            check if - even missing - files are ignored in dry run
    --chmod (+|-)x              override the executable bit of the listed files
    --pathspec-from-file <file> read pathspec from file
    --pathspec-file-nul         with --pathspec-from-file, pathspec elements are separated with NUL character
```

## Zusammenfassung {#_zusammenfassung}

Sie sollten jetzt ein grundlegendes Verständnis dafür haben, was Git ist
und wie es sich von anderen zentralisierten Versionsverwaltungssystemen
unterscheidet, die Sie evtl. bisher eingesetzt haben. Sie sollten nun
auch eine funktionierende Version von Git auf Ihrem System haben, die
mit Ihrer persönlichen Identität eingerichtet ist. Es ist jetzt an der
Zeit, einige Grundlagen von Git zu lernen.
