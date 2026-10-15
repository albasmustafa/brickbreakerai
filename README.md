# Brick Breaker AI

Tek dosya, sıfır bağımlılık — tarayıcıda aç, oyna. İstersen kendi video ve müziğini arka plana koy.

## Çalıştırma

```
xdg-open /home/albas/brickbreakerai/index.html
```

## Kontroller

- **Fare / dokunma** veya **← → / A D**: raket hareketi
- **Boşluk / tıklama / dokunma**: topu fırlat; menü, duraklatma ve oyun sonu ekranlarında ilerlet
- **P / Esc**: duraklat (sekme değişince otomatik duraklar)
- **⚙️ Ayarlar** (ses çubuğunda): efekt ses seviyesi, ekran sarsıntısı (Açık/Azaltılmış/Kapalı — `prefers-reduced-motion` açıksa varsayılan Azaltılmış), zorluk (Kolay ×0.85 hız +5 can / Normal / Zor ×1.15 hız +2 can; yalnızca yeni oyunda uygulanır), HP noktacıkları (tuğlada kalan can noktası; WCAG göreli parlaklığına göre temaya kontrastlı renk). Tümü kalıcı.

## Mekanikler

- **Raket fiziği:** ortaya yakın vuruş topu dik yukarı, uçlara yakın vuruş 60°'ye kadar eğik gönderir. Her raket vuruşunda top %1.5 hızlanır (bölüm hızının 1.3 katına kadar), can/bölüm başına sıfırlanır.
- **Tünelleme yok:** fizik 120 Hz sabit adımla işler, hızlı top alt adımlara bölünür.
- **Tuğla türleri:** 1–3 vuruş (mavi/sarı/kırmızı, hasar aldıkça çatlaklar), **X** kırılmaz (gri, bölüm bitirmek için kırılması gerekmez), **E** patlayıcı (turuncu; kırılınca çevresindeki tuğlaları da patlatır, zincirlenir).
- **Combo:** rakete dokunmadan üst üste kırılan her tuğla çarpanı büyütür (x8) ve kırılma sesi perde perde incelir. Skor = 10 × dayanıklılık × çarpan.
- **Bölümler:** 5 harita `LEVELS` dizisinde (satır = sıra, karakter = tuğla türü). Sonrası döngü, bölüm başına top hızı +%7.
- **Güç kapsülleri:** **W** raket 10 sn genişler · **M** her toptan 2 kopya (en çok 6 top) · **S** 8 sn yavaşlatma. Tuğla kırılınca %12 ihtimalle düşer.
- **Efektler:** her isabette tuğla bir an parlar; kırılmada parçacık + dönen parçalar; kenar vuruşu/patlama/kayıp ekranı sarsar; top izi.
- **Rekor:** `localStorage` (`bb_highscore`), oyun sonunda "YENİ REKOR" işareti.

## Sesler

Web Audio ile sentezlenir; her yüzeyin kendi sesi vardır: yan duvar (tok), tavan (tink), tuğla kırma (combo ile incelen pop), dayanıklı tuğla (clack), kırılmaz tuğla (metalik clank), patlama (güm), kapsüller, kayıp, bölüm/oyun sonu melodileri.

- **Raket sesi** seçilebilir: 🎲 Rastgele, Pop, Bip, Lazer, Tık, Bas, Zil, Boing, Pluck, 8-bit — her vuruşta küçük rastgele ton farkıyla çalar, seçim `localStorage`'da.
- **🔊 Sesler açık/kapalı** düğmesi tüm oyun seslerini susturur (`bb_muted`), seçim kalıcıdır.

## Arka plan medyası

Oyunun altındaki **Arka plan** çubuğundan kendi dosyanı seç:

- **🎬 Video**: video oyunun arkasında oynar (tuval yarı saydam olur), sesi duyulur.
- **🎵 Müzik**: seçtiğin müzik döngüde çalar.
- **Ses sürgüsü** arka plan medyasının sesini ayarlar; **✕ Kapat** ikisini de kaldırır.

Tarayıcı sesi ancak ilk tıklama/tuştan sonra çalmaya izin verir; sayfaya bir kez tıklamak yeterli.

## Temalar ve efektler

**🎨 Tema düğmesi** (ses çubuğunda) veya **T** tuşuyla 8 tema arasında geçilir; seçim kalıcıdır: Gece, Gün Batımı, Orman, Neon, Gameboy, Buz, Şeker, Klasik. Her tema sayfa tahtasını, tuğla/raket/top renklerini, yıldız alanını ve tüm ekranları boyar.

Efektler: yıldız alanı, tuğlaların bölüm başında satır satır belirmesi, her isabette tuğla/raket parlaması, raket kıvılcımları, kırılen tuğladan yükselen "+puan" yazıları, parçacıklar, dönen parçalar, top izi, mikro ekran sarsıntısı.

## Yapı (`index.html`)

1. Sabitler ve yardımcılar
2. Ses motoru + raket sesi seçimi + sessize alma (`#soundbar`)
3. `LEVELS` — bölüm haritaları
4. Durum ve varlıklar (`balls[]`, `bricks`, `powerups`, `particles`, `fragments`)
5. Oyun akışı (`startGame`, `serveBall`, `launchBall`, `loseLife`, `gameOver`, `levelCleared`, `pauseGame`)
6. Fizik (`moveBall` — alt adımlı, hıza kilitli; `hitBrick`/`destroyBrick`/`explode`; `isLevelDone`)
7. Efektler (`spawnParticles`, `spawnFragments`, `makeCracks`, `addShake`)
8. `update(dt)` / `draw()` / girdi / arka plan medyası (`#mediabar`) / ana döngü
