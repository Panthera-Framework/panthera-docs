Instalacja Panthery
=====================

Projekt hostowany jest w serwisie github.com, używamy systemu kontroli wersji GIT dzięki czemu cały kod można pobrać z poziomu powłoki shell jednym poleceniem, a drugim pobrać zależności przy użyciu menadżera pakietów PHP - composer.

1. Pobieranie plików źródłowych z GIT razem z zależnościami
  
  ```
  git clone https://github.com/webnull/panthera -b master
  cd panthera
  ./install.sh
  ```
  Po wykonaniu powyższych poleceń powinny zostać pobrane wszystkie pliki oraz zainstalowane zależności w postaci m.in. parsera szablonów RainTPL czy biblioteki wysyłającej pocztę e-mail - phpMailer.

  ```
  mv example-app nazwij-mnie
  ```
  
  W katalogu example-app znajduje się przykładowa aplikacja dostępna do zainstalowania, jest to zdecydowanie najlepszy moment aby zmienić jej nazwę, ponieważ w dalszym etapie rozwoju aplikacji może być to nieco trudniejsze.

2. Instalacja poprzez wbudowany instalator
  Teraz przyszedł czas na uruchomienie instalatora w przeglądarce. Przechodzimy na stronę /nazwij-mnie/install.php, jeśli instalator zwrócił komunikat o braku możliwości zapisu konfiguracji musimy nadać poprawne uprawnienia całemu katalogowi z zawartością strony WWW. Konieczne jest to aby każdy moduł Panthery miał prawa do zapisu w celach aktualizacji treści strony czy instalacji nowych modułów.

3. Powtarzanie procesu instalacji

  Z różnych powodów możemy czasami potrzebować powtórzyć proces instalacji - na przykład może on uprościć przenoszenie aplikacji na inny serwer.
  W tym celu kasujemy wszystkie pliki z katalogu installer, który znajduje się w głownym katalogu aplikacji oraz tworzymy zmienną konfiguracyjną "requires_instalation" z wartością True oraz "preconfigured" z wartością False.

  Możemy też całkowicie usunąć plik app.php, spowoduje to całkowite usunięcie jakiejkolwiek konfiguracji aplikacji.
  Następnie możemy standardowo uruchomić instalator z poziomu przeglądarki WWW.