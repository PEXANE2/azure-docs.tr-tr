---
title: Azure VMware Çözüm dağıtımı veya sağlama hatalarıyla ilgili yardım alın
description: Azure VMware Çözüm dağıtımı veya sağlama hatalarıyla ilgili bir hizmet isteği için Azure VMware Çözüm özel bulutunuzda gereken bilgileri alma.
ms.topic: how-to
ms.date: 06/09/2020
ms.openlocfilehash: 1f46dde895db417fd2b488a6203d5482e73d3c5e
ms.sourcegitcommit: 400f473e8aa6301539179d4b320ffbe7dfae42fe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/28/2020
ms.locfileid: "92779501"
---
# <a name="get-help-with-azure-vmware-solution-deployment-or-provisioning-failures"></a>Azure VMware Çözüm dağıtımı veya sağlama hatalarıyla ilgili yardım alın

Bu makale, Azure VMware Çözüm dağıtımı ve sağlama hatalarıyla ilgili size yardımcı olur. Özel bulutunuzda hatalara sahip olduğunuzda, Azure portal bir [destek isteği](https://rc.portal.azure.com/#create/Microsoft.Support) (SR) açmanız gerekir. 

Ancak, Azure portal bazı önemli bilgileri toplamanız gerekir:

- Bağıntı Kimliği
- ExpressRoute bağlantı hattı KIMLIĞI

## <a name="collect-the-correlation-id"></a>Bağıntı KIMLIĞINI toplayın
 
Bir bağıntı KIMLIĞI, Azure 'da özel bir bulut veya herhangi bir kaynak oluşturduğunuzda üretilir. Her Azure Resource Manager dağıtımı da bir bağıntı KIMLIĞI oluşturur. Bu KIMLIK, daha hızlı SR oluşturma ve çözümleme sağlar. 
 
Bağıntı KIMLIĞI vurgulanmış şekilde, başarısız bir özel bulut dağıtımının çıkışının bir örneği aşağıda verilmiştir.

:::image type="content" source="media/fix-deployment-provisioning-failures/failed-private-cloud-deployment.png" alt-text="Bağıntı KIMLIĞIYLE özel bulut dağıtımı başarısız oldu.":::

Hizmet isteğine dahil etmek için bu bağıntı KIMLIĞINI kopyalayın ve kaydedin. Ayrıntılar için bu makalenin sonundaki [destek Isteğinizi oluşturma](#create-your-support-request) bölümüne bakın.

Başarısızlık ön doğrulama aşamasında oluşursa, hiçbir bağıntı KIMLIĞI oluşturulmaz. Bu durumda, aşağıdakiler dahil olmak üzere Azure VMware çözümü özel bulutu oluştururken kullandığınız bilgileri sağlayabilirsiniz:

- Konum
- Kaynak grubu
- Kaynak adı
 
### <a name="collect-a-summary-of-errors"></a>Hataların özetini toplayın

Hataların ayrıntıları sorununuzu çözmeye da yardımcı olabilir. Önceki ekrandan, hataların özetini görmek için uyarı iletisini seçin.
 
 :::image type="content" source="media/fix-deployment-provisioning-failures/summary-of-errors.png" alt-text="Bağıntı KIMLIĞIYLE özel bulut dağıtımı başarısız oldu.":::

Bu Özeti, SR 'ye dahil etmek için kopyalayıp kaydedin.
 
### <a name="retrieve-past-deployments"></a>Geçmiş dağıtımları al

Bildirimler simgesini seçerek dağıtım etkinlik günlüğü ' nde arayarak, başarısız olanlar da dahil olmak üzere geçmiş dağıtımları alabilirsiniz.

:::image type="content" source="media/fix-deployment-provisioning-failures/open-notifications.png" alt-text="Bağıntı KIMLIĞIYLE özel bulut dağıtımı başarısız oldu.":::

Bildirimler ' de, **etkinlik günlüğünde daha fazla olay** seçin.

:::image type="content" source="media/fix-deployment-provisioning-failures/more-events-in-activity-log.png" alt-text="Bağıntı KIMLIĞIYLE özel bulut dağıtımı başarısız oldu.":::

Ardından, başarısız dağıtımı ve onun bağıntı KIMLIĞINI bulmak üzere kaynağı oluşturmak için kullanılan kaynağın adını veya diğer bilgileri arayın. Aşağıdaki örnek, bir özel bulut kaynağında (PC03) arama sonuçlarını gösterir.
 
:::image type="content" source="media/fix-deployment-provisioning-failures/find-past-deployments.png" alt-text="Bağıntı KIMLIĞIYLE özel bulut dağıtımı başarısız oldu.":::
 
Başarısız dağıtımın işlem adını seçmek, ayrıntıları içeren bir pencere açar. JSON sekmesini seçin ve Bağıntıkimliği ' ı arayın. SR 'ye kopyalayın ve ekleyin. 
 
## <a name="collect-the-expressroute-id-uri"></a>ExpressRoute KIMLIĞINI (URI) toplayın
 
Özel bulut ExpressRoute bağlantı hattını kullanarak var olan bir özel bulutu ölçeklendirmeye veya ona eşlemenize çalışıyor olabilirsiniz. Bu durumda, ExpressRoute KIMLIĞI gerekir. 

Azure portal, **bağlantı > ExpressRoute** ' ı seçin ve **ExpressRoute kimliğini** panonuza kopyalayın.
 
:::image type="content" source="media/fix-deployment-provisioning-failures/expressroute-id.png" alt-text="Bağıntı KIMLIĞIYLE özel bulut dağıtımı başarısız oldu."::: 
 
> [!NOTE]
> Bazen, ön doğrulama denetimleri dağıtımdan önce başarısız olabilir ve yalnızca kullanılabilir bilgiler hata ve hata iletileri olur. Bunlar, örneğin, kotayla ilgili sorunlar gibi birçok hatalarda yararlı olabilir ve bu iletilerin destek isteğine eklenmesi önemlidir. Bunları toplamak için önceki bölüme bakın ve [hata özetini toplayın](#collect-a-summary-of-errors).

## <a name="create-your-support-request"></a>Destek isteğinizi oluşturun

Destek isteğinizi oluşturma konusunda genel yönergeler için bkz. [Azure destek isteği oluşturma](../azure-portal/supportability/how-to-create-azure-support-request.md). 

Azure VMware Çözüm dağıtımı veya sağlama hatalarıyla ilgili bir SR oluşturmak için aşağıda belirli bir kılavuzluk vardır.

1. **Yardım** simgesini ve ardından **+ Yeni destek isteği** ' ni seçin.

    :::image type="content" source="media/fix-deployment-provisioning-failures/open-sr-on-avs.png" alt-text="Bağıntı KIMLIĞIYLE özel bulut dağıtımı başarısız oldu.":::

2. Tüm gerekli alanları ve **temel bilgiler** sekmesinde doldurabilirsiniz:

    - **Sorun türü** için **yapılandırma ve kurulum sorunları** ' nı seçin.

    - **Sorun alt türü** için **özel bulut sağla** ' yı seçin.

3. **Ayrıntılar** sekmesinde:

    - Tüm gerekli alanları doldur.

    - Bağıntı KIMLIĞINIZI veya ExpressRoute KIMLIĞINIZI, belirtilen belirli alanlara yapıştırın. Belirli bir alanı görmüyorsanız, **sorun hakkındaki ayrıntıları sağlamak** altındaki metin kutusuna yapıştırabilirsiniz.

    - Kopyaladığınız hataların Özeti dahil olmak üzere herhangi bir hata ayrıntılarını, **sorun hakkındaki ayrıntıları sağlayan** metin kutusuna yapıştırın.

4. SR 'nizi oluşturmak için gözden geçirin ve **Oluştur** ' u seçin.
