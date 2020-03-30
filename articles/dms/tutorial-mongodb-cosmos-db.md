---
title: "Öğretici: MongoDB'yi MongoDB için Azure Cosmos DB API'ye çevrimdışı olarak geçirin"
titleSuffix: Azure Database Migration Service
description: Azure Veritabanı Geçiş Hizmeti'ni kullanarak MongoDB'den mongodb için çevrimdışı Olarak Azure Cosmos DB API'sine şirket içi geçiş yapmayı öğrenin.
services: dms
author: pochiraju
ms.author: rajpo
manager: craigg
ms.reviewer: craigg
ms.service: dms
ms.workload: data-services
ms.custom: seo-lt-2019
ms.topic: article
ms.date: 01/08/2020
ms.openlocfilehash: 08fa94dbe71299a6653df0b40aa5083375526172
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78255584"
---
# <a name="tutorial-migrate-mongodb-to-azure-cosmos-dbs-api-for-mongodb-offline-using-dms"></a>Öğretici: MongoDB'yi DMS kullanarak MongoDB'nin MongoDB'u çevrimdışı olarak Azure Cosmos DB'nin API'sine geçirin

MongoDB'nin şirket içi veya bulut örneğinden Azure Cosmos DB'nin MongoDB için API'sine çevrimdışı (tek seferlik) veritabanları geçişini gerçekleştirmek için Azure Veritabanı Geçiş Hizmeti'ni kullanabilirsiniz.

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:
> [!div class="checklist"]
>
> * Azure Veritabanı Geçiş Hizmeti örneği oluşturun.
> * Azure Veritabanı Geçiş Hizmeti'ni kullanarak bir geçiş projesi oluşturun.
> * Geçişi çalıştırma.
> * Geçişi izleme.

Bu öğreticide, Azure Sanal Makinesi'nde barındırılan MongoDB'deki bir veri kümesini Azure Veritabanı Geçiş Hizmeti'ni kullanarak Azure Cosmos DB'nin MongoDB apisine geçirebilirsiniz. Zaten ayarlanmış bir MongoDB kaynağınız yoksa, [Azure'da bir Windows VM'de MongoDB yükle makalesine bakın ve mongodb'u yapılandırın.](https://docs.microsoft.com/azure/virtual-machines/windows/install-mongodb)

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiyi tamamlamak için aşağıdakileri yapmanız gerekir:

* İş ortasını tahmin etme, bölüm anahtarı seçme ve dizin oluşturma ilkesi gibi geçiş öncesi adımları [tamamlayın.](../cosmos-db/mongodb-pre-migration.md)
* [MongoDB hesabı için Azure Cosmos DB API'si oluşturun.](https://ms.portal.azure.com/#create/Microsoft.DocumentDB)
* [ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-introduction) veya [VPN](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways)kullanarak şirket içi kaynak sunucularınıza site bağlantısı sağlayan Azure Kaynak Yöneticisi dağıtım modelini kullanarak Azure Veritabanı Geçiş Hizmeti için bir Microsoft Azure Sanal Ağı oluşturun. Sanal ağ oluşturma hakkında daha fazla bilgi için [Sanal Ağ Belgeleri'ne](https://docs.microsoft.com/azure/virtual-network/)ve özellikle adım adım ayrıntılarıiçeren hızlı başlangıç makalelerini görün.

    > [!NOTE]
    > Sanal ağ kurulumu sırasında, Microsoft'a ağ la bakan ExpressRoute kullanıyorsanız, hizmetin sağlandığı alt ağa aşağıdaki hizmet [bitiş noktalarını](https://docs.microsoft.com/azure/virtual-network/virtual-network-service-endpoints-overview) ekleyin:
    >
    > * Hedef veritabanı bitiş noktası (örneğin, SQL bitiş noktası, Cosmos DB bitiş noktası vb.)
    > * Depolama bitiş noktası
    > * Servis veri günü bitiş noktası
    >
    > Azure Veritabanı Geçiş Hizmeti internet bağlantısından yoksun olduğundan bu yapılandırma gereklidir.

* Sanal ağ Ağ Güvenlik Grubu (NSG) kurallarınızın aşağıdaki iletişim bağlantı noktalarını engellemediğinden emin olun: 53, 443, 445, 9354 ve 10000-20000. Sanal ağ NSG trafik filtreleme hakkında daha fazla ayrıntı için, [ağ güvenlik grupları ile](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg)makale Filtre ağ trafiği bakın.
* Azure Veritabanı Geçiş Hizmeti'nin varsayılan olarak TCP bağlantı noktası 27017 olan kaynak MongoDB sunucusuna erişmesine izin vermek için Windows güvenlik duvarınızı açın.
* Kaynak veritabanınızın (ler) önünde bir güvenlik duvarı cihazı kullanırken, Azure Veritabanı Geçiş Hizmeti'nin geçiş için kaynak veritabanına(ler) erişmesine izin vermek için güvenlik duvarı kuralları eklemeniz gerekebilir.

## <a name="register-the-microsoftdatamigration-resource-provider"></a>Microsoft.DataMigration kaynak sağlayıcısını kaydetme

1. Azure portal'da oturum açın, **Tüm hizmetler**'i ve ardından **Abonelikler**'i seçin.

   ![Portal aboneliklerini gösterme](media/tutorial-mongodb-to-cosmosdb/portal-select-subscription1.png)

2. Azure Veritabanı Geçiş Hizmeti örneğini oluşturmak istediğiniz aboneliği seçin ve sonra **Kaynak sağlayıcıları**’nı seçin.

    ![Kaynak sağlayıcılarını gösterme](media/tutorial-mongodb-to-cosmosdb/portal-select-resource-provider.png)

3. "migration" araması yapın ve **Microsoft.DataMigration** öğesinin sağ tarafındaki **Kaydet**'i seçin.

    ![Kaynak sağlayıcısını kaydetme](media/tutorial-mongodb-to-cosmosdb/portal-register-resource-provider.png)    

## <a name="create-an-instance"></a>Örnek oluşturma

1. Azure portalında + **Kaynak Oluştur'** seçeneğini belirleyin, Azure Veritabanı Geçiş Hizmeti'ni arayın ve ardından açılan listeden **Azure Veritabanı Geçiş Hizmeti'ni** seçin.

    ![Azure Market](media/tutorial-mongodb-to-cosmosdb/portal-marketplace.png)

2. **Azure Veritabanı Geçiş Hizmeti** ekranında **Oluştur**'u seçin.

    ![Azure Veritabanı Geçiş Hizmeti örneğini oluşturma](media/tutorial-mongodb-to-cosmosdb/dms-create1.png)
  
3. **Geçiş Hizmeti oluşturun** ekranında hizmet için bir ad belirtin, aboneliği ve yeni ya da var olan bir kaynak grubunu seçin.

4. Azure Veritabanı Geçiş Hizmeti örneğini oluşturmak istediğiniz konumu seçin. 

5. Varolan bir sanal ağ seçin veya yeni bir ağ oluşturun.

    Sanal ağ, Kaynak MongoDB örneğine ve hedef Azure Cosmos DB hesabına erişim sağlayan Azure Veritabanı Geçiş Hizmeti sağlar.

    Azure portalında sanal ağ oluşturma hakkında daha fazla bilgi için Azure [portalını kullanarak sanal ağ oluşturma makalesine](https://aka.ms/DMSVnet)bakın.

6. Fiyatlandırma katmanını seçin.

    Maliyetler ve fiyatlandırma katmanları hakkında daha fazla bilgi için [fiyatlandırma sayfasına](https://aka.ms/dms-pricing) bakın.

    ![Azure Veritabanı Geçiş Hizmeti örneği ayarlarını yapılandırma](media/tutorial-mongodb-to-cosmosdb/dms-settings2.png)

7. Hizmeti oluşturmak için **Oluştur**’u seçin.

## <a name="create-a-migration-project"></a>Geçiş projesi oluşturma

Hizmet oluşturulduktan sonra Azure portaldan bulun, açın ve yeni bir geçiş projesi oluşturun.

1. Azure portalda **Tüm hizmetler**'i seçin, Azure Veritabanı Geçiş Hizmeti araması yapın ve **Azure Veritabanı Geçiş Hizmeti**'ni seçin.

      ![Azure Veritabanı Geçiş Hizmeti'nin tüm örneklerini bulma](media/tutorial-mongodb-to-cosmosdb/dms-search.png)

2. Azure **Veritabanı Geçiş Hizmetleri** ekranında, oluşturduğunuz Azure Veritabanı Geçiş Hizmeti örneğinin adını arayın ve ardından örneği seçin.

3. +**Yeni Geçiş Projesi**'ni seçin.

4. Yeni **geçiş proje** ekranında, **Kaynak sunucu türü** metin kutusunda proje için bir ad belirtin, Hedef **sunucu türü** metin kutusunda **MongoDB'yi**seçin, **CosmosDB 'yi (MongoDB API)** seçin ve ardından **etkinlik türünü seçin**, Çevrimdışı veri **geçişini**seçin. 

    ![Veritabanı Geçiş Hizmeti projesi oluşturma](media/tutorial-mongodb-to-cosmosdb/dms-create-project.png)

5. Projeyi oluşturmak ve geçiş etkinliğini çalıştırmak için **Etkinlik oluştur ve çalıştır**'ı seçin.

## <a name="specify-source-details"></a>Kaynak ayrıntılarını belirtme

1. Kaynak **ayrıntıları** ekranında, kaynak MongoDB sunucusunun bağlantı ayrıntılarını belirtin.

   > [!IMPORTANT]
   > Azure Veritabanı Geçiş Hizmeti kaynak olarak Azure Cosmos DB'yi desteklemez.

    Bir kaynağa bağlanmak için üç mod vardır:
   * Tam nitelikli bir etki alanı adı veya IP adresi, Bağlantı noktası numarası ve bağlantı kimlik bilgilerini kabul eden **standart mod.**
   * **Bağlantı dize modu,** makalede açıklandığı gibi bir MongoDB Bağlantı dizesi kabul eder [Bağlantı String URI Biçimi](https://docs.mongodb.com/manual/reference/connection-string/).
   * Blob kapsayıcı SAS URL'sini kabul eden **Azure depolamasından gelen veriler.** Blob'un MongoDB [bsondump aracı](https://docs.mongodb.com/manual/reference/program/bsondump/)tarafından üretilen BSON dökümleri varsa **Blob'un BSON dökümleri içerdiğini** seçin ve kapsayıcı JSON dosyaları içeriyorsa seçin.

     Bu seçeneği seçerseniz, depolama hesabı bağlantı dizesinin biçimde göründüğünden emin olun:

     ```
     https://blobnameurl/container?SASKEY
     ```

     Bu blob kapsayıcı SAS bağlantı dizesi Azure Depolama gezgininde bulunabilir. İlgili kapsayıcı için SAS oluşturmak, size istenen yukarıda istenen biçimde URL sağlayacaktır.
     
     Ayrıca, Azure Depolama'daki tür dökümü bilgilerine bağlı olarak, aşağıdaki ayrıntıları göz önünde bulundurun.

     * BSON dökümleri için, blob kapsayıcısı içindeki veriler bsondump biçiminde olmalıdır, bu nedenle veri dosyaları biçim koleksiyonu.bson'daki veritabanlarının adını taşıyan klasörlere yerleştirilir. Meta veri dosyaları (varsa) biçim *koleksiyonu*.metadata.json kullanılarak adlandırılmalıdır.

     * JSON dökümleri için, blob kapsayıcısındaki dosyaların içeren veritabanlarından sonra adlandırılmış klasörlere yerleştirilmesi gerekir. Her veritabanı klasöründe, veri dosyaları "veri" adlı bir alt klasöre yerleştirilmeli ve biçim *koleksiyonu*.json kullanılarak adlandırılmalıdır. Meta veri dosyaları (varsa) "meta data" adlı bir alt klasöre yerleştirilmeli ve aynı biçim, *collection*.json kullanılarak adlandırılmalıdır. Meta veri dosyaları MongoDB bsondump aracı tarafından üretilen aynı biçimde olmalıdır.

    > [!IMPORTANT]
    > Mongo sunucusunda kendi imzalı bir sertifika kullanmak önerilmez. Ancak, biri kullanılırsa, lütfen **bağlantı dize modunu** kullanarak sunucuya bağlanın ve bağlantı dizenizin ""
    >
    >```
    >&sslVerifyCertificate=false
    >```

   DNS ad çözümlemenin mümkün olmadığı durumlarda IP Adresini de kullanabilirsiniz.

   ![Kaynak ayrıntılarını belirtme](media/tutorial-mongodb-to-cosmosdb/dms-specify-source.png)

2. **Kaydet'i**seçin.

## <a name="specify-target-details"></a>Hedef ayrıntılarını belirtme

1. Geçiş **hedef ayrıntıları** ekranında, MongoDB verilerinizi aktardığınız MongoDB hesabı için önceden sağlanmış Azure Cosmos DB API'si olan hedef Azure Cosmos DB hesabının bağlantı ayrıntılarını belirtin.

    ![Hedef ayrıntılarını belirtme](media/tutorial-mongodb-to-cosmosdb/dms-specify-target.png)

2. **Kaydet'i**seçin.

## <a name="map-to-target-databases"></a>Hedef veritabanlarıyla eşleyin

1. **Haritada hedef veritabanları** ekranı, geçiş için kaynak ve hedef veritabanı eş.

    Hedef veritabanı kaynak veritabanıyla aynı veritabanı adını içeriyorsa, Azure Veritabanı Geçiş Hizmeti varsayılan olarak hedef veritabanını seçer.

    **Create** dizesi veritabanı adının yanında görünüyorsa, Azure Veritabanı Geçiş Hizmeti'nin hedef veritabanını bulmadığını ve hizmetin veritabanını sizin için oluşturacağını gösterir.

    Geçişin bu [noktasında, iş elde etmek](https://docs.microsoft.com/azure/cosmos-db/set-throughput)için bir iş. Cosmos DB'de, her koleksiyon için veri tabanı düzeyinde veya ayrı ayrı iş ortası sağlayabilirsiniz. Elde etme birimi [İstek Birimleri](https://docs.microsoft.com/azure/cosmos-db/request-units) (RUs) cinsinden ölçülür. [Azure Cosmos DB fiyatlandırması](https://azure.microsoft.com/pricing/details/cosmos-db/)hakkında daha fazla bilgi edinin.

    ![Hedef veritabanlarıyla eşleyin](media/tutorial-mongodb-to-cosmosdb/dms-map-target-databases.png)

2. **Kaydet'i**seçin.
3. Koleksiyon **ayarı** ekranında, koleksiyonlar listesini genişletin ve ardından geçirilecek koleksiyonlistesini gözden geçirin.

    Azure Veritabanı Geçiş Hizmeti, hedef Azure Cosmos DB hesabında bulunmayan kaynak MongoDB örneğinde bulunan tüm koleksiyonları otomatik olarak seçer. Zaten veri içeren koleksiyonları yeniden geçirmek istiyorsanız, bu bıçaktaki koleksiyonları açıkça seçmeniz gerekir.

    Koleksiyonların kullanmasını istediğiniz RUS miktarını belirtebilirsiniz. Azure Veritabanı Geçiş Hizmeti, koleksiyon boyutuna bağlı olarak akıllı varsayılanlar önerir.

    > [!NOTE]
    > Çalıştırmayı hızlandırmak için gerekirse Azure Veritabanı Geçiş Hizmeti'nin birden çok örneğini kullanarak veritabanı geçişini ve koleksiyonunu paralel olarak gerçekleştirin.

    En iyi ölçeklenebilirlik için Azure [Cosmos DB'de bölümleme](https://docs.microsoft.com/azure/cosmos-db/partitioning-overview) avantajından yararlanmak için basılı bir anahtar da belirtebilirsiniz. [Bir parça/bölme anahtarı seçmek için en iyi uygulamaları](https://docs.microsoft.com/azure/cosmos-db/partitioning-overview#choose-partitionkey)gözden geçirmek için emin olun.

    ![Koleksiyon tablolarını seçin](media/tutorial-mongodb-to-cosmosdb/dms-collection-setting.png)

4. **Kaydet'i**seçin.

5. **Geçiş özeti** ekranının **Etkinlik adı** metin kutusunda geçiş etkinliği için bir ad belirtin.

    ![Geçiş özeti](media/tutorial-mongodb-to-cosmosdb/dms-migration-summary.png)

## <a name="run-the-migration"></a>Geçişi çalıştırma

* **Geçişi çalıştır**'ı seçin.

    Geçiş etkinliği penceresi görüntülenir ve etkinliğin **Durumu** **başlatılamadı.**

    ![Etkinlik durumu](media/tutorial-mongodb-to-cosmosdb/dms-activity-status.png)

## <a name="monitor-the-migration"></a>Geçişi izleme

* Geçiş etkinliği ekranında **Yenile**'yi seçerek geçişin **Durum** bilgisi **Tamamlandı** olana kadar gösterilen verileri güncelleştirebilirsiniz.

   > [!NOTE]
   > Veritabanı ve koleksiyon düzeyinde geçiş ölçümlerinin ayrıntılarını almak için Etkinlik'i seçebilirsiniz.

    ![Etkinlik durumu tamamlandı](media/tutorial-mongodb-to-cosmosdb/dms-activity-completed.png)

## <a name="verify-data-in-cosmos-db"></a>Cosmos DB'deki verileri doğrulama

* Geçiş tamamlandıktan sonra, tüm koleksiyonların başarıyla geçirilmiş olduğunu doğrulamak için Azure Cosmos DB hesabınızı denetleyebilirsiniz.

    ![Etkinlik durumu tamamlandı](media/tutorial-mongodb-to-cosmosdb/dms-cosmosdb-data-explorer.png)

## <a name="post-migration-optimization"></a>Geçiş sonrası optimizasyon

MongoDB veritabanında depolanan verileri Azure Cosmos DB'nin MongoDB api'sine geçirttikten sonra Azure Cosmos DB'ye bağlanabilir ve verileri yönetebilirsiniz. Dizin oluşturma ilkesini en iyi duruma getirme, varsayılan tutarlılık düzeyini güncelleştirme veya Azure Cosmos DB hesabınız için genel dağıtımı yapılandırma gibi diğer geçiş sonrası eniyim adımlarını da gerçekleştirebilirsiniz. Daha fazla bilgi için [geçiş sonrası optimizasyon](../cosmos-db/mongodb-post-migration.md) makalesine bakın.

## <a name="additional-resources"></a>Ek kaynaklar

* [Cosmos DB hizmet bilgileri](https://azure.microsoft.com/services/cosmos-db/)

## <a name="next-steps"></a>Sonraki adımlar

* Microsoft [Veritabanı Geçiş Kılavuzu'ndaki](https://datamigration.microsoft.com/)ek senaryolar için geçiş kılavuzunu gözden geçirin.
