---
title: Kaynak Yöneticisi için Azure Defender-avantajlar ve Özellikler
description: Kaynak Yöneticisi için Azure Defender 'ın avantajları ve özellikleri hakkında bilgi edinin
author: memildin
ms.author: memildin
ms.date: 12/07/2020
ms.topic: overview
ms.service: security-center
manager: rkarlin
ms.openlocfilehash: 922189909da21aac65d1e2e898df58598868b929
ms.sourcegitcommit: e559daa1f7115d703bfa1b87da1cf267bf6ae9e8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/17/2021
ms.locfileid: "100590469"
---
# <a name="introduction-to-azure-defender-for-resource-manager"></a>Kaynak Yöneticisi için Azure Defender 'a giriş

[Azure Resource Manager](../azure-resource-manager/management/overview.md) , Azure dağıtım ve yönetim hizmetidir. Azure hesabınızda kaynak oluşturma, güncelleştirme ve silme işlemlerini gerçekleştirmenizi sağlayan bir yönetim katmanı sunar. Dağıtım sonrasında kaynaklarınızın güvenliğini sağlamak ve onları düzenlemek için erişim denetimleri, kilitler ve etiketler gibi yönetim özelliklerini kullanabilirsiniz.

Bulut yönetimi katmanı, tüm bulut kaynaklarınıza bağlı olan önemli bir hizmettir. Bu nedenle, saldırganlar için de potansiyel bir hedeftir. Sonuç olarak, güvenlik işlemleri ekiplerinin kaynak yönetimi katmanını yakından izlemesini öneririz. 

Kaynak Yöneticisi için Azure Defender, kuruluşunuzdaki kaynak yönetimi işlemlerini otomatik olarak izleyerek Azure portal, Azure REST API 'Leri, Azure CLı veya diğer Azure programlı istemcileri aracılığıyla gerçekleştirilirler. Azure Defender, tehditleri tespit etmek ve şüpheli etkinliklerle ilgili sizi uyarmak için gelişmiş güvenlik analizlerini çalıştırır.

## <a name="availability"></a>Kullanılabilirlik

|Görünüş|Ayrıntılar|
|----|:----|
|Yayın durumu:|Önizleme<br>[!INCLUDE [Legalese](../../includes/security-center-preview-legal-text.md)] |
|Fiyat|**Kaynak Yöneticisi Için Azure Defender** , [fiyatlandırma sayfasında](security-center-pricing.md) gösterildiği gibi faturalandırılır|
|Larının|![Yes](./media/icons/yes-icon.png) Ticari bulutlar<br>![No](./media/icons/no-icon.png) Ulusal/Sogeign (US Gov, Çin gov, diğer gov)|
|||

## <a name="what-are-the-benefits-of-azure-defender-for-resource-manager"></a>Kaynak Yöneticisi için Azure Defender 'ın avantajları nelerdir?

Kaynak Yöneticisi için Azure Defender, aşağıdakiler de dahil olmak üzere sorunları önler:

- Şüpheli IP adreslerinden gerçekleştirilen işlemler, VM uzantılarında çalışan kötü amaçlı yazılımdan koruma ve şüpheli betikleri devre dışı bırakarak **şüpheli kaynak yönetimi işlemleri**
- Mikro patlama veya PowerZure gibi **yararlanılması araç takımları kullanımı**
- Azure Yönetim katmanından Azure kaynakları veri düzlemine **yan yana taşıma**

:::image type="content" source="media/defender-for-resource-manager-introduction/consistent-management-layer-with-defender.png" alt-text="Azure Resource Manager genel bakış Diyagramı":::

Kaynak Yöneticisi için Azure Defender tarafından sunulan uyarıların tam listesi, [Uyarılar başvurusu sayfasında](alerts-reference.md#alerts-resourcemanager)bulunur.


 ## <a name="how-to-investigate-alerts-from-azure-defender-for-resource-manager"></a>Kaynak Yöneticisi için Azure Defender 'daki uyarıları araştırma

Kaynak Yöneticisi için Azure Defender 'daki güvenlik uyarıları, Azure Resource Manager işlemlerini izleyerek algılanan tehditleri temel alır. Azure Defender, Azure 'da abonelik düzeyindeki olaylar hakkında Öngörüler sağlayan bir platform günlüğü olan Azure Resource Manager iç günlük kaynaklarını ve Azure etkinlik günlüğü 'nü kullanır.

[Azure etkinlik günlüğü](../azure-monitor/essentials/activity-log.md)hakkında daha fazla bilgi edinin.

Kaynak Yöneticisi için Azure Defender 'daki güvenlik uyarılarını araştırmak için:

1. Azure etkinlik günlüğü 'nü açın.

    :::image type="content" source="media/defender-for-resource-manager-introduction/opening-azure-activity-log.png" alt-text="Azure etkinlik günlüğü 'nü açma":::

1. Olayları şu şekilde filtreleyin:
    - Uyarıda belirtilen abonelik
    - Algılanan etkinliğin zaman dilimi
    - İlgili Kullanıcı hesabı (ilgiliyse)

1. Şüpheli etkinlikleri arayın.

> [!TIP]
> Daha iyi, daha zengin bir araştırma deneyimi sağlamak için Azure etkinlik günlüklerinizi Azure [etkinlik günlüğünden veri bağlama](../sentinel/connect-azure-activity.md)bölümünde açıklandığı gibi Azure 'un Sentinel 'e akışı yapın.



## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, Kaynak Yöneticisi için Azure Defender hakkında bilgi edindiniz. İlgili malzemeler için aşağıdaki makaleye bakın: 

- Güvenlik uyarıları Güvenlik Merkezi tarafından üretilebilir veya Güvenlik Merkezi tarafından farklı güvenlik ürünlerinden alınabilir. Bu uyarıların tümünü Azure Sentinel 'e, herhangi bir üçüncü taraf SıEM 'e veya herhangi bir harici araca aktarmak için [uyarıları BIR SıEM 'ye aktarma](continuous-export.md)konusundaki yönergeleri izleyin.

- > [!div class="nextstepaction"]
    > [Azure Defender’ı etkinleştirme](security-center-pricing.md#enable-azure-defender)