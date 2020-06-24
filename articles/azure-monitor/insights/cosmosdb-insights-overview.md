---
title: Cosmos DB için Azure Izleyici ile Azure Cosmos DB izleme | Microsoft Docs
description: Bu makalede, CosmosDB hesaplarıyla performans ve kullanım sorunlarını hızlı bir şekilde anlamak için Cosmos DB sahipler sağlayan Cosmos DB özelliği için Azure Izleyicisi açıklanmaktadır.
ms.subservice: ''
ms.topic: conceptual
author: mrbullwinkle
ms.author: mbullwin
ms.date: 05/11/2020
ms.openlocfilehash: 3fd4bcd13b4d92d2c6e61d10ae9e62996b5dfcf5
ms.sourcegitcommit: 51977b63624dfd3b4f22fb9fe68761d26eed6824
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/17/2020
ms.locfileid: "84945503"
---
# <a name="explore-azure-monitor-for-azure-cosmos-db"></a>Azure Cosmos DB için Azure Izleyicisini keşfet

Azure Cosmos DB için Azure Izleyici, Birleşik etkileşimli bir deneyimde tüm Azure Cosmos DB kaynaklarınızın genel performans, başarısızlık, kapasite ve işletimsel sistem durumunun bir görünümünü sağlar. Bu makale, bu yeni izleme deneyiminin avantajlarını ve bu deneyimi kuruluşunuzun benzersiz ihtiyaçlarına uyacak şekilde nasıl değiştirebileceğiniz ve uyarlayabileceğinizi anlamanıza yardımcı olur.   

## <a name="introduction"></a>Giriş

Deneyime girmeden önce, bilgilerin nasıl sunulduklarını ve görselleştirir. 

Şunları sağlar:

* Tek bir konumdaki tüm aboneliklerinizde Azure Cosmos DB kaynaklarınızın **Ölçek perspektifinde** , yalnızca değerlendirmede ilgilendiğiniz abonelikler ve kaynaklarla seçmeli kapsam yapabilme özelliği ile.

* Sorunları tanılamaya veya kategori kullanımı, hataları, kapasiteyi ve işlemlere göre ayrıntılı analiz gerçekleştirmeye yardımcı olması için belirli bir Azure CosmosDB kaynağının **detaya gitme Analizi** . Bu seçeneklerden herhangi birini seçmek, ilgili Azure Cosmos DB ölçümlerinin derinlemesine bir görünümünü sağlar.  

* **Özelleştirilebilir** -bu deneyim, Azure izleyici çalışma kitabı şablonlarının üzerine kurulmuştur. bu deneyim, hangi ölçümlerin görüntülendiğini değiştirmenize, Sınırlarınızla hizalı olan eşikleri değiştirmenize veya ayarlamanıza ve sonra özel bir çalışma kitabına kaydetmenizi sağlar. Çalışma kitaplarındaki grafikler daha sonra Azure panolarına sabitlenebilir.  

Bu özellik herhangi bir şeyi etkinleştirmenizi veya yapılandırmanızı gerektirmez; bu Azure Cosmos DB ölçümleri varsayılan olarak toplanır.

>[!NOTE]
>Bu özelliğe erişmek için ücret alınmaz ve [Azure izleyici fiyatlandırma ayrıntıları](https://azure.microsoft.com/pricing/details/monitor/) sayfasında açıklandığı gibi, yalnızca yapılandırdığınız veya etkinleştirdiğiniz Azure izleyici temel özellikleri için ücretlendirilirsiniz.

## <a name="view-utilization-and-performance-metrics-for-azure-cosmos-db"></a>Azure Cosmos DB için kullanım ve performans ölçümlerini görüntüleme

Tüm aboneliklerinizde depolama hesaplarınızın kullanımını ve performansını görüntülemek için aşağıdaki adımları gerçekleştirin.

1. [Azure Portal](https://portal.azure.com) oturum açın.

2. **İzleme** araması yapın ve **izleyiciyi**seçin.

    !["Monitor" sözcüğünün bulunduğu ve bir hızölçer Style görüntüsüyle Hizmetleri "Monitor" adlı bir açılan pencerede arama kutusu](./media/cosmosdb-insights-overview/search-monitor.png)

3. **Cosmos DB**seçin.

    ![Cosmos DB genel bakış çalışma kitabının ekran görüntüsü](./media/cosmosdb-insights-overview/cosmos-db.png)

### <a name="overview"></a>Genel Bakış

**Genel bakışta**, tabloda etkileşimli Azure Cosmos DB ölçümleri görüntülenir. Aşağıdaki açılan listelerden seçtiğiniz seçeneklere göre sonuçlara filtre uygulayabilirsiniz:

* **Abonelikler** -yalnızca bir Azure Cosmos DB kaynağına sahip abonelikler listelenir.  

* **Cosmos DB** -tümü, bir alt küme veya tek bir Azure Cosmos DB kaynağı seçebilirsiniz.

* **Zaman aralığı** -varsayılan olarak, yapılan ilgili seçimlere göre son 4 saati bilgileri görüntüler.

Aşağı açılan listelerin altındaki sayaç kutucuğu, Azure Cosmos DB kaynakların toplam sayısı seçili aboneliklerdir. Çalışma kitabındaki, işlem ölçümlerini rapor eden sütunlar için koşullu renk kodlaması veya heavmaps vardır. En büyük renk en yüksek değere sahiptir ve en düşük değere göre daha açık bir renge sahiptir. 

Azure Cosmos DB kaynaklarından birinin yanındaki açılan oku seçmek, tek veritabanı kapsayıcısı düzeyindeki performans ölçümlerinin bir dökümünü açığa çıkarır:

![Genişletilmiş açılan liste, tek tek veritabanı kapsayıcıları ve ilişkili performans dökümünü](./media/cosmosdb-insights-overview/container-view.png)

Mavi renkle vurgulanmış Azure Cosmos DB kaynak adının seçilmesi, sizi ilişkili Azure Cosmos DB hesabı için varsayılan **genel bakışa** götürür. 

### <a name="failures"></a>Hatalar

Sayfanın üst kısmındaki **hataların** yanı sıra çalışma kitabı şablonunun **arızalarının** bir kısmı açılır. Bu, istekleri oluşturan yanıtların dağıtımına göre toplam istek olduğunu gösterir:

![HTTP istek türüne göre dökümdeki hataların ekran görüntüsü](./media/cosmosdb-insights-overview/failures.png)

| Kod      |  Description       | 
|-----------|:--------------------|
| `200 OK`  | Aşağıdaki REST işlemlerinden biri başarılı oldu: </br>-Bir kaynağa ULAŞıN. </br> -Bir kaynağa koyun. </br> -Bir kaynakta GÖNDERIN. </br> -Saklı yordamı yürütmek için bir saklı yordam kaynağını GÖNDERIN.|
| `201 Created` | Kaynak oluşturmak için bir gönderme işlemi başarılı. |
| `404 Not Found` | İşlem, artık mevcut olmayan bir kaynak üzerinde işlem yapmaya çalışıyor. Örneğin, kaynak zaten silinmiş olabilir. |

Durum kodlarının tam listesi için [Azure Cosmos DB http durum kodu makalesine](https://docs.microsoft.com/rest/api/cosmos-db/http-status-codes-for-cosmosdb)başvurun.

### <a name="capacity"></a>Kapasite

Sayfanın üst kısmındaki **kapasiteyi** seçin ve çalışma kitabı şablonunun **Kapasite** kısmı açılır. Sahip olduğunuz belge sayısını, belgenizin zaman içinde büyümesini, veri kullanımını ve bıraktığınız toplam kullanılabilir depolama miktarını gösterir.  Bu, olası depolama ve veri kullanımı sorunlarını belirlemenize yardımcı olmak için kullanılabilir.

![Kapasite çalışma kitabı](./media/cosmosdb-insights-overview/capacity.png) 

Genel Bakış çalışma kitabında olduğu gibi, **abonelik** sütunundaki bir Azure Cosmos DB kaynağının yanındaki açılan aşağı seçme, veritabanını oluşturan bağımsız kapsayıcıların bir dökümünü açığa çıkarır.

### <a name="operations"></a>Operations 

Sayfanın üst kısmındaki **işlemler** ' i seçin ve çalışma kitabı şablonunun **işlemler** bölümü açılır. Bu, isteklerinizi yapılan istek türlerine göre kırarak görmenizi sağlar. 

Bu nedenle aşağıdaki örnekte, `eastus-billingint` ağırlıklı okuma isteklerinin alınması, ancak az sayıda büyük miktarda ve oluşturma isteği ile ilgili bilgi edinebilirsiniz. `westeurope-billingint`, Bir istek perspektifinden, en az son dört saat içinde, çalışma kitabının zaman aralığı parametresi aracılığıyla şu anda kapsama alınmış olduğu, salt okunurdur.

![İşlemler çalışma kitabı](./media/cosmosdb-insights-overview/operation.png) 

## <a name="pin-export-and-expand"></a>Sabitle, dışarı aktar ve Genişlet

Bölümün sağ üst köşesindeki raptiye simgesini seçerek ölçüm bölümlerinin herhangi birini bir [Azure panosuna](https://docs.microsoft.com/azure/azure-portal/azure-portal-dashboards) sabitleyebilirsiniz.

![Ölçüm bölümü panoya sabitle örneği](./media/cosmosdb-insights-overview/pin.png)

Verilerinizi Excel biçiminde dışarı aktarmak için raptiye simgesinin solundaki aşağı ok simgesini seçin.

![Çalışma kitabı simgesini dışarı aktar](./media/cosmosdb-insights-overview/export.png)

Çalışma kitabındaki tüm açılan görünümleri genişletmek veya daraltmak için dışa aktarma simgesinin solundaki genişlet simgesini seçin:

![Çalışma kitabı simgesini Genişlet](./media/cosmosdb-insights-overview/expand.png)

## <a name="customize-azure-monitor-for-azure-cosmos-db"></a>Azure Cosmos DB için Azure Izleyicisini özelleştirme

Bu deneyim, Azure izleyici çalışma kitabı şablonlarının üzerine inşa edildiğinden, **Customize**  >  değiştirilmiş sürümünüzün bir kopyasını**düzenleme** ve bir kopyasını özel çalışma kitabına **kaydetme** olanağınız vardır. 

![Çubuğu Özelleştir](./media/cosmosdb-insights-overview/customize.png)

Çalışma kitapları, sizin için özel **Raporlarım** bölümünde veya kaynak grubuna erişimi olan herkesin erişebileceği **paylaşılan raporlar** bölümünde bir kaynak grubuna kaydedilir. Özel çalışma kitabını kaydettikten sonra başlatmak için çalışma kitabı galerisine gitmeniz gerekir.

![Çalışma kitabı galerisini komut çubuğundan Başlat](./media/cosmosdb-insights-overview/gallery.png)

## <a name="troubleshooting"></a>Sorun giderme

Sorun giderme kılavuzu için, adanmış çalışma kitabı tabanlı Öngörüler [sorun giderme makalesine](troubleshoot-workbooks.md)bakın.

## <a name="next-steps"></a>Sonraki adımlar

* Sorunları algılamaya yardımcı olmak üzere otomatik uyarı ayarlamak için [ölçüm uyarılarını](../platform/alerts-metric.md) ve [hizmet durumu bildirimlerini](../../service-health/alerts-activity-log-service-notifications.md) yapılandırın.

* Çalışma kitaplarının desteklemek için tasarlandıkları senaryoları, mevcut raporların yeni nasıl yazılacağını ve özelleştirildiğini ve [Azure izleyici çalışma kitaplarını kullanarak etkileşimli raporlar oluşturma](../platform/workbooks-overview.md)konusunu gözden geçirin.
