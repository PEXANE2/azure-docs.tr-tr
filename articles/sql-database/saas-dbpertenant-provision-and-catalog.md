---
title: Çok kiracılı bir uygulamada yeni kiracılar sağlama
description: Azure SQL Veritabanı çok kiracılı SaaS uygulamasında yeni kiracıları nasıl sağlayıp kataloglayıştınız öğrenin
services: sql-database
ms.service: sql-database
ms.subservice: scenario
ms.custom: seo-lt-2019
ms.devlang: ''
ms.topic: conceptual
author: stevestein
ms.author: sstein
ms.reviewer: ''
ms.date: 09/24/2018
ms.openlocfilehash: 6ec8f8835e925663fc6ac21a6eb1df09d6927109
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74132112"
---
# <a name="learn-how-to-provision-new-tenants-and-register-them-in-the-catalog"></a>Yeni kiracıları nasıl temin edin ve katalogda kaydetmeyi öğrenin

Bu eğitimde, SaaS desenlerini nasıl sağlayıp katalogladığınızı öğreneceksiniz. Ayrıca Wingtip Tickets SaaS veritabanı-kiracı başına uygulamada nasıl uygulandığını da öğrenirsiniz. Yeni kiracı veritabanları oluşturur ve başlatır ve bunları uygulamanın kiracı kataloğuna kaydedersiniz. Katalog, SaaS uygulamasının birçok kiracısı ve verileri arasındaki eşlemi koruyan bir veritabanıdır. Katalog, uygulama ve yönetim isteklerini doğru veritabanına yönlendirmede önemli bir rol oynar.

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
>
> * Tek bir yeni kiracı sağlama.
> * Ek kiracılar bir toplu sağlama.


Bu öğreticiyi tamamlamak için aşağıdaki ön koşulların karşılandığından emin olun:

* Wingtip Tickets SaaS veritabanı başına kiracı uygulaması dağıtılır. Beş dakikadan kısa bir süre içinde dağıtmak için, [Wingtip Tickets SaaS veritabanı başına kiracı uygulamasını dağıt'a](saas-dbpertenant-get-started-deploy.md)bakın ve keşfedin.
* Azure PowerShell’in yüklendiğinden. Daha fazla bilgi için bkz. [Azure PowerShell kullanmaya başlayın](https://docs.microsoft.com/powershell/azure/get-started-azureps).

## <a name="introduction-to-the-saas-catalog-pattern"></a>SaaS katalog desenine giriş

Veritabanı destekli çok kiracılı SaaS uygulamasında, her kiracının bilgilerinin nerede depolandığı önemlidir. SaaS katalog deseninde, her kiracı ile verilerinin depolandığı veritabanı arasındaki eşlemi tutmak için bir katalog veritabanı kullanılır. Bu desen, kiracı verileri birden çok veritabanına dağıtıldığında geçerlidir.

Her kiracı, veritabanının konumuna eşlenen katalogdaki bir anahtarla tanımlanır. Wingtip Biletleri uygulamasında anahtar, kiracının adının bir karmasından oluşur. Bu şema, uygulamanın uygulama URL'sinde yer alan kiracı adından anahtarı oluşturmasına olanak tanır. Diğer kiracı anahtar düzenleri kullanılabilir.

Katalog, veritabanının adının veya konumunun uygulama üzerinde en az etkiyle değiştirilmesine izin verir. Çok kiracılı veritabanı modelinde, bu özellik aynı zamanda bir kiracıyı veritabanları arasında taşımayı da barındırır. Katalog, kiracının veya veritabanının bakım veya diğer eylemler için çevrimdışı olup olmadığını belirtmek için de kullanılabilir. Bu [özellik, geri yükleme tek kiracı öğretici](saas-dbpertenant-restore-single-tenant.md)araştırılır.

Katalog ayrıca, kiracılara sunulan şema sürümü, hizmet planı veya SLA'lar gibi ek kiracı veya veritabanı meta verilerini de depolayabilir. Katalog, uygulama yönetimi, müşteri desteği veya DevOps sağlayan diğer bilgileri depolayabilir.

Katalog, SaaS uygulamasının ötesinde veritabanı araçlarını etkinleştirebilir. Wingtip Tickets SaaS veritabanı-per-tenant örnek, katalog Geçici [raporlama öğreticisinde](saas-tenancy-cross-tenant-reporting.md)incelenir çapraz kiracı sorgusu etkinleştirmek için kullanılır. Veritabanı arası iş [yönetimi, Şema yönetimi](saas-tenancy-schema-management.md) ve [Kiracı analiz](saas-tenancy-tenant-analytics.md) eğitimlerinde incelenir.

Wingtip Tickets SaaS örneklerinde, katalog [Elastik Veritabanı istemci kitaplığı (EDCL)](sql-database-elastic-database-client-library.md)Shard Management özellikleri kullanılarak uygulanır. EDCL Java ve .NET Framework mevcuttur. EDCL, bir uygulamanın veritabanı destekli parça haritası oluşturmasına, yönetmesine ve kullanmasını sağlar.

Parçalı harita, parçaların (veritabanlarının) ve anahtarlar (kiracılar) ve kırıklar arasındaki eşlemenin bir listesini içerir. EDCL işlevleri, parça lı haritadaki girişleri oluşturmak için kiracı sağlama sırasında kullanılır. Doğru veritabanına bağlanmak için uygulamalar tarafından çalışma zamanında kullanılırlar. EDCL, katalog veritabanına gelen trafiği en aza indirmek ve uygulamayı hızlandırmak için bağlantı bilgilerini önbelleğe alarak bağlantı bilgilerini önbelleğe getirir.

> [!IMPORTANT]
> Eşleme verilerine katalog veritabanında erişilebilir, ancak *bunu da birlikte atmayın.* Yalnızca Elastik Veritabanı İstemci Kitaplığı API'lerini kullanarak eşleme verilerini düzenleme. Eşleme verilerini doğrudan işlemek kataloğu bozarak risk taşır ve desteklenmez.


## <a name="introduction-to-the-saas-provisioning-pattern"></a>SaaS sağlama desenine giriş

Tek kiracıveritabanı modeli kullanan bir SaaS uygulamasına yeni bir kiracı eklediğinizde, yeni bir kiracı veritabanı sağlamanız gerekir. Veritabanı uygun konum ve hizmet katmanında oluşturulmalıdır. Ayrıca uygun şema ve referans verileri ile başharfe alınmalıdır. Ve uygun kiracı anahtarı altında katalogda kayıtlı olmalıdır.

Veritabanı sağlama için farklı yaklaşımlar kullanılabilir. SQL komut dosyalarını yürütebilir, bir bacpac dağıtabilir veya şablon veritabanını kopyalayabilirsiniz.

Veritabanı sağlama şema yönetim stratejinizin bir parçası olması gerekir. Yeni veritabanlarının en son şema ile birlikte sağlanmıştır emin olmalısınız. Bu gereksinim [Şema yönetimi öğreticisinde](saas-tenancy-schema-management.md)incelenir.

Wingtip Tickets veritabanı-kiracı başına uygulama hükümleri, katalog sunucusunda dağıtılan _basetenantdb_adlı bir şablon veritabanı kopyalayarak yeni kiracılar. Sağlama, kayıt deneyiminin bir parçası olarak uygulamaya entegre edilebilir. Ayrıca komut dosyaları kullanılarak çevrimdışı olarak da desteklenebilir. Bu öğretici PowerShell kullanarak sağlama yı inceler.

Komut dosyaları nın sağlanması, elastik bir havuzda yeni bir kiracı veritabanı oluşturmak için _basetenantdb_ veritabanını kopyalar. Kiracı veritabanı, _yeni kiracı_ DNS diğer adıyla eşlenen kiracı sunucusunda oluşturulur. Bu diğer ad, yeni kiracılar sağlamak için kullanılan sunucuya bir başvuru tutar ve olağanüstü durum kurtarma öğreticiler bir kurtarma kiracı sunucuya işaret etmek için güncelleştirilir[(DR georestore kullanarak](saas-dbpertenant-dr-geo-restore.md), [DR jeoçoğaltma kullanarak).](saas-dbpertenant-dr-geo-replication.md) Komut dosyaları daha sonra kiracıya özgü bilgilerle veritabanını açar ve katalog parça haritasına kaydeder. Kiracı veritabanlarına, kiracı adına göre adverilir. Bu adlandırma düzeni desenin önemli bir parçası değildir. Katalog, herhangi bir adlandırma kuralının kullanılabileceğini, böylece kiracı anahtarını veritabanı adının eşlenebleri.


## <a name="get-the-wingtip-tickets-saas-database-per-tenant-application-scripts"></a>Wingtip Tickets SaaS veritabanı-kiracı başına uygulama komut dosyalarını edinin

Wingtip Tickets SaaS komut dosyaları ve uygulama kaynak kodu [WingtipTicketsSaaS-DbPerTenant](https://github.com/Microsoft/WingtipTicketsSaaS-DbPerTenant) GitHub repo mevcuttur. Wingtip Tickets SaaS komut dosyalarını indirmek ve engelini kaldırmak için gereken adımlar için [genel kılavuza](saas-tenancy-wingtip-app-guidance-tips.md) göz atın.


## <a name="provision-and-catalog-detailed-walkthrough"></a>Sağlama ve kataloğa kaydetme ile ilgili ayrıntılı kılavuz

Wingtip Biletleri uygulamasının yeni kiracı sağlama uygulamasını anlamak için, bir kesme noktası ekleyin ve kiracı sağlarken iş akışını izleyin.

1. PowerShell ISE, açık ... \\Öğrenme Modülleri\\HükmüAndCatalog\\_Demo-ProvisionAndCatalog.ps1_ ve aşağıdaki parametreleri ayarlayın:

   * **$TenantName** = yeni mekanın adı (örneğin, *Bushwillow Blues*).
   * **$VenueType** = önceden tanımlanmış mekan türlerinden biri: _blues, klasik müzik, dans, caz, judo, motor yarışı, çok amaçlı, opera, rockmüzik, futbol_.
   * **$DemoScenario** = **1**, Tek bir *kiracı hükmü*.

2. Bir kesme noktası eklemek için imlecinizi *Yeni Kiracı*' yazan satırın herhangi bir yerine koyun. Sonra F9 tuşuna basın.

   ![Kesme noktası](media/saas-dbpertenant-provision-and-catalog/breakpoint.png)

3. Komut dosyasını çalıştırmak için F5 tuşuna basın.

4. Komut dosyası yürütme kesme noktasında durduktan sonra, koda adım atmak için F11 tuşuna basın.

   ![Hata Ayıklama](media/saas-dbpertenant-provision-and-catalog/debug.png)



**Hata Ayıklama** menüsü seçeneklerini kullanarak komut dosyasının yürütülmesini izleyin. Çağrılan işlevlerin üzerine veya içine adım atmak için F10 ve F11 tuşuna basın. PowerShell komut dosyalarının hata ayıklanması hakkında daha fazla bilgi için [PowerShell komut dosyalarıyla çalışma ve hata ayıklama hakkındaki ipuçlarına](https://docs.microsoft.com/powershell/scripting/components/ise/how-to-debug-scripts-in-windows-powershell-ise)bakın.


Bu iş akışını açıkça izlemeniz gerekmez. Komut dosyasının nasıl hata ayıklanılmasını açıklar.

* **CatalogAndDatabaseManagement.psm1 modüllerini içe aktarın.** [Shard Management](sql-database-elastic-scale-shard-map-management.md) işlevleri üzerinde bir katalog ve kiracı düzeyinde soyutlama sağlar. Bu modül katalog deseninin çoğunu kapsüller ve keşfetmeye değer.
* **SubscriptionManagement.psm1 modüllerini içeri aktarın.** Azure'da oturum açmak ve çalışmak istediğiniz Azure aboneliğini seçmek için işlevler içerir.
* **Yapılandırma ayrıntılarını alın.** F11 kullanarak Get-Configuration'a adım atın ve uygulama config'inin nasıl belirtildiğini görün. Kaynak adları ve uygulamaya özgü diğer değerler burada tanımlanır. Komut dosyalarını öğrenene kadar bu değerleri değiştirmeyin.
* **Katalog nesnesini alın.** Üst düzey komut dosyasında kullanılan bir katalog nesnesini oluşturan ve döndüren Get-Catalog'a adım atın. Bu işlev, **AzureShardManagement.psm1'den**alınan Shard Management işlevlerini kullanır. Katalog nesnesi aşağıdaki öğelerden oluşur:

   * $catalogServerFullyQualifiedName standart kök artı kullanıcı adı kullanılarak oluşturulur: _katalog-\<kullanıcı\>.database.windows .net_.
   * $catalogDatabaseName, *tenantcatalog* yapılandırmasından alınır.
   * $shardMapManager nesnesi, katalog veritabanından başlatılır.
   * $shardMap nesnesi, katalog veritabanındaki _tenantcatalog_ parça eşlemesinden başlatılır. Katalog nesnesi oluşturulur ve döndürülür. Üst düzey komut dosyasında kullanılır.
* **Yeni kiracı anahtarını hesaplayın.** Kiracı adından kiracı anahtarı oluşturmak için bir karma işlevi kullanılır.
* **Kiracı anahtarının zaten var olup olmadığını denetleyin.** Anahtarın kullanılabilir olduğundan emin olmak için katalog işaretlenir.
* **Kiracı veritabanına New-TenantDatabase öğesi sağlanır.** [Azure Kaynak Yöneticisi şablonu](../azure-resource-manager/resource-manager-template-walkthrough.md)kullanarak veritabanının nasıl sağlanmış olduğunu öğrenmek için F11'i kullanın.

    Veritabanı adı, hangi parçanın hangi kiracıya ait olduğunu netleştirmek üzere kiracı adından oluşturulur. Diğer veritabanı adlandırma kurallarını da kullanabilirsiniz. Kaynak Yöneticisi şablonu, katalog sunucusunda bir şablon veritabanı _(baseTenantDB)_ kopyalayarak kiracı veritabanı oluşturur. Alternatif olarak, bir veritabanı oluşturabilir ve bir bacpac alarak onu başlatma. Veya tanınmış bir konumdan bir başlatma komut dosyası yürütebilirsiniz.

    Kaynak Yöneticisi şablonu ...\Öğrenme Modülleri\Ortak\ klasöründe: *tenantdatabasecopytemplate.json*

* **Kiracı veritabanı daha da başharfe kaydedilir.** Mekan (kiracı) adı ve mekan türü eklenir. Ayrıca burada diğer başlatma yapabilirsiniz.

* **Kiracı veritabanı katalogda kayıtlıdır.** Kiracı anahtarını kullanarak *Add-TenantDatabaseToCatalog'a* kaydedilir. Ayrıntıları görmek için F11 tuşunu kullanın:

    * Katalog veritabanı, parça eşlemesine eklenir (bilinen veritabanları listesi).
    * Anahtar değerini parçaya bağlayan eşleme oluşturulur.
    * Kiracı (mekanın adı) ile ilgili ek meta veriler katalogdaki Kiracılar tablosuna eklenir. Kiracılar tablosu Shard Management şemasının bir parçası değildir ve EDCL tarafından yüklenmez. Bu tablo, ek uygulamaya özgü verileri desteklemek için katalog veritabanının nasıl genişletilebileceğini göstermektedir.


Sağlama tamamlandıktan sonra, yürütme orijinal *Demo-ProvisionAndCatalog* komut dosyasına döner. **Etkinlikler** sayfası tarayıcıdaki yeni kiracı için açılır.

   ![Etkinlikler sayfası](media/saas-dbpertenant-provision-and-catalog/new-tenant.png)


## <a name="provision-a-batch-of-tenants"></a>Kiracıların bir toplu hükmü

Bu egzersiz 17 kiracı bir toplu madde yi karşılar. Diğer Wingtip Tickets SaaS veritabanı başına kiracı öğreticilerini başlatmadan önce bu kiracı toplu işlerini sağlamanızı öneririz. Çalışmak için sadece birkaç veritabanları daha vardır.

1. PowerShell ISE, açık ... \\Öğrenme Modülleri\\ProvizyonAndCatalog\\*Demo-ProvisionAndCatalog.ps1*. *$DemoScenario* parametresini 3 olarak değiştirin:

   * **$DemoScenario** = **3**, Madde kiracı *bir toplu*.
2. Komut dosyasını çalıştırmak için F5 tuşuna basın.

Betik, ek kiracı grubu dağıtır. Toplu iş ve temsilcilerin her veritabanının bağlı bir şablona sağlanmasını kontrol eden bir [Azure Kaynak Yöneticisi şablonu](../azure-resource-manager/resource-manager-template-walkthrough.md) kullanır. Şablonların bu şekilde kullanılması, Azure Resource Manager’ın betiğinizin sağlama işlemine aracılık etmesine olanak tanır. Şablonlar veritabanını paralel olarak sağlar ve gerekirse yeniden denemeleri işler. Komut dosyası idempotent, bu nedenle başarısız olursa veya herhangi bir nedenle durursa, yeniden çalıştırın.

### <a name="verify-the-batch-of-tenants-that-successfully-deployed"></a>Başarıyla dağıtılan kiracı toplu larını doğrulayın

* Azure [portalında](https://portal.azure.com)sunucu listenize göz atın ve *kiracı1* sunucusunu açın. **SQL veritabanlarını**seçin ve 17 ek veritabanının toplu olarak listede olduğunu doğrulayın.

   ![Veritabanı listesi](media/saas-dbpertenant-provision-and-catalog/database-list.png)



## <a name="other-provisioning-patterns"></a>Diğer sağlama düzenleri

Bu öğreticide yer almayan diğer sağlama kalıpları:

**Ön sağlama veritabanları**: Ön sağlama deseni, elastik havuzdaki veritabanlarının ekstra maliyet eklememesi durumundan yararlanır. Faturalandırma elastik havuz içindir, veritabanları için değil. Boşta kalan veritabanları kaynak tüketir. Veritabanlarını havuzda ön provizyon yaparak ve gerektiğinde ayırarak, kiracı ekleme süresini azaltabilirsiniz. Önceden sağlanan veritabanlarının sayısı, beklenen sağlama oranına uygun bir arabellek tutmak için gerektiğinde ayarlanabilir.

**Otomatik sağlama**: Otomatik sağlama deseninde, hizmet sağlama hizmeti sunucularını, havuzları ve veritabanlarını gerektiğinde otomatik olarak sağlar. İsterseniz, elastik havuzlarda ön sağlama veritabanları ekleyebilirsiniz. Veritabanları kullanımdan kaldırılıp silinirse, elastik havuzlarda boşluklar sağlama hizmeti tarafından doldurulabilir. Böyle bir hizmet, birden çok coğrafyada sağlama işlemini işlemek ve olağanüstü durum kurtarma için coğrafi çoğaltma kurmak gibi basit veya karmaşık olabilir.

Otomatik sağlama deseniyle, istemci başvurusu veya komut dosyası, sağlama hizmeti tarafından işlenecek bir kuyruğa sağlama isteği gönderir. Daha sonra tamamlanmasını belirlemek için hizmeti anketler. Ön sağlama kullanılırsa, istekler hızlı bir şekilde işlenir. Hizmet, arka planda yedek bir veritabanı nı karşılar.


## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
>
> * Tek bir yeni kiracı sağlama.
> * Ek kiracılar bir toplu sağlama.
> * Kiracı sağlama ve bunları kataloga kaydetme ayrıntılarına adım atın.

Performans [izleme öğreticisini](saas-dbpertenant-performance-monitoring.md)deneyin.

## <a name="additional-resources"></a>Ek kaynaklar

* [Wingtip Tickets SaaS veritabanı-kiracı başına uygulama üzerine inşa](saas-dbpertenant-wingtip-app-overview.md#sql-database-wingtip-saas-tutorials) ek öğreticiler
* [Elastik veritabanı istemci kitaplığı](sql-database-elastic-database-client-library.md)
* [Windows PowerShell ISE'de hata ayıklama komut dosyaları](https://docs.microsoft.com/powershell/scripting/components/ise/how-to-debug-scripts-in-windows-powershell-ise)
