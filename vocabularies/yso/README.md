Yleinen suomalainen ontologia YSO
=================================

Tässä kansiossa sijaitsevat sekä YSOn tuottamiseen käytetyt ohjelmanpätkät että itse YSOn kehitys ja julkaisuversiot.

### YSOn kehitysversion päivittyminen

Ajantasainen kehitysversio haetaan tasatunnein cronjobilla `dump-yso-to-svn` onki-kk:n Jena SDB-tietokannasta. Mikäli tietokantadumppi ei ole tyhjä tiedosto, samainen cronjob työntää sanaston GitHubiin käyttäen SVN:ää.

### YSOn julkaisuversion päivittyminen

YSOn purittaminen tapahtuu joka yö ajettavalla `update-yso-1-purify` cronjobilla. Kun puritus on valmis ajetaan `update-yso-2-deprecator` cronjob, joka vastaa käsitteiden hautaamisesta eli deprekoinnista. YSO viedään takaisin SDB-tietokantaan sekä vaiheen 1 että 2 päätteeksi. YSOn yöpäivitys viimeistellään ajamalla `update-yso-3-skosify` cronjob, joka valmistelee puritetusta ja deprekoinnit sisältävästä kehitysversiosta uuden julkaisuversion. Uutta julkaisuversiota ei lähetetä GitHubiin, jos yläkäsitteitä on enemmän kuin kolme tai tiedosto on tyhjä.

### YSAn muutosten päivittyminen YSOn kehitysversioon

YSAn muutokset tuodaan SDB-tietokannan YSO-kehitysversioon aina kuukauden ensimmäisenä päivänä. Päivityksestä vastaa `skos-history-ysa-yso-update` cronjob, joka löytyy onki-kk:n kansiosta `/etc/cron.d/`.  

### Ratkaisuja yleisiin ongelmatilanteisiin

Kootaan tänne yleisimpiä YSO-niksejä.

#### YSO ei päivity koska yläkäsitteiden lukumäärä on jotain muuta kuin kolme:

YSOn kehitysversioon on todennäköisesti lipsahtanut uusi käsite, jolla ei ole vielä merkattuna yläkäsitettä. Tällöin käsite tulkitaan virheellisesti YSOn uudeksi yläkäsitteeksi ja julkaisu estetään. Tilanne ratkeaa automaattisesti kun virheellinen data on korjattu SDB-tietokantaan TBC:llä.

Jos yläkäsitteiden määrä on nolla, kehitystiedosto on todennäköisesti tyhjä jonkin käsittelyssä tapahtuneen virheen takia. Tällöin oikea ratkaisu on mennä palvelimelle katsomaan miksei päivitysputki toimi oikein.

#### YSOn julkaisuversion käsitteillä näkyy ylimääräisiä kehitykseen liittyviä propertyjä:

YSOn kehitysversiossa on tällöin otettu todennäköisesti käyttöön uusi property, mutta sitä ei ole muistettu lisätä poistettavien propertyjen joukkoon skosifyn konfiguraatiossa `finnonto.cfg`.