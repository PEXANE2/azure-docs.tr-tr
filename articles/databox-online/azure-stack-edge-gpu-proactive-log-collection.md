---
title: Azure Stack Edge Pro cihazında öngörülü günlük toplamayı anlama
description: Azure Stack Edge Pro cihazında öngörülü günlük toplamanın nasıl yapıldığını ve devre dışı bırakılacağını açıklar.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: conceptual
ms.date: 02/23/2021
ms.author: alkohli
ms.openlocfilehash: bdd44bf81e34b60ee648b71c6dc3bde5a96d3deb
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/20/2021
ms.locfileid: "102443021"
---
# <a name="proactive-log-collection-on-your-azure-stack-edge-device"></a>Azure Stack Edge cihazınızda öngörülü günlük toplama

[!INCLUDE [applies-to-GPU-and-pro-r-and-mini-r-skus](../../includes/azure-stack-edge-applies-to-gpu-pro-r-mini-r-sku.md)]

Öngörülü günlük toplama, cihaz sorunlarını verimli bir şekilde gidermenize yardımcı olması için Azure Stack Edge cihazınızda sistem durumu göstergelerini toplar. Öngörülü günlük toplama varsayılan olarak etkindir. Bu makalede nelerin günlüğe kaydedildiği, Microsoft 'un verileri nasıl işlediği ve öngörülü günlük toplamanın nasıl devre dışı bırakılacağı veya etkinleştirileceği açıklanmaktadır. 

Bu makaledeki bilgiler Azure Stack Edge Pro GPU, Azure Stack Edge Pro R ve Azure Stack Edge Mini R cihazları için geçerlidir.

## <a name="about-proactive-log-collection"></a>Öngörülü günlük toplama hakkında

Microsoft müşteri desteği ve Mühendisliği ekipleri, işlem sırasında ortaya çıkan sorunları verimli bir şekilde tanımlamak ve gidermek için Azure Stack Edge cihazınızdan sistem günlüklerini kullanır. Öngörülü günlük toplama, Microsoft 'un müşterinin Azure Stack Edge gereci tarafından bir sorun/olay algıladığını belirten bir yöntemdir. İzlenen olaylar için bkz. [proaktif günlük toplama göstergeleri](#proactive-log-collection-indicators) . Sorunla ilgili destek günlükleri, Microsoft tarafından yönetilen ve denetleyen bir Azure depolama hesabına otomatik olarak yüklenir. Microsoft Desteği ve Microsoft mühendisleri, müşteriyle ilgili sorunu çözmeye yönelik en iyi eylemi öğrenmek için bu destek günlüklerini gözden geçirir.

> [!NOTE]
> Bu günlükler yalnızca hata ayıklama amacıyla ve sorunlar durumunda müşterilere destek sağlamak için kullanılır.


## <a name="enabling-proactive-log-collection"></a>Öngörülü günlük toplamayı etkinleştirme

Öngörülü günlük toplama varsayılan olarak etkindir. Cihazı yerel kullanıcı arabirimi aracılığıyla etkinleştirmeye çalışırken öngörülü günlük toplamayı devre dışı bırakabilirsiniz. 

1. Cihazın yerel Web Kullanıcı arabiriminde **Başlarken** sayfasına gidin.

2. **Etkinleştirme** kutucuğunda **Etkinleştir**' i seçin. 

    ![Yerel Web Kullanıcı arabirimi "bulut ayrıntıları" sayfa 1](./media/azure-stack-edge-pro-r-deploy-activate/activate-1.png)

3. **Etkinleştir** bölmesinde:

   1. [Azure Stack Edge Pro R için etkinleştirme anahtarını al](azure-stack-edge-pro-r-deploy-prep.md#get-the-activation-key)bölümünde aldığınız **etkinleştirme anahtarını** girin.

      Etkin olduktan sonra, öngörülü günlük toplama varsayılan olarak etkindir ve Microsoft 'un, cihazın sistem durumunu temel alarak günlükleri toplamasına izin verir. Bu günlükler bir Azure depolama hesabına yüklenir. 

      Microsoft 'un günlükleri toplamasını durdurmak için öngörülü günlük toplamayı devre dışı bırakabilirsiniz.

   1. Cihazda öngörülü günlük toplamayı devre dışı bırakmak istiyorsanız, **devre dışı bırak**' ı seçin.

   1. **Etkinleştir**' i seçin.

   ![Yerel Web Kullanıcı arabirimi "bulut ayrıntıları" sayfa 2](./media/azure-stack-edge-pro-r-deploy-activate/activate-2.png)

## <a name="proactive-log-collection-indicators"></a>Öngörülü günlük toplama göstergeleri

Öngörülü günlük koleksiyonu etkinleştirildikten sonra, cihazda aşağıdaki olaylardan biri algılandığında Günlükler otomatik olarak yüklenir:  


|Uyarı/hata/koşul  |Description  |
|---------|---------|
|Acsunhealthyıcondition     |Azure tutarlı hizmetler sağlıklı değil.         |
|IOTEdgeAgentNotRunningCondition      |IoT Edge Aracısı çalışmıyor.         |
|Updateınstallfailedevent | Güncelleştirme yüklenemedi.        |

 
Microsoft, önceki listeye yeni olaylar eklemeye devam edecektir. Yeni olaylar için ek izin gerekmez. En güncel öngörülü günlük toplama göstergeleri hakkında bilgi edinmek için bu sayfaya bakın.    
 

## <a name="other-log-collection-methods"></a>Diğer günlük toplama yöntemleri

Algılanan belirli bir sorunla ilgili belirli günlükleri toplayan öngörülü günlük toplamanın yanı sıra, diğer günlük koleksiyonları sistem durumu ve davranışını daha ayrıntılı bir şekilde anlayabilirler. Genellikle, bu diğer Günlükler destek isteği sırasında toplanabilir veya Microsoft tarafından cihazdaki telemetri verilerine bağlı olarak tetiklenebilir.

## <a name="handling-data"></a>Verileri işleme

Öngörülü günlük toplama etkinleştirildiğinde, müşteri, burada açıklandığı gibi Azure Stack Edge cihazında Microsoft 'un günlükleri toplamasını kabul eder. Müşteri Ayrıca, Microsoft tarafından yönetilen ve denetlenen bir Azure depolama hesabında bu günlüklerin karşıya yüklenmesini ve saklanmasını de onaylar ve onaylar.

Microsoft, yalnızca sistem durumu ve sorunlarını gidermek için verileri kullanır. Veriler pazarlama, reklam veya müşteri izni olmadan herhangi bir ticari amaçla kullanılmaz. 

Microsoft tarafından toplanan veriler standart gizlilik uygulamalarımıza göre işlenir. Bir müşteri, öngörülü günlük toplama iznini iptal etmeye karar verirse, iptalden önce onay ile toplanan tüm veriler etkilenmez.

## <a name="next-steps"></a>Sonraki adımlar

[Destek paketini nasıl toplayacağınızı](azure-stack-edge-gpu-troubleshoot.md#collect-support-package)öğrenin.