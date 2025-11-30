---
title: Tekoälyllä tuotettujen tietoturvatutkimusten vaikutus avoimen lähdekoodin projekteihin.
description:
    'Tekoäly on saavuttanut jalansijaa tietoturvatutkimuksessa samaa tahtia kuin muillakin elämän
    osa-alueilla. Ilmiö on osoittautunut erityisen ongelmalliseksi etenkin avoimen lähdekoodin, eli
    OSS, -projektien ylläpitäjille. Tekoälyn teoreettiset hyödyt tietoturvatutkimuksessa eivät
    realisoidu OSS-ylläpitäjille, vaan tekoälyllä tuotetut tulokset kuormittavat kehittäjiä.'
pubDate: 'Nov 30 2025'
published: false
heroImage: ./pile-of-trash.png
---

<i>Tämä julkaisu on kirjoitettu koulutehtävää varten. Sisältö ei välttämättä ole anteliasta, jos on
aiheesta jo aikaisemmin kuullut.</i>

Tämän vuoden toukokuussa tunnetun Curl-kirjaston ylläpitäjä Daniel Stenberg saatiin voimakas
ulostulo turhautumisessaan tekoälyn tuottamiin "AI slop" -tietoturva raportteihin [1].

> That's it. I've had it. I'm putting my foot down on this craziness.
>
> <i> - Daniel Standberg LinkedIn-alustalla</i>

Monet alan julkaisut haastattelivat Stenbergiä aiheen tiimoilta ja asia oli vahvasti esillä.
Stenberg ylläpitää [listaa](https://gist.github.com/bagder/07f7581f6e3d78ef37dfbfc81fd1d1cd)
saamistaan raporteista, joissa väitetään löydetyn vakavia haavoittuvuuksia Curl-kirjastossa. Näitä
haavoittuvuuksia ei kuitenkaan ole olemassa. Alla aiheen tiimoilta muutama artikkeli ja video.

- [AI slop attacks on the curl project - Daniel Stenberg](https://www.youtube.com/watch?v=6n2eDcRjSsk)
- [Death by a thousand slops](https://daniel.haxx.se/blog/2025/07/14/death-by-a-thousand-slops/)
- [the curl situation keeps getting worse… ](https://www.youtube.com/watch?v=8w6r4MKSe4I)
- [Curl Fights a Flood of AI-Generated Bug Reports From HackerOne](https://thenewstack.io/curl-fights-a-flood-of-ai-generated-bug-reports-from-hackerone/)
- [Curl project founder snaps over deluge of time-sucking AI slop bug reports](https://www.theregister.com/2025/05/07/curl_ai_bug_reports/)

Tekoälyn hallusinoimien haavoittuvuuksien tarkastaminen kuormittaa merkittävästi etenkin isojen
OSS-projektien ylläpitäjiä. Vielä suurempi tulva olemattomia haavoittuvuuksia raportoidaan niihin
projekteihin liittyen, joilla on olemassa avoin "bug bounty" -palkitsemisohjelma. Bug bounty
-ohjelmista voit lukea lisää esimerkiksi
[täältä](https://www.hackerone.com/blog/what-are-bug-bounties-and-how-do-they-work).

## Verrattavissa palvelunestohyökkäyksiin

Tekoälyn havaitsemien "haavoittuvuuksien" pääasiallienn haitta on niiden tutkimiseen kuluva aika.
Python ja Curl -projektien ylläpitäjät kertovat lisääntyneestä stressistä, turhautumisesta,
tuhlatusta ajasta ja loppuunpalamisesta [2], [3]. Stenberg on verrannut "AI slop" -raportteja
palvelunestohyökkäyksiin. Ne siirtävät resursseja pois oikeasta kehitystyöstä ja murentavat
luottamusta sekä raportteihin ja raportoijiin, että OSS-projekteihin [3].

Python -projektin tietoturvaraporttien -triage tiimiin kuuluva Seth Larson on yksi aiheesta huolta
ilmaisseista [2]. Hän suhtautuu tekoälyllä tuotettuihin raportteihin, kuin niiden tarkoitus olisi
haitallinen, vaikkeivat ne sitä tavoittelisikaan [2].

## Kohina sokeuttaa

"AI slop" -raportit eivät ainoastaan polta kehittäjiä loppuun. Hallusinoitujen raporttien tulva
hukuttaa oikeita raportteja alleen [4]. Tekoälyn hallusinaatiot voivat jopa pahimmillaan tukahduttaa
avoimen lähdekoodin projekteja kokonaisuudessaan [4]. OSS-projekteihin osallistuvat kehittäjät eivät
lähtökohtaisestikaan käytä aikaansa tietoturvan parantamiseen [2]. Virheelliset raportit kääntävät
mielenkiintoa entisestään pois.

## Jotain positiivista

Tekoäly on voimakas työkalu, ja sitä se voi olla tälläkin saralla. Tietoturva-alalla toimiva Joshua
Rogers löysi curl-kirjastosta yli sata ongelmaa käyttäen puhtaasti tekoälytyökaluja [5]. En löytänyt
Stenbergin mastodon julkaisusta kommenttia, että mikään näistä olisi ollut haavoittuvuus. Tämä on
silti osoitus tekoälyn tehokkuudesta koodin analysoinnissa oikeiden käsien ohjaamana.

## Loppusanat

Tekoäly on väline, joka skaalaa käyttäjänsä toimintaa – niin hyvässä kuin pahassa. Taitamattomissa
käsissä se toimii roskapostitykin tavoin, kuormittaen jo valmiiksi tiukoilla olevia vapaaehtoisia
ylläpitäjiä ja uhaten jopa kokonaisten projektien elinvoimaisuutta. Kuten Curl-tapaus ja Stenbergin
turhautuminen osoittavat, ongelma ei ole pohjimmiltaan itse teknologiassa, vaan sen vastuuttomassa
käytössä, jossa nopean rahastuksen toivo syrjäyttää ammattitaidon ja huolellisuuden.

Jotta avoimen lähdekoodin yhteisö ei tukehtuisi tähän digitaaliseen saasteeseen, tarvitaan muutosta
sekä bug bounty -alustojen pelisääntöihin että raportointikulttuuriin. Joshua Rogersin esimerkki
valaa kuitenkin toivoa: kun tekoäly valjastetaan asiantuntijan työkaluksi eikä hänen korvaajakseen,
se voi parantaa ohjelmistojen laatua merkittävästi. Tulevaisuuden tietoturvatutkimus hyötyy tekoälyn
laskentatehosta, mutta se ei selviä ilman ihmisen tekemää kriittistä arviointia ja validointia.

---

[1] D. Stenberg, “That’s it. I’ve had it. I’m putting my foot down on this craziness. 1. Every
reporter submitting security reports on...,” May 04, 2025.
https://www.linkedin.com/posts/danielstenberg_hackerone-curl-activity-7324820893862363136-glb1

[2] S. M. Larson, “New era of slop security reports for open source,” sethmlarson.dev.
https://sethmlarson.dev/slop-security-reports

[3] D. Stenberg, “Death by a thousand slops,” daniel.haxx.se, Jul. 14, 2025.
https://daniel.haxx.se/blog/2025/07/14/death-by-a-thousand-slops/

[4] L. Franceschi-Bicchierai, “AI slop and fake reports are coming for your bug bounty programs,”
TechCrunch, Jul. 24, 2025. [Online]. Saatavilla:
https://techcrunch.com/2025/07/24/ai-slop-and-fake-reports-are-exhausting-some-security-bug-bounties/

[5] D. Stenberg, “Joshua Rogers sent us a massive list of potential issues...,” Mastodon, 21.9.2025.
[Online]. Saatavilla: https://mastodon.social/@bagder/115241241075258997
