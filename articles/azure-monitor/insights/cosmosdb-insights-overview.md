---
title: Cosmos DB için Azure Monitörü ile Azure Cosmos DB'yi izleyin (önizleme)| Microsoft Dokümanlar
description: Bu makalede, Cosmos DB sahiplerine CosmosDB hesaplarıyla ilgili performans ve kullanım sorunlarını hızlı bir şekilde anlamalarını sağlayan Cosmos DB için Azure Monitörü açıklanmaktadır.
ms.subservice: ''
ms.topic: conceptual
author: mrbullwinkle
ms.author: mbullwin
ms.date: 10/27/2019
ms.openlocfilehash: 9a900a2f2e950fe9b9846ebcc047d7c344284948
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78250670"
---
# <a name="explore-azure-monitor-for-azure-cosmos-db-preview"></a>Azure Cosmos DB için Azure Monitörünü keşfedin (önizleme)

Azure Cosmos DB için Azure Monitor (önizleme), tüm Azure Cosmos DB kaynaklarınızın genel performansını, hatalarını, kapasitesini ve operasyonel durumunu birleşik bir etkileşimli deneyimle görüntüleyebilir. Bu makale, bu yeni izleme deneyiminin faydalarını ve deneyimi kuruluşunuzun benzersiz gereksinimlerine uyacak şekilde nasıl değiştirebileceğinizi ve uyarlayabileceğinizi anlamanıza yardımcı olacaktır.   

## <a name="introduction"></a>Giriş

Deneyime dalmadan önce, bilgiyi nasıl sunduğunu ve görselleştirdiğini anlamalısınız. 

Bu sunar:

* Azure Cosmos DB kaynaklarınızın **ölçek perspektifinde,** yalnızca değerlendirmek istediğiniz abonelikleri ve kaynakları seçerek kapsama olanağı yla, tüm aboneliklerinizde tek bir konumda.

* Sorunları tanılamaya veya kategoriye göre ayrıntılı analizler gerçekleştirmeye yardımcı olmak için belirli bir Azure CosmosDB kaynağının **analizini ayrıntılı** olarak analiz edin - kullanım, hatalar, kapasite ve işlemler. Bu seçeneklerden herhangi birini seçmek, ilgili Azure Cosmos DB ölçümlerinin ayrıntılı bir görünümünü sağlar.  

* **Özelleştirilebilir** - Bu deneyim, hangi ölçümlerin görüntülendiğini değiştirmenize, sınırlarınızla uyumlu eşikleri değiştirmenize ve ardından özel bir çalışma kitabına kaydetmenize olanak tanıyan Azure Monitor çalışma kitabı şablonlarının üzerine inşa edilmiştir. Çalışma kitaplarındaki grafikler daha sonra Azure panolarına sabitlenebilir.  

Bu özellik herhangi bir şeyi etkinleştirmenizi veya yapılandırmanızı gerektirmez, bu Azure Cosmos DB ölçümleri varsayılan olarak toplanır.

>[!NOTE]
>Bu özelliğe erişmek için herhangi bir ücret alınmaz ve yalnızca Azure Monitor [fiyatlandırma ayrıntıları](https://azure.microsoft.com/pricing/details/monitor/) sayfasında açıklandığı gibi, yapılandırmanız veya etkinleştirdiğiniz Azure Monitor temel özellikleri için ücretlendirilirsiniz.

## <a name="view-utilization-and-performance-metrics-for-azure-cosmos-db"></a>Azure Cosmos DB için kullanım ve performans ölçümlerini görüntüleme

Tüm aboneliklerinizde depolama hesaplarınızın kullanımını ve performansını görüntülemek için aşağıdaki adımları gerçekleştirin.

1. [Azure portalında](https://portal.azure.com)oturum açın.

2. **Monitör'ü** arayın ve **Monitör'ü**seçin.

    !["Monitör" sözcüğünün ve hız göstergesi tarzı görüntüye sahip "Monitör" hizmetlerinin "Monitör" yazan açılır kutusunu arama](./media/cosmosdb-insights-overview/search-monitor.png)

3. **Cosmos DB 'yi seçin (önizleme)**.

    ![Cosmos DB genel çalışma kitabının ekran görüntüsü](./media/cosmosdb-insights-overview/cosmos-db.png)

### <a name="overview"></a>Genel Bakış

**Genel Bakış'ta**tablo, etkileşimli Azure Cosmos DB ölçümlerini görüntüler. Sonuçları, aşağıdaki açılır listelerden seçtiğiniz seçeneklere göre filtreleyebilirsiniz:

* **Abonelikler** - yalnızca Azure Cosmos DB kaynağı olan abonelikler listelenir.  

* **Cosmos DB** - Tümünü, bir alt kümeyi veya tek bir Azure Cosmos DB kaynağını seçebilirsiniz.

* **Zaman Aralığı** - varsayılan olarak, yapılan ilgili seçimlere göre son 4 saatlik bilgileri görüntüler.

Açılan listelerin altındaki sayaç döşemesi, seçilen aboneliklerde toplam Azure Cosmos DB kaynağı sayısının toplamıdır. Çalışma kitabında hareket ölçümlerini bildiren sütunlar için koşullu renk kodlama veya ısı haritaları vardır. En derin renk en yüksek değere sahiptir ve daha açık bir renk en düşük değerlere dayanır. 

Azure Cosmos DB kaynaklarından birinin yanında açılır ok seçmek, tek tek veritabanı kapsayıcı düzeyindeperformans ölçümlerinin bir dökümünü ortaya çıkarır:

![Tek tek veritabanı kapsayıcılarını ve ilişkili performans dökümünü ortaya çıkaran genişletilmiş açılır bırakma](./media/cosmosdb-insights-overview/container-view.png)

Mavi ile vurgulanan Azure Cosmos DB kaynak adını seçmek, sizi ilişkili Azure Cosmos DB hesabı için varsayılan **Genel Bakış'a** götürür. 

### <a name="failures"></a>Hatalar

Sayfanın üst kısmında **hatalar** seçin ve çalışma kitabı şablonunun **Hatalar** bölümü açılır. Bu istekleri oluşturan yanıtların dağıtımı ile toplam istekleri gösterir:

![HTTP istek türüne göre arıza ile hataların ekran görüntüsü](./media/cosmosdb-insights-overview/failures.png)

| Kod      |  Açıklama       | 
|-----------|:--------------------|
| `200 OK`  | Aşağıdaki REST operasyonlarından biri başarılı oldu: </br>- Bir kaynak alın. </br> - Bir kaynak üzerine KOYUN. </br> - Post bir kaynak üzerinde. </br> - Saklanan yordamı yürütmek için depolanan yordam kaynağı na post.|
| `201 Created` | Kaynak oluşturmak için bir POST işlemi başarılı olur. |
| `404 Not Found` | İşlem, artık var olmayan bir kaynak üzerinde hareket etmeye çalışıyor. Örneğin, kaynak zaten silinmiş olabilir. |

Durum kodlarının tam listesi için [Azure Cosmos DB HTTP durum kodu makalesine başvurun.](https://docs.microsoft.com/rest/api/cosmos-db/http-status-codes-for-cosmosdb)

### <a name="capacity"></a>Kapasite

Sayfanın üst kısmında **Kapasite'yi** seçin ve çalışma kitabı şablonunun **Kapasite** bölümü açılır. Kaç belgeye sahip olduğunuzu, zaman içinde belge büyümenizi, veri kullanımınızı ve kalan toplam kullanılabilir depolama alanı miktarını gösterir.  Bu, olası depolama ve veri kullanımı sorunlarını belirlemeye yardımcı olmak için kullanılabilir.

![Kapasite çalışma kitabı](./media/cosmosdb-insights-overview/capacity.png) 

Genel bakış çalışma kitabında olduğu gibi, **Abonelik** sütunundaki Bir Azure Cosmos DB kaynağının yanındaki açılır oranı seçmek, veritabanını oluşturan tek tek kapsayıcıların bir dökümünü ortaya çıkarır.

### <a name="operations"></a>İşlemler 

Sayfanın üst kısmında **Kiİşlemeler'i** seçin ve çalışma kitabı şablonunun **İşlemler** bölümü açılır. Bu, isteklerinizin yapılan istek türüne göre ayrıldığını görmenizi sağlar. 

Yani aşağıdaki örnekte ağırlıklı `eastus-billingint` olarak okuma istekleri alıyor, ancak yükseltme ve istekoluşturmak az sayıda bakın. Yalnızca `westeurope-billingint` istek perspektifinden okunurken, çalışma kitabının şu anda zaman aralığı parametresi üzerinden yalnızca son dört saat içinde yalnızca okunur.

![İşlemler çalışma kitabı](./media/cosmosdb-insights-overview/operation.png) 

## <a name="pin-export-and-expand"></a>Sabitleme, dışa aktarma ve genişletme

Bölümün sağ üst kısmındaki toka simgesini seçerek metrik bölümlerden herhangi birini [Azure Panosu'na](https://docs.microsoft.com/azure/azure-portal/azure-portal-dashboards) sabitleyebilirsiniz.

![Pano örneğine metrik kesit pini](./media/cosmosdb-insights-overview/pin.png)

Verilerinizi Excel biçimine aktarmak için, toka simgesinin solundaki aşağı ok simgesini seçin.

![Çalışma kitabı simgesini dışa aktarma](./media/cosmosdb-insights-overview/export.png)

Çalışma kitabındaki tüm açılır görünümleri genişletmek veya daraltmak için dışa aktarma simgesinin solundaki genişletme simgesini seçin:

![Çalışma kitabı simgesini genişletme](./media/cosmosdb-insights-overview/expand.png)

## <a name="customize-azure-monitor-for-azure-cosmos-db-preview"></a>Azure Cosmos DB için Azure Monitörünü Özelleştir (önizleme)

Bu deneyim Azure Monitor çalışma kitabı şablonlarının üzerine kurulduğundan, değiştirilmiş sürümünüzün bir kopyasını özelleştirilmiş bir çalışma kitabına **özelleştirve** > **Edit** **kaydedebilirsiniz.** 

![Çubuğu özelleştir](./media/cosmosdb-insights-overview/customize.png)

Çalışma kitapları, sizin için özel olan **Raporlarım** bölümünde veya kaynak grubuna erişimi olan herkesin erişebileceği **Paylaşılan Raporlar** bölümünde bir kaynak grubuna kaydedilir. Özel çalışma kitabını kurtardıktan sonra, başlatmak için çalışma kitabı galerisine gitmeniz gerekir.

![Komut çubuğundan çalışma kitabı galerisini başlatın](./media/cosmosdb-insights-overview/gallery.png)

## <a name="next-steps"></a>Sonraki adımlar

* Sorunları algılamaya yardımcı olmak için otomatik uyarı ayarlamak için [metrik uyarıları](../platform/alerts-metric.md) ve hizmet [durumu bildirimlerini](../../service-health/alerts-activity-log-service-notifications.md) yapılandırın.

* Çalışma kitaplarının desteklemek için tasarladığı senaryoları öğrenin, yeni raporları nasıl yazarken ve özelleştirin ve [Azure Monitor çalışma kitaplarıyla etkileşimli raporlar oluştur'u](../app/usage-workbooks.md)gözden geçirerek daha fazlasını öğrenin.
