# Praca z repozytorium

## Wprowadzenie
Na laboratoriach zaprezentowane zostaną krótkie zadania wprowadzające do tematyki pracy z repozytorium.

## Część 1

Głównymi pojęciami oraz technikami, które zostaną omówione podczas zajęć są:

1. Rebase
2. Force Push
3. Feature branche
4. Issue
5. Pull Request
6. Code Review

### Rebase
Rebasing jest procesem przenoszenia lub łączenia sekwencji commitów w nowy commit bazowy. Rebasing jest najbardziej użyteczny i łatwy do zwizualizowania w kontekście pracy z feature branchami. Ogólny proces może być przedstawiony następująco:

![rebase](https://wac-cdn.atlassian.com/dam/jcr:4e576671-1b7f-43db-afb5-cf8db8df8e4a/01%20What%20is%20git%20rebase.svg?cdnVersion=668)

Rebasing to zmiana podstawy brancha z jednego commitu na inny, tak aby wyglądało to na stworzenie swojego brancha z innego commitu. Sam Git osiąga to poprzez tworzenie nowych commitów i zaaplikowanie ich do określonej bazy. Bardzo ważne jest, aby zrozumieć, że nawet jeśli branch wygląda tak samo, to składa się z zupełnie nowych commitów.

Podstawowym powodem rebasingu jest utrzymanie liniowej historii projektu. Na przykład rozważ sytuację, w której główny branch rozwinął się od czasu, gdy zacząłeś pracować nad feature branchem. Ogólnie, chciałbyś uzyskać najnowsze aktualizacje głównego brancha w swoim feature branchu, ale chcesz zachować czystą historię, aby wyglądało na to, że pracowałeś z najnowszego głównego brancha. Daje to późniejszą korzyść w postaci czystego merge twojego feature brancha z powrotem do main brancha. Dlaczego chcemy utrzymywać "czystą historię"? Korzyści z posiadania czystej historii stają się namacalne w momencie badania histori Twojego repozytorium podczas przeprowadzanej regresji. ~ by Atlassian
bashCopy# Wykonanie rebase brancha feature na main
```git
git checkout feature
git rebase main

# Interaktywny rebase - pozwala na modyfikację historii commitów
git rebase -i HEAD~3  # Ostatnie 3 commity
```

**Lepszy przykład:**
```
# Początkowa sytuacja:
# Mamy gałąź main i feature-branch

# 1. Tworzymy nową gałąź z main
git checkout main
git checkout -b feature-branch

# 2. Wykonujemy kilka commitów na feature-branch
echo "new feature" > feature.txt
git add feature.txt
git commit -m "Add new feature"

echo "bug fix" > bugfix.txt
git add bugfix.txt
git commit -m "Fix critical bug"

echo "documentation" > docs.txt
git add docs.txt
git commit -m "Add documentation"

# 3. W międzyczasie ktoś dodał nowe zmiany do main
git checkout main
echo "main update" > main-update.txt
git add main-update.txt
git commit -m "Update main branch"

# 4. Teraz chcemy przebazować nasze zmiany z feature-branch na aktualny main
git checkout feature-branch
git rebase main

# 5. Jeśli chcemy połączyć (squash) nasze 3 commity w jeden
git rebase -i HEAD~3

# W edytorze pojawi się coś takiego:
# pick abc1234 Add new feature
# pick def5678 Fix critical bug
# pick ghi9012 Add documentation
#
# Zmieniamy na:
# pick abc1234 Add new feature
# squash def5678 Fix critical bug
# squash ghi9012 Add documentation

# 6. Po zakończeniu rebase możemy wypchnąć zmiany (force push wymagany!)
git push origin feature-branch --force-with-lease
```
### Force push

Użycie `force push` powoduje że historia commitów na zdalnym serwerze zostanie siłą nadpisana twoją własną, lokalną historią. Jest to dość niebezpieczny proces, ponieważ bardzo łatwo jest nadpisać (a tym samym stracić) commity od swoich kolegów.

```
# Force push (należy używać ostrożnie!)
git push --force

# Bezpieczniejsza wersja force push
git push --force-with-lease  # Sprawdza czy nie nadpiszemy cudzych zmian
```
### Feature Branche

Podstawową ideą feature branchy jest to, że cały rozwój funkcjonalności powinien odbywać się na dedykowanym branchu zamiast na mainie. Taka enkapsulacja ułatwia wielu programistom pracę nad daną funkcjonalnością bez naruszania głównej bazy kodu. Oznacza to również, że główna gałąź nigdy nie będzie zawierać uszkodzonego kodu, co jest ogromną zaletą dla środowisk ciągłej integracji.
```
# Tworzenie nowego feature brancha
git checkout -b feature/nazwa-funkcjonalnosci

# Przełączanie się między branchami
git checkout nazwa-brancha

# Pobranie zmian z remote
git fetch origin

# Aktualizacja brancha lokalnego
git pull origin nazwa-brancha
```
### Issues
Świetnym sposobem na śledzenie zadań, nad którymi trzeba popracować w repozytorium, jest użycie GitHub Issues. Zawierają one tytuł, opis i osobę przypisaną do danego issue.  Pozwalają one również na zaprojektowanie milestone oraz etykiet, dzięki którym jesteśmy w stanie łatwo organizować całym repozytorium. ~ by [codecademy](ttps://static-assets.codecademy.com/Courses/learn-git-github/project-management/)

![issues](https://static-assets.codecademy.com/Courses/learn-git-github/project-management/issues-board-docs.png
)

### Pull Request

Pull request jest formą poinformowania osób zaangażowanych w projekt o nowych przygotowanych przez Ciebie zmianach oraz prośbą o zaakceptowanie tych zmian.
Podczas tworzenia pull request’a generuje się widok w wybranym narzędziu (np. GitHub, czy Bitbucket), który pokazuje zmiany w kodzie, które chcemy wprowadzić. Zmiany można omówić przez m.in. dodanie komentarzy i ostatecznie zaakceptować, wstrzymać lub całkowicie odrzucić. ~ by [stormit](https://stormit.pl/pull-request/)

![pr](https://docs.github.com/assets/cb-87213/images/help/pull_requests/pull-request-review-edit-branch.png)

### Code Review

Code Review, jest aktem świadomego i systematycznego publikowania kodu do oceny dla innych programistów, w celu sprawdzenia kodu pod kątem błędów.  Praktyka przyspiesza i usprawnia proces tworzenia oprogramowania.

## Zadania

1. Dobierzcie się w zespoły
2. Jako zespół stworzcie wspolnego brancha, który będzie pełnił rolę waszego maina. Nazwa brancha powinna być nazwą waszego zespołu
3. Stwórz nowe issue na githubie a w nim:
   1. Ustaw jego tytuł w formacie: `<nr z dziennika> new feature enhacement`
   2. Dodaj komentarz opisujący: "add my student index number to the hello.md on the line 3"
   3. Przydziel siebie w Assignies
   4. Przydziel label enhacement, jeśli go nie ma - stwórz nowy
   5. Zapisz issue klikając `Submit new issue`
4. Sklonuj repozytorium na swoją lokalną maszynę
5. Odbranchuj się od brancha swojego zespołu. Twój nowy branch powinien byc nazwany zgodnie z danym schematem: `feature/<nazwa twojego zespolu>/<numer twojego issue>/<krotki opis twoich zmian>`
6. Wykonaj zadanie ze swojego issue
7. Wykonane zadanie powinno zostac zacommitowane i wypushowane na remote. Wiadomości commitów powinny być skontruowane zgodnie z podanym schematem: `<Numer issue>: krótki opis wprowadzonych zmian w commitcie`
8. Wykonaj rebase swojego brancha z branchem swojego zespołu. W przypadku konfliktów, dodaj swój numer indeksu po przecinku. Zrób commita i force pusha.
9. Utwórz Pull Requesta
   1. Ustaw jego tytuł
   2. Dodaj komentarz opisujący co Twoje zmiany wprowadzają
   3. Dodaj do Reviewers członków swojego zespołu
   4. Przydziel siebie na Assignies
   5. Zalinkuj swoje issue. Ściągawka [tutaj](https://docs.github.com/en/issues/tracking-your-work-with-issues/linking-a-pull-request-to-an-issue)
   6. Zapisz
10. Wykonaj Code Review innym członkom zespołu
11. W momencie otrzymania approve, zmerguj swojego Pull Requesta
12. Zamknij stworzone przez siebie issue

## Część 2:
### Fork
Fork to nowe repozytorium, które dzieli kod i ustawienia widoczności z oryginalnym repozytorium. Operacja kopiuje główne repozytorium na nasze główne konto. Forka można dokonać wchodząć na główną stronę repozytorium, które chcemy skopiować, a nastepnię klikając w zielony przycisk po prawej stronie z napisem `Fork`.
### Markdown
Markdown to lekki język znaczników ze składnią formatowania zwykłego tekstu. Pliki w języku MarkDown są standardem w branży do tworzenia tzw. plików Readme, które służą do opisania oraz stworzenia instrukcji do naszego repozytorium, bądź projektu. \
Podstawy języka markdown dostępne są pod danym linkiem: [Markdown Syntax](https://www.markdownguide.org/basic-syntax/).\
Podczas zajęć potrzebne będzie użycie takich znaczników jak
   - Headings `#`
   - Lista `-`
   - Link do obrazka `![Tux, the Linux mascot](http://obrazek)`
   - formatowanie jako "kod" \` <kod> \` 
### Squash
Squash jest sposobem na przepisanie historii commitów; ta akcja pomaga oczyścić i uprościć historię commitów przed podzieleniem się swoją pracą z członkami zespołu. Squash commitów oznacza, że bierzesz zmiany z jednego commitu i dodajesz je do commitu macierzystego.
   
Aby dokonac squasha w terminalu, należy użyć interkatywnej funkcji rebase za pomocą komendy `git rebase -i`. Komenda powoduje otwarcie listy z historią commitów w edytorze VIM. Aby rozpocząć edycje tekstu za kursorem należy naciśnąć przycisk `i` na klawiaturze. Wówczas należy wybrać z listy commitów te które, powinny zostać zesquashowane. 
![squash1](https://github.com/user-attachments/assets/91726ac4-fdf4-469d-921b-f84ad95e6839)
Aby tego dokanać, należy przy odpowiednich commitach zamienić słowo `pick` na `squash` bądź `s`.
![squash2](https://github.com/user-attachments/assets/8dc66878-ff26-44e0-ae13-38482c6fc4dc)
By wyjść z trybu edycji należy wciśnąć przycisk `esc` na klawiaturze. Aby zapisać zmiany należy dokonać poniższej komnbinacji znaków na klawiaturze: `:x`.
![squash3](https://github.com/user-attachments/assets/f22e4f53-55a6-4d8a-bcec-ff4c397af203)
Następnym oknem po zapisie, jest okno wyboru wiadomości commitów.
![squash4](https://github.com/user-attachments/assets/7ab9e879-b826-480c-bf91-71051a5120c9)
Za pomocą wcześniej podanych kombinacji klawiszy, należy usunąć wiadomości dla ze squashowanych commitów oraz zedytować pierwszy commit message. 
![squash5](https://github.com/user-attachments/assets/f7778c93-5f63-493d-b4c0-ad3790a01cc0)
### Cherry-pick
Cherry-pick to zaawansowane polecenie, które umożliwia wybranie dowolnego commita Git przez referencję i dołączenie go do bieżącego roboczego wskaźnika HEAD. Operacja „cherry pick” polega na wybraniu commita z gałęzi i dołączeniu go do innej gałęzi. Polecenie git cherry-pick może być przydatnym narzędziem do cofania zmian. Załóżmy na przykład, że commit zostanie przypadkowo wprowadzony w niewłaściwej gałęzi. Możesz przełączyć się do właściwej gałęzi i za pomocą operacji „cherry pick” wstawić commit tam, gdzie powinien się znaleźć. - By Atlassian.

Operacje prosto można również wykonać za pomocą graficznych interejsów użytkownika dla Gita takich jak SourceTree, czy wbudowane narzędzia w IDE. W najpopularniejszych rozwiązaniach często wystarczy w historii commitów wybrać interesujący nas commit za pomocą prawego przyciska myszy oraz kliknąć polecenie `cherry-pick`.

![cherrypick1](https://github.com/user-attachments/assets/b05a5d3e-0f65-4a74-885a-ceffa3977d97)

      
## Zadania   
1. Wykonaj forka bieżącego repozytorium do swojego konta na Githubie
2. Z clonuj repozytorium na lokalną maszynę
3. Stwórz brancha `feature/<twoj_numer_dziennika>/markdown`
4. Stwórz plik `AboutMe.md` i uzupełnij go. Przejrzyj najpierw skladnie pliku `README.md`. Plik `AboutMe.md` powinien zawierać takie dane jak (każdy z podpunktów powinien zostać wykonany jako osobny commit):
   - Krótki opis przedstawiający kim jesteś
   - Listę Twoich hobby
   - Obraz z kotem
   - Link do twojego Githuba
5. Dokument powinien być odpowiednio sformatowany
   - Każdy z punktów powinien mieć swój własny heading
   - Lista z hobby powinna zostać stworzona jako unordered list
   - Obrazek powinien być po prostu linkiem do obrazku z internetu
   - Link do GitHuba powinien zostać sformatowany jako "kod"
7. Za pomocą komendy `git rebase -i` w konsoli dokonaj squasha trzech commitów. Zesquashowany commit powinien mieć następującą nazwę `squashed commits <numder dziennika> - added AboutMe.md file`
9. Wypushuj zmiany na remote
10. Stwórz PR i poproś prowadzącego. Po pomyślnej weryfikacji zmerguj PR
11. Z głównego brancha stwórz nowego brancha o nazwie `feature/<twoj_numer_dziennika>/gui`
12. Z głównego brancha stwórz nowego brancha o nazwie `feature/<twoj_numer_dziennika>/cherrypick`
13. Wróć do głównego brancha i wpliku AboutMe.md dodaj jeden znak specjalny do pierwszej linijki i wypushuj zmiany
14. Wróć do brancha `feature/<twoj_numer_dziennika>/gui`
15. Otwórz repozytorium w jednym z narzędzi, który umożliwia steowanie gitem za pomocą graficznego interfejsu użytkownika (Jetbrains IDEs / Sourcetree / Fork / VS Code itd..)
16. Zrób zmiany w pliku `AboutMe.md`, dodaj zdanie "lorem ipsum" w pierwszej linijce
17. Zacommituj i wypushuj zmiany za pomocą graficznego interfejsu użytkownika
18. Za pomocą GUI zrób rebase, rozwiąż konflikty oraz wypushuj rozwiązane konflikty na remote
19. Stwórz PullRequesta i poproś prowadzącego. Po pomyślnej weryfikacji zmerguj PR.
20. Wróc do brancha `feature/<twoj_numer_dziennika>/cherrypick`
21. Zrób cherry-picka ostatniego commita z głównego brancha
22. Wypushuj zmiany, stwórz PR i poproś prowadzącego

## Przykłady:
```
# 1. Feature Branch
# Tworzenie nowej funkcjonalności - system logowania
git checkout -b feature/auth/login
echo "function login() {}" > auth.js
git add auth.js
git commit -m "Add login function"

# 2. Rebase
# Aktualizacja brancha feature nowymi zmianami z main
git checkout main
git pull
git checkout feature/auth/login
git rebase main

# 3. Force Push
# Po rebase musimy użyć force push
git push --force-with-lease origin feature/auth/login

# 4. Squash (poprzez rebase interaktywny)
# Łączenie wielu commitów w jeden
git rebase -i HEAD~3
# W edytorze pojawi się:
# pick abc123 Add login function
# pick def456 Add password validation
# pick ghi789 Add error handling
#
# Zmieniamy na:
# pick abc123 Add login function
# squash def456 Add password validation
# squash ghi789 Add error handling

# 5. Cherry-pick
# Przenoszenie konkretnego commita między branchami
git checkout main
git log # kopiujemy hash interesującego nas commita
git checkout feature/bugfix
git cherry-pick abc123f # wklejamy skopiowany hash

# 6. Fork (przez API/CLI)
# Tworzenie forka (zazwyczaj przez interfejs GitHub)
gh repo fork owner/repository

# 7. Praca z remote
# Pobieranie zmian
git fetch origin
git pull origin main

# 8. Rozwiązywanie konfliktów
# Po wystąpieniu konfliktu podczas rebase
git status # sprawdzamy pliki w konflikcie
# Edytujemy pliki
git add .
git rebase --continue

# 9. Pull Request (przez API/CLI)
# Tworzenie PR (zazwyczaj przez interfejs GitHub)
gh pr create --title "Add login system" --body "Implementation of user authentication"

# 10. Issue (przez API/CLI)
# Tworzenie nowego issue (zazwyczaj przez interfejs GitHub)
gh issue create --title "Bug in login system" --body "Users cannot reset password"

# 11. Code Review (przez API/CLI)
# Dodawanie komentarza do PR (zazwyczaj przez interfejs GitHub)
gh pr review 123 --comment "Please add error handling"
```
