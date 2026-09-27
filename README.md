# Adisyon — Restoran Masa Sipariş Uygulaması

Basit, tek adres üzerinde çalışan, mobil uyumlu masa siparişi uygulaması.
Flask + SQLite ile yazıldı, ek bir veritabanı sunucusuna ihtiyaç duymaz.

## Kurulum

```bash
pip install -r requirements.txt
python app.py
```

Uygulama `http://<sunucu-ip>:5000` adresinde çalışır. Aynı ağdaki mobil
cihazlardan bu adrese tarayıcıyla girilerek kullanılabilir (garsonlar
telefonlarına kısayol/ana ekrana ekle yapabilir).

İlk çalıştırmada `adisyon.db` dosyası otomatik oluşturulur ve örnek
veriler eklenir:

- Yönetici hesabı: **admin / admin123** (ilk girişten sonra mutlaka
  değiştirin — şu an için şifre değiştirme ekranı yok, en hızlı yol
  Garsonlar sayfasından yeni bir yönetici hesabı açıp eskisini silmek,
  ya da doğrudan veritabanından güncellemek).
- 10 örnek masa (Masa 1..10)
- 2 örnek kategori ve birkaç örnek ürün

## Roller

- **Yönetici**: menü kategorileri/ürünleri ve fiyatları yönetir, masa
  sayısını değiştirir, garson/yönetici hesabı oluşturur veya siler,
  masa hesabını kapatır.
- **Garson**: sadece masalara girip sipariş ekleyebilir/kaldırabilir.
  Menü, masa ayarları ve kullanıcı yönetimine erişemez.

## Eş zamanlı kullanım

Bir masanın sipariş ekranı 3 saniyede bir kendini otomatik yeniler, bu
sayede aynı masaya birden fazla garson (veya garson + yönetici) aynı
anda girip sipariş ekleyebilir; her ekleme ayrı bir satır olarak
kaydedildiği için çakışma olmaz. Masa listesi ekranı da 4 saniyede bir
kendini yeniler.

## Üretim ortamı notu

`python app.py` geliştirme sunucusudur. Gerçek kullanımda (restoran
ortamında sürekli açık kalması için) `gunicorn` gibi bir WSGI sunucusu
ile çalıştırmanız önerilir, örnek:

```bash
pip install gunicorn
gunicorn -w 4 -b 0.0.0.0:5000 app:app
```

`-w 4` ile 4 worker process; masa/sipariş verisi SQLite + WAL modunda
tutulduğu için worker'lar arasında sorun yaşanmaz.

## Dosya yapısı

```
restoran_app/
  app.py                 -> tüm route'lar ve veritabanı işlemleri
  requirements.txt
  templates/             -> Jinja2 şablonları
  static/style.css        -> tüm arayüz stilleri
  adisyon.db              -> otomatik oluşturulan SQLite veritabanı
```
