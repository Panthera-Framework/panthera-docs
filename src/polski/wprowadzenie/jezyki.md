Języki
==========

Tłumaczenia są przechowywane w zserializowanych tablicach PHP podzielonych na `domeny` jak w GNU Gettext.
*Domena jest grupą tłumaczeń*, na przykład zarządzanie użytkownikami mogłoby mieć własną nazwę "users". To zapobieganie systemu tłumaczeń przed ładowaniem niepotrzebnych danych dla obecnie ładowanej strony.

Plik tłumaczeń nie jest odczytywalny dla użytkownika, ale jest elegancka klasa stworzona do edycji tych plików. Klasa jest włączona w moduł `liblangtool`.

## Tworzenie nowego języka

Języki są folderami w `/content/locales` oraz `/lib/locales`, np. _/content/locales/polski_. Pamiętaj aby *nie tworzyć* żadnych plików w katalogu /lib!

```php
localesManagement::create('polish'); // stwórz język polski
```

Jest także możliwość stworzenia języka z wbudowanego w stronę Panelu Admina.

## Tworzenie nowej domeny języka

Aby stworzyć nową domenę możesz użyć jednej linii kodu i wykonać ją w CGI albo konsoli.

```php
localesManagement::createDomain('polish', 'welcome'); // witam - to jest nazwa nowej domeny
```

## Tłumaczenie tekstu

Nieprzetłumaczonymi wyrażeniami jest tekst umieszczony w kodzie PHP lub szablonie w natywnym języku (w zdecydowanej większości przypadków jest to angielski)

Przykład:

```html
<div class="body">{function="localize('Welcome to my first website based on Panthera Framework', 'welcome')"}!</div>
```

W przykładzie powyżej mamy jeden nieprzetłumaczony tekst - "Welcome to my first website based on Panthera Framework", chcemy go przetłumaczyć na język polski.

```php
$domain = new localeDomain('polish', 'welcome');
$domain -> setString('Welcome to my first website based on Panthera Framework', 'Witam na mojej pierwszej stronie internetowej opartej o Panthera Framework');
$domain -> save();

// zróbmy test
$panthera -> locale -> setLocale('polish');
var_dump(localize('Welcome to my first website based on Panthera Framework', 'welcome'));
```

Możesz także użyć langtoola z Panelu Admina aby przetłumaczyć tekst.

## Tekst zawierający dynamiczne dane np._You have %s messages_

```html
{$user="John"}
<div class="body">{function="slocalize('Hello %s and welcome to my application based on Panthera Framework!', 'welcome', $user)"}</div>
```

Jedyna różnica jest w nazwie funkcji - *slocalize*, a nie localize. Oczywiście później musi być także podana domena i argument , który będzie jest tekstem lub liczbą.
