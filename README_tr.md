# KristalENGINE 2

=[EN](./README.md)=

**KristalENGINE**, **KristalBASIC** programlama diliyle calisan bagimsiz bir entegre geliştirme ortamıdır. KristalBASIC; C# ve Swift'in okunakliligini C'nin ham performansiyla birlestiren modern, statik tipli bir dildir. Program mantiginizi KristalBASIC ile yazin, IDE projenizi bagimsiz bir Windows calistirilabilir dosyasina (`.exe`) derlesin — harici runtime yok, bagimlilk yok.

---

## Ozellikler

- **KristalBASIC 2.2 Dili** — C# ve Swift'ten ilham alan temiz, ifade gucu yuksek sozdizimi. Statik tipleme, siniflar, fonksiyonlar ve oyun odakli yerlesik API'ler hazir olarak gelir.
- **Tamamen Bagimsiz Derleyici Cekirdegi** — Derleyici (Lexer, Parser, CodeGen), oyun motoru bagimlilarindan tamamen ayrildi. KristalBASIC artik genel amacli, moduler ve bagimsiz bir programlama dilidir.
- **@OBJECT ile Moduler Standart Kutuphane** — Harici `.kbas` dosyalari `@OBJECT` direktifi araciligiyla derleme surecine entegre edilir. Raylib baglamalari, Win32 API ve diger moduller resmi standart kutuphane dosyalari olarak dagitilir (ornegin `lib/raylib.kbas`, `lib/win32/win32api.kbas`).
- **Dogrudan C Entegrasyonu** — Izole `external C` ve `external H` bloklari ile uclu tirnak isareti (`"""`) kullanilarak cok satirli ham C/C++ kaynak ve baslik kodlari dogrudan `.kbas` dosyalarinin icine yazilabilir.
- **Tam 2D ve 3D Destegi** — Tepeden bakis RPG'lerinden ve yandan kaydirmali platformer oyunlarindan tam anlamiyla gerceklestirilmis 3D oyunlara kadar her seyi insaa edin.
- **Gorsel Roman / Hikaye Modu** — Metin tabanli ve anlati odakli oyunlar icin birinci sinif destek.
- **Fizik Motoru** — Gercekci hareket ve carpisme icin yerlesik fizik simulasyonu.
- **Ses Sistemi** — Entegre ses ve muzik oynatma API'si.
- **Native Windows API (Win32)** — `lib/win32/win32api.kbas` araciligiyla, harici hicbir DLL gerektirmeden Windows isletim sistemine tam erisim. Desteklenen moduller: etkilesimli mesaj kutulari, sistem/RAM/CPU/monitor bilgisi, konsol okuma/yazma, dosya ve dizin islemleri, Windows Registry okuma/yazma, pano yonetimi, WAV ses oynatma, process ve mutex yonetimi ile gelismis pencere efektleri (kenarluksiz tam ekran, saydam katmanli pencere).
- **Entegre IDE** — Sozdizimi vurgulama, proje yonetimi ve canli derleme ozellikleriyle amaca ozel gelistirilmis bir gelistirme ortami.
- **Native Derleme** — Projeniz dogrudan bagimsiz bir `.exe` dosyasina derlenir. Oyunculardan herhangi bir sey yuklemelerini istemeden oyununuzu dagitabilirsiniz.

---

## Dil Ozellikleri (2.2)

### Yeni Ilkel ve Tip Destegi

- **`const` anahtar kelimesi** — Derleme zamani sabitleri tanimlayin. Sabit atamalar dogal olarak C karsiligina aktarilir.
- **Onaltilik (Hexadecimal) literaller** — Renk ve bayt degerlerini hex gosteriminde yazin (ornegin `0xFFFFFF`). C kutuphaneleriyle olusabilecek makro isim cakismalari izole derleme ortamlariyla onlenir.
- **`void` tipi** — Geriye deger dondurmeyen fonksiyonlar artik `void` ile tanimlanabilir. Parser ve kod uretici `void` tipini taniyor ve dogru C ciktisini uretiyor.

### Nesne Yonelimli Programlama

- **Statik siniflar** — `static class` bildirimleri global bir C struct ornegine derlenir; boylece `PlayerState.pitch = ...` seklindeki erisimler dogrudan bu global nesne uzerinden isler.
- **`new` anahtar kelimesi** — `kbc_alloc` uzerinden dinamik bellek tahsisi, nesne ornekleme icin dogru sekilde uretilir.
- **`this` anahtar kelimesi** — Metotlarin icinde `this`, fonksiyona gizlice gecirilen `THIS` isaretcisini dogru sekilde referans gosterir.

### Bellek Yonetimi

Scope Manager (Kapsam Yoneticisi) koklunden iyilestirildi. Dongu ve kosul bloklari (`while`, `if`) icinde statik olarak tahsis edilmis C dizgileri artik `kbc_scope_pop` sirasinda yanlis sekilde serbest birakilmiyor; bu sayede olumcul Heap Corruption hatalari (0xC0000374) engelleniyor. Bellek yoneticisi artik `kbc_alloc` ile uretilen isaretcileri harici Win32 API cagrilarindan gelenlere karsi guvenli bicimde ayirt edebiliyor. Harici cagrilardan gelen dizgiler `kbc_str_dup` entegrasyonu sayesinde bellek sizintisi olmadan otomatik olarak temizleniyor.

### Dizgi Karsilastirma

`STR_EQ`, kosul ifadelerinde guvenli dizgi karsilastirmasi (ornegin `if (STR_EQ(secim, "1"))`) icin C'nin `strcmp` fonksiyonuna kopru kuran yeni bir yerlesik fonksiyondur. Isaretci belirsizliklerini ortadan kaldirir.

### AST Duzeyinde Parametre Guvenligi

Sifir parametre bekleyen bir fonksiyona bir arguman gonderilmesi gibi hatali kullanim noktalarindan kaynaklanan hatalar artik C derleyicisine birakilmak yerine `kbc.exe` tarafindan AST asamasinda yakalanip kullaniciya anlamli bir hata mesaji olarak iletiliyor.

### IDE / G/C Pipe Uyumlulugu

Bir uygulama KristalENGINE IDE icerisinden baslatildiginda standart G/C kanallari pipe hatlarına yonlendirilir. `WriteConsole` ve `ReadConsole` artik bu durumu algilayip otomatik olarak `WriteFile` ve `ReadFile` yedek fonksiyonlarina dusuyor; sonsuz dongu olusumu engelleniyor ve ciktilar IDE log panelinde duzgun goruntuleniyor. Terminal gerektiren uygulamalar icin IDE baglantisini zorla koparip bagimsiz, temiz bir konsol penceresi acan `CON_OPEN` fonksiyonu da kullanima sunuldu.

---

## Ne Insaa Edebilirsiniz?

| Tur | Destekleniyor |
|---|---|
| 2D Platformer / Sprite tabanli | Evet |
| Tepeden bakis / RPG | Evet |
| 3D Oyunlar | Evet |
| Gorsel Roman / Metin Macerasi | Evet |

---

## Dil Sozdizimi

KristalBASIC 2.2 modern, statik tipli bir sozdizimi sunmaktadir. Asagida gercek dunya ornegi olarak 3D birinci sahis oyuncu giris sistemi yer almaktadir:

```kristal
@OBJECT PlayerState.kbas

static class InputSystem {
    func int PlayerInput(PlayerState playerState) {
        decimal dt = FRAME_TIME()
        decimal mdx = MOUSE_DELTA_X() * -0.15
        decimal mdy = MOUSE_DELTA_Y() * 0.15

        playerState.yaw = playerState.yaw + mdx
        playerState.pitch = playerState.pitch - mdy

        if (playerState.pitch > 89.0) { playerState.pitch = 89.0 }
        if (playerState.pitch < -89.0) { playerState.pitch = -89.0 }

        playerState.dx = SIN(RAD(playerState.yaw)) * COS(RAD(playerState.pitch))
        playerState.dy = SIN(RAD(playerState.pitch))
        playerState.dz = COS(RAD(playerState.yaw)) * COS(RAD(playerState.pitch))

        decimal rx = SIN(RAD(playerState.yaw - 90.0))
        decimal rz = COS(RAD(playerState.yaw - 90.0))

        decimal moveSpeed = playerState.speed * dt

        if (KEY_DOWN(87) == true) {
            playerState.x = playerState.x - (SIN(RAD(playerState.yaw)) * moveSpeed * -1)
            playerState.z = playerState.z - (COS(RAD(playerState.yaw)) * moveSpeed * -1)
        }
        if (KEY_DOWN(83) == true) {
            playerState.x = playerState.x + (SIN(RAD(playerState.yaw)) * moveSpeed * -1)
            playerState.z = playerState.z + (COS(RAD(playerState.yaw)) * moveSpeed * -1)
        }
        if (KEY_DOWN(65) == true) {
            playerState.x = playerState.x - (rx * moveSpeed)
            playerState.z = playerState.z - (rz * moveSpeed)
        }
        if (KEY_DOWN(68) == true) {
            playerState.x = playerState.x + (rx * moveSpeed)
            playerState.z = playerState.z + (rz * moveSpeed)
        }
        return 0
    }
}
```

Bu ornekte one cikan dil ozellikleri: statik siniflar, tipli fonksiyon imzalari (`func int`), yerlesik motor fonksiyonlari (`FRAME_TIME()`, `KEY_DOWN()`, `SIN()`, `RAD()`) ve nesne ozelligi erisimi.

---

## Baslangic

1. [GitHub Releases](../../releases) sayfasindan en son surumu **indirin**.
2. Yukleyiciyi calistirin ve **KristalENGINE IDE**'yi baslatın.
3. Yeni bir proje olusturun, ilk `.kbas` dosyanizi yazin ve `.exe` olarak derlemek icin **Build** dugmesine basin.

> **Gereksinimler:** Windows 10 veya uzeri.

---

## Dagitim

KristalENGINE **ucretsiz kullanima** aciktir. Projenizin urettigi `.exe` size aittir; istediginiz sekilde dagitabilirsiniz.

> KristalENGINE kapali kaynak kodlu bir yazilimdir. Derleyici, IDE ve runtime ic yapisi ozguludur.

---

## Yol Haritasi

Ozellik oneriniz mi var ya da bir hata mi buldunuz? Bir [sorun bildirimi](../../issues) acin ve bize bildirin.

---

## Lisans

KristalENGINE ucretsiz indirilebilir ve kullanilabilir. Tam kosullar icin [LICENSE](./LICENSE.txt) dosyasina bakin.
