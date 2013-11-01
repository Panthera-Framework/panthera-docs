Szablony
==========

Każda strona internetowa wygląda w jakiś sposób, niektóre posiadaja nawet możliwość wyboru motywu zmieniającego kolory czy nawet cały układ wyświetlania treści.
To wszystko jest możliwe dzięki separacji części logicznej oraz bazy danych od widoku. Widok w takim przypadku można wstawić dowolny.

Wszystkie szablony Panthery są przechowywane w `/lib/templates` oraz `/content/templates`. Funkcje używane do zarządzania szablonami można znaleźć w pliku template.class.php lub module libtemplate.

## Struktura szablonów

<table>
    <tr><td>configs</td><td>Zawiera pliki konfiguracyjne w formacie JSON - każdy szablon musi posiadać swój własny (np. main.tpl.json)</td></tr>
    <tr><td>templates</td><td>Tutaj są przechowywane wszystkie pliki szablonów, które są gotowe do kompilacji</td></tr>
    <tr><td>webroot</td><td>Ten katalog zawiera wszystkie pliki, które są kopiowane do głównego katalogu aplikacji np. style CSS, skrypty javascript itp.</td></tr>
    <tr><td>config.json</td><td>Meta plik, opisuje główne pliki szablonów (desktop, mobile i tablet)</td></tr>
    <tr><td>thumbnail.png</td><td>Miniaturka do wyświetlenia w Panelu Admina</td></tr>
</table>

## Webroot

Katalog `webroot` szablonu zawiera wszystkie pliki, które mogę być skopiowane do głównego katalogu aplikacji (gdzie położone są publiczne pliki jak index.php).
Dobrze jest utrzymywać w tym katalogu porządek, style CSS powinny być w katalogu `css`, javascript i obrazki także powinny być rozdzielone.

## Praktyczne użycie

Podstawowym użyciem systemu szablonów jest przypisywanie wartości zmiennym i... ich wyświetlanie. Oczywiście w Pantherze możemy także dynamicznie dołączać skrypty, style css oraz inne meta tagi w nagłówku strony.

```php
$panthera -> template -> setTitle('Wow, ta strona ma tytuł, ale nie użyłem jeszcze żadnego taga tytułowego...');
$panthera -> template -> push('tekst', 'cześć :-)');
$panthera -> template -> push('owoce', array(
    1 => 'banan',
    2 => 'jabłko',
    3 => 'truskawka'
);
$panthera -> template -> display('tekst_demo.tpl');
```

tekst_demo.tpl

```html
<!DOCTYPE html>
<html>
    <head>
    {$site_header}
    </head>
    
    <body>
    {$tekst|strtolower}<br>
    "Yes" w Twoim języku: {function="localize('Yes')"}<br>
    
    <ul>
    {loop="$owoce"}
    <li>#{$key} - {$value}</li>
    {/loop}
    </ul>
    
    </body>
</html>
```

Jeśli chcesz dowiedzieć się więcej o składnii systemu szablonów, odwiedź oficjalną stronę [RainTPL](http://www.raintpl.com/Documentation/).

### Środowisko

Najbardziej przydatną zmienną środowiskową jest - `{$PANTHERA_URL}`. Przedstawia ona adres URL prowadzący do Twojego głównego katalogu strony.
