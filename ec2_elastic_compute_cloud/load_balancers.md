# 7. Load Balancer (Yük Dengeleyiciler)

## Load Balancer Nedir?

Amazon **Elastic Load Balancer (ELB)**, gelen ağ trafiğini birden fazla Amazon EC2 instance'ına otomatik olarak dağıtarak uygulamalarınızın yüksek erişilebilirlik ve ölçeklenebilirlik sağlamasına yardımcı olur. Load Balancer'lar, uygulama performansını artırır ve tek bir hata noktası olmasını engeller.

## Load Balancer Türleri

AWS, farklı ihtiyaçlara yönelik üç ana Load Balancer türü sunar:

### 1. **Application Load Balancer (ALB)**

- **Katman**: OSI modelinin 7. katmanında (Uygulama Katmanı) çalışır.
- **Protokoller**: HTTP, HTTPS, WebSocket.
- **Özellikler**:
  - **Gelişmiş Yönlendirme**: URL, host veya sorgu parametrelerine göre yönlendirme yapabilir.
  - **WebSocket Desteği**: Gerçek zamanlı iletişim gerektiren uygulamalar için idealdir.
  - **HTTP/2 Desteği**: Daha verimli ve hızlı iletişim sağlar.
- **Kullanım Alanları**:
  - Mikro servis mimarileri.
  - Yüksek trafikli web siteleri ve API'ler.

### 2. **Network Load Balancer (NLB)**

- **Katman**: OSI modelinin 4. katmanında (Taşıma Katmanı) çalışır.
- **Protokoller**: TCP, UDP, TLS.
- **Özellikler**:
  - **Yüksek Performans**: Milyonlarca isteği saniyede işleyebilir.
  - **Düşük Gecikme Süresi**: Yüksek performanslı network uygulamaları için idealdir.
  - **Sabit IP**: Her AZ'de sabit bir IP adresi sağlar.
- **Kullanım Alanları**:
  - Gerçek zamanlı veri işleme.
  - Oyun sunucuları.
  - IoT uygulamaları.

### 3. **Gateway Load Balancer (GWLB)**

- **Amaç**: Üçüncü taraf sanal ağ cihazlarını entegre etmek için tasarlanmıştır.
- **Özellikler**:
  - **Ağ Paketlerinin Yönlendirilmesi**: Güvenlik duvarları, izleme ve denetim araçları gibi hizmetlerle çalışır.
  - **Şeffaf Ölçeklenebilirlik**: Ağ cihazlarını ölçeklendirmek ve yönetmek için kullanılır.
- **Kullanım Alanları**:
  - Ağ güvenliği ve izleme.
  - Üçüncü taraf ağ hizmetleri.

### 4. **Classic Load Balancer (CLB)**

- **Not**: AWS tarafından eski (deprecated) olarak kabul edilir ve yeni uygulamalar için önerilmez.
- **Katman**: Hem 4. hem de 7. katmanda çalışabilir.
- **Özellikler**:
  - **Temel Yük Dengeleme**: Basit yönlendirme ihtiyaçları için kullanılır.
- **Kullanım Alanları**:
  - Eski uygulamalar ve sistemler.

---

## Load Balancer Özellikleri

### Sağlık Kontrolleri (Health Checks)

- **Amaç**: Arka uçtaki EC2 instance'larının sağlığını düzenli olarak kontrol eder.
- **Nasıl Çalışır**: Belirli aralıklarla HTTP/S istekleri gönderir ve yanıtları izler.
- **Fayda**: Sağlıklı olmayan instance'lara trafik yönlendirilmez, böylece uygulama kesintileri önlenir.

### Oturum Yapışması (Session Stickiness)

- **Amaç**: Kullanıcı isteklerinin aynı arka uç instance'ına yönlendirilmesini sağlar.
- **Nasıl Çalışır**: Çerez tabanlı yapışma veya kaynak IP adresine göre.
- **Fayda**: Durum bilgisi olan uygulamalar için gereklidir.

### SSL/TLS Sonlandırma

- **Amaç**: SSL/TLS şifrelemesini Load Balancer üzerinde sonlandırarak arka uç sunucuların yükünü azaltır.
- **Fayda**: Sertifika yönetimini kolaylaştırır ve performansı artırır.

### Yönlendirme Kuralları

- **Application Load Balancer için**:
  - **Path-based Routing**: URL yoluna göre.
  - **Host-based Routing**: Alan adına göre.
  - **Query String ve Header Routing**: Sorgu parametreleri ve başlıklara göre.
- **Network Load Balancer için**:
  - **Port-based Routing**: Farklı portlara göre trafik yönlendirme.

---

## Load Balancer Kurulumu ve Yapılandırması

### Adım 1: Load Balancer Oluşturma

1. AWS Management Console'da EC2 hizmetine gidin.
2. Sol menüden **Load Balancers** seçeneğine tıklayın.
3. **Create Load Balancer** butonuna basın.
4. İhtiyacınıza uygun Load Balancer türünü seçin (ALB, NLB, GWLB).

### Adım 2: Temel Ayarlar

- **Name**: Load Balancer için bir isim belirleyin.
- **Scheme**:
  - **Internet-facing**: İnternet üzerinden erişilebilir.
  - **Internal**: VPC içinde özel erişim.

### Adım 3: Ağ ve Güvenlik Ayarları

- **VPC**: Load Balancer'ın çalışacağı VPC'yi seçin.
- **Subnets**: En az iki farklı Availability Zone seçin.
- **Security Groups** (ALB için):
  - İzin verilen trafiği belirleyin (ör. HTTP, HTTPS).

### Adım 4: Dinleyiciler ve Yönlendirme

- **Listeners**: Load Balancer'ın dinleyeceği protokol ve portları tanımlayın (ör. HTTP 80, HTTPS 443).
- **Target Groups**: Arka uç sunucularının gruplandırılması.
  - **Target Type**: Instance, IP veya Lambda fonksiyonu.
  - **Protocol** ve **Port**: Arka uç iletişim ayarları.
  - **Health Checks**: Sağlık kontrolü ayarları (path, aralık, zaman aşımı).

### Adım 5: Hedefleri (Targets) Ekleme

- Oluşturduğunuz Target Group'a EC2 instance'larını veya IP adreslerini ekleyin.

### Adım 6: Gözden Geçirme ve Oluşturma

- Ayarlarınızı kontrol edin ve **Create** butonuna basarak Load Balancer'ınızı oluşturun.

---

## Load Balancer Yönetimi

### Sağlık Kontrolleri İzleme

- **CloudWatch** metriklerini kullanarak instance'ların sağlık durumunu izleyin.
- **Alarm** ayarlayarak sorunları proaktif olarak yönetin.

### Loglama ve İzleme

- **Access Logs**: Gelen isteklerin ayrıntılı kayıtlarını tutar.
- **Request Tracing**: Sorun gidermek için bireysel istekleri izleyin.

### Güvenlik

- **SSL/TLS Sertifikaları**: AWS Certificate Manager (ACM) ile sertifikaları yönetin.
- **Güvenlik Grupları**: En az ayrıcalık ilkesiyle inbound ve outbound kurallarını belirleyin.

### Otomatik Ölçeklendirme ile Entegrasyon

- **Auto Scaling Groups** ile Load Balancer'ınızı entegre ederek, yük altında otomatik ölçeklendirme sağlayın.

---

## En İyi Uygulamalar (Best Practices)

1. **Çoklu AZ Kullanımı**: Load Balancer'ınızı birden fazla Availability Zone'da dağıtarak yüksek erişilebilirlik sağlayın.
2. **Güvenlik**: SSL/TLS kullanarak verilerinizi şifreleyin ve güvenlik gruplarını doğru yapılandırın.
3. **Monitörleme**: CloudWatch ile performans metriklerini ve günlükleri düzenli olarak izleyin.
4. **Düzenli Testler**: Sağlık kontrollerini ve yönlendirme kurallarını düzenli olarak test edin.
5. **Yedeklilik**: Olası bir arıza durumunda hızlı kurtarma için planlar yapın.

---

## Ücretlendirme

- **Load Balancer Saatlik Ücreti**: Kullanılan Load Balancer türüne ve bölgeye göre değişir.
- **Veri İşlem Ücreti**: Load Balancer üzerinden geçen veri miktarına bağlı olarak ücretlendirilir.
- **Ek Özellikler**: Örneğin, **AWS WAF** kullanımı ek maliyet getirebilir.

---

## Load Balancer ile İlgili Entegrasyonlar

- **Amazon EC2**: Arka uç sunucuları olarak EC2 instance'ları kullanılır.
- **Amazon ECS ve EKS**: Konteyner tabanlı uygulamalar için yük dengeleme.
- **AWS Auto Scaling**: Trafiğe göre otomatik ölçeklendirme sağlar.
- **AWS Certificate Manager (ACM)**: SSL/TLS sertifikalarının kolay yönetimi.
- **Amazon Route 53**: DNS yönlendirmesi ve coğrafi yük dengeleme.

---

## Sık Karşılaşılan Sorunlar ve Çözümleri

### Sorun: Load Balancer Trafiği Yönlendirmiyor

- **Kontrol Edilecekler**:
  - Security Group ayarları (hem Load Balancer hem de EC2 instance'ları için).
  - Sağlık kontrollerinin başarılı olup olmadığı.
  - Hedef gruplarının doğru yapılandırılması.

### Sorun: Yüksek Gecikme Süreleri

- **Olası Nedenler**:
  - Arka uç instance'larının performans sorunları.
  - Aşırı yük altında kalan kaynaklar.
- **Çözümler**:
  - Auto Scaling ile daha fazla instance eklemek.
  - Instance türlerini gözden geçirmek.


---

## AWS CLI ile Load Balancer Yönetimi
Load Balancer'ları AWS CLI kullanarak kolayca yönetebilirsiniz.

### Load Balancer Oluşturma (ALB Örneği):
```bash
aws elbv2 create-load-balancer --name my-alb --subnets subnet-abc123 subnet-def456 --security-groups sg-123abc
```

### Hedef Grupları Ekleme:
```bash
aws elbv2 create-target-group --name my-target-group --protocol HTTP --port 80 --vpc-id vpc-123abc
```

### Sağlık Kontrolü Yapılandırması:
```bash
aws elbv2 modify-target-group --target-group-arn <target-group-arn> --health-check-path /health
```

---

## Load Balancer Ücretlendirmesi
- **Saatlik Ücret**: Kullanılan Load Balancer türüne göre değişir.
- **Veri Transferi Ücreti**: Load Balancer üzerinden geçen veri için ücretlendirilir.

---

## Best Practices
1. **Sağlık Kontrolü**:
   - Sağlık kontrolü yapılandırmasını doğru bir şekilde yapın.
2. **Hedef Grupları**:
   - Trafiği yönlendirmek için hedef gruplarını organize edin.
3. **HTTPS Kullanımı**:
   - Web uygulamalarında güvenli bağlantılar için HTTPS yapılandırmasını kullanın.
4. **CloudWatch İzleme**:
   - CloudWatch kullanarak Load Balancer performansını ve kullanımını izleyin.

---

Load Balancer, yüksek erişilebilirlik ve performans sağlamak için kritik bir bileşendir. AWS'nin sunduğu farklı Load Balancer türleri, farklı iş yüklerine göre esneklik ve ölçeklenebilirlik sunar.