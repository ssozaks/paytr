# PayTR

> Bu dosya Claude Code tarafından her oturumda otomatik okunur.

<!-- SCAFFOLD:identity:START -->
## Proje Kimliği
- **Proje:** PayTR
- **Slug:** paytr
- **Açıklama:** PayTR ödeme sistemi PHP kütüphanesi. Symfony HTTP Foundation/Client tabanlı, PSR-4 autoload uyumlu ödeme entegrasyon paketi.
- **Dizin:** ~/projects/paytr/
<!-- SCAFFOLD:identity:END -->

<!-- SCAFFOLD:tech-stack:START -->
## Teknoloji Stack
- **Dil:** PHP >= 7.1.3
- **Framework:** Symfony HttpFoundation + HttpClient
- **Paket Yöneticisi:** Composer
- **Namespace:** Mews\PayTr
- **Lisans:** MIT
- **Proje Tipi:** PHP Library (Composer package)
<!-- SCAFFOLD:tech-stack:END -->

## Kodlama Standartları
- PSR-4 autoloading
- PSR-12 kodlama standartları
- camelCase metod isimleri, PascalCase class isimleri
- Type declarations kullanımı (PHP 7.1+)
- Git commit mesajları Conventional Commits formatında (feat/fix/refactor/docs)
- Kod ve yorumlar İngilizce

## Güvenlik
- Hassas bilgileri ASLA koda yazma
- .env dosyalarını ASLA commit etme
- API key/merchant bilgilerini environment variable olarak sakla
- Ödeme işlemlerinde HMAC doğrulaması zorunlu

## Bilinen Sorunlar (2026-02-14 Audit)

| # | Sorun | Konum | Öncelik | Durum |
|---|-------|-------|---------|-------|
| 1 | ~~Timing attack vulnerability~~ | `Payment.php:369` — `hash_equals()` zaten kullanılıyor (doğrulanma: 2026-02-16) | ~~ORTA~~ | ÇÖZÜLDÜ |
| 2 | **Test yok** | Tüm proje | PHPUnit veya benzeri test framework eklenebilir | AÇIK |
| 3 | **OpenSpec yok** | Proje kökü | Stabil kütüphane için opsiyonel | AÇIK |

## Detaylı Kurallar
@.claude/rules/coding-standards.md
@.claude/rules/security.md
@.claude/rules/language.md
@.claude/rules/turkish-characters.md


## Plan Fidelity Rule — KIRMIZI ÇİZGİ
> Detay: `~/webreka-system/shared/rules/base/plan-fidelity.md`

**Onaylanan plan ne içeriyorsa BİREBİR uygulanır. İstisna YOKTUR.**
- Madde atlanmaz, kapsam daraltılmaz, "bu kritik değil" değerlendirmesi YAPILMAZ
- Plan → tasks: HER madde ayrı task satırı olur. Fidelity %100 ZORUNLU
- tasks.md sonrası plan ile cross-check ZORUNLU
- Her task GERÇEKTEN uygulanmadan [x] işaretlenmez
- Tüm maddeler [x] olmadan arşivleme YASAK

### KAPSAM
Bu kural TÜM görev türleri için geçerlidir:
- OpenSpec workflow'ları, direkt görevler, faz bazlı planlar, checklist'ler
- Hatta tek cümlelik görevler bile ("X, Y ve Z yap" = 3 ayrı iş)

### ZORUNLU CHECKLIST PROTOKOLÜ
Her görev/faz/adım grubu tamamlandığında tablo ZORUNLU:

| # | Görev (plandaki birebir metin) | Durum | Kanıt |
|---|---|---|---|
| 1 | [birebir metin] | durumu | [dosya path/komut çıktısı] |

### SAYISAL DOĞRULAMA
Başlarken: "Bu planda X faz, toplam Y madde var."
Bitirirken: "Yapılan: A/Y — Tamamlanma: %Z"

### YASAKLAR
- "Bu adımlar kozmetik, önce core işleri yaptım" → YASAK
- "Bunları sonraki iterasyonda yapabiliriz" → YASAK
- Planda N faz varken <N faz yapıp "tamamlandı" demek → YASAK
- Planı "özetleyip" daha az maddeye indirmek → YASAK
- "Daha az kritik" diye kendi kafasına göre triage yapmak → YASAK
