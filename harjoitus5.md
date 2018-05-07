Tein harjoituksen kotonani HP Pavilion 15-aw022no-tietokoneella, käyttöjärjestelmänä toimi 64-bittinen Ubuntu 16.04.3 livetikulta. Livetikku toteutettu Pendrivelinuxilla.
Tehtävät Tero Karvisen sivuilta. http://terokarvinen.com/2018/aikataulu-%e2%80%93-palvelinten-hallinta-ict4tn022-4-ti-5-ke-5-loppukevat-2018-5p

## a) Valitse aihe omaksi kurssityöksi ja varaa se kommenttina aikataulusivun perään.

Kurssityön aiheeksi valitsen "websivukehittäjälle" tarkoitetun koneen, eli Apachea, kuvanmuokkausohjelmaa, etäyhteyksiä etc...

## b) Julkaise raportti MarkDownilla. Jos käytät GitHub:ia, se tekee muotoilun automaattisesti “.md”-päätteisiin dokumentteihin.

Kohdan c raportti.

## c) Aja oma Salt-tila suoraa git-varastosta. Voit joko tehdä tilan alusta lähtien itse tai forkata sirottimen.

Ensiksi loin perus master-slave arkkitehtuurin, jossa oli molemmat omalla koneellani. 

> sudo apt-get update
> sudo apt-get install -y salt-minion salt-master

Minionille masteri siirtymällä tiedostoon

> sudoedit /etc/salt/minion

Ja tiedoston loppuun:

> master: 127.0.0.1
> id: mati

Käynnistin minionin uudestaan, jotta muutokset tulee voimaan.

> sudo systemctl restart salt-minion.service

Sitten hyväksyin masterilla avaimen

> sudo salt-key -A

Testasin vielä toimivuuden komennolla 

> sudo salt '*' cmd.run whoami

Josta sain oikean vastauksen, eli "root".

Sitten tein Salt-tiloille kansion, jonne loin tilat. Tilat löytyy salt kansiosta osoitteesta https://github.com/matilinux/test.git

Tilat asentuivat komennolla *sudo salt '*' state.highstate*  niinkuin pitikin

Siirsin tilat Githubiin tekemääni repositoryyn (ohjeet repositoryn tekemiseen http://terokarvinen.com/2016/publish-your-project-with-github) ihan vain klikkamalla upload files ja raahaamalla /srv/salt kansion sinne. 

Poistin omalta koneeltani /srv/salt kansion komennolla 

> sudo rm -r /srv/salt/

Jonka jälkeen latasin ne takaisin komennollasudo

> git clone https://github.com/matilinux/test.git

Se latasi nähtävästi koko test kansion, josta siirsin vain halutut osoitteeseen /srv

> sudo mv test/salt/ /srv/

Poistin openssh serverin ja curlin, koska ne olivat edellisistä testeistä asennettuina

> sudo apt-get purge openssh-server curl -y

Nyt kokeilin uudestaan, että kaikki toimii niinkuin aikaisemminkin. Ajoin komennon

>sudo salt '*' state.highstate

Josta saatiin seuraavanlainen sanoma

```
xubuntu@xubuntu:~$ sudo salt '*' state.highstate
[WARNING ] Key 'file_ignore_glob' with value None has an invalid type of NoneType, a list is required for this value
[WARNING ] Key 'file_ignore_glob' with value None has an invalid type of NoneType, a list is required for this value
[WARNING ] Key 'file_ignore_glob' with value None has an invalid type of NoneType, a list is required for this value
[WARNING ] Key 'file_ignore_glob' with value None has an invalid type of NoneType, a list is required for this value
mati:
----------
          ID: curl
    Function: pkg.installed
      Result: True
     Comment: The following packages were installed/updated: curl
     Started: 20:39:37.846616
    Duration: 9254.071 ms
     Changes:   
              ----------
              curl:
                  ----------
                  new:
                      7.47.0-1ubuntu2.7
                  old:
----------
          ID: openssh-server
    Function: pkg.installed
      Result: True
     Comment: The following packages were installed/updated: openssh-server
     Started: 20:39:47.109129
    Duration: 6279.496 ms
     Changes:   
              ----------
              openssh-server:
                  ----------
                  new:
                      1:7.2p2-4ubuntu2.4
                  old:
              ssh-server:
                  ----------
                  new:
                      1
                  old:

Summary for mati
------------
Succeeded: 2 (changed=2)
Failed:    0
------------
Total states run:     2
```

Kokeilin toimiiko ssh komennolla

ssh localhost

Johon vastattiin kysymällä sormenjälkiä, eli voidaan todeta tämän onnistuneeksi.

Lähteitä:
https://help.github.com/articles/basic-writing-and-formatting-syntax/
http://terokarvinen.com/2016/publish-your-project-with-github
http://terokarvinen.com/2018/aikataulu-%e2%80%93-palvelinten-hallinta-ict4tn022-4-ti-5-ke-5-loppukevat-2018-5p
https://github.com/kristiansyrjanen/servermanagementcourse/blob/master/Harjoitus5.md





