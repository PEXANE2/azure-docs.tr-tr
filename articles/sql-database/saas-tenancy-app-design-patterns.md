---
title: Çok kiracılı SaaS desenleri
description: Azure bulut ortamında çalışan hizmet (SaaS) veritabanı uygulamaları olarak çok kiracılı yazılımların gereksinimleri ve yaygın veri mimarisi desenleri hakkında bilgi edinin.
services: sql-database
ms.service: sql-database
ms.subservice: scenario
ms.topic: conceptual
author: MightyPen
ms.author: genemi
ms.reviewer: billgib, sstein
ms.date: 01/25/2019
ms.custom: seoapril2019
ms.openlocfilehash: 956d74467c69d9924d26f9cae8d902a6ddd84496
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80067497"
---
# <a name="multi-tenant-saas-database-tenancy-patterns"></a>Çok kiracılı SaaS veritabanı kira desenleri

Bu makalede, çok kiracılı SaaS uygulaması için kullanılabilir çeşitli kira modelleri açıklanmaktadır.

Çok kiracılı bir SaaS uygulaması tasarlarken, uygulamanızın gereksinimlerine en uygun kira modelini dikkatle seçmelisiniz.  Kira modeli, her kiracının verilerinin depolama alanına nasıl eşleniyi belirler.  Kira modeli seçiminiz uygulama tasarımı nı ve yönetimini etkiler.  Daha sonra farklı bir modele geçmek bazen maliyetlidir.

## <a name="a-saas-concepts-and-terminology"></a>A. SaaS kavramları ve terminolojisi

Hizmet Olarak Yazılım (SaaS) modelinde, şirketiniz yazılımınıza *lisans* satmaz. Bunun yerine, her müşteri şirketinize kira ödemeleri yaparak her müşteriyi şirketinizin *kiracısı* yapar.

Kira ödeme karşılığında, her kiracı SaaS uygulama bileşenlerinize erişebilir ve verileri SaaS sisteminde depolanır.

Kira *modeli* terimi, kiracıların depolanan verilerinin nasıl düzenlendiğini ifade eder:

- *Tek kira:* &nbsp; Her veritabanı verileri yalnızca bir kiracıdan depolar.
- *Çoklu kira:* &nbsp; Her veritabanı verileri birden çok ayrı kiracıdan (veri gizliliğini koruma mekanizmalarıyla) depolar.
- Hibrid kira modelleri de mevcuttur.

## <a name="b-how-to-choose-the-appropriate-tenancy-model"></a>B. Uygun kira modeli nasıl seçilir?

Genel olarak, kira modeli bir uygulamanın işlevini etkilemez, ancak büyük olasılıkla genel çözümün diğer yönlerini etkiler.  Modellerin her birini değerlendirmek için aşağıdaki ölçütler kullanılır:

- **Ölçeklenebilir -lik:**
    - Kiracı sayısı.
    - Kiracı başına depolama.
    - Toplu depolama.
    - Iş yük -ünü.

- **Kiracı yalıtımı:** &nbsp; Veri yalıtımı ve performansı (bir kiracının iş yükünün başkalarını etkileyip etkilemediği).

- **Kiracı başına maliyet:** &nbsp; Veritabanı maliyetleri.

- **Geliştirme karmaşıklığı:**
    - Şema değişiklikleri.
    - Sorgularda değişiklikler (desen tarafından gereklidir).

- **Operasyonel karmaşıklık:**
    - Performansı izleme ve yönetme.
    - Şema yönetimi.
    - Kiracıyı geri geri.
    - Olağanüstü durum kurtarma.

- **Özelleştirilebilirlik:** &nbsp; Kiracıya özgü veya kiracı sınıfına özgü şema özelleştirmelerini destekleme kolaylığı.

Kira tartışması *veri* katmanına odaklanmıştır.  Ama bir an için *uygulama* katmanı düşünün.  Uygulama katmanı yekpare bir varlık olarak kabul edilir.  Uygulamayı birçok küçük bileşene bölerseniz, kira modeli seçiminiz değişebilir.  Bazı bileşenlere hem kira hem de kullanılan depolama teknolojisi veya platform açısından diğerlerinden farklı davranabilirsiniz.

## <a name="c-standalone-single-tenant-app-with-single-tenant-database"></a>C. Tek kiracıveritabanı ile tek başına tek kiracı uygulaması

#### <a name="application-level-isolation"></a>Uygulama düzeyi yalıtımı

Bu modelde, uygulamanın tamamı her kiracı için bir kez tekrar tekrar yüklenir.  Uygulamanın her örneği bağımsız bir örnektir, bu nedenle hiçbir zaman başka bir bağımsız örnekle etkileşime girmez.  Uygulamanın her örneğinin yalnızca bir kiracısı vardır ve bu nedenle yalnızca bir veritabanıgerekir.  Kiracı veritabanını kendine ait.

![Tam olarak tek kiracılı veritabanı ile bağımsız uygulama tasarımı.][image-standalone-app-st-db-111a]

Her uygulama örneği ayrı bir Azure kaynak grubunda yüklenir.  Kaynak grubu, yazılım satıcısına veya kiracıya ait bir aboneye ait olabilir.  Her iki durumda da, satıcı kiracı için yazılım yönetebilirsiniz.  Her uygulama örneği, ilgili veritabanına bağlanmak üzere yapılandırılır.

Her kiracı veritabanı tek bir veritabanı olarak dağıtılır.  Bu model en büyük veritabanı yalıtımı sağlar.  Ancak yalıtım, en yüksek yüklerini işlemek için her veritabanına yeterli kaynak ayrılmasını gerektirir.  Burada, elastik havuzların farklı kaynak gruplarında dağıtılan veritabanları veya farklı abonelikler için kullanılamayacağı önemlidir.  Bu sınırlama, bu bağımsız tek kiracılı uygulama modelini genel veritabanı maliyet perspektifinden en pahalı çözüm haline getirir.

#### <a name="vendor-management"></a>Satıcı yönetimi

Satıcı, uygulama örnekleri farklı kiracı aboneliklerinde yüklü olsa bile, tüm bağımsız uygulama örneklerindeki tüm veritabanlarına erişebilir.  Erişim SQL bağlantıları üzerinden elde edilir.  Bu çapraz örnek erişimi, satıcının raporlama veya analiz amacıyla şema yönetimini ve veritabanı arası sorgusunu merkezileştirmesini sağlayabilir.  Bu tür merkezi leştirilmiş yönetim isteniyorsa, kiracı tanımlayıcılarını veritabanı UrI'lerine eşleyen bir katalog dağıtılmalıdır.  Azure SQL Veritabanı, bir katalog sağlamak için bir SQL veritabanıyla birlikte kullanılan bir parçalama kitaplığı sağlar.  Parçalama kitaplığı resmen [Elastik Veritabanı İstemci Kitaplığı][docu-elastic-db-client-library-536r]olarak adlandırılır.

## <a name="d-multi-tenant-app-with-database-per-tenant"></a>D. Veritabanı başına kiracı ile çok kiracılı uygulama

Sonraki desen, tümü tek kiracıveritabanları olan birçok veritabanlarıiçeren çok kiracılı bir uygulama kullanır.  Her yeni kiracı için yeni bir veritabanı sağlanmıştır.  Uygulama katmanı, düğüm başına daha fazla kaynak eklenerek *dikey* olarak ölçeklendirilir.  Veya uygulama daha fazla düğüm ekleyerek *yatay* olarak ölçeklendirilir.  Ölçeklendirme iş yükünü temel alıyor ve tek tek veritabanlarının sayısından veya ölçeğinden bağımsızdır.

![Çok kiracılı uygulamanın, kiracı başına veritabanına sahip tasarımı.][image-mt-app-db-per-tenant-132d]

#### <a name="customize-for-a-tenant"></a>Kiracı için özelleştirme

Bağımsız uygulama deseni gibi, tek kiracıveritabanlarının kullanımı da güçlü kiracı yalıtımı sağlar.  Modeli yalnızca tek kiracıveritabanlarını belirten herhangi bir uygulamada, belirli bir veritabanıiçin şema özelleştirilebilir ve kiracı için optimize edilebilir.  Bu özelleştirme, uygulamadaki diğer kiracıları etkilemez. Belki de bir kiracının tüm kiracıların ihtiyaç duyduğu temel veri alanlarının ötesinde verilere ihtiyacı olabilir.  Ayrıca, ek veri alanı nın bir dizin eki gerekebilir.

Kiracı başına veritabanında, şemayı bir veya daha fazla kiracı için özelleştirmek kolaydır.  Uygulama satıcısı, şema özelleştirmelerini ölçekte dikkatle yönetmek için yordamlar tasarlamalıdır.

#### <a name="elastic-pools"></a>Esnek havuzlar

Veritabanları aynı kaynak grubunda dağıtıldığında, elastik havuzlar halinde gruplandırılabilir.  Havuzlar, kaynakları birçok veritabanları arasında paylaşmanın uygun maliyetli bir yolunu sağlar.  Bu havuz seçeneği, her veritabanının deneyimlenen kullanım zirvelerini barındıracak kadar büyük olmasını gerektirmekten daha ucuzdur.  Havuza giren veritabanları kaynaklara erişimi paylaşsa da, yine de yüksek derecede performans yalıtımı elde edebilirler.

![Elastik havuz kullanarak, kiracı başına veritabanı ile çok kiracılı uygulama tasarımı.][image-mt-app-db-per-tenant-pool-153p]

Azure SQL Veritabanı, paylaşımı yapılandırmak, izlemek ve yönetmek için gereken araçları sağlar.  Hem havuz düzeyinde hem de veritabanı düzeyinde performans ölçümleri Azure portalında ve Azure Monitor günlüklerinde kullanılabilir.  Ölçümler, hem toplu hem de kiracıya özgü performans hakkında harika bilgiler verebilir.  Belirli bir kiracıya ayrılmış kaynakları sağlamak için tek tek veritabanları havuzlar arasında taşınabilir.  Bu araçlar, uygun maliyetli bir şekilde iyi performans sağlamanızı sağlar.

#### <a name="operations-scale-for-database-per-tenant"></a>Kiracı başına veritabanı için işlem ölçeği

Azure SQL Veritabanı platformu, 100.000'den fazla veritabanı gibi çok sayıda veritabanını ölçekte yönetmek üzere tasarlanmış birçok yönetim özelliğine sahiptir.  Bu özellikler, kiracı başına veritabanı deseni akla yatkın hale getirin.

Örneğin, bir sistemin tek bir veritabanı olarak 1000 kiracılı bir veritabanı olduğunu varsayalım.  Veritabanında 20 dizin olabilir.  Sistem 1000 tek kiracıveritabanlarına sahip olursa, dizin miktarı 20.000'e yükselir.  SQL Veritabanı'nda [Otomatik ayar][docu-sql-db-automatic-tuning-771a]ın bir parçası olarak, otomatik dizin oluşturma özellikleri varsayılan olarak etkinleştirilir.  Otomatik dizin oluşturma, tüm 20.000 dizinve bunların devam eden oluşturma ve bırakma optimizasyonlarını sizin için yönetir.  Bu otomatik eylemler tek bir veritabanı içinde oluşur ve diğer veritabanlarındaki benzer eylemlerle koordine edilmez veya kısıtlanmaz.  Otomatik dizin oluşturma, yoğun bir veritabanında dizinleri daha az meşgul bir veritabanından farklı şekilde işler.  Bu büyük yönetim görevinin el ile yapılması gerekiyorsa, bu tür dizin yönetimi özelleştirmesi, kiracı başına veritabanı ölçeğinde pratik olmaz.

İyi ölçeklendirilen diğer yönetim özellikleri şunlardır:

- Dahili yedeklemeler.
- Yüksek kullanılabilirlik.
- Disk şifreleme.
- Performans telemetrisi.

#### <a name="automation"></a>Automation

Yönetim işlemleri komut dosyası olabilir ve [bir devops][http-visual-studio-devops-485m] modeli ile sunulabilir.  Operasyonlar bile otomatik ve uygulamada maruz olabilir.

Örneğin, tek bir kiracının kurtarılmasını daha önceki bir noktaya otomatikleştirebilirsiniz.  Kurtarma yalnızca kiracıyı depolayan tek kiracılı veritabanını geri yüklemesi gerekir.  Bu geri yüklemenin diğer kiracılar üzerinde hiçbir etkisi yoktur, bu da yönetim işlemlerinin her bir kiracının ince parçalı düzeyinde olduğunu doğrular.

## <a name="e-multi-tenant-app-with-multi-tenant-databases"></a>E. Çok kiracılı veritabanlarına sahip çok kiracılı uygulama

Başka bir kullanılabilir desen çok kiracı veritabanında birçok kiracı depolamaktır.  Uygulama örneğinde herhangi bir sayıda çok kiracılı veritabanları olabilir.  Çok kiracılı veritabanışeması, belirli bir kiracıdan gelen verilerin seçici olarak alınabilmesi için bir veya daha fazla kiracı tanımlayıcı sütununa sahip olmalıdır.  Ayrıca, şema yalnızca kiracı bir alt kümesi tarafından kullanılan birkaç tablo veya sütun gerektirebilir.  Ancak, statik kod ve başvuru verileri yalnızca bir kez depolanır ve tüm kiracılar tarafından paylaşılır.

#### <a name="tenant-isolation-is-sacrificed"></a>Kiracı izolasyonkurban edilir

*Veri:* &nbsp; Çok kiracılı bir veritabanı mutlaka kiracı yalıtımı feda eder.  Birden çok kiracının verileri tek bir veritabanında birlikte depolanır.  Geliştirme sırasında, sorguların hiçbir zaman birden fazla kiracıdan gelen verileri açığa çıkarmadığından emin olun.  SQL Veritabanı, sorgudan döndürülen verilerin tek bir kiracıya kapsamını genişletmesini zorunlu kınlayan [satır düzeyinde güvenliği][docu-sql-svr-db-row-level-security-947w]destekler.

*İşleme:* &nbsp; Çok kiracılı veritabanı, tüm kiracıları arasında bilgi işlem ve depolama kaynaklarını paylaşır.  Veritabanı bir bütün olarak kabul edilebilir bir şekilde performans gösterdiğinden emin olmak için izlenebilir.  Ancak, Azure sisteminin bu kaynakların tek bir kiracı tarafından kullanımını izlemek veya yönetmek için yerleşik bir yolu yoktur.  Bu nedenle, çok kiracılı veritabanı, aşırı etkin bir kiracının iş yükünün aynı veritabanındaki diğer kiracıların performans deneyimini etkilediği gürültülü komşularla karşılaşma riskini taşır.  Ek uygulama düzeyinde izleme kiracı düzeyinde performansı izleyebilir.

#### <a name="lower-cost"></a>Daha düşük maliyet

Genel olarak, çok kiracılı veritabanları en düşük kiracı başına maliyete sahiptir.  Tek bir veritabanı için kaynak maliyetleri, eşit büyüklükteki bir elastik havuzdan daha düşüktür.  Ayrıca, kiracıların yalnızca sınırlı depolama alanına ihtiyaç duyduğu senaryolarda, milyonlarca kiracı tek bir veritabanında depolanabilir.  Hiçbir elastik havuz milyonlarca veritabanı içeremez.  Ancak, havuz başına 1000 veritabanları içeren bir çözüm, 1000 havuzları ile, yönetmek için hantal olma riski milyonlarca ölçeğine ulaşabilir.

Çok kiracılı veritabanı modelinin iki varyasyonu, parçalanmış çok kiracılı modelin en esnek ve ölçeklenebilir olmasıyla, aşağıdakiler üzerinde tartışılır.

## <a name="f-multi-tenant-app-with-a-single-multi-tenant-database"></a>F. Tek bir çok kiracılı veritabanına sahip çok kiracılı uygulama

En basit çok kiracılı veritabanı deseni, tüm kiracılar için verileri barındırmak için tek bir veritabanı kullanır.  Daha fazla kiracı eklendikçe, veritabanı daha fazla depolama ve işlem kaynaklarıyla ölçeklendirilir.  Her zaman nihai bir ölçek sınırı olmasına rağmen, bu ölçek kadar gerekli olan tüm olabilir.  Ancak, bu sınıra ulaşılmadan çok önce veritabanı yönetmek için hantal hale gelir.

Tek tek kiracılara odaklanan yönetim işlemleri, çok kiracılı bir veritabanında uygulanması daha karmaşıktır.  Ve ölçekte bu operasyonlar kabul edilemez yavaş olabilir.  Bir örnek, yalnızca bir kiracı için verilerin zamanında geri yüklenir.

## <a name="g-multi-tenant-app-with-sharded-multi-tenant-databases"></a>G. Kırık çok kiracılı veritabanları ile çok kiracılı uygulama

Çoğu SaaS uygulaması aynı anda yalnızca bir kiracının verilerine erişir.  Bu erişim deseni, kiracı verilerinin birden çok veritabanları veya kırıklar arasında dağıtılmasını sağlar ve burada herhangi bir kiracıya ait tüm veriler tek parça içinde bulunur.  Çok kiracılı veritabanı deseni yle birlikte, parçalanmış bir model neredeyse sınırsız ölçek sağlar.

![Çok kiracılı uygulamanın, çok kiracılı veritabanlarının basılı tasarımı.][image-mt-app-sharded-mt-db-174s]

#### <a name="manage-shards"></a>Parçaları yönetme

Sharding hem tasarıma hem de operasyonel yönetime karmaşıklık katar.  Kiracılar ve veritabanları arasındaki eşleşme yi korumak için bir katalog gereklidir.  Buna ek olarak, yönetim prosedürleri parçaları ve kiracı nüfusu yönetmek için gereklidir.  Örneğin, yordamlar kırıkları eklemek ve kaldırmak ve kiracı verilerini kırıklar arasında taşımak için tasarlanmalıdır.  Ölçeklendirmenin bir yolu, yeni bir parça ekleyerek ve onu yeni kiracılarla doldurmaktır.  Diğer zamanlarda, yoğun nüfuslu bir parçayı daha az nüfuslu iki parçaya bölebilirsiniz.  Birkaç kiracı taşındıktan veya durdurulduktan sonra, seyrek nüfuslu parçaları bir araya getirebilirsiniz.  Birleştirme, daha uygun maliyetli kaynak kullanımına neden olur.  Kiracılar, iş yüklerini dengelemek için kırıklar arasında da taşınabilir.

SQL Veritabanı, parçalama kitaplığı ve katalog veritabanıyla birlikte çalışan bir birleştirme aracı sağlar.  Sağlanan uygulama parçaları bölebilir ve birleştirir ve kiracı verilerini kırıklar arasında taşıyabilir.  Uygulama ayrıca bu işlemler sırasında kataloğu korur ve etkilenen kiracıları taşımadan önce çevrimdışı olarak işaretler.  Hareketten sonra, uygulama kataloğu yeni eşlemeyle yeniden güncelleştirir ve kiracıyı yeniden çevrimiçi olarak işaretler.

#### <a name="smaller-databases-more-easily-managed"></a>Daha küçük veritabanları daha kolay yönetilir

Kiracıları birden çok veritabanlarına dağıtarak, parçalanan çok kiracılı çözüm, daha kolay yönetilen daha küçük veritabanlarıyla sonuçlanır.  Örneğin, belirli bir kiracıyı zaman içinde önceki bir noktaya geri getirmek, artık tüm kiracıları içeren daha büyük bir veritabanı yerine, bir yedeklemeden tek bir küçük veritabanını geri oluşturmayı içerir. Veritabanı boyutu ve veritabanı başına kiracı sayısı, iş yükünü ve yönetim çabalarını dengelemek için seçilebilir.

#### <a name="tenant-identifier-in-the-schema"></a>Şemada kiracı tanımlayıcısı

Kullanılan parçalama yaklaşımına bağlı olarak, veritabanı şemasına ek kısıtlamalar empoze edilebilir.  SQL Veritabanı bölme/birleştirme uygulaması, şema genellikle kiracı tanımlayıcısı olan parçalama anahtarını içerir gerektirir.  Kiracı tanımlayıcısı, tüm kırık tabloların birincil anahtarının önde gelen öğesidir.  Kiracı tanımlayıcısı, bölme/birleştirme uygulamasının belirli bir kiracıyla ilişkili verileri hızla bulmasını ve taşımasını sağlar.

#### <a name="elastic-pool-for-shards"></a>Kırıklar için elastik havuz

Kırık çok kiracılı veritabanları elastik havuzlara yerleştirilebilir.  Genel olarak, bir havuzda birçok tek kiracı veritabanları olması olarak maliyet olarak birkaç çok kiracı veritabanlarında birçok kiracı sahip olarak verimlidir.  Çok sayıda etkin olmayan kiracı olduğunda çok kiracılı veritabanları avantajlıdır.

## <a name="h-hybrid-sharded-multi-tenant-database-model"></a>H. Hibrit kırık çok kiracılı veritabanı modeli

Karma modelde, tüm veritabanlarında şemalarında kiracı tanımlayıcısı vardır.  Veritabanlarının tümü birden fazla kiracıyı depolama yeteneğine sahiptir ve veritabanları parçalanabilir.  Yani şema anlamda, hepsi çok kiracılı veritabanları.  Ancak uygulamada bu veritabanlarından bazıları yalnızca bir kiracı içerir.  Ne olursa olsun, belirli bir veritabanında depolanan kiracı miktarı veritabanı şeması üzerinde hiçbir etkisi yoktur.

#### <a name="move-tenants-around"></a>Kiracıları hareket ettirin

İstediğiniz zaman, belirli bir kiracıyı kendi çok kiracıveritabanına taşıyabilirsiniz.  Ve istediğiniz zaman fikrinizi değiştirebilir ve kiracıyı birden çok kiracı içeren bir veritabanına geri taşıyabilirsiniz.  Ayrıca, yeni veritabanını sağlarken yeni tek kiracı veritabanına kiracı atayabilirsiniz.

Tanımlanabilir kiracı gruplarının kaynak gereksinimleri arasında büyük farklar olduğunda hibrit model parlar.  Örneğin, ücretsiz deneme sürümüne katılan kiracıların, kiracılara abone olan yüksek performans düzeyinin garanti olmadığını varsayalım.  İlke, ücretsiz deneme aşamasındaki kiracıların, tüm ücretsiz deneme kiracıları arasında paylaşılan çok kiracılı bir veritabanında depolanması olabilir.  Ücretsiz deneme kiracısı temel hizmet katmanına abone olduğunda, kiracı daha az kiracıya sahip olabilecek başka bir çok kiracı veritabanına taşınabilir.  Premium hizmet katmanı için ödeme yapan bir abone, kendi yeni tek kiracıveritabanına taşınabilir.

#### <a name="pools"></a>Havuzlar

Bu karma modelde, abone kiracılar için tek kiracı veritabanları, kiracı başına veritabanı maliyetlerini azaltmak için kaynak havuzlarına yerleştirilebilir.  Bu, kiracı başına veritabanı modelinde de yapılır.

## <a name="i-tenancy-models-compared"></a>I. Kira modelleri karşılaştırıldığında

Aşağıdaki tabloda ana kira modelleri arasındaki farklar özetlenmiştir.

| Ölçüm | Tek başına uygulama | Kiracı başına veritabanı | Kırık çok kiracı |
| :---------- | :------------- | :------------------ | :------------------- |
| Ölçek | Orta<br />1-100'lü arası | Çok yüksek <br />1-100.000'ler | Sınırsız<br />1-1.000.000'ler arası |
| Kiracı yalıtımı | Çok yüksek  | Yüksek | Düşük; herhangi bir tek kiracı (bir MT db yalnız) dışında. |
| Kiracı başına veritabanı maliyeti | Yüksek; zirveleri için boyutlandırılır. | Düşük; kullanılan havuzlar. | En düşük, MT DBs küçük kiracılar için. |
| Performans izleme ve yönetimi | Yalnızca kiracı başına | Toplam + kiracı başına | Toplam; her ne kadar sadece bekarlar için kiracı başına. |
| Geliştirme karmaşıklığı | Düşük | Düşük | Orta; kırık nedeniyle. |
| Operasyonel karmaşıklık | Düşük-yüksek. Tek tek basit, ölçekte karmaşık. | Düşük-Orta. Desenler ölçekte karmaşıklığı ele. | Düşük-yüksek. Bireysel kiracı yönetimi karmaşıktır. |
| &nbsp; ||||

## <a name="next-steps"></a>Sonraki adımlar

- [Çok kiracılı Wingtip uygulamasını dağıtın ve keşfedin kiracı başına veritabanı SaaS modelini kullanır - Azure SQL Veritabanı][docu-sql-db-saas-tutorial-deploy-wingtip-db-per-tenant-496y]

- [Wingtip Biletleri örnek SaaS Azure SQL Veritabanı kiralama uygulamasına hoş geldiniz][docu-saas-tenancy-welcome-wingtip-tickets-app-384w]


<!--  Article link references.  -->

[http-visual-studio-devops-485m]: https://www.visualstudio.com/devops/

[docu-sql-svr-db-row-level-security-947w]: https://docs.microsoft.com/sql/relational-databases/security/row-level-security

[docu-elastic-db-client-library-536r]: sql-database-elastic-database-client-library.md
[docu-sql-db-saas-tutorial-deploy-wingtip-db-per-tenant-496y]: sql-database-saas-tutorial.md
[docu-sql-db-automatic-tuning-771a]: sql-database-automatic-tuning.md
[docu-saas-tenancy-welcome-wingtip-tickets-app-384w]: saas-tenancy-welcome-wingtip-tickets-app.md


<!--  Image references.  -->

[image-standalone-app-st-db-111a]: media/saas-tenancy-app-design-patterns/saas-standalone-app-single-tenant-database-11.png "Tam olarak tek kiracılı veritabanı ile bağımsız uygulama tasarımı."

[image-mt-app-db-per-tenant-132d]: media/saas-tenancy-app-design-patterns/saas-multi-tenant-app-database-per-tenant-13.png "Çok kiracılı uygulamanın, kiracı başına veritabanına sahip tasarımı."

[image-mt-app-db-per-tenant-pool-153p]: media/saas-tenancy-app-design-patterns/saas-multi-tenant-app-database-per-tenant-pool-15.png "Elastik havuz kullanarak, kiracı başına veritabanı ile çok kiracılı uygulama tasarımı."

[image-mt-app-sharded-mt-db-174s]: media/saas-tenancy-app-design-patterns/saas-multi-tenant-app-sharded-multi-tenant-databases-17.png "Çok kiracılı uygulamanın, çok kiracılı veritabanlarının basılı tasarımı."

