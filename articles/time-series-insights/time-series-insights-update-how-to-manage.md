---
title: Önizleme ortamı sağlama ve yönetme-Azure zaman serisi | Microsoft Docs
description: Azure Time Series Insights önizleme ortamı sağlamayı ve yönetmeyi öğrenin.
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
ms.sourcegitcommit: cfbea479cc065c6343e10c8b5f09424e9809092e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/08/2020
ms.locfileid: "77087222"
---
# <a name="provision-and-manage-azure-time-series-insights-preview"></a>Azure Time Series Insights önizlemeyi sağlayın ve yönetin

Bu makalede, [Azure Portal](https://portal.azure.com/)kullanılarak Azure Time Series Insights önizleme ortamının nasıl oluşturulacağı ve yönetileceği açıklanmaktadır.

## <a name="overview"></a>Genel Bakış

Azure Time Series Insights Preview ortamları *Kullandıkça Öde* (PAYG) ortamlardır.

Azure Time Series Insights bir önizleme ortamı sağladığınızda şu Azure kaynaklarını oluşturursunuz:

* Azure Time Series Insights önizleme ortamı  
* Azure depolama genel amaçlı v1 hesabı
* Daha hızlı ve sınırsız sorgu için isteğe bağlı bir sıcak mağaza

> [!TIP]
> * [Ortamınızı nasıl planlayacağınızı](./time-series-insights-update-plan.md)öğrenin.
> * [Bir olay hub 'ı kaynağı ekleme](./time-series-insights-how-to-add-an-event-source-eventhub.md) veya [IoT Hub kaynağı ekleme](./time-series-insights-how-to-add-an-event-source-iothub.md)hakkında bilgi edinin.

Şunları öğrenirsiniz:

1. **(Isteğe bağlı)** Her Azure Time Series Insights önizleme ortamını bir olay kaynağıyla ilişkilendirin. Ayrıca, ortamın uygun olaylara erişimi olduğundan emin olmak için bir zaman damgası KIMLIĞI özelliği ve benzersiz bir tüketici grubu da sağlarsınız.

   > [!NOTE]
   > Bir ortam sağlanırken yukarıdaki adım isteğe bağlıdır. Bu adımı atlarsanız, ortama daha sonra veri erişilebilmesi için bir olay kaynağı eklemeniz gerekir.

1. Sağlama tamamlandıktan sonra, erişim ilkelerinizi ve diğer ortam özniteliklerini iş gereksinimlerinize uyacak şekilde değiştirebilirsiniz.

## <a name="create-the-environment"></a>Ortamı oluşturma

Azure Time Series Insights önizleme ortamı oluşturmak için:

1. **Katman**olarak **PAYG** seçin. Bir ortam adı sağlayın ve kullanılacak abonelik grubunu ve kaynak grubunu seçin. Ardından, ortamı barındırmak için desteklenen bir konum seçin.

   [![bir Azure Time Series Insights örneği oluşturun.](media/v2-update-manage/create-and-manage-configuration.png)](media/v2-update-manage/create-and-manage-configuration.png#lightbox)

1. Bir zaman serisi KIMLIĞI girin.

    > [!NOTE]
    > * Zaman serisi KIMLIĞI, *büyük/küçük harfe duyarlıdır* ve *sabittir*. (Ayarlandıktan sonra değiştirilemez.)
    > * Zaman serisi kimlikleri en fazla *üç* anahtar olabilir.
    > * [Zaman SERISI kimliğini seçme](time-series-insights-update-how-to-id.md) hakkında daha fazla bilgi edinin

1. Bir depolama hesabı adı seçerek ve bir çoğaltma seçeneği belirleyerek bir Azure depolama hesabı oluşturun. Bunun yapılması, otomatik olarak bir Azure depolama genel amaçlı v1 hesabı oluşturur. Hesap, daha önce seçtiğiniz Azure Time Series Insights önizleme ortamıyla aynı bölgede oluşturulur.

    [![soğuk depolama yapılandırması](media/v2-update-manage/create-and-manage-cold-store.png)](media/v2-update-manage/create-and-manage-cold-store.png#lightbox)

1. **(Isteğe bağlı)** Ortamınızdaki en son veriler üzerinde daha hızlı ve sınırsız sorgu istiyorsanız ortamınız için ısınma mağazasını etkinleştirin. Ayrıca, bir Time Series Insights önizleme ortamı oluşturduktan sonra sol gezinti bölmesindeki **depolama yapılandırması** seçeneği aracılığıyla bir sıcak mağaza oluşturabilir veya silebilirsiniz.

    [![ısınma depolama yapılandırması](media/v2-update-manage/create-and-manage-warm-storage.png)](media/v2-update-manage/create-and-manage-warm-storage.png#lightbox)

1. **(Isteğe bağlı)** Şimdi bir olay kaynağı ekleyebilirsiniz. Örnek sağlandıktan sonra da bekleyebilirsiniz.

   * Time Series Insights, [azure IoT Hub](./time-series-insights-how-to-add-an-event-source-iothub.md) ve [Azure Event Hubs](./time-series-insights-how-to-add-an-event-source-eventhub.md) 'yi olay kaynağı seçenekleri olarak destekler. Ortamı oluştururken yalnızca tek bir olay kaynağı ekleyebilseniz de daha sonra başka bir olay kaynağı ekleyebilirsiniz. 
   
     Olay kaynağını eklediğinizde, mevcut bir tüketici grubunu seçebilir veya yeni bir tüketici grubu oluşturabilirsiniz. Tüm olayların Azure Time Series Insights önizleme ortamınızda göründüğünden emin olmak için benzersiz bir tüketici grubu oluşturmak en iyisidir.

   * Uygun zaman damgası özelliğini seçin. Varsayılan olarak, Azure Time Series Insights her olay kaynağı için ileti temelli sıraya alma süresini kullanır.

     > [!TIP]
     > İleti temelli sıraya alma saati, toplu olay senaryolarında veya geçmiş verileri karşıya yükleme senaryolarında kullanılacak en iyi yapılandırılmış ayar olmayabilir. Bu gibi durumlarda, bir zaman damgası özelliği kullanmayın veya kullanma kararını doğruladığınızdan emin olun.

     [![olay kaynağı Yapılandırma sekmesi](media/v2-update-manage/create-and-manage-event-source.png)](media/v2-update-manage/create-and-manage-event-source.png#lightbox)

1. Ortamınızın sağlandığını ve istediğiniz şekilde yapılandırıldığını onaylayın.

    [![gözden geçir + Oluştur sekmesi](media/v2-update-manage/create-and-manage-review-and-confirm.png)](media/v2-update-manage/create-and-manage-review-and-confirm.png#lightbox)

## <a name="manage-the-environment"></a>Ortamı yönetme

Azure Time Series Insights önizleme ortamınızı Azure portal kullanarak yönetebilirsiniz. Bir PAYG Azure Time Series Insights önizleme ortamı ve genel olarak kullanılabilir S1 veya S2 ortamları arasında, ortamınızı Azure portal aracılığıyla yönetirken göz önünde bulundurmanız gereken birkaç temel fark vardır:

* Azure portal önizlemeye **genel bakış** dikey penceresi aşağıdaki değişikliklere sahiptir:

  * Kapasite, PAYG ortamları için uygulanamadığından kaldırılır.
  * **Zaman SERISI kimliği** özelliği eklendi. Verilerinizin bölümlenme şeklini belirler.
  * Başvuru veri kümeleri kaldırılır.
  * Görüntülenmiş URL sizi [Azure Time Series Insights Preview Explorer](./time-series-insights-update-explorer.md)'a yönlendirir.
  * Azure depolama hesabınızın adı listelenir.

* Azure portal **yapılandırma** dikey penceresi Azure Time Series Insights önizlemede KALDıRıLARAK, PAYG ortamları yapılandırılamaz. Ancak, Yeni tanıtılan yarı depoyu yapılandırmak için **depolama yapılandırması** 'nı kullanabilirsiniz.

* Başvuru verileri PAYG ortamlarının parçası olmadığından Azure portal **başvuru verileri** dikey penceresi Azure Time Series Insights önizlemede kaldırılır.

[Azure portal ![Time Series Insights önizleme ortamı](media/v2-update-manage/create-and-manage-overview-confirm.png)](media/v2-update-manage/create-and-manage-overview-confirm.png#lightbox)

## <a name="next-steps"></a>Sonraki adımlar

- [Ortamınızın planını](./time-series-insights-update-plan.md)okuyarak, genel olarak kullanılabilir ortamlar ve önizleme ortamları Time Series Insights hakkında daha fazla bilgi edinin.

- [Bir olay hub 'ı kaynağı eklemeyi](./time-series-insights-how-to-add-an-event-source-eventhub.md)öğrenin.

- [IoT Hub 'ı kaynağı](./time-series-insights-how-to-add-an-event-source-iothub.md)yapılandırın.
