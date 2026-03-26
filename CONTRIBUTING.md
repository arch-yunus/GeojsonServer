# 🤝 Katkıda Bulunma Rehberi (Contributing)

Öncelikle bu projeye zaman ayırmak istediğin için teşekkürler! GeojsonServer, topluluk desteği ile gelişen açık kaynaklı bir girişimdir.

## 🚀 Nasıl Katkıda Bulunabilirsin?

### 1. Hata Bildirimi (Bug Reports)
Eğer bir hata bulursan, lütfen GitHub üzerinden bir `Issue` aç. Bildirirken şunları eklemeyi unutma:
- Hangi Java sürümünü kullanıyorsun?
- Hata hangi işlem sırasında oluştu?
- Hata logları (Console output).

### 2. Özellik Önerileri (Feature Requests)
"Şu özellik de olsa harika olurdu!" dediğin her fikir bizim için değerlidir. Açık bir şekilde amacını anlatarak Issue açabilirsin.

### 3. Kod Katkısı (Pull Requests)
- Projeyi forkla.
- Yeni bir feature branch aç (`git checkout -b feature/YeniOzellik`).
- Değişikliklerini yap. **Not:** Mimariyi bozmamaya (Controller-Service-DAO) özen göster.
- Değişikliklerini commit yap (`git commit -m 'Yeni bir özellik eklendi'`).
- Branch'ini pushla (`git push origin feature/YeniOzellik`).
- Bir Pull Request aç.

## 📜 Kod Standartları

- Java 11 standartlarına uyulmalıdır.
- JDBC sorguları SQL enjeksiyonuna karşı korunmalıdır (Template kullanımı zorunludur).
- Yeni eklenen Controller uç noktaları mutlaka dokümantasyona (README) eklenmelidir.

## 🏗️ Mimari Kural

Bu proje, performansı korumak için **Heavy-ORM (Hibernate/JPA) kullanmaz.** Doğrudan JDBC ve Spatial SQL fonksiyonları ile çalışır. Katkı sağlarken bu prensibe sadık kalınmalıdır.

Teşekkürler,
**Bahattin Yunus Çetin**
