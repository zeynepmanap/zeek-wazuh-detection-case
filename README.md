# zeek-wazuh-detection-case
Turning network logs into detections using Zeek and Wazuh


# Zeek & Wazuh – Network Loglarından Detection Üretimi

## Genel Bakış
Bu projede ağ trafiğinden elde edilen logların, güvenlik tespitlerine (detection) nasıl dönüştürülebileceği gösterilmektedir. Çalışmada Zeek kullanılarak ağ trafiği izlenmiş, üretilen loglar Wazuh Agent aracılığıyla SIEM ortamına aktarılmıştır.

Amaç yalnızca log toplamak değil, bu logların SOC (Security Operations Center) bakış açısıyla nasıl anlamlandırılabileceğini göstermektir.

---

## Mimari Yapı

Network Traffic
↓
Zeek (Docker)
↓
Zeek Logları (conn.log, dns.log)
↓
Wazuh Agent
↓
Detection & Alarm Üretimi (SIEM)


---

## Ortam Bilgileri
- İşletim Sistemi: Ubuntu 24.04 (ARM64)
- Zeek: Docker (zeek/zeek:8.0.4)
- SIEM Agent: Wazuh Agent
- Ağ Arayüzü: enp0s1

---

## Log Toplama
Zeek, Docker container içerisinde host ağını dinleyecek şekilde yapılandırılmıştır.

Toplanan temel loglar:
- **conn.log** – Ağ bağlantı kayıtları
- **dns.log** – DNS sorgu kayıtları

Bu loglar ağ üzerindeki aktivitelerin detaylı şekilde analiz edilmesini sağlar.

---

## Detection Use Case

### Şüpheli DNS Aktivitesi Tespiti

**Senaryo:**  
Kısa süre içerisinde aynı istemciden olağandışı sayıda DNS sorgusu yapılması, zararlı yazılım beaconing davranışına veya şüpheli dış iletişime işaret edebilir.

**Log Kaynağı:**  
- Zeek `dns.log`

**Detection Mantığı:**
- Aynı kaynak IP adresi
- Kısa zaman aralığı
- Normalin üzerinde DNS sorgu sayısı

Bu davranış Wazuh tarafından korelasyon kurallarına tabi tutulduğunda potansiyel bir güvenlik tehdidi olarak değerlendirilebilir.

---

## Sonuçlar
- Zeek ile ağ trafiği başarıyla izlenmiştir
- Üretilen loglar Wazuh Agent aracılığıyla SIEM ortamına aktarılmıştır
- Loglardan detection üretilebilecek bir güvenlik senaryosu kurgulanmıştır

Bu çalışma, ağ izleme (NDR) ile SIEM entegrasyonunun temel mantığını ortaya koymaktadır.

---

## Geliştirme Önerileri
- Özel Wazuh detection kurallarının yazılması
- Zabbix ile sistem ve servis izleme entegrasyonu
- Alarm ve logların dashboard üzerinde görselleştirilmesi

---

## Teknik Dokümantasyon
Kullanılan komutlar ve yapılandırma örnekleri `docs/commands-and-snippets.md` dosyasında yer almaktadır.

---

## Not
Bu proje eğitim ve laboratuvar ortamı için hazırlanmıştır.

## Teknik Dokümantasyon
Kullanılan komutlar ve yapılandırma örnekleri `docs/commands-and-snippets.md` dosyasında yer almaktadır.
