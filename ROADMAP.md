# Kotisatama — Roadmap

*Päivitetty kesäkuu 2026*

---

## v0.1 — Proof of Concept ✅
*Tavoite: Toimiiko whitelist-idea ylipäätään?*

- [x] FFF-forkin kloonaus ja projektin rakenne
- [x] `WhitelistManager`-luokka (kovakoodattu lista)
- [x] `WebRequestInterceptor` — estää ei-whitelistatut URL:t
- [x] 403-virheviesti käyttäjälle
- [x] Manuaalinen testaus emulaattorilla

---

## v0.2 — Hallittava whitelist
*Tavoite: Whitelist ei ole enää kovakoodattu.*

- [ ] Whitelistin lataus paikallisesta JSON-tiedostosta
- [ ] Yksinkertainen hallintanäkymä (lisää/poista domain)
- [ ] Whitelistin tallennus SQLiteen
- [ ] "Avomeri"-tila: käyttäjä voi tilapäisesti poistua kotisatamasta
- [ ] Visuaalinen siirtymä kotisataman ja avomeren välillä

---

## v0.3 — Ilio-palvelin ja synkronointi
*Tavoite: Whitelist tulee pilvestä, ei laitteelta.*

- [ ] Ilio-backend (minimaalinen): whitelist JSON -endpoint
- [ ] Selain hakee whitelistin automaattisesti käynnistyksessä
- [ ] Kaksi tasoa: `self_managed` (perhe hallitsee) ja `ilio_managed` (Ilio hallitsee)
- [ ] Whitelistin versionumero — päivitys vain jos muuttunut
- [ ] Offline-fallback: käytetään viimeksi ladattua whitelistiä

---

## v0.4 — Sertifikaatit ja laiteidentiteetti
*Tavoite: Laite = identiteetti, ei salasanaa.*

- [ ] Ilio Root CA ja Intermediate CA pystytetty (offline root)
- [ ] Asiakassertifikaatin generointi laitteelle ensiasennuksessa
- [ ] Palvelinyhteys vaatii validi asiakassertifikaatti (mTLS)
- [ ] Sertifikaatin peruutus etänä (CRL tai OCSP)
- [ ] Testi: varastettu laite — sertifikaatti peruutetaan, yhteys katkeaa

---

## v0.5 — Hakukone
*Tavoite: Haku toimii, mutta hakee vain kotisatamasta.*

- [ ] Meilisearch-instanssi pystytetty palvelimelle
- [ ] Crawler indeksoi whitelist-sivustot (viikoittain)
- [ ] Hakukenttä selaimessa — tulokset Meilisearchista
- [ ] Fallback-viesti: *"Ei löydy kotisatamasta — haluatko mennä avomerelle?"*
- [ ] Fallback-haut lokitetaan datana (mitä whitelistiltä puuttuu)

---

## v0.6 — Hallintapaneeli
*Tavoite: Ostaja (vanhempi / aikuinen lapsi) voi hallita ilman teknistä osaamista.*

- [ ] Web-pohjainen hallintapaneeli (ei erillistä apppia)
- [ ] Laitteiden hallinta: lisää, poista, nimeä
- [ ] Whitelistin muokkaus graafisesti
- [ ] Tilin luonti ja tilauksen hallinta (ilmainen vs. 5€/kk)
- [ ] Sertifikaattien tila per laite

---

## v1.0 — Julkaisuvalmis
*Tavoite: Ensimmäinen oikea käyttäjä voi ottaa käyttöön.*

- [ ] Kaikki v0.x-toiminnallisuudet vakaat
- [ ] Play Store -julkaisu (tai sivulatauspaketti)
- [ ] Onboarding-flow: osta → luo tili → asenna laitteelle → valmis
- [ ] MPL 2.0 -velvoitteet täytetty (muutettu Focus-koodi julkaistu)
- [ ] Tietosuojaseloste ja käyttöehdot
- [ ] PRH: Kotisatama-toiminimi tai aputoiminimi rekisteröity

---

## Myöhemmin — ei aikataulussa vielä

- Hakumainonta (yksi mainostaja/hakusana/paikkakunta)
- WireGuard VPN -integraatio (näkymätön käyttäjälle)
- iOS-versio
- Lapsiversio erillisellä brändillä (päätös auki)
- Servo-moottoriin migraatio kun 1.0 saavutettu

---

*Projekti on osa Ilio-toiminimeä. Lisenssi: MPL 2.0 (Focus-fork) + MIT (Meilisearch).*
