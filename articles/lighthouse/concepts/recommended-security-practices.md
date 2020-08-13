---
title: Önerilen güvenlik uygulamaları
description: Azure ışıklı kullanımı kullanılırken, güvenlik ve erişim denetimi göz önünde bulundurmanız önemlidir.
ms.date: 08/12/2020
ms.topic: conceptual
ms.openlocfilehash: ddc4d2c69e7aacc49c5309bc407aecd9bc8e572f
ms.sourcegitcommit: c28fc1ec7d90f7e8b2e8775f5a250dd14a1622a6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/13/2020
ms.locfileid: "88163280"
---
# <a name="recommended-security-practices"></a>Önerilen güvenlik uygulamaları

[Azure ışıklı kullanımı](../overview.md)kullanılırken, güvenlik ve erişim denetimi göz önünde bulundurmanız önemlidir. Kiracınızdaki kullanıcılar müşteri aboneliklerine ve kaynak gruplarına doğrudan erişebilir, bu nedenle kiracınızın güvenliğini sağlamak için adımları uygulamanız gerekir. Ayrıca, yalnızca müşterilerinizin kaynaklarını etkin bir şekilde yönetmek için gereken erişime izin verdiğinizden emin olmak isteyeceksiniz. Bu konu, bunu yapmanıza yardımcı olacak öneriler sağlar.

> [!TIP]
> Bu öneriler, Azure açık Thouse ile [birden çok kiracıyı yöneten kuruluşlar](enterprise.md) için de geçerlidir.

## <a name="require-azure-multi-factor-authentication"></a>Azure Multi-Factor Authentication’ı gerektirme

[Azure Multi-Factor Authentication](../../active-directory/authentication/concept-mfa-howitworks.md) (iki adımlı doğrulama olarak da bilinir), saldırganların birden çok kimlik doğrulama adımı gerektirerek bir hesaba erişim sağlamasını önlemeye yardımcı olur. Temsilcili müşteri kaynaklarına erişimi olacak kullanıcılar da dahil olmak üzere, yönettiğiniz kiracınızdaki tüm kullanıcılar için Multi-Factor Authentication gerekir.

Müşterilerinizin kiracılarında Azure Multi-Factor Authentication uygulamasını da sağlamasını öneririz.

## <a name="assign-permissions-to-groups-using-the-principle-of-least-privilege"></a>En az ayrıcalık ilkesini kullanarak gruplara izin atama

Yönetimi kolaylaştırmak için müşterilerinizin kaynaklarını yönetmek için gereken her bir rol için Azure Active Directory (Azure AD) gruplarını kullanın. Bu, her bir kullanıcıya izinleri doğrudan atamak yerine, her kullanıcıyı gerektiği gibi eklemenize veya kaldırmanıza olanak sağlar.

> [!IMPORTANT]
> Bir Azure AD grubu için izinler eklemek üzere, **Grup türü** **güvenlik** olmalıdır ve **Office 365**' i değil. Grup oluşturulduğunda bu seçenek seçilidir. Daha fazla bilgi için bkz. [temel Grup oluşturma ve Azure Active Directory kullanarak üye ekleme](../../active-directory/fundamentals/active-directory-groups-create-azure-portal.md).

İzin yapınızı oluştururken, kullanıcıların yalnızca işini tamamlaması için gerekli izinlere sahip olması için en az ayrıcalık ilkesini izlediğinizden emin olun, böylece yanlışlıkla oluşan hatalara karşı daha fazla hata olasılığını azaltır.

Örneğin, aşağıdaki gibi bir yapı kullanmak isteyebilirsiniz:

|Grup adı  |Tür  |principalId  |Rol tanımı  |Rol tanımı KIMLIĞI  |
|---------|---------|---------|---------|---------|
|Ları     |Kullanıcı grubu         |\<principalId\>         |Katkıda Bulunan         |b24988ac-6180-42a0-ab88-20f7382dd24c  |
|Değerlendirme     |Kullanıcı grubu         |\<principalId\>         |Okuyucu         |acdd72a7-3385-48ef-bd42-f606fba81ae7  |
|VM uzmanları     |Kullanıcı grubu         |\<principalId\>         |VM Katılımcısı         |9980e02c-c2be-4d73-94E8-173b1dc7cf3c  |
|Otomatikleştirme     |Hizmet asıl adı (SPN)         |\<principalId\>         |Katılımcı         |b24988ac-6180-42a0-ab88-20f7382dd24c  |

Bu grupları oluşturduktan sonra, gerektiğinde kullanıcıları atayabilirsiniz. Yalnızca, erişimi olması gereken kullanıcıları ekleyin. Grup üyeliğini düzenli olarak gözden geçirdiğinizden emin olun ve artık uygun veya gerekli olmayan tüm kullanıcıları kaldırın.

[Ortak yönetilen bir hizmet teklifi aracılığıyla müşterileri](../how-to/publish-managed-services-offers.md)eklediğinizde, eklediğiniz herhangi bir grup (veya Kullanıcı veya hizmet sorumlusu) planı satın alan her müşteri için aynı izinlere sahip olacağını unutmayın. Her müşteriyle çalışacak farklı gruplar atamak için, her müşteri için özel olarak özel bir plan yayımlamanız veya Azure Resource Manager şablonlarını kullanarak müşterileri ayrı ayrı birlikte yayımlamanız gerekir. Örneğin, çok sınırlı erişimi olan bir genel planı yayımlayabilir ve daha sonra gerekli şekilde ek erişim sağlayan özelleştirilmiş bir Azure kaynak şablonu kullanarak daha fazla erişim sağlamak için müşteriyle doğrudan birlikte çalışabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Multi-Factor Authentication 'Yi dağıtın](../../active-directory/authentication/howto-mfa-getstarted.md).
- [Çapraz kiracı yönetim deneyimleri](cross-tenant-management-experience.md)hakkında bilgi edinin.
