# Zadania Kryptograficzne (OpenSSL, GPG, Hashcat)

---

## Zadanie 1. Skrót SHA-384 i Kodowanie Base64

Wygenerowanie skrótu SHA-384 dla słowa "CWEs" i zakodowanie innego skrótu w Base64.

1.  Wyliczenie skrótu SHA-384 dla tekstu "CWEs":
    ```bash
    echo -n "CWEs" | openssl dgst -sha384
    ```

2.  Kodowanie skrótu (prawdopodobnie dla weryfikacji) do formatu Base64:
    ```bash
    echo -n "09422c4a15b668b699f0ad159f2231ff45f7d96ef8d233181a0bfdba6229bc2fde355dbd65ce456b75feb53538a5f37f" | base64 > zad1.b64
    ```

3.  Wyświetlenie zawartości pliku Base64:
    ```bash
    cat zad1.b64
    ```

4.  Wysłanie wyniku:
    ```bash
    curl -X POST -F "id=124124" -F zad1=@zad1.b64 http
    ```

---

## Zadanie 2. Generowanie Kluczy Kryptografii Eliptycznej (ECC)

Generowanie pary kluczy (prywatnego i publicznego) z wykorzystaniem krzywej **brainpoolP256r1**.

1.  Generowanie klucza prywatnego:
    ```bash
    openssl ecparam -name brainpoolP256r1 -genkey -noout -out priv.pem
    ```

2.  Wyodrębnienie klucza publicznego z klucza prywatnego:
    ```bash
    openssl pkey -in priv.pem -out pub.pem
    ```

3.  Wyświetlenie kluczy:
    ```bash
    ls
    cat pub.pem
    cat priv.pem
    ```

4.  Wysłanie klucza publicznego:
    ```bash
    curl -X POST -F "id=125123" -F "zad2=@pub.pem" http
    ```

---

## Zadanie 3. Szyfrowanie RSA z OAEP

Szyfrowanie słowa za pomocą klucza publicznego **RSA** z trybem dopełnienia **OAEP**.

1.  Szyfrowanie:
    ```bash
    openssl pkeyutl -encrypt -pubin -inkey 'klucz ze strony' -in 'słowo do zahaszowania' -out encrypted.bin -pkeyopt rsa_padding_mode:oaep
    ```

2.  Kodowanie zaszyfrowanego pliku w Base64:
    ```bash
    base64 encrypted.bin > zad3.b64
    ```

---

## Zadanie 4. Szyfrowanie AES-256-CFB

Szyfrowanie słowa "Linux" za pomocą **AES-256-CFB** z podanym kluczem (K) i wektorem inicjującym (IV).

1.  Utworzenie pliku z tekstem jawnym:
    ```bash
    echo -n "Linux" > word.txt
    ```

2.  Próba szyfrowania (pierwsza, bez Base64):
    ```bash
    openssl enc -aes-256-cfb -in word.txt -out zad4.bin -K e592bc9e5fa8618a02edf437bdf3ffd4fcdfbe9e588749db898a3662b461a80e -iv 83c468dc47754386906912b6a1344416 -a
    ```

3.  Wyświetlenie zawartości:
    ```bash
    ls
    cat zad4.bin
    ```

4.  Próba szyfrowania z innym wyjściem lub opcją Base64 (różne warianty testowe):
    ```bash
    openssl enc -aes-256-cfb -in word.txt -out zad4.bin -K e592bc9e5fa8618a02edf437bdf3ffd4fcdfbe9e588749db898a3662b461a80e -iv 83c468dc47754386906912b6a1344416 -base64
    ls
    openssl enc -aes-256-cfb -in word.txt -out zad4v1.b64 -K e592bc9e5fa8618a02edf437bdf3ffd4fcdfbe9e588749db898a3662b461a80e -iv 83c468dc477543a6906912b6a1344416 -a
    ls
    openssl enc -aes-256-cfb -in word.txt -out zad4v1.txt -K e592bc9e5fa8618a02edf437bdf3ffd4fcdfbe9e588749db898a3662b461a80e -iv 83c468dc47754386906912b6a1344416 -a
    ```

5.  Wypisanie wyników:
    ```bash
    cat zad4.bin
    cat zad4v1.txt
    cat zad4v1.664
    ```

---

## Zadanie 5. Łamanie Hasła Hashcat (SHA3-384)

Wykorzystanie **Hashcat** do złamania skrótu (hash) z trybem **SHA3-384** (`-m 10800`) i atakiem z maską (`-a 3`).

1.  Zapisanie skrótu do pliku:
    ```bash
    echo -n "a30c81097d7fe490faf324c82cbc07a2cc74d0d9bab4e52e6a36c4ac01d2eabe7b707870dfd0bce15ba6a3573e9b6d2b" > zad5.txt
    ```

2.  Wyświetlenie pomocy Hashcat:
    ```bash
    hashcat -h
    ```

3.  Uruchomienie łamania (6 cyfr `?d?d?d?d?d?d`):
    ```bash
    hashcat -m 10800 -a 3 zad5.txt ?d?d?d?d?d?d
    ```

4.  Wyświetlenie znalezionego hasła:
    ```bash
    hashcat -m 10800 -a 3 zad5.txt ?d?d?d?d?d?d --show
    ```

5.  Wysłanie rozwiązania:
    ```bash
    curl -X POST -F "id=141241" -F "zad5=857686" http://
    ```

6.  Dodatkowa pomoc:
    ```bash
    hashcat --help
    ```

---

## Zadanie 6. Obsługa GPG

Import, wyświetlenie linii papilarnych i odszyfrowanie pliku z użyciem **GPG**.

1.  Import klucza GPG (publicznego lub prywatnego):
    ```bash
    gpg --import 'klucz z plikiem'.asc
    ```

2.  Wyświetlenie linii papilarnych (fingerprint):
    ```bash
    gpg --fingerprint
    ```

3.  Odszyfrowanie podpisanego/zaszyfrowanego pliku:
    ```bash
    gpg --decrypt 'nazwa podpisanego pliku'.asc
    ```

4.  Wysłanie rozwiązania (prawdopodobnie klucz ID i hasło):
    ```bash
    curl -X POST -F "id=125125" -F "D9B079387826129BA66242603AD9982A89283B:pomocdlaukrainy" http...
    ```
