---
title: Azure Güvenlik Merkezi 'Nde yanlış yapılandırmaları önleme
description: Güvenlik Merkezi 'nin ' zorla ' ve ' Reddet ' seçeneklerini öneriler Ayrıntılar sayfalarında nasıl kullanacağınızı öğrenin
services: security-center
author: memildin
manager: rkarlin
ms.service: security-center
ms.topic: how-to
ms.date: 09/08/2020
ms.author: memildin
ms.openlocfilehash: ceafbe1fd1682fc5e92ab3048ed09866fb9ebbdf
ms.sourcegitcommit: d0541eccc35549db6381fa762cd17bc8e72b3423
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/09/2020
ms.locfileid: "89570495"
---
# <a name="prevent-misconfigurations-with-enforcedeny-recommendations"></a>Zorla/reddetme önerilerini kullanarak yanlış yapılandırma önleme

Güvenlik yapılandırması hataları, güvenlik olaylarının önemli bir nedendir. Güvenlik Merkezi artık, belirli önerilere göre yeni kaynakların yanlış yapılandırmalarını *önlemeye* yardımcı olur. 

Bu özellik, iş yüklerinizi güvende tutmaya ve güvenli puanınızı sabitetmenize yardımcı olabilir.

Belirli bir öneriye göre güvenli bir yapılandırma uygulamak, iki modda sunulur:

- Azure Ilkesinin **reddetme** efektini kullanarak, sağlıksız kaynakların oluşturulmasını durdurabilirsiniz

- **Uygula** seçeneğini kullanarak Azure Ilkesinin **Deployifnotexist** efektinin avantajlarından yararlanabilir ve oluşturma sırasında uyumlu olmayan kaynakları otomatik olarak düzeltebilirsiniz
 
Bu, seçilen güvenlik önerileri için kullanılabilir ve kaynak Ayrıntıları sayfasının en üstünde bulunabilir.

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

1. **Gözden geçir + oluştur**’u seçin.

## <a name="recommendations-with-denyenforce-options"></a>Reddetme/zorlama seçenekleriyle ilgili öneriler

Bu öneriler, **reddetme** seçeneğiyle birlikte kullanılabilir:

- Sanal makinelerin yeni Azure Resource Manager kaynaklara geçirilmesi gerekir
- Depolama hesaplarının yeni Azure Resource Manager kaynaklarına geçirilmesi gerekir
- RootManageSharedAccessKey hariç tüm yetkilendirme kuralları, Olay Hub 'ı ad alanından kaldırılmalıdır
- RootManageSharedAccessKey hariç tüm yetkilendirme kuralları Service Bus ad alanından kaldırılmalıdır
- Depolama hesaplarına güvenli aktarım etkinleştirilmelidir
- Yalnızca Redis Cache güvenli bağlantılar etkinleştirilmelidir
- Otomasyon hesabı değişkenleri şifrelenmelidir
- Service Fabric kümeler yalnızca istemci kimlik doğrulaması için Azure Active Directory kullanmalıdır
- Service Fabric kümelerinde ClusterProtectionLevel özelliği EncryptAndSign olarak ayarlanmalıdır
- Depolama hesaplarına Kısıtlanmamış ağ erişimini denetleme


Bu öneriler, **zorla** seçeneğiyle birlikte kullanılabilir:

- Logic Apps tanılama günlükleri etkinleştirilmelidir
- Data Lake Analytics tanılama günlükleri etkinleştirilmelidir
- IoT Hub tanılama günlükleri etkinleştirilmelidir
- Batch hesaplarındaki tanılama günlükleri etkinleştirilmelidir
- Azure Stream Analytics tanılama günlükleri etkinleştirilmelidir
- Service Bus tanılama günlükleri etkinleştirilmelidir
- Arama hizmetlerindeki tanılama günlükleri etkinleştirilmelidir
- Olay Hub 'ındaki tanılama günlükleri etkinleştirilmelidir
- Sanal makine ölçek kümelerindeki tanılama günlükleri etkinleştirilmelidir
- Key Vault tanılama günlükleri etkinleştirilmelidir
- SQL Server üzerinde denetim etkinleştirilmelidir
- Gelişmiş veri güvenliği SQL sunucularınızda etkinleştirilmelidir



