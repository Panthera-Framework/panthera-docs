Front controllery
=================

Obsługują wszelkie rządania pochodzące od klienta bezpośrednio, uruchamiając całą aplikację opartą na Pantherze. Znajdują się w głównym katalogu aplikacji. Normalnie front controllery powinny być zlinkowane z katalogu **/lib/frontpages**, niepoprawnie jest je kopiować ponieważ po aktualizacji Panthery nie zostaną one w takim przypadku zaaktualizowane.

 

## Domyślne front controllery i ich działanie

Panthera zawiera wbudowane i gotowe do użytku przykładowe front controllery, które nawet mogą okazać się wystarczające dla Twojej aplikacji. Każdy z nich pełni inną rolę dlatego jest ich wiele.

* index.php - *wyświetla strony z katalogu pages*
* pa-login.php - *loguje użytkownika (używany głównie do logowania do Panelu Administratora, lecz może również służyć do logowania zwykłych użytkowników do aplikacji)*
* _ajax.php - *wyświetla strony z katalogu ajaxpages*
* pa-admin.php - *wyświetla główną stronę Panelu Administratora*
* _crontab.php - *uruchamia zaplanowane zadania takie jak masowa wysyłka poczty*
* install.php - *służy do instalacji aplikacji, może być zastosowany także do przenoszenia z serwera na serwer lub jako narzędzie resetu/naprawy konfiguracji*
* _phpsh.php - *integruje narzędzie phpsh z Pantherą tworząc interaktywną konsolę Panthery z poziomu shella*