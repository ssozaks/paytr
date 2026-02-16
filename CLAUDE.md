# PayTR

> Bu dosya Claude Code tarafindan her oturumda otomatik okunur.

<!-- SCAFFOLD:identity:START -->
## Proje Kimligi
- **Proje:** PayTR
- **Slug:** paytr
- **Aciklama:** PayTR odeme sistemi PHP kutuphanesi. Symfony HTTP Foundation/Client tabanli, PSR-4 autoload uyumlu odeme entegrasyon paketi.
- **Dizin:** ~/projects/paytr/
<!-- SCAFFOLD:identity:END -->

<!-- SCAFFOLD:tech-stack:START -->
## Teknoloji Stack
- **Dil:** PHP >= 7.1.3
- **Framework:** Symfony HttpFoundation + HttpClient
- **Paket Yoneticisi:** Composer
- **Namespace:** Mews\PayTr
- **Lisans:** MIT
- **Proje Tipi:** PHP Library (Composer package)
<!-- SCAFFOLD:tech-stack:END -->

## Kodlama Standartlari
- PSR-4 autoloading
- PSR-12 kodlama standartlari
- camelCase metod isimleri, PascalCase class isimleri
- Type declarations kullanimi (PHP 7.1+)
- Git commit mesajlari Conventional Commits formatinda (feat/fix/refactor/docs)
- Kod ve yorumlar Ingilizce

## Guvenlik
- Hassas bilgileri ASLA koda yazma
- .env dosyalarini ASLA commit etme
- API key/merchant bilgilerini environment variable olarak sakla
- Odeme islemlerinde HMAC dogrulamasi zorunlu

## Bilinen Sorunlar (2026-02-14 Audit)

| # | Sorun | Konum | Oncelik | Durum |
|---|-------|-------|---------|-------|
| 1 | ~~Timing attack vulnerability~~ | `Payment.php:369` — `hash_equals()` zaten kullaniliyor (dogrulanma: 2026-02-16) | ~~ORTA~~ | COZULDU |
| 2 | **Test yok** | Tum proje | PHPUnit veya benzeri test framework eklenebilir | ACIK |
| 3 | **OpenSpec yok** | Proje koku | Stabil kutuphane icin opsiyonel | ACIK |

## Detayli Kurallar
@.claude/rules/coding-standards.md
@.claude/rules/security.md
@.claude/rules/language.md
@.claude/rules/turkish-characters.md
