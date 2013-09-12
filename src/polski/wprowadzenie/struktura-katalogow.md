Struktura katalogów
========================

Trzema najważniejszymi katalogami w Panthera Framework są **/lib**, jego ścieżkę wewnątrz kodu przechowuje stała PANTHERA_DIR, katalog główny aplikacji określany wewnątrz jako SITE_DIR oraz content czyli SITE_DIR/content.

Istnieje podstawowa zasada - nigdy nie modyfikujemy bibliotek Panthery, pracujemy tylko i wyłącznie na plikach aplikacji. Panthera jest frameworkiem który może być rozszerzany, lecz nie służy do tego aby modyfikować jego kod źródłowy na potrzeby pojedynczej aplikacji.

**Wskazówka:** Z jednego katalogu /lib może korzystać kilka aplikacji WWW.

 

## Katalog główny aplikacji

W katalogu głównym aplikacji znajdują się tzw. front controllery czyli pliki z kodem PHP, które wywoływane są bezpośrednio z przeglądarki. Jest to m.in. index.php, _ajax.php czy install.php

Aby otrzymać kompletną ścieżkę do katalogu głównego aplikacji należy odczytać wartość stałej SITE_DIR.

```
php> var_dump(SITE_DIR);
string(42) "/home/myuser/public_html"
```

## /lib i /content

Pierwszy z tych katalogów - **/lib** przechowuje pliki Panthery, których wedle zasady nie ruszamy, są to po prostu biblioteki. Znajdują się tutaj także domyślne szablony Panelu Administratora, które oczywiście można nadpisać tworząc pliki o takich samych nazwach w katalogu **/content**.


* ajaxpages - strony wyświetlane przez frontcontroller _ajax.php
* backups - miejsce na kopie zapasowe głównie baz danych
* database - znajdują się tutaj pliki baz danych SQLite3 oraz szablony (szkielety) baz danych gotowe do zaimportowania
* installer - pliki instalatora, przechowuje tutaj bazę danych oraz pliki tymczasowe
* locales - tłumaczenia językowe
* packages - pliki pakietów instalowanych przez menadżera pakietów Leopard, zawiera głównie kopie zapasowe nadpisanych plików przez pakiet
* pages - strony wyświetlane przez frontcontroller główny index.php
* plugins - wtyczki rozszerzające funkcjonalność aplikacji
* templates - szablony oraz ich konfiguracja
* modules - moduły dynamicznie ładowane
* tmp - pliki tymczasowe
* uploads - wszystkie pliki wrzucane do aplikacji przez formularze uploadu plików