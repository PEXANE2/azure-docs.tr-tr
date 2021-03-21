---
title: Azure Güvenlik Merkezi'nde güvenlik önerileri
description: Bu belge, Azure Güvenlik Merkezi 'ndeki önerilerin Azure kaynaklarınızı korumanıza ve güvenlik ilkeleriyle uyumlu kalmanıza nasıl yardımcı olduğunu gösterir.
author: memildin
manager: rkarlin
ms.service: security-center
ms.topic: conceptual
ms.date: 01/24/2021
ms.author: memildin
ms.openlocfilehash: 57760443746e111750e74ef55fc18729f6ba32c4
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/20/2021
ms.locfileid: "102100347"
---
# <a name="review-your-security-recommendations"></a>Güvenlik önerilerinizi gözden geçirin

Bu konu başlığı altında, Azure Güvenlik Merkezi 'nde Azure kaynaklarınızı korumanıza yardımcı olan önerilerin nasıl görüntüleneceği ve anlayabileceği açıklanmaktadır.

## <a name="monitor-recommendations"></a>İzleme önerileri <a name="monitor-recommendations"></a>

Güvenlik Merkezi, olası güvenlik açıklarını belirlemek için kaynaklarınızın güvenlik durumunu analiz eder. 

1. Ortamınız için geçerli olan önerileri görmek için Güvenlik Merkezi 'nin menüsünde **öneriler** sayfasını açın. Öneriler, güvenlik denetimleri halinde gruplandırılır.

    :::image type="content" source="./media/security-center-recommendations/view-recommendations.png" alt-text="Güvenlik denetimine göre gruplanmış öneriler" lightbox="./media/security-center-recommendations/view-recommendations.png":::

1. Kaynak türüne, önem derecesine, ortama veya sizin için önemli olan diğer ölçütlere özgü önerileri bulmak için, öneriler listesinin üzerindeki isteğe bağlı filtreleri kullanın.

    :::image type="content" source="media/security-center-recommendations/recommendation-list-filters.png" alt-text="Azure Güvenlik Merkezi önerileri listesini iyileştirme filtreleri":::

1. Bir denetimi genişletin ve öneri ayrıntıları sayfasını görüntülemek için belirli bir öneri seçin.

    :::image type="content" source="./media/security-center-recommendations/recommendation-details-page.png" alt-text="Öneri ayrıntıları sayfası." lightbox="./media/security-center-recommendations/recommendation-details-page.png":::

    Sayfa şunları içerir:

    1. Desteklenen öneriler için, üstteki araç çubuğunda aşağıdaki düğmelerin herhangi biri veya tümü gösterilir:
        - **Zorla** ve **Reddet** (bkz. [zorlama/reddetme önerilerini kullanarak yapılandırmaları önleme](prevent-misconfigurations.md))
        - **İlke tanımını** , temel alınan ilke Için doğrudan Azure ilke girişine gidecek şekilde görüntüle
    1. **Önem derecesi göstergesi**
    1. **Yenilik aralığı** (ilgili yerlerde)
    1. **Muaf tutulan kaynak sayısı** bu öneri için muafiyet varsa, bu, muaf tutulan kaynak sayısını gösterir
    1. **Açıklama** -sorunun kısa bir açıklaması
    1. **Düzeltme adımları** -etkilenen kaynaklardaki güvenlik sorununu düzeltmek için gereken el ile adımların bir açıklaması. ' Hızlı düzeltme ' ile ilgili öneriler için, kaynaklarınıza önerilen düzeltmeyi uygulamadan önce **Düzeltme mantığını görüntüle** ' yi seçebilirsiniz. 
    1. **Etkilenen kaynaklar** -kaynaklarınız sekmeler halinde gruplandırılır:
        - **Sağlıklı kaynaklar** – etkilenmeyen veya üzerinde sorunu zaten seçtiğiniz ilgili kaynaklar.
        - **Sağlıksız kaynaklar** – hala belirtilen sorundan etkilenen kaynaklar.
        - **Geçerli olmayan kaynaklar** – önerinin kesin yanıt veremediğinde kaynaklar. Uygulanamaz sekmesi her bir kaynağın nedenlerini de içerir. 

            :::image type="content" source="./media/security-center-recommendations/recommendations-not-applicable-reasons.png" alt-text="Nedenlerden dolayı geçerli olmayan kaynaklar değildir.":::
    1. Öneriyi düzeltecek veya bir mantıksal uygulamayı tetikleyen eylem düğmeleri.

## <a name="preview-recommendations"></a>Önizleme önerileri

**Önizleme** olarak işaretlenen öneriler, güvenli puanınızın hesaplamalarına dahil değildir.

Önizleme dönemi sona erdiğinde puanınızın altına katkıda bulunduklarında, bunlar mümkün olduğunda yine de düzeltilmelidir.

Önizleme önerisi örneği:

:::image type="content" source="./media/secure-score-security-controls/example-of-preview-recommendation.png" alt-text="Önizleme bayrağıyla ilgili öneri":::
 
## <a name="next-steps"></a>Sonraki adımlar

Bu belgede, güvenlik merkezi 'nde güvenlik önerilerine sunulmuştur. İlgili bilgiler için:

- [Önerileri düzeltin](security-center-remediate-recommendations.md)-Azure abonelikleriniz ve kaynak gruplarınız için güvenlik ilkelerini yapılandırma hakkında bilgi edinin.
- [Zorla/reddetme önerilerini kullanarak yanlış yapılandırma yapılmasını engelleyin](prevent-misconfigurations.md).
- [Güvenlik Merkezi tetikleyicilerine yönelik yanıtları otomatikleştirin](workflow-automation.md)--önerilerin yanıtlarını otomatikleştirin
- [Kaynağı öneriden muaf tutma](exempt-resource.md)
- [Güvenlik önerileri - başvuru kılavuzu](recommendations-reference.md)