Użytkownicy i grupy
=================

Wszystkie funkcje dotyczące zarządzania użytkownikami i grupami znajdują się w /lib/user.class.php. Zarządzaniem użytkownikami zajmuje się klasa `pantheraUser`, która posiada możliwość zmiany meta tagów użytkownika, zmiany hasła czy odczytywania większości danych.

Interfejs jest bardzo prosty, wszystko jest oparte na pantheraFetchDB. Użytkownika możemy znaleźć posiadając tylko login, pełną nazwę albo id.

```php
$u = new pantheraUser('id', 1);
```

```php
$u = new pantheraUser('login', 'myuser');
var_dump($u -> login);
var_dump($u -> full_name);
```

**# NOTE: login i full_name są kolumnami tabeli w bazie danych**

#### Zmiana hasła

Funkcja changePassword automatycznie używa soli i szyfruje podane hasło za pomocą wcześniej wybranego algorytmu podczas etapu konfiguracji Panthery.
Dostępne algorytmy szyfrujące to blowfish, md5 oraz sha512.

```php
$u -> changePassword('test123');

if ($u -> verifyPassword('test123'))
{
    print("Hasła się zgadzają.");
}
```

##### Blokowanie użytkownika

To już nie może być prostsze.

```php
if (!$u -> isBanned())
{
    print("Użytkownik nie jest zablokowany.");
    $u -> isBanned(True); // Ups, już jest. :D
}
```

##### Pobieranie pełnej nazwy użytkownika (lub loginu jeśli nazwa nie jest dostępna)

Zwraca pełną nazwę albo login użytkownika.

```php
var_dump($u -> getName());
```

#### Meta atrybuty

Klasa `metaAttributes` zapewnia meta atrybuty. Używamy jej także w innych objektach - grupach, komentarzach, stronach statycznych itp.
Użycie jest bardzo proste - podobne do zarządzania cache lub konfiguracją.

```php
$u -> acl -> set('admin', True); // nadaj użytkownikowi prawa admina
var_dump($u -> acl -> get('admin'));

// jeśli chcesz być pewien, że dane zostały zapisane
$u -> acl -> save();
```

#### Edycja danych

Jako że `pantheraUser` jest klasą bazowaną na `pantheraFetchDB` to posiada taki sam interfejs pozwalający na modyfikowanie danych użytkownika poprzez ustawianie wartości zmiennych utworzonego objektu.

```php
$u -> full_name = 'Jan Kowalski';
$u -> profile_picture = 'http://example.com/example.jpg';
$u -> save();
```

Powyższy przykład edytuje kolumny `full_name` oraz `profile_picture` w bazie danych dla wybranego użytkownika

##### Pobieranie zdjęcia użytkownika

Funkcja poniżej zwraca zdjęcie użytkownika.

```php
var_dump($u -> getAvatar());
```
