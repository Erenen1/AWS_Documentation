
# 7. Security Groups (Güvenlik Grupları)

## Security Group Nedir?
Security Group, AWS üzerinde EC2 instance’larınız için bir güvenlik duvarı görevi gören ve gelen-giden trafiği kontrol eden bir güvenlik katmanıdır. AWS, Security Group'ları kullanarak instance’larınıza yalnızca güvenli ve belirlenmiş kaynaklardan erişime izin vermenizi sağlar.

## Security Group Özellikleri
1. **Durum Bilgisi**:
   - Security Group’lar durumsaldır. Gelen bir bağlantıya izin verildiyse, o bağlantıya ait giden trafik otomatik olarak izin alır.
2. **İzin Temelli**:
   - Sadece izin kuralları eklenebilir, bloklama kuralları eklenemez.
3. **Kaynağa Özgü**:
   - Security Group’lar birden fazla EC2 instance’a atanabilir, ancak her instance birden fazla Security Group ile ilişkilendirilebilir.
4. **Anlık Güncellemeler**:
   - Kurallar güncellendiğinde anında uygulanır.

---

## Security Group Kuralları
Security Group’lar iki ana türde kural içerir:

### 1. Inbound Rules (Gelen Trafik Kuralları)
- EC2 instance’a gelen trafiği kontrol eder.
- **Örnek**:
  - SSH erişimi için `Port 22` izni.
  - HTTP erişimi için `Port 80` izni.

### 2. Outbound Rules (Giden Trafik Kuralları)
- EC2 instance’tan dışarı giden trafiği kontrol eder.
- Varsayılan olarak tüm giden trafiğe izin verilir.

---

## Security Group Oluşturma ve Yönetimi

### AWS Management Console ile
1. **Oluşturma**:
   - AWS Management Console’da EC2 Dashboard’a gidin.
   - **Security Groups** sekmesine tıklayın ve yeni bir Security Group oluşturun.
   - Gerekli ad, açıklama ve VPC seçimini yapın.
2. **Kurallar Eklemek**:
   - Inbound ve Outbound Rules sekmelerine kural ekleyin.
   - IP aralıklarını (`0.0.0.0/0` veya belirli IP) ve izin verilen protokolleri belirtin.
3. **Instance’a Atama**:
   - Bir Security Group’u EC2 instance’a atamak için instance ayarlarını düzenleyin.

### AWS CLI ile
#### Security Group Oluşturma:
```bash
aws ec2 create-security-group --group-name MySecurityGroup --description "My security group" --vpc-id vpc-123abc
```

#### Gelen Trafik Kuralı Eklemek:
```bash
aws ec2 authorize-security-group-ingress --group-id sg-123abc --protocol tcp --port 22 --cidr 0.0.0.0/0
```

#### Giden Trafik Kuralı Eklemek:
```bash
aws ec2 authorize-security-group-egress --group-id sg-123abc --protocol tcp --port 80 --cidr 0.0.0.0/0
```

#### Kural Silmek:
```bash
aws ec2 revoke-security-group-ingress --group-id sg-123abc --protocol tcp --port 22 --cidr 0.0.0.0/0
```

---

## Security Group Örnek Senaryoları
1. **Web Sunucusu**:
   - HTTP (Port 80) ve HTTPS (Port 443) trafiğine izin verir.
   - Yönetim için yalnızca belirli IP adreslerinden SSH (Port 22) erişimi sağlar.

2. **Veritabanı Sunucusu**:
   - Yalnızca belirli bir uygulama sunucusundan gelen trafiğe izin verir.
   - Örneğin, MySQL (Port 3306) erişimi sadece belirli IP aralıklarıyla sınırlı olabilir.

3. **Bastion Host**:
   - Yönetimsel erişim için SSH bağlantılarını yönetir ve diğer instance’lara güvenli erişim sağlar.

---

## Security Group İzinleri
- **Protokoller**:
  - TCP, UDP, ICMP, ve özel protokolleri destekler.
- **IP Aralıkları**:
  - CIDR blokları (ör. `192.168.1.0/24`) veya belirli IP adresleri tanımlanabilir.
- **AWS Kaynakları**:
  - Belirli bir Security Group ile ilişkili kaynaklardan gelen trafiğe izin verilebilir.

---

## En İyi Uygulamalar (Best Practices)
1. **Minimum İzin Politikası**:
   - Sadece gerekli protokoller ve portlar için izin verin.
2. **Belirli IP Kısıtlamaları**:
   - Genel trafiğe (`0.0.0.0/0`) izin vermekten kaçının. Mümkünse IP’leri sınırlayın.
3. **Düzenli Denetim**:
   - Kullanılmayan veya gereksiz izin kurallarını kaldırın.
4. **Security Group Etiketleme**:
   - Her Security Group’a anlamlı bir ad ve açıklama ekleyin.
5. **Loglama ve İzleme**:
   - AWS CloudWatch Logs ile trafik izleme ve analiz yapın.

---

## Güvenlik ve Maliyet
- **Güvenlik**:
  - Security Group kurallarını yanlış yapılandırmak, kaynaklarınıza yetkisiz erişime yol açabilir.
- **Maliyet**:
  - Security Group’ların kendisi ücretsizdir, ancak bunların kullandığı kaynaklar (ör. EC2 instance trafiği) maliyete dahil olabilir.

---

Security Group'lar, AWS üzerinde güçlü bir güvenlik katmanı sağlar. Kuralların doğru yapılandırılması, uygulamalarınızın güvenliğini artırır ve saldırılara karşı daha dayanıklı hale getirir.