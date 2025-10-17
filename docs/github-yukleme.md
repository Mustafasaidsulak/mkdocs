# 🚀 MkDocs Site Yayınlama Rehberi (GitHub Pages)

Bu dökümantasyon, MkDocs tabanlı siteni GitHub Pages üzerinde yayınlamak için gereken adımları içerir. Ayrıca özel domain (örneğin `docs.mustafasaidsulak.com`) kullanımını da kapsar.

---

## 🧩 1. Gerekli Kurulumlar

Sistemde MkDocs ve Material temasının kurulu olduğundan emin ol:

```bash
pip install mkdocs
pip install mkdocs-material
```

Özel alan adı kullanıyorsan proje kök dizinine bir `CNAME` dosyası ekle:

```
docs.mustafasaidsulak.com
```

---

## ⚙️ 2. Yapılandırma (mkdocs.yml)

Proje kökünde `mkdocs.yml` adlı dosya bulunmalı. Örnek yapılandırma:

```yaml
site_name: Mustafa Said Sulak Docs
site_url: https://docs.mustafasaidsulak.com
theme:
  name: material
nav:
  - Ana Sayfa: index.md
  - Kurulumlar: kurulumlar.md
```

---

## 🧱 3. Siteyi Yerelde Test Et

Değişiklikleri yerelde görmek için aşağıdaki komutu çalıştır:

```bash
mkdocs serve
```

Ardından tarayıcıdan [http://127.0.0.1:8000](http://127.0.0.1:8000) adresine gir. Her değişiklik anında tarayıcıya yansır.

---

## 📦 4. Siteyi Derle (Statik Dosya Üretimi)

Aşağıdaki komutla siteni statik HTML dosyalarına dönüştür:

```bash
mkdocs build
```

Bu işlem sonunda `site/` klasörü oluşturulur. Tüm HTML, CSS ve JavaScript dosyaların burada yer alır.

---

## 🌍 5. GitHub Pages Üzerinde Yayınla

Siteni GitHub Pages’a göndermek için tek komut yeterlidir:

```bash
mkdocs gh-deploy --force
```

Bu komut:

* Siteni yeniden oluşturur
* `gh-pages` adlı branch’e yükler
* GitHub Pages üzerinde otomatik olarak yayınlar

---

## 🔗 6. GitHub Pages Ayarlarını Kontrol Et

GitHub’da:

1. Repository → **Settings** sekmesine git
2. **Pages** bölümünü aç
3. Aşağıdaki ayarların olduğundan emin ol:

   * **Source:** `gh-pages`
   * **Folder:** `/ (root)`

---

## 🧹 7. `site/` Klasörünü Git Takibinden Çıkar (Önerilir)

`site/` klasörü her deploy’da yeniden oluşturulduğu için git tarafından izlenmesine gerek yoktur. `.gitignore` dosyana şu satırı ekle:

```
site/
```

---

## 💡 8. Güncelleme Yapmak

Yeni bir sayfa eklediğinde veya içerik değiştirdiğinde sadece şu komutu çalıştır:

```bash
mkdocs gh-deploy --force
```

GitHub Pages ve özel domain (örneğin `docs.mustafasaidsulak.com`) otomatik olarak güncellenir. DNS veya Cloudflare önbellek nedeniyle bazen değişikliklerin görünmesi 1–5 dakika sürebilir.

---

## 🧠 Ek İpucu: Önbellek Sorunlarını Önleme

Tarayıcıların eski sürümü göstermemesi için Cloudflare ayarlarında şunu yapabilirsin:

* **Caching → Configuration → Browser Cache TTL:** `Respect Existing Headers`
* **Cache Rules:**

  ```
  URL: docs.mustafasaidsulak.com/*
  Setting: Cache Level = Bypass
  ```

Alternatif olarak JavaScript ve CSS dosyalarının sonuna sürüm parametresi ekleyebilirsin:

```yaml
extra_javascript:
  - js/main.js?v=1.0.3
extra_css:
  - css/style.css?v=1.0.3
```

---

✅ Artık MkDocs tabanlı siteni GitHub Pages üzerinde kolayca yayınlayabilir, her güncellemede sadece tek bir komutla yeni sürümü canlıya alabilirsin.
