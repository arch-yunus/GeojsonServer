# 🌍 GeojsonServer: A-'dan Z-'ye Mekânsal Veri Sunucusu Rehberi

[![Spring Boot](https://img.shields.io/badge/Spring%20Boot-2.3.3-6DB33F?style=for-the-badge&logo=spring-boot&logoColor=white)](https://spring.io/projects/spring-boot)
[![PostGIS](https://img.shields.io/badge/PostGIS-Spatial-0064a5?style=for-the-badge&logo=postgresql&logoColor=white)](https://postgis.net/)
[![Java](https://img.shields.io/badge/Java-11-ED8B00?style=for-the-badge&logo=java&logoColor=white)](https://www.oracle.com/java/)

> **Not:** Bu doküman, mekânsal veri dünyasına yeni adım atanlar için "her şeyi en ince ayrıntısına kadar" açıklamak üzere hazırlanmıştır.

---

## 📖 1. Nedir Bu Proje? (En Temel Seviye)

Diyelim ki elinde bir harita var ve bu harita üzerinde binlerce bina, yol ve park yerini göstermek istiyorsun. Bu veriler normalde bir veritabanında (PostgreSQL) durur. Ancak tarayıcı (browser), veritabanındaki ham veriyi anlayamaz. 

**GeojsonServer** burada devreye girer:
1.  Veritabanına gider.
2.  Harita verilerini (koordinatları) alır.
3.  Harita kütüphanelerinin (Google Maps, Leaflet, Mapbox) anlayacağı **GeoJSON** veya **MVT** formatına çevirir.
4.  İnternet üzerinden bu veriyi yayınlar.

Özetle: **Veritabanı ile Harita arasındaki köprüdür.**

---

## 🏗️ 2. Proje Yapısı (Dosya Dosya İnceleme)

Proje, "Katmanlı Mimari" denilen profesyonel bir yapıyla kurulmuştur. Her dosyanın görevi bellidir:

### 🚪 Controller (Kapı) - `GeojsonController.java`
Dış dünyanın (tarayıcınızın) projeye girdiği kapıdır. 
- Eğer tarayıcı `/geojson/getPoints` adresine giderse, bu dosya isteği karşılar ve "Hey Service, bana noktaları getir!" der.

### 🍱 Service (Garson) - `GeojsonService.java`
İş mantığının döndüğü yerdir. Controller'dan gelen emirleri alır, gerekirse düzenler ve Database katmanına iletir. 

### 🍳 DAO (Aşçı) - `GeojsonDao.java`
Veritabanına giren tek yer burasıdır. SQL sorgularını burada yazarız.
- Örn: `SELECT * FROM egitim_nokta` sorgusunu burada çalıştırırız.

---

## 💾 3. Veritabanında Neler Var? (Spatial Data)

Projeyle birlikte gelen `.backup` dosyaları aslında senin "harita verilerin"dir.

1.  **egitim_nokta**: Şehirlerin, dükkanların veya ağaçların sadece X ve Y koordinatlarını (Enlem/Boylam) tutar.
2.  **egitim_cizgi**: Yollar, nehirler veya boru hatları gibi "başlangıcı ve sonu olan" çizgileri tutar.
3.  **egitim_poligon**: İl sınırları, binalar veya parklar gibi "kapalı alanları" tutar.
4.  **poi**: (Point of Interest) Önemli noktaları tutar.

---

## 🧠 4. Sihirli SQL Fonksiyonları

Bu projenin en "cool" tarafı, veriyi Java koduyla değil, direkt **veritabanı motoruyla** (PostGIS) çevirmesidir.

### `ST_AsGeoJSON(geom)`
Veritabanındaki karmaşık binary koordinat verisini alır ve şöyle bir yazıya çevirir:
`{"type": "Point", "coordinates": [32.85, 39.92]}`

### `ST_AsMVT`
Binlerce veriyi sıkıştırıp "Vector Tile" dediğimiz küçük paketler haline getirir. Google Maps'te haritayı kaydırdıkça parça parça yüklenen o kareler işte budur!

---

## 🚀 5. Adım Adım Kurulum (Hiç Bilmeyenler İçin)

### Adım 1: PostgreSQL ve PostGIS Kur
Bilgisayarına PostgreSQL kurduktan sonra, query ekranına şunu yazmalısın:
```sql
CREATE EXTENSION postgis;
```
Bu komut, veritabanına "harita okuma yeteneği" kazandırır.

### Adım 2: Verileri Yükle
Projedeki `nokta.backup`, `cizgi.backup` dosyalarını PostgreSQL'de `Restore` diyerek içeri aktar.

### Adım 3: Ayarları Yap
`src/main/resources/application.properties` dosyasına git:
- `url`: Veritabanı adın (Örn: `jdbc:postgresql://localhost:5432/haritadb`)
- `username`: Veritabanı kullanıcı adın (Genelde `postgres`)
- `password`: Şifren

### Adım 4: Çalıştır
Projeye sağ tıkla ve `Run` de. Eğer her şey tamamsa, tarayıcına şu adresi yaz:
`http://localhost:8080/geojson/getPoints`
Ekranda koca bir JSON listesi görüyorsan başardın demektir! 🎉

---

## 📡 6. API Uç Noktaları (Özet Liste)

-   📍 **Noktalar**: `GET /geojson/getPoints`
-   🛣️ **Yollar**: `GET /geojson/getLinestrings`
-   🧱 **Alanlar**: `GET /geojson/getPolygons`
-   🏙️ **Filtreleme**: `GET /geojson/getPointPolygons?city=Ankara`  
    *(Bu komut: Ankara poligonunun içine giren tüm noktaları bulur!)*
-   📦 **Hızlı Harita (MVT)**: `GET /mvt/{z}/{x}/{y}.pbf?layername=poi`

---

## ✒️ Yazar ve Destek

**Bahattin Yunus Çetin**  
*GIS Mimarı & Yazılım Mühendisi*

Bu proje, mekânsal veri bilimini herkes için kolaylaştırmak amacıyla geliştirilmiştir. Herhangi bir sorunda GitHub üzerinden issue açabilirsin.

---
> [!TIP]
> **Öneri:** Verileri haritada görmek için [geojson.io](http://geojson.io) sitesine `/getPoints`'ten gelen cevabı yapıştırabilirsin!
