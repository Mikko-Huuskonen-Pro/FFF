# Kotisatama (FFF Fork) - v0.1

> **Tavoite**: Testata idean toimivuus — whitelist-pohjainen, yksityisyyttä korostava selainratkaisu, joka rajoittaa verkkosivujen lataamista ennalta määriteltyyn luotettujen sivustojen listaan.

---

## 📌 Yleiskuvaus

Kotisatama on **Firefox Focusin (FFF) forkki**, joka muokataan **whitelist-pohjaiseksi selaimeksi**. Tässä **v0.1-versiossa** testataan idean perustoiminnallisuutta:

- **Whitelist-rajoitus**: Vain ennalta hyväksytyt sivustot ladataan.
- **Yksinkertainen UI**: Perusselaintoiminnallisuus ilman turhia ominaisuuksia.
- **Testaus**: Varmistetaan, että whitelist-toiminnallisuus estää ei-sallittuja sivustoja.

---

## 🛠️ Teknologiat

- **Pohja**: [Firefox Focus (FFF)](https://github.com/mozilla-mobile/focus-android) (arkistoitu 17.6.2024)
- **Moottori**: GeckoView (Mozilla)
- **Kieli**: Kotlin (Android), Java
- **Tuleva integraatio**: Tauri 2.0 (Rust) + WebView

---

## 🚀 Asennus ja käynnistys

### Edellytykset
- Android Studio (uusin versio)
- Java JDK 17+
- Git

### Asennusohjeet
1. **Kloonaa repositorio**:
   ```bash
   git clone https://github.com/Mikko-Huuskonen-Pro/FFF.git
   cd FFF
   ```

2. **Avaa Android Studio**:
   - Avaa projekti Android Studion kautta.
   - Odota, että Gradle synkronoi riippuvuudet.

3. **Käynnistä emulaattorissa tai laitteella**:
   - Valitse `app` moduuli ja käynnistä sovellus.

---

## ⚙️ Whitelist-toiminnallisuus (v0.1)

### Tavoite
Rajoittaa selaimen lataamaan **vain whitelist-sivustot**. Muut sivustot estetään ja näyttävät virheviestin.

### Toteutus
Whitelist-tarkistus lisätään `android-components`-kirjastoon, erityisesti:
- **`browser-engine-gecko`**: Moottorin puoleinen rajoitus.
- **`browser-session`**: Sessioiden hallinta ja URL-tarkistus.

### Esimerkki: Whitelist-tarkistus
Whitelist-sivustot määritellään **`WhitelistManager`-luokassa** (uusi tiedosto):

```kotlin
// WhitelistManager.kt
class WhitelistManager {
    private val whitelist = setOf(
        "https://example.com",
        "https://kotisatama.fi",
        "https://wikipedia.org"
    )

    fun isAllowed(url: String): Boolean {
        return whitelist.any { allowed -> url.startsWith(allowed) }
    }
}
```

### Integrointi selaimeen
Whitelist-tarkistus lisätään **`WebRequestInterceptor`-luokkaan** (uusi tai muokattu):

```kotlin
// WebRequestInterceptor.kt
class WebRequestInterceptor(private val whitelistManager: WhitelistManager) : WebRequestInterceptor {
    override fun onLoadRequest(
        request: WebRequest,
        next: WebRequestInterceptor.Chain
    ): WebResponse? {
        if (!whitelistManager.isAllowed(request.uri)) {
            // Estä pyynnöt, jotka eivät ole whitelistillä
            return WebResponse.Builder()
                .status(403)
                .body("Sivusto ei ole sallittu Kotisatamassa.")
                .build()
        }
        return next.proceed(request)
    }
}
```

### Rekisteröi interceptor
Lisää interceptor **`BrowserEngine`-asetuksiin** (esim. `App.kt` tai `BrowserEngine.kt`):

```kotlin
// App.kt
val whitelistManager = WhitelistManager()
val interceptor = WebRequestInterceptor(whitelistManager)

val engine = GeckoWebEngine.Builder()
    .addWebRequestInterceptor(interceptor)
    .build()
```

---

## 📝 Testaus (v0.1)

### Testitapaukset
| Toiminto | Odotettu tulos |
|----------|-----------------|
| Avaa whitelist-sivusto (esim. `https://example.com`) | Sivu latautuu normaalisti |
| Avaa ei-whitelist-sivusto (esim. `https://google.com`) | Näytä virheviesti: "Sivusto ei ole sallittu Kotisatamassa." |
| Yritä ladata sivua, joka ei ole whitelistillä | Estetään ja näytetään 403-virhe |

### Manuaalinen testaus
1. Käynnistä sovellus emulaattorissa.
2. Yritä avata sivustoja whitelistiltä ja sen ulkopuolelta.
3. Varmista, että ei-whitelist-sivustot estetään.

---

## 🔜 Seuraavat vaiheet (v0.2+)

- [ ] **Whitelistin dynaaminen hallinta**: Mahdollisuus lisätä/poistaa sivustoja UI:sta.
- [ ] **Tauri 2.0 -integraatio**: Upota selain Tauri-sovellukseen Rustilla.
- [ ] **Meilisearch-hakukone**: Hakutoiminnallisuus, joka rajoittuu whitelist-sivustoihin.
- [ ] **Sertifikaattien hallinta**: Laitteiden tunnistus sertifikaateilla (Ilio-palvelin).
- [ ] **Offline-tuki**: Whitelistin ja sertifikaattien paikallinen tallennus (SQLite).

---

## 🤝 Osallistuminen

Jos haluat osallistua kehitykseen:
1. **Forkkaa repositorio**.
2. **Luo uusi branch** (`git checkout -b feature/whitelist`).
3. **Tee muutokset** ja lähetä **Pull Request**.

---

## 📜 Lisenssi

Projekti noudattaa **Mozilla Public License 2.0** -lisenssiä (perintönä FFF:ltä).

---

## 🔗 Linkit

- [Alkuperäinen FFF-repositorio (arkistoitu)](https://github.com/mozilla-mobile/focus-android)
- [Mozilla Central (gecko-dev)](https://github.com/mozilla/gecko-dev)
- [Tauri 2.0](https://tauri.app/v2/)
- [Meilisearch](https://www.meilisearch.com/)
