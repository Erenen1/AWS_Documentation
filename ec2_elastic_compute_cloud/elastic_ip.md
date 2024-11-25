
# Esnek IP Adresleri (Elastic IPs)

Amazon Elastic Compute Cloud (EC2), sabit bir genel IPv4 adresi sağlayan **Esnek IP Adresleri** (Elastic IPs) özelliğini sunar. Bu adresler, EC2 instance'larınızın kamuya açık ve erişilebilir olmasını sağlar.

## Esnek IP Nedir?
- **Tanım**: Esnek IP, AWS hesabınıza atanmış sabit bir genel IP adresidir. 
- **Amaç**: EC2 instance'larınıza istikrarlı ve değişmeyen bir IP adresiyle erişim sağlar.
- **Dinamik IP'lerden Farkı**: EC2 instance'larını yeniden başlattığınızda, otomatik olarak atanan IP adresleri değişebilir. Ancak, Esnek IP adresleri değişmeden kalır.

## Özellikler
1. **Kullanıcı Ataması**:
   - Esnek IP adresleri AWS hesabınıza atanır ve istediğiniz instance'a bağlanabilir.
2. **Hareketlilik**:
   - Bir Esnek IP adresi, bir instance'dan diğerine kolayca taşınabilir.
3. **Yedekleme**:
   - Bir instance durdurulduğunda, Esnek IP adresini başka bir instance'a bağlayarak hizmet kesintisini önleyebilirsiniz.

## Kullanım Senaryoları
1. **Sabit IP Gerektiren Uygulamalar**:
   - Müşterilere veya hizmetlere erişim sağlamak için sabit bir IP adresine ihtiyaç duyulan durumlar.
2. **Felaket Kurtarma**:
   - Instance başarısızlığı durumunda, Esnek IP adresini yeni bir instance'a bağlayarak hızlı kurtarma.
3. **Karmaşık Ağ Yapılandırmaları**:
   - Uygulama veya servisler arasında belirli IP adresleriyle sabit iletişim sağlamak.

## Avantajlar
- **Sabitlik**: IP adresi değişmez, bu da sabit DNS yapılandırmaları ve güvenlik kuralları için uygundur.
- **Esneklik**: Instance'lar arasında kolayca taşınabilir.
- **Kesintisiz Erişim**: Instance durumu ne olursa olsun IP adresine erişim devam eder.

## Limitasyonlar
- Her AWS hesabı için varsayılan Esnek IP adresi limiti bulunur (genellikle 5 adres).
- Kullanılmayan bir Esnek IP adresi için AWS ücret talep eder.

## Esnek IP Adresinin Oluşturulması ve Kullanımı

### 1. Esnek IP Adresi Oluşturma
1. AWS Management Console’a giriş yapın.
2. EC2 Hizmeti -> Elastic IPs sekmesine gidin.
3. **Allocate Elastic IP Address** seçeneğini seçin.
4. Bir Elastic IP adresi ayırmak için talimatları izleyin.

### 2. Elastic IP Adresini EC2 Instance’a Bağlama
1. Oluşturulan Elastic IP adresini seçin.
2. **Actions** -> **Associate Elastic IP Address** seçeneğini seçin.
3. Bağlanmak istediğiniz EC2 instance’ı seçin.

### 3. Elastic IP Adresini Kaldırma veya Taşıma
- Esnek IP adresini başka bir instance’a taşımak için mevcut bağlantıyı kaldırın ve yeni instance’a bağlayın.

### AWS CLI ile Elastic IP Adresi Yönetimi
Elastic IP adreslerini AWS CLI kullanarak da yönetebilirsiniz.

#### Elastic IP Ayırma:
```bash
aws ec2 allocate-address --domain vpc
```

#### Elastic IP’yi Instance’a Bağlama:
```bash
aws ec2 associate-address --instance-id <instance-id> --allocation-id <allocation-id>
```

#### Elastic IP’yi Kaldırma:
```bash
aws ec2 release-address --allocation-id <allocation-id>
```

## Ücretlendirme
- Kullanılmayan bir Elastic IP adresi için AWS, saatlik ücret talep eder.
- Kullanılan Elastic IP adresleri ücretsizdir.

## Best Practices
- **Kullanılmayan Elastic IP’leri Serbest Bırakın**: Kullanılmayan Elastic IP adreslerinden kaçının, çünkü bunlar ücretlendirilir.
- **Yedekleme için Plan Yapın**: Felaket kurtarma senaryoları için Elastic IP adreslerini yapılandırın.
- **Güvenlik Gruplarını Yapılandırın**: Elastic IP adreslerini yalnızca güvenilir kaynaklar için erişilebilir hale getirin.

---

Elastic IP adresleri, EC2 instance'larınıza sabit ve güvenilir bir erişim sağlar. Doğru yönetildiğinde, uygulamalarınızın erişilebilirliğini ve dayanıklılığını artırmak için mükemmel bir araçtır.