---
title: Azure Güvenlik Merkezi 'ndeki önerileri düzeltin | Microsoft Docs
description: Bu makalede, Azure Güvenlik Merkezi 'nde kaynaklarınızın korunması ve güvenlik ilkelerini karşılamak için önerilerin nasıl yanıtlanacağı açıklanır.
author: memildin
manager: rkarlin
ms.service: security-center
ms.topic: conceptual
ms.date: 03/04/2021
ms.author: memildin
ms.openlocfilehash: f382646c889d004738064cae2d09fd66d897b110
ms.sourcegitcommit: 5bbc00673bd5b86b1ab2b7a31a4b4b066087e8ed
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/07/2021
ms.locfileid: "102438276"
---
# <a name="remediate-recommendations-in-azure-security-center"></a>Azure Güvenlik Merkezi'nde düzeltme önerileri

Öneriler, kaynaklarınızın daha iyi güvenliğini sağlamaya yönelik öneriler sağlar. Öneri bölümünde belirtilen düzeltme adımlarını izleyerek bir öneri uygulıyordu.

## <a name="remediation-steps"></a>Düzeltme adımları <a name="remediation-steps"></a>

Tüm önerileri inceledikten sonra, hangisi önce düzeltileceğine karar verin. Güvenli puanınızı artırmak için en yüksek olabilecek güvenlik denetimlerini önceliklendirmenizi öneririz.

1. Listeden bir öneri seçin.

1. **Düzeltme adımları** bölümündeki yönergeleri izleyin. Her öneri kendi yönerge kümesine sahiptir. Aşağıdaki ekran görüntüsünde, uygulamaları yalnızca HTTPS üzerinden trafiğe izin verecek şekilde yapılandırmaya yönelik düzeltme adımları gösterilmektedir.

    :::image type="content" source="./media/security-center-remediate-recommendations/security-center-remediate-recommendation.png" alt-text="Öneri için el ile düzeltme adımları" lightbox="./media/security-center-remediate-recommendations/security-center-remediate-recommendation.png":::

1. Tamamlandıktan sonra, sorunun çözümlenip çözümlenmediğini bildiren bir bildirim görüntülenir.

## <a name="quick-fix-remediation"></a>Hızlı düzeltme düzeltme

Düzeltmeyi basitleştirmek ve ortamınızın güvenliğini artırmak (ve güvenli puanınızı artırmak) için birçok öneri, hızlı bir düzeltme seçeneği içerir.

Hızlı düzeltme, birden fazla kaynak üzerinde bir öneriyi hızlıca düzeltmenize yardımcı olur.

> [!TIP]
> Hızlı düzelme çözümleri yalnızca belirli öneriler için kullanılabilir. Kullanılabilir bir hızlı düzelme sahip önerileri bulmak için, öneriler listesi için **Yanıt eylemleri** filtresini kullanın:
> 
> :::image type="content" source="media/security-center-remediate-recommendations/quick-fix-filter.png" alt-text="Hızlı düzelme seçeneğine sahip önerileri bulmak için öneriler listesinin üzerindeki filtreleri kullanın":::

Hızlı bir çözüm uygulamak için:

1. **Hızlı düzelme** sahip öneriler listesinden! etiket, bir öneri seçin.

    [![Hızlı düzelme 'yi seçin!](media/security-center-remediate-recommendations/security-center-quick-fix-select.png)](media/security-center-remediate-recommendations/security-center-quick-fix-select.png#lightbox)

1. **Sağlıksız kaynaklar** sekmesinden, öneriyi uygulamak istediğiniz kaynakları seçin ve **Düzelt**' i seçin.

    > [!NOTE]
    > Listelenen bazı kaynaklar devre dışı bırakılabilir, çünkü bunları değiştirmek için uygun izinlere sahip değilsiniz.

1. Onay kutusunda düzeltme ayrıntılarını ve etkilerini okuyun.

    ![Hızlı çözüm](./media/security-center-remediate-recommendations/security-center-quick-fix-view.png)

    > [!NOTE]
    > Bu etkiler, **Düzelt**' i tıkladıktan sonra açılan **kaynakları Düzelt** penceresindeki gri kutusunda listelenir. Hızlı düzeltme düzeltmesine devam eden değişikliklerin ne olduğunu listeler.

1. Gerekirse ilgili parametreleri ekleyin ve düzeltmeyi onaylayın.

    > [!NOTE]
    > **Sağlıklı kaynaklar** sekmesindeki kaynakları görmek için düzeltme tamamlandıktan sonra işlem birkaç dakika sürebilir. Düzeltme eylemlerini görüntülemek için [etkinlik günlüğünü](#activity-log)kontrol edin.

1. Tamamlandıktan sonra, düzeltme başarılı olursa bildirim görüntülenir.

## <a name="quick-fix-remediation-logging-in-the-activity-log"></a>Etkinlik günlüğünde düzeltme günlüğü hızlı düzeltme <a name="activity-log"></a>

Düzeltme işlemi, kaynak üzerinde yapılandırmayı uygulamak için bir şablon dağıtımı veya REST PATCH API çağrısı kullanır. Bu işlemler [Azure etkinlik günlüğünde](../azure-resource-manager/management/view-activity-logs.md)günlüğe kaydedilir.


## <a name="next-steps"></a>Sonraki adımlar

Bu belgede, güvenlik merkezi 'nde önerilerin nasıl düzeltileceği gösterildi. Güvenlik Merkezi hakkında daha fazla bilgi edinmek için aşağıdaki sayfalara bakın:

* [Azure Güvenlik Merkezi 'nde güvenlik Ilkelerini ayarlama](tutorial-security-policy.md) -Azure abonelikleriniz ve kaynak gruplarınız için güvenlik ilkelerini yapılandırma hakkında bilgi edinin
* [Güvenlik ilkeleri, girişimler ve öneriler nelerdir?](security-policy-concept.md)