---
title: Azure Stack Edge Pro cihazında öngörülü günlük toplamayı anlama
description: Azure Stack Edge Pro cihazında öngörülü günlük toplamanın nasıl yapılacağını açıklar.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: conceptual
ms.date: 11/03/2020
ms.author: alkohli
ms.openlocfilehash: f79de47ec0ffad11f650054b581dbbaae030edbf
ms.sourcegitcommit: 6a350f39e2f04500ecb7235f5d88682eb4910ae8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/01/2020
ms.locfileid: "96466964"
---
# <a name="proactive-log-collection-on-your-azure-stack-edge-device"></a>Azure Stack Edge cihazınızda öngörülü günlük toplama

Cihaz sorunlarını verimli bir şekilde gidermeye yardımcı olması için, Azure Stack Edge cihazınızda, sistem durumu göstergelerine göre öngörülü günlük toplamayı etkinleştirebilirsiniz. Bu makalede öngörülü günlük toplama, nasıl etkinleştirileceği ve öngörülü günlük toplama etkinleştirildiğinde verilerin nasıl işlendiği açıklanır.
   
Bu makaledeki bilgiler Azure Stack Edge Pro GPU, Azure Stack Edge Pro R ve Azure Stack Edge Mini R cihazları için geçerlidir.

## <a name="about-proactive-log-collection"></a>Öngörülü günlük toplama hakkında

Microsoft müşteri desteği ve Mühendisliği ekipleri, işlem sırasında ortaya çıkan sorunları verimli bir şekilde tanımlamak ve gidermek için Azure Stack Edge cihazınızdan sistem günlüklerini kullanır. Öngörülü günlük toplama, Microsoft 'un bir sorun/olay (izlenmekte olan olaylar için öngörülü günlük toplama göstergeleri bölümüne bakın), müşterinin Azure Stack Edge gereci tarafından algılandığına yönelik bir yöntemdir. Sorunla ilgili destek günlükleri, Microsoft tarafından yönetilen ve denetlenen bir Azure depolama hesabına otomatik olarak yüklenir. Microsoft Desteği ve Microsoft mühendisleri, müşteriyle ilgili sorunu çözmeye yönelik en iyi eylemi öğrenmek için bu destek günlüklerini gözden geçirir.    

> [!NOTE]
> Bu günlükler yalnızca hata ayıklama amacıyla kullanılır ve sorunlar söz konusu olduğunda müşterilere destek sağlar. 


## <a name="enabling-proactive-log-collection"></a>Öngörülü günlük toplamayı etkinleştirme

Cihazı, yerel kullanıcı arabirimi aracılığıyla etkinleştirmeye çalışırken öngörülü günlük toplamayı etkinleştirebilirsiniz. 

1. Cihazın yerel Web Kullanıcı arabiriminde, **Başlarken** sayfasına gidin.
2. **Etkinleştirme** kutucuğunda **Etkinleştir**' i seçin. 

    ![Yerel Web Kullanıcı arabirimi "bulut ayrıntıları" sayfa 1](./media/azure-stack-edge-pro-r-deploy-activate/activate-1.png)
    
3. **Etkinleştir** bölmesinde:
    1. [Azure Stack Edge Pro R için etkinleştirme anahtarını al](azure-stack-edge-pro-r-deploy-prep.md#get-the-activation-key)bölümünde aldığınız **etkinleştirme anahtarını** girin.

    1. Microsoft 'un günlükleri cihazın sistem durumunu temel alarak toplamasını sağlamak için öngörülü günlük toplamayı etkinleştirebilirsiniz. Bu şekilde toplanan Günlükler bir Azure depolama hesabına yüklenir.
    
    1. **Uygula**’yı seçin.

    ![Yerel Web Kullanıcı arabirimi "bulut ayrıntıları" sayfa 2](./media/azure-stack-edge-pro-r-deploy-activate/activate-2.png)



## <a name="proactive-log-collection-indicators"></a>Öngörülü günlük toplama göstergeleri

Öngörülü günlük koleksiyonu etkinleştirildikten sonra, cihazda aşağıdaki olaylardan biri algılandığında Günlükler otomatik olarak yüklenir:  


|Uyarı/hata/koşul  |Açıklama  |
|---------|---------|
|Acsunhealthyıcondition     |Azure tutarlı hizmetler sağlıklı değil.         |
|IOTEdgeAgentNotRunningCondition      |IoT Edge Aracısı çalışmıyor.         |
|Updateınstallfailedevent | Güncelleştirme yüklenemedi.        |

 
Microsoft, önceki listeye yeni olaylar eklemeye devam edecektir. Yeni olaylar için ek izin gerekmez. En güncel öngörülü günlük toplama göstergeleri hakkında bilgi edinmek için bu sayfaya bakın.    
 

## <a name="other-log-collection-methods"></a>Diğer günlük toplama yöntemleri

Algılanan belirli bir sorunla ilgili belirli günlükleri toplayan öngörülü günlük toplamanın yanı sıra sistem durumu ve davranışının çok daha ayrıntılı olarak anlaşılmasına neden olabilecek başka günlük koleksiyonları vardır. Genellikle bu diğer günlük koleksiyonları bir destek isteği sırasında Yürütülebilirler veya Microsoft tarafından cihazın sağladığı telemetri verilerine göre tetiklenir.  

## <a name="handling-data"></a>Verileri işleme

Bir müşteri öngörülü günlük toplamayı etkinleştiriyorlarsa, burada açıklandığı gibi, Microsoft 'un günlükleri Azure Stack Edge cihazından toplamasını kabul etmiş olursunuz. Müşteri Ayrıca, Microsoft tarafından yönetilen ve denetlenen bir Azure depolama hesabında bu günlüklerin karşıya yüklenmesini ve saklanmasını de onaylar ve onaylar.

Microsoft, yalnızca sistem durumu ve sorunlarını gidermek için verileri kullanır. Veriler pazarlama, reklam veya müşteri izni olmadan herhangi bir ticari amaçla kullanılmaz. 

Microsoft tarafından toplanan veriler standart gizlilik uygulamalarımıza göre işlenir. Bir müşteri, öngörülü günlük toplama iznini iptal etmeye karar verirse, iptalden önce onay ile toplanan tüm veriler etkilenmez.

## <a name="next-steps"></a>Sonraki adımlar

[Destek paketini nasıl toplayacağınızı](azure-stack-edge-gpu-troubleshoot.md#collect-support-package)öğrenin.