---
layout: post
title: "Some articles are just so short that we have to make the footer stick"
categories: misc
published: false
---

Key take away drejer sig om en grundlæggende udfordring i ledelsen ift. hvordan man de sidste mange år IKKE har bygget software produkter.

Teamet har været forankret på baggrund af en traditionel beureaukratisk opstilling. Organisatorisk har det set således ud;

Manager og kravstiler (samme person)
    -> Arkitekt og people manager (samme person).
        -> Domæne eksperter
        -> Udviklere

I begyndelsen er holdet 4 mand med 3 på siden.

Over tid bliver dette til 7 mand og 2 på siden (pseudo scrum master kommer til efter 5 måneder samt en forretnings-udvikler efter ca. 6).

Det er værd at tage notits af at fra begyndelsen og til slutningen af min oplevelse bliver denne konstellation langsomt udfordret, så den vokser og "bobler" i størrelse. 
Vi går ikke "hårdt" fra at være 4 til 7 men det sniger sig langsomt ind, hvor interaktionen imellem kravstiller og dele af holdet vokser.

Det har den gjort umiddelbart efter managers behov er min antagelse, og dette er sket er min antagelse, grundet pres.

Manager agerer også kunde og kravstiller i dette tilfælde. Det har vedkommende naturligvis gjort på baggrund af noget viden fra andre steder i organisationen. Viden er tilmed ikke valideret med andre interne organisationer og de andre interne organisationer er blot anset som eksertne aftagere men uden at bringe dem særlig tæt ind. Manager er altså ydre kravstiller. Sat på spidsen; manager har fået en idé omkring et produkt, "gjort det færdigt på papiret" og "oppe i hovedet". Så har arkitekt tegnet diagrammer og flows. Og derfra har udviklingsholdet skulle arbejde. Ret klassisk og tradiotionelt setup.

Det er værd at bide mærke i kompleksiteten af de på papiret ønskede løsning. Der er slet ingen tvivl om, at den på papiret er over-engineered.

Der er samtidig ikke præsenteret, en eneste gang, vision eller mission omkring det ønskede produkt.

Der er samtidig ikke præsenteret, en eneste gang, nedfælede krav fra manager udover abstraktioner og til tider meget detaljeret ønsker til endelige mål. Men alt sammen mundligt.

Der er som der er i disse organiationer samtidig en manglende udfordring til kravstiller, altså kunden, altså manageren. Der skrives ikke intet op på en opgaveliste når manager præsenterer ønsker. Ønsker kan være af enhver arbitræt størrelse uden umiddelbar drøftelse af afhængigheder og prioriteringer. Et ønske bliver derfor ikke konkret fordi der ikke stilles indgående spørgsmål og kravstiller involverer sig ikke i det praktiske ift. eksekvering. Vedkommende forsøger at sætte en retning og derfra forsvinder vedkommende fra den daglige ekskvering.

Kravstiller har en godhjertet forventning om, at mundtlige krav kan omdannes til forretningens behov. Disse bliver på ad-hoc vis valideret med én eller to potentielle aftagere af produktet. Men forretningen er ikke til stede i eksekveringen og har på intet tidspunkt deltaget i omdannelsen af krav til reele behov hos forretningen. 

Altså, kravstiller som er manager er kilden til hvilke krav der eksisterer. Derfor bliver det uundgåeligt at det samtidig er vedkommende "man går til" når der skal afdækkes
hvad forventningen egentligt er. Husk at vedkommende ikke er til stede i den daglige eksekvering.

En anekdote;

Over et par måneder designes og implementeres der en teknisk kontrakt (kontrakten er en stor del af produktet) som skal holde på informationer som evt. aftager af kontrakten kan benytte.

Denne kontrakt er stor og kan holde på mange informationer. Disse informationer skal over tid hentes fra mange forskellige systemer.

Der bliver implementeret delvise informationer i kontrakten men udviklingsholder bliver naturligvis klogere undervejs og når data fra eksterne kilder enten 
ikke eksisterer eller passer i kontrakten, bremses de.

Kravstiller får indsigt i det arbejde som er forestået og udført. Vedkommende vælger at skærer kontrakten til og lave den om.

Det tager mange møder af få "kontrakten på plads", alt imens udviklingsholdet er sat mere eller mindre på standby.

Nu er der gået flere måneder med arbejde som kravstiller mener er delvist irrelevant.

Samtidig deles organisationen op i to:

- En del skal tage sig af kontraktens udformning; domæne-ejeren i spidsen samt en udvikler som implementere kontraktens design up-front.
- Anden del skal implementere det bagvedliggende af kontrakt som består sig i, at finde data og udstille disse igennem kontrakten.

Der er mange møder om den kontrakt. Fordi ønsket er "at den skal være færdig".

Flere gange i forløbet 

Der forhandles muligvis mundtligt.

Den generelle "notion" i forløbet har været:

Vi har travlt (- men vi ved ikke hvordan vi kravstiller).
