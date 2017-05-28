# Zanim zaczniemy

- Node.js w wersji 4.5+
- NPM
- Yarn (zalecany)
- Lokalny serwer WWW (MAMP, WAMP, LAMP)
    - Windows (XAMPP, WampServer)
- opcjonalnie Git
- możliwość odpalenia PHP z poziomu konsoli / wiersza poleceń
```
php -v
```

Przykładowy wynik polecenia:
```
PHP 5.6.29 (cli) (built: Dec  9 2016 11:14:08)
Copyright (c) 1997-2016 The PHP Group
Zend Engine v2.6.0, Copyright (c) 1998-2016 Zend Technologies
```

W razie problemów upewnij się, że PHP jest w PATH
- https://stackoverflow.com/questions/10753024/how-to-access-the-command-line-for-xampp-on-windows

Alternatywnie, narzędzia powinny mieć swoją konsolę np. XAMPP

![XAMPP Control Panel](img/xampp-running.png)

# Instalacja

### Instalacja Yarna
```
npm install -g yarn
```

Dlaczego warto? Chisel domyślnie korzysta z Yarna ponieważ szybciej instaluje nam wszyskie paczki. Jak nie mamy Yarna to korzysta z npm, który jest nieco wolniejszy.

## Sprawdzamy czy mamy wszystko co jest na potrzebne:
```
node -v
npm -v
php -v
yarn - v
```

## Yeomen + Chisel generator
```
npm install -g yo generator-chisel
```

## Tworzymy pierwszy projekt

### Stwórz folder i wejdź do niego
```
mkdir nazwa_projetku 
cd nazwa_projektu
```

### Uruchom generator Chisel
```
yo chisel
```

#### Opcje instalatora
1. Nazwa projektu 
2. Autor projektu
3. Typ projektu 
    - wybieramy WordPress with Front-end
4. Możemy wybrać dodatkowo czy chcemy jQuery oraz wsparcie dla ES6 + Babel
    - spacją zaznaczamy pojedyńcze elemeny
    - srzałkami góra / dół przechodzimy między opcjami
    - wciśnięcie "a" spowoduje zaznaczenie wszystkich opcji (ang. all)
    - wciśnięcie "i" spowoduje odwrócenie zaznaczenia (ang. inverse)
5. Tytuł strony
6. Adres URL używany lokalnie
7. Nazwa użytkownika dla administratora
8. Hasło administratora
9. Adres email administratora
10. Gdzie mamy przechowywać pliki wersji deweloperskiej
    - bezpośrednio w "roocie" projektu
    - w folderze motywu
11. Ustawienia bazy danych
    - host
    - port
    - nazwa bazy
    - nazwa użytkownika
    - hasło
12. Instalacja popularnych wtyczek
    - ACF Pro
    - Gravity Forms
    - WP Sync DB
    - WP Sync DB Media File Addon
13. Potwierdzamy i czekamy aż wszystko się zainstaluje.
14. W międzyczasie ustawimy sobie Virtual Hosta

### Virtual host

#### Plik hosts

W pliku _hosts_ dodajemy naszego hosta. W zależności od systemu operacyjnego i konifiguracji plik ten może znajdować się różnych miejsach.
###### Windows
```
c:\Windows\System32\Drivers\etc\hosts
```
**Ważne:** Edytuj ten plik jako administrator. Najlepiej odpalić sobie notatnik poprzez prawy przycisk myszki i wybrać opcję _Uruchom jako administrator_. 
###### macOS (i inne Unixy w większości)
```
/etc/hosts
```

##### Edycja pliki i sprawdzanie

```
127.0.0.1 nazwa_projektu.dev
```
Po zapisaniu możemy sprawdzić czy działa poprawnie poprzez wydanie polecenia:
```
ping nazwa_projektu.dev
PING nazwa_projektu.dev (127.0.0.1): 56 data bytes
64 bytes from 127.0.0.1: icmp_seq=0 ttl=64 time=0.065 ms
64 bytes from 127.0.0.1: icmp_seq=1 ttl=64 time=0.071 ms
64 bytes from 127.0.0.1: icmp_seq=2 ttl=64 time=0.119 ms
```

#### Virtual host
W pliku _httpd.conf_ lub _http-vhost.conf_ należy ustawić odpowiednią scieżkę do naszego pliku _dev-vhost.conf_, który został utworzony na podstawie danych podanych podczas procesu instalacji.

```
IncludeOptional /sciezka/do/projektow/*/dev-vhost.conf
```
Minusem takiego rozwiązania jest to, że teraz każdy projekt musi mieć plik dev-vhost.conf, inaczej Apache będzie zwracał błąd. W takim przypadku można zlinkować pojedyńczy plik:
```
IncludeOptional /sciezka/do/projektow/nazwa_projektu/dev-vhost.conf
```

**Ważne**: Po wszystkim zrestuj Apache
```
#macOS
sudo apachectl restrart
#Ubuntu
sudo service apache2 restart
```

### Projekt jest gotowy do pracy

Jeśli wszystko poszło prawidłowo to po otwarciu przeglądąrki i wpiasaniu w pasku adresu http://nazwa_projektu.dev powinnyśmy zobaczyć stronę startową naszego projektu.

![Chisel - Strona domyślna](img/chisel-default-theme.png)

# Omówienie struktury
- **gulp** - zadania Gulpa
- **node_modules** - moduły Node.js do realizacji zadań Gulpa 
- **src** - pliki źródłowe
    - **assets** - czcionki, pliki graficzne, itp
    - **scripts** - pliki JavaScript
    - **app.js** - główny plik JavaScript
    - **greeting.js** - przykładowy moduł JS
    - **styles** - pliki Sass w architekturze ITCSS
        - **components** - komponenty, większość kodu tworzymy tutaj
        - **elements** - generyczne elementy HTML (h1, a, itd), bez klas czy ID
        - **generic** - reset / normalize, box-sizing itp
        - **objects** - selektory oparte na klasach, nie zawierają styli dekorujących 
        - **settings** - ustawienia 
        - **tools** - narzędzia czyli wszyskie funkcje, mixiny itp
        - **utilities** - narzędzia pomocniczne, ładowane na końcu i nadpisujące wszystkie style
        - **vendor** - dodatkowe biblioteki, itp
        - **main.scss** - główny plik Sass, importujące wszystkie moduły naszej architektury 
- **wp** - folder z WordPressem
    - **wp-content/themes/nazwa_projektu/** - automatycznie utworzony motyw
        - **dist** - miejsce docelowe na pliki CSS, JS, obrazki itp
        - **templates** - szablony Twiga
        - **index.php** - pliki wymagane przez WP (Chisel Starter theme file) 
        - **functions.php**
        - ...
    - **wp-config-local.php** - lokalna konfiguracja WP
- **.editorconfig** - zapewania jendolite formatowanie w plikach projektu
- **.eslintignore** - pomijaj wskazane pliki / folderu przy walidacji eslint
- **.eslintrc.yml** - konfiguracja eslint
- **.gitattributes** - konfiguracja wymuszająca Unixowe zakończenie pliku w plikach tekstowych
- **.gitignore** - pomijaj wskazane pliki / foldery przy używaniu Gita
- **.htmlhintrc** - konfiguracja htmllint
- **.stylelintignore** - pomijaj wskazane pliki / foldery przy walidacji stylelint
- **.stylelintrc.yml** - konfigruacja stylelint
- **.yo-rc.json** - konfiguracja Yeomana
- **dev-host.conf** - konfiguracja Virtual Host
- **gulpfile.js** - importuje zadania Gulpa z folderu gulp/ i uruchamia odpowiednie zadania w zależności od wydanego polecenia
- **package.json** - konfiguracja zależności NPM

# Trochę na temat pliku wp-config-local.php

Plik _wp-config.php_ jest zmodyfikowany na potrzeby Chisela. W przypadku lokalnej pracy konfiguracja bazy danych i inne zmienne definujemy w pliku _wp-confg-local.php_. Plik ten domyślnie jest dodawany do .gitignore, więc nie ma obaw, że zostanie wysłany do repozytorium.

##### Źródła i przydatne linki
- https://developer.microsoft.com/en-us/microsoft-edge/tools/vms/
- http://devtuts.butlerccwebdev.net/testserver/xampp-cpanel-running.png
