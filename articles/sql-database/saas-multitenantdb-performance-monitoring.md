---
title: Çok kiracılı bir veritabanının performansını izleme
description: Çok kiracılı bir SaaS uygulamasında, çok kiracılı Azure SQL veritabanının performansını izleyin ve yönetin
services: sql-database
ms.service: sql-database
ms.subservice: scenario
ms.custom: seo-lt-2019
ms.devlang: ''
ms.topic: conceptual
author: stevestein
ms.author: sstein
ms.reviewer: ''
ms.date: 01/25/2019
ms.openlocfilehash: 0af476b69f2effd836fe76d62059259076c16f53
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79214153"
---
# <a name="monitor-and-manage-performance-of-sharded-multi-tenant-azure-sql-database-in-a-multi-tenant-saas-app"></a>Çok kiracılı bir SaaS uygulamasında, çok kiracılı Azure SQL veritabanının performansını izleyin ve yönetin

Bu eğitimde, SaaS uygulamalarında kullanılan birkaç önemli performans yönetimi senaryosu incelenmiştir. Parçalı çok kiracılı veritabanları ndaki etkinliği simüle etmek için bir yük jeneratörü kullanarak, SQL Veritabanı'nın yerleşik izleme ve uyarı özellikleri gösterilmiştir.

Wingtip Tickets SaaS Çok kiracılı Veritabanı uygulaması, mekan (kiracı) verilerinin kiracı kimliğiyle potansiyel olarak birden fazla veritabanıarasında dağıtıldığı çok kiracılı bir veri modeli kullanır. Birçok SaaS uygulaması gibi, beklenen kiracı iş yükü düzeni öngörülemez ve düzensizdir. Diğer bir deyişle, bilet satışı herhangi bir zamanda gerçekleşebilir. Bu tipik veritabanı kullanım deseninden yararlanmak için, veritabanları bir çözümün maliyetini en iyi duruma getirmek için yukarı ve aşağı ölçeklendirilebilir. Bu tür bir desenle, yüklerin potansiyel olarak birden çok veritabanları arasında makul bir şekilde dengelendiğinden emin olmak için veritabanı kaynak kullanımını izlemek önemlidir. Ayrıca, tek tek veritabanlarının yeterli kaynaklara sahip olduğundan ve [DTU](sql-database-purchase-models.md#dtu-based-purchasing-model) sınırlarına isabet etmediğini de sağlamanız gerekir. Bu öğretici, veritabanlarını izlemenin ve yönetmenin yollarını ve iş yükündeki farklılıklara yanıt olarak düzeltici eylemin nasıl yapılacağını inceler.

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
> 
> * Sağlanan bir yük jeneratörü çalıştırarak kırık çok kiracılı veritabanındaki kullanımı simüle edin
> * Yük artışına yanıt vererken veritabanını izleme
> * Artan veritabanı yüküne yanıt olarak veritabanını ölçeklendirin
> * Kiracıyı tek kiracıveritabanına sağlama

Bu öğreticiyi tamamlamak için aşağıdaki ön koşulların karşılandığından emin olun:

* Wingtip Tickets SaaS Çok kiracılı Veritabanı uygulaması dağıtılır. Beş dakikadan kısa bir süre içinde dağıtmak için [Wingtip Tickets SaaS Çok kiracılı Veritabanı uygulamasını dağıtın ve keşfedin](saas-multitenantdb-get-started-deploy.md)
* Azure PowerShell’in yüklendiğinden. Ayrıntılar için bkz. [Azure PowerShell’i kullanmaya başlama](https://docs.microsoft.com/powershell/azure/get-started-azureps)

## <a name="introduction-to-saas-performance-management-patterns"></a>SaaS performans yönetimi modellerine giriş

Veritabanı performans yönetimi, performans verilerini derleyip çözümlemeyi ve ardından uygulamanız için kabul edilebilir bir yanıt süresi sağlamak için parametreleri ayarlayarak bu verilere yanıt vermeyi içerir. 

### <a name="performance-management-strategies"></a>Uygulama performansı stratejileri

* Performansı el ile izlemek zorunda kalmamak için, **veritabanları normal aralıklardan çıktığında tetikleyen uyarıları ayarlamak**en etkilidir.
* Bir veritabanının işlem boyutundaki kısa vadeli dalgalanmalara yanıt vermek için **DTU düzeyi yukarı veya aşağı ölçeklendirilebilir.** Bu dalgalanma düzenli veya öngörülebilir olarak oluşursa, **veritabanıölçekleme otomatik olarak oluşacak şekilde zamanlanabilir.** Örneğin, iş yükünüzün hafif olduğunu bildiğiniz gece veya hafta sonları gibi zamanlarda ölçeği azaltabilirsiniz.
* Uzun vadeli dalgalanmalara veya kiracılardaki değişikliklere yanıt vermek **için, kiracı ayrı ayrı başka bir veritabanına taşınabilir.**
* *Bireysel* kiracı yükündeki kısa vadeli artışlara yanıt vermek için, **bireysel kiracılar bir veritabanından çıkarılabilir ve tek bir işlem boyutu atanabilir.** Yük azaltıldıktan sonra, kiracı daha sonra çok kiracı veritabanına döndürülebilir. Bu önceden bilindiğinde, kiracılar veritabanının her zaman ihtiyaç duyduğu kaynaklara sahip olduğundan emin olmak ve çok kiracılı veritabanındaki diğer kiracılar üzerinde etki yiönlemek için önceden taşınabilir. Popüler bir etkinlik için bilet satışı yoğunluğu yaşanan bir mekanda olduğu gibi bu gereksinim öngörülebildiği takdirde bu yönetim davranışı uygulamayla tümleştirilebilir.

[Azure portalı](https://portal.azure.com), çoğu kaynak üzerinde yerleşik izleme ve uyarı özelliği sağlar. SQL Veritabanı için veritabanlarında izleme ve uyarı kullanılabilir. Bu yerleşik izleme ve uyarı kaynağa özgüdür, bu nedenle az sayıda kaynak için kullanmak uygundur, ancak birçok kaynakla çalışırken kullanışlı değildir.

Birçok kaynakla çalıştığınız yüksek hacimli [senaryolarda Azure Monitor günlükleri](https://azure.microsoft.com/services/log-analytics/) kullanılabilir. Bu, Log Analytics çalışma alanında toplanan yayılan günlükler üzerinden analitik sağlayan ayrı bir Azure hizmetidir. Azure Monitor günlükleri birçok hizmetten telemetri toplayabilir ve uyarıları sorgulamak ve ayarlamak için kullanılabilir.

## <a name="get-the-wingtip-tickets-saas-multi-tenant-database-application-source-code-and-scripts"></a>Wingtip Biletleri SaaS Çok Kiracı Veritabanı uygulama kaynak kodu ve komut alın

Wingtip Biletleri SaaS Çok kiracıveritabanı komut dosyaları ve uygulama kaynak kodu [WingtipTicketsSaaS-MultitenantDB](https://github.com/microsoft/WingtipTicketsSaaS-MultiTenantDB) GitHub repo mevcuttur. Wingtip Tickets SaaS komut dosyalarını indirmek ve engelini kaldırmak için gereken adımlar için [genel kılavuza](saas-tenancy-wingtip-app-guidance-tips.md) göz atın.

## <a name="provision-additional-tenants"></a>Ek kiracılar sağlama

Performans izleme ve yönetimin ölçekte nasıl çalıştığını iyi anlamak için, bu öğretici, çok kiracılı bir veritabanında birden çok kiracıya sahip olmayı gerektirir.

Önceden bir öğreticide bir kiracı grubu zaten saysanız, [tüm kiracı veritabanları bölümündeki Kullanımı Benzetilen'e](#simulate-usage-on-all-tenant-databases) atlayın.

1. **PowerShell ISE**, açık ... \\Öğrenme Modülleri\\Performans İzleme\\ve Yönetimi*Demo-PerformanceMonitoringAndManagement.ps1*. Bu öğretici sırasında birkaç senaryo çalıştıracağından bu betiği açık tutun.
1. Set **$DemoScenario** = **1**, Kiracılar Bir _toplu hükmü_
1. Betiği çalıştırmak için **F5**'e basın.

Komut dosyası birkaç dakika içinde çok kiracı veritabanına 17 kiracı dağıtılır. 

*New-TenantBatch* komut dosyası, parçalanan çok kiracı veritabanında benzersiz kiracı anahtarlarıyla yeni kiracılar oluşturur ve bunları kiracı adı ve mekan türüyle başharfe adatır. Bu, uygulamanın yeni bir kiracıyı karşılar.'ı karşılar. 

## <a name="simulate-usage-on-all-tenant-databases"></a>Tüm kiracı veritabanlarındaki kullanımın benzetimini gerçekleştirme

*Demo-PerformanceMonitoringAndManagement.ps1* komut dosyası, çok kiracıveritabanına karşı çalışan bir iş yükünü simüle etmek sağlanır. Yük, kullanılabilir yük senaryolarından biri kullanılarak oluşturulur:

| Tanıtım | Senaryo |
|:--|:--|
| 2 | Normal yoğunluk yükü oluşturma (yaklaşık 30 DTU) |
| 3 | Kiracı başına daha uzun patlamalar ile yük oluşturma|
| 4 | Kiracı başına daha yüksek DTU patlamaları ile yük oluşturma (yaklaşık 70 DTU)|
| 5 | Tek bir kiracıda yüksek yoğunluklu (yaklaşık 90 DTU) ve diğer tüm kiracılarüzerinde normal yoğunluk yükü oluşturun |

Yük oluşturucu her kiracı veritabanına *yapay* bir yalnızca CPU yükü uygular. Oluşturucu her kiracı veritabanı için yükü oluşturan saklı yordamı düzenli olarak çağıran bir iş başlatır. Yük düzeyleri (DTÜ), süre ve aralıklar öngörülemeyen kiracı etkinliğini taklit ederek tüm veritabanları arasında değişir.

1. **PowerShell ISE**, açık ... \\Öğrenme Modülleri\\Performans İzleme\\ve Yönetimi*Demo-PerformanceMonitoringAndManagement.ps1*. Bu öğretici sırasında birkaç senaryo çalıştıracağından bu betiği açık tutun.
1. Set **$DemoScenario** = **2**, Normal yoğunluk _yük oluşturma_
1. Tüm kiracılarınıza yük uygulamak için **F5** tuşuna basın.

Wingtip Tickets SaaS Multi-tenant Database bir SaaS uygulamasıdır ve Bir SaaS uygulamasındaki gerçek yük genellikle düzensiz ve öngörülemezdir. Yük oluşturucu, bunun benzetimini gerçekleştirmek için kiracılar genelinde dağıtılan rastgele yük oluşturur. Yük deseni ortaya çıkması için birkaç dakika gereklidir, bu nedenle aşağıdaki bölümlerde yük izlemek için denemeden önce 3-5 dakika boyunca yük jeneratörçalıştırın.

> [!IMPORTANT]
> Yük jeneratörü yeni bir PowerShell penceresinde bir dizi iş olarak çalışıyor. Oturumu kapatırsanız, yük jeneratörü durur. Yük jeneratörü, jeneratör başladıktan sonra sağlanan herhangi bir yeni kiracı üzerinde yük üreten bir *iş çağrıştıran* durumda kalır. Yeni iş çağırmak ve komut dosyasından çıkmak için *Ctrl-C'yi* kullanın. Yük jeneratörü çalışmaya devam edecektir, ancak sadece mevcut kiracılar üzerinde.

## <a name="monitor-resource-usage-using-the-azure-portal"></a>Azure portalını kullanarak kaynak kullanımını izleme

Yüklenen yükten kaynaklanan kaynak kullanımını izlemek için, portalı kiracıiçeren çok kiracıveritabanına **açın1:**

1. Azure [portalını](https://portal.azure.com) açın ve sunucu kiracılarına göz *atın1-mt-&lt;USER&gt;*.
1. Aşağı kaydırın ve veritabanlarını bulun ve **kiracı1**tıklatın. Bu kırık çok kiracıveritabanı şimdiye kadar oluşturulan tüm kiracı içerir.

![veritabanı grafiği](./media/saas-multitenantdb-performance-monitoring/multitenantdb.png)

**DTU** grafiğini gözlemleyin.

## <a name="set-performance-alerts-on-the-database"></a>Veritabanında performans uyarıları ayarlama

Veritabanında \>%75'lik kullanımı tetikleyen bir uyarı ayarlayın:

1. [Azure portalında](https://portal.azure.com) *kiracı1* veritabanını *(tenants1-mt-&lt;USER&gt; * sunucusunda) açın.
1. **Uyarı Kuralları**ve ardından **+ Uyarı ekle**’ye tıklayın:

   ![uyarı ekle](media/saas-multitenantdb-performance-monitoring/add-alert.png)

1. **Yüksek DTU** gibi bir ad belirtin,
1. Aşağıdaki değerleri ayarlayın:
   * **Metrik = DTU yüzdesi**
   * **Koşul = daha büyük**
   * **Eşik = 75**.
   * **Periyot = Son 30 dakika içinde**
1. *Ek yönetici e-posta(lar)* kutusuna bir e-posta adresi ekleyin ve **Tamam'ı**tıklatın.

   ![uyarı ayarlama](media/saas-multitenantdb-performance-monitoring/set-alert.png)

## <a name="scale-up-a-busy-database"></a>Meşgul bir veritabanını ölçeklendirme

Bir veritabanındaki yük düzeyi veritabanının en üst düzeye çıkarı ve %100 DTU kullanımına ulaştığı noktaya kadar artarsa, veritabanı performansı etkilenir ve sorgu yanıt sürelerini yavaşlatır.

**Kısa vadede,** ek kaynaklar sağlamak için veritabanını ölçeklemeyi veya kiracıları çok kiracıveritabanından kaldırmayı (çok kiracılı veritabanından bağımsız bir veritabanına taşımayı) düşünün.

**Uzun vadede,** veritabanı performansını artırmak için sorguları veya dizin kullanımını en iyi duruma almayı düşünün. Uygulamanın performans sorunlarına olan duyarlılığına bağlı olarak, bir veritabanını %100 DTU kullanımına ulaşmadan önce ölçeklendirmek en iyi yöntemdir. Sizi önceden uyarması için bir uyarı ayarlayın.

Jeneratör tarafından üretilen yükü artırarak meşgul bir veritabanı simüle edebilirsiniz. Kiracıların daha sık ve daha uzun süre patlamasına neden olarak, kiracıların gereksinimlerini değiştirmeden çok kiracı veritabanındaki yükü artırır. Veritabanını ölçekleme kolayca portal veya PowerShell yapılır. Bu alıştırmada portal kullanılmaktadır.

1. **3** *$DemoScenario* = ayarlayın , Her kiracı nın gerektirdiği en yüksek yükü değiştirmeden veritabanındaki toplam yükün yoğunluğunu artırmak için veritabanı başına daha uzun ve daha _sık patlamalarla yük oluşturun._
1. Bir yükü tüm kiracı veritabanlarınıza uygulamak için **F5** tuşuna basın.
1. Azure portalındaki **kiracılar1** veritabanına gidin.

Üst grafikte artan veritabanı DTU kullanımını izleyin. Yeni yüksek yükün devreye girmesi birkaç dakika sürer, ancak veritabanının maksimum kullanıma vurmaya başladığını hızlı bir şekilde görmeniz gerekir ve yük yeni modele sabitlenirken, veritabanını hızla aşırı yükler.

1. Veritabanını ölçeklendirmek için ayarlar bıçağında **Fiyatlandırma katmanını (ölçek DT'leri)** tıklatın.
1. **DTU** ayarını **100'e**ayarlayın. 
1. Veritabanını ölçeklendirmek için isteği göndermek için **Uygula'yı** tıklatın.

İzleme **çizelgelerini** > görüntülemek için kiracıya geri dön1**Genel Bakış.** Veritabanına daha fazla kaynak sağlamanın etkisini izleyin (birkaç kiracı ve rasgele bir yük olmasına rağmen, bir süre çalışana kadar kesin olarak görmek her zaman kolay değildir). Grafiklere bakarken üst grafikteki %100'ün şu anda 100 DSU'yu temsil ettiğini, alt grafikte ise %100'ünün hala 50 DTUs olduğunu unutmayın.

İşlem boyunca veritabanları çevrimiçi ve tam olarak kullanılabilir durumdadır. Uygulama kodu her zaman bırakılan bağlantıları yeniden denemek için yazılmalıdır ve böylece veritabanına yeniden bağlanır.

## <a name="provision-a-new-tenant-in-its-own-database"></a>Kendi veritabanında yeni bir kiracı sağlama 

Parçalanmış çok kiracılı model, diğer kiracılarla birlikte çok kiracılı bir veritabanında yeni bir kiracı sağlamayı mı yoksa kiracıyı kendi veritabanında mı sağlamanızı seçmenize olanak tanır. Kiracıyı kendi veritabanında sağlayarak, ayrı veritabanında bulunan yalıtımdan yararlanır ve bu kiracının performansını diğerlerinden bağımsız olarak yönetmeniz, kiracıyı diğerlerinden bağımsız olarak geri yüklemenize olanak sağlar. Örneğin, ücretsiz deneme sürümünü veya normal müşterileri çok kiracılı bir veritabanına, premium müşterileri ise tek tek veritabanlarına koymayı seçebilirsiniz.  Yalıtılmış tek kiracıveritabanları oluşturulursa, kaynak maliyetlerini en iyi duruma getirmek için elastik bir havuzda toplu olarak yönetilebilirler.

Kendi veritabanında zaten yeni bir kiracı bulduysanız, sonraki birkaç adımı atlayın.

1. **PowerShell ISE**, açık ... \\Öğrenme Modülleri\\ProvisionKiracılar\\*Demo-ProvisionTenants.ps1*. 
1. **$TenantName değiştirin = "Salix Salsa"** ve **$VenueType = "dans"**
1. Set **$Scenario** = **2**, Yeni bir tek _kiracı veritabanında bir kiracı sağlama_
1. Betiği çalıştırmak için **F5**'e basın.

Komut dosyası bu kiracıyı ayrı bir veritabanında sağlar, veritabanını ve kiracıyı katalogla kaydeder ve ardından kiracının Etkinlikler sayfasını tarayıcıda açar. Etkinlikler Hub sayfasını yenileyin ve "Salix Salsa"nın mekan olarak eklendiğini göreceksiniz.

## <a name="manage-performance-of-an-individual-database"></a>Tek bir veritabanının performansını yönetme

Çok kiracılı bir veritabanındaki tek bir kiracı sürekli yüksek bir yükle karşılaşıyorsa, veritabanı kaynaklarına hakim olma ve aynı veritabanındaki diğer kiracıları etkileme eğiliminde olabilir. Etkinliğin bir süre daha devam etmesi olasıysa, kiracı geçici olarak veritabanından çıkarılabilir ve kendi tek kiracıveritabanına taşınabilir. Bu, kiracının ihtiyacı olan ek kaynaklara sahip olmasını sağlar ve onu diğer kiracılardan tamamen yalıtlar.

Bu egzersiz, salix Salsa'nın popüler bir etkinlik için biletler satışa çıktığında yüksek bir yük yaşamasının etkisini simüle eder.

1. Açık ... \\ *Demo-PerformanceMonitoringAndManagement.ps1* komut dosyası.
1. **set $DemoScenario = 5**, Normal bir yük artı tek bir kiracı _(yaklaşık 90 DTU) üzerinde yüksek bir yük oluşturun._
1. Set **$SingleTenantName = Salix Salsa**
1. **F5**’i kullanarak betiği yürütün.

Portala gidin ve izleme grafiklerini görüntülemek için **salixsalsa** > **Genel Bakış'a** gidin. 

## <a name="other-performance-management-patterns"></a>Diğer performans yönetimi kalıpları

**Kiracı self servis ölçekleme**

Ölçeklendirme, yönetim API'si aracılığıyla kolayca çağrılan bir görev olduğundan, kiracı veritabanlarını kiracıya dönük uygulamanıza ölçeklendirme ve SaaS hizmetinizin bir özelliği olarak kolayca sunabilirsiniz. Örneğin, kiracıların ölçek artırma ve azaltma işlemini kendi kendine yönetmesine, belki de doğrudan faturalarına bağlamasına izin verebilirsiniz!

**Kullanım desenlerini eşleştirmek için bir zamanlamada veritabanını yukarı ve aşağı ölçekleme**

Toplu kiracı kullanımının öngörülebilir kullanım modellerini izlediği durumlarda, veritabanını zamanlamada yukarı ve aşağı ölçeklendirmek için Azure Otomasyonu'ni kullanabilirsiniz. Örneğin, kaynak gereksinimlerinde bir düşüş olduğunu bildiğinizde, veritabanını hafta içi 06:00'dan önce saat 18:00'den sonra ve tekrar yukarı ölçeklendirin.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
> * Sağlanan bir yük jeneratörü çalıştırarak kırık çok kiracılı veritabanındaki kullanımı simüle edin
> * Yük artışına yanıt vererken veritabanını izleme
> * Artan veritabanı yüküne yanıt olarak veritabanını ölçeklendirin
> * Kiracıyı tek kiracıveritabanına sağlama

## <a name="additional-resources"></a>Ek kaynaklar

<!--* [Additional tutorials that build upon the Wingtip Tickets SaaS Multi-tenant Database application deployment](saas-multitenantdb-wingtip-app-overview.md#sql-database-wingtip-saas-tutorials)-->
* [Azure otomasyonu](../automation/automation-intro.md)