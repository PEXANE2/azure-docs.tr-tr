---
title: Tek kiracılı SaaS eğitimi
description: Azure SQL Veritabanı'nı kullanan tek kiracılı tek kiracılı bir SaaS uygulamasını dağıtın ve keşfedin.
services: sql-database
ms.service: sql-database
ms.subservice: scenario
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: MightyPen
ms.author: genemi
ms.reviewer: sstein
ms.date: 11/07/2018
ms.openlocfilehash: e3afc8aa58551b995070ffaca978c8e7c8454da3
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "73822128"
---
# <a name="deploy-and-explore-a-standalone-single-tenant-application-that-uses-azure-sql-database"></a>Azure SQL Veritabanı'nı kullanan tek kiracılı tek kiracılı bir uygulamayı dağıtma ve keşfetme

Bu eğitimde, bağımsız uygulama veya kiracı başına uygulama deseni kullanılarak geliştirilen Wingtip Tickets SaaS örnek uygulamasını dağıtıyor ve keşfetmiş oluyorsunuz.  Uygulama, Azure SQL Veritabanı'nın çok kiracılı SaaS senaryolarını etkinleştirmeyi kolaylaştıran özelliklerini sergilemek üzere tasarlanmıştır.

Bağımsız uygulama veya kiracı başına uygulama deseni, her kiracı için bir uygulama örneği dağıtıyor.  Her uygulama belirli bir kiracı için yapılandırılır ve ayrı bir Azure kaynak grubunda dağıtılır. Çok kiracılı bir çözüm sağlamak için uygulamanın birden çok örneği sağlanmaktadır. Bu desen, kiracı yalıtımının en önemli öncelik olduğu kiracıların küçük sayıları için en iyi şekilde uygundur. Azure, kaynakların kiracının aboneliğine dağıtılmasına ve kiracı adına bir hizmet sağlayıcısı tarafından yönetilmesine olanak tanıyan iş ortağı programlarına sahiptir. 

Bu eğitimde, Azure aboneliğinize üç kiracı için üç bağımsız uygulama dağıtmış olursunuz.  Tek tek uygulama bileşenlerini keşfetmek ve onlarla çalışmak için tam erişime sahipsiniz.

Uygulama kaynak kodu ve yönetim komut ları [WingtipTicketsSaaS-StandaloneApp](https://github.com/Microsoft/WingtipTicketsSaaS-StandaloneApp) GitHub repo mevcuttur. Uygulama Visual Studio 2015 kullanılarak oluşturuldu ve Visual Studio 2019'da güncellemeden başarıyla açılmıyor ve derlemiyor.


Bu öğreticide şunları öğrenirsiniz:

> [!div class="checklist"]
> * Wingtip Biletleri SaaS Tek Başına Uygulaması nasıl dağıtılır?
> * Uygulama kaynak kodu ve yönetim komut dosyaları nereden alınacağını.
> * Uygulamayı oluşturan sunucular ve veritabanları hakkında.

Ek öğreticiler çıkacaktır. Bu uygulama desenine dayalı bir dizi yönetim senaryosu keşfetmenize olanak sağlar.   

## <a name="deploy-the-wingtip-tickets-saas-standalone-application"></a>Wingtip Biletleri SaaS Tek Başına Uygulama Dağıt

Sağlanan üç kiracı için uygulamayı dağıtın:

1. [Azure portalındaki](https://portal.azure.com)dağıtım şablonunu açmak için her mavi **Azure'a Dağıt** düğmesini tıklatın. Her şablon iki parametre değeri gerektirir; yeni bir kaynak grubunun adı ve bu dağıtımı uygulamanın diğer dağıtımlarından ayıran bir kullanıcı adı. Sonraki adım, bu değerleri ayarlamak için ayrıntıları sağlar.<br><br>
    <a href="https://aka.ms/deploywingtipsa-contoso" target="_blank"><img style="vertical-align:middle" src="media/saas-standaloneapp-get-started-deploy/deploy.png"/></a>&nbsp; **Contoso Konser Salonu**
<br><br>
    <a href="https://aka.ms/deploywingtipsa-dogwood" target="_blank"><img style="vertical-align:middle" src="media/saas-standaloneapp-get-started-deploy/deploy.png"/></a>&nbsp; **Kızılcık Dojo**
<br><br>
    <a href="https://aka.ms/deploywingtipsa-fabrikam" target="_blank"><img style="vertical-align:middle" src="media/saas-standaloneapp-get-started-deploy/deploy.png"/></a>&nbsp; **Fabrikam Caz Kulübü**

2. Her dağıtım için gerekli parametre değerlerini girin.

    > [!IMPORTANT]
    > Bazı kimlik doğrulama ve sunucu güvenlik duvarları, gösteri amacıyla kasıtlı olarak güvenli değildir. Her uygulama dağıtımı için **yeni bir kaynak grubu oluşturun.**  Varolan bir kaynak grubu kullanmayın. Bu uygulamayı veya oluşturduğu kaynakları üretim için kullanmayın. İlgili faturalandırmayı durdurmak için uygulamaları bitirdiğinizde tüm kaynak gruplarını silin.

    Kaynak adlarınızda yalnızca küçük harfleri, sayıları ve tireleri kullanmak en iyisidir.
    * **Kaynak grubu**için yeni oluştur'u seçin ve ardından kaynak grubu için küçük bir Ad sağlayın. **wingtip-sa-\<\>-\<venueName\> kullanıcı** önerilen desendir.  VenueName \<\>için, mekan adını boşluk suz olarak değiştirin. Kullanıcı \<\>için, kullanıcı değerini aşağıdan değiştirin.  Bu desen ile kaynak grubu adları *wingtip-sa-contosoconcerthall-af1*, *wingtip-sa-dogwooddojo-af1*, *wingtip-sa-fabrikamjazzclub-af1*olabilir.
    * Açılan listeden bir **Konum** seçin.

    * **Kullanıcı** için - Baş harfleriniz ve bir rakam gibi kısa bir kullanıcı değeri öneririz: *örneğin, af1*.


3. **Uygulamayı dağıtın**.

    * Hüküm ve koşulları kabul etmek için tıklayınız.
    * **Satın al**’a tıklayın.

4. **Bildirimler'i** (arama kutusunun sağındaki çan simgesi) tıklatarak üç dağıtımın durumunu izleyin. Uygulamaların dağıtılması yaklaşık beş dakika sürer.


## <a name="run-the-applications"></a>Uygulamaları çalıştırma

Uygulama, etkinliklere ev sahipliği yapan mekanları sergiler.  Mekanları uygulamanın kiracı vardır. Her mekan, etkinliklerini listelemek ve bilet satmak için kişiselleştirilmiş bir web sitesine girer. Mekan türleri arasında konser salonları, caz kulüpleri ve spor kulüpleri yer almaktadır. Örnekte, mekanın türü, mekanın web sitesinde gösterilen arka plan fotoğrafını belirler.   Bağımsız uygulama modelinde, her mekanın kendi bağımsız SQL veritabanına sahip ayrı bir uygulama örneği vardır.

1. Ayrı tarayıcı sekmelerinde üç kiracının her biri için etkinlikler sayfasını açın:

   - http://events.contosoconcerthall.&lt;kullanıcı&gt;.trafficmanager.net
   - http://events.dogwooddojo.&lt;kullanıcı&gt;.trafficmanager.net
   - http://events.fabrikamjazzclub.&lt;kullanıcı&gt;.trafficmanager.net

     (Her URL'de,&gt; kullanıcıyı dağıtımınızın kullanıcı değeriyle değiştirin.) &lt;

   ![Olaylar](./media/saas-standaloneapp-get-started-deploy/fabrikam.png)

Gelen isteklerin dağıtımını denetlemek için uygulama [*Azure Trafik Yöneticisi'ni*](../traffic-manager/traffic-manager-overview.md)kullanır. Kiracıya özel her uygulama örneği, URL'deki alan adının bir parçası olarak kiracı adını içerir. Tüm kiracı URL'leri belirli **Kullanıcı** değerinizi içerir. URL'ler aşağıdaki biçimi izler:
- http://events.&lt;mekan&gt;adı . &lt;kullanıcı&gt;.trafficmanager.net

Her kiracının veritabanı **Konumu,** ilgili dağıtılan uygulamanın uygulama ayarlarına dahildir.

Üretim ortamında, genellikle [*bir şirketin internet etki alanını*](../traffic-manager/traffic-manager-point-internet-domain.md) trafik yöneticisi profilinin URL'sine işaret eden bir CNAME DNS kaydı oluşturursunuz.


## <a name="explore-the-servers-and-tenant-databases"></a>Sunucuları ve kiracı veritabanlarını keşfedin

Dağıtılan kaynaklardan bazılarına bakalım:

1. Azure [portalında](https://portal.azure.com)kaynak grupları listesine göz atın.
2. Üç kiracı kaynak gruplarını görmelisiniz.
3. Fabrikam Jazz Club dağıtımı için kaynakları içeren **wingtip-sa-fabrikam kullanıcı&lt;&gt; ** kaynak grubunu açın.  **Fabrikamjazzclub&lt;&gt; kullanıcı sunucusu fabrikamjazzclub** veritabanını içerir. **fabrikamjazzclub**

Her kiracı veritabanı 50 DTU *bağımsız* veritabanıdır.

## <a name="additional-resources"></a>Ek kaynaklar

<!--
* Additional [tutorials that build on the Wingtip SaaS application](saas-dbpertenant-wingtip-app-overview.md#sql-database-wingtip-saas-tutorials)
* To learn about elastic pools, see [*What is an Azure SQL elastic pool*](sql-database-elastic-pool.md)
* To learn about elastic jobs, see [*Managing scaled-out cloud databases*](elastic-jobs-overview.md)
-->

- Çok kiracılı SaaS uygulamaları hakkında bilgi edinmek [için, çok kiracılı SaaS uygulamaları için tasarım desenleri](saas-tenancy-app-design-patterns.md)bölümüne bakın.

 
## <a name="delete-resource-groups-to-stop-billing"></a>Faturalandırmayı durdurmak için kaynak gruplarını silme ##

Örneği kullanmayı bitirdiğinizde, ilişkili faturalandırmayı durdurmak için oluşturduğunuz tüm kaynak gruplarını silin.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide şunları öğrendiniz:

> [!div class="checklist"]
> * Wingtip Biletleri SaaS Tek Başına Uygulaması nasıl dağıtılır?
> * Uygulamayı oluşturan sunucular ve veritabanları hakkında.
> * İlgili faturalandırmayı durdurmak için örnek kaynaklar nasıl silinir?

Ardından, şema yönetimi ve kiracı analitiği gibi bir dizi kiracı senaryosuna olanak tanıyan bir kiracı kataloğunun kullanımını keşfedeceğiniz Hüküm ve [Katalog](saas-standaloneapp-provision-and-catalog.md) öğreticisini deneyin.
 

