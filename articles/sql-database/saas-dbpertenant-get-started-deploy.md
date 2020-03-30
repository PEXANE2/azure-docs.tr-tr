---
title: Veritabanı başına kiracı SaaS öğretici
description: Azure SQL Veritabanı'nı kullanarak kiracı başına veritabanı deseni ve diğer SaaS desenlerini gösteren Wingtip Tickets SaaS çok kiracılı uygulamasını dağıtın ve keşfedin.
services: sql-database
ms.service: sql-database
ms.subservice: scenario
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: MightyPen
ms.author: genemi
ms.reviewer: sstein
ms.date: 01/25/2019
ms.openlocfilehash: 31f712f80ee2492e4bbaec99bd093d46f9d04e6d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "73824005"
---
# <a name="deploy-and-explore-a-multitenant-saas-app-that-uses-the-database-per-tenant-pattern-with-sql-database"></a>SQL Veritabanı ile kiracı başına veritabanı deseni kullanan çok kiracılı bir SaaS uygulamasını dağıtın ve keşfedin

Bu eğitimde, Wingtip Tickets SaaS veritabanı-kiracı başına uygulamasını (Wingtip) dağıtıyor ve keşfediyorsun. Uygulama, birden çok kiracının verilerini depolamak için kiracı başına veritabanı deseni kullanır. Uygulama, Azure SQL Veritabanı'nın SaaS senaryolarını etkinleştirmeyi kolaylaştıran özelliklerini sergilemek üzere tasarlanmıştır.

**Azure'a Dağıt'ı**seçtikten beş dakika sonra çok kiracılı bir SaaS uygulamanız vardır. Uygulama, bulutta çalışan bir SQL veritabanı içerir. Uygulama, her biri kendi veritabanına sahip üç örnek kiracıyla dağıtılır. Tüm veritabanları bir SQL elastik havuza dağıtılır. Uygulama Azure aboneliğinize dağıtılır. Uygulamanın tek tek bileşenlerini keşfetmek ve onlarla çalışmak için tam erişime sahipsiniz. Uygulama C # kaynak kodu ve yönetim komut [wingtipTicketsSaaS-DbPerTenant GitHub repo][github-wingtip-dpt]mevcuttur.

Bu öğreticide şunları öğrenirsiniz:

> [!div class="checklist"]
> - Wingtip SaaS uygulaması nasıl dağıtılır?
> - Uygulama kaynak kodu ve yönetim komut dosyaları nereden alınacağını.
> - Uygulamayı oluşturan sunucular, havuzlar ve veritabanları hakkında.
> - Nasıl kiracı *katalog*ile kendi verilerine eşlenir.
> - Nasıl yeni bir kiracı sağlamak için.
> - Uygulamada kiracı etkinliğini izlemek için nasıl.

[İlgili öğreticiler](saas-dbpertenant-wingtip-app-overview.md#sql-database-wingtip-saas-tutorials) bir dizi çeşitli SaaS tasarım ve yönetim desenleri keşfetmek için sunuyor. Öğreticiler bu ilk dağıtımın ötesine geçmektedir. Öğreticileri kullandığınızda, farklı SaaS desenlerinin nasıl uygulandığını görmek için sağlanan komut dosyalarını inceleyebilirsiniz. Komut dosyaları, SQL Veritabanı özelliklerinin SaaS uygulamalarının gelişimini nasıl basitleştirttiklerini gösterir.

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiyi tamamlamak için Azure PowerShell'in yüklü olduğundan emin olun. Daha fazla bilgi için bkz. [Azure PowerShell kullanmaya başlayın](https://docs.microsoft.com/powershell/azure/get-started-azureps).

## <a name="deploy-the-wingtip-tickets-saas-application"></a>Wingtip Biletleri SaaS uygulamasını dağıtın

### <a name="plan-the-names"></a>Adları planla

Bu bölümün adımlarında, kaynak adlarının genel olarak benzersiz olduğundan emin olmak için kullanılan bir kullanıcı değeri sağlarsınız. Ayrıca, uygulamanın dağıtımı yla oluşturulan tüm kaynakları içeren kaynak grubu için bir ad da sağlarsınız. Ann Finley adında hayali bir kişi için, biz öneririz:

- **Kullanıcı**: *af1* Ann Finley'in baş harflerinden ve bir rakamdan oluşur. Uygulamayı ikinci kez dağıtıyorsanız, farklı bir değer kullanın. Bir örnek af2 olduğunu.
- **Kaynak grubu**: *wingtip-dpt-af1* bunun kiracı başına veritabanı uygulaması olduğunu gösterir. Kaynak grup adını içerdiği kaynakların adlarıyla ilişkilendirmek için af1 kullanıcı adını ekle.

Şimdi isimlerinizi seçin ve yazın.

### <a name="steps"></a>Adımlar

1. Azure portalında Wingtip Tickets SaaS veritabanı başına kiracı dağıtım şablonu açmak **için Azure'a Dağıt'ı**seçin.

   <a href="https://aka.ms/deploywingtipdpt" target="_blank"><img src="https://azuredeploy.net/deploybutton.png"/></a>

1. Gerekli parametreler için şablona değerleri girin.

    > [!IMPORTANT]
    > Bazı kimlik doğrulama ve sunucu güvenlik duvarları, gösteri amacıyla kasıtlı olarak güvenli değildir. Yeni bir kaynak grubu oluşturmanızı öneririz. Varolan kaynak gruplarını, sunucuları veya havuzları kullanmayın. Bu uygulamayı, komut dosyalarını veya dağıtılmış kaynakları üretim için kullanmayın. İlgili faturalandırmayı durdurmak için uygulamanız bittiğinde bu kaynak grubunu silin.

    - **Kaynak grubu**: **Yeni Oluştur'u**seçin ve kaynak grubu için daha önce seçtiğiniz benzersiz adı sağlayın.
    - **Konum**: Açılan listeden bir konum seçin.
    - **Kullanıcı**: Daha önce seçtiğiniz kullanıcı adı değerini kullanın.

1. Uygulamayı dağıtın.

    a. Hüküm ve koşulları kabul etmek için seçin.

    b. **Satın al**'ı seçin.

1. Dağıtım durumunu izlemek için **Bildirimler'i** (arama kutusunun sağındaki çan simgesi) seçin. Wingtip Tickets SaaS uygulamasının dağıtımı yaklaşık beş dakika sürer.

   ![Dağıtım başarılı oldu](media/saas-dbpertenant-get-started-deploy/succeeded.png)

## <a name="download-and-unblock-the-wingtip-tickets-management-scripts"></a>Wingtip Tickets yönetim komut dosyalarını indirin ve engelini kaldırın

Uygulama dağıtılırken, kaynak kodu ve yönetim komut dosyalarını indirin.

> [!IMPORTANT]
> Yürütülebilir içerikler (komut dosyaları ve DL'ler), .zip dosyaları harici bir kaynaktan indirilip ayıklandığında Windows tarafından engellenebilir. Komut dosyalarını ayıklamadan önce .zip dosyasının engelini kaldırmak için adımları izleyin. Engellemeyi kaldırmak, komut dosyalarının çalışmasına izin verildiğini sağlar.

1. [WingtipTicketsSaaS-DbPerTenant GitHub repo][github-wingtip-dpt]göz atın.
1. **Clone or download**'u (Kopyala veya indir) seçin.
1. **ZIP İndir'i**seçin ve ardından dosyayı kaydedin.
1. **WingtipTicketsSaaS-DbPerTenant-master.zip** dosyasına sağ tıklayın ve ardından **Özellikler'i**seçin.
1. **Genel** sekmesinde Uygula **engelini kaldır'ı** > **seçin.**
1. **Tamam'ı**seçin ve dosyaları ayıklayın

Scriptler bulunmaktadır ... \\WingtipTicketsSaaS-DbPerTenant-master\\Öğrenme Modülleri klasörü.

## <a name="update-the-user-configuration-file-for-this-deployment"></a>Bu dağıtım için kullanıcı yapılandırma dosyasını güncelleştirme

Herhangi bir komut dosyasını çalıştırmadan önce, Kullanıcı Config dosyasındaki kaynak grubunu ve kullanıcı değerlerini güncelleştirin. Bu değişkenleri dağıtım sırasında kullandığınız değerlere ayarlayın.

1. PowerShell ISE, açık ... \\Öğrenme Modülleri\\**UserConfig.psm1**
1. **KaynakGroupName** ve **Adını** dağıtımınız için belirli değerlerle güncelleştirin (yalnızca 10 ve 11. satırlarda).
1. Değişiklikleri kaydedin.

Bu değerler hemen hemen her komut dosyasında başvurulmaktadır.

## <a name="run-the-application"></a>Uygulamayı çalıştırma

Uygulama, etkinliklere ev sahipliği yapan mekanları sergiler. Mekan türleri arasında konser salonları, caz kulüpleri ve spor kulüpleri yer almaktadır. Wingtip Biletleri'nde mekânlar kiracı olarak kaydedilir. Kiracı olmak, bir mekana etkinlikleri listeletmek ve müşterilerine bilet satmak için kolay bir yol sağlar. Her mekan kendi olayları listelemek ve bilet satmak için kişiselleştirilmiş bir web sitesi alır.

Uygulamada dahili olarak, her kiracı bir SQL veritabanı sql elastik havuza dağıtılan alır.

Merkezi **Olaylar Hub** sayfası, dağıtımınızdaki kiracılara yapılan bağlantıların bir listesini sağlar.

1. Web tarayıcınızda Etkinlikler Hub'ını açmak http://events.wingtip-dpt.&ltiçin&gt;URL'yi kullanın: ;kullanıcı .trafficmanager.net. &lt;Kullanıcıyı&gt; dağıtımınızın kullanıcı değeriyle değiştirin.

    ![Etkinlikler Merkezi](media/saas-dbpertenant-get-started-deploy/events-hub.png)

2. Etkinlikler Merkezi'nde **Fabrikam Jazz Club'ı** seçin.

    ![Olaylar](./media/saas-dbpertenant-get-started-deploy/fabrikam.png)

### <a name="azure-traffic-manager"></a>Azure Traffic Manager

Wingtip uygulaması, gelen isteklerin dağıtımını denetlemek için [*Azure Trafik Yöneticisi'ni*](../traffic-manager/traffic-manager-overview.md) kullanır. Belirli bir kiracının etkinlikler sayfasına erişmek için URL aşağıdaki biçimi kullanır:

- http://events.wingtip-dpt.&lt;kullanıcı&gt;.trafficmanager.net/fabrikamjazzclub

    Önceki biçimin bölümleri aşağıdaki tabloda açıklanmıştır.

    | URL bölümü        | Açıklama       |
    | :-------------- | :---------------- |
    | http://events.wingtip-dpt | Wingtip uygulamasının etkinlik bölümleri.<br /><br /> *-dpt,* Wingtip Biletlerinin *kiracı başına veritabanı* uygulamasını diğer uygulamalardan ayırır. Örnekler tek *single* app-per-kiracı (*-sa*) veya *çok kiracıveritabanı* (*-mt*) uygulamalarıdır. |
    | . * &lt;kullanıcı&gt;* | örnekte *af1.* |
    | .trafficmanager.net/ | Trafik Yöneticisi, temel URL. |
    | fabrikamjazzclub | Fabrikam Jazz Club adlı kiracıyı tanımlar. |
    | &nbsp; | &nbsp; |

- Kiracı adı, etkinlik uygulaması tarafından URL'den ayrıştırılır.
- Kiracı adı bir anahtar oluşturmak için kullanılır.
- Anahtar, kiracının veritabanının konumunu edinmek için kataloga erişmek için kullanılır.
  - Katalog *shard harita yönetimi*kullanılarak uygulanır.
- Olaylar Hub'ı, her kiracı için etkinlik listesi sayfası URL'lerini oluşturmak için katalogda genişletilmiş meta verileri kullanır.

Üretim ortamında, genellikle [*bir şirketin internet etki alanını*](../traffic-manager/traffic-manager-point-internet-domain.md) Trafik Yöneticisi DNS adına işaret eden bir CNAME DNS kaydı oluşturursunuz.

> [!NOTE]
> Trafik yöneticisinin bu öğreticide ne olduğu hemen belli olmayabilir. Öğreticiler bu serinin amacı karmaşık bir üretim ortamının ölçeğini işleyebilir desenleri vitrin etmektir. Böyle bir durumda, örneğin, dünya çapında dağıtılan, veritabanlarıyla birlikte bulunan birden çok web uygulaması na sahip olur sunuz ve bu örnekler arasında yol yönlendirmek için trafik yöneticisine ihtiyacınız olacaktır.
Trafik yöneticisi nin kullanımını gösteren bir diğer öğreticiler kümesi ise [coğrafi geri yükleme](saas-dbpertenant-dr-geo-restore.md) ve coğrafi [çoğaltma](saas-dbpertenant-dr-geo-replication.md) öğreticileridir. Bu eğitimlerde, trafik yöneticisi bölgesel bir kesinti durumunda SaaS uygulamasının kurtarma örneğine geçmeye yardımcı olmak için kullanılır.

## <a name="start-generating-load-on-the-tenant-databases"></a>Kiracı veritabanları üzerinde yük oluşturmaya başlama

Şimdi uygulama dağıtıldı, bunu işe koyalım.

*Demo-LoadGenerator* PowerShell komut dosyası, tüm kiracı veritabanlarına karşı çalışan bir iş yükü başlatır. Birçok SaaS uygulamasındaki gerçek yük düzensiz ve öngörülemezdir. Bu tür bir yükü simüle etmek için, jeneratör her kiracı üzerinde randomize sivri veya etkinlik patlamaları ile bir yük üretir. Patlamalar rastgele aralıklarla meydana gelir. Yük deseninin ortaya çıkması birkaç dakika sürer. Yükü izlemeden önce jeneratörün en az üç veya dört dakika çalışmasına izin verin.

1. PowerShell ISE, açık ... \\Öğrenme Modülleri\\\\Utilities*Demo-LoadGenerator.ps1* script.
2. Komut dosyasını çalıştırmak ve yük jeneratörü başlatmak için F5 tuşuna basın. Varsayılan parametre değerlerini şimdilik bırakın.
3. Azure hesabınızda oturum açın ve gerekirse kullanmak istediğiniz aboneliği seçin.

Yük jeneratörü komut dosyası, katalogdaki her veritabanı için bir arka plan işi başlatır ve ardından durur. Yük jeneratörü komut dosyasını yeniden çalıştırıyorsanız, yenilerini başlatmadan önce çalışan tüm arka plan işlerini durdurur.

### <a name="monitor-the-background-jobs"></a>Arka plan daki işleri izleme

Arka plandaki işleri kontrol etmek ve izlemek istiyorsanız, aşağıdaki cmdlets'i kullanın:

- `Get-Job`
- `Receive-Job`
- `Stop-Job`

### <a name="demo-loadgeneratorps1-actions"></a>Demo-LoadGenerator.ps1 eylemleri

*Demo-LoadGenerator.ps1* müşteri işlemlerinin aktif bir iş yükünü taklit eder. Aşağıdaki adımlar *Demo-LoadGenerator.ps1* başlattı eylemlerin sırasını açıklar:

1. *Demo-LoadGenerator.ps1* ön planda *LoadGenerator.ps1* başlar.

    - Her iki .ps1 dosyaları klasörleri Öğrenme\\Modülleri\\Utilities altında saklanır.

2. *LoadGenerator.ps1* katalogdaki tüm kiracı veritabanları üzerinden döngüler.

3. *LoadGenerator.ps1* her kiracı veritabanı için bir arka plan PowerShell iş başlar:

    - Varsayılan olarak, arka plan işleri 120 dakika çalışır.
    - Her *iş, sp_CpuLoadGenerator*çalıştırarak bir kiracı veritabanında CPU tabanlı bir yüke neden olur. Yükün yoğunluğu ve süresi ne bağlı `$DemoScenario`olarak değişir.
    - *sp_CpuLoadGenerator* yüksek CPU yüküne neden olan bir SQL SELECT deyimietrafında döngüler. SELECT sorunları arasındaki zaman aralığı denetlenebilir bir CPU yükü oluşturmak için parametre değerlerine göre değişir. Yük düzeyleri ve aralıkları daha gerçekçi yükleri simüle etmek için randomize edilir.
    - Bu .sql dosyası *WingtipTenantDB\\dbo\\StoredProcedures\\*altında depolanır.

4. Eğer `$OneTime = $false`, yük jeneratörü arka plan işleri başlar ve daha sonra çalıştırmaya devam ediyor. Her 10 saniyede bir, sağlanan herhangi bir yeni kiracı için izler. Ayarlarsanız, `$OneTime = $true`LoadGenerator arka plan işlerini başlatır ve ön planda çalışmayı durdurur. Bu öğretici için, bırakın. `$OneTime = $false`

   Yük jeneratörünü durdurmak veya yeniden başlatmak istiyorsanız Ctrl-C veya Stop Operation Ctrl-Break'i kullanın.

   Yük jeneratörünü ön planda bırakırsanız, diğer PowerShell komut dosyalarını çalıştırmak için başka bir PowerShell ISE örneğini kullanın.

&nbsp;

Bir sonraki bölüme devam etmeden önce, yük jeneratörünü iş çağrıştıran durumda çalıştırın.

## <a name="provision-a-new-tenant"></a>Yeni bir kiracı sağlama

İlk dağıtım üç örnek kiracı oluşturur. Şimdi dağıtılan uygulama üzerindeki etkisini görmek için başka bir kiracı oluşturun. Wingtip uygulamasında, yeni kiracıların sağlanmasıiçin iş [akışı, Hüküm ve katalog öğreticisinde](saas-dbpertenant-provision-and-catalog.md)açıklanmıştır. Bu aşamada, bir dakikadan az süren yeni bir kiracı oluşturursunuz.

1. Yeni bir PowerShell ISE açın.
2. Açık... \\Öğrenme Modülleri\Provizyon\\ve Katalog*Demo-ProvisionAndCatalog.ps1*.
3. Komut dosyasını çalıştırmak için F5 tuşuna basın. Varsayılan değerleri şimdilik bırakın.

   > [!NOTE]
   > Birçok Wingtip SaaS komut dosyası, diğer komut dosyalarındaki işlevleri aramak için klasörlere göz atmak için *$PSScriptRoot* kullanır. Bu değişken yalnızca tam komut dosyası F5 tuşuna basılarak yürütüldüğünde değerlendirilir.F8 ile bir seçimi vurgulama ve çalıştırma hatalara neden olabilir. Komut dosyalarını çalıştırmak için F5 tuşuna basın.

Yeni kiracı veritabanı:

- SQL elastik havuzunda oluşturulur.
- Başlatılan.
- Katalogda kayıtlı.

Başarılı bir sağlamadan sonra, yeni kiracının *Etkinlikler* sitesi tarayıcınızda görünür.

![Yeni kiracı](./media/saas-dbpertenant-get-started-deploy/red-maple-racing.png)

Yeni kiracının listede görünmesini sağlamak için Etkinlikler Hub'ını yenileyin.

## <a name="explore-the-servers-pools-and-tenant-databases"></a>Sunucuları, havuzları ve kiracı veritabanlarını öğrenme

Artık kiracı koleksiyonuna karşı bir yük çalıştırmaya başladığınıza göre, dağıtılan bazı kaynaklara bakalım.

1. Azure [portalında,](https://portal.azure.com)SQL sunucuları listenize göz atın. Daha sonra **&lt;katalog-dpt-&gt; USER** sunucusunu açın.
    - Katalog sunucusu iki veritabanı, **kiracı kataloğu** ve **basetenantdb** (yeni kiracılar oluşturmak için kopyalanan bir şablon veritabanı) içerir.

   ![Veritabanları](./media/saas-dbpertenant-get-started-deploy/databases.png)

2. SQL sunucuları listenize geri dön.

3. Kiracı veritabanlarını tutan **kiracı1-dpt-&lt;USER&gt; ** sunucusunu açın.

4. Aşağıdaki öğelere bakın:

    - Her kiracı veritabanı, 50 eDTU standart havuzunda bulunan **Elastik Standart** veritabanıdır.
    - Red Maple Racing veritabanı, daha önce sağlanmış kiracı veritabanıdır.

   ![Veritabanları ile sunucu](./media/saas-dbpertenant-get-started-deploy/server.png)

## <a name="monitor-the-pool"></a>Havuzu izleme

*LoadGenerator.ps1* birkaç dakika çalışır sonra, yeterli veri bazı izleme yetenekleri bakarak başlamak için kullanılabilir olmalıdır. Bu özellikler havuzlar ve veritabanları yerleşiktir.

Sunucu **kiracı1-dpt-&lt;kullanıcıya&gt;** göz atın ve havuz için kaynak kullanımını görüntülemek için **Pool1'i** seçin. Aşağıdaki grafiklerde, yük jeneratörü bir saat boyunca çalıştırılabın.

   ![Monitör havuzu](./media/saas-dbpertenant-get-started-deploy/monitor-pool.png)

- **Kaynak kullanımı**etiketli ilk grafik, havuz eDTU kullanımını gösterir.
- İkinci grafik, havuzdaki en etkin beş veritabanının eDTU kullanımını gösterir.

İki grafik, elastik havuzların ve SQL Veritabanının öngörülemeyen SaaS uygulama iş yüklerine uygun olduğunu göstermektedir. Grafikler, her biri 40 eDTU'ya kadar dört veritabanının patladığını ve ancak tüm veritabanlarının 50 eDTU havuzu tarafından rahatlıkla desteklenerek desteklendiğini göstermektedir. 50 eDTU havuzu daha da ağır iş yüklerini destekleyebilir. Veritabanları tek veritabanları olarak sağlanıyorsa, patlamaları desteklemek için her birinin bir S2 (50 DTU) olması gerekir. Dört tek S2 veritabanlarının maliyeti havuzun fiyatının yaklaşık üç katıdır. Gerçek dünyadaki durumlarda, SQL Veritabanı müşterileri 200 eDTU havuzunda 500 veritabanına kadar çalışır. Daha fazla bilgi için Performans izleme öğreticisi'ne bakın. [Performance monitoring tutorial](saas-dbpertenant-performance-monitoring.md)

## <a name="additional-resources"></a>Ek kaynaklar

- Daha fazla bilgi için [Wingtip Tickets SaaS veritabanı başına kiracı uygulaması na dayalı](saas-dbpertenant-wingtip-app-overview.md#sql-database-wingtip-saas-tutorials)ek eğitimlere bakın.
- Esnek havuzlar hakkında bilgi edinmek için Azure [SQL elastik havuzu nedir?](sql-database-elastic-pool.md)
- Esnek işler hakkında bilgi edinmek için [bkz.](elastic-jobs-overview.md)
- Çok kiracılı SaaS uygulamaları hakkında bilgi edinmek [için, çok kiracılı SaaS uygulamaları için tasarım desenleri bölümüne](saas-tenancy-app-design-patterns.md)bakın.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide şunları öğrendiniz:

> [!div class="checklist"]
> - Wingtip Tickets SaaS uygulaması nasıl dağıtılır?
> - Uygulamayı oluşturan sunucular, havuzlar ve veritabanları hakkında.
> - Nasıl kiracı *katalog*ile kendi verilerine eşlenir.
> - Nasıl yeni kiracılar sağlamak için.
> - Kiracı etkinliğini izlemek için havuz kullanımını görüntüleme.
> - İlgili faturalandırmayı durdurmak için örnek kaynaklar nasıl silinir?

Ardından, [Hüküm ve katalog öğretici](saas-dbpertenant-provision-and-catalog.md)deneyin.

<!-- Link references. -->

[github-wingtip-dpt]: https://github.com/Microsoft/WingtipTicketsSaaS-DbPerTenant
