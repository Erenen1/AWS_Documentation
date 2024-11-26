
# AMI (Amazon Machine Image)

## AMI Nedir?
Amazon Machine Image (AMI), EC2 instance’larınızı başlatmak için gerekli olan bir şablondur. AMI, işletim sistemi, uygulamalar ve konfigürasyon bilgilerini içerir. AMI, AWS'de yeni EC2 instance'ları hızlı bir şekilde başlatmak ve aynı yapılandırmayı birden fazla instance üzerinde tekrarlamak için kullanılır.

### AMI'nin Temel Bileşenleri
1. **Root Volume Template**:
   - İşletim sistemini ve önceden yüklenmiş uygulamaları içeren temel dosya sistemi.
2. **Başlatma İzinleri**:
   - Hangi AWS hesaplarının AMI’yi kullanabileceğini kontrol eder.
3. **Blok Cihaz Haritası**:
   - Instance’a bağlı tüm ek hacimlerin bilgilerini içerir.

## AMI Türleri
AWS, farklı kullanım senaryolarına uygun birkaç AMI türü sunar:

### 1. **AWS Tarafından Sağlanan AMI’ler**
- AWS tarafından sağlanan ve çeşitli işletim sistemlerini içeren genel şablonlardır.
- **Örnekler**:
  - Amazon Linux.
  - Ubuntu, Red Hat, Windows Server.

### 2. **Marketplace AMI’ler**
- Üçüncü taraf satıcılar tarafından sağlanır ve genellikle ek yazılımlar içerir.
- **Kullanım Alanları**:
  - Özel yazılım gereksinimleri.
  - Lisanslı işletim sistemleri veya araçlar.

### 3. **Özel AMI’ler**
- Kullanıcıların kendi gereksinimlerine göre oluşturduğu AMI’lerdir.
- **Avantajlar**:
  - Özel yazılım ve ayarların önceden yüklenmesi.
  - Aynı yapılandırmanın birden fazla instance üzerinde hızlıca uygulanması.

### 4. **Topluluk AMI’leri**
- AWS kullanıcıları tarafından paylaşılmış ve ücretsiz veya ücretli olarak kullanılabilir.

## AMI Oluşturma
Yeni bir AMI oluşturmak için aşağıdaki adımları izleyebilirsiniz:

### 1. AMI Hazırlığı
1. Bir EC2 instance başlatın.
2. Instance’ı gerekli işletim sistemi, uygulamalar ve konfigürasyonlarla yapılandırın.

### 2. AMI Oluşturma
1. AWS Console'da EC2 Dashboard'a gidin.
2. Oluşturulan instance'ı seçin.
3. **Actions** -> **Image and Templates** -> **Create Image** seçeneğini seçin.
4. AMI için bir isim ve açıklama girin.
5. Gerekli blok cihaz haritasını kontrol edin ve oluşturmayı tamamlayın.

### 3. AMI Kullanımı
1. Yeni bir EC2 instance başlatırken oluşturduğunuz AMI’yi seçin.
2. Instance boyutunu ve diğer ayarları yapılandırın.
3. AMI’den başlatılan tüm instance’lar aynı yapılandırmaya sahip olacaktır.

## AMI Yönetimi
### AMI Paylaşımı
- Bir AMI’yi diğer AWS hesaplarıyla paylaşabilirsiniz.
- Paylaşımı sınırlamak veya genel erişim sağlamak için başlatma izinlerini ayarlayın.

### AMI Kopyalama
- AMI’yi farklı bölgelerde kullanmak için kopyalayabilirsiniz.
- **Örnek**: Bir AMI’yi `us-east-1` bölgesinden `eu-west-1` bölgesine taşımak.

### AMI Silme
- Artık kullanılmayan AMI’leri silerek yönetimi kolaylaştırabilirsiniz.
- Not: AMI silindiğinde, bağlı snapshot’lar da silinir.

## Avantajlar
1. **Standartlaştırma**:
   - Uygulamalarınız ve işletim sistemi yapılandırmalarınız için standart bir temel sağlar.
2. **Hızlı Dağıtım**:
   - AMI kullanarak dakikalar içinde yeni EC2 instance’ları başlatabilirsiniz.
3. **Esneklik**:
   - Özel yazılım yüklemeleri ve ayarlarla özelleştirilmiş AMI’ler oluşturabilirsiniz.
4. **Taşınabilirlik**:
   - AMI’ler farklı bölgeler arasında kopyalanabilir.

## AMI Ücretlendirmesi
- AMI'nin kendisi ücretsizdir.
- Ancak AMI’ye bağlı snapshot’ların depolama maliyetleri AWS tarafından faturalandırılır.

## AWS CLI ile AMI Yönetimi
AMI’leri AWS CLI kullanarak da yönetebilirsiniz:

### AMI Oluşturma:
```bash
aws ec2 create-image --instance-id <instance-id> --name "<ami-name>" --description "<description>"
```

### AMI Listeleme:
```bash
aws ec2 describe-images --owners self
```

### AMI Silme:
```bash
aws ec2 deregister-image --image-id <ami-id>
```

## Best Practices
1. **Güncel Kalma**:
   - AMI’lerinizi düzenli olarak güncelleyin.
2. **Paylaşım Güvenliği**:
   - AMI paylaşımı sırasında izinleri dikkatlice yapılandırın.
3. **Snapshot Yönetimi**:
   - Gereksiz snapshot’ları silerek maliyetleri düşürün.
4. **Versiyonlama**:
   - Önemli değişiklikler için farklı AMI versiyonları oluşturun.

---

AMI, Amazon EC2 instance’larınızı başlatmak ve yönetmek için güçlü ve esnek bir araçtır. Doğru yapılandırılmış AMI'ler, uygulamalarınızın hızını, esnekliğini ve ölçeklenebilirliğini artırır.