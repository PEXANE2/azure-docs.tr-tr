---
title: Ortam oluşturma - Azure Zaman Serisi Öngörüleri | Microsoft Dokümanlar
description: Yeni bir Zaman Serisi Öngörüleri ortamı oluşturmak için Azure portalını nasıl kullanacağınızı öğrenin.
ms.service: time-series-insights
services: time-series-insights
author: deepakpalled
ms.author: dpalled
manager: cshankar
ms.reviewer: v-mamcge, jasonh, kfile
ms.workload: big-data
ms.topic: conceptual
ms.date: 01/31/2020
ms.custom: seodec18
ms.openlocfilehash: 2c946c49884ef0de6843028976d4ec00ccfbcdfe
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76934843"
---
# <a name="create-a-new-time-series-insights-environment-in-the-azure-portal"></a>Azure Portal’da yeni Zaman Serisi Görüşleri ortamı oluşturma

Bu makalede, Azure portalını kullanarak yeni bir Zaman Serisi Öngörüleri ortamının nasıl oluşturulacak olduğu açıklanmaktadır.

Zaman Serisi Öngörüleri, Azure IoT Hub'larına ve Etkinlik Hub'larına akan verileri dakikalar içinde görselleştirmeye ve sorgulamaya başlamanızı sağlayarak büyük hacimli zaman serisi verilerini saniyeler içinde sorgulamanızı sağlar.  Nesnelerin interneti (IoT) ölçeği için tasarlanmıştır ve terabaytlardır veri işleyebilir.

## <a name="steps-to-create-the-environment"></a>Ortam oluşturma adımları

Ortam oluşturmak için aşağıdaki adımları izleyin:

1. [Azure portalında](https://portal.azure.com)oturum açın.

1. + **Kaynak oluştur** düğmesini seçin.

1. Nesnelerin **İnterneti** kategorisini seçin ve **Zaman Serisi Öngörüleri'ni**seçin.

   [![Zaman Serisi Görüşleri oluşturma ortam](media/time-series-insights-get-started/tsi-create-new-environment.png)](media/time-series-insights-get-started/tsi-create-new-environment.png#lightbox)

1. Zaman **Serisi Öngörüleri** sayfasında **Oluştur'u**seçin.

1. Gerekli parametreleri doldurun. Aşağıdaki tabloda her parametre açıklanmaktadır:
   
   [![Zaman Serisi Görüşleri oluşturma kaynak grubu](media/time-series-insights-get-started/tsi-configure-and-create.png)](media/time-series-insights-get-started/tsi-configure-and-create.png#lightbox)
   
   Ayar|Önerilen değer|Açıklama
   ---|---|---
   Ortam adı | Benzersiz bir ad | Bu [ad, zaman serisi gezginindeki](https://insights.timeseries.azure.com) ortamı temsil eder
   Abonelik | Aboneliğiniz | Birden çok aboneliğiniz varsa, etkinlik kaynağınızı içeren aboneliği tercihen seçin. Time Series Öngörüleri, aynı abonelikte bulunan Azure IoT Hub ve Etkinlik Hub kaynaklarını otomatik olarak algılayabilir.
   Kaynak grubu | Yeni bir yeni oluşturma veya varolan kullanımı | Kaynak grubu, birlikte kullanılan Azure kaynakları koleksiyonudur. Varolan bir kaynak grubu (örneğin, Olay Hub'ınızı veya IoT Hub'ınızı içeren bir kaynak grubu) seçebilirsiniz. Veya bu kaynak diğer kaynaklarla ilgili değilse yeni bir kaynak yapabilirsiniz.
   Konum | Etkinlik kaynağınıza en yakın | Tercihen, bölge ve bölgeler arası bant genişliği maliyetlerinin eklenmesini ve verileri bölgeden çıkarırken gecikme gecikmesi eklenmesini önlemek amacıyla olay kaynağı verilerinizi içeren aynı veri merkezi konumunu seçin.
   Fiyatlandırma katmanı | S1 | Gerekli aktarım hızını seçin. En düşük maliyet ve başlangıç kapasitesi için S1'i seçin.
   Kapasite | 1 | Kapasite, seçili SKU ile ilişkili giriş hızı, depolama kapasitesi ve maliyet için uygulanan çarpandır.  Oluşturduktan sonra ortam kapasitesini değiştirebilirsiniz. En düşük maliyetler için 1'lik bir kapasite seçin. 
  
1. Sağlama işlemini başlatmak için **Oluştur'u** seçin. Birkaç dakika sürebilir.

1. Dağıtım işlemini izlemek için **Bildirimler** simgesini (çan simgesi) seçin.

   [![Bildirimleri izleyin](media/time-series-insights-get-started/tsi-deploy-notifications.png)](media/time-series-insights-get-started/tsi-deploy-notifications.png#lightbox)

1. Kaynağa **Genel Bakış'ta**dağıtım yapılandırma ayarlarınızı doğrulayın.

   [![Zaman Serisi Görüşleri oluşturma panoya sabitleme](media/time-series-insights-get-started/tsi-verify-deployment.png)](media/time-series-insights-get-started/tsi-verify-deployment.png#lightbox)

1. **(İsteğe bağlı)** İleride Time Series Insights ortamınıza kolayca erişmek için sağ üst köşedeki **pin simgesini** seçin.

## <a name="next-steps"></a>Sonraki adımlar

* Ortamınızı korumak için [veri erişim ilkelerini tanımlayın.](time-series-insights-data-access.md)

* Azure Zaman Serisi Öngörüleri ortamınıza [bir Etkinlik Hub etkinlik kaynağı ekleyin.](time-series-insights-how-to-add-an-event-source-eventhub.md)

* Olayları olay kaynağına [gönderin.](time-series-insights-send-events.md)

* Zaman Serisi [Öngörüler explorer'da](https://insights.timeseries.azure.com)ortamınızı görüntüleyin.
