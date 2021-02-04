---
title: Azure Güvenlik Merkezi 'Nde yanlış yapılandırmaları önleme
description: Güvenlik Merkezi 'nin ' zorla ' ve ' Reddet ' seçeneklerini öneriler Ayrıntılar sayfalarında nasıl kullanacağınızı öğrenin
services: security-center
author: memildin
manager: rkarlin
ms.service: security-center
ms.topic: how-to
ms.date: 02/04/2021
ms.author: memildin
ms.openlocfilehash: a3da9cdea543894aa7aec66112e28658beac84b5
ms.sourcegitcommit: f82e290076298b25a85e979a101753f9f16b720c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/04/2021
ms.locfileid: "99558181"
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

- Güvenlik Duvarı ve sanal ağ yapılandırmalarına sahip depolama hesaplarına erişim kısıtlı olmalıdır
- Redo için Azure önbelleği bir sanal ağ içinde yer almalıdır
- Azure Cosmos DB hesaplar, bekleyen verileri şifrelemek için müşteri tarafından yönetilen anahtarları kullanmalıdır
- Azure Machine Learning çalışma alanları, müşteri tarafından yönetilen bir anahtarla şifrelenmelidir (CMK)
- Azure yay bulutu, ağ ekleme kullanmalıdır
- Bilişsel hizmetler hesapları, müşteri tarafından yönetilen bir anahtarla (CMK) veri şifrelemeyi etkinleştirmelidir
- Kapsayıcı CPU ve bellek sınırları zorunlu kılınmalıdır
- Kapsayıcı görüntüleri yalnızca güvenilen kayıt defterlerinden dağıtılmalıdır
- Kapsayıcı kayıt defterleri, müşteri tarafından yönetilen bir anahtarla şifrelenmelidir (CMK)
- Ayrıcalık yükseltme ile kapsayıcının önlenebilir olması gerekir
- Gizli ana bilgisayar ad alanlarını paylaşan kapsayıcıların önlenebilir olması gerekir
- Kapsayıcılar yalnızca izin verilen bağlantı noktalarında dinleme yapılmalıdır
- Kapsayıcılar için sabit (salt okunurdur) kök dosya sistemi zorunlu kılınmalıdır
- Key Vault anahtarların bir sona erme tarihi olmalıdır
- Key Vault gizli dizileri için bir sona erme tarihi olmalıdır
- Anahtar kasaları Temizleme koruması etkin olmalıdır
- Anahtar kasaları geçici silme etkin olmalıdır
- Kapsayıcılar için en az ayrıcalıklı Linux özellikleri zorlanmalıdır
- Yalnızca Redis Cache güvenli bağlantılar etkinleştirilmelidir
- Kapsayıcılar AppArmor profilinin geçersiz kılınması veya devre dışı bırakılması kısıtlanıyor
- Ayrıcalıklı kapsayıcılar kaçınılmalıdır
- Kök kullanıcı olarak çalışan kapsayıcılar önlenebilir olmalıdır
- Depolama hesaplarına güvenli aktarım etkinleştirilmelidir
- Service Fabric kümelerinde ClusterProtectionLevel özelliği EncryptAndSign olarak ayarlanmalıdır
- Service Fabric kümeler yalnızca istemci kimlik doğrulaması için Azure Active Directory kullanmalıdır
- Hizmetlerin yalnızca izin verilen bağlantı noktalarını dinlemesi gerekir
- Depolama hesaplarının yeni Azure Resource Manager kaynaklarına geçirilmesi gerekir
- Depolama hesapları, sanal ağ kurallarını kullanarak ağ erişimini kısıtlamalı
- Konak ağ ve bağlantı noktalarının kullanımı sınırlandırılmalıdır
- , Güvenliği aşılmış kapsayıcılardan düğüm erişimini kısıtlamak için pod HostPath birimi takiciler, bilinen bir listeyle sınırlandırılmalıdır
- Azure Key Vault depolanan sertifikaların geçerlilik süresi 12 ayı aşmamalıdır
- Sanal makinelerin yeni Azure Resource Manager kaynaklara geçirilmesi gerekir
- Web uygulaması güvenlik duvarı (WAF) Application Gateway için etkinleştirilmelidir
- Azure ön kapı hizmeti hizmeti için Web uygulaması güvenlik duvarı (WAF) etkinleştirilmelidir

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