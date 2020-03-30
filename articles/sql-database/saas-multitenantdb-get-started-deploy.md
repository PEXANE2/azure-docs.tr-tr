---
title: Kırık çok kiracılı bir veritabanı SaaS uygulamasını dağıtma
description: Azure SQL Veritabanı'nı kullanarak SaaS desenlerini gösteren kırık Wingtip Tickets SaaS çok kiracılı veritabanı uygulamasını dağıtın ve keşfedin.
services: sql-database
ms.service: sql-database
ms.subservice: scenario
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: MightyPen
ms.author: genemi
ms.reviewer: billgib, stein
ms.date: 10/16/2018
ms.openlocfilehash: 3277318e01362df8fc21ff7ca769aaeb8006abc6
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "73828003"
---
# <a name="deploy-and-explore-a-sharded-multi-tenant-application"></a>Parçalı çok kiracılı bir uygulamayı dağıtma ve keşfetme

Bu eğitimde, Wingtip Tickets adlı örnek çok kiracılı SaaS uygulamasını dağıtıyor ve keşfe sunuz. Wingtip Biletleri uygulaması, Azure SQL Veritabanı'nın SaaS senaryolarının uygulanmasını kolaylaştıran özelliklerini sergilemek üzere tasarlanmıştır.

Wingtip Tickets uygulamasının bu uygulaması, çok kiracılı çok kiracılı bir veritabanı deseni kullanır. Kırık kiracı tanımlayıcı tarafından. Kiracı verileri, kiracı tanımlayıcı değerlerine göre belirli bir veritabanına dağıtılır. 

Bu veritabanı deseni, her parçada veya veritabanında bir veya daha fazla kiracı depolamanızı sağlar. Her veritabanının birden çok kiracı tarafından paylaşılmasını sağlayarak en düşük maliyetle en iyi duruma getirebilirsiniz. Veya her veritabanı yalnızca bir kiracı depolayarak yalıtım için optimize edebilirsiniz. Optimizasyon seçiminiz her belirli kiracı için bağımsız olarak yapılabilir. Seçiminiz kiracı ilk depolandığında yapılabilir veya daha sonra fikrinizi değiştirebilirsiniz. Uygulama her iki şekilde de çalışacak şekilde tasarlanmıştır.

## <a name="app-deploys-quickly"></a>Uygulama hızla dağıtılır

Uygulama Azure bulutu içinde çalışır ve Azure SQL Veritabanı'nı kullanır. Aşağıdaki dağıtım bölümü mavi **Azure'a Dağıt** düğmesini sağlar. Düğmeye basıldığında, uygulama beş dakika içinde Azure aboneliğinize tamamen dağıtılır. Tek tek uygulama bileşenleriyle çalışmaya tam erişime sahipsiniz.

Uygulama üç örnek kiracı için veri ile dağıtılır. Kiracılar tek bir çok kiracıveritabanında birlikte depolanır.

Herkes [GitHub deposundan][link-github-wingtip-multitenantdb-55g]Wingtip Biletleri için C# ve PowerShell kaynak kodunu indirebilirsiniz.

## <a name="learn-in-this-tutorial"></a>Bu öğreticide öğrenin

> [!div class="checklist"]
> - Wingtip Tickets SaaS uygulaması nasıl dağıtılır?
> - Uygulama kaynak kodu ve yönetim komut dosyaları nereden alınacağını.
> - Uygulamayı oluşturan sunucular ve veritabanları hakkında.
> - Nasıl kiracı *katalog*ile kendi verilerine eşlenir.
> - Nasıl yeni bir kiracı sağlamak için.
> - Uygulamada kiracı etkinliğini izlemek için nasıl.

Bu ilk dağıtım üzerine inşa ilgili öğreticiler bir dizi kullanılabilir. Öğreticiler SaaS tasarım ve yönetim desenleri bir dizi keşfetmek. Öğreticiler aracılığıyla çalışırken, farklı SaaS desenlerinin nasıl uygulandığını görmek için sağlanan komut dosyalarını gözden geçirmeniz için teşvik edilirsiniz.

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiyi tamamlamak için aşağıdaki ön koşulların karşılandığından emin olun:

- En son Azure PowerShell yüklenir. Ayrıntılar için Azure [PowerShell ile başlarken][link-azure-get-started-powershell-41q]bakın.

## <a name="deploy-the-wingtip-tickets-app"></a>Wingtip Biletleri uygulamasını dağıtın

### <a name="plan-the-names"></a>Adları planla

Bu bölümün adımlarında, kaynak adlarının genel olarak benzersiz olmasını sağlamak için kullanılan bir *kullanıcı* değeri ve uygulamanın dağıtımı yla oluşturulan tüm kaynakları içeren *kaynak grubu* için bir ad sağlarsınız. *Ann Finley*adında bir kişi için, biz öneririz:
- *Kullanıcı:* **af1**  *(Baş harfleri, artı bir rakam. Uygulamayı ikinci kez dağıtıyorsanız farklı bir değer (örn. af2) kullanın.)*
- *Kaynak grubu:* **wingtip-mt-af1** *(wingtip-mt bu kırık çok kiracı uygulaması olduğunu gösterir. Kullanıcı adı af1 ekleyen kaynak grup adı içerdiği kaynakların adlarıyla ilişkilidir.)*

Şimdi isimlerinizi seçin ve yazın. 

### <a name="steps"></a>Adımlar

1. Azure'a aşağıdaki mavi **Dağıt düğmesini** tıklatın.
   - Wingtip Tickets SaaS dağıtım şablonuyla Azure portalını açar.

     [![Azure'a Dağıt düğmesi.][image-deploy-to-azure-blue-48d]][link-aka-ms-deploywtp-mtapp-52k]

1. Dağıtım için gerekli parametre değerlerini girin.

    > [!IMPORTANT]
    > Bu gösteri için, önceden varolan kaynak gruplarını, sunucuları veya havuzları kullanmayın. Bunun yerine, **yeni bir kaynak grubu oluştur'u**seçin. İlgili faturalandırmayı durdurmak için uygulamayla işiniz bittiğinde bu kaynak grubunu silin.
    > Bu uygulamayı veya oluşturduğu kaynakları üretim için kullanmayın. Kimlik doğrulamanın bazı yönleri ve sunucu güvenlik duvarı ayarları, uygulamayı gösterikolaylaştırmak için kasıtlı olarak güvensizdir.

    - **Kaynak grubu** için - **Yeni Oluştur'u**seçin ve ardından kaynak grubu için bir **Ad** sağlayın (büyük/küçük harf duyarlı).
        - Açılan listeden bir **Konum** seçin.
    - **Kullanıcı** için - Kısa bir **Kullanıcı** değeri seçmenizi öneririz.

1. **Uygulamayı dağıtın**.

    - Hüküm ve koşulları kabul etmek için tıklayınız.
    - **Satın al**’a tıklayın.

1. Arama kutusunun sağındaki çan simgesi olan **Bildirimler'i**tıklatarak dağıtım durumunu izleyin. Wingtip uygulamasını dağıtmak yaklaşık beş dakika sürer.

   ![dağıtım başarılı](media/saas-multitenantdb-get-started-deploy/succeeded.png)

## <a name="download-and-unblock-the-management-scripts"></a>Yönetim komut dosyalarını indirin ve engelini kaldırın

Uygulama dağıtılırken, uygulama kaynak kodunu ve yönetim komut dosyalarını indirin.

> [!NOTE]
> Yürütülebilir içerikler (komut dosyaları, DL'ler), zip dosyaları harici bir kaynaktan indirilip ayıklandığında Windows tarafından engellenebilir. Bir zip dosyasından komut dosyalarını ayıklarken, ayıklamadan önce .zip dosyasının engelini kaldırmak için aşağıdaki adımları kullanın. .zip dosyasının engelini kaldırarak, komut dosyalarının çalışmasına izin verildiğini garanti elersiniz.

1. [WingtipTicketsSaaS-MultiTenantDb GitHub repo](https://github.com/Microsoft/WingtipTicketsSaaS-MultiTenantDb)göz atın.
2. **Clone'u tıklatın veya indirin.**
3. **ZIP İndir'i** ve dosyayı kaydedin'i tıklatın.
4. **WingtipTicketsSaaS-MultiTenantDb-master.zip** dosyasına sağ tıklayın ve **Özellikleri**seçin.
5. **Genel** sekmesinde, **Engeli Kaldır'ı**seçin ve **Uygula'yı**tıklatın.
6. **Tamam**'a tıklayın.
7. Dosyaları ayıklayın.

Komut dosyaları ... * WingtipTicketsSaaS-MultiTenantDb-master\\Öğrenme\\ Modülleri klasörü. \\*

## <a name="update-the-configuration-file-for-this-deployment"></a>Bu dağıtım için yapılandırma dosyasını güncelleştirme

Herhangi bir komut dosyası çalıştırmadan *önce, Kaynak grubunu* ve *kullanıcı* değerlerini **UserConfig.psm1'de**ayarlayın. Bu değişkenleri dağıtım sırasında ayarladığınız değerlere ayarlayın.

1. Açık... \\ *PowerShell*\\ISE Öğrenme Modülleri*UserConfig.psm1* .
2. *KaynakGroupName* ve *Adını* dağıtımınız için belirli değerlerle güncelleştirin (yalnızca 10 ve 11. satırlarda).
3. Değişiklikleri kaydedin.

Bu dosyada ayarlanan değerler tüm komut dosyaları tarafından kullanılır, bu nedenle doğru olmaları önemlidir. Uygulamayı yeniden dağıtıyorsanız, Kullanıcı ve Kaynak Grubu için farklı değerler seçmeniz gerekir. Daha sonra UserConfig.psm1 dosyasını yeni değerlerle yeniden güncelleyin.

## <a name="run-the-application"></a>Uygulamayı çalıştırma

Wingtip uygulamasında, kiracılar mekanlardır. Bir mekan konser salonu, spor kulübü veya etkinliklere ev sahipliği yapan başka bir yer olabilir. Mekanlar Wingtip'te müşteri olarak kaydedilir ve her mekan için bir kiracı tanımlayıcısı oluşturulur. Her mekan Wingtip'te yaklaşan etkinlikleri listeler, böylece halk etkinliklere bilet satın alabilir.

Her mekan, etkinliklerini listelemek ve bilet satmak için kişiselleştirilmiş bir web uygulamasına geçer. Her web uygulaması bağımsızdır ve diğer kiracılardan izole edilmiştir. Azure SQL Veritabanı'nda dahili olarak, her kiracının verileri varsayılan olarak çok kiracılı bir veritabanında depolanır. Tüm veriler kiracı tanımlayıcısı ile etiketlenir.

Merkezi **Bir Olaylar Hub** web sayfası, belirli dağıtımınızdaki kiracılara yapılan bağlantıların bir listesini sağlar. **Etkinlikler Hub** web sayfasını ve tek bir web uygulamasını deneyimlemek için aşağıdaki adımları kullanın:

1. Web tarayıcınızda **Etkinlikler Hub'ını** açın:
   - http://events.wingtip-mt.&lt;user&gt;.trafficmanager.net &nbsp; * &lt;(Kullanıcıyı&gt; dağıtımınızın kullanıcı değeriyle değiştirin.)*

     ![olay hub’ı](media/saas-multitenantdb-get-started-deploy/events-hub.png)

2. **Olay Hub’ında****Fabrikam Caz Kulübü**’ne tıklayın.

   ![Olaylar](./media/saas-multitenantdb-get-started-deploy/fabrikam.png)

### <a name="azure-traffic-manager"></a>Azure Traffic Manager

Gelen isteklerin dağıtımını denetlemek için Wingtip uygulaması [Azure Trafik Yöneticisi'ni](../traffic-manager/traffic-manager-overview.md)kullanır. Her kiracının etkinlik sayfası URL'sinde kiracı adını içerir. Her URL aynı zamanda belirli Kullanıcı değerinizi de içerir. Her URL aşağıdaki adımları kullanarak gösterilen biçime uyar:

- http://events.wingtip-mt.&lt;kullanıcı&gt;.trafficmanager.net/*fabrikamjazzclub*

1. Etkinlik uygulaması, kiracı nın adını URL'den ayrışdırır. Kiracı adı önceki örnek URL'de *fabrikamjazzclub* olduğunu.
2. Uygulama daha sonra [parça harita yönetimini](sql-database-elastic-scale-shard-map-management.md)kullanarak bir kataloga erişmek için bir anahtar oluşturmak için kiracı adını haşlar.
3. Uygulama anahtarı katalogda bulur ve kiracının veritabanının ilgili konumunu alır.
4. Uygulama, kiracının tüm verilerini içeren tek veritabanını bulmak ve bunlara erişmek için konum bilgilerini kullanır.

### <a name="events-hub"></a>Etkinlikler Merkezi

1. **Etkinlikler** Hub'ı, katalogda kayıtlı olan tüm kiracıları ve mekanlarını listeler.
2. **Olaylar** Hub'ı, URL'leri oluşturmak için her eşlemeyle ilişkili kiracının adını almak için katalogdaki genişletilmiş meta verileri kullanır.

Üretim ortamında, genellikle [bir şirketin internet etki alanını](../traffic-manager/traffic-manager-point-internet-domain.md) trafik yöneticisi profiline çekmek için bir CNAME DNS kaydı oluşturursunuz.

## <a name="start-generating-load-on-the-tenant-databases"></a>Kiracı veritabanları üzerinde yük oluşturmaya başlama

Şimdi uygulama dağıtıldı, bunu işe koyalım! *Demo-LoadGenerator* PowerShell komut dosyası, her kiracı için çalışan bir iş yükü başlatır. Birçok SaaS uygulamasındaki gerçek yük genellikle düzensiz ve öngörülemezdir. Bu tür bir yükü simüle etmek için, jeneratör tüm kiracılar arasında dağıtılan bir yük üretir. Yük, rasgele aralıklarla meydana gelen her kiracı üzerinde randomize patlamalar içerir. Yük deseni ortaya çıkması birkaç dakika sürer, bu nedenle en iyisi yükü izlemeden önce jeneratörün en az üç veya dört dakika çalışmasına izin vermek.

1. *PowerShell ISE*, açık ... \\Öğrenme Modülleri\\\\Utilities*Demo-LoadGenerator.ps1* script.
2. Betiği çalıştırmak ve yük oluşturucuyu başlatmak için **F5**’e basın (şimdilik varsayılan parametre değerlerini bırakın).

*Demo-LoadGenerator.ps1* komut dosyası yük jeneratörü çalışır başka bir PowerShell oturumu açılır. Yük jeneratörü, bu oturumda, her kiracı için bir tane olmak üzere arka plan yük oluşturma işlerini çağıran bir ön plan görevi olarak çalışır.

Ön plan görevi başladıktan sonra, iş çağrıştıran bir durumda kalır. Görev, daha sonra sağlanan yeni kiracılar için ek arka plan işleri başlatır.

PowerShell oturumunun kapatılması tüm işleri durdurur.

Farklı parametre değerlerini kullanmak için yük jeneratörü oturumunu yeniden başlatmak isteyebilirsiniz. Eğer öyleyse, PowerShell üretim oturumunu kapatın ve sonra *Demo-LoadGenerator.ps1*yeniden.

## <a name="provision-a-new-tenant-into-the-sharded-database"></a>Parçalanan veritabanına yeni bir kiracı sağlama

İlk *dağıtım, Tenants1* veritabanında üç örnek kiracı içerir. Başka bir kiracı oluşturalım ve dağıtılan uygulama üzerindeki etkilerini gözlemleyelim. Bu adımda, yeni bir kiracı oluşturmak için bir tuşa basın:

1. Açık... \\PowerShell\\ISE Öğrenme\\Modülleri Sağlanması ve Katalog*Demo-ProvisionTenants.ps1* *.*
2. Komut dosyasını çalıştırmak için **F5** **(F8**değil) tuşuna basın (varsayılan değerleri şimdilik bırakın).

   > [!NOTE]
   > PowerShell komut dosyalarını yalnızca **F5** tuşuna basarak çalıştırmalısınız, komut dosyasının seçilmiş bir bölümünü çalıştırmak için **F8** tuşuna basarak değil. **F8** ile sorun *$PSScriptRoot* değişken değerlendirilmemiştir olmasıdır. Bu değişken, klasörlerde gezinmek, diğer komut dosyalarını çağırmak veya modülleri almak için birçok komut dosyası tarafından gereklidir.

Yeni Red Maple Racing kiracı *Kiracı1* veritabanına eklenir ve katalogda kayıtlıdır. Yeni kiracının bilet satan **Etkinlikler** sitesi tarayıcınızda açılır:

![Yeni kiracı](./media/saas-multitenantdb-get-started-deploy/red-maple-racing.png)

Etkinlikler **Hub'ını**yenileyin ve yeni kiracı artık listede görünür.

## <a name="provision-a-new-tenant-in-its-own-database"></a>Kendi veritabanında yeni bir kiracı sağlama

Parçalanmış çok kiracılı model, yeni bir kiracıyı diğer kiracıların bulunduğu bir veritabanına mı yoksa kendi veritabanına mı sağlayıp sağlamamayı seçmenize olanak tanır. Kendi veritabanında yalıtılmış bir kiracı aşağıdaki avantajlardan yararlanır:

- Kiracının veritabanının performansı, diğer kiracıların gereksinimleriyle uzlaşmaya gerek kalmadan yönetilebilir.
- Gerekirse, veritabanı zaman içinde daha önceki bir noktaya geri yüklenebilir, çünkü başka kiracı etkilenemez.

Ücretsiz deneme müşterileri veya ekonomi müşterilerini çok kiracılı veritabanlarına koymayı seçebilirsiniz. Her prim kiracıkendi özel veritabanına koyabilirsiniz. Yalnızca bir kiracı içeren çok sayıda veritabanı oluşturursanız, kaynak maliyetlerini en iyi duruma getirmek için bunların tümünün bir kısmını elastik bir havuzda topluca yönetebilirsiniz.

Sonra, başka bir kiracı, kendi veritabanında bu kez hükmü:

1. Inç... \\Öğrenme\\Modülleri Sağlanması\\ve Katalog*Demo-ProvisionTenants.ps1*, **Salix Salsa** *$TenantName* değiştirmek , **dans** ve *$Scenario* için *$VenueType* **2**.

2. Komut dosyasını yeniden çalıştırmak için **F5** tuşuna basın.
    - Bu **F5** basın ayrı bir veritabanında yeni kiracı hükümleri. Veritabanı ve kiracı katalogda kayıtlıdır. Ardından tarayıcı kiracının Etkinlikler sayfasına açılır.

   ![Salix Salsa etkinlikleri sayfası](./media/saas-multitenantdb-get-started-deploy/salix-salsa.png)

   - Sayfanın altına kaydırın. Banner'da kiracı verilerinin depolandığı veritabanı adını görürsünüz.

3. Etkinlikler **Hub'ını** yenileyin ve iki yeni kiracı artık listede görünür.

## <a name="explore-the-servers-and-tenant-databases"></a>Sunucuları ve kiracı veritabanlarını keşfedin

Şimdi dağıtılan kaynaklardan bazılarına bakıyoruz:

1. Azure [portalında](https://portal.azure.com)kaynak grupları listesine göz atın. Uygulamayı dağıtırken oluşturduğunuz kaynak grubunu açın.

   ![kaynak grubu](./media/saas-multitenantdb-get-started-deploy/resource-group.png)

2. **Katalog-mt&lt;kullanıcı&gt; ** sunucusuna tıklayın. Katalog sunucusu *kiracı kataloğu* ve *basetenantdb*adlı iki veritabanı içerir. *Basetenantdb* veritabanı boş bir şablon veritabanıdır. Birçok kiracı veya sadece bir kiracı için kullanılan olsun, yeni bir kiracı veritabanı oluşturmak için kopyalanır.

   ![katalog sunucusu seçeneğine tıklayın](./media/saas-multitenantdb-get-started-deploy/catalog-server.png)

3. Kaynak grubuna geri dön ve kiracı veritabanlarını tutan *kiracı1 mt* sunucusunu seçin.
    - Kiracı1 veritabanı, orijinal üç kiracının ve eklediğiniz ilk kiracının depolandığı çok kiracılı bir veritabanıdır. 50 DTU Standart veritabanı olarak yapılandırılmıştır.
    - **Salixsalsa** veritabanı, Salix Salsa dans mekanını tek kiracısı olarak tutar. Varsayılan olarak 50 DSU ile Standart sürüm veritabanı olarak yapılandırılır.

   ![kiracı sunucusu](./media/saas-multitenantdb-get-started-deploy/tenants-server.png)

## <a name="monitor-the-performance-of-the-database"></a>Veritabanının performansını izleme

Yük jeneratörü birkaç dakikadır çalışıyorsa, Azure portalında yerleşik veritabanı izleme özelliklerine bakmak için yeterli telemetri kullanılabilir.

1. **Kiracı1&lt;mt kullanıcı&gt; ** sunucusuna göz atın ve içinde dört kiracı bulunan veritabanı için kaynak kullanımını görüntülemek için **kiracı1'ı** tıklatın. Her kiracı yük jeneratörü bir sporadik ağır yük tabidir:

   ![kiracıları izlemek1](./media/saas-multitenantdb-get-started-deploy/monitor-tenants1.png)

   DTU kullanım grafiği, çok kiracılı bir veritabanının birçok kiracı arasında öngörülemeyen bir iş yükünü nasıl desteklenebildiğini güzel bir şekilde göstermektedir. Bu durumda, yük jeneratörü her kiracıya yaklaşık 30 DTUs'luk düzensiz bir yük uygular. Bu yük, 50 DTU veritabanının %60 kullanımına eşittir. %60'ı aşan zirveler, yükün aynı anda birden fazla kiracıya uygulanmasının sonucudur.

2. **Kiracılar1-mt&lt;kullanıcı&gt; ** sunucusuna göz atın ve **salixsalsa** veritabanını tıklatın. Yalnızca bir kiracı içeren bu veritabanında kaynak kullanımını görebilirsiniz.

   ![salixsalsa veritabanı](./media/saas-multitenantdb-get-started-deploy/monitor-salix.png)

Yük jeneratörü, her kiracının hangi veritabanında olduğuna bakılmaksızın, her kiracıya benzer bir yük uyguluyor. **Salixsalsa** veritabanında yalnızca bir kiracı ile, veritabanı birkaç kiracı ile veritabanı çok daha yüksek bir yük sürdürebilir görebilirsiniz. 

### <a name="resource-allocations-vary-by-workload"></a>Kaynak ayırmaları iş yüküne göre değişir

Bazen çok kiracılı bir veritabanı, iyi performans için tek kiracılı veritabanından daha fazla kaynak gerektirir, ancak her zaman değil. Kaynakların en uygun tahsisi, sisteminizdeki kiracılar için belirli iş yükü özelliklerine bağlıdır.

Yük jeneratörü komut dosyası tarafından oluşturulan iş yükleri yalnızca illüstrasyon amaçlıdır.

## <a name="additional-resources"></a>Ek kaynaklar

- Çok kiracılı SaaS uygulamaları hakkında bilgi edinmek [için, çok kiracılı SaaS uygulamaları için tasarım desenleri](saas-tenancy-app-design-patterns.md)bölümüne bakın.

- Elastik havuzlar hakkında bilgi edinmek için bkz:

  - [Elastik havuzlar birden fazla Azure SQL veritabanını yönetmenize ve ölçeklendirmenize yardımcı olur](sql-database-elastic-pool.md)
  - [Azure SQL Veritabanı ile ölçek genişletme](sql-database-elastic-scale-introduction.md)

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide şunları öğrendiniz:

> [!div class="checklist"]
> - Wingtip Biletleri SaaS Çok Kiracı veritabanı uygulaması nasıl dağıtılır.
> - Uygulamayı oluşturan sunucular ve veritabanları hakkında.
> - Kiracılar *katalog*ile verilerine eşlenir.
> - Yeni kiracıların çok kiracılı veritabanına ve tek kiracılı veritabanına nasıl sağlanıp sağılabildiğini.
> - Kiracı etkinliğini izlemek için havuz kullanımını görüntüleme.
> - İlgili faturalandırmayı durdurmak için örnek kaynaklar nasıl silinir?

Şimdi [Hüküm ve katalog öğretici](sql-database-saas-tutorial-provision-and-catalog.md)deneyin.


<!--  Link references.

A [series of related tutorials] is available that build upon this initial deployment.
[link-wtp-overivew-jumpto-saas-tutorials-97j]: saas-multitenantdb-wingtip-app-overview.md#sql-database-wingtip-saas-tutorials

-->

[link-aka-ms-deploywtp-mtapp-52k]: https://aka.ms/deploywtp-mtapp


[link-azure-get-started-powershell-41q]: https://docs.microsoft.com/powershell/azure/get-started-azureps

[link-github-wingtip-multitenantdb-55g]: https://github.com/Microsoft/WingtipTicketsSaaS-MultiTenantDB/



<!--  Image references.

[image-deploy-to-azure-blue-48d]: https://aka.ms/deploywtp-mtapp "Button for Deploy to Azure."
-->

[image-deploy-to-azure-blue-48d]: media/saas-multitenantdb-get-started-deploy/deploy.png "Azure'a dağıtmak için düğme."

