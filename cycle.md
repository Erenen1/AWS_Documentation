# Büyük Ölçekli Mikroservis CI/CD Süreci

**Evet, genel mantık tam da anlattığın gibi işliyor.** Tabii bazı detaylar ve sıralamalar projeye veya kuruma özel olarak değişebilir ama **temel akış** aşağı yukarı şöyle:

---

## 1. Kod Versiyonlama (SCM) ve Commit

- Geliştirici, GitHub’taki *user servisi* örneğinde “main” (veya başka bir ana branch) üzerine kodunu push’ladığında CI pipeline tetiklenir.

---

## 2. CI Süreci (Jenkins/GitLab CI/GitHub Actions vb.)

1. **Build**  
   - Kod dilden bağımsız olarak (Java, Node.js, Go vb.) önce derlenir veya paketlenir (artifact oluşturulur).

2. **Test**  
   - Unit test, integration test, code coverage, lint, security scan vb. adımlar burada koşar.

3. **(Test Başarılı İse) Docker Build & Push**  
   - Oluşan artifact’tan Docker image oluşturulur. Versiyon (tag) belirlenip image Docker Registry’ye (Docker Hub, GitLab Container Registry, AWS ECR vb.) push edilir.

---

## 3. (Opsiyonel) Altyapı Kurulumu veya Güncellemesi (Terraform/Ansible)

- Eğer **yeni** bir ortam (ör. staging) hazırlanacaksa veya var olan altyapı konfigürasyonu güncellenecekse:
  - **Terraform** ile bulut kaynakları (ör. AWS’de EC2, VPC, RDS, Security Group vb.) tanımlanır veya güncellenir.  
  - **Ansible** ile sunuculara ek paket yükleme, konfigürasyon dosyası kopyalama gibi işlemler yapılır.
- Bu adım, sürekli (her deploy’da) değil, altyapı güncellemesi gerektiğinde çalışacak şekilde de organize edilebilir.

---

## 4. CD Süreci (Deployment Aşamaları)

1. **Dev / Staging Ortamına Kurulum**  
   - Mikroservisin Kubernetes manifest’leri veya Helm chart’ındaki image tag (örn. `:v1.2.3`) güncellenir.  
   - `kubectl apply` veya Helm ile deploy edilerek container Kubernetes içinde ayağa kalkar.  
   - Eğer Kubernetes yoksa, basit bir sunucuda (EC2/VM) “docker pull” komutuyla registry’den yeni image çekilir ve container başlatılır.

2. **Automated Tests**  
   - Staging ortamına deploy edilen uygulama için ek olarak entegrasyon testleri, API testleri, e2e testler (Selenium vb.) koşulabilir.

3. **Approval Gate**  
   - Testler geçtiyse üretim (production) ortamına geçiş için manuel onay adımı eklenebilir (Jenkins’in “Input step” özelliği, GitLab/GitHub’daki onay mekanizmaları vb.).

4. **Production’a Dağıtım (Prod Deploy)**  
   - **Rolling Update / Blue-Green / Canary** gibi stratejilerle canlı ortamda kesintisiz güncelleme sağlanır.  
   - Kubernetes kullanılıyorsa, yeni container’lar ayağa kalkıp eski sürüm kapatılır (rolling update).  
   - Klasik yöntemle, basit bir sunucuda docker container’ı durdurup yeni container’ı başlatmak da yapılabilir, ancak büyük projelerde orkestratörlerle otomasyon tercih edilir.

---

## 5. Monitoring & Observability

- Deploy tamamlandıktan sonra, **Prometheus, Grafana, ELK/EFK Stack, Datadog, New Relic** vb. monitoring araçları ile servislerin durumu anlık izlenir.  
- Olası hata/performans sorunlarında alarm mekanizmaları (**PagerDuty, OpsGenie, Slack uyarıları** vb.) devreye girer.

---

### Sıralama ve Detaylar Değişebilir

- Bazı şirketler, **Terraform/Ansible adımını** pipeline’ın en sonuna koyar veya tamamen ayrı bir “altyapı pipeline” olarak organize eder.  
- Kimisi “**GitOps**” (Argo CD, Flux) yaklaşımıyla, manifest dosyası Git repo’ya push edilince Kubernetes’in otomatik almasını sağlar. Böylece Jenkins sadece imajı registry’ye push eder, gerisini GitOps halleder.  
- Kimisi de QA aşamasında **manuel test** ekler, ondan sonra prod’a geçiş adımına izin verir.

Fakat sonuç olarak bu akış:
1. **Commit**  
2. **CI (Build, Test, Docker Build + Push)**  
3. **Opsiyonel Altyapı (Terraform, Ansible)**  
4. **CD (Deploy → Test → Approval → Prod)**  

büyük ölçekli projelerde sıkça görülen bir düzendir ve **“Evet, doğru anlamışsın”** diyebiliriz.
