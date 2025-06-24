# ✨ Clean Code

**Hedefimiz; okunabilirliği, sürdürülebilirliği ve test edilebilirliği ön planda tutarak, geliştiricilere yüksek standartlarda kod yazma alışkanlığı kazandırmaktır.**

## 🎯 Neden Clean Code?

Clean Code, sadece kodun çalışmasından öte, başkaları tarafından kolayca anlaşılabilen, bakımı yapılabilen ve geliştirilebilen kod yazma pratiğidir.

- 📈 **Bakım Kolaylığı:** İyi yazılmış kodun hatası daha az olur ve değişmesi kolaydır.
- 🤝 **Takım Çalışması:** Anlaşılır kod, takım üyeleri arasında işbirliğini artırır.
- ⏱️ **Zaman Tasarrufu:** Yeni özellik eklemek veya hata düzeltmek için harcanan zaman azalır.
- 🐛 **Hata Azaltma:** Karmaşık ve dağınık kod, hatalara davetiye çıkarır. Clean Code bu riski minimize eder.

## 📖 Temel Clean Code Prensipleri

İşte Clean Code'un temel taşları ve her birini C# dilinde nasıl uygulayabileceğinize dair örnekler:

### 1. 📛 Meaningful Naming (Anlamlı İsimlendirme)

Değişkenlerinizin, metotlarınızın, sınıflarınızın ve dosyalarınızın ne işe yaradığını açıkça belirten isimler kullanın.

- **👎 Kötü Örnek:**

```csharp
int d; // Geçen gün sayısı
List<int> theList; // Kullanıcı ID'leri
void DoStuff();
```

- **👍 İyi Örnek:**

```csharp
int elapsedDays;
List<int> userIds;
void CalculateTotalOrderAmount();
```

- **İpuçları:**

- **Niyetini Açıkla:** Bir değişkenin veya metodun ne için kullanıldığını ismiyle anlatın.
- **Telaffuz Edilebilir Olun:** İsimler kolayca okunabilmeli ve telaffuz edilebilmeli.
- **Aranabilir Olun:** Kısa, tek harfli isimlerden kaçının; kod aramalarda kolayca bulunabilen isimler kullanın.

### 2. 📏 Functions (Fonksiyonlar)

Fonksiyonlarınız kısa, tek bir iş yapan ve okunması kolay olmalıdır.

- **👎 Kötü Örnek:**

```csharp
public void ProcessOrder(Order order)
{
    // Sipariş doğrulama
    if (order.Items.Count == 0)
    {
        throw new ArgumentException("Sipariş öğesi içermiyor.");
    }

    // Stok kontrolü
    foreach (var item in order.Items)
    {
        if (!_inventoryService.IsInStock(item.ProductId, item.Quantity))
        {
            throw new InvalidOperationException($"Ürün {item.ProductId} stokta yok.");
        }
    }

    // Ödeme işlemleri
    _paymentService.ProcessPayment(order.CustomerId, order.TotalAmount);

    // Sipariş durumu güncelleme
    order.Status = OrderStatus.Processed;
    _orderRepository.Update(order);

    // E-posta gönderimi
    _notificationService.SendOrderConfirmationEmail(order.CustomerId, order.OrderId);
}
```

- **👍 İyi Örnek:**

```csharp
public void ProcessOrder(Order order)
{
    ValidateOrder(order);
    CheckInventory(order);
    ProcessPayment(order);
    UpdateOrderStatus(order);
    SendOrderConfirmation(order);
}

private void ValidateOrder(Order order)
{
    if (order.Items.Count == 0)
    {
        throw new ArgumentException("Sipariş öğesi içermiyor.");
    }
}

private void CheckInventory(Order order)
{
    foreach (var item in order.Items)
    {
        if (!_inventoryService.IsInStock(item.ProductId, item.Quantity))
        {
            throw new InvalidOperationException($"Ürün {item.ProductId} stokta yok.");
        }
    }
}

private void ProcessPayment(Order order)
{
    _paymentService.ProcessPayment(order.CustomerId, order.TotalAmount);
}

private void UpdateOrderStatus(Order order)
{
    order.Status = OrderStatus.Processed;
    _orderRepository.Update(order);
}

private void SendOrderConfirmation(Order order)
{
    _notificationService.SendOrderConfirmationEmail(order.CustomerId, order.OrderId);
}
```

- **İpuçları:**

- **Single Responsibility Principle - SRP (Tek Sorumluluk Prensibi):** Her fonksiyon sadece bir iş yapmalı.
- **Kısa Olun:** Fonksiyonlar olabildiğince kısa olmalı. Genellikle 5-10 satırı geçmemeli.
- **İyi İsimlendirin:** Fonksiyonun ne yaptığını ismiyle netleştirin.
- **Argüman Sayısını Azaltın:** Fonksiyonlara çok fazla argüman geçmek karmaşıklığı artırır. 0-2 arası argüman idealdir.

### 3. 💬 Comments (Yorumlar)

Yorumlar, kötü yazılmış kodu telafi etmek yerine, neden belirli bir kararın alındığını veya karmaşık bir algoritmik yapının ne anlama geldiğini açıklamak için kullanılmalıdır. Kodunuz kendini açıklamalıdır.

- **👎 Kötü Örnek:**

```csharp
// Müşterinin bakiyesini günceller
public void UpdateCustomerBalance(int customerId, decimal amount)
{
    // Müşteriyi veritabanından al
    var customer = _customerRepository.GetById(customerId);
    // Bakiyeye miktarı ekle
    customer.Balance += amount;
    // Müşteriyi veritabanına kaydet
    _customerRepository.Save(customer);
}
```

- **👍 İyi Örnek:**

```csharp
// Finansal raporlama nedeniyle, bu metodun müşteri bakiyesini doğrudan güncellemesi gerekmektedir.
// Gelecekte, bu işlem için bir Transaction servisi kullanılabilir.
public void UpdateCustomerBalanceForReporting(int customerId, decimal amount)
{
    var customer = _customerRepository.GetById(customerId);
    customer.Balance += amount;
    _customerRepository.Save(customer);
}
```

- **İpuçları:**

- Gereksiz Yorumlardan Kaçının: Kodunuz kendini açıklıyorsa, yorum eklemeyin.
- "Neden" Açıklayın: Yorumlar, kodun "nasıl" çalıştığından çok "neden" çalıştığını açıklamalıdır.
- TODO ve FIX ME: Gelecekte yapılacak işleri veya düzeltmeleri belirtmek için kullanın.

### 4. 🧮 Formatting (Biçimlendirme)

Tutarlı bir kod biçimlendirmesi, kodun okunabilirliğini büyük ölçüde artırır. Boşluklar, girintiler, satır sonları ve parantez konumlandırmaları önemlidir.

- **👎 Kötü Örnek:**

```csharp
public class Product{public int Id{get;set;}public string Name{get;set;}}
public void Save(Product p){
//kaydet
_repo.Add(p);
}
```

- **👍 İyi Örnek:**

```csharp
public class Product
{
    public int Id { get; set; }
    public string Name { get; set; }
}

public void Save(Product product)
{
    _repository.Add(product);
}
```

- **İpuçları:**

- **Tutarlılık:** Tüm projede aynı biçimlendirme kurallarını kullanın (örn. Visual Studio Ayarları, EditorConfig).
- **Kısa Satırlar:** Genellikle bir kod satırı 120 karakteri geçmemeli.
- **Boşluklar:** Operatörler, virgüller ve parantezler etrafında uygun boşlukları kullanın.

### 5. 🛠️ Error Handling (Hata Yönetimi)

Hataları düzgün bir şekilde ele almak, uygulamanın sağlamlığını ve kullanıcı deneyimini artırır.

- **👎 Kötü Örnek:**

```csharp
public User GetUserById(int userId)
{
    // Try-catch bloğu yok, hata durumunda uygulama çökebilir
    return _userRepository.GetById(userId);
}
```

- **👍 İyi Örnek:**

```csharp
public User GetUserById(int userId)
{
    try
    {
        return _userRepository.GetById(userId);
    }
    catch (NotFoundException ex)
    {
        _logger.LogError($"Kullanıcı bulunamadı: {userId}", ex);
        throw new UserNotFoundException($"Belirtilen ID ile kullanıcı bulunamadı: {userId}", ex);
    }
    catch (Exception ex)
    {
        _logger.LogError($"Kullanıcı alınırken beklenmeyen bir hata oluştu: {userId}", ex);
        throw new ApplicationException("Kullanıcı bilgileri alınırken bir hata oluştu.", ex);
    }
}
```

- **İpuçları:**

- **Özel Exception'lar:** Hata durumunu daha spesifik hale getirmek için özel istisnalar oluşturun.
- **Hata Yakalama Sorumluluğu:** Hatayı nerede yakalayacağınıza ve ne yapacağınıza karar verin. Her yerde try-catch kullanmak doğru değildir.
- **Loglama:** Hataları uygun bir şekilde loglayarak, sorunların tespiti ve çözümü kolaylaşır.

### 6. 🧪 Unit Tests (Birim Testleri)

Clean Code, test edilebilir kod yazmayı teşvik eder. Birim testleri, kodunuzun beklendiği gibi çalıştığını doğrulamak için kritik öneme sahiptir.

- **👎 Kötü Örnek:**

```csharp
// Test edilemez veya zor test edilebilir bir yapı
public class OrderProcessor
{
    private OrderRepository _orderRepository = new OrderRepository(); // Bağımlılık doğrudan yaratılıyor

    public void ProcessOrder(Order order)
    {
        // ... sipariş işleme mantığı ...
        _orderRepository.Save(order);
    }
}
```

- **👍 İyi Örnek:**

```csharp
// Bağımlılık Enjeksiyonu ile test edilebilir yapı
public class OrderProcessor
{
    private readonly IOrderRepository _orderRepository;

    public OrderProcessor(IOrderRepository orderRepository) // Bağımlılık enjekte ediliyor
    {
        _orderRepository = orderRepository;
    }

    public void ProcessOrder(Order order)
    {
        // ... sipariş işleme mantığı ...
        _orderRepository.Save(order);
    }
}

// Birim Testi Örneği (xUnit veya NUnit ile)
public class OrderProcessorTests
{
    [Fact]
    public void ProcessOrder_ShouldSaveOrder_WhenValid()
    {
        // Arrange
        var mockOrderRepository = new Mock<IOrderRepository>();
        var processor = new OrderProcessor(mockOrderRepository.Object);
        var order = new Order { Id = 1, Status = OrderStatus.New };

        // Act
        processor.ProcessOrder(order);

        // Assert
        mockOrderRepository.Verify(repo => repo.Save(order), Times.Once);
        Assert.Equal(OrderStatus.Processed, order.Status); // Örnek olarak ekledim, iş mantığına göre değişir
    }
}
```

- **İpuçları:**

- **Test Edilebilir Tasarım:** Kodunuzu yazarken, bağımlılık enjeksiyonu gibi prensipleri kullanarak test edilebilir olmasını sağlayın.
- **Hızlı ve Bağımsız Testler:** Birim testleri hızlı çalışmalı ve birbirlerinden bağımsız olmalı.
- **Kapsam:** Önemli iş mantığını kapsayan yeterli test yazın.

---

## Temiz Kod İçin Önerilen Araçlar ve Eklentiler

### Code Formatters (Kod Biçimlendiriciler) 

**Stilsel Tutarlılığı Sağlama**

Kod biçimlendiricilerin temel amacı, projeler arasında tutarlı bir kodlama stilini zorunlu kılmaktır. Bu tutarlılık, kodu birden fazla geliştirici için daha kolay okunur ve anlaşılır hale getirir. Manuel biçimlendirme sırasında ortaya çıkabilecek insan hatalarını azaltır ve kod incelemelerini basitleştirir; çünkü gözden geçirenler, biçimlendirme sorunları yerine kodun mantığına ve işlevselliğine odaklanabilirler. Biçimlendiriciler, geliştiricilerin manuel biçimlendirme görevlerini otomatikleştirerek zaman ve çaba tasarrufu yapmalarını sağlar, böylece onların daha karmaşık geliştirme görevlerine odaklanmalarına olanak tanır. 

**Piyasada birçok popüler kod biçimlendirici bulunmaktadır:**

- Prettier: JavaScript, TypeScript, HTML, CSS, Less, SCSS, JSX, Angular, Vue, Flow, JSON, GraphQL, Markdown ve YAML gibi birçok web geliştirme dilini destekler.
- Black: Python için tavizsiz bir kod biçimlendiricidir ve PEP 8 standartlarına uygunluğu otomatik olarak sağlar.
- gofmt: Go dili için standart bir biçimlendiricidir.
- RuboCop: Ruby dili için kullanılır.
- clang-format: C ve C++ dilleri için biçimlendirme sağlar.
- PHP_CodeSniffer: PHP dili için stil denetimi yapar.
- DeepSource gibi platformlar, bu biçimlendiricilerin çoğunu "Dönüştürücüler" (Transformers) olarak entegre ederek otomatik biçimlendirme yetenekleri sunar.   

### Linters (Lint Araçları) 

**Stil Sorunlarını ve Potansiyel Hataları Tespit Etme**

Lint araçlarının temel amacı, geliştirme sürecinin mümkün olan en erken aşamalarında hataları ve potansiyel bugları tespit etmektir. Bu proaktif yaklaşım, sorunların üretime ulaşmasını engelleyerek hata ayıklama süresini ve maliyetini önemli ölçüde azaltır. Lint araçları, tanımlanmış kodlama standartlarını ve en iyi uygulamaları zorunlu kılar, böylece kod kalitesini ve sürdürülebilirliğini artırır. Ayrıca, Entegre Geliştirme Ortamları'na (IDE'ler) entegre olarak gerçek zamanlı geri bildirim sağlarlar, bu da geliştiricilerin kod yazarken sorunları anında görmelerini ve düzeltmelerini sağlar. Bazı lint araçları, API'nin yanlış kullanım örneklerini otomatik olarak düzeltebilir, bu da geliştiriciler için değerli bir eğitim aracı olarak işlev görür.   

**Popüler lint araçları şunları içerir:**

- **ESLint:** JavaScript ve TypeScript için yaygın olarak kullanılan bir lint aracıdır. Hızlı, esnek yapısıyla bilinir ve otomatik düzeltme yetenekleri ile zengin bir eklenti ekosistemine sahiptir.
- **Pylint:** Python için kapsamlı bir statik kod analizcisidir. Hataları kontrol eder, kodlama standartlarını uygular, kod kokularını arar ve yeniden düzenleme önerileri sunar.
- **Checkstyle:** Java kodu için bir kodlama standardı denetleyicisidir. Biçimlendirme, adlandırma kuralları ve diğer stilistik konuları zorunlu kılmaya odaklanır.
- **Clang Static Analyzer:** C ve C++ dilleri için tasarlanmış güçlü bir statik analiz aracıdır.
- **PMD:** Java odaklı olmakla birlikte, birçok dili destekleyen ve hem linter hem de statik analiz aracı olarak işlev gören çok dilli bir araçtır.
- **SpotBugs:** Java bytecode'unu analiz ederek potansiyel bugları ve kod kalitesi sorunlarını tespit eden bir araçtır.   

### SCA (Statik Kod Analizi) 

SCA araçlarının temel amacı, yazılım sorunlarını geliştirme sürecinin mümkün olan en erken aşamasında tespit etmektir. Bu, sorunların üretime ulaşmadan önce düzeltilmesini sağlayarak maliyetleri önemli ölçüde düşürür. SCA araçları, Yazılım Geliştirme Yaşam Döngüsü'nü (SDLC) "sola kaydırma" prensibini destekler; yani, sorunlu çekme isteklerinin birleştirilmesini engellemek ve önemli bilgileri sorunlar son kullanıcılara ulaşmadan önce geliştiricilere sunmak için IDE'ler, Git kancaları ve CI/CD işlem hatlarıyla entegre olurlar.   

Bu araçlar, kod stili ve adlandırma kuralı ihlallerinden, hataya açık kod kalıplarına ve SQL enjeksiyonları gibi güvenlik sorunlarına kadar çok çeşitli problemleri tespit edebilir. Ayrıca, güvenlik sorunlarını üretime çıkmadan önce azaltmaya yardımcı olurlar. Kuruluşların kalite ve bakım kolaylığı için kodlama yönergelerini zorunlu kılmalarına yardımcı olurlar ve zamanla kod kalitesini sürekli olarak iyileştirmeye teşvik ederler.   

**Piyasada birçok güçlü SCA aracı bulunmaktadır:**

- **SonarQube:** Java, C#, JavaScript/TypeScript, Python ve Go dahil 30'dan fazla dili destekleyen açık kaynaklı bir platformdur. Bugları, kod kokularını ve güvenlik açıklarını tespit etmeye odaklanır.
- **DeepSource:** Statik analiz yoluyla bugları, anti-pattern'ları ve güvenlik açıklarını tespit eder. Birçok dili ve biçimlendiriciyi destekler, ayrıca Yazılım Bileşimi Analizi (SCA) ile açık kaynak bağımlılıklarındaki güvenlik açıklarını da tanımlar.
- **Code Climate:** Kod kalitesini (karmaşıklık, tekrar) iyileştirmeye odaklanan bir statik analiz aracıdır. GitHub ile entegre olarak gerçek zamanlı geri bildirim sağlar ve "Kod Puanı" gibi metrikler kullanır.
- **PMD:** Java odaklı olmakla birlikte, kopyala-yapıştır hataları, kullanılmayan değişkenler ve aşırı karmaşık kod gibi sorunları yakalamak için tasarlanmıştır.
- **SpotBugs:** Java bytecode'unu analiz ederek null pointer dereferansı, sonsuz döngüler ve performans darboğazları gibi yaygın programlama hatalarını tespit eder.
- **ESLint:** Temel olarak bir lint aracı olsa da, JavaScript ve TypeScript kodunda güvenlik açıklarını ve kod kalitesi sorunlarını tespit etmek için de kullanılır.
- Diğer önemli SCA araçları arasında Checkmarx, Veracode, Fortify Static Code Analyzer (SCA), Semgrep, Snyk Code, Coverity, Klocwork, Bandit, RuboCop, Infer, Brakeman, PHPStan, Gosec ve CodeQL yer almaktadır.   

### Refactoring (Yeniden Düzenleme)  

**Kod İyileştirmeyi Otomatikleştirme**

Yeniden düzenleme araçlarının temel amacı, sıkıcı ve tekrarlayan kod değişikliklerini otomatikleştirerek geliştirici üretkenliğini artırmaktır. Bu araçlar, kod tabanındaki potansiyel optimizasyonları belirler, yaygın kod kokularını (örneğin, çok uzun fonksiyonlar, karmaşık koşullu ifadeler) tespit eder ve gereksiz veya yinelenen kod bloklarını otomatik olarak kaldırır. Gerçek zamanlı kod analizi ve hızlı kod değişiklikleri sağlamak için doğrudan Entegre Geliştirme Ortamlarına (IDE'ler) entegre olurlar, genellikle klavye kısayolları aracılığıyla erişilebilirler. Bu sayede, kodun okunabilirliği ve bakım kolaylığı önemli ölçüde artırılır, bu da gelecekteki geliştirmeleri ve hata ayıklamayı daha verimli hale getirir.   

**Piyasada birçok etkili yeniden düzenleme aracı bulunmaktadır:**

- **IntelliJ IDEA:** Özellikle Java ve Kotlin geliştiricileri için tasarlanmış akıllı otomatik yeniden düzenleme yetenekleri sunar. Metot çıkarma, metotları yeniden adlandırma ve kod tekrarı kontrolleri gibi özelliklere sahiptir. İçe aktarmaları optimize etme ve kullanılmayan kod/metotları tespit etme gibi temiz kod özelliklerine de sahiptir.
- **ReSharper:** Visual Studio için bir eklenti olup, C# geliştiricileri için anlık kod analizi, değişkenleri yeniden adlandırma, metot çıkarma ve kullanılmayan değişkenleri otomatik düzeltme gibi özellikler sunar. Çözüm genelinde otomatik yeniden düzenlemeler ve kod stilini zorunlu kılma yetenekleri ile öne çıkar.
- **Visual Studio Code Eklentileri:** Yerleşik yeniden düzenleme eylemleri (metot çıkarma, değişken çıkarma) ve üçüncü taraf eklentileri aracılığıyla genişletilmiş destek sunar. TypeScript ve JavaScript için yerleşik yeniden düzenleme desteği mevcuttur.
- **SonarLint:** IDE'ler için hafif bir statik kod analiz aracıdır. Kod yazarken gerçek zamanlı linting ve kod kokularının tespiti konusunda yardımcı olur.
- **Tabnine:** Yapay zeka destekli kod tamamlama ve otomatik yeniden düzenleme önerileri sunar.
- **CodeMaid:** .NET ekipleri için kod bloklarını düzenler ve temizler, boşlukları kaldırır ve kullanımları sıralar.
- **Sourcery:** Python için gerçek zamanlı yeniden düzenleme yetenekleri sunan bir araçtır.   

**Ekipler için Eyleme Geçirilebilir Öneriler:**

1. Net Standartlar Belirleyin ve İletin: Ekip genelinde açık ve anlaşılır kodlama standartları oluşturun. Bu standartlar, stil rehberlerini, adlandırma kurallarını ve temel tasarım ilkelerini içermelidir. Tüm ekip üyelerinin bu standartları anlamasını ve benimsemesini sağlayın.
2. Araçları SDLC Boyunca Entegre Edin: Biçimlendiricileri ve lint araçlarını IDE'lere entegre ederek geliştiricilere gerçek zamanlı geri bildirim sağlayın. Statik kod analizi araçlarını ve kalite kapılarını CI/CD işlem hatlarına dahil ederek kodun ana dala birleşmeden önce otomatik olarak denetlenmesini zorunlu kılın. Ön-taahhüt kancalarını kullanarak yerel geliştirme ortamında bile kalite kontrolleri uygulayın.
3. Dil ve Proje İhtiyaçlarına Uygun Araçları Seçin: Her dilin ve projenin kendine özgü ihtiyaçları vardır. Örneğin, Python için Black ve Pylint, JavaScript/TypeScript için Prettier ve ESLint, Java için Checkstyle, PMD ve SpotBugs gibi araçları değerlendirin. Kapsamlı SCA için SonarQube veya DeepSource gibi platformları düşünün.
4. Katılık ile Esnekliği Dengeleyin: Otomatik araçların katı kuralları, "uyarı yorgunluğuna" veya gereksiz "bikeshedding" tartışmalarına yol açabilir. Araç yapılandırmalarını dikkatlice ayarlayarak yanlış pozitifleri en aza indirin ve geliştiricilerin bağlamsal istisnaları yönetmesine izin veren mekanizmalar sağlayın (örneğin, // prettier-ignore veya kural devre dışı bırakma).
5. Sürekli İyileştirme ve Öğrenme Kültürünü Teşvik Edin: Araçların sağladığı geri bildirimi bir öğrenme fırsatı olarak görün. Geliştiricileri, kod kalitesi sorunlarını anlamaya ve bunları çözmek için en iyi uygulamaları öğrenmeye teşvik edin. Düzenli kod incelemeleri ve bilgi paylaşımı oturumları düzenleyerek bu kültürü destekleyin.
6. Kapsamlı Bir Araç Kombinasyonu Kullanın: Tek bir araç tüm kod kalitesi sorunlarını çözemez. Biçimlendirme, linting, derinlemesine statik analiz ve yeniden düzenleme için farklı araçların bir kombinasyonunu kullanmak, en kapsamlı kalite güvencesini sağlayacaktır. Örneğin, Prettier ve ESLint'in birlikte kullanımı gibi.

---
