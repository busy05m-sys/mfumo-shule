# Kuweka Mfumo wa Shule LIVE (GitHub Pages + Firebase + Domain yako)

## HATUA 1 — Tengeneza mradi wa Firebase (bure)
1. Nenda https://console.firebase.google.com → **Add project** → ipe jina (mf. `mfumo-shule`) → unaweza kuzima Google Analytics → **Create project**.
2. Ndani ya mradi: **Build → Firestore Database → Create database**.
   - Chagua **Start in production mode**.
   - Chagua eneo (region) lililo karibu (mf. `europe-west1` au `asia-south1`) → **Enable**.
3. **Build → Authentication → Get started → Sign-in method → Anonymous → Enable** (hii inaruhusu mfumo kuunganisha na Firestore kwa usalama bila kuunda akaunti za Firebase kwa kila mtumiaji — mfumo una login yake mwenyewe tofauti).
4. **Firestore Database → Rules**, badilisha kuwa:
   ```
   rules_version = '2';
   service cloud.firestore {
     match /databases/{database}/documents {
       match /kv/{docId} {
         allow read, write: if request.auth != null;
       }
     }
   }
   ```
   Bofya **Publish**.
5. Bofya gia (⚙️) juu kushoto → **Project settings → General → Your apps → </> (Web)** → ipe jina → **Register app** (usihitaji "Firebase Hosting").
   Utapewa kitu kama:
   ```js
   const firebaseConfig = {
     apiKey: "AIza...",
     authDomain: "mfumo-shule.firebaseapp.com",
     projectId: "mfumo-shule",
     storageBucket: "mfumo-shule.appspot.com",
     messagingSenderId: "...",
     appId: "..."
   };
   ```
   **Nakili taarifa hizi zote sita.**

## HATUA 2 — Weka taarifa za Firebase kwenye faili
Fungua `index.html` kwa kihariri chochote cha maandishi, tafuta sehemu ya juu yenye:
```js
window.FIREBASE_CONFIG = {
  apiKey: "WEKA_API_KEY_HAPA",
  ...
};
```
Badilisha thamani zote sita na zile ulizopewa na Firebase (Hatua 1.5). Hifadhi faili.

> Taarifa hizi SI siri kali (ni salama zionekane kwenye kificho cha ukurasa) — usalama halisi unatoka kwenye "Rules" ulizoweka Hatua 1.4.

## HATUA 3 — Pakia kwenye GitHub Pages (bure)
1. Fungua https://github.com → tengeneza akaunti kama huna.
2. **New repository** → jina lolote (mf. `mfumo-shule`) → **Public** → Create repository.
3. Pakia (upload) faili `index.html` (lililo na taarifa zako za Firebase) kwenye repo hiyo — kwa "Add file → Upload files".
4. Nenda **Settings → Pages** (upande wa kushoto). Chini ya "Build and deployment", chagua **Deploy from a branch**, tawi (branch) `main`, folder `/ (root)` → **Save**.
5. Baada ya dakika chache, GitHub itaonyesha link kama `https://jina-lako.github.io/mfumo-shule/` — hii tayari ni tovuti yako "live".

## HATUA 4 — Unganisha domain yako mwenyewe
1. Kwenye repo ile ile, **Settings → Pages → Custom domain**, andika domain yako (mf. `shule.mfano.co.tz` au `www.mfano.co.tz`) → **Save**.
   (GitHub itatengeneza faili la `CNAME` kwenye repo yako moja kwa moja — nimekuandalia mfano wa faili hilo pia, `CNAME`, ukitaka kuliweka mwenyewe kabla.)
2. Nenda kwenye msimamizi wa domain yako (mahali ulipolisajili domain, mf. Truehost, GoDaddy, Namecheap, n.k) → **DNS settings**:
   - Kama unatumia **subdomain** (mf. `shule.mfano.co.tz`):
     - Ongeza rekodi ya **CNAME**: Jina=`shule`, Thamani=`jina-lako.github.io`
   - Kama unatumia **domain kuu** (apex, mf. `mfano.co.tz` bila "www"):
     - Ongeza rekodi nne za **A** zinazoelekeza kwenye:
       ```
       185.199.108.153
       185.199.109.153
       185.199.110.153
       185.199.111.153
       ```
3. Subiri dakika 10–60 (wakati mwingine hadi masaa machache) DNS ienee, kisha kwenye GitHub **Settings → Pages**, tiki **Enforce HTTPS** ukiona chaguo hilo limepatikana (huja baada ya DNS kuthibitika).

## Kumbuka
- Taasisi zote (shule) zitashiriki hifadhidata moja ya Firebase — utengano wao unafanywa na mfumo wenyewe (kila taasisi ina "funguo" zake za data), sawa na ilivyokuwa Claude.
- Free tier ya Firebase (Spark plan) inatosha kwa matumizi ya shule kadhaa/nyingi za kawaida bila malipo; ukizidi mipaka yake (mf. maandishi/masomo mengi sana kwa siku), Firebase itakuomba kupandisha hadi Blaze plan (bado ina kiwango cha bure ndani yake, unalipa tofauti tu ukizidi).
- Nywila za watumiaji bado zinahifadhiwa wazi (si zilizofichwa/encrypted) ndani ya Firestore — sawa na ilivyokuwa awali. Kwa matumizi makubwa ya kibiashara, ni vizuri baadaye kuboresha hili (hashing ya nywila).
- GitHub Pages ni "static hosting" — hakuna gharama, lakini pia hakuna kikomo cha "ukurasa mmoja" kinachozuia mfumo huu (ni faili moja tu la HTML).
