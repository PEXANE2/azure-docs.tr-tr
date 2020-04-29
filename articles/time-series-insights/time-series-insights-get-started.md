---
title: Ortam oluşturma-Azure Time Series Insights | Microsoft Docs
description: Yeni bir Time Series Insights ortamı oluşturmak için Azure portal nasıl kullanacağınızı öğrenin.
ms.service: time-series-insights
services: time-series-insights
author: deepakpalled
ms.author: dpalled
manager: cshankar
ms.reviewer: v-mamcge, jasonh, kfile
ms.workload: big-data
ms.topic: conceptual
ms.date: 04/16/2020
ms.custom: seodec18
ms.openlocfilehash: 7459c6afc775aa0df43d6f9285191c4c7e1b8cb8
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81602375"
---
# <a name="create-a-new-time-series-insights-environment-in-the-azure-portal"></a>Azure Portal’da yeni Zaman Serisi Görüşleri ortamı oluşturma

Bu makalede, Azure portal kullanılarak yeni bir Time Series Insights ortamının nasıl oluşturulacağı açıklanır.

Time Series Insights, Azure IoT Hub 'Larına veri akışını görselleştirmenizi ve sorgulamayı ve dakikalar içinde Event Hubs, büyük miktarlarda zaman serisi verilerini Saniyeler içinde sorgulamanızı sağlar.  Bu, Nesnelerin İnterneti (IoT) ölçeği için tasarlanmıştı ve terabayt veri işleyebilir.

## <a name="steps-to-create-the-environment"></a>Ortam oluşturma adımları

Ortam oluşturmak için aşağıdaki adımları izleyin:

1. [Azure Portal](https://portal.azure.com) oturum açın.

1. **+ Kaynak oluştur** düğmesini seçin.

1. **Nesnelerin interneti** kategorisini seçin ve **Time Series Insights**' ı seçin.

   [![Zaman Serisi Görüşleri oluşturma ortam](media/time-series-insights-get-started/tsi-create-new-environment.png)](media/time-series-insights-get-started/tsi-create-new-environment.png#lightbox)

1. **Time Series Insights** sayfasında **Oluştur**' u seçin.

1. Gerekli parametreleri girin. Aşağıdaki tabloda her bir parametre açıklanmaktadır:

   [![Zaman Serisi Görüşleri oluşturma kaynak grubu](media/time-series-insights-get-started/tsi-configure-and-create.png)](media/time-series-insights-get-started/tsi-configure-and-create.png#lightbox)

   Ayar|Önerilen değer|Açıklama
   ---|---|---
   Ortam adı | Benzersiz bir ad | Bu ad, [zaman serisi Gezgininde](https://insights.timeseries.azure.com) ortamı temsil eder
   Abonelik | Aboneliğiniz | Birden çok aboneliğiniz varsa, tercihen olay kaynağınızı içeren aboneliği seçin. Time Series Insights, aynı abonelikte bulunan Azure IoT Hub ve Olay Hub kaynaklarını otomatik olarak algılayabilir.
   Kaynak grubu | Yeni oluştur veya var olanı kullan | Kaynak grubu, birlikte kullanılan Azure kaynakları koleksiyonudur. Örneğin, Olay Hub 'ınızı veya IoT Hub içeren mevcut bir kaynak grubunu seçebilirsiniz. Ya da bu kaynak diğer kaynaklarla ilgili değilse yeni bir tane oluşturabilirsiniz.
   Konum | Olay kaynağınızı en yakın | Tercihen, siteler arası ve çapraz bölge bant genişliği maliyetlerini ve verileri bölgenin dışına taşırken gecikme süresi eklenmesini önlemek için, olay kaynak verilerinizi içeren veri merkezi konumunu seçin.
   Fiyatlandırma katmanı | S1 | Gerekli aktarım hızını seçin. En düşük maliyetler ve başlangıç kapasitesi için S1 ' i seçin.
   Kapasite | 1 | Kapasite, seçili SKU ile ilişkili giriş fiyatı, depolama kapasitesi ve maliyet için geçerlidir.  Oluşturduktan sonra ortam kapasitesini değiştirebilirsiniz. En düşük maliyetler için 1 kapasitesini seçin.
  
1. Hazırlama işlemini başlatmak için **Oluştur** ' u seçin. Birkaç dakika sürebilir.

1. Dağıtım sürecini izlemek için **Bildirimler** simgesini (zil simgesi) seçin.

   [![Bildirimleri izleyin](media/time-series-insights-get-started/tsi-deploy-notifications.png)](media/time-series-insights-get-started/tsi-deploy-notifications.png#lightbox)

1. Kaynağa **genel bakış**bölümünde dağıtım yapılandırma ayarlarınızı doğrulayın.

   [![Zaman Serisi Görüşleri oluşturma panoya sabitleme](media/time-series-insights-get-started/tsi-verify-deployment.png)](media/time-series-insights-get-started/tsi-verify-deployment.png#lightbox)

1. **(Isteğe bağlı)** Gelecekte Time Series Insights ortamınıza kolayca erişmek için sağ üst köşedeki **sabitleme simgesini** seçin.

## <a name="next-steps"></a>Sonraki adımlar

* Ortamınızın güvenliğini sağlamak için [veri erişim Ilkeleri tanımlayın](time-series-insights-data-access.md) .

* Azure Time Series Insights ortamınıza [bir olay hub 'ı olay kaynağı ekleyin](time-series-insights-how-to-add-an-event-source-eventhub.md) .

* [Olayları](time-series-insights-send-events.md) olay kaynağına gönderin.

* [Time Series Insights Explorer](https://insights.timeseries.azure.com)'da ortamınızı görüntüleyin.
