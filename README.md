# Ditribution for simpleGameBook
Win executables for simpleGameBook.
Created with pyinstaller from python 3.9.2 - only usable on Window 8+

Click green button 'Code' - Download zip file.
Unpack to directory and run sgb.bat

You can create your own database with story/adventure.

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

Program sa pripaja cez modu gTTS na Internet a generuje zvukove subory.
