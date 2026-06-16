# KristalENGINE 2

=[EN](./README.md)=

**KristalENGINE**, **KristalBASIC** programlama dili üzerine inşa edilmiş bağımsız bir oyun motorudur. KristalBASIC; C#'ın ve Swift'in okunabilirliğini C'nin ham performansıyla birleştiren modern, statik tipli bir dildir. Oyun mantığınızı KristalBASIC ile yazın, motor onu doğrudan çalışan bir Windows uygulamasına (`.exe`) derlesin — harici runtime yok, bağımlılık yok.

---

##  Özellikler

- **KristalBASIC 2.0 Dili** — C# ve Swift'ten ilham alan temiz, ifadeli sözdizimi. Statik tipleme, sınıflar, fonksiyonlar ve oyun odaklı yerleşik API'lar kutudan çıkar çıkmaz kullanıma hazır.
- **Tam 2D & 3D Desteği** — Tepeden bakış RPG'lerinden ve yan kaydırmalı platform oyunlarından tam kapsamlı 3D oyunlara kadar her şeyi yapın.
- **Görsel Roman / Hikaye Modu** — Metin tabanlı ve anlatı odaklı oyunlar için birinci sınıf destek.
- **Fizik Motoru** — Gerçekçi hareket ve çarpışma için yerleşik fizik simülasyonu.
- **Ses Sistemi** — Entegre ses ve müzik oynatma API'ı.
- **Entegre IDE** — Sözdizimi vurgulama, proje yönetimi ve canlı derleme içeren özel geliştirme ortamı. KristalBASIC 2.0'dan itibaren kullanılabilir.
- **Yerel Derleme** — Projeniz doğrudan bağımsız bir `.exe` olarak derlenir. Oyunculardan herhangi bir şey yüklemelerini istemeden oyununuzu yayına alın.

---

##  Ne İnşa Edebilirsiniz?

| Tür | Destek |
|---|---|
| 2D Platform / Sprite Tabanlı | ✅ |
| Tepeden Bakış / RPG | ✅ |
| 3D Oyunlar | ✅ |
| Görsel Roman / Metin Macerası | ✅ |

---

##  Dil Sözdizimi

KristalBASIC 2.0, modern ve statik tipli bir sözdizimine sahiptir. İşte gerçek bir örnek — 3D birinci şahıs oyuncu girdi sistemi:

```kristal
@OBJECT PlayerState.kbas

static class InputSystem {
    func int PlayerInput(PlayerState playerState) {
        decimal dt = FRAME_TIME();
        decimal mdx = MOUSE_DELTA_X() * -0.15;
        decimal mdy = MOUSE_DELTA_Y() * 0.15;

        playerState.yaw = playerState.yaw + mdx;
        playerState.pitch = playerState.pitch - mdy;

        if (playerState.pitch > 89.0) { playerState.pitch = 89.0; }
        if (playerState.pitch < -89.0) { playerState.pitch = -89.0; }

        playerState.dx = SIN(RAD(playerState.yaw)) * COS(RAD(playerState.pitch));
        playerState.dy = SIN(RAD(playerState.pitch));
        playerState.dz = COS(RAD(playerState.yaw)) * COS(RAD(playerState.pitch));

        decimal rx = SIN(RAD(playerState.yaw - 90.0));
        decimal rz = COS(RAD(playerState.yaw - 90.0));

        decimal moveSpeed = playerState.speed * dt;

        if (KEY_DOWN(87) == true) {
            playerState.x = playerState.x - (SIN(RAD(playerState.yaw)) * moveSpeed * -1);
            playerState.z = playerState.z - (COS(RAD(playerState.yaw)) * moveSpeed * -1);
        }
        if (KEY_DOWN(83) == true) {
            playerState.x = playerState.x + (SIN(RAD(playerState.yaw)) * moveSpeed * -1);
            playerState.z = playerState.z + (COS(RAD(playerState.yaw)) * moveSpeed * -1);
        }
        if (KEY_DOWN(65) == true) {
            playerState.x = playerState.x - (rx * moveSpeed);
            playerState.z = playerState.z - (rz * moveSpeed);
        }
        if (KEY_DOWN(68) == true) {
            playerState.x = playerState.x + (rx * moveSpeed);
            playerState.z = playerState.z + (rz * moveSpeed);
        }
        return 0;
    }
}
```

Burada görülen temel dil özellikleri: statik sınıflar, tipli fonksiyon imzaları (`func int`), yerleşik motor fonksiyonları (`FRAME_TIME()`, `KEY_DOWN()`, `SIN()`, `RAD()`) ve nesne özelliğine erişim.

---

##  Başlarken

1. Son sürümü [GitHub Releases](../../releases) sayfasından **indirin**.
2. Yükleyiciyi çalıştırın ve **KristalENGINE IDE**'yi başlatın.
3. Yeni bir proje oluşturun, ilk `.kbas` dosyanızı yazın ve `.exe` olarak derlemek için **Build**'e basın.

> **Sistem Gereksinimi:** Windows 10 veya üzeri.

---

##  Dağıtım

KristalENGINE **ücretsizdir**. Projenizin ürettiği `.exe` sizindir — istediğiniz gibi dağıtın.

> KristalENGINE kapalı kaynaklı bir yazılımdır. Derleyici, IDE ve runtime iç yapısı tescillidir.

---

##  Yol Haritası

Özellik isteğiniz mi var veya bir hata mı buldunuz? Bir [issue](../../issues) açın, bildirim yapın.

---

##  Lisans

KristalENGINE ücretsiz olarak indirilebilir ve kullanılabilir. Tam koşullar için [LICENSE](./LICENSE) dosyasına bakın.
