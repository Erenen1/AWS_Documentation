
## 2. Ödeme Yöntemleri

AWS EC2, kullanıcıların iş yükleri ve bütçelerine göre seçebileceği çeşitli ödeme yöntemleri sunar:

### 2.1. On-Demand (Talep Üzerine)
- Kullanıcılar yalnızca kullandıkları süre kadar ödeme yapar.
- Uzun vadeli taahhüt gerekmez.
- **Avantajlar**:
  - Trafik dalgalanmaları olan uygulamalar için idealdir.
  - Yeni projeler ve test ortamları için risk içermez.
- **Dezavantajlar**:
  - Sürekli kullanım durumunda daha pahalı olabilir.

### 2.2. Reserved Instances (Rezerve Edilmiş Instance)
- 1 veya 3 yıllık taahhüt karşılığında %75'e varan maliyet avantajı sağlar.
- **Kullanım Alanları**:
  - Sürekli çalışan uygulamalar (ör. web sunucuları, veritabanları).
- **Avantajlar**:
  - Daha düşük birim maliyet.
  - Kesin kaynak kapasitesi sağlanır.

### 2.3. Spot Instances
- AWS'nin boşta kalan kapasitelerini çok düşük maliyetle kullanmanızı sağlar.
- **Avantajlar**:
  - On-Demand fiyatlarına göre %90'a varan tasarruf.
- **Dezavantajlar**:
  - AWS tarafından ihtiyaç durumunda sonlandırılabilir.
- **Kullanım Alanları**:
  - Batch işlemleri.
  - Büyük veri analitiği.
  - Fault-tolerant uygulamalar.

### 2.4. Savings Plans (Tasarruf Planları)
- EC2 ve Fargate gibi hizmetlerde maliyet tasarrufu sağlar.
- **Avantajlar**:
  - Bölge, instance tipi veya işletim sistemi fark etmeksizin esneklik.
  - Düşük maliyetle uzun vadeli kullanım imkanı.
