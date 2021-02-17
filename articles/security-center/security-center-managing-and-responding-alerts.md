---
title: Azure Güvenlik Merkezi'nde güvenlik uyarılarını yönetme | Microsoft Docs
description: Bu belge, güvenlik uyarılarını yönetmek ve yanıtlamak için Azure Güvenlik Merkezi işlevlerini kullanmanıza yardımcı olur.
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.assetid: b88a8df7-6979-479b-8039-04da1b8737a7
ms.service: security-center
ms.topic: how-to
ms.devlang: na
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/17/2021
ms.author: memildin
ms.openlocfilehash: a47ece762f2df3fa18cf2b79d338c28d4069c4ad
ms.sourcegitcommit: 58ff80474cd8b3b30b0e29be78b8bf559ab0caa1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/17/2021
ms.locfileid: "100633489"
---
# <a name="manage-and-respond-to-security-alerts-in-azure-security-center"></a>Azure Güvenlik Merkezi 'nde güvenlik uyarılarını yönetme ve yanıtlama

Bu konu başlığı altında, güvenlik merkezi 'nin uyarılarını görüntüleme ve işleme ve kaynaklarınızı koruma işlemlerinin nasıl yapılacağı gösterilmektedir.

Güvenlik uyarılarını tetikleyen gelişmiş algılamalar yalnızca Azure Defender ile kullanılabilir. Ücretsiz deneme sürümü mevcuttur. Yükseltmek için bkz. [Azure Defender 'ı etkinleştirme](security-center-pricing.md#enable-azure-defender).

## <a name="what-are-security-alerts"></a>Güvenlik uyarıları nedir?
Güvenlik Merkezi, gerçek tehditleri algılamak ve hatalı pozitif sonuçları azaltmak için Azure kaynaklarınızdan, ağınızdan ve güvenlik duvarı ve uç nokta koruma çözümleri gibi bağlı iş ortağı çözümlerinden günlük verilerini otomatik olarak toplar, çözümler ve tümleştirir. Öncelikli güvenlik uyarıları listesi, sorunu hızlıca araştırmanız gereken bilgiler ve saldırıyı düzeltme hakkındaki önerilerle birlikte Güvenlik Merkezi'nde gösterilir.

Farklı Uyarı türleri hakkında bilgi edinmek için bkz. [güvenlik uyarıları-bir başvuru kılavuzu](alerts-reference.md).

Güvenlik Merkezi 'nin nasıl uyarı üretdiğine ilişkin genel bakış için bkz. [Azure Güvenlik Merkezi 'nin tehditleri nasıl algıladığı ve yanıt verdiği](security-center-alerts-overview.md).


## <a name="manage-your-security-alerts"></a>Güvenlik uyarılarınızı yönetme

1. Güvenlik Merkezi 'nin Genel Bakış sayfasında, sayfanın üst kısmındaki **güvenlik uyarıları** kutucuğunu veya kenar çubuğundan bağlantıyı seçin.

    :::image type="content" source="media/security-center-managing-and-responding-alerts/overview-page-alerts-links.png" alt-text="Azure Güvenlik Merkezi 'nin Genel Bakış sayfasından güvenlik uyarıları sayfasına alma":::

    Güvenlik uyarıları sayfası açılır.

    :::image type="content" source="media/security-center-managing-and-responding-alerts/alerts-page.png" alt-text="Azure Güvenlik Merkezi 'nin güvenlik uyarıları listesi":::

1. Uyarılar listesini filtrelemek için ilgili filtrelerin birini seçin. İsteğe bağlı olarak **Filtre Ekle** seçeneği ile daha fazla filtre ekleyebilirsiniz.

    :::image type="content" source="./media/security-center-managing-and-responding-alerts/alerts-adding-filters-small.png" alt-text="Uyarılar görünümüne filtreler ekleme" lightbox="./media/security-center-managing-and-responding-alerts/alerts-adding-filters-large.png":::

    Liste, seçtiğiniz filtreleme seçeneklerine göre güncelleştirilir. Filtreleme çok faydalı olabilir. Örneğin, sistemde olası bir ihlali araştırdığınız için son 24 saatte oluşan güvenlik uyarılarını ele almak isteyebilirsiniz.


## <a name="respond-to-security-alerts"></a>Güvenlik uyarılarını yanıtlama

1. **Güvenlik uyarıları** listesinden bir uyarı seçin. Bir yan bölme açılır ve uyarının açıklaması ve etkilenen tüm kaynaklar gösterilir. 

    :::image type="content" source="./media/security-center-managing-and-responding-alerts/alerts-details-pane.png" alt-text="Güvenlik uyarısının mini Ayrıntılar görünümü":::

    > [!TIP]
    > Bu yan bölme açıkken, klavyenizdeki yukarı ve aşağı oklarıyla uyarı listesini hızlıca inceleyebilirsiniz.

1. Daha fazla bilgi için **tam ayrıntıları görüntüle**' yi seçin.

    Güvenlik Uyarısı sayfasının sol bölmesi, güvenlik uyarısıyla ilgili üst düzey bilgileri gösterir: Başlık, önem derecesi, durum, etkinlik süresi, şüpheli etkinliğin açıklaması ve etkilenen kaynak. Etkilenen kaynağın yanı sıra kaynakla ilgili Azure etiketlerdir. Uyarıyı araştırırken kaynağın kuruluş bağlamını anlamak için bunları kullanın.

    Sağ bölmede, sorunu araştırmanıza yardımcı olmak üzere uyarının daha ayrıntılı ayrıntılarını içeren **Uyarı ayrıntıları** sekmesi bulunur: IP adresleri, dosyalar, süreçler ve daha fazlası.
     
    ![Güvenlik uyarıları hakkında ne yapacaklarıyla ilgili öneriler](./media/security-center-managing-and-responding-alerts/security-center-alert-remediate.png)

    Sağ bölmedeki **eylem al** sekmesi de bulunur. Güvenlik uyarısıyla ilgili daha fazla eylem gerçekleştirmek için bu sekmeyi kullanın. Şöyle eylemler:
    - *Tehdidi azaltma* -bu güvenlik uyarısı için el ile düzeltme adımları sağlar
    - *Gelecekteki saldırıları önleme* -saldırı yüzeyini azaltmaya, güvenlik duruşunu artırmaya ve bu nedenle gelecekteki saldırıları önlemeye yardımcı olmak için güvenlik önerileri sağlar
    - *Otomatik yanıtı Tetikle* -bu güvenlik uyarısı için bir mantıksal uygulamayı bir yanıt olarak tetikleme seçeneği sağlar
    - *Benzer Uyarıları Gizle* -uyarı kuruluşunuza uygun değilse, benzer özelliklerle gelecekteki uyarıları gösterme seçeneği sağlar

    ![Eylem al sekmesi](./media/security-center-managing-and-responding-alerts/alert-take-action.png)




## <a name="see-also"></a>Ayrıca bkz.

Bu belgede, güvenlik uyarılarını görüntülemeyi öğrendiniz. İlgili malzemeler için aşağıdaki sayfalara bakın:

- [Uyarı gizleme kurallarını yapılandırma](alerts-suppression-rules.md)
- [Güvenlik Merkezi tetikleyicilerine yönelik yanıtları otomatikleştirin](workflow-automation.md)
- [Güvenlik uyarıları - başvuru kılavuzu](alerts-reference.md)
