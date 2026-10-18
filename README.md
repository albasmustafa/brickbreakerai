# Brick Breaker AI

Tek dosya, sıfır bağımlılık — tarayıcıda aç, oyna. İstersen kendi video ve müziğini arka plana koy.

## Çalıştırma

```
xdg-open /run/media/albas/albas/brickbreakerai/index.html
```

Bu dosya, modüllere ayrılmış proje sürümünden derlenir: `/run/media/albas/albas/brickbreaker-web` (kaynak kod, testler, uygulama olarak kurulabilen sürüm). Ayrıntılar en altta.

## Kontroller

- **Fare / dokunma** veya **← → / A D**: raket hareketi
- **Boşluk / tıklama / dokunma**: topu fırlat (YAPIŞ kapsülünde yakalanan topu bırak); menü, duraklatma ve oyun sonu ekranlarında ilerlet
- **P / Esc**: duraklat (sekme değişince otomatik duraklar); bir panel açıksa **Esc** paneli kapatır
- **T**: sıradaki tema · **R**: sonsuz rastgele tema · **F**: tam ekran (🖥️ düğmesi de)
- **Araç çubuğu** (tuvalin altında): 🔊 sesler · 🎨 tema · 🎲 rastgele tema · 🖌️ tema stüdyosu · ⚔ rekabet · 🏅 kariyer · ⚙️ ayarlar · 🖥️ tam ekran. Paneller ekranın ortasında açılır, aynı anda tek panel açık kalır; ✕, Esc ya da dışarı tıklamak kapatır, panel açıkken oyun arkada duraklar.
- **⚙️ Ayarlar**: efekt ses seviyesi, raket sesi, ekran sarsıntısı (Açık/Azaltılmış/Kapalı — `prefers-reduced-motion` açıksa varsayılan Azaltılmış), zorluk (Kolay ×0.85 hız +5 can / Normal / Zor ×1.15 hız +2 can; yalnızca yeni oyunda uygulanır), HP noktacıkları (tuğlada kalan can noktası; WCAG göreli parlaklığına göre temaya kontrastlı renk). Tümü kalıcı.

## Mekanikler

- **Raket fiziği:** ortaya yakın vuruş topu dik yukarı, uçlara yakın vuruş 60°'ye kadar eğik gönderir. Her raket vuruşunda top %1.5 hızlanır (bölüm hızının 1.3 katına kadar), can/bölüm başına sıfırlanır.
- **Tünelleme yok:** fizik 120 Hz sabit adımla işler, hızlı top alt adımlara bölünür.
- **Falso:** raket hareket ederken vurursan top raketin hızı kadar falso alır ve kavis çizerek uçar (topun üstünde dönen çizgi). Falsolu top duvara/tavana çarpınca sekme açısı falsoya göre sapar (aynı vuruş 25°–65° arası çıkabilir), falso tersine döner ve zayıflar.
- **SMASH:** raketin en ucuyla (%20) vuruş topa geçici hız patlaması verir.
- **Duvar bölgeleri:** yan duvarlarda parlayan şeritler, bölüm ilerledikçe çoğalır (bölüm numarasıyla tohumlanır, her oynayışta aynı): **tampon** (pembe, oklu: pinball gibi fırlatır, ×1.6 hız + rastgele sapma) · **falso** (yeşil: topa sert kavis verir) · **portal** (mor, 3. bölümden itibaren: soldan giren top sağ duvardaki eşinden aynı yönde çıkar).
- **Yatay sekme koruması:** top hiçbir zaman yataya 16°'den fazla yaklaşmaz, duvarlar arasında sonsuza dek sekmez.
- **Tuğla türleri:** 1–3 vuruş (mavi/sarı/kırmızı, hasar aldıkça çatlaklar), **X** kırılmaz (gri, bölüm bitirmek için kırılması gerekmez), **E** patlayıcı (turuncu; kırılınca çevresindeki tuğlaları da patlatır, zincirlenir), **Z** elektrik (mor, 2 vuruş; her vuruşta en yakın 3 tuğlaya yıldırım, yıldırım yiyen elektrikli tuğla bir kez daha zincirler), **$** altın (2 vuruş; ×10 puan, altın yağmuru, kesin iyi kapsül), **+** çoğaltan (yeşil; kırılınca aşağı doğru yeni top). Satırın 11. karakteri `>` ise o sıra sağa sola kayar.
- **Combo:** rakete dokunmadan üst üste kırılan her tuğla çarpanı büyütür (x8) ve kırılma sesi perde perde incelir. Skor = 10 × dayanıklılık × çarpan. 5/8/15/20/30 combo'da ekranda dev yazı çıkar.
- **ÇILGIN MOD:** 10 combo'da 6 sn boyunca tüm toplar ateş topu olur, puanlar ×2, tahta gökkuşağına döner.
- **Bölümler:** 11 harita `LEVELS` dizisinde (satır = sıra, karakter = tuğla türü); 5. ve 10. bölüm boss, 11. bölüm *Siber Labirent*. Sonrası döngü, bölüm başına top hızı +%7, her turda boss canı +%50.
- **Boss:** *Tuğla Kral* (40 can) ve *Demir İmparator* (70 can). Sağa sola gezer, gözleriyle topu izler, rakete nişanlı mermi atar (değerse raket 2.5 sn sersemler ve küçülür). Canı yarıya inince öfkelenir: hızlanır, üçlü yelpaze atar, altına yeni tuğlalar diker. Her 6 isabette iyi kapsül düşürür. Ölünce ağır çekim patlama + 5000 puan.
- **Güç kapsülleri** (tuğla kırılınca %16 ihtimal): **GENİŞ** raket 10 sn · **ÇOKLU** her toptan 2 kopya (en çok 8 top) · **YAVAŞ** 8 sn · **ATEŞ** 6 sn top kırılabilir tuğlaları sekmeden deler · **LAZER** 8 sn raketten otomatik lazer (boss mermilerini de vurur) · **YAPIŞ** 10 sn top rakete yapışır, tıkla bırak (2.5 sn sonra kendisi kalkar) · **DEV** 10 sn kocaman top, 2 hasar · **KALKAN** alt kenarda 3 kurtarış · **DELİK** 7 sn topları büken kara delik; çok yaklaşan topu fırlatır, sonunda çöküp çevresindeki tuğlaları yutar · **+CAN** (en çok 9) · **KAOS?** rastgele herhangi biri.
- **Kötü kapsüller** (kırmızı, kesikli çerçeve): **TERS** 6 sn kontroller ters · **MİNİ** 8 sn küçük raket · **HIZ** 6 sn top ×1.3 · **LANET** hemen bir lanet çağırır. Yakalamak cesaret ister: +250 puan.
- **Ağır çekim:** son tuğla kırılınca ve boss ölünce zaman yavaşlar, ekran hafif yakınlaşır; büyük patlamalarda an donması.
- **Efektler:** her isabette tuğla bir an parlar; kırılmada parçacık + dönen parçalar; kenar vuruşu/patlama/kayıp ekranı sarsar; top izi.
- **Rekor:** `localStorage` (`bb_highscore`), oyun sonunda "YENİ REKOR" işareti.

## Lanetler (engeller)

Oyun sırasında rastgele aralıklarla bir **lanet** gelir: ekranın ortasında 1.2 sn önceden "⚠ AD" diye duyurulur, sonra başlar; HUD'da kalan süresiyle mor etiket olarak durur. Lanet aktifken kırılan her tuğla **×1.5 puan** verir. Oyun sonunda **atlatılan lanet** sayısı gösterilir. Can kaybında ve bölüm geçişinde tüm lanetler silinir, her can/bölüm başında kısa bir nefes payı vardır; bölüm ilerledikçe sıklaşırlar.

**⚙️ Ayarlar → Lanetler**: **Kapalı** · **Az** (~26 sn'de bir, aynı anda 1) · **Normal** (~16 sn, aynı anda 2) · **Kâbus** (~8 sn, aynı anda 3, süreler ×1.25, grup kısıtı yok). Kâbus dışında aynı gruptan (görüş / kamera / fizik / raket) aynı anda tek lanet olur.

| Lanet | Süre | Etkisi |
|---|---|---|
| Görünmez raket | 8 sn | Raket yalnızca vuruşta ve 1.6 sn'de bir anlık belirir |
| Hayalet top | 7 sn | Top rakete yaklaşana kadar yalnızca ara ara görünür |
| Karanlık | 9 sn | Ekran kararır, yalnızca topların ve raketin çevresi aydınlık |
| Elektrik kesintisi | 8 sn | Ekran düzenli aralıklarla tamamen kararır |
| Sis | 10 sn | Sahanın ortasını kalın bir sis bandı kaplar |
| Ayna dünya | 8 sn | Görüntü yatay aynalanır (kontroller değişmez!) |
| Sarhoş kamera | 8 sn | Ekran sallanır, döner, yakınlaşıp uzaklaşır |
| Rüzgar | 9 sn | Top sürekli bir yana itilir |
| Ağır yerçekimi | 8 sn | Yukarı giden top yavaşlar, düşen hızlanır, yön aşağı bükülür |
| Nabız | 8 sn | Top hızı dalga dalga artıp azalır |
| Buzlu raket | 9 sn | Raket kayar: hedefi geçer, geç durur |
| Yarık raket | 8 sn | Raketin ortasında boşluk; top oradan düşer |
| Sahte toplar | 8 sn | Gerçeğinden ayırt edilemeyen 3 top: raketten geçer, tuğla kırmaz |
| Meteor yağmuru | 8 sn | Yukarıdan meteorlar düşer; rakete değerse sersemletir (lazer vurur) |
| Daralan duvarlar | 10 sn | Yan duvarlar 110 px içeri kapanır |
| Gezgin bariyerler | 12 sn | Sahada kayan iki kırılmaz bariyer |
| Tavan çöküyor | anlık | Tüm tuğlalar bir sıra aşağı iner |
| Diriliş | anlık | Kırılmış 3 tuğla geri gelir |
| Zırhlanma | anlık | Tuğlalar +1 dayanıklılık kazanır (en çok 3) |
| UFO baskını | anlık | Geçen UFO kötü kapsül bırakır; 2 vuruşta düşer, +750 ve iyi kapsül |

## Rekabet (⚔ Rekabet paneli)

**Oyuncu adı** liderlik tablosunda ve maçlarda görünür.

**Dereceli lig:** panelin en üstünde rütbe kartın durur: lig, puan (**LP**), sonraki lige kalan puan, en yüksek puanın (zirve) ve dereceli galibiyetlerin. **🏅 Dereceli maç** 3 bot ve 3 dakikalık bir kapışma başlatır. Botların puanı seninkine yakındır (biri ~110 LP zayıf, biri denk, biri ~110 LP güçlü), güçleri de puanlarından türetilir: 900 LP = Çaylak, 1250 = Usta, 1600 = Efsane, arası yumuşak geçiş, 2000'e kadar daha da güçlenir. Kazandıkça rakipler zorlaşır, kaybettikçe kolaylaşır.

- **Puan:** çok oyunculu Elo. Her rakiple ikili karşılaştırma yapılır: önünde bitirdiğin rakip kazandırır, arkanda kaldığın kaybettirir; güçlü rakibi geçmek çok, zayıfı geçmek az kazandırır. Denk 4 kişilik maçta 1. ≈ +20, 4. ≈ −20 LP.
- **Yerleştirme:** ilk 5 dereceli maçta puan iki kat hızlı oynar (K = 64, sonra 40). Herkes 1000 LP (Bronz) başlar.
- **Ligler:** 🥉 Bronz (<1100) · 🥈 Gümüş (1100) · 🥇 Altın (1250) · 💠 Platin (1400) · 💎 Elmas (1550) · 👑 Usta (1700) · 🔥 Efsane (1850). Lig atlayınca ya da düşünce ekranda duyurulur.
- **Terk:** bitmemiş dereceli maçı bırakmak (yeni maç başlatmak, odadan ayrılmak, sayfayı kapatıp açmak) son sıra sayılır. Oda sahibiyle bağlantı koparsa maç puansız biter.
- Dereceli maçta rakip kartlarında ve sonuç ekranında herkesin ligi ve puanı, sonuç ekranında da kazandığın/kaybettiğin puan görünür. Menüde rekorunun yanında ligin yazar.
- **Botlarla kapışma** bölümündeki ayarlı maçlar dostluk maçıdır: puanı değiştirmez, istatistiklere ve başarımlara sayılır.

**Kapışma kuralları:** herkes aynı anda, 3 canla ve normal hızla başlar (3-2-1 geri sayım). Maç 2 / 3 / 5 dakika sürer; süre bitince en yüksek skor, ya da son ayakta kalan kazanır (elenenler elenme sırasına göre dizilir). Tuğla kırdıkça **saldırı enerjisi** dolar (tuğla +1, 5+ combo'da +2, bölüm +6, boss +12, ÇILGIN MOD +5); 10'a ulaşınca seçtiğin hedefe (**Lider / Rastgele / Son sıradaki**) otomatik bir **lanet** gider. Gelen saldırı ekranda "⚔ Ad: LANET" diye duyurulur; **KALKAN** hakkın varsa saldırıyı engeller, aynı anda 3 lanet varsa saldırı boşa gider. Kapışmada rastgele lanetler kapalıdır, lanetler rakiplerden gelir. Tuvalin yanında canlı rakip kartları (sıra, skor, can, bölüm ilerlemesi, lanetli durumu, saldırı akışı), HUD'da kalan süre, sıran ve saldırı enerjisi çubuğu görünür.

**Botlarla (dostluk):** 1–7 bot; seviye **Çaylak** (~700 puan/dk, 3 dakikayı ~%26 ihtimalle bitirir) · **Usta** (~2000/dk) · **Efsane** (~4250/dk, zor) · **Karışık**. Botlar fiziksiz ama istatistiksel olarak gerçekçi oynar: tuğla kırar, combo yapar (raket vuruşunda sıfırlanır gibi), bölüm atlar, can kaybeder, birbirlerine ve sana saldırır; lanetlendiklerinde 8 sn yavaşlar ve daha çok can kaybeder. Bot maçında duraklatınca maç saati de durur.

**Arkadaşlarla (çevrim içi, sunucusuz):** oyunlar WebRTC veri kanalıyla doğrudan bağlanır.
1. Oda sahibi **🏠 Oda kur** → **➕ Davet kodu oluştur** (her arkadaş için ayrı kod, ~650 karakter, sıkıştırılmış) → kodu mesajla gönderir.
2. Arkadaş **🔗 Odaya katıl** → davet kodunu yapıştırır → **Cevap kodu oluştur** → cevap kodunu geri gönderir.
3. Oda sahibi cevap kodunu yapıştırıp **Bağla**'ya basar; lobide isimler görünür. İstenirse **Ek bot** ekleyip **⚔ Maçı başlat**.

Oda sahibi **Dereceli: Açık** seçerse (varsayılan) maç herkes için derecelidir: her oyuncu bağlanırken kendi puanını getirir (lobide ligiyle görünür), maç sonunda herkes kendi puan değişimini aynı formülle hesaplar. Ek botların puanı seviyelerinden gelir (Çaylak 900, Usta 1250, Efsane 1600).

En çok 4 insan (oda sahibi merkezdedir, botları da o çalıştırır). Skorlar saniyede 4 kez eşitlenir; çevrim içi maçta duraklatma yoktur; elenen oyuncu maç bitene kadar izler; bağlantısı kopan oyuncu elenmiş sayılır. Bağlantı kurulurken adres öğrenmek için Google'ın genel STUN sunucusu (`stun.l.google.com`) kullanılır, oyun verisi oradan geçmez. Çok sıkı ağlarda (simetrik NAT) doğrudan bağlantı kurulamayabilir.

**Liderlik tablosu:** bu cihazda, **Klasik** ve **Kapışma** için ayrı ilk 10 (ad, skor, bölüm, kapışma sırası, tarih; `bb_leaderboard`). Oyun sonu ekranında tablodaki yerin gösterilir.

## Kariyer (🏅 Kariyer paneli)

Araç çubuğundaki **🏅 Kariyer** (ya da Rekabet panelindeki **📊 Kariyer ve başarımlar**) açar. Hepsi bu cihazda saklanır (`bb_career`); **Kariyeri sıfırla** (iki tıkla onaylı) puanı, sayaçları, geçmişi ve başarımları siler.

- **Rütbe kartı:** lig, LP, sonraki lige ilerleme çubuğu, yerleştirme durumu, zirve puan.
- **18 sayaç:** klasik oyun, kırılan tuğla, en iyi combo, en uzak bölüm, yenilen boss, ÇILGIN MOD, atlatılan lanet, yakalanan kapsül, oynama süresi, kapışma, galibiyet (yüzdesiyle), podyum (ilk 3), en uzun galibiyet serisi (şu anki seriyle), dereceli maç ve galibiyet, çevrim içi maç, gönderilen saldırı, kalkanla engellenen saldırı. Sayaçlar oyun sırasında canlı artar; klasik oyunlar da kapışmalar da sayılır.
- **Son maçlar:** son 20 kapışma: tarih, mod (🏅 dereceli / 🤖 botlar / 🌐 çevrim içi), sıra (🏆 kazandın, 🏳 yarıda kaldı), skor, LP değişimi.
- **Başarımlar:** 25 başarım. Açıldığı anda tuvalin üstünde bildirim çıkar (tam ekranda da), kısa bir melodi çalar; birden çok başarım açılırsa bildirimler sırayla gelir. Kilitli başarımlar panelde soluk görünür, açılanın üstüne gelince açıldığı tarih yazar.

| | Başarım | Nasıl açılır |
|---|---|---|
| 🧱 | İlk Tuğla | İlk tuğlanı kır |
| 🏗️ | Yıkım Ekibi | Toplam 1.000 tuğla kır |
| 🔗 | Zincirleme | 20 combo yap |
| ★ | Çılgın | ÇILGIN MOD'a gir |
| 👹 | Boss Avcısı | Bir boss yen |
| ⚙️ | İmparator Düştü | Demir İmparator'u yenip 11. bölüme ulaş |
| 🌀 | Labirentin Sonu | 11. bölümü de bitir |
| 💯 | 50.000 Kulübü | Tek oyunda 50.000 puan topla |
| 🔮 | Lanet Avcısı | Toplam 50 lanet atlat |
| 💊 | Kapsül Bağımlısı | Toplam 100 kapsül yakala |
| ⚔ | İlk Kapışma | Bir kapışmayı bitir |
| 🏆 | İlk Zafer | Bir kapışmayı kazan |
| 📈 | Seri Galip | 3 kapışmayı üst üste kazan |
| 🚀 | Durdurulamaz | 5 kapışmayı üst üste kazan |
| 🎪 | Kalabalığın Kralı | 8 kişilik bir maçı kazan |
| 🤖 | Makine Avcısı | Efsane güçte (1600+ LP) bot olan bir maçı kazan |
| ✨ | Kusursuz | Hiç can kaybetmeden bir kapışmayı kazan |
| 💓 | Son Nefes | Tek canın kalmışken bir kapışmayı kazan |
| 🌐 | Sokakların Kralı | Arkadaşlarınla oynanan bir maçı kazan |
| 💣 | Sabotajcı | Toplam 50 saldırı gönder |
| 🛡 | Kalkan Duvarı | 10 saldırıyı kalkanla engelle |
| 🎖 | Yerleşti | 5 yerleştirme maçını tamamla |
| 🥇 | Altın Lig | Altın lige yüksel |
| 💎 | Elmas Lig | Elmas lige yüksel |
| 🔥 | Efsane Lig | Efsane lige yüksel |

## Sesler

Web Audio ile sentezlenir; her yüzeyin kendi sesi vardır: yan duvar (tok), tavan (tink), tuğla kırma (combo ile incelen pop), dayanıklı tuğla (clack), kırılmaz tuğla (metalik clank), patlama (güm), kapsüller, kayıp, bölüm/oyun sonu melodileri. Kaos sürümüyle: tampon (boing), falso (vızıltı), portal, SMASH, yıldırım, lazer, altın, kötü kapsül, kalkan, kara delik uğultusu ve fırlatma, ÇILGIN MOD arpeji, boss kükremesi/isabet/mermi, sersemleme. Rekabetle: geri sayım, saldırı, başarım melodisi.

- **Raket sesi** (⚙️ Ayarlar → Raket sesi): 🎲 Rastgele, Pop, Bip, Lazer, Tık, Bas, Zil, Boing, Pluck, 8-bit — seçince önizleme çalar; oyunda her vuruşta küçük rastgele ton farkıyla çalar.
- **🔊 Sesler açık/kapalı** düğmesi tüm oyun seslerini susturur (`bb_muted`), seçim kalıcıdır.

## Arka plan medyası

Oyunun altındaki **Arka plan** çubuğundan kendi dosyanı seç:

- **🎬 Video**: video oyunun arkasında oynar (tuval yarı saydam olur), sesi duyulur.
- **🎵 Müzik**: seçtiğin müzik döngüde çalar.
- **Ses sürgüsü** arka plan medyasının sesini ayarlar; **✕ Kapat** ikisini de kaldırır.

Tarayıcı sesi ancak ilk tıklama/tuştan sonra çalmaya izin verir; sayfaya bir kez tıklamak yeterli.

## Müzikle senkron efektler

Arka plana müzik (ya da sesli video) koyunca ses gerçek zamanlı analiz edilir (Web Audio frekans analizi): bas / orta / tiz enerjisi, **vuruş** (bas kendi ortalamasının belirgin üstüne çıkınca), **tiz vuruş** (hi-hat) ve vuruş aralıklarının ortancasından **BPM**. Analiz ses seviyesinden bağımsızdır: müziği kısınca efektler zayıflamaz. Medya çubuğunda vuruşla atan bir nokta, şarkının profil adı ve BPM'i görünür.

**Efektler:** bas vuruşunda kamera tekmesi · şok dalgası halkaları · tuğla dansı (sütun sütun zıplama + basla nefes alma, yalnızca görüntü) · ekolayzer çubukları · spektrum halkası · vuruş flaşı · renk nabzı · yıldız hızlanması (yıldızlar çizgiye döner) · ışın şovu · hi-hat'lerle parıltı yağmuru · aurora · top ve raket neon haleleri. Duvar bölgeleri de vuruşla parlar.

**Her şarkıya özel:** dosya adı + boyutundan bir tohum çıkar (şarkının renk tonu ve bir imza efekti). İlk 8 saniye dinlendikten sonra şarkının karakteri belirlenir ve kendi efekt setini alır:

| Karakter | Ne zaman | Efektler |
|---|---|---|
| Bas Fırtınası | bas ağır | kamera tekmesi, şok dalgası, tuğla dansı, ekolayzer, renk nabzı |
| Hız Treni | hızlı (≥135 BPM) ve yüksek enerji | flaş, ışın şovu, yıldız hızı, kamera tekmesi, ekolayzer |
| Kristal Yağmur | tizler parlak | parıltı yağmuru, spektrum halkası, neon hale, tuğla dansı |
| Gece Sakinliği | sakin, yavaş | aurora, neon hale, renk nabzı, spektrum halkası |
| Neon Dalga | ortalar baskın | ışın şovu, şok dalgası, spektrum halkası, neon hale, tuğla dansı |

Her profile tohumdan bir ek efekt ve bir sıfat eklenir ("Antik Bas Fırtınası · 129 BPM" gibi), aynı karakterdeki şarkılar da birbirinden ayrışır. Profil kaydedilir (`bb_song_profiles`, en çok 60 şarkı); aynı şarkı tekrar açılınca anında gelir.

**⚙️ Ayarlar → Müzik:** Müzik efektleri **Kapalı / Hafif / Tam** (vuruş flaşı yalnızca Tam'da ve düşük yoğunlukta; ışığa duyarlıysan Hafif) · **Şarkıya özel tema** (açıkken her şarkı kendi tohumundan bir tema üretir).

## Temalar ve efektler

Her tema 13 renkten oluşur (sayfa, tahta, 3 tuğla, kırılmaz, patlayan, raket, top, yazı, soluk yazı, vurgu, arka plan efekti) ve tahtayı, tuğlaları, ekranları, hatta tuvalin altındaki çubuk etiketlerini boyar. Seçim kalıcıdır (`bb_theme_cur`).

- **25 hazır tema:** Gece, Gün Batımı, Orman, Neon, Gameboy, Buz, Şeker, Klasik, Lav, Okyanus, Synthwave, Matrix, Sakura, Çöl, Uzay, Kahve, Altın, Lavanta, Vampir, Kağıt (açık), Pastel (açık), Amber, Sonbahar, Aurora, Siberpunk. **🎨** düğmesi veya **T** ile sırayla geçilir (kayıtlı temalar da sıraya girer).
- **Sonsuz rastgele tema:** **🎲 Tema** düğmesi veya **R**. Her tema bir tohumdan üretilir (analog, üçlü, tamamlayıcı, bölünmüş, dörtlü veya tek renk şeması); yazı tahtaya karşı en az 7:1, tuğlalar en az 2.4:1 kontrastla okunaklı kalacak şekilde ayarlanır; %20'si açık zeminlidir. Her birine isim verilir ("Zümrüt Rüya #4242" gibi), aynı tohum hep aynı temayı üretir.
- **🖌️ Tema stüdyosu:** tüm hazır ve kayıtlı temaların renk önizlemeli galerisi, yenilenebilir 8 rastgele öneri, 13 rengin her biri için renk seçici (canlı önizleme), tema adı, **💾 Kaydet** / **🗑 Sil** (en çok 60 kayıtlı tema, `bb_custom_themes`), **📤 Kodu al** / **📥 Yükle** ile `BB1:...` paylaşım kodu. Hazır ya da rastgele bir temayı düzenlemek onu bozmaz, kaydedince senin temana dönüşür.
- **Otomatik tema** (Ayarlar → Görünüm): her bölümde yeni rastgele tema.

## Görünüm ayarları

**⚙️ Ayarlar → Görünüm** (hepsi kalıcı, "Görünümü varsayılana döndür" ile sıfırlanır):

- **Top şekli:** Daire, Kare, Elmas, Yıldız, Halka (şekil topla birlikte döner; çarpışma her zaman daire)
- **Top izi:** Normal, Uzun, Gökkuşağı, Kapalı
- **Raket:** Parlak (degrade, gölge, vurgu renginde uç kapakları), Köşeli, Neon, Gökkuşağı (akan renkler)
- **Tuğla stili:** Parlak (varsayılan: degrade gövde, cam parlaması, gölge), Düz, Yuvarlak, 3D (ışık/gölge kenarlı), Cam (yarı saydam), Neon (parlayan çerçeve)
- **Tuğla rengi:** Dayanıklılığa göre ya da Gökkuşağı (sıra sıra renk; dayanıklılık parlaklığa yansır)
- **Arka plan:** Yıldızlar, Kar, Kabarcık, Matrix yağmuru, Synthwave ızgara, Kapalı
- **Parçacık:** Az, Normal, Çok, Aşırı (×0.4 – ×4)

Efektler: yıldız alanı, tuğlaların bölüm başında satır satır belirmesi, her isabette tuğla/raket parlaması, raket kıvılcımları, kırılen tuğladan yükselen "+puan" yazıları, parçacıklar, dönen parçalar, top izi, mikro ekran sarsıntısı.

## Görsel cila

- **Tuğlalar** renk/stil/tür başına bir kez önceden çizilen sprite'lardır (hızlı ve keskin). Özel tuğlaların ikonları: kırılmaz = perçinli fırçalanmış metal, patlayan = fitili kıvılcım saçan bomba, elektrik = şimşek (üstünde ara ara kıvılcım yayı), altın = para + kayan parıltı, çoğaltan = ikiz top.
- **Tuğla animasyonları:** bölüm başında sıra sıra "pop" ile belirir, vuruşta esner, ara ara rastgele bir tuğlada parıltı çakar.
- **HUD:** skor/rekor, combo hapı, kalan süresini çubukla gösteren efekt hapları, bölüm rozeti (boss bölümünde degrade can çubuğu), çizilmiş kalpler (tek can kalınca nabız atar).
- **Top ve raket** zemine gölge düşürür; daire top parlak küre gibi çizilir. Kapsüller parlak hap görünümünde, hafifçe süzülür.
- **Ekranlar:** menü, duraklatma, bölüm sonu ve oyun sonu degrade başlıklı kartlarda; menüdeki gösterge gerçek tuğla görselleriyle çizilir. Skor yazıları kenarlıklı ve büyüyerek belirir; her bölüm "BÖLÜM N" duyurusuyla başlar.
- **HiDPI:** tuval ekranın piksel yoğunluğunda (en çok 2×) çizilir, yüksek çözünürlüklü ekranlarda bulanıklaşmaz.
- **Sayfa arayüzü** temaya uyar: düğmeler, paneller, kenarlıklar ve vurgu rengi temadan gelir. Tuval pencereye sığar (genişliği hem pencere genişliğine hem yüksekliğine göre ayarlanır), paneller ortada açılan pencerelerdir, tam ekran desteklenir.

## Kaynak kod

Bu `index.html`, proje sürümünün (`/run/media/albas/albas/brickbreaker-web`) okunaklı tek dosya derlemesidir; her bölümün başında hangi kaynak dosyadan geldiği yazar (`// js/physics.js` gibi). Değişiklikler projede yapılıp yeniden derlenir:

```
cd /run/media/albas/albas/brickbreaker-web
npm test                                                           # 17 özellik testi (başsız Chromium)
node tools/build.mjs --out /run/media/albas/albas/brickbreakerai/index.html
```

Modüller (sırayla): `core` (sabitler, renk/çizim/DOM yardımcıları) · `themes` · `settings` (ayar tablosu; ayarlar paneli bundan üretilir) · `audio` (ses motoru + ses tablosu) · `levels` · `state` · `flow` (oyun akışı) · `effects` · `physics` (kapsül tablosu, tuğlalar, duvarlar, top, lazer, kara delik, boss) · `curses` (lanetler) · `update` · `render` (sprite'lar, HUD, ekranlar) · `music` (medya + müzik senkronu) · `ui` (araç çubuğu, paneller, tema stüdyosu, girdi) · `career` (dereceli lig, sayaçlar, maç geçmişi, başarımlar) · `versus` (botlar, dereceli maç, WebRTC, liderlik tablosu) · `main` (ana döngü).
