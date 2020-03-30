---
title: Önerilen güvenlik uygulamaları
description: Azure temsilci kaynak yönetimini kullanırken, güvenlik ve erişim denetimini göz önünde bulundurmak önemlidir.
ms.date: 03/24/2020
ms.topic: conceptual
ms.openlocfilehash: d9b806aaf988fedfde6ce468f3eff948aa8ce344
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80246917"
---
# <a name="recommended-security-practices"></a>Önerilen güvenlik uygulamaları

Azure [temsilci kaynak yönetimini](azure-delegated-resource-management.md)kullanırken, güvenlik ve erişim denetimini göz önünde bulundurmak önemlidir. Kiracınızdaki kullanıcılar müşteri aboneliklerine ve kaynak gruplarına doğrudan erişebilir, bu nedenle kiracınızın güvenliğini sağlamak için adımlar atmak isteyebilirsiniz. Ayrıca, yalnızca müşterilerinizin kaynaklarını etkili bir şekilde yönetmek için gereken erişime izin verdiğinizden emin olmak istersiniz. Bu konu, bunu yapmanıza yardımcı olacak öneriler sağlar.

## <a name="require-azure-multi-factor-authentication"></a>Azure Çok Faktörlü Kimlik Doğrulamayı İsteyin

[Azure Çok Faktörlü Kimlik Doğrulaması](../../active-directory/authentication/concept-mfa-howitworks.md) (iki aşamalı doğrulama olarak da bilinir), saldırganların birden çok kimlik doğrulama adımı gerektirerek bir hesaba erişmesini önlemeye yardımcı olur. Müşteri kaynaklarına erişimi olan kullanıcılar da dahil olmak üzere, hizmet sağlayıcı kiracınızdaki tüm kullanıcılar için Çok Faktörlü Kimlik Doğrulaması gerekir.

Müşterilerinizden kiracılarında da Azure Çok Faktörlü Kimlik Doğrulaması uygulamalarını istemenizi öneririz.

## <a name="assign-permissions-to-groups-using-the-principle-of-least-privilege"></a>En az ayrıcalık ilkesini kullanarak gruplara izin atama

Yönetimi kolaylaştırmak için, müşterilerinizin kaynaklarını yönetmek için gereken her rol için Azure AD kullanıcı gruplarını kullanmanızı öneririz. Bu, izinleri doğrudan o kullanıcıya atamak yerine tek tek kullanıcıları gerektiğinde gruba eklemenize veya kaldırmanıza olanak tanır.

> [!IMPORTANT]
> Bir Azure REKLAM grubu için izin eklemek için **Grup türü** **Office 365** **değil, Güvenlik** olmalıdır. Grup oluşturulduğunda bu seçenek seçilir. Daha fazla bilgi için [bkz.](../../active-directory/fundamentals/active-directory-groups-create-azure-portal.md)

İzin yapınızı oluştururken, kullanıcıların yalnızca işlerini tamamlamak için gereken izinlere sahip olmaları ve yanlışlıkla hata olasılığını azaltmaya yardımcı olmaları için en az ayrıcalık ilkesini uyguladığınızdan emin olun.

Örneğin, şu gibi bir yapı kullanmak isteyebilirsiniz:

|Grup adı  |Tür  |principalId  |Rol tanımı  |Rol tanımı kimliği  |
|---------|---------|---------|---------|---------|
|Mimarlar     |Kullanıcı grubu         |\<principalId\>         |Katılımcı         |b24988ac-6180-42a0-ab88-20f7382dd24c  |
|Değerlendirme     |Kullanıcı grubu         |\<principalId\>         |Okuyucu         |acdd72a7-3385-48ef-bd42-f606fba81ae7  |
|VM Uzmanları     |Kullanıcı grubu         |\<principalId\>         |VM Katılımcısı         |9980e02c-c2be-4d73-94e8-173b1dc7cf3c  |
|Automation     |Hizmet ana adı (SPN)         |\<principalId\>         |Katılımcı         |b24988ac-6180-42a0-ab88-20f7382dd24c  |

Bu grupları oluşturduktan sonra, kullanıcıları gerektiği gibi atayabilirsiniz. Yalnızca gerçekten erişimi olması gereken kullanıcıları ekleyin. Grup üyeliğini düzenli olarak gözden geçirdiğinizden ve artık dahil edilmesi uygun veya gerekli olmayan kullanıcıları kaldırdıklarına emin olun.

Bir kamu tarafından [yönetilen hizmet teklifi yle yerleşik müşterilere,](../how-to/publish-managed-services-offers.md)eklediğiniz herhangi bir grubun (veya kullanıcı veya hizmet ilkesinin) planı satın alan her müşteri için aynı izinlere sahip olacağını unutmayın. Her müşteriyle çalışmak üzere farklı gruplar atamak için, Azure Kaynak Yöneticisi şablonlarını kullanarak her müşteriye özel ayrı bir özel plan veya yerleşik müşterilere ayrı ayrı yayımlamanız gerekir. Örneğin, çok sınırlı erişimi olan bir genel plan yayımlayabilir, ardından gerektiğinde ek erişim sağlayan özelleştirilmiş bir Azure Kaynak Şablonu kullanarak ek erişim için doğrudan müşteriyle birlikte kaynaklarıyla birlikte çalışabilir.


## <a name="next-steps"></a>Sonraki adımlar

- [Azure Çok Faktörlü Kimlik Doğrulama'yı dağıtın.](../../active-directory/authentication/howto-mfa-getstarted.md)
- Kiracılar [arası yönetim deneyimleri](cross-tenant-management-experience.md)hakkında bilgi edinin.
