---
title: SaaS çok kiracılı hüküm
description: Azure SQL Veritabanı çok kiracılı SaaS uygulamasında yeni kiracıları nasıl sağlayıp kataloglayışta nasil değerlendireceklerini öğrenin
services: sql-database
ms.service: sql-database
ms.subservice: scenario
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: MightyPen
ms.author: genemi
ms.reviewer: billgib,andrela,stein
ms.date: 09/24/2018
ms.openlocfilehash: 4ea18ee23d845b2d16209b23de14dc3cd70aaa59
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74133141"
---
# <a name="provision-and-catalog-new-tenants-in-a-saas-application-using-a-sharded-multi-tenant-azure-sql-database"></a>Çok kiracılı azure SQL veritabanını kullanarak SaaS uygulamasında yeni kiracılar sağlama ve kataloglama

Bu makalede, *çok kiracılı parçalı veritabanı* modeli veya deseni, yeni kiracıların sağlanması ve kataloglama kapsar.

Bu makalede iki ana bölüm vardır:

- Yeni kiracıların sağlanması ve kataloglanması kavramsal [tartışma.](#goto_2_conceptual)

- Sağlama ve kataloglama yı başaran PowerShell komut dosyası kodunu vurgulayan [öğretici.](#goto_1_tutorial)
  - Öğretici, çok kiracılı parçalı veritabanı desenine uyarlanmış Wingtip Tickets SaaS uygulamasını kullanır.

<a name="goto_2_conceptual"/>

## <a name="database-pattern"></a>Veritabanı deseni

Bu bölümde, artı birkaç daha takip, çok kiracı kırık veritabanı deseni kavramları tartışmak.

Bu çok kiracılı parçalanmış modelde, her veritabanının içindeki tablo şemaları, kiracı verilerini depolayan tabloların birincil anahtarında bir kiracı anahtarı içerir. Kiracı anahtarı, her bir veritabanının 0, 1 veya çok sayıda kiracı yı depolamasını sağlar. Kırık veritabanlarının kullanımı, uygulama sisteminin çok sayıda kiracıyı desteklemesini kolaylaştırır. Herhangi bir kiracıya ait tüm veriler tek bir veritabanında depolanır. Çok sayıda kiracı, çok sayıda parçalanmış veritabanları arasında dağıtılır. Katalog veritabanı, her kiracının eşlemesini veritabanına saklar.

#### <a name="isolation-versus-lower-cost"></a>Yalıtım ve daha düşük maliyet

Kendi başına bir veritabanı olan bir kiracı, yalıtımın avantajlarından yararlanır. Kiracı, veritabanını diğer kiracılar üzerindeki etkisiyle sınırlandırılmadan daha önceki bir tarihe geri yükleyebilir. Veritabanı performansı, diğer kiracılarla uzlaşmak zorunda kalmadan yine bir kiracı için en iyi duruma getirilecek şekilde ayarlanabilir. Sorun, yalıtımın bir veritabanını diğer kiracılarla paylaşmanın maliyetinden daha pahalı olmasıdır.

Yeni bir kiracı sağlandığında, veritabanını diğer kiracılarla paylaşabilir veya kendi yeni veritabanına yerleştirilebilir. Daha sonra fikrinizi değiştirebilir ve veritabanını diğer duruma taşıyabilirsiniz.

Birden çok kiracı ve tek kiracıiçeren veritabanları, her kiracı için maliyeti veya yalıtımı optimize etmek için aynı SaaS uygulamasında karıştırılır.

   ![Kiracı kataloğu ile sharded çok kiracı veritabanı uygulaması](media/saas-multitenantdb-provision-and-catalog/MultiTenantCatalog.png)

## <a name="tenant-catalog-pattern"></a>Kiracı katalog deseni

Her biri en az bir kiracı içeren iki veya daha fazla veritabanınız varsa, uygulamanın geçerli ilgi alanının kiracısını depolayan veritabanını bulmanın bir yolu olmalıdır. Katalog veritabanı bu eşlemi depolar.

#### <a name="tenant-key"></a>Kiracı anahtarı

Her kiracı için Wingtip uygulaması, kiracı anahtarı olan benzersiz bir anahtar türetilebilir. Uygulama, kiracı adını web sayfası URL'sinden ayıklar. Uygulama anahtarı elde etmek için adı haşlar. Uygulama, kataloga erişmek için anahtarı kullanır. Katalog, kiracının depolandığı veritabanı hakkındaki bilgileri çapraz başvuruolarak aktarar. Uygulama bağlanmak için veritabanı bilgilerini kullanır. Diğer kiracı anahtar düzenleri de kullanılabilir.

Katalog kullanmak, kiracı veritabanının adının veya konumunun, uygulamayı kesintiye uğratmadan sağlanmıştırktan sonra değiştirilmesine olanak tanır. Çok kiracılı veritabanı modelinde, katalog bir kiracıyı veritabanları arasında taşımayı barındırır.

#### <a name="tenant-metadata-beyond-location"></a>Konum dışında kiracı meta verileri

Katalog, kiracının bakım veya diğer eylemler için çevrimdışı olup olmadığını da gösterebilir. Ayrıca, aşağıdaki öğeler gibi ek kiracı veya veritabanı meta verilerini depolamak için katalog genişletilebilir:
- Hizmet katmanı veya veritabanının sürümü.
- Veritabanı şemasının sürümü.
- Kiracı adı ve SLA (hizmet düzeyi sözleşmesi).
- Uygulama yönetimini, müşteri desteğini veya devops süreçlerini etkinleştirmek için bilgi.

Katalog, kiracılar arası raporlamayı, şema yönetimini ve analiz amacıyla veri ayıklanmasını etkinleştirmek için de kullanılabilir.

### <a name="elastic-database-client-library"></a>Elastik Veritabanı İstemci Kitaplığı

Wingtip'te katalog *kiracı kataloğu* veritabanında uygulanır. *Kiracı kataloğu,* [Elastik Veritabanı İstemci Kitaplığı'nın (EDCL)](sql-database-elastic-database-client-library.md)Shard Management özellikleri kullanılarak oluşturulur. Kitaplık, bir uygulamanın veritabanında depolanan *bir parça eşlemi* oluşturmasına, yönetmesini ve kullanmasını sağlar. Bir parça harita, kiracı anahtarına parçalı veritabanı nı ifade eden bir çapraz referans verir.

Kiracı sağlama sırasında, EDCL işlevleri, parça lı haritadaki girişleri oluşturmak için uygulamalardan veya PowerShell komut dosyalarından kullanılabilir. Daha sonra EDCL işlevleri doğru veritabanına bağlanmak için kullanılabilir. EDCL, katalog veritabanındaki trafiği en aza indirmek ve bağlanma işlemini hızlandırmak için bağlantı bilgilerini önbelleğe alıyor.

> [!IMPORTANT]
> Katalog veritabanındaki verileri doğrudan erişim yoluyla da *yönetmeyin!* Veri bozulması riski yüksek nedeniyle doğrudan güncelleştirmeler desteklenmez. Bunun yerine, yalnızca EDCL API'leri kullanarak eşleme verilerini düzenleme.

## <a name="tenant-provisioning-pattern"></a>Kiracı sağlama deseni

#### <a name="checklist"></a>Denetim Listesi

Paylaşılan veritabanının varolan paylaşılan veritabanına yeni bir kiracı sağlamak istediğinizde aşağıdaki soruları sormanız gerekir:
- Yeni kiracı için yeterli alanı kaldı mı?
- Yeni kiracı için gerekli başvuru verilerine sahip tablolar var mı, yoksa veriler eklenebilir mi?
- Yeni kiracı için temel şema uygun varyasyon var mı?
- Yeni kiracıya yakın uygun coğrafi konumda mı?
- Yeni kiracı için doğru hizmet katmanında mı?

Yeni kiracının kendi veritabanında yalıtılmasını istediğinizde, kiracının belirtimlerini karşılayacak şekilde oluşturabilirsiniz.

Hüküm tamamlandıktan sonra kiracıyı kataloga kaydettirmelisiniz. Son olarak, kiracı eşleme uygun parça başvuru eklenebilir.

#### <a name="template-database"></a>Şablon veritabanı

SQL komut dosyalarını çalıştırarak, bir bacpac dağıtarak veya şablon veritabanını kopyalayarak veritabanını sağlama. Wingtip uygulamaları, yeni kiracı veritabanları oluşturmak için bir şablon veritabanıkopyalar.

Herhangi bir uygulama gibi, Wingtip zaman içinde gelişecektir. Bazen Wingtip veritabanında değişiklik gerektirir. Değişiklikler aşağıdaki öğeleri içerebilir:
- Yeni veya değiştirilmiş şema.
- Yeni veya değiştirilmiş başvuru verileri.
- En iyi uygulama performansını sağlamak için rutin veritabanı bakım görevleri.

SaaS uygulamasıyla, bu değişikliklerin oldukça büyük olabilecek bir kiracı veritabanı filosunda eşgüdümlü şekilde dağıtılması gerekir. Bu değişikliklerin gelecekteki kiracı veritabanlarında olması için, bunların sağlama sürecine dahil edilmesi gerekir. Bu meydan okuma [şema yönetimi öğretici](saas-tenancy-schema-management.md)daha fazla araştırılır.

#### <a name="scripts"></a>Betikler

Bu öğreticide komut dosyalarını sağlayan kiracı aşağıdaki senaryoların her ikisini de destekler:
- Kiracıyı diğer kiracılarla paylaşılan varolan bir veritabanına sağlama.
- Kiracıyı kendi veritabanına sokmak.

Kiracı verileri daha sonra başharfe kaydedilir ve katalog parça haritasında kaydedilir. Örnek uygulamada, birden çok kiracı içeren veritabanlarına *kiracı1* veya kiracı2 gibi genel bir ad *verilir.* Tek bir kiracı içeren veritabanlarına kiracının adı verilir. Örnekte kullanılan belirli adlandırma kuralları, kataloğun kullanımı veritabanına herhangi bir ad atanmasına izin verdiğinden, desenin önemli bir parçası değildir.

<a name="goto_1_tutorial"/>

## <a name="tutorial-begins"></a>Öğretici başlar

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
> * Kiracıyı çok kiracılı bir veritabanına sağlama
> * Kiracıyı tek kiracıveritabanına sağlama
> * Hem çok kiracılı hem de tek kiracılı veritabanlarına bir toplu kiracı sağlama
> * Veritabanını ve kiracı eşlecilerini katalogda kaydetme

#### <a name="prerequisites"></a>Ön koşullar

Bu öğreticiyi tamamlamak için aşağıdaki ön koşulların karşılandığından emin olun:

- Azure PowerShell’in yüklendiğinden. Ayrıntılar için bkz. [Azure PowerShell’i kullanmaya başlama](https://docs.microsoft.com/powershell/azure/get-started-azureps)

- Wingtip Tickets SaaS Çok kiracılı Veritabanı uygulaması dağıtılır. Beş dakikadan kısa bir süre içinde dağıtmak için [Wingtip Tickets SaaS Çok kiracılı Veritabanı uygulamasını dağıtın ve keşfedin](saas-multitenantdb-get-started-deploy.md)

- Wingtip komut dosyalarını ve kaynak kodunu alın:
    - Wingtip Biletleri SaaS Çok kiracıveritabanı komut dosyaları ve uygulama kaynak kodu [WingtipTicketsSaaS-MultitenantDB](https://github.com/microsoft/WingtipTicketsSaaS-MultiTenantDB) GitHub repo mevcuttur.
    - Wingtip komut dosyalarını indirmek ve engelini kaldırmak için adımlar için [genel kılavuza](saas-tenancy-wingtip-app-guidance-tips.md) bakın.

## <a name="provision-a-tenant-into-a-database-shared-with-other-tenants"></a>Kiracıyı diğer kiracılarla paylaşılan bir veritabanına *sağlama*

Bu bölümde, PowerShell komut dosyaları tarafından gerçekleştirilen sağlama için önemli eylemlerin bir listesini görürsünüz. Ardından, kodları kodda görmek için komut dosyaları üzerinden adım için PowerShell ISE hata ayıklayıcı kullanın.

#### <a name="major-actions-of-provisioning"></a>Sağlamanın başlıca eylemleri

Aşağıdakiler, adım attığınız sağlama iş akışının temel öğeleridir:

- **Yeni kiracı anahtarını hesapla**: Kiracı adından kiracı anahtarını oluşturmak için karma işlevi kullanılır.
- **Kiracı anahtarının zaten var olup olmadığını kontrol edin**: Anahtar zaten kaydedilmemiş olduğundan emin olmak için katalog denetlenir.
- **Varsayılan kiracı veritabanında kiracı başlatma**: Kiracı veritabanı yeni kiracı bilgilerini eklemek için güncelleştirilir.
- **Katalogda kiracıyı kaydedin**: Yeni kiracı anahtarı ile varolan kiracı1 veritabanı arasındaki eşleme kataloga eklenir.
- **Kiracının adını katalog uzantısı tablosuna ekleyin**: Mekan adı katalogdaki Kiracı tablosuna eklenir.  Bu ek, ek uygulamaya özgü verileri desteklemek için Katalog veritabanının nasıl genişletilebileceğini gösterir.
- **Yeni kiracı için Açık Etkinlikler sayfası**: *Bushwillow Blues* olaylar sayfası tarayıcıda açılır.

   ![etkinlikler](media/saas-multitenantdb-provision-and-catalog/bushwillow.png)

#### <a name="debugger-steps"></a>Hata ayıklama adımları

Wingtip uygulamasının paylaşılan bir veritabanında yeni kiracı sağlamayı nasıl uyguladığını anlamak için bir kesme noktası ekleyin ve iş akışında adım atın:

1. *PowerShell ISE*, açık ... \\Öğrenme Modülleri\\ProvisionTenants\\*Demo-ProvisionTenants.ps1* ve aşağıdaki parametreleri ayarlayın:
   - **$TenantName** = **Bushwillow Blues**, yeni bir mekan adı.
   - **blues $VenueType** = **,** önceden tanımlanmış mekan türlerinden biri: blues, klasik müzik, dans, caz, judo, motorracing, çok amaçlı, opera, rockmüzik, futbol (küçük, boşluk).
   - **$DemoScenario** = **1,** diğer kiracılar ile paylaşılan bir veritabanında bir kiracı sağlamak için.

2. İmlecinizi 38. satıra koyarak bir kesme noktası ekleyin, şöyle diyor: *Yeni Kiracı '*, ve sonra **F9 tuşuna**basın.

   ![kesme noktası](media/saas-multitenantdb-provision-and-catalog/breakpoint.png)

3. **F5**tuşuna basarak komut dosyasını çalıştırın.

4. Komut dosyası yürütme kesme noktasında durduktan sonra, koda adım atmak için **F11** tuşuna basın.

   ![hata ayıklama](media/saas-multitenantdb-provision-and-catalog/debug.png)

5. **Hata Ayıklama** menüsü seçenekleri, **F10** ve **F11**kullanarak komut dosyasının yürütülmesini izleyin, çağrılan işlevlerin üzerine veya içine adım atın.

PowerShell komut dosyalarının hata ayıklanması hakkında daha fazla bilgi için [PowerShell komut dosyalarıyla çalışma ve hata ayıklama hakkındaki ipuçlarına](https://docs.microsoft.com/powershell/scripting/components/ise/how-to-debug-scripts-in-windows-powershell-ise)bakın.

## <a name="provision-a-tenant-in-its-own-database"></a>Kiracıyı *kendi* veritabanında sağlama

#### <a name="major-actions-of-provisioning"></a>Sağlamanın başlıca eylemleri

Komut dosyasını takip ederken adım ını atdığınız iş akışının temel öğeleri şunlardır:

- **Yeni kiracı anahtarını hesapla**: Kiracı adından kiracı anahtarını oluşturmak için karma işlevi kullanılır.
- **Kiracı anahtarının zaten var olup olmadığını kontrol edin**: Anahtar zaten kaydedilmemiş olduğundan emin olmak için katalog denetlenir.
- **Yeni bir kiracı veritabanı oluşturma**: Veritabanı, kaynak yöneticisi şablonu kullanarak *basetenantdb* veritabanıkopyalayarak oluşturulur.  Yeni veritabanı adı kiracının adını temel almaktadır.
- **Kataloğu katılım ekleme**: Yeni kiracı veritabanı kataloğuna bir parça olarak kaydedilir.
- **Varsayılan kiracı veritabanında kiracı başlatma**: Kiracı veritabanı yeni kiracı bilgilerini eklemek için güncelleştirilir.
- **Katalogda kiracı yı kaydedin**: Yeni kiracı anahtarı ile *sekoya futbol* veritabanı arasındaki eşleme kataloga eklenir.
- **Kiracı adı kataloga eklenir**: Mekan adı katalogdaki Kiracı uzatma tablosuna eklenir.
- **Yeni kiracı için Açık Etkinlikler sayfası**: *Sequoia Soccer* Events sayfası tarayıcıda açılır.

   ![etkinlikler](media/saas-multitenantdb-provision-and-catalog/sequoiasoccer.png)

#### <a name="debugger-steps"></a>Hata ayıklama adımları

Şimdi kendi veritabanında bir kiracı oluştururken komut dosyası işlemi üzerinden yürüyün:

1. Hala içinde ... \\Öğrenme Modülleri\\ProvisionTenants\\*Demo-ProvisionTenants.ps1* aşağıdaki parametreleri ayarlayın:
   - **$TenantName** = **Sequoia Soccer**, yeni bir mekan adı.
   - **$VenueType** = **futbol**, önceden tanımlanmış mekan türlerinden biri: blues, klasik müzik, dans, caz, judo, motorracing, çok amaçlı, opera, rockmüzik, futbol (küçük harf, boşluk yok).
   - **$DemoScenario** = **2,** kendi veritabanına bir kiracı sağlamak için.

2. Imlecinizi 57. satıra koyarak yeni bir kesme noktası ekleyin, satır57' deyiş * & &nbsp;$PSScriptRoot\New-TenantAndDatabase '* ve **F9**tuşuna basın.

   ![kesme noktası](media/saas-multitenantdb-provision-and-catalog/breakpoint2.png)

3. **F5**tuşuna basarak komut dosyasını çalıştırın.

4. Komut dosyası yürütme kesme noktasında durduktan sonra, koda adım atmak için **F11** tuşuna basın.  Yürütmeyi izlemek için **f10** ve **F11'i** kullanarak işlevlerin üzerine çıkın.

## <a name="provision-a-batch-of-tenants"></a>Kiracıların bir toplu hükmü

Bu egzersiz 17 kiracı bir toplu madde yi karşılar. Diğer Wingtip Biletleri öğreticilerine başlamadan önce bu kiracı grubu sağlamanız önerilir, böylece çalışabileceğiniz daha fazla veritabanları vardır.

1. *PowerShell ISE*, açık ... \\\\Öğrenme Modülleri HükmüKiracılar\\*Demo-ProvisionTenants.ps1* ve *$DemoScenario* parametresini 4 olarak değiştirin:
   - **$DemoScenario** = **4,** paylaşılan bir veritabanına kiracı bir toplu sağlamak için.

2. **F5** tuşuna basıp betiği çalıştırın.

### <a name="verify-the-deployed-set-of-tenants"></a>Dağıtılan kiracı kümesini doğrulama

Bu aşamada, paylaşılan bir veritabanına dağıtılan kiracıların bir karışımı ve kiracılar ın kendi veritabanlarına dağıtılması vardır. Azure portalı oluşturulan veritabanlarını incelemek için kullanılabilir. Azure [portalında,](https://portal.azure.com)SQL sunucuları listesine göz atarak **\<\> kiracı1-mt- USER** sunucusunu açın.  **SQL veritabanları** listesi paylaşılan **kiracı1** veritabanını ve kendi veritabanında bulunan kiracılar için veritabanlarını içermelidir:

   ![veritabanı listesi öğesine tıklayın](media/saas-multitenantdb-provision-and-catalog/Databases.png)

Azure portalı kiracı veritabanlarını gösterirken, paylaşılan *veritabanındaki* kiracıları görmenize izin vermez. Kiracıların tam listesi **Wingtip'in Events Hub** web sayfasında ve kataloga göz atarak görülebilir.

#### <a name="using-wingtip-tickets-events-hub-page"></a>Wingtip Biletleri etkinlik merkezi sayfasını kullanma

Tarayıcıda Events Hub sayfasını açın (http:events.wingtip-mt.\<USER\>.trafficmanager.net)

#### <a name="using-catalog-database"></a>Katalog veritabanını kullanma

Kiracıların tam listesi ve her biri için ilgili veritabanı katalogda mevcuttur. Kiracı adını veritabanı adına birleştiren bir SQL görünümü sağlanır. Görünüm, katalogda depolanan meta verileri genişletmenin değerini güzel bir şekilde gösterir.
- SQL görünümü kiracı kataloğu veritabanında kullanılabilir.
- Kiracı adı Kiracı tablosunda depolanır.
- Veritabanı adı Shard Management tablolarında depolanır.

1. SQL Server Management Studio 'da (SSMS), Giriş = **geliştirici**ile **catalog-mt.\<USER\>.database.windows.net**adresindeki kiracı sunucusuna bağlanın ve Password = P **\@ssword1**

    ![SSMS bağlantı iletişim kutusu](media/saas-multitenantdb-provision-and-catalog/SSMSConnection.png)

2. SSMS Object Explorer'da, *kiracı kataloğu* veritabanındaki görünümlere göz atın.

3. Görünüme sağ tıklayın *TenantsExtended* ve **Top 1000 Satır seçin.** Farklı kiracılar için kiracı adı ve veritabanı arasındaki eşleme not edin.

    ![SSMS'te Genişletilmiş Kiracılar görünümü](media/saas-multitenantdb-provision-and-catalog/extendedtenantsview.png)

## <a name="other-provisioning-patterns"></a>Diğer sağlama düzenleri

Bu bölümde diğer ilginç sağlama kalıpları tartışılmaktadır.

#### <a name="pre-provisioning-databases-in-elastic-pools"></a>Elastik havuzlarda veritabanlarıön sağlama

Ön sağlama deseni, elastik havuzlar kullanırken faturalandırmanın veritabanları için değil havuz için olduğu gerçeğinden yararlanır. Böylece veritabanları ek ücrete tabi olmadan ihtiyaç duyulmadan önce elastik bir havuza eklenebilir. Bu ön vizyon önemli ölçüde bir veritabanına bir kiracı sağlamak için alınan süreyi azaltır. Önceden sağlanan veritabanlarının sayısı, beklenen sağlama oranına uygun bir arabellek tutmak için gerektiğinde ayarlanabilir.

#### <a name="auto-provisioning"></a>Otomatik sağlama

Otomatik sağlama deseninde, sunucuları, havuzları ve veritabanlarını gerektiğinde otomatik olarak sağlamak için özel bir sağlama hizmeti kullanılır. Bu otomasyon, elastik havuzlarda veritabanlarının ön sağlanmasını içerir. Veritabanları kullanımdan kaldırılıp silinirse, elastik havuzlarda oluşan boşluklar, sağlama hizmeti tarafından istenildiği gibi doldurulabilir.

Bu tür otomatik hizmet basit veya karmaşık olabilir. Örneğin, otomasyon birden çok coğrafyada sağlama işlemini işleyebilir ve olağanüstü durum kurtarma için coğrafi çoğaltma ayarlayabilir. Otomatik sağlama deseniyle, istemci başvurusu veya komut dosyası, bir sağlama hizmeti tarafından işlenecek sıraya bir sağlama isteği gönderir. Komut dosyası daha sonra tamamlanmasını tespit etmek için anket olur. Ön sağlama kullanılırsa, istekler hızlı bir şekilde işlenirken, arka plan hizmeti değiştirilen veritabanının sağlanmasını yönetir.

## <a name="additional-resources"></a>Ek kaynaklar

<!-- - Additional [tutorials that build upon the Wingtip SaaS application](saas-dbpertenant-wingtip-app-overview.md#sql-database-wingtip-saas-tutorials)-->
- [Elastik veritabanı istemci kitaplığı](sql-database-elastic-database-client-library.md)
- [Windows PowerShell ISE’de Betiklerde Hata Ayıklama](https://docs.microsoft.com/powershell/scripting/components/ise/how-to-debug-scripts-in-windows-powershell-ise)


## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Paylaşılan çok kiracılı veritabanına ve kendi veritabanına tek bir yeni kiracı sağlama
> * Ek kiracı grubu sağlama
> * Kiracı sağlama nın ayrıntılarını gözden geçirin ve bunları kataloga kaydettirin

Performans [izleme öğreticisini](saas-multitenantdb-performance-monitoring.md)deneyin.

