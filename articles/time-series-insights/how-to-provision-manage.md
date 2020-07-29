---
title: Gen 2 ortamı sağlama ve yönetme-Azure zaman serisi | Microsoft Docs
description: Azure Time Series Insights Gen 2 ortamını sağlamayı ve yönetmeyi öğrenin.
author: shipra1mishra
ms.author: shmishr
manager: diviso
ms.workload: big-data
ms.service: time-series-insights
services: time-series-insights
ms.topic: conceptual
ms.date: 07/08/2020
ms.custom: seodec18
ms.openlocfilehash: e71c426fee62187bb680c7b8bd1af92d87e85274
ms.sourcegitcommit: dccb85aed33d9251048024faf7ef23c94d695145
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/28/2020
ms.locfileid: "87288802"
---
# <a name="provision-and-manage-azure-time-series-insights-gen2"></a>Azure Time Series Insights Gen2 sağlama ve yönetme

Bu makalede [Azure Portal](https://portal.azure.com/)kullanarak bir Azure Time Series Insights Gen2 ortamının nasıl oluşturulacağı ve yönetileceği açıklanmaktadır.

## <a name="overview"></a>Genel Bakış

Bir Azure Time Series Insights Gen2 ortamı sağladığınızda şu Azure kaynaklarını oluşturursunuz:

* Kullandıkça Öde fiyatlandırma modelini izleyen bir Azure Time Series Insights Gen2 ortamı
* Bir Azure depolama hesabı
* Daha hızlı ve sınırsız sorgu için isteğe bağlı bir sıcak mağaza

> [!TIP]
> * [Ortamınızı nasıl planlayacağınızı](./time-series-insights-update-plan.md)öğrenin.
> * [Bir olay hub 'ı kaynağı ekleme](./time-series-insights-how-to-add-an-event-source-eventhub.md) veya [IoT Hub kaynağı ekleme](./time-series-insights-how-to-add-an-event-source-iothub.md)hakkında bilgi edinin.

Şunları öğrenirsiniz:

1.  Her Azure Time Series Insights Gen 2 ortamını bir olay kaynağıyla ilişkilendirin. Ayrıca, ortamın uygun olaylara erişimi olduğundan emin olmak için bir zaman damgası KIMLIĞI özelliği ve benzersiz bir tüketici grubu da sağlarsınız.

1. Sağlama tamamlandıktan sonra, erişim ilkelerinizi ve diğer ortam özniteliklerini, iş gereksinimlerinize uyacak şekilde değiştirebilirsiniz.

   > [!NOTE]
   > Bir ortam sağlanırken ilk adım isteğe bağlıdır. Bu adımı atlarsanız, verilerin ortamınıza akmasını ve sorgu aracılığıyla erişilebilir olması için, daha sonra ortama bir olay kaynağı eklemeniz gerekir.

## <a name="create-the-environment"></a>Ortamı oluşturma

Azure Time Series Insights Gen 2 ortamı oluşturmak için:
1. [Azure Portal](https://portal.azure.com/)üzerinde *Nesnelerin İnterneti* altında bir Azure Time Series Insights kaynağı oluşturun.

1. **Katman**olarak **Gen2 (L1)** seçeneğini belirleyin. Bir ortam adı sağlayın ve kullanılacak abonelik grubunu ve kaynak grubunu seçin. Ardından, ortamı barındırmak için desteklenen bir konum seçin.

   [![Azure Time Series Insights bir örnek oluşturun.](media/v2-update-manage/create-and-manage-configuration.png)](media/v2-update-manage/create-and-manage-configuration.png#lightbox)

1. Bir zaman serisi KIMLIĞI girin.

    > [!NOTE]
    > * Zaman serisi KIMLIĞI, *büyük/küçük harfe duyarlıdır* ve *sabittir*. (Ayarlandıktan sonra değiştirilemez.)
    > * Zaman serisi kimlikleri en fazla *üç* anahtar olabilir. Bunu, ortamınızdaki verileri gönderen her bir cihaz algılayıcısını benzersiz bir şekilde temsil eden bir veritabanında birincil anahtar olarak düşünün. Tek bir özellik veya bir veya daha fazla üç özelliğin birleşimi olabilir.
    > * [Zaman SERISI kimliğini seçme](time-series-insights-update-how-to-id.md) hakkında daha fazla bilgi edinin

1. Bir depolama hesabı adı, hesap türü seçerek ve bir [çoğaltma](https://docs.microsoft.com/azure/storage/common/redundancy-migration?tabs=portal) seçeneği belirleyerek bir Azure depolama hesabı oluşturun. Bunun yapılması, otomatik olarak bir Azure depolama hesabı oluşturur. Varsayılan olarak, [genel amaçlı v2](https://docs.microsoft.com/azure/storage/common/storage-account-overview) hesabı oluşturulur. Hesap, daha önce seçtiğiniz Azure Time Series Insights Gen2 ortamı ile aynı bölgede oluşturulur. Alternatif olarak, yeni bir Azure zaman serisi Gen2 ortamı oluştururken [ARM şablonu](./time-series-insights-manage-resources-using-azure-resource-manager-template.md) aracılığıyla kendi depolama alanınızı (BYOS) de getirebilirsiniz. 

1. **(Isteğe bağlı)** Ortamınızdaki en son veriler üzerinde daha hızlı ve sınırsız sorgu istiyorsanız ortamınız için ısınma mağazasını etkinleştirin. Ayrıca, bir Azure Time Series Insights Gen2 ortamı oluşturduktan sonra sol gezinti bölmesindeki **depolama yapılandırması** seçeneği aracılığıyla bir sıcak mağaza oluşturabilir veya silebilirsiniz.

1. **(Isteğe bağlı)** Şimdi bir olay kaynağı ekleyebilirsiniz. Örnek sağlandıktan sonra da bekleyebilirsiniz.

   * Azure Time Series Insights, [azure IoT Hub](./time-series-insights-how-to-add-an-event-source-iothub.md) ve [Azure Event Hubs](./time-series-insights-how-to-add-an-event-source-eventhub.md) 'yi olay kaynağı seçenekleri olarak destekler. Ortamı oluştururken yalnızca tek bir olay kaynağı ekleyebilseniz de daha sonra başka bir olay kaynağı ekleyebilirsiniz. 
   
     Olay kaynağını eklediğinizde, mevcut bir tüketici grubunu seçebilir veya yeni bir tüketici grubu oluşturabilirsiniz. Lütfen olay kaynağının, ortamınızda veri okuyabilmesi için benzersiz bir tüketici grubu gerektirdiğini unutmayın.

   * Uygun zaman damgası özelliğini seçin. Varsayılan olarak, Azure Time Series Insights her olay kaynağı için ileti temelli sıraya alma süresini kullanır.

     > [!TIP]
     > İleti temelli sıraya alma saati, toplu olay senaryolarında veya geçmiş verileri karşıya yükleme senaryolarında kullanılacak en iyi yapılandırılmış ayar olmayabilir. Bu gibi durumlarda, bir zaman damgası özelliği kullanmayın veya kullanma kararını doğruladığınızdan emin olun.

     [![Olay kaynağı Yapılandırma sekmesi](media/v2-update-manage/create-and-manage-event-source.png)](media/v2-update-manage/create-and-manage-event-source.png#lightbox)

1. Ortamınızın sağlandığını ve istediğiniz şekilde yapılandırıldığını onaylayın.

    [![Gözden geçir + Oluştur sekmesi](media/v2-update-manage/create-and-manage-review-and-confirm.png)](media/v2-update-manage/create-and-manage-review-and-confirm.png#lightbox)

## <a name="manage-the-environment"></a>Ortamı yönetme

Azure portal kullanarak Azure Time Series Insights Gen2 ortamınızı yönetebilirsiniz. Ortamınızı Azure portal aracılığıyla yönetirken göz önünde bulundurmanız için Gen2 ortamı ve Gen1 S1 veya Gen1 S2 ortamları arasında birkaç temel fark vardır:

* Azure portal Gen2 **genel bakış** dikey penceresi aşağıdaki değişikliklere sahiptir:

  * Kapasite, Gen2 ortamları için uygulanmadığından kaldırılır.
  * **Zaman SERISI kimliği** özelliği eklendi. Verilerinizin bölümlenme şeklini belirler.
  * Başvuru veri kümeleri kaldırılır.
  * Görüntülenmiş URL sizi [Azure Time Series Insights Gezgini](./time-series-insights-update-explorer.md)'ne yönlendirir.
  * Azure depolama hesabınızın adı listelenir.

* Ölçek birimleri Azure Time Series Insights Gen2 ortamlarına uygulanmadığından Azure portal **yapılandırma** dikey penceresi kaldırılır. Ancak, Yeni tanıtılan yarı depoyu yapılandırmak için **depolama yapılandırması** 'nı kullanabilirsiniz.

* Başvuru verileri kavramı, [zaman serisi modeliyle (TSD)](/azure/time-series-insights/concepts-model-overview)değiştirildiğinden, Azure Portal **başvuru verileri** dikey penceresi Azure Time Series Insights Gen2 ' de kaldırılır.

[![Azure portal Gen2 ortamı Azure Time Series Insights](media/v2-update-manage/create-and-manage-overview-confirm.png)](media/v2-update-manage/create-and-manage-overview-confirm.png#lightbox)

## <a name="next-steps"></a>Sonraki adımlar

- [Ortamınızın planını](./time-series-insights-update-plan.md)okuyarak, genel olarak kullanılabilir ortamlar ve Gen2 ortamları Azure Time Series Insights hakkında daha fazla bilgi edinin.

- [Bir olay hub 'ı kaynağı eklemeyi](./time-series-insights-how-to-add-an-event-source-eventhub.md)öğrenin.

- [IoT Hub 'ı kaynağı](./time-series-insights-how-to-add-an-event-source-iothub.md)yapılandırın.
