Zarządzanie cache
=================

Cache jest bardzo prostą pamięcią (klucz-wartość), która ma na celu bycie jak najbardziej wydajną.
Jest to używane do przechowywania rezultatów bardzo wielu dużych operacji. Może być także użyte to poprawy szybkości działania bazy danych (przechowywując użytkowników, zmienne czy komentarze w pamięci RAM).

Panthera wspiera wiele systemów cachowania, które się różnią nie tylko architekturą ale także wydajnością.

<table>
    <tr>
        <td></td>
        <td><b>Memcached</b></td>
        <td><b>APC</b></td>
        <td><b>XCache</b></td>
        <td><b>Redis</b></td>
        <td><b>Baza danych</b></td>
    </tr>
    
    <tr>
        <td>Architektura</td>
        <td>klient-serwer</td>
        <td>pamięć udostępniona przez PHP</td>
        <td>pamięć udostępniona przez PHP</td>
        <td>klient-serwer</td>
        <td>zewnątrzny relacyjny socket bazy danych</td>
    </tr>
    
    <tr>
        <td>Osiągi</td>
        <td>szybki</td>
        <td>bardzo szybki</td>
        <td>bardzo szybki</td>
        <td>szybki</td>
        <td>powolny</td>
    </tr>
</table>

## Konfiguracja

Są dwa sloty cachowania - `cache` i `varCache`. VarCache powinien być używany do przechowywania lekkich, małych elementów.
W slocie cache są przechowywane elementy, które potrzebują bardzo dobrej wydajności - cache nie może być bazą danych.

<table>
    <tr>
        <td><b>varCache</b></td>
        <td><b>cache</b></td>
    </tr>
    
    <tr>
        <td>Proste dane</td>
        <td>Wielkie połacie danych każdego typu</td>
    </tr>
    
    <tr>
        <td>Może być nawet bazą danych</td>
        <td>Musi być bardzo wydajnym systemem cachowania</td>
    </tr>
</table>

Jeśli posiadasz jakikolwiek dostęp do szybkiego systemu cache (APC albo Memcached) powinieneś go użyć w obydwu slotach.

Przykładowa konfiguracja (config_overlay - może być ustawiony poprzez $panthera -> config -> setKey()):

<table>
    <tr>
        <td><b>key</b></td>
        <td><b>value</b></td>
        <td><b>type</b></td>
        <td><b>section</b></td>
    </tr>
    
    <tr>
        <td>varcache_type</td>
        <td>apc</td>
        <td>string</td>
        <td></td>
    </tr>
    
    <tr>
        <td>cache_type</td>
        <td>apc</td>
        <td>string</td>
        <td></td>
    </tr>
</table>

## Praktyczny przykład użycia

W langtoolu - edytorze tłumaczeń dla Panthery posiadamy automatyczne wyszukiwanie nieprzetłumaczonych tekstów. W tym celu skanujemy wszystkie pliki w katalogach /content i /lib, aby je odnaleźć i sparsować - ta czynność zajmuje ~3 sekundy, więc nie może to być wykonywane przy każdym odświeżeniu strony. Langtool wykonuje wyszukwianie tylko raz, wtedy wszystkie rezultaty są przechowywane w pamięci cache. Przy odświeżeniu strony rezultaty są pobierane z cache (jeśli cache wygaśnie lub jest puste, generowanie nastąpi jeszcze raz).

```php
if ($panthera->cache)
{
    if ($panthera -> cache -> exists('rezultaty-ciezkiej-pracy'))
    {
        $rezultaty = $panthera -> cache -> get('rezultaty-ciezkiej-pracy');
    }
}

if (!isset($rezultaty))
{
     $rezultaty = wykonajCiezkaPrace(); // zajmuje około 10 sekund
     
     if ($panthera->cache)
         $panthera -> cache -> set('rezultaty-ciezkiej-pracy', $results, 86400);
}
```
