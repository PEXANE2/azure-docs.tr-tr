---
title: 'Saas uygulaması: Birçok veritabanının performansını izleyin'
description: Çok kiracılı bir SaaS uygulamasında Azure SQL veritabanlarının ve havuzlarının performansını izleme ve yönetme
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
ms.openlocfilehash: 34c50795567615637e31446ad3dc51a5e1b355f6
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79214460"
---
# <a name="monitor-and-manage-performance-of-azure-sql-databases-and-pools-in-a-multi-tenant-saas-app"></a>Çok kiracılı bir SaaS uygulamasında Azure SQL veritabanlarının ve havuzlarının performansını izleme ve yönetme

Bu eğitimde, SaaS uygulamalarında kullanılan birkaç önemli performans yönetimi senaryosu incelenmiştir. Tüm kiracı veritabanlarındaki etkinliği simüle etmek için bir yük jeneratörü kullanarak, SQL Veritabanı ve elastik havuzların yerleşik izleme ve uyarı özellikleri gösterilmiştir.

Wingtip Tickets SaaS Database Per Tenant uygulaması, her mekanın (kiracının) kendi veritabanına sahip olduğu tek kiracılı bir veri modeli kullanır. Birçok SaaS uygulaması gibi, beklenen kiracı iş yükü düzeni öngörülemez ve düzensizdir. Diğer bir deyişle, bilet satışı herhangi bir zamanda gerçekleşebilir. Bu tipik veritabanı kullanım deseninden yararlanmak için kiracı veritabanları elastik havuzlara dağıtılır. Elastik havuzlar, kaynakları çok sayıda veritabanı arasında paylaştırarak çözüm maliyetini en iyi duruma getirir. Bu düzen türü ile yüklerin havuzlar arasında makul bir şekilde dengelendiğinden emin olmak için veritabanı ve havuz kaynak kullanımının izlenmesi önemlidir. Ayrıca, her bir veritabanının yeterli kaynağa sahip olduğundan ve havuzların [eDTU](sql-database-purchase-models.md#dtu-based-purchasing-model) sınırına ulaşmadığından emin olmanız gerekir. Bu öğreticide, veritabanı ve havuzları izleyip yönetme yollarını ve iş yükündeki değişikliklere yanıt olarak nasıl düzeltici işlem yapılacağını incelenmektedir.

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
> 
> * Sağlanan bir yük oluşturucuyu çalıştırarak kiracı veritabanlarındaki kullanımın benzetimini gerçekleştirme
> * Yükteki artışa yanıt veren kiracı veritabanlarını izleme
> * Artan veritabanı yüküne yanıt olarak Elastik havuzun ölçeğini artırma
> * Veritabanı etkinliğinin yükünü dengelemek üzere ikinci bir Elastik havuz sağlama


Bu öğreticiyi tamamlamak için aşağıdaki ön koşulların karşılandığından emin olun:

* Wingtip Tickets SaaS Database Per Tenant uygulaması dağıtılır. Beş dakikadan kısa bir süre içinde dağıtmak için, [Wingtip Tickets SaaS Veritabanı'nı](saas-dbpertenant-get-started-deploy.md) Kiracı Başına dağıtın ve keşfedin
* Azure PowerShell’in yüklendiğinden. Ayrıntılar için bkz. [Azure PowerShell’i kullanmaya başlama](https://docs.microsoft.com/powershell/azure/get-started-azureps)

## <a name="introduction-to-saas-performance-management-patterns"></a>SaaS performans yönetimi modellerine giriş

Veritabanı performans yönetimi, performans verilerini derleyip çözümlemeyi ve ardından uygulamanız için kabul edilebilir bir yanıt süresi sağlamak için parametreleri ayarlayarak bu verilere yanıt vermeyi içerir. Birden çok kiracı barındırırken, Elastik havuzlar öngörülemeyen iş yüklerine sahip bir veritabanı grubu için kaynak sağlamanın ve yönetmenin uygun maliyetli bir yoludur. Bazı iş yükü düzenlerinde iki S3 veritabanı bile tek bir havuzda yönetilebilir.

![uygulama diyagramı](./media/saas-dbpertenant-performance-monitoring/app-diagram.png)

Havuzlar ve havuzdaki veritabanları, kabul edilebilir performans aralıklarında kaldıklarından emin olmak için izlenmelidir. Havuz eDT'lerinin genel iş yüküne uygun olmasını sağlayarak tüm veritabanlarının toplam iş yükünün gereksinimlerini karşılamak için havuz yapılandırmasını ayarlayın. Veritabanı başına en düşük ve veritabanı başına en yüksek eDTU değerlerini, uygulamanıza özel gereksinimler için uygun değerlere ayarlayın.

### <a name="performance-management-strategies"></a>Uygulama performansı stratejileri

* Performansı el ile izlemek zorunda kalmamak için, **veritabanları veya havuzlar normal aralıklardan çıktığında tetikleyen uyarıları ayarlamak**en etkilidir.
* Bir havuzun toplam işlem boyutundaki kısa vadeli dalgalanmalara yanıt vermek için **havuz eDTU düzeyi yukarı veya aşağı ölçeklendirilebilir.** Bu dalgalanma düzenli veya öngörülebilir aralıklarla gerçekleşiyorsa, **havuz ölçeklendirmesi otomatik olarak gerçekleşecek şekilde zamanlanabilir**. Örneğin, iş yükünüzün hafif olduğunu bildiğiniz gece veya hafta sonları gibi zamanlarda ölçeği azaltabilirsiniz.
* Daha uzun vadeli dalgalanmalara ya da veritabanı sayısındaki değişikliklere yanıt vermek için, **tek veritabanları diğer havuzlara taşınabilir**.
* Bireysel veritabanı yükü tek *tek* veritabanlarıkısa vadeli artışlara yanıt vermek için **bir havuz dan alınabilir ve tek bir işlem boyutu atanabilir.** Yükü azaldıktan sonra veritabanını havuza döndürebilirsiniz. Bu önceden bilindiğinde, veritabanları veritabanının her zaman ihtiyaç duyduğu kaynaklara sahip olduğundan emin olmak ve havuzdaki diğer veritabanları üzerindeki etkiyi önlemek için önceden taşınabilir. Popüler bir etkinlik için bilet satışı yoğunluğu yaşanan bir mekanda olduğu gibi bu gereksinim öngörülebildiği takdirde bu yönetim davranışı uygulamayla tümleştirilebilir.

[Azure portalı](https://portal.azure.com), çoğu kaynak üzerinde yerleşik izleme ve uyarı özelliği sağlar. SQL Veritabanı için veritabanı ve havuzlarda izleme ve uyarı özellikleri kullanılabilir. Bu yerleşik izleme ve uyarı kaynağa özgüdür, bu nedenle az sayıda kaynak için kullanmak uygundur, ancak birçok kaynakla çalışırken çok kullanışlı değildir.

Birçok kaynakla çalıştığınız yüksek hacimli [senaryolarda Azure Monitor günlükleri](saas-dbpertenant-log-analytics.md) kullanılabilir. Bu, Log Analytics çalışma alanında toplanan yayılan günlükler üzerinden analitik sağlayan ayrı bir Azure hizmetidir. Azure Monitor günlükleri birçok hizmetten telemetri toplayabilir ve uyarıları sorgulamak ve ayarlamak için kullanılabilir.

## <a name="get-the-wingtip-tickets-saas-database-per-tenant-application-scripts"></a>Wingtip Biletleri SaaS Veritabanı Kiracı başına uygulama komut alın

Wingtip Biletleri SaaS Çok kiracıveritabanı komut dosyaları ve uygulama kaynak kodu [WingtipTicketsSaaS-DbPerTenant](https://github.com/Microsoft/WingtipTicketsSaaS-DbPerTenant) GitHub repo mevcuttur. Wingtip Tickets SaaS komut dosyalarını indirmek ve engelini kaldırmak için gereken adımlar için [genel kılavuza](saas-tenancy-wingtip-app-guidance-tips.md) göz atın.

## <a name="provision-additional-tenants"></a>Ek kiracılar sağlama

Havuzlar yalnızca iki adet S3 veritabanı ile uygun maliyetli olabilse de, havuzdaki veritabanı sayısı arttıkça ortalama etki daha uygun maliyetli olur. Performans izleme ve yönetiminin ölçeklenerek nasıl çalıştığını anlamak için bu öğreticide en az 20 veritabanınızın dağıtılmış olması gerekir.

Önceden bir öğreticide bir kiracı grubu zaten saysanız, [tüm kiracı veritabanları bölümündeki Kullanımı Benzetme](#simulate-usage-on-all-tenant-databases) bölümüne atlayın.

1. **PowerShell ISE**, açık ... \\Öğrenme Modülleri\\Performans İzleme\\ve Yönetimi*Demo-PerformanceMonitoringAndManagement.ps1*. Bu öğretici sırasında birkaç senaryo çalıştıracağından bu betiği açık tutun.
1. Set **$DemoScenario** = **1**, Kiracılar Bir **toplu hükmü**
1. Betiği çalıştırmak için **F5**'e basın.

Bu betik, beş dakikadan daha kısa bir süre içinde 17 kiracı dağıtır.

*Yeni KiracıToplu* komut dosyası, varsayılan olarak veritabanı **basetenantdb'ı** katalog sunucusunda kopyalayarak yeni kiracı veritabanlarını oluşturmak için bunları katalogda kaydeden ve son olarak bunları kiracı adı ve mekan türüyle açan bir yığın kiracı oluşturan iç içe veya bağlantılı [Kaynak Yöneticisi](../azure-resource-manager/index.yml) şablonları kümesikullanır. Bu, uygulamanın yeni bir kiracıyı karşılar.'ı karşılar. *Basetenantdb'da* yapılan değişiklikler, daha sonra sağlanan yeni kiracılara uygulanır. *Varolan* kiracı veritabanlarında *(basetenantdb* veritabanı dahil) şema değişikliklerinin nasıl yapılacağını görmek için [Şema Yönetimi öğreticisine](saas-tenancy-schema-management.md) bakın.

## <a name="simulate-usage-on-all-tenant-databases"></a>Tüm kiracı veritabanlarındaki kullanımın benzetimini gerçekleştirme

*Demo-PerformanceMonitoringAndManagement.ps1* komut dosyası, tüm kiracı veritabanlarına karşı çalışan bir iş yükünü simüle sağlanır. Yük, kullanılabilir yük senaryolarından biri kullanılarak oluşturulur:

| Tanıtım | Senaryo |
|:--|:--|
| 2 | Normal yoğunluk yükü oluşturma (yaklaşık 40 DTU) |
| 3 | Veritabanı başına daha uzun ve daha sık artışla yük oluşturma|
| 4 | Veritabanı başına daha yüksek DTU patlamaları ile yük oluşturma (yaklaşık 80 DTU)|
| 5 | Normal bir yük artı tek bir kiracıya yüksek yük oluşturma (yaklaşık 95 DTU)|
| 6 | Birden çok havuzda dengesiz yük oluşturma|

Yük oluşturucu her kiracı veritabanına *yapay* bir yalnızca CPU yükü uygular. Oluşturucu her kiracı veritabanı için yükü oluşturan saklı yordamı düzenli olarak çağıran bir iş başlatır. Yük düzeyleri (eDTU cinsinden), süresi ve aralıkları tüm veritabanlarında farklıdır ve öngörülemez kiracı etkinliğini benzetimi gerçekleştirir.

1. **PowerShell ISE**, açık ... \\Öğrenme Modülleri\\Performans İzleme\\ve Yönetimi*Demo-PerformanceMonitoringAndManagement.ps1*. Bu öğretici sırasında birkaç senaryo çalıştıracağından bu betiği açık tutun.
1. Set **$DemoScenario** = **2**, *Normal yoğunlukyük oluşturun.*
1. Bir yükü tüm kiracı veritabanlarınıza uygulamak için **F5** tuşuna basın.

Wingtip Tickets SaaS Database Per Tenant bir SaaS uygulamasıdır ve bir SaaS uygulamasındaki gerçek yük genellikle düzensiz ve öngörülemezdir. Yük oluşturucu, bunun benzetimini gerçekleştirmek için kiracılar genelinde dağıtılan rastgele yük oluşturur. Yük deseni ortaya çıkması için birkaç dakika gereklidir, bu nedenle aşağıdaki bölümlerde yük izlemek için denemeden önce 3-5 dakika boyunca yük jeneratörçalıştırın.

> [!IMPORTANT]
> Yük oluşturucu, yerel PowerShell oturumunuzda bir dizi iş çalıştırıyor. *Demo-PerformanceMonitoringAndManagement.ps1* sekmesini açık tutun! Sekmeyi kapatırsanız veya makineyi askıya alırsanız, yük oluşturucu durur. Yük jeneratörü, jeneratör başladıktan sonra sağlanan herhangi bir yeni kiracı üzerinde yük üreten bir *iş çağrıştıran* durumda kalır. Yeni iş çağırmak ve komut dosyasından çıkmak için *Ctrl-C'yi* kullanın. Yük jeneratörü çalışmaya devam edecektir, ancak sadece mevcut kiracılar üzerinde.

## <a name="monitor-resource-usage-using-the-azure-portal"></a>Azure portalını kullanarak kaynak kullanımını izleme

Yüklenen yükten kaynaklanan kaynak kullanımını izlemek için, kiracı veritabanlarını içeren havuza portalı açın:

1. Azure [portalını](https://portal.azure.com) açın ve *kiracılar1-dpt-&lt;USER&gt; * sunucusuna göz atın.
1. Aşağı kaydırıp elastik havuzları bulun ve **Pool1**’e tıklayın. Bu havuz o ana kadar oluşturulmuş tüm kiracı veritabanlarını içerir.

**Elastik havuz izleme** ve **Elastik veritabanı izleme** grafiklerini gözlemleyin.

Havuzun kaynak kullanımı, havuzdaki tüm veritabanları için toplam veritabanı kullanımıdır. Veritabanı grafiği en popüler beş veritabanını gösterir:

![veritabanı grafiği](./media/saas-dbpertenant-performance-monitoring/pool1.png)

Havuzda ilk beşin ötesinde ek veritabanları olduğundan, havuz kullanımı ilk beş veritabanları grafiğine yansıtılmayan etkinliği gösterir. Ek ayrıntılar için **Veritabanı Kaynak Kullanımı'nı**tıklatın:

![veritabanı kaynak kullanımı](./media/saas-dbpertenant-performance-monitoring/database-utilization.png)


## <a name="set-performance-alerts-on-the-pool"></a>Havuzda performans uyarılarını ayarlama

\>%75'lik kullanımı tetikleyen havuzda aşağıdaki gibi bir uyarı ayarlayın:

1. [Azure portalında](https://portal.azure.com) *Pool1'i* açın *(kiracılar1-dpt kullanıcı\<\> * sunucusunda) .
1. **Uyarı Kuralları**ve ardından **+ Uyarı ekle**’ye tıklayın:

   ![uyarı ekle](media/saas-dbpertenant-performance-monitoring/add-alert.png)

1. **Yüksek DTU** gibi bir ad belirtin,
1. Aşağıdaki değerleri ayarlayın:
   * **Ölçüm = eDTU yüzdesi**
   * **Koşul = daha büyük**
   * **Eşik = 75**
   * **Periyot = Son 30 dakika içinde**
1. *Ek yönetici e-posta(lar)* kutusuna bir e-posta adresi ekleyin ve **Tamam'ı**tıklatın.

   ![uyarı ayarlama](media/saas-dbpertenant-performance-monitoring/alert-rule.png)


## <a name="scale-up-a-busy-pool"></a>Meşgul bir havuzun ölçeğini artırma

Bir havuzdaki toplam yük düzeyi havuzu kapasitesini aşacak bir noktaya yükselir ve %100 eDTU kullanımına ulaşırsa, bağımsız veritabanı performansı etkilenir ve havuzdaki tüm veritabanları için sorgu yanıt süreleri yavaşlayabilir.

**Kısa vadeli,** ek kaynaklar sağlamak için havuzu ölçeklemeyi veya veritabanlarını havuzdan kaldırmayı (bunları diğer havuzlara taşımayı veya havuz dışına bağımsız bir hizmet katmanına taşımayı) düşünün.

**Uzun vadede,** veritabanı performansını artırmak için sorguları veya dizin kullanımını en iyi duruma almayı düşünün. Uygulamanın performans sorunlarına karşı duyarlılığına bağlı olarak, bir havuzun ölçeğini havuz %100 eDTU kullanımına ulaşmadan artırmak idealdir. Sizi önceden uyarması için bir uyarı ayarlayın.

Oluşturucu tarafından üretilen yükü artırarak meşgul bir havuzun benzetimini gerçekleştirebilirsiniz. Veritabanlarının daha sık ve daha uzun süre patlamasına neden olarak, tek tek veritabanlarının gereksinimlerini değiştirmeden havuzdaki toplam yükü artırır. Havuz ölçeğini, portaldan veya PowerShell’den kolayca artırabilirsiniz. Bu alıştırmada portal kullanılmaktadır.

1. **3** *$DemoScenario* = ayarlayın , Her veritabanının gerektirdiği en yüksek yükü değiştirmeden havuzdaki toplam yükün yoğunluğunu artırmak için veritabanı başına daha uzun ve daha _sık patlamalarla yük oluşturun._
1. Bir yükü tüm kiracı veritabanlarınıza uygulamak için **F5** tuşuna basın.

1. Azure portalındaki **Pool1'e** gidin.

Artırılmış havuz eDTU kullanımını üst grafikte izleyin. Yeni yüksek yük için tekme için birkaç dakika sürer, ama hızlı bir şekilde havuz maksimum kullanım vurmak için başlar görmeniz gerekir, ve yük yeni desen içine sabitlenir gibi, hızla havuzu aşırı yükler.

1. Havuzu büyütmek için **Pool1** sayfasının üst kısmındaki **havuzu Yapılandır'ı** tıklatın.
1. Havuz **eDTU** ayarını **100'e**ayarlayın. Havuz eDTU değerinin değiştirilmesi, veritabanı başına ayarları değiştirmez (veritabanı başına hala en fazla 50 eDTU’dur). **Yapılaşı havuzu** sayfasının sağ tarafında veritabanı başına ayarları görebilirsiniz.
1. Havuzu ölçeklendirmek için isteği göndermek için **Kaydet'i** tıklatın.

İzleme grafiklerini görüntülemek için **Pool1** > **Genel Bakış'a** geri dön. Havuza daha fazla kaynak sağlamanın etkisini izleyin (birkaç veritabanları ve rasgele bir yük olmasına rağmen, bir süre çalışana kadar kesin olarak görmek her zaman kolay değildir). Grafiklere bakarken, üst grafikteki %100 değerinin 100 eDTU’yu, alt grafikteki %100 değerinin ise veritabanı başına en yüksek değer hala 50 eDTU olduğundan 50 eDTU’yu temsil ettiğini aklınızda bulundurun.

İşlem boyunca veritabanları çevrimiçi ve tam olarak kullanılabilir durumdadır. Her veritabanının yeni havuz eDTU değeriyle etkinleştirilmeye hazır olduğu son anda tüm etkin bağlantılar kesilir. Kesilen bağlantıları yeniden denemek için her zaman uygulama kodunun yazılması gerekir, böylece ölçeği artırılmış havuzda veritabanına yeniden bağlanılır.

## <a name="load-balance-between-pools"></a>Havuzlar arasındaki yük dengesi

Havuz ölçeğini artırmanın alternatif bir yolu, ikinci bir havuz oluşturup veritabanlarını bu havuza taşımak ve iki havuz arasındaki yükü dengelemektir. Bunu yapmak için yeni havuzun birinci havuzla aynı sunucuda oluşturulması gerekir.

1. Azure [portalında,](https://portal.azure.com) **&lt;&gt; kiracılar1-dpt- USER** sunucusunu açın.
1. Geçerli sunucuda havuz oluşturmak için **+ Yeni havuzunu** tıklatın.
1. **Elastik havuz** şablonunda:

   1. **Adını** *Pool2*olarak ayarlayın.
   1. Fiyatlandırma katmanını **Standart Havuz** olarak bırakın.
   1. **Havuzu yapılandır**'a tıklayın,
   1. **Havuz eDTU'u** *50 eDTU*olarak ayarlayın.
   1. *Sunucuda Pool2'ye*eklenebilecek veritabanlarının listesini görmek için **veritabanları** ekle'yi tıklatın.
   1. Bunları yeni havuza taşımak için herhangi bir 10 veritabanını seçin ve sonra **Seç'i**tıklatın. Yük jeneratörü çalıştırıyorsanız, hizmet performans profilinizin varsayılan 50 eDTU boyutundan daha büyük bir havuz gerektirdiğini zaten bilir ve 100 eDTU ayarı ile başlamanızı önerir.

      ![Öneri](media/saas-dbpertenant-performance-monitoring/configure-pool.png)

   1. Bu öğretici için varsayılan ı 50 eDTUs'ta bırakın ve yeniden **Seç'i** tıklatın.
   1. Yeni havuzu oluşturmak ve seçili veritabanlarını bu havuza taşımak için **Tamam'ı** seçin.

Havuzu oluşturma ve veritabanlarını taşıma birkaç dakika sürer. Veritabanları taşındıkça çevrimiçi kalır ve son ana kadar tamamen erişilebilir kalır ve bu noktada açık bağlantılar kapatılır. Bazı yeniden deneme mantığınız olduğu sürece, istemciler yeni havuzdaki veritabanına bağlanır.

Havuzu açmak ve performansını izlemek için **Pool2'ye** *(kiracı1-dpt kullanıcı\<\> * sunucusunda) göz atın. Görmüyorsanız, yeni havuzun sağlanmasının tamamlanmasını bekleyin.

Artık *Pool1'deki* kaynak kullanımının düştüğünü ve *Pool2'nin* artık benzer şekilde yüklendiğini görüyorsunuz.

## <a name="manage-performance-of-an-individual-database"></a>Tek bir veritabanının performansını yönetme

Havuzdaki tek bir veritabanı, havuz yapılandırmasına bağlı olarak sürekli bir yüksek yük le karşılarsa, havuzdaki kaynaklara hakim olma ve diğer veritabanlarını etkileme eğiliminde olabilir. Etkinliğin bir süre devam etmesi olasıysa, veritabanı geçici olarak havuzdışına taşınabilir. Bu, veritabanının ihtiyaç duyduğu ek kaynaklara sahip olmasını sağlar ve veritabanını diğer veritabanlarından yalıtlar.

Bu alıştırmada, popüler bir konser için biletler satışa çıktığında yüksek bir yükle karşılaşan Contoso Konser Salonu etkisinin benzetimi gerçekleştirilmektedir.

1. **PowerShell ISE**, açık ... \\ *Demo-PerformanceMonitoringAndManagement.ps1* komut dosyası.
1. **set $DemoScenario = 5, Normal bir yük artı tek bir kiracı (yaklaşık 95 DTU) üzerinde yüksek yük oluşturun.**
1. **$SingleTenantDatabaseName = contosoconcerthall** değerini ayarlayın
1. **F5**’i kullanarak betiği yürütün.


1. Azure [portalında,](https://portal.azure.com) *\<\> kiracılar1-dpt-kullanıcı* sunucusundaki veritabanları listesine göz atın. 
1. **Contosoconcerthall** veritabanına tıklayın.
1. **Contosoconcerthall'un** içinde olduğu havuza tıklayın. **Elastik havuz** bölümündeki havuzu bulun.

1. **Elastik havuz izleme** grafiğini inceleyin ve artan havuz eDTU kullanımını arayın. Bir veya iki dakika sonra, daha yüksek olan yük etkisini göstermeye başlar ve havuzun %100 kullanıma ulaştığını görürsünüz.
2. Son bir saat içinde en sıcak veritabanlarını gösteren **Elastik veritabanı izleme** ekranını inceleyin. *Contosoconcerthall* veritabanı yakında en sıcak beş veritabanlarından biri olarak görünmelidir.
3. **Elastik veritabanı izleme grafiğine tıklayın** **chart** ve veritabanlarından herhangi birini izleyebileceğiniz Veritabanı **Kaynak Kullanımı** sayfasını açar. Bu, *contosoconcerthall* veritabanı için ekranı yalıtmanızı sağlar.
4. Veritabanları listesinden **contosoconcerthall'a**tıklayın.
5. Veritabanı için tek başına bir bilgi işlem boyutu ayarlayabildiğiniz **Yapılandır performans** sayfasını açmak için **Fiyatlandırma Katmanı'nı (ölçek DTUs'ları)** tıklatın.
6. Standart katmanındaki ölçeklendirme seçeneklerini açmak için **Standart** sekmesine tıklayın.
7. **DTU kaydırıcısını** sağa kaydırarak **100** DTU seçin. Bu hizmet hedefi, **S3**karşılık gelir unutmayın.
8. Veritabanını havuzdışına taşımak ve *Standart S3* veritabanı yapmak için **Uygula'yı** tıklatın.
9. Ölçekleme tamamlandıktan sonra, elastik havuz ve veritabanı bıçakları üzerinde contosoconcerthall veritabanı ve Pool1 üzerindeki etkisini izleyin.

Contosoconcerthall veritabanındaki yüksek yük azaldıktan sonra maliyetini azaltmak için derhal havuza geri vermelisiniz. Bunun ne zaman gerçekleşeceği belli değilse, dtu kullanımı havuzdaki veritabanı başına maksimumun altına düştüğünde tetikleyecek bir veritabanı nda bir uyarı ayarlayabilirsiniz. Veritabanını havuza taşıma işlemi 5. alıştırmada açıklanmıştır.

## <a name="other-performance-management-patterns"></a>Diğer performans yönetimi kalıpları

**Önleyici ölçekleme** Yalıtılmış bir veritabanını nasıl ölçeklendirebileceğinizi keşfettiğiniz yukarıdaki alıştırmada, hangi veritabanını arayacağınızı biliyordunuz. Eğer Contoso Konser Salonu'nun yönetimi Wingtips'e yaklaşan bilet satışı hakkında bilgi vermiş olsaydı, veritabanı havuzdan önceden taşınabilirdi. Aksi takdirde, neler olduğunu belirlemek için havuz veya veritabanı üzerinde bir uyarı verilmesini isteyebilirdi. Bu durumu, havuzda performans düşüklüğünden şikayetçi olan diğer kiracılardan öğrenmek istemezsiniz. Kiracı ek kaynaklara ne süreyle ihtiyaç duyduğunu öngörebiliyorsa, tanımlanmış bir zamanlamaya göre veritabanını havuz dışına ve sonra tekrar içine taşımak için bir Azure Otomasyonu runbook'u ayarlayabilirsiniz.

**Kiracı self-servis ölçeklendirme** Ölçeklendirme, yönetim API’si aracılığıyla kolayca çağrılan bir görev olduğundan, kiracıya yönelik uygulamanızda kiracı veritabanlarını ölçeklendirme özelliğini kolayca oluşturabilir ve SaaS hizmetinizin bir özelliği olarak sunabilirsiniz. Örneğin, kiracıların ölçek artırma ve azaltma işlemini kendi kendine yönetmesine, belki de doğrudan faturalarına bağlamasına izin verebilirsiniz!

**Kullanım düzenlerine uygun bir zamanlama ile havuz ölçeğini artırma ve azaltma**

Toplu kiracı kullanımının öngörülebilir kullanım düzenlerini takip ettiği durumlarda, belirli bir zamanlamaya göre bir havuzun ölçeğini artırıp azaltmak için Azure Otomasyonu’nu kullanabilirsiniz. Örneğin, kaynak gereksinimlerinde düşüş olduğunu bildiğiniz hafta içi günlerinde havuz ölçeğini akşam 6’dan sonra azaltıp sabah 6’dan önce yeniden artırabilirsiniz.



## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
> * Sağlanan bir yük oluşturucuyu çalıştırarak kiracı veritabanlarındaki kullanımın benzetimini gerçekleştirme
> * Yükteki artışa yanıt veren kiracı veritabanlarını izleme
> * Artan veritabanı yüküne yanıt olarak Elastik havuzun ölçeğini artırma
> * Veritabanı etkinliğinin yük dengelemesini yapmak üzere ikinci bir Elastik havuz sağlama

[Tek bir kiracıyı geri yükleme öğreticisi](saas-dbpertenant-restore-single-tenant.md)


## <a name="additional-resources"></a>Ek kaynaklar

* [Wingtip Tickets SaaS Veritabanı Başına Kiracı uygulama dağıtımı üzerine inşa](saas-dbpertenant-wingtip-app-overview.md#sql-database-wingtip-saas-tutorials) ek öğreticiler
* [SQL Elastik havuzları](sql-database-elastic-pool.md)
* [Azure otomasyonu](../automation/automation-intro.md)
* [Azure Monitor günlükleri](saas-dbpertenant-log-analytics.md) - Azure Monitor günlükleri öğreticinin ayarlanması ve kullanılması
