# Bilgi Notu — Yayınlar ve Uzaktan Yönetim

Bu repo **yalnızca** dağıtılan programın exe dosyalarını ve uzaktan yönetim
dosyasını barındırır. Programın kaynak kodu ayrı (gizli) bir repodadır.

- **İndirme:** [Releases → en son sürüm](https://github.com/bekircanguler/pdfcreator-releases/releases/latest)
- **Uzaktan yönetim:** [`update_manifest.json`](update_manifest.json)

Dağıtılmış tüm uygulamalar (1.0.12 ve sonrası) açılışta ve açık kaldıkları
sürece 15 dakikada bir bu repodaki `update_manifest.json` dosyasını okur. Dosyayı
GitHub'da açıp kalem simgesiyle düzenlemek ve kaydetmek (Commit changes)
yeterlidir — derleme veya yeni sürüm gerekmez.

## Sık işlemler

**Uygulamayı kapatmak**

```json
"disabled": true,
"message": "Bu uygulama kullanımdan kaldırılmıştır."
```

**Yeniden açmak:** `"disabled": false`. Kapatılmış bir bilgisayar internete
bağlandığı ilk açılışta yeniden açılır.

**Tek modülü kapatmak**

```json
"disabled_modules": ["merge"],
"modules_message": "PDF Birleştir geçici olarak kullanıma kapatılmıştır."
```

Modül anahtarları: `note` (Bilgi Notu), `photo` (Fotoğraf → PDF), `merge` (PDF Birleştir).

**Eski sürümleri zorla güncellemek:** `"min_supported_version": "1.0.12"` —
altındaki sürümler kullanılamaz, "Şimdi Güncelle" ile yeni sürüme geçilir.

## Alanlar

| Alan | Örnek | Etkisi |
|---|---|---|
| `disabled` | `true` | Uygulama tamamen kapanır: `message` gösterilir, "Kapat"tan başka seçenek yoktur. |
| `message` | `"Bu uygulama kullanımdan kaldırılmıştır."` | Kapatma / zorunlu güncelleme penceresindeki metin. |
| `disabled_modules` | `["merge", "photo"]` | Listelenen modüller kilitlenir. |
| `modules_message` | `"Bakım nedeniyle geçici olarak kapalıdır."` | Kilitli modül kartında ve uyarıda gösterilen metin. |
| `lease_days` | `7` | Kullanım izninin geçerlilik süresi (gün, 1–365). |
| `min_supported_version` | `"1.0.12"` | Bu sürümün altındaki uygulamalar kullanılamaz. |
| `latest_version`, `exe_url`, `sha256`, `download_url` | — | Her yeni sürümde otomatik yazılır; **elle değiştirmeyin.** |

JSON'da virgül ve tırnaklara dikkat edin: dosya bozulursa uygulamalar onu
okuyamaz ve kapatma/güncelleme bilgisi bir sonraki düzgün kayda kadar
güncellenmez (çalışan uygulamalar son geçerli izinle devam eder).

## Süreli kullanım izni

Her başarılı okumada bilgisayara imzalı bir kullanım izni yazılır
(`%LOCALAPPDATA%\PDFMotor\lease.json`). İzin `lease_days` gün (varsayılan 7)
geçerlidir ve kapatma alanlarını da taşır; bu sürede internete hiç bağlanmayan
bilgisayar "Doğrulama Gerekli" penceresiyle kilitlenir, bağlandığında birkaç
saniye içinde kendiliğinden açılır. İzin dosyasını silmek, değiştirmek, başka
bilgisayara kopyalamak ya da saati geri almak programı açmaz.

## Yeni sürüm

Yeni sürümler kaynak kod reposunda `vX.Y.Z` etiketiyle yayınlanır; GitHub
Actions exe'yi derleyip bu repoya Release olarak koyar ve bu dosyadaki sürüm
alanlarını günceller. Bu repoda elle bir şey yapmak gerekmez.
