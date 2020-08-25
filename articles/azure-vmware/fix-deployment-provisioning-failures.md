---
title: Azure VMware Çözüm dağıtımı veya sağlama hatalarıyla ilgili yardım alın
description: Azure VMware Çözüm dağıtımı veya sağlama hatalarıyla ilgili bir hizmet isteği için Azure VMware Çözüm özel bulutunuzda gereken bilgileri alma.
ms.topic: how-to
ms.date: 06/09/2020
ms.openlocfilehash: 63d5440a9e2b15463e465e1d32762889508feca1
ms.sourcegitcommit: 62717591c3ab871365a783b7221851758f4ec9a4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/22/2020
ms.locfileid: "88752227"
---
# <a name="get-help-with-azure-vmware-solution-deployment-or-provisioning-failures"></a>Azure VMware Çözüm dağıtımı veya sağlama hatalarıyla ilgili yardım alın

Bu makalede, Azure portal bir hizmet isteği (SR) açarak özel bulutunuzda Azure VMware Çözüm dağıtımı veya sağlama hatalarıyla ilgili nasıl yardım alabileceğinizi öğreneceksiniz. Ancak, Azure portal bazı önemli bilgileri toplamanız gerekir. Çoğu durumda, şunlar gerekir:

- Bağıntı KIMLIĞI (başarısız olan dağıtım)
- ExpressRoute bağlantı hattı KIMLIĞI (özel bulut ExpressRoute devresi ile var olan bir özel bulutu ölçeklendirmeye veya eşe yönelik olarak

## <a name="collect-the-correlation-id"></a>Bağıntı KIMLIĞINI toplayın
 
Önce bağıntı KIMLIĞINE bakalım. Özel bir bulut (veya Azure 'da herhangi bir kaynak) oluşturduğunuzda ilişkili bir bağıntı KIMLIĞI oluşturulur. Her Azure Resource Manager dağıtımı da benzersiz bir bağıntı KIMLIĞI üretir. Bu KIMLIK, daha hızlı SR oluşturma ve çözümleme sağlar. 
 
Bağıntı KIMLIĞI vurgulanmış şekilde, başarısız bir özel bulut dağıtımının çıkışının bir örneği aşağıda verilmiştir.

:::image type="content" source="media/fix-deployment-provisioning-failures/failed-private-cloud-deployment.png" alt-text="Bağıntı KIMLIĞIYLE özel bulut dağıtımı başarısız oldu.":::

Hizmet isteğine dahil etmek için bu bağıntı KIMLIĞINI kopyalayın ve kaydedin. Ayrıntılar için bu makalenin sonundaki [destek Isteğinizi oluşturma](#create-your-support-request) bölümüne bakın.

Hata, ön doğrulama aşamasında oluşursa, özel bir bulut dağıtılmadan önce bir bağıntı KIMLIĞI oluşturulmaz. Bu durumda, aşağıdakiler de dahil olmak üzere Azure VMware çözümü özel bulutu oluştururken kullandığınız bilgileri sağlamanız yeterlidir:

- Konum
- Kaynak grubu
- Kaynak adı
 
### <a name="collect-a-summary-of-errors"></a>Hataların özetini toplayın

Hataların ayrıntıları, sorununuzun çözümünde de yararlı olabilir. Önceki ekrandan, **Ayrıntılar için buraya tıklayın** (vurgulanmış) ve hata Özeti aşağıdaki ekran görüntüsünde gösterildiği gibi açılır.
 
 :::image type="content" source="media/fix-deployment-provisioning-failures/summary-of-errors.png" alt-text="Hataların Özeti.":::

Bu Özeti, SR 'ye dahil etmek için kopyalayıp kaydedin.
 
### <a name="retrieve-past-deployments"></a>Geçmiş dağıtımları al

Bildirimler simgesini seçerek dağıtım etkinlik günlüğü ' nde arayarak, başarısız olanlar da dahil olmak üzere geçmiş dağıtımları alabilirsiniz.

:::image type="content" source="media/fix-deployment-provisioning-failures/open-notifications.png" alt-text="Bildirimleri açın.":::

Bildirimler ' de, **etkinlik günlüğünde daha fazla olay**seçin.

:::image type="content" source="media/fix-deployment-provisioning-failures/more-events-in-activity-log.png" alt-text="Bağlantı: etkinlik günlüğünde daha fazla olay.":::

Ardından, başarısız dağıtımı ve onun bağıntı KIMLIĞINI bulmak için kaynak adını veya kaynağı oluştururken kullandığınız başka bir benzersiz bilgi parçasını arayın. Aşağıdaki örnek, bir özel bulut kaynağında (PC03) arama sonuçlarını gösterir.
 
:::image type="content" source="media/fix-deployment-provisioning-failures/find-past-deployments.png" alt-text="Geçmiş Azure VMware Çözüm dağıtımlarını bulma işlemi başarısız oldu.":::
 
Başarısız dağıtımın işlem adını seçmek, ayrıntıları içeren bir pencere açar. JSON sekmesini seçin ve Bağıntıkimliği ' ı arayın. SR 'ye kopyalayın ve ekleyin. 
 
## <a name="collect-the-expressroute-id-uri"></a>ExpressRoute KIMLIĞINI (URI) toplayın
 
Belki de özel bir buluta sahipsiniz ve özel Cloud ExpressRoute bağlantı hattı ile bunu veya eşi ölçeklendirmeye çalıştığınızda bir hata yaşıyorsunuz. Bu durumda, özel bulutun ExpressRoute KIMLIĞI, bir SR oluşturduğunuzda tanımlamak için kullanılabilir.

Portalda özel bir bulutu görüntülerken **bağlantı > ExpressRoute** ' ı seçin ve **ExpressRoute kimliğini** panonuza kopyalayın.
 
:::image type="content" source="media/fix-deployment-provisioning-failures/expressroute-id.png" alt-text="ExpressRoute KIMLIĞINI panoya kopyalayın."::: 
 
ExpressRoute KIMLIĞINI yeni destek isteğindeki ilgili alana yapıştırın. Daha fazla bilgi için aşağıdaki bölüme bakın, [destek Isteğinizi oluşturun](#create-your-support-request).
 
> [!NOTE]
> Bazen, ön doğrulama denetimleri bir dağıtımdan önce başarısız olabilir ve kullanılabilir tek bilgiler hata ve/veya hata iletileri olacaktır. Bunlar, kota ile ilgili sorunlar ve bu iletilerin destek isteğine dahil edilmesi açısından çok sayıda hatalarda yararlı olabilir. Bunları toplamak için önceki bölüme bakın ve [hata özetini toplayın](#collect-a-summary-of-errors).

## <a name="create-your-support-request"></a>Destek isteğinizi oluşturun

Destek isteğinizi oluşturma konusunda genel yönergeler için bkz. [Azure destek isteği oluşturma](../azure-portal/supportability/how-to-create-azure-support-request.md). 

Azure VMware Çözüm dağıtımı veya sağlama hatalarıyla ilgili bir SR oluşturmaya özgü ek rehberlik aşağıda verilmiştir.

1. **Yardım** simgesini ve ardından **+ Yeni destek isteği**' ni seçin.

    :::image type="content" source="media/fix-deployment-provisioning-failures/open-sr-on-avs.png" alt-text="SR 'niz için bir ExpressRoute KIMLIĞI toplayın.":::

2. Tüm gerekli alanları ve **temel bilgiler** sekmesinde doldurabilirsiniz:

    - **Sorun türü**için **yapılandırma ve kurulum sorunları**' nı seçin.

    - **Sorun alt türü**için **özel bulut sağla**' yı seçin.

3. **Ayrıntılar** sekmesinde:

    - Tüm gerekli alanları doldur.

    - Bağıntı KIMLIĞINIZI veya ExpressRoute KIMLIĞINIZI, belirtilen belirli alanlara yapıştırın. Bunlar için belirli bir alan görmüyorsanız, **sorun hakkındaki ayrıntıları sağlamak** altındaki metin kutusuna yapıştırabilirsiniz.

    - Kopyaladığınız hataların Özeti dahil olmak üzere herhangi bir hata ayrıntılarını, **sorun hakkındaki ayrıntıları sağlayan** metin kutusuna yapıştırın.

4. SR 'nizi oluşturmak için gözden geçirin ve **Oluştur** ' u seçin.
