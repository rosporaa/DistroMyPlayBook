# DistroMyPlayBook
Win executables for MyPlayBook

Download all files, unpack to directory and run mesec.bat

You can create your own database with story/adventure.

Co je MyPlayBook?

# MyPlayBook
program z ponuknutej sqlite databazy vytvori zvuky (ak uz neexistuju) a spusti/prejde hru (gamebook, playbook). Spustenie/prejdenie hry znamena, ze program cita a zobrazuje jednotlive 'miestnosti/akcie' z databazy a caka na interakciu uzivatela.

Spustenie:
pb.exe [-h] [--lang LANG] [--notext] database

 - Povinny argument
   - database     SQLite databaza s playbook datami
 - Nepovinne argumenty:
   -  -h, --help   Zobraz help
   - --lang LANG  Pouzi dany jazyk (en, es, sk ...)
   - --notext     Nezobrazuj texty (iba zvuk)
  
Interakcia uzivatela:
  - sipky pre prechod do inej miestnosti/akcie
  - enter na opatovne precitanie textu
  - medzeru na celkove ukoncenie programu

Program sa pripaja cez modu gTTS na Internet a generuje zvukove subory.
