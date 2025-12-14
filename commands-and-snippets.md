# Commands & Configuration Snippets

Bu dosya, case study kapsamında kullanılan **tüm komutları** ve **örnek yapılandırmaları** içermektedir. README.md dosyası kavramsal olarak sade tutulmuş, teknik adımlar burada toplanmıştır.

---

## 1. Docker Kurulumu ve Kontrol

```bash
sudo apt update
sudo apt install -y docker.io
sudo systemctl enable docker
sudo systemctl start docker
sudo usermod -aG docker $USER
newgrp docker
```

Docker sürüm kontrolü:

```bash
docker --version
```

---

## 2. Zeek Docker Image İndirme

```bash
docker pull zeek/zeek
```

Zeek sürüm kontrolü:

```bash
docker run --rm zeek/zeek zeek --version
```

---

## 3. Zeek Log Dizini Oluşturma

```bash
mkdir -p ~/zeek-logs
```

---

## 4. Zeek’i Docker Üzerinden Çalıştırma

Host ağ arayüzü üzerinden trafik dinlemek için:

```bash
sudo docker run --rm -it \
  --net=host \
  -v ~/zeek-logs:/logs \
  zeek/zeek \
  zeek -i enp0s1 Log::default_logdir=/logs
```

> Not: `enp0s1` sistemde aktif olan ağ arayüzüdür. Arayüz adı `ip a` komutu ile kontrol edilmiştir.

---

## 5. Zeek Tarafından Üretilen Loglar

Zeek çalıştıktan sonra aşağıdaki loglar oluşturulur:

```text
~/zeek-logs/
 ├── conn.log
 ├── dns.log
 ├── files.log
 └── notice.log
```

---

## 6. Wazuh Agent Servis Kontrolü

Wazuh Agent durum kontrolü:

```bash
systemctl status wazuh-agent
```

Servisi başlatma / yeniden başlatma:

```bash
sudo systemctl restart wazuh-agent
```

---

## 7. Zeek Loglarının Wazuh Tarafından İzlenmesi (Konsept)

Wazuh Agent, Zeek log dizinini izleyecek şekilde yapılandırılabilir:

```xml
<localfile>
  <log_format>json</log_format>
  <location>/home/zeynep/zeek-logs/*.log</location>
</localfile>
```

> Bu yapılandırma örnek amaçlıdır ve logların SIEM'e aktarılabileceğini göstermek için eklenmiştir.

---

## 8. Örnek (Konsept) Wazuh Detection Rule

```xml
<rule id="100001" level="7">
  <if_matched_sid>zeek_dns</if_matched_sid>
  <description>Şüpheli DNS beaconing aktivitesi tespit edildi</description>
</rule>
```

> Not: Bu rule örneği çalıştırılmak zorunda değildir, detection mantığını göstermek amacıyla eklenmiştir.

---

## 9. Detection Mantığı (Özet)

* Aynı kaynak IP adresinden
* Kısa zaman aralığında
* Olağandışı sayıda DNS sorgusu

Bu davranış, olası malware beaconing göstergesi olarak değerlendirilebilir.

---

## Not

Bu doküman eğitim ve laboratuvar ortamı için hazırlanmıştır.
