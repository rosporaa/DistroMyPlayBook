# Ditribution for simpleGameBook
Win executables for simpleGameBook.
Created with pyinstaller from python 3.9.2 - only usable on Window 8/10

To use small test gameBook:
  - Click green button 'Code' - Download zip file.
  - Unpack downloaded zip file to directory and run(click) sgb_en.bat (or sgb_sk.bat).

You can create your own database with story/adventure/game book.

Co je simpleGameBook?

# simpleGameBook
program z ponuknutej sqlite databazy vytvori zvuky (ak uz neexistuju) a spusti/prejde hru (gamebook). Spustenie/prejdenie hry znamena, ze program cita a zobrazuje jednotlive 'miestnosti/akcie' z databazy a caka na interakciu uzivatela.

Spustenie:
sgb.exe [-h] [--lang LANG] [--notext] database

 - Povinny argument
   - database     SQLite databaza s gameBook datami
 - Nepovinne argumenty:
   -  -h, --help   Zobraz help
   - --lang LANG  Pouzi dany jazyk (en, es, sk ...)
   - --notext     Nezobrazuj texty (iba zvuk)
  
Interakcia uzivatela:
  - sipky pre prechod do inej miestnosti/akcie
  - enter na opatovne precitanie textu
  - medzeru na celkove ukoncenie programu

Program sa pri prvom spusteni daneho gamebooku pripaja cez modu gTTS na Internet a generuje zvukove subory. Ak sa programu nepodari vygenerovat zvuky (nedostupny Internet), zobrazuje iba texty gamebooku. Vygenerovanie zvukov sa opakuje pri dalsom spusteni programu.

# Vytvorenie vlastneho pribehu

Pribeh je mozne pisat priamo do SQLIte databazy (nap. pomocou DB Browser for SQLite), alebo je ho mozne zapisat do textoveho dokumentu a 'prekonvertovat' do SQLite databazy pomocou programu dbmaker.exe.

Cistu databazu s potrebnymi tabulkami je mozne vytvorit pomocou programu dbmaker.exe zadamin: dbmaker.exe --empty

Struktura textoveho dokumentu, ktory je mozno konvertovat do databazy je nasledovna:

Databaza potrebuje tri tabulky : settings, rooms, routes.
Obsah tychto tabuliek teda musi byt uvedeny v textovom subore.

!tabulka setting obsahuje obecne nastavenie, jazyk, aky sa pouziva a vseobecne vety pouzite v programe (vid. nizzsie za riadkom s $S)
   - v tomto texte riadok tabulky setting zacina $S 
      - moze byt viacej riadkov v DB - pre rozne jazyky (rozna polozka LANG)
      - iba jeden riadok by mal mat nastavene ACTUAL = 1. Tento jazyk sa spusti automaticky (Iny jazyk pri spusteni programu 'pb' je mozne zadat s volbou --lang).
      - vsetky polozky musia byt vyplnene (NAME, AUTHOR, LANG, STARTROOM, .....)
      - kazda polozka v tomto texte ma strukturu:
         - NAZOV_POLOZKY hodnota_polozky_alebo_text (na oddelovanie pouzivajte IBA medzery, nie tabulatory a pod.)
         - napr.    LANG ru 

!tabulka rooms obsahuje cisla miestnosti/akcie/atd.. ,kratky a dlhy popis miestnosti/akcie/... v danom jazyku,  nazov suboru .wav, ktory by sa mal prehrat pri vstupe/pouziti miestnosti
   - v tomto texte riadky tabulky rooms zacinaju $R 
      - riadky mozu obsahovat rovnake cisla miestnosti pre rozne JAZYKY.
      - kazda miestnost je popisana 6mi riadkami
      - struktura v texte: 
         - @N   - cislo miestnosti - unikatne pre dany JAZYK! - na jednom novom riadku, zacina znakom @ (kazda miestnost zacina takto znakom @)
         - lang - znacka jazyka, napr. sk, en, ru, es - na jednom novom riadku.
         - short descriprion - kratky popis, zobrazi sa ako nadpis/nazov/kapitola. IBA na jednom riadku.
         - long description  - dlhy popis miestnosti, co sa stalo apod. Cely text musi byt IBA na jednom riadku.
         - route_description - popis cinnosti prip. miesta, co mas spravit, kam mas ist, aby si sa dostal do tejto miestnosti. IBA na jednom riadku.
         - bg_sound          - je nazov .wav suboru, ktory bude prehrany na pozadi (napr, les, krcma, ohen...). Je potrebne ho pisat s celou cestou napr. bgs/zvuk.wav. Znak - znamena, ze bez zvuku.

!tabulka routes zacina $O (nie nula ale o) 
   - obsajuhe pre kazdu miestnost smery/akcie (1,2,3,4) do inej miestnosti 
   - nemusia byt vsetky 4 smery/akcie (maximum je ale 4 smery/akcie)
   - ak je smer/akcia zaporna, je to ukoncenie pribehu, vid. nizsie
   - kazdy riadok obsahuje tri cisla oddelene medzerou
   - strukrura v texte
     -  cislo_miestnosti_odkial(v ktorej sme) cislo_vychodu(1,2,3,4) cislo_miestnosti_kam_vychod_vedie_alebo_zaporne_cislo_pre_ukoncenie_hry
     -  napr. 3 5 23
      
 V tabulke routes moze byt ako cislo vychodu (posledne cislo) uvedene aj zaporne cislo. To znamena, ze tu pribeh konci (koncov pribehu moze byt viac)
 
 Zaporne cisla znamenaju:
   - -1 Našli ťa mŕtveho po niekoľkých dňoch...
   - -2 Vrátil si sa  domov a užívaš si nadobudnuté bohatstvo...
   - -3 Vrátil si sa domov ako veľký hrdina. Teraz môžeš aj ty rozprávať v krčme svoje dobrodružstvá...
   - -4 Si rád, že si sa vrátil domov živý a zdravý. Už viac nikam nepôjdeš a radšej o svojom dobrodružstve ani nikomu nič nepovieš.
       
Priklad celeho suboru:
```
#riadok tabulky settings - vsetky hodnoty MUSIA byt vyplnene. Vacsinou staci zmenit NAME, AUTHOR, LANG, STARROOM a ACTUAL
$S
NAME        Testovací príbeh
AUTHOR      VlNa
LANG        sk
STARTROOM   1
END_DEF     To je koniec tvojho dobrodružstva.
END_DEAD    Našli ťa mŕtveho po niekoľkých dňoch...
END_GOLD    Vrátil si sa  domov a užívaš si nadobudnuté bohatstvo...
END_HERO    Vrátil si sa domov ako veľký hrdina. Teraz môžeš aj ty rozprávať v krčme svoje dobrodružstvá... 
END_LIVE    Si rád, že si sa vrátil domov živý a zdravý. Už viac nikam nepôjdeš a radšej o svojom dobrodružstve ani nikomu nič nepovieš.
QUESTION    Čo urobíš?
MOVE_1      Šípka hore
MOVE_2      Šípka dole
MOVE_3      Šípka vpravo
MOVE_4      Šípka vľavo
ENTER       Enter:  Prerozprávaj odznova.
ACTUAL      1
GTTS_SLOW   0
NO_WAY      Neplatná voľba
EXIT_GAME   Medzera: ukončí program.
TALKING     Rozprávam...
THE_END     KONIEC
#riadky tabulky rooms - popis miestnosti a akcii
$R
@1
sk
Zlatá miestnosť
Si v zlatej miestnosti. Pred sebou vidíš strieborné dvere.
#ako sa do tejto miestnosti dostaneme z inych miestnosti
Prejdi cez zlaté dvere.
bg/wind.wav
@2
sk
Strieborná miestnosť
Si v striebornej miestnosti.  Pred sebou vidíš zlaée dvere.
Prejdi cez strieborné dvere.
bg/wind.wav
#riadky tabulky routes - odkial sa kam ide
$O
#z prvej miestnosti prvym vychodom do druhej miestnosti
1 1 2
#z druhej miestnosti prvym vychodom do prvej miestnosti
2 1 1
```
