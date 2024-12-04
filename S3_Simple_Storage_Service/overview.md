# Amazon S3 Detaylı Rehber

## **Giriş**

Amazon Simple Storage Service (S3), AWS tarafından sağlanan ölçeklenebilir ve yüksek performanslı bir nesne depolama hizmetidir. Veriler S3 üzerinde "bucket" adı verilen depolama konteynerlerinde saklanır ve dünya çapında milyonlarca müşteriye hizmet verir.

---

## **S3 Depolama Sınıfları**

### **1. S3 Standard (STANDARD)**

- **Amaç**: Sık erişim gerektiren veriler için uygundur.
- **Erişim Süresi**: Milisaniye düzeyinde.
- **Dayanıklılık**: 11 9'lar (99.999999999%).
- **Kullanım Örnekleri**: Web siteleri, mobil uygulamalar, veri analitiği, işlem kayıtları.
- **Maliyet**: En yüksek maliyetli depolama sınıfıdır.
- **Erişim Ücreti**: Yok.

---

### **2. S3 Intelligent-Tiering**

- **Amaç**: Erişim düzeni bilinmeyen veya değişken olan veriler için uygundur.
- **Katmanlar**:
  - **Sık Erişim (Frequent Access)**: Verilere sık erişim durumunda kullanılır.
  - **Seyrek Erişim (Infrequent Access)**: Seyrek erişilen veriler için kullanılır.
  - **Glacier ve Glacier Deep Archive** katmanlarına otomatik geçiş yapılabilir.
- **Avantajı**: Yönetim maliyetlerini ve veri geçişlerini otomatikleştirir.
- **Erişim Ücreti**: Küçük bir yönetim ücreti alınır.

---

### **3. S3 Standard-IA (Infrequent Access)**

- **Amaç**: Seyrek erişim gerektiren, ancak gerektiğinde hızlı erişim gereken veriler.
- **Dayanıklılık**: 11 9'lar (99.999999999%).
- **Kullanım Örnekleri**: Yedekleme, olağanüstü durum kurtarma.
- **Maliyet**: Daha düşük depolama maliyeti; erişim başına ücret alınır.

---

### **4. S3 Glacier ve Glacier Deep Archive**

- **S3 Glacier**:
  - Uzun vadeli saklama ve seyrek erişim gerektiren veriler için.
  - **Erişim Süresi**: Dakikalar içinde (standart) veya milisaniyeler içinde (hızlı alım, ek ücretle).
  - **Maliyet**: Çok düşük.

- **S3 Glacier Deep Archive**:
  - Çok nadiren erişilen ve uzun süreli saklanacak veriler için.
  - **Erişim Süresi**: Saatler içinde.
  - **Maliyet**: En düşük depolama maliyeti.

---

### **5. S3 One Zone-IA**

- **Amaç**: Seyrek erişilen ancak yüksek dayanıklılık yerine düşük maliyetin tercih edildiği veriler.
- **Kullanım Örnekleri**: Tekrar oluşturulabilir veriler, yedekler.
- **Dayanıklılık**: Veriler yalnızca bir erişim bölgesinde saklanır.
- **Maliyet**: Standard-IA'dan daha ucuz.

---

## **S3 Temel Özellikleri**

### **1. Versiyonlama (Versioning)**

- **Amaç**: Nesnelerin önceki sürümlerini saklar, silinmiş nesneleri kurtarır.
- **Kullanım Alanları**:
  - Yanlışlıkla yapılan değişiklik veya silinmelere karşı koruma.
  - Yedekleme ve geri yükleme işlemleri.

---

### **2. Lifecycle Policies (Yaşam Döngüsü Politikaları)**

- **Amaç**: Depolama maliyetlerini optimize etmek için nesnelerin belirli bir süre sonra farklı bir depolama sınıfına taşınmasını veya silinmesini otomatikleştirir.
- **Kullanım Örnekleri**:
  - 30 gün boyunca Standard, ardından Glacier depolama sınıfına taşıma.
  - Yasal saklama süreleri dolduğunda otomatik olarak silme.

---

### **3. Çoğaltma (Replication)**

- **Amaç**: Verilerin farklı AWS bölgelerinde veya aynı bölgede yedeklenmesini sağlar.
- **Türler**:
  - **Cross-Region Replication (CRR)**: Farklı bölgeler arası çoğaltma.
  - **Same-Region Replication (SRR)**: Aynı bölge içinde çoğaltma.
- **Kullanım Örnekleri**:
  - İş sürekliliği ve felaket kurtarma.
  - Bölgesel erişim hızlandırma.

---

### **4. S3 Access Points (Erişim Noktaları)**

- **Amaç**: Tek bir bucket için birden fazla erişim noktası oluşturarak farklı uygulamalara özel erişim politikaları sağlar.
- **Avantajı**:
  - Erişim yönetimini basitleştirir.
  - Büyük veri işleme ve analitik uygulamaları için kullanışlıdır.

---

### **5. Sunucu Taraflı Şifreleme (Server-Side Encryption)**

- **Amaç**: Verilerin otomatik olarak şifrelenmesini sağlar.
- **Seçenekler**:
  - **SSE-S3**: AWS tarafından yönetilen şifreleme anahtarları kullanılır.
  - **SSE-KMS**: AWS Key Management Service (KMS) kullanılır.
  - **SSE-C**: Kullanıcı tarafından sağlanan anahtarlarla şifreleme yapılır.

---

### **6. S3 Select**

- **Amaç**: Büyük dosyaların tamamını indirmeden, belirli bir kısmını sorgulayarak veri işleme maliyetini azaltır.
- **Özellikler**:
  - Sorgular SQL ile yapılır.
  - JSON, CSV veya Parquet formatlarını destekler.
- **Kullanım Örnekleri**:
  - Veri analizi için sadece ihtiyaç duyulan kısımların indirilmesi.

---

## **Güvenlik ve Erişim Kontrolü**

### **1. IAM Policies**
- Bucket ve nesne erişimini AWS IAM politikalarıyla yönetebilirsiniz.

### **2. Bucket Policies**
- Belirli bir bucket için genel veya özel erişim kuralları tanımlanabilir.

### **3. ACLs (Access Control Lists)**
- Daha ince ayarlı erişim kontrolü sağlar, ancak genelde bucket politikaları tercih edilir.

---

## **S3 Performans İpuçları**

1. **Nesne Anahtar Adlandırması**:
   - Performansı optimize etmek için anahtar adlarında hash kullanın.
   - Çok fazla nesne içeren bir bucket'ta rastgele erişim modeli sunar.

2. **Çok Parçalı Yükleme (Multipart Uploads)**:
   - Büyük dosyaları parçalara bölerek yükleme performansını artırır.

3. **CloudFront Entegrasyonu**:
   - İçerik dağıtımını hızlandırmak için S3 ile CloudFront kullanabilirsiniz.

4. **S3 Transfer Acceleration**:
   - Verilerin S3'e daha hızlı yüklenmesini sağlar.

---

## **S3 Kullanım Senaryoları**

- **Web Uygulamaları**:
  - Statik içerik barındırma (HTML, CSS, JavaScript).
  - Yüksek erişim taleplerini karşılamak için CloudFront ile entegre çalışır.

- **Yedekleme ve Arşivleme**:
  - Yasal dokümanlar, günlük kayıtları, medya dosyaları.
  - Lifecycle politikalarıyla maliyet optimizasyonu.

- **Big Data Analitiği**:
  - Veri gölleri oluşturma (Data Lakes).
  - AWS Glue, Athena, ve EMR ile entegre veri işleme.

- **Medya Akışı ve Depolama**:
  - Video, ses ve görüntü barındırma.
  - Elastic Transcoder ile medya dosyalarını dönüştürme.

---

Amazon S3, ölçeklenebilirliği, dayanıklılığı ve geniş özellik yelpazesi ile birçok kullanım senaryosuna uygun bir depolama çözümüdür. Yukarıdaki bilgiler, S3 ile çalışırken rehber olarak kullanılabilir.
