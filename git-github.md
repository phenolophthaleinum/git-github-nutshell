


# git & github in a nutshell
## git
**Git jest najpopularniejszym rozproszonym systemem kontroli wersji. Pozwala on na**
- **śledzenie zmian kodu (plików),** 
- **śledzenie tego, kto te zmiany wykonał,** 
- **synchronizację kodu (plików)**
### Możliwości git (te najważniejsze)
-   zarządzanie projektami poprzez  **repozytoria (repository)**
-   **Klonowanie (clone)**  projektów, dzięki czemu pracuje się na lokalnej kopii
-   Kontrola i śledzenie zmian poprzez wersjonowanie **wersjonowanie (wystawianie do kontroli wersji  -staging)** i wprowadzanie zmian **(committing)**
-   Możliwość tworzenia **gałęzi projektów (branch)**, które pozwalają dzielić projekt na kilka osobnych stanów, w których mogą być wykonywane zmiany, które są niewidoczne pomiędzy tymi gałęziami
- Możliwość **łączenia gałęzi (merge)**, co pozwala na łączenie i synchronizację stanów projektów i zmian w nich zachodzących
-   **Przeciąganie (pull)**  najnowszych wersji projektu do lokalnej kopii
-   **Wypychanie (push)**  lokalnych zmian do głównego projektu (czyli nie kopii)

> Od tej pory będę używać nazw angielskich, bo polskie wprowadzają tylko zamieszanie i są tutaj wytworzone na siłę
### Przegląd
#### Stany plików
Zanim można przejść do wdrożenia gita do projektu, dobrze jest wiedzieć, jak git zarządza plikami. Git rozpoznaje trzy stany plików i na podstawie tego wie, co może z tymi plikami robić. Te stany to:
 -   **zmodyfikowany**  – plik był edytowany, ale zmiana o tym nie została jeszcze nigdzie zapisana;
 -   **śledzony**  – zmodyfikowany plik został oznaczony do zatwierdzenia przy najbliższej operacji  _commit;_
 -   **zatwierdzony**  – dokonana zmiana została zapisana i utrwalona w lokalnej bazie danych;
Dzięki tym stanom wiadomo, gdzie pliki mogą się aktualnie znajdować. Rozróżnić można następujace stany git:
![Stany gita; source: stormit.pl/git; author: Tomasz Woliński](http://stormit.pl/wp-content/uploads/stany-git.png)
> source: stormit.pl/git; author: Tomasz Woliński
#### Tworzenie repozytorium
Aby wdorżyć git do folderu projektu należy wykonać polecenie:
```bash
git init "sciezka projektu"
```
To sprawi, że git zostanie aktywowany w podanym folderze przez co delikatnie zmieni się struktura folderu:
 - dodany jest **katalog git**, w którym git przechowuje metadane o plikach oraz obiektową bazę danych. Ten katalog jest kopiowany podczas klonowania repozytorium
 -   w **katalogu roboczym** jest to odtworzony obraz wersji projektu. To właśnie zawartość tego katalogu jest modyfikowana przez użytkownika.
 - **przechowalnia (stage) –** to miejsce pośrednie, między katalogiem roboczym, a lokalną bazą danych. Dzięki niej można utrwalić tylko wybrane zmiany
 - może być dodany **plik *gitignore***, który specyfikuje, jakie elementy powinny być wykluczone z wersjonowania
#### Klonowanie repozytorium
Gdy projekt nie znajduje się lokalnie na komputerze, można je sklonować do swojej maszyny poprzez polecenie:
```bash
git clone "adres do repozytorum"
```
Podczas klonowania git pobiera wszystkie pliki przechowywane w repozytorium oraz całą ich historię.
#### Staging pliku (rozpoczęcie śledzenia/wersjonowania)
Git jak najbardzej wykrywa automatyczie pliki w projekcie, natomiast nie są one śledzone, co oznacza, że nie jesteśmy ich w stanie wysłać do zdalnego repozytorum lub prowadzić historii zmian danego pliku. W tym celu należy go zacząć obserwować poprzez polecenie:
```bash
git add "sciezka do pliku"
```
Można również dodać wszystkie aktualnie nieśledzone pliki:
```bash
git add -A
```
Git operuje na pojedynczych zmianach, a nie na plikach (tego nie czuć podczas korzystania z IDE, co jest wspomniane w rodziale ["W praktyce"](#w-praktyce)).  To oznacza, że gdy zechcemy zmiany commitować (zatwierdzić zmiany z poczekalni, aby można było jest wysłać do zdalnego repozytorium) należy powtórzyć operację `git add`, aby aktualna wersja pliku zamieniła tą z poczekalni.
#### Podgląd zmian
Warto zobaczyć, czy zdeponowane zmiany pliku są odpowiednie, co realizowane jest to poprzez polecenie:
```bash
git diff
```
Przykładowo może to wyglądać tak:
```
diff --git a/User.java b/User.java
new file mode 100644
index 0000000..853d1c3
--- /dev/null
+++ b/User.java
@@ -0,0 +1,3 @@
+public class User {
+ String name;
+ bool javaEnjoyer;
+}
```
Najważniejsze informacje są tak naprawdę te od linii, która zaczyna się od `@@` , pokazujące zmiany w zawartości pliku (`+` - dodana zawartość `-` - usunięta zawartość) i miejscu edycji (np. `+1,3` oznacza, że została dodana zawartość od linii 1 przez kolejne 3 linie).

#### Commit zmian (zatwierdzenie)
Jeżeli mamy już pewność, że dokonane zmiany są poprawne, można utrwalić je w lokalnym repozytorium, co realizuje polecenie:
```bash
git commit -m "jakis komentarz"
```
#### Historia zmian
Do przeglądania historii służy polecenie:
```bash
git log
```
Domyślne wywowałanie może dać taki przykładowy wynik:
```
commit 9a21fh3hh35fea008333beea1341933dd47269b58
Author: JavaEnjoyer
Date: Fri Nov 12 13:43:37 2016 +0100
Add new field age
commit 99f1661b04d23b3411f66df9a9562824544b3d7e1
Author: JavaEnjoyer
Date: Fri Nov 12 13:53:50 2016 +0100
User initial commit
```
#### Cofanie zmian
Można cofnąć wysłane zmiany tworząc commity wycofujące. Ten mechanizm nie modyfikuje historii, a generuje commit, który jest przeciwieństwem zmiany, którą chcemy wycofać. Przykładowo:
```bash
git revert 2b5ed9d
```
![Cofnięcie commita; pogląd w SmartGit; source: stormit.pl/git; author: Tomasz Woliński](http://stormit.pl/wp-content/uploads/smartgit-revert.png)

> source: stormit.pl/git; author: Tomasz Woliński
#### Praca ze zdalnym repozytorium (głównie push oraz pull)
Aby dodać coś do zdalnego repozytorium (na githubie lub jakimś serwerze) należy najpierw dodać takie repeozytorium:
```bash
git remote add "skrot" "link do repo"
```
Następnie jesteśmy w stanie wysłać zmianę na zewnątrz, która będzie widoczna poza lokalną wersją projektu:
```bash
git push origin "nazwa galezi"
```
Aby zaaktualizować lokalny projekt, gdy wiemy, że ktoś inny mógł wprowadzić zmiany używamy następujące polecenia:
```bash
git merge "nazwa galezi"
git pull origin "nazwa galezi"
```
#### Usunięcie pliku z wersjonowania 
Zdarza się, że przez przypadek zawersjonujemy plik lub nie chcemy, aby był on dalej wersjonowany. Wtedy należy wykonać następujące polecenia:
```bash
git rm --cached "sciezka do pliku"
git commit -m "jakis komentarz"
```
Po tych akcjach plik będzie istniał w katalogu roboczym, ale nie będzie istniał w repozytorium.
#### Praca z gałęziami (git-flow) - bardzo ogólnie
Szczegółowa praca z gałęziami w terminalu jest dobrze opisana [tutaj](https://stormit.pl/git/). Aby wygodniej korzystać z tych funkcjonalności, polecam korzystanie z IDE, które najczęściej upraszczają te akcje oraz automatyzują pewne wywoływania.
Warto jednak wiedzieć, co umożliwia nam praca na gałęziach oraz jak je skutecznie wykorzystywać. Najlepiej opisze to następujący schemat, przedstawiający model zarządzania gałęziami *Git-Flow*:
![Git-flow](http://stormit.pl/wp-content/uploads/git-flow.png)
> source: stormit.pl/git; author: Tomasz Woliński

- Master
	- jedynie stabilny kod aplikacji
	- brak commitów ze zmianami kodu, a jedynie commity mergujące z gałęzi  _hotfix_  oraz  _release_
	- każda kolejna wydana wersja powinna być oznaczana nowym tagiem z wersją. 

- HotFix
	- zawsze „wystrzeliwany” z gałęzi  _master_
	- po naprawie błędu taką gałąź należy zmergować do master 
	- poprawkę można dodać do obecnie rozwijanej gałęzi  _develop,_  jeżeli chcemy, żeby już teraz została poprawiona. 
	- w tym samym czasie może być kilka gałęzi typu  _HotFix_.

- Develop
	- gałąź _develop_  wystrzeliwana jest z gałęzi  _master_  po wydaniu nowej wersji.
	- główna gałąź rozwojowa aplikacji z niestabilnym kodem. 
	- spotykają się tu i łączą wszystkie niezależnie rozwijane funkcjonalności

- Feature
	- większe funkcjonalności powinny być rozwijane na osobnych gałęziach, dzięki temu można je niezależnie rozwijać, nie przeszkadzając jednocześnie w rozwoju innych funkcjonalności. 
	- możliwość wypuszczenia nowej wersji aplikacji, bez konieczności czekania aż wszystkie rozwijane rzeczy zostaną ukończone. 
	- po zakończeniu prac kod jest mergowany do gałęzi  _develop_, gdzie testowany jest razem z innymi funkcjonalnościami.

- Release
	- po zakończeniu okresu rozwoju aplikacji, z gałęzi Develop wystrzeliwana jest nowa gałąź, tak zwany Release candidate
	- nie są już rozwijane żadne nowe funkcjonalności
	- stabilizowanie obecnie wytworzonego kodu przed ostatecznym mergem do master.
#### Konflikty
Podczas mergowania gałęzi może się zdarzyć, że git nie da rady automatycznie rozstrzygnąć, którą zawartość powinien zachować. W konfliktującym pliku git umieszcza kod z obu gałęzi ze specjalnymi znacznikami i pozostawia kwestię zachowania zawartości użytkownikowi. Przykładowo:
![Plik z konfliktem](http://stormit.pl/wp-content/uploads/git-konflikt.png)
> source: stormit.pl/git; author: Tomasz Woliński

Naturalnie, po poprawieniu kodu (tj. **zachowaniu odpowiedniego kodu i usunięciu znaczników**) należy go od razu commitować do repozytorium, co będzie równoznaczne z rozwiązaniem konfliktu.
Jeśli użytkownik korzysta z IDE, to bardzo prawdopodobne jest, że IDE pokaże nam pliki obok siebie dokładnie wskazując, gdzie są miejsca konfliktowe i rola użytkownika ograniczy się do jedynie do przycisku akceptacji lub odrzucenia dalej linii, bez martwienia się o znaczniki.
### W praktyce
Z mojego doświadczenia:
 - Najczęściej korzystam z gita zintegrowanego w IDE, w którym pisze się kod:
	 -  występuje tam zazwyczaj nakładka graficzna
	 - pewne kroki są upraszczane (korzystanie z gałęzi, commit+push, pull)
	 - automatyczny staging plików (dlatego tam wydaje się, że git działa na plikach, a tak nie jest)
	 - przystępne rozwiązywanie konfliktów
	 - wizualizacja gałęzi
- Pewne funkcje są w IDE niedostępne, bądź działają gorzej:
	- najczęściej miałem problem z usunięciem pliku z wersjonowania, dlatego korzystam wtedy wyłącznie z terminala
- Można używać graficznej wersji git, albo github (co da tę samą funkcjonalność), ale jest to mniej wygodnie niż w przypadku zintegrowanego git w IDE
- Pewnych funkcji tutaj przedstawionych nie udało mi się jeszcze użyć (np. git revert, bo najczęściej szybko można błąd naprawić i wykonać to ręcznie, chyba, że chcemy wrócić do bardzo odległej wersji, to wtedy to jest przydatne)

## github

**GitHub to platforma hostingowa do wersjonowania kodu i kolaboracji w pracy nad projektami (różnymi, nie tylko programistycznymi). Jest to w istocie sewerm na którymi deponowany jest kod z lokalnych projektów i jest on zarządzany również przez git, zatem wszystkie funkcjonalności gita są tam zawarte. Github ma wiele więcej możlwości, ponieważ jest to platforma a narzędzie**
### Możlwości github
- **zdalne zarządzanie** projektem poprzez git (są zapewnione funkcje gita)
- **pull reuqest** workflow - sposób tworzenia i recenzjonowania kodu
- kompleksowa prezentacja projektu na jej stronie (np. automatycznie wyświetlane **README**, które daje automatyczny dostęp do informacji o projekcie)
- system zgłaszania problemów, zgłaszanie zapotrzebowań funkcjonalności, tworzenia dyskusji o projekcie (zakładka **Issues**)
- system szybkiego wdrażania kodu - np. możliwość tworzenia paczki pythona z użyciem Anacondy (zakładka **Actions**)
- organizacja projektów - przydzielanie zadań, planowanie zadań, śledzenie postępu, dyskusje (zakładka **Projects**)
- możliwość stworzenia Wiki projektu, gdzie może znajdować się dokumentacja, tutoriale, cookbooki (zakładka **Wiki**)
- zarządzanie bezpieczeństwem projektu, np. poprzez automatyczne poszukiwanie niezgodności w zależnościach kodu (depedencies) - zakładka **Security**
- możliwość dostawania powiadomień o projekcie, gdy nastąpią w nim jakieś akcje oraz zapewnione są szerokie statystyki odnośnie projektu, co przyczynia się do śledzenia zmian
### Przegląd funkcji
Najprościej przejrzeć sobie jakieś repozytorium i popatrzeć jak to wszystko wygląda, ponieważ wszystko jest opakowane w ładny i raczej przejrzysty interfejs. Nie mniej, warto trochę bliżej przyjrzeć się sekcjom ***Issues*** oraz ***Pull requests***
#### Issues
Sekcja, która przypomina trochę forum dyskusyjne. Można tam tworzyć wątki związane z repozytorium pod różnym kątem, dzięki etykietom (labels), które określają, czego dotyczy dany temat (bug, question, feature, enhancement, itd.). Labele można tworzyć samemu, dzięki czemu to "forum" będzie dostosowane bardziej do aktualnego projektu. Jeśli dyskusje się zakończą, bądź zgłoszony tam problem zostanie rozwiązany i zakończony commitem z naprawą, to zwykle temat jest zamykany, ale nadal widoczny w sekcji tematów zamkniętych przez co zawsze można do zawartości tych dyskusji wrócić.
![Issues na githubie](/.github/github_issues.png)
#### Pull requests
Pull requesty pozwalają poinformować innych o zmianach, które zostały wprowadzine do pewnej gałęzi w repozytorium na GitHubie. Po utworzeniu pull requesta, można przedyskutować i przejrzeć potencjalne zmiany ze współpracownikami i dodać kolejne zobowiązania/poprawki, zanim zmiany zostaną zmergowane z gałęzią bazową.
Ciekawe w pull requestach jest to, że każdy może je stworzyć, nawet jeżeli nie pracował nigdy nad kodem. Takim przypadkiem mogłaby być sytuacja, w której ktoś pobrał sobie dane repozytorium i zauważył w nim jakiś błąd - może wtedy go naprawić i utworzyć pull request, aby z jego wersji repozytoruim daną gałąź scalić z gałęzią z repozytorium oryginalnego, przez co bezpośrednio przyczyni się do poprawy kodu zupełnie nieznanej dla siebie osoby/grupy ludzi (o ile zmiany zostaną zaakceptowane przez uprawnionego właściciela repozytorium oryginalnego).
Tak jak w przypadku zakładki ***Issues***, tutaj również znajdują się etykiety, którymi można oznaczać requesty oraz istnieje również możliwość dyskutowania zmian. Istnieje również opcja, w której do zaakceptowania pull requesta jest wymagana odpowiednia ilość zgód, dzięki czemu można się ustrzec przypadkowych decyzji (być może błędnych) jednej osoby z zespołu pracującego nad projektem.
Pull reuqest zawiera również informacje o wszystkich commitach, które w sobie posiada - inaczej mówiąc, pokazuje całą historię zmian, którą ktoś wykonał do momentu, w którym ten ktoś uznał, że chce wprowadzone zmiany wcielić do określonej gałęzi głównego projektu. Poza tym obecne są również informacje o przejściu testów (w przypadku kodu) oraz informacje o różnicach w deponowanych plikach (***diff***, które były wspomniane w sekcji gita).
Pull requesty mogą tworzyć nie tylko ludzie, ale również narzędzia (boty), nadzorujące bezpieczeństwo repozytorium, np. **[dependabot](https://github.com/apps/dependabot)**, który automatycznie sprawdza i naprawia niezgodności z dependencies potrzebnych do poprawnego działania zawartości naszego repozytorium.
Przyjęcie pull requesta oznacza automatyczne zamknięcie danego wątku.
![Pull request na githubie](/.github/github_pullrequest.png)
#### git lfs
##### Przegląd
Github ma pewne ograniczenia w wielkości przesyłanych plików. Github blokuje próby pushowania, których pliki sumarycznie przekraczają 100 MB.
Aby móc śledzić zmiany plików o dużych rozmiarach (oraz żeby móc wykonać tego typu push), należy użyć **git lfs (git large file storage)**. Git LFS obsługuje duże pliki poprzez przechowywanie referencji do pliku w repozytorium, ale nie do samego pliku. Aby obejść architekturę Gita, Git LFS tworzy plik wskaźnika, który działa jako odniesienie do rzeczywistego pliku (który jest przechowywany gdzie indziej). GitHub w istocie zarządza tym plikiem wskaźnika w repozytorium. Kiedy się klonuje repozytorium, to GitHub używa pliku wskaźnika jako mapy, aby znaleźć ten duży plik. Naturalnie, żeby móc poprawnie używać repozytorium z git lfs lokalnie, to należy na swojej maszynie również posiadać git lfs.
Ograniczenia git lfs:
|Product  | Maximum file size |
|--|--|
| GitHub Free | 2 GB |
| GitHub Pro | 2 GB |
| GitHub Team | 4 GB |
| GitHub Enterprise Cloud | 5 GB |
##### Użycie
Najprostszy przypadek użycia git lfs byłby taki, że jeżeli wiemy, że pewien rodzaj plików będzie miał większe rozmiary, to wtedy dane rozszerzenie można skojarzyć z git lfs. Przykładowo:
```bash
cd "sciezka do repozytorium"
git lfs track "*.bsp"
git add "sciezka do ciezkiego pliku bsp"
git commit -m "added biggy"
git push
```
Powyższe polecenia powodują skojarzenie plików `.bsp` z git lfs oraz potencjalne dodanie do repozytorium i do zdalnego repozytorium na githubie pewnego dużego (jak na github) pliku `.bsp`.
##### Wniosek
Generalnie wniosek płynie z tego taki, że Github nie nadaje się do składownaia i śledzenia dużych plików, warto trzymać się tego, aby wersjonować tylko te najbardziej ważne pliki i nieduże pliki z danymi. Składowanie dużych wolumenów danych trzeba realizować w inny sposób.
#### Github pages
Możliwe jest utworzenie repozytorium o specjalnej nazwie - **_username_.github.io**, co sprawi, że github będzie wiedział, że jest to repozytorium z plikami tworzącymi **statyczną** stronę internetową. Taka strona internetowa jest możliwa tylko na jedną organizację lub nazwę użytkownika.
Istnieje jednak również możliwość stworzenia strony internetowej dla pojedynczego repozytorium, a tych ilość jest nieograniczona (z punktu widzenia użytkownika), ponieważ każde repo może posiadać taką stronę.
Strony mogą być tworzone z gotowych szablonów jak i zupełnie od podstaw przez użytkownika. Należy pamiętać o pewnym ograniczeniu - strona musi być statyczna. **Statyczna strona internetowa** to strona internetowa, która jest dostarczana użytkownikowi dokładnie w takiej postaci, w jakiej została zapisana, w przeciwieństwie do dynamicznych stron internetowych, które są generowane przez aplikację internetową. To oznacza, że np. strona internetowa napisana we frameworku **Django** nie może w ten sposób być hostowana na githubie.
Nie mniej, jest to bardzo prosty i szybki sposób, aby móc zaprezentować swój projekt na stronie internetowej oraz prowadzić pewien blog rozwoju projektu z **Jekyll,** który ogbsługują github pages. Również możliwe jest utworzenie własnego URL, które nie musi się składać z *.github.io*.
