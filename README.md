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
