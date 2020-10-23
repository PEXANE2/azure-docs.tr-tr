---
title: Azure Güvenlik Merkezi'nde güvenlik önerileri
description: Bu belge, Azure Güvenlik Merkezi 'ndeki önerilerin Azure kaynaklarınızı korumanıza ve güvenlik ilkeleriyle uyumlu kalmanıza nasıl yardımcı olduğunu gösterir.
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.assetid: 86c50c9f-eb6b-4d97-acb3-6d599c06133e
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/22/2020
ms.author: memildin
ms.openlocfilehash: f3fb31dc7db4808db83f33abdf14179265d147f9
ms.sourcegitcommit: 6906980890a8321dec78dd174e6a7eb5f5fcc029
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/22/2020
ms.locfileid: "92425192"
---
# <a name="security-recommendations-in-azure-security-center"></a>Azure Güvenlik Merkezi'nde güvenlik önerileri 
Bu konu başlığı altında, Azure Güvenlik Merkezi 'nde Azure kaynaklarınızı korumanıza yardımcı olan önerilerin nasıl görüntüleneceği ve anlayabileceği açıklanmaktadır.


## <a name="what-are-security-recommendations"></a>Güvenlik önerileri nelerdir?

Önerileriniz kaynaklarınızın güvenliğini sağlamak için yapmanız gereken eylemlerdir.

Güvenlik Merkezi, olası güvenlik açıklarını belirlemek için Azure kaynaklarınızın güvenlik durumunu düzenli olarak analiz eder. Daha sonra bu güvenlik açıklarını nasıl düzeltebileceğiniz konusunda öneriler sağlar.

Her öneri şunları sağlar:

- Sorunun kısa bir açıklaması
- Öneriyi uygulamak için gerçekleştirilecek düzeltme adımları
- Etkilenen kaynaklar

## <a name="monitor-recommendations"></a>İzleme önerileri <a name="monitor-recommendations"></a>

Güvenlik Merkezi, olası güvenlik açıklarını belirlemek için kaynaklarınızın güvenlik durumunu analiz eder. 

1. Ortamınız için geçerli olan önerileri görmek için Güvenlik Merkezi 'nin menüsünde **öneriler** sayfasını açın. Öneriler, güvenlik denetimleri halinde gruplandırılır.

      ![Güvenlik denetimine göre gruplanmış öneriler](./media/security-center-recommendations/view-recommendations.png)

1. Bir denetimi genişletin ve öneri ayrıntıları sayfasını görüntülemek için belirli bir öneri seçin.

    :::image type="content" source="./media/security-center-recommendations/recommendation-details-page.png" alt-text="Öneri ayrıntıları sayfası." lightbox="./media/security-center-recommendations/recommendation-details-page.png":::

    Sayfa şunları içerir:

    1. Desteklenen önerilerden düğmeleri **zorla** ve **Reddet** (bkz. [zorla/reddetme önerilerini kullanarak yapılandırmaları önleme](prevent-misconfigurations.md))
    1. **Önem derecesi göstergesi**
    1. **Yenilik aralığı**  (ilgili yerlerde) 
    1. **Açıklama** -sorunun kısa bir açıklaması
    1. **Düzeltme adımları** -etkilenen kaynaklardaki güvenlik sorununu düzeltmek için gereken el ile adımların bir açıklaması. ' Hızlı düzeltme ' ile ilgili öneriler için, kaynaklarınıza önerilen düzeltmeyi uygulamadan önce **Düzeltme mantığını görüntüle** ' yi seçebilirsiniz. 
    1. **Etkilenen kaynaklar** -kaynaklarınız sekmeler halinde gruplandırılır:
        - **Sağlıklı kaynaklar** – etkilenmeyen veya üzerinde sorunu zaten seçtiğiniz ilgili kaynaklar.
        - **Sağlıksız kaynaklar** – hala belirtilen sorundan etkilenen kaynaklar.
        - **Geçerli olmayan kaynaklar** – önerinin kesin yanıt veremediğinde kaynaklar. Uygulanamaz sekmesi her bir kaynağın nedenlerini de içerir. 

            :::image type="content" source="./media/security-center-recommendations/recommendations-not-applicable-reasons.png" alt-text="Öneri ayrıntıları sayfası.":::
    1. Öneriyi düzeltecek veya bir mantıksal uygulamayı tetikleyen eylem düğmeleri.

## <a name="preview-recommendations"></a>Önizleme önerileri

**Önizleme** olarak işaretlenen öneriler, güvenli puanınızın hesaplamalarına dahil değildir.

Önizleme dönemi sona erdiğinde puanınızın altına katkıda bulunduklarında, bunlar mümkün olduğunda yine de düzeltilmelidir.

Önizleme önerisi örneği:

:::image type="content" source="./media/secure-score-security-controls/example-of-preview-recommendation.png" alt-text="Öneri ayrıntıları sayfası.":::
 
## <a name="next-steps"></a>Sonraki adımlar

Bu belgede, güvenlik merkezi 'nde güvenlik önerilerine sunulmuştur. İlgili bilgiler için:

- [Önerileri](security-center-remediate-recommendations.md) düzeltme — Azure abonelikleriniz ve kaynak gruplarınız için güvenlik ilkelerini yapılandırma hakkında bilgi edinin.
- [Zorla/reddetme önerilerini kullanarak yanlış yapılandırma yapılmasını engelleyin](prevent-misconfigurations.md).
- [Önerilere verilen yanıtları otomatikleştirme](workflow-automation.md)
- [Kaynağı öneriden muaf tutma](exempt-resource.md)
- [Güvenlik önerileri - başvuru kılavuzu](recommendations-reference.md)