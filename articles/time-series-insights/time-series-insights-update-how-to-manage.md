---
title: Azure zaman serisi önizlemesi sağlama ve yönetme | Microsoft Docs
description: Azure Time Series Insights önizlemesini sağlamayı ve yönetmeyi anlama.
author: ashannon7
ms.author: dpalled
ms.workload: big-data
manager: cshankar
ms.service: time-series-insights
services: time-series-insights
ms.topic: conceptual
ms.date: 09/04/2019
ms.custom: seodec18
ms.openlocfilehash: a84c587a2666982e862f0640b30d2d6bef3b9a42
ms.sourcegitcommit: 86d49daccdab383331fc4072b2b761876b73510e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/06/2019
ms.locfileid: "70744626"
---
# <a name="provision-and-manage-azure-time-series-insights-preview"></a>Azure Time Series Insights önizlemeyi sağlayın ve yönetin

Bu makalede, [Azure Portal](https://portal.azure.com/)kullanılarak Azure Time Series Insights önizleme ortamının nasıl oluşturulacağı ve yönetileceği açıklanmaktadır.

## <a name="overview"></a>Genel Bakış

Azure Time Series Insights Preview ortamları Kullandıkça Öde (PAYG) ortamlardır.

Azure Time Series Insights bir önizleme ortamı sağladığınızda iki Azure kaynağı oluşturursunuz:

* Azure Time Series Insights önizleme ortamı  
* Azure depolama genel amaçlı v1 hesabı
  
[Ortamınızı nasıl planlayacağınızı](./time-series-insights-update-plan.md)öğrenin.

>[!IMPORTANT]
> Önizleme için bir Azure depolama genel amaçlı v1 (GPv1) hesabı kullandığınızdan emin olun.

İsteğe bağlı olarak, her bir Azure Time Series Insights önizleme ortamını bir olay kaynağıyla ilişkilendirebilirsiniz. Daha fazla bilgi için, [Olay Hub 'ı kaynağı ekleme](./time-series-insights-how-to-add-an-event-source-eventhub.md) ve [IoT Hub kaynağı ekleme](./time-series-insights-how-to-add-an-event-source-iothub.md)makalesini okuyun. Bu adım sırasında zaman damgası KIMLIĞI özelliği ve benzersiz bir tüketici grubu sağlarsınız. Bunun yapılması, ortamın uygun olaylara erişmesini sağlar.

Sağlama tamamlandıktan sonra, erişim ilkelerinizi ve diğer ortam özniteliklerini iş gereksinimlerinize uyacak şekilde değiştirebilirsiniz.

## <a name="create-the-environment"></a>Ortamı oluşturma

Aşağıdaki adımlarda Azure Time Series Insights önizleme ortamının nasıl oluşturulacağı açıklanır:

1. **SKU** menüsünün altındaki **PAYG** düğmesini seçin. Bir ortam adı sağlayın ve hangi abonelik grubunu ve hangi kaynak grubunun kullanılacağını seçin. Daha sonra, ortamında barındırılmak için desteklenen bir konum seçin.

   [![Azure Time Series Insights bir örnek oluşturun.](media/v2-update-manage/manage-three.png)](media/v2-update-manage/manage-three.png#lightbox)

1. Bir zaman serisi KIMLIĞI girin.

    >[!NOTE]
    > * Zaman serisi KIMLIĞI, büyük/küçük harfe duyarlıdır ve sabittir. (Ayarlandıktan sonra değiştirilemez.)
    > * Zaman serisi kimlikleri en fazla üç anahtar olabilir.
    > * Zaman serisi KIMLIĞI seçme hakkında daha fazla bilgi için, okuma [zaman SERISI kimliği seçin](./time-series-insights-update-how-to-id.md).

1. Bir depolama hesabı adı seçerek ve bir çoğaltma seçeneği belirleyerek bir Azure depolama hesabı oluşturun. Bunun yapılması, otomatik olarak bir Azure depolama genel amaçlı v1 hesabı oluşturur. Daha önce seçtiğiniz Azure Time Series Insights önizleme ortamıyla aynı bölgede oluşturulur.

    [![Örneğiniz için bir Azure depolama hesabı oluşturun](media/v2-update-manage/manage-five.png)](media/v2-update-manage/manage-five.png#lightbox)

1. İsteğe bağlı olarak, bir olay kaynağı ekleyebilirsiniz.

   * Time Series Insights, [azure IoT Hub](./time-series-insights-how-to-add-an-event-source-iothub.md) ve [Azure Event Hubs](./time-series-insights-how-to-add-an-event-source-eventhub.md) seçeneklerini destekler. Ortam oluşturma zamanında yalnızca tek bir olay kaynağı ekleyebilseniz de daha sonra başka bir olay kaynağı ekleyebilirsiniz. Tüm olayların Azure Time Series Insights önizleme örneğiniz tarafından göründüğünden emin olmak için benzersiz bir tüketici grubu oluşturmak en iyisidir. Olay kaynağını eklerken mevcut bir tüketici grubunu seçebilir veya yeni bir tüketici grubu oluşturabilirsiniz.

   * Ayrıca uygun zaman damgası özelliğini de seçmeniz gerekir. Varsayılan olarak, Azure Time Series Insights her olay kaynağı için sıraya alınan iletiyi kullanır.

     > [!TIP]
     > Sıraya alınan mesaj saati, toplu iş olayında veya geçmiş verileri karşıya yükleme senaryolarında kullanılacak en iyi yapılandırılmış ayar olmayabilir. Bu gibi durumlarda zaman damgası özelliği kullanma veya kullanma kararını doğruladığınızdan emin olun.

     [![Olay kaynağı sekmesi](media/v2-update-manage/manage-two.png)](media/v2-update-manage/manage-two.png#lightbox)

1. Ortamınızın istenen ayarlarla sağlandığını doğrulayın.

    [![Gözden geçir + Oluştur sekmesi](media/v2-update-manage/manage-three.png)](media/v2-update-manage/manage-three.png#lightbox)

## <a name="manage-the-environment"></a>Ortamı yönetme

Azure Time Series Insights önizleme ortamınızı Azure portal kullanarak yönetebilirsiniz. Bu, Azure portal kullanarak bir bir bir, bir bir S1 veya S2 ortamının aksine, bir PAYG Azure Time Series Insights önizleme ortamının yönetilmesine ilişkin önemli farklardır:

* Azure portal **genel bakış** dikey penceresi, aşağıdaki yollarla Azure Time Series Insights değiştirilmez:
  * Bu kavram, PAYG ortamlarına uygun olmadığından kapasite kaldırılır.
  * Zaman serisi KIMLIĞI özelliği eklendi. Verilerinizin bölümlenme şeklini belirler.
  * Başvuru veri kümeleri kaldırılır.
  * Görüntülenmiş URL sizi [Azure Time Series Insights Preview Explorer](./time-series-insights-update-explorer.md)'a yönlendirir.
  * Azure depolama hesabınızın adı listelenir.

* Azure portal **yapılandırma** dikey penceresi Azure Time Series Insights önizlemede KALDıRıLMıŞTıR çünkü PAYG ortamları yapılandırılamaz.

* Başvuru verileri PAYG ortamlarının bir bileşeni olmadığından, Azure portal **başvuru verileri** dikey penceresi Azure Time Series Insights önizlemede kaldırılmıştır.

[![Azure portal Time Series Insights önizleme ortamı](media/v2-update-manage/manage-four.png)](media/v2-update-manage/manage-four.png#lightbox)

## <a name="next-steps"></a>Sonraki adımlar

- [Ortamınızı planınızı](./time-series-insights-update-plan.md)okuyun.

- [Bir olay hub 'ı kaynağı eklemeyi](./time-series-insights-how-to-add-an-event-source-eventhub.md)öğrenin.

- [IoT Hub 'ı kaynağı](./time-series-insights-how-to-add-an-event-source-iothub.md)yapılandırın.