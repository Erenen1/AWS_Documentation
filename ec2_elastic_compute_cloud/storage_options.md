
# 4. Depolama Seçenekleri

Amazon EC2, farklı iş yükleri ve kullanım senaryoları için çeşitli depolama seçenekleri sunar. Bu seçenekler, performans, dayanıklılık ve erişim özellikleri açısından farklılık gösterir. Aşağıda, EC2 ile birlikte kullanılabilen başlıca depolama çözümleri detaylı olarak incelenmiştir.

## 4.1. Amazon Elastic Block Store (EBS)

**Amazon EBS**, EC2 instance'ları için kalıcı ve yüksek performanslı blok depolama hizmeti sunar. EBS, verilerinizi güvenli bir şekilde depolamanıza ve gerektiğinde yedeklemenize olanak tanır.

### Özellikler
- **Kalıcı Depolama**: EBS, EC2 instance'ınız durdurulsa veya yeniden başlatılsa bile verilerinizi korur.
- **Yüksek Performans**: Farklı performans ihtiyaçlarına uygun çeşitli EBS hacim türleri mevcuttur.
- **Snapshot Desteği**: EBS snapshot'ları ile verilerinizi yedekleyebilir ve farklı bölgelerde saklayabilirsiniz.
- **Şifreleme**: EBS, verilerinizi hem aktarım sırasında hem de depolama alanında şifreleyerek güvenliği artırır.

### Hacim Türleri
1. **Genel Amaçlı SSD (gp3 ve gp2)**: Dengeli fiyat-performans oranı sunar. Çoğu iş yükü için uygundur.
2. **Provisioned IOPS SSD (io2 ve io1)**: Kritik iş yükleri için yüksek IOPS (Input/Output Operations Per Second) performansı sağlar.
3. **Throughput Optimized HDD (st1)**: Büyük, ardışık veri işleme ihtiyaçları için tasarlanmıştır.
4. **Cold HDD (sc1)**: Seyrek erişilen, düşük maliyetli depolama ihtiyaçları için uygundur.

### Snapshot İşlemleri
- **Oluşturma**: EBS snapshot, hacminizin bir yedeğini alır ve Amazon S3'te depolar.
- **Kopyalama**: Snapshot'lar bölgeler arası taşınabilir ve şifrelenebilir.
- **Kurtarma**: Snapshot'tan yeni bir EBS hacmi oluşturularak veriler geri yüklenebilir.

---

## 4.2. Instance Store

**Instance Store**, EC2 instance'larına geçici ve yüksek performanslı depolama sağlar.

### Özellikler
- **Geçici Depolama**: Instance durdurulduğunda veya sonlandırıldığında tüm veriler kaybolur.
- **Düşük Gecikme**: Yüksek hızda okuma/yazma işlemleri için optimize edilmiştir.
- **Yüksek Performans**: Büyük miktarda geçici verinin hızlıca işlenmesi için uygundur.

### Avantajlar
- Düşük gecikme süresi ile yüksek performanslı veri işleme sağlar.
- Büyük veri kümeleri, ara bellekteki işlemler ve geçici dosyalar için idealdir.

### Dezavantajlar
- Kalıcı depolama sağlamaz, bu nedenle yedekleme yapılması önerilir.
- Instance yeniden başlatıldığında veriler kaybolur.

---

## 4.3. Amazon Elastic File System (EFS)

**Amazon EFS**, birden fazla EC2 instance'ı arasında paylaşılan ve ölçeklenebilir bir dosya sistemi sunar.

### Özellikler
- **Paylaşımlı Erişim**: Tüm bağlı instance'lar aynı dosyalara erişebilir.
- **Dinamik Ölçeklenebilirlik**: Veri hacmi otomatik olarak artar veya azalır.
- **NFS Protokolü**: NFSv4 protokolü ile uyumludur.
- **Dayanıklılık**: EFS, verilerinizi otomatik olarak çoğaltır ve yüksek kullanılabilirlik sağlar.

### Avantajlar
- Birden çok instance için ortak dosya erişimi sunar.
- Kullanıcıların manuel işlem yapmasına gerek kalmadan otomatik ölçeklenir.

### Kullanım Alanları
- Büyük veri analitiği.
- İçerik yönetimi ve medya işleme.
- Paylaşımlı depolama gerektiren uygulamalar.


## RAID 5'in EFS ile Kullanımı

### RAID Nedir?
RAID (Redundant Array of Independent Disks), birden fazla fiziksel diskin bir araya getirilerek veri güvenliği, hız veya her ikisini birden sağlamaya yönelik bir teknolojidir. RAID, farklı seviyelerde yapılandırılabilir ve her seviye farklı avantajlar sunar.

### RAID 5
RAID 5, veri koruma ve performansı dengeleyen popüler bir RAID seviyesidir.
- **Özellikler**:
  - Veriler ve parite bilgileri birden fazla diske dağıtılır.
  - Bir disk arızalandığında, kaybolan veriler parite bilgileri kullanılarak yeniden oluşturulabilir.
  - En az 3 disk gerektirir.
- **Avantajlar**:
  - Veri koruma sağlar.
  - Okuma performansı yüksektir.
- **Dezavantajlar**:
  - Yazma işlemleri sırasında parite hesaplamaları nedeniyle biraz yavaş olabilir.

## EFS ile RAID 5 Kullanımı
Amazon EFS (Elastic File System), birden fazla EC2 instance tarafından paylaşılan, ölçeklenebilir bir dosya sistemidir. EFS'nin kendisi RAID gibi çalıştığı için EFS üzerinde RAID yapılandırması genellikle gerekli değildir. Ancak, belirli iş yüklerinde RAID yapılandırması tercih edilebilir.

### RAID 5 ve EFS Uygulaması
RAID 5, genellikle EBS hacimleri üzerinde uygulanır. Ancak, EFS gibi ağ tabanlı dosya sistemlerinde RAID'e ihtiyaç duyulmaz çünkü:
- EFS, yüksek dayanıklılık ve kullanılabilirlik sağlar.
- Veriler otomatik olarak birden fazla AZ'de çoğaltılır.

### RAID 5'i Amazon EC2'de Kurmak
EFS üzerinde RAID yapılandırması yerine, RAID 5 genellikle EBS hacimleri ile uygulanır. İşte EBS kullanarak RAID 5 yapılandırması adımları:
1. **EBS Hacimlerini Yaratın**:
   - En az 3 EBS hacmi oluşturun.
2. **EC2 Instance’a Bağlayın**:
   - Bu EBS hacimlerini bir EC2 instance’a bağlayın.
3. **RAID Yapılandırması**:
   - `mdadm` aracı ile RAID 5 yapılandırması yapın:
     ```bash
     sudo apt update
     sudo apt install -y mdadm
     sudo mdadm --create --verbose /dev/md0 --level=5 --raid-devices=3 /dev/xvdf /dev/xvdg /dev/xvdh
     ```
4. **Dosya Sistemini Biçimlendirin**:
   - RAID cihazını bir dosya sistemi ile biçimlendirin:
     ```bash
     sudo mkfs.ext4 /dev/md0
     ```
5. **Mount Edin**:
   - RAID cihazını bir dizine mount edin:
     ```bash
     sudo mkdir -p /mnt/raid5
     sudo mount /dev/md0 /mnt/raid5
     ```
6. **Otomatik Başlatma**:
   - RAID yapılandırmasını fstab’a ekleyerek otomatik başlatılmasını sağlayın.

### RAID ve EFS Kullanımı
- RAID yapılandırması EBS hacimlerinde daha yaygındır.
- EFS, kendiliğinden yedekli ve dayanıklı olduğu için RAID kullanımı gereksizdir.