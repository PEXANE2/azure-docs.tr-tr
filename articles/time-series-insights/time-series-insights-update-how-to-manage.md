---
title: Önizleme ortamı sağlama ve yönetme - Azure Zaman Serisi | Microsoft Dokümanlar
description: Azure Zaman Serisi Öngörüleri Önizleme ortamını nasıl sağlayıp yöneteceklerini öğrenin.
author: deepakpalled
ms.author: dpalled
manager: cshankar
ms.workload: big-data
ms.service: time-series-insights
services: time-series-insights
ms.topic: conceptual
ms.date: 02/07/2020
ms.custom: seodec18
ms.openlocfilehash: 1ec0d9c7ecf16c60c32abdf08b358268f460edb0
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77087222"
---
# <a name="provision-and-manage-azure-time-series-insights-preview"></a>Azure Zaman Serisi Öngörüleri Önizlemesini sağlama ve yönetme

Bu makalede, [Azure portalını](https://portal.azure.com/)kullanarak Bir Azure Zaman Serisi Öngörüleri Önizleme ortamının nasıl oluşturulup yönetilen anlatılmaktadır.

## <a name="overview"></a>Genel Bakış

Azure Zaman Serisi Öngörüleri Önizleme *ortamları, istediğiniz kadar öde* (PAYG) ortamlarıdır.

Bir Azure Zaman Serisi Öngörüleri Önizleme ortamı oluşturduğunuzda, şu Azure kaynaklarını oluşturursunuz:

* Azure Zaman Serisi Öngörüleri Önizleme ortamı  
* Azure Depolama genel amaçlı v1 hesabı
* Daha hızlı ve sınırsız sorgular için isteğe bağlı sıcak mağaza

> [!TIP]
> * [Ortamınızı nasıl planlayınız](./time-series-insights-update-plan.md)öğrenin.
> * Olay hub [kaynağı](./time-series-insights-how-to-add-an-event-source-eventhub.md) nın nasıl eklence veya [IoT hub kaynağı](./time-series-insights-how-to-add-an-event-source-iothub.md)nın nasıl eklence öğrenilebildiğini okuyun.

Şunları öğrenirsiniz:

1. **(İsteğe bağlı)** Her Azure Zaman Serisi Öngörüleri Önizleme ortamını bir etkinlik kaynağıyla ilişkilendirin. Ayrıca, ortamın uygun olaylara erişmesini sağlamak için bir Timestamp Kimliği özelliği ve benzersiz bir tüketici grubu da sağlayacaksınız.

   > [!NOTE]
   > Bir ortamı sağlarken önceki adım isteğe bağlıdır. Bu adımı atlarsanız, verilere ortamda erişilebilmek için daha sonra ortama bir olay kaynağı eklemeniz gerekir.

1. Sağlama tamamlandıktan sonra, erişim ilkelerinizi ve diğer çevre özniteliklerinizi iş gereksinimlerinize uyacak şekilde değiştirebilirsiniz.

## <a name="create-the-environment"></a>Ortamı oluşturma

Azure Zaman Serisi Öngörüleri Önizleme ortamı oluşturmak için:

1. **Tier**olarak **PAYG'yi** seçin. Bir ortam adı sağlayın ve kullanılacak abonelik grubunu ve kaynak grubunu seçin. Ardından ortamı barındırmak için desteklenen bir konum seçin.

   [![Azure Zaman Serisi Öngörüleri örneği oluşturun.](media/v2-update-manage/create-and-manage-configuration.png)](media/v2-update-manage/create-and-manage-configuration.png#lightbox)

1. Bir Zaman Serisi Kimliği girin.

    > [!NOTE]
    > * Zaman Serisi Kimliği *büyük/küçük harf duyarlı* ve *değişmez.* (Ayarlandıktan sonra değiştirilemez.)
    > * Zaman Serisi teşekkünler en fazla *üç* tuş olabilir.
    > * [Time Series ID nasıl seçilir](time-series-insights-update-how-to-id.md) hakkında daha fazla bilgi edinin

1. Bir depolama hesabı adı seçerek ve bir çoğaltma seçeneği belirleyerek bir Azure Depolama hesabı oluşturun. Bunu yapmak otomatik olarak bir Azure Depolama genel amaçlı v1 hesabı oluşturur. Hesap, daha önce seçtiğiniz Azure Zaman Serisi Öngörüler Önizleme ortamıyla aynı bölgede oluşturulur.

    [![Soğuk hava deposu yapılandırması](media/v2-update-manage/create-and-manage-cold-store.png)](media/v2-update-manage/create-and-manage-cold-store.png#lightbox)

1. **(İsteğe bağlı)** Ortamınızdaki en son veriler üzerinde daha hızlı ve sınırsız sorgu istiyorsanız, ortamınız için sıcak depolamayı etkinleştirin. Ayrıca, Bir Zaman Serisi Öngörüleri Önizleme ortamı oluşturduktan sonra, sol gezinti bölmesinde **Depolama Yapılandırması** seçeneği aracılığıyla sıcak bir mağaza oluşturabilir veya silebilirsiniz.

    [![Sıcak depolama yapılandırması](media/v2-update-manage/create-and-manage-warm-storage.png)](media/v2-update-manage/create-and-manage-warm-storage.png#lightbox)

1. **(İsteğe bağlı)** Şimdi bir olay kaynağı ekleyebilirsiniz. Ayrıca, örnek sağlanana kadar da bekleyebilirsiniz.

   * Time Series Insights, etkinlik kaynağı seçenekleri olarak [Azure IoT Hub'Larını](./time-series-insights-how-to-add-an-event-source-iothub.md) ve [Azure Etkinlik Hub'larını](./time-series-insights-how-to-add-an-event-source-eventhub.md) destekler. Ortamı oluştururken yalnızca tek bir olay kaynağı ekleyebilirsiniz, ancak daha sonra başka bir olay kaynağı ekleyebilirsiniz. 
   
     Etkinlik kaynağını eklediğinizde varolan bir tüketici grubu seçebilir veya yeni bir tüketici grubu oluşturabilirsiniz. Tüm olayların Azure Zaman Serisi Öngörüleri Önizleme ortamınızda görünür olmasını sağlamak için benzersiz bir tüketici grubu oluşturmak en iyisidir.

   * Uygun Timestamp özelliğini seçin. Varsayılan olarak, Azure Zaman Serisi Öngörüleri her olay kaynağı için ileti ye bağlı zamanı kullanır.

     > [!TIP]
     > İleti yle çevrilen süre, toplu iş senaryolarında veya geçmiş veri yükleme senaryolarında kullanılacak en iyi yapılandırılmış ayar olmayabilir. Bu gibi durumlarda, Timestamp özelliğini kullanma veya kullanmama kararınızı doğruladığından emin olun.

     [![Olay Kaynağı yapılandırma sekmesi](media/v2-update-manage/create-and-manage-event-source.png)](media/v2-update-manage/create-and-manage-event-source.png#lightbox)

1. Ortamınızın sağlandığını ve istediğiniz şekilde yapılandırıldığını doğrulayın.

    [![İnceleme + Oluştur sekmesi](media/v2-update-manage/create-and-manage-review-and-confirm.png)](media/v2-update-manage/create-and-manage-review-and-confirm.png#lightbox)

## <a name="manage-the-environment"></a>Çevreyi yönetme

Azure Zaman Serisi Öngörüleri Önizleme ortamınızı Azure portalını kullanarak yönetebilirsiniz. PAYG Azure Zaman Serisi Öngörüleri Önizleme ortamı ile azure portalı üzerinden ortamınızı yönetirken aklınızda bulundurulması gereken genel s1 veya S2 ortamları arasında birkaç önemli fark vardır:

* Azure portalı Önizleme **Genel Bakış** bıçağında aşağıdaki değişiklikler vardır:

  * PAYG ortamları için geçerli olmadığı için kapasite kaldırılır.
  * **Zaman serisi kimlik** özelliği eklenir. Verilerinizin nasıl bölümleniyi belirler.
  * Başvuru veri kümeleri kaldırılır.
  * Görüntülenen URL sizi Azure [Zaman Serisi Öngörüleri Önizleme gezginine](./time-series-insights-update-explorer.md)yönlendirir.
  * Azure Depolama hesap adınız listelenir.

* AZURE portalının **Yapılandırma** bıçağı, PAYG ortamları yapılandırılamadığı için Azure Zaman Serisi Öngörüleri Önizleme'de kaldırılır. Ancak, yeni tanıtılan sıcak depoyu yapılandırmak için **Depolama Yapılandırması'nı** kullanabilirsiniz.

* Azure portalının **Referans veri** bıçağı, başvuru verileri PAYG ortamlarının bir parçası olmadığından Azure Zaman Serisi Öngörüler Önizleme'de kaldırılır.

[![Azure portalında Zaman Serisi Öngörüleri Önizleme ortamı](media/v2-update-manage/create-and-manage-overview-confirm.png)](media/v2-update-manage/create-and-manage-overview-confirm.png#lightbox)

## <a name="next-steps"></a>Sonraki adımlar

- [Ortamınızı](./time-series-insights-update-plan.md)Planla'yı okuyarak Zaman Serisi Öngörüleri hakkında daha fazla bilgi edinin.

- [Olay hub kaynağı](./time-series-insights-how-to-add-an-event-source-eventhub.md)nı nasıl ekleyeceğinizi öğrenin.

- [Bir IoT hub kaynağını](./time-series-insights-how-to-add-an-event-source-iothub.md)yapılandırın.
