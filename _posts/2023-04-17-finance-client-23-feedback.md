---
layout: post
title: "Some articles are just so short that we have to make the footer stick"
categories: misc
published: false
---

Key take aways drejer sig grundlæggende om udfordringer i ledelsen og hvordan ens idé til en software løsning er baseret på ikke-validerede
hypoteser og mangel på evidens fra potentielle kunder og aftagere.

- Idéen er ikke blevet valideret.
- Idéen er ikke brudt ned i iterationer.
- Idéen har ikke været igennem indledende øvleser som f.eks. user-journey mapping.
- Idéen forsøges implementeret med Big Design Up Front (BDUF).
- Ledelsen anses for at være dem der ved bedst.
- Passiv aggresivitet når der udfordres fra teamets medlemmer imod ledelsen.
- Silo kommunikation uden transparens.
- Process er stort set ikke eksisterende.
- Ledelsen dikterere fra hånd-til-mund. Intet er beskrevet i tekst.
- Glimrende eksempel af Planning fallacy.
- Glimrende eksempel på Conways lov.
- Teknik over forretningsmæssig værdiskabelse.

Teamet har været forankret på baggrund af en traditionel beureaukratisk organisering. Organisatorisk har det set således ud;

Team-manager og kravstiler (samme person)
    -> Arkitekt og people/first-line manager (samme person).
        -> Domæne eksperter.
        -> Udviklere.

I begyndelsen er holdet 4 mand med 3 andre på på siden.

Over tid bliver dette til 7 mand og 2 andre på siden (pseudo scrum master kommer til efter 5 måneder samt en forretnings-udvikler efter ca. 6).

Det er værd at tage notits af, at fra begyndelsen og til slutningen af min oplevelse, bliver denne konstellation langsomt udfordret. Den vokser og "bobler" i størrelse. 
Vi går dog ikke "hårdt" fra at være 4 til 7 men det sniger sig langsomt ind, hvor interaktionen imellem kravstiller og dele af holdet vokser i personer.

Det gør den, er min antagelse, umiddelbart efter managers behov, og højst sandsyndlig grundet pres og sin fiksering på kunstige deadlines.

Manager agerer også kunde og kravstiller i dette tilfælde. Det har vedkommende givet gjort på baggrund af optaget viden fra andre steder i organisationen samt et muligt ønske fra vedkommendes ledelse omkring et tiltag som f.eks. digital transformation og/eller effektivitet etc. 

Fra den viden er der kommet en idé til et internt produkt. Fra denne idé er der kommet et løsningsforslag. Dette løsningsforslag er blevet implementeret ud fra BDUF.

Langt størstedelen (90%) af løsningenens krav har været baseret på tekniske ønsker og ikke brugerens/forretningens værdiskabelse. Teknikken har været målet, langt mere end
at forretningen fik hvad de potentielt så som værdiskabende. Arkitekten har haft for vane, at bede om løsninger fra enkeltpersoner som det lige passede ind i vedkommendes
egen verden. Forventninger er afstemt mundligt. Tilbageløb.

Denne viden syntes heller ikke valideret med andre interne organisationer eller forretningen. Andre interne organisationer er blot anset som eksertne aftagere men uden at bringe dem særlig tæt ind i implementeringen og meget få udfrakommende ønsker har fundet vej ind i implementeringen. Med den store undtagelse af, at når kravstiller, arkitket og domæne-eksperter er blevet klogere. Men det er sket efter man har implementeret en løsning omkring BDUF.

Når kravene har ændret sig fordi man er blevet klogere undervejs, og der allerede har forelagt diagrammer og tekniske implementationer for sit tidligere BDUF løsning så har det over tid taget moralen fra hele teamet. 

Man har følt sig i konstant forandring uden at vide hvad der egentlig kan lade sig gøre, hvad en success er, hvad der skal laves om, laves til, og hvad de nye deadlines er for en løsning som ingen ved bliver forandret igen lige om lidt. 

Der er som der i disse organiationer samtidig er en manglende udfordring til kravstiller, altså kunden, altså manageren. Der skrives intet op på en opgaveliste når manager præsenterer ønsker. Ønsker kan være af enhver arbitræt størrelse uden umiddelbar drøftelse af afhængigheder og prioriteringer. Et ønske bliver derfor ikke konkret fordi der ikke stilles indgående spørgsmål og kravstiller involverer sig ikke i det praktiske ift. eksekvering. Vedkommende forsøger at sætte en retning og derfra forsvinder vedkommende fra den daglige ekskvering.

Kravstiller har en godhjertet forventning om, at mundtlige krav kan omdannes til forretningens behov. Disse bliver på ad-hoc vis valideret med én eller to potentielle aftagere af produktet. Men forretningen er ikke til stede i eksekveringen og har på intet tidspunkt deltaget i omdannelsen af krav til reele behov hos forretningen. 

Altså, kravstiller som er manager er kilden til hvilke krav der eksisterer. Derfor bliver det uundgåeligt at det samtidig er vedkommende "man går til" når der skal afdækkes
hvad forventningen egentligt er. Husk at vedkommende ikke er til stede i den daglige eksekvering.

En anekdote;

Over et par måneder designes og implementeres der en teknisk kontrakt (kontrakten er en stor del af produktet) som skal holde på informationer som evt. aftager af kontrakten kan benytte.

Denne kontrakt er stor og kan holde på mange informationer. Disse informationer skal over tid hentes fra potentielt mange forskellige systemer og kilder.

Der bliver implementeret delvise informationer i kontrakten men udviklingsholdet bliver naturligvis klogere undervejs og når data fra eksterne kilder enten 
ikke eksisterer eller passer i kontrakten, bremses de.

Kravstiller får indsigt i det arbejde som er forestået og udført. Vedkommende vælger at skærer kontrakten til og lave den om.

Det tager mange møder af få "kontrakten på plads", alt imens udviklingsholdet er sat mere eller mindre på standby.

Nu er der altså gået flere måneder med arbejde som kravstiller mener er delvist irrelevant eller unødvendigt.

Samtidig deles organisationen op i to:

- En del skal tage sig af kontraktens udformning; domæne-ejeren i spidsen samt en udvikler som implementere kontraktens design up-front.
- Anden del skal implementere det bagvedliggende af kontrakt som består sig i, at finde data og udstille disse igennem kontrakten.

Der er mange møder om den kontrakt. Fordi ønsket er "at den skal være færdig". Også selvom der syntes, at komme nye tiltag til den ugentligt.

En anden anekdote;

Kommunikationen er helt tydeligt forankret omkring topstyring. Kravstilleren kommunikerer internt til arkitekt men samtidigt også til domæne-eksperter. Udviklerne
bliver klædt på af arkitekt. 

Det medfører initielt at der er rimelig ro omkring udviklingsforløbet men husk, at kravstiller er idémand og har hverken valideret eller trygprøvet sine idé-hypoteser 
uden for organisationen. Der er ganske enkelt ikke fra starten af forløbet blevet spurgt eller på nogen måde delagtiggjort til mulige anvendere eller interne kunder.

Fra det øjeblik, at kravstiller siger sig imod det arbejde som initielt allerede er lavet, skrider resten af organisationen under vedkommende.

Kravstiller kommunikerer sine nye ønsker, som stadig ikke er nedfældet men kun eksisterer ud fra vedkommendes egen viden og bedst mulige ønsker. Vedkommende må det derfor 
antages mener, at vide bedre end samtlige potentielle kunder og aftagere. 

Arkitekten er underlagt kravstiller, både 
