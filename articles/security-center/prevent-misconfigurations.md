---
title: Azure Güvenlik Merkezi 'Nde yanlış yapılandırmaları önleme
description: Güvenlik Merkezi 'nin ' zorla ' ve ' Reddet ' seçeneklerini öneriler Ayrıntılar sayfalarında nasıl kullanacağınızı öğrenin
author: memildin
manager: rkarlin
ms.service: security-center
ms.topic: how-to
ms.date: 02/04/2021
ms.author: memildin
ms.openlocfilehash: 5d172a73674195e7f64f5ef02322e2bd2d6314df
ms.sourcegitcommit: 5bbc00673bd5b86b1ab2b7a31a4b4b066087e8ed
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/07/2021
ms.locfileid: "102439536"
---
# <a name="prevent-misconfigurations-with-enforcedeny-recommendations"></a>Zorlama/Reddetme önerileriyle yanlış yapılandırmaları önleme

Güvenlik yapılandırması hataları, güvenlik olaylarının önemli bir nedendir. Güvenlik Merkezi artık, belirli önerilere göre yeni kaynakların yanlış yapılandırmalarını *önlemeye* yardımcı olur. 

Bu özellik, iş yüklerinizi güvende tutmaya ve güvenli puanınızı sabitetmenize yardımcı olabilir.

Belirli bir öneriye göre güvenli bir yapılandırma uygulamak, iki modda sunulur:

- Azure Ilkesinin **reddetme** efektini kullanarak, sağlıksız kaynakların oluşturulmasını durdurabilirsiniz
- **Uygula** seçeneğini kullanarak Azure Ilkesinin **Deployifnotexist** efektinin avantajlarından yararlanabilir ve oluşturma sırasında uyumlu olmayan kaynakları otomatik olarak düzeltebilirsiniz

Bu, seçilen güvenlik önerileri için kaynak Ayrıntıları sayfasının en üstünde bulunabilir (bkz. [reddetme/zorlama seçenekleriyle öneriler](#recommendations-with-denyenforce-options)).

## <a name="prevent-resource-creation"></a>Kaynak oluşturmayı engelle

1. Yeni kaynaklarınızın karşılaması gereken öneriyi açın ve sayfanın üst kısmındaki **Reddet** düğmesini seçin.

    :::image type="content" source="./media/security-center-remediate-recommendations/recommendation-deny-button.png" alt-text="Reddet düğmesinin vurgulandığı öneri sayfası":::

    Kapsam seçeneklerini listelemek için yapılandırma bölmesi açılır. 

1. İlgili aboneliği veya yönetim grubunu seçerek kapsamı ayarlayın.

    > [!TIP]
    > Tek bir aboneliği değiştirmek için satırın sonundaki üç noktayı kullanabilir veya birden fazla abonelik veya Grup seçmek için onay kutularını kullanabilir, sonra da **Reddet olarak değiştir**' i seçin.

    :::image type="content" source="./media/security-center-remediate-recommendations/recommendation-prevent-resource-creation.png" alt-text="Azure Ilkesi reddetme kapsamını ayarlama":::


## <a name="enforce-a-secure-configuration"></a>Güvenli bir yapılandırma zorla

1. Yeni kaynaklar bunu karşılamadığı takdirde, bir şablon dağıtımı dağıtacağınız öneriyi açın ve sayfanın üst kısmındaki **zorla** düğmesini seçin.

    :::image type="content" source="./media/security-center-remediate-recommendations/recommendation-enforce-button.png" alt-text="Zorunlu Düğme vurgulanmış öneri sayfası":::

    Yapılandırma bölmesi tüm ilke yapılandırma seçenekleriyle açılır. 

    :::image type="content" source="./media/security-center-remediate-recommendations/recommendation-enforce-config.png" alt-text="Yapılandırma seçeneklerini zorunlu kıl":::

1. Kapsam, atama adı ve diğer ilgili seçenekleri ayarlayın.

1. **Gözden geçir ve oluştur**’u seçin.

## <a name="recommendations-with-denyenforce-options"></a>Reddetme/zorlama seçenekleriyle ilgili öneriler

Bu öneriler, **reddetme** seçeneğiyle birlikte kullanılabilir:

[!INCLUDE [azure-security-center-recommendations-deny](../../includes/asc/recommendations-with-deny.md)]

Bu öneriler, **zorla** seçeneğiyle birlikte kullanılabilir:

- SQL Server üzerinde denetim etkinleştirilmelidir
- Sanal makineler için Azure Backup etkinleştirilmelidir
- SQL Server 'larınızdaki SQL için Azure Defender etkinleştirilmelidir
- Azure Stream Analytics tanılama günlükleri etkinleştirilmelidir
- Batch hesaplarındaki tanılama günlükleri etkinleştirilmelidir
- Data Lake Analytics tanılama günlükleri etkinleştirilmelidir
- Olay Hub 'ındaki tanılama günlükleri etkinleştirilmelidir
- Key Vault tanılama günlükleri etkinleştirilmelidir
- Logic Apps tanılama günlükleri etkinleştirilmelidir
- Arama hizmetlerindeki tanılama günlükleri etkinleştirilmelidir
- Service Bus tanılama günlükleri etkinleştirilmelidir
