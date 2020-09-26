---
title: Azure AD dış kimlikleri için MAU faturalandırma modeli
description: Azure AD 'de Konuk Kullanıcı işbirliği (B2B) için Azure AD dış kimlikleri aylık etkin kullanıcıları (MAU) Faturalandırma modeli hakkında bilgi edinin. Azure AD kiracınızı bir Azure aboneliğine bağlamayı öğrenin.
services: active-directory
ms.service: active-directory
ms.subservice: B2B
ms.topic: conceptual
ms.date: 09/21/2020
ms.author: mimart
author: msmimart
manager: celestedg
ms.workload: identity
ms.collection: M365-identity-device-management
ms.openlocfilehash: d281b0ecd38ebbb76e093fd4f85213da4fc7d713
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/25/2020
ms.locfileid: "91265807"
---
# <a name="billing-model-for-azure-ad-external-identities"></a>Azure AD dış kimlikleri için faturalandırma modeli

Azure Active Directory (Azure AD) dış kimlikler fiyatlandırması, aylık etkin kullanıcıları (MAU) temel alır. Bu, bir takvim ayı içinde kimlik doğrulama etkinliği olan benzersiz kullanıcıların sayısıdır. Bu Faturalandırma modeli hem Azure AD Konuk Kullanıcı işbirliği (B2B) hem de [Azure AD B2C kiracılar](https://docs.microsoft.com/azure/active-directory-b2c/billing)için geçerlidir. MAU faturalandırma, ücretsiz bir katman ve esnek ve öngörülebilir bir fiyatlandırma sunarak maliyetleri düşürmenize yardımcı olur. Bu makalede, MAU faturalandırma ve Azure AD Kiracılarınızı bir aboneliğe bağlama hakkında bilgi edinin.

> [!IMPORTANT]
> Bu makale fiyatlandırma ayrıntıları içermez. Kullanım faturalaması ve fiyatlandırmayla ilgili en son bilgiler için bkz. [Azure Active Directory fiyatlandırması](https://azure.microsoft.com/pricing/details/active-directory/).

## <a name="what-do-i-need-to-do"></a>Neler yapmam gerekir?

MAU faturalandırmasından faydalanmak için Azure AD kiracınızın bir Azure aboneliğine bağlanması gerekir.

|Kiracınız:  |Şunları yapmanız gerekir:  |
|---------|---------|
| Zaten bir aboneliğe bağlanmış bir Azure AD kiracısı     | Hiçbir şey yapmayın. Konuk kullanıcılarla işbirliği yapmak için dış kimlikler özelliklerini kullandığınızda, MAU modeli kullanılarak otomatik olarak faturalandırılırsınız.        |
| Henüz bir aboneliğe bağlanmayan bir Azure AD kiracısı     | MAU faturalandırmayı etkinleştirmek için [Azure AD kiracınızı bir aboneliğe bağlayın](#link-your-azure-ad-tenant-to-a-subscription) .        |
|  |  |

## <a name="about-monthly-active-users-mau-billing"></a>Aylık etkin kullanıcılar (MAU) Faturalandırma hakkında

Azure AD kiracınızda Konuk Kullanıcı işbirliği kullanımı, bir takvim ayı içinde kimlik doğrulama etkinliği olan benzersiz Konuk kullanıcı sayısına göre faturalandırılır. Bu model, kiracınızdaki Azure AD Premium her bir lisans için beş adede kadar Konuk kullanıcıya izin veren 1:5 orandaki faturalandırma modelinin yerini alır. Kiracınız bir aboneliğe bağlandığında ve konuk kullanıcılarla işbirliği yapmak için dış kimlikler özelliklerini kullandığınızda, MAU tabanlı faturalandırma modeli kullanılarak otomatik olarak faturalandırılırsınız.
  
Konuk kullanıcılarınız için geçerli olan fiyatlandırma katmanı, Azure AD kiracınıza atanan en yüksek fiyatlandırma katmanını temel alır. Örneğin, kiracınızdaki en yüksek fiyatlandırma katmanı Azure AD Premium P1 ise, Premium P1 fiyatlandırma katmanı da konuk kullanıcılarınız için de geçerlidir. En yüksek fiyatlandırma Azure AD Ücretsiz, Konuk kullanıcılar için Premium özellikleri kullanmaya çalıştığınızda bir Premium fiyatlandırma katmanına yükseltmeniz istenir.

## <a name="link-your-azure-ad-tenant-to-a-subscription"></a>Azure AD kiracınızı bir aboneliğe bağlama

Uygun faturalandırma ve özelliklere erişim için Azure AD kiracısı bir Azure aboneliğine bağlanmalıdır. Dizinde bağlantı için kullanabileceğiniz bir abonelik yoksa, bu işlem sırasında bir tane ekleme fırsatına sahip olacaksınız.

1. Abonelik içinde en azından [katılımcı](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles) rolü veya abonelik içindeki bir kaynak grubu atanmış bir Azure hesabıyla [Azure Portal](https://portal.azure.com/) oturum açın.

2. Bağlamak istediğiniz dizini seçin: Azure portal araç çubuğunda **Dizin + abonelik** simgesini seçin ve ardından dizini seçin.

    ![Dizin + abonelik simgesini seçin](media/external-identities-pricing/portal-mau-pick-directory.png)

3. **Azure hizmetleri**altında **Azure Active Directory**' yi seçin.

4. Sol taraftaki menüden **dış kimlikler**' i seçin.

5. **Abonelikler**' ın altında **bağlı abonelikler**' i seçin.

6. Kiracı listesinde, kiracının yanındaki onay kutusunu işaretleyin ve ardından **abonelik bağlantısı**' nı seçin.

    ![Kiracıyı seçin ve bir aboneliği bağlayın](media/external-identities-pricing/linked-subscriptions.png)

7. Abonelik bağlantısı bölmesinde bir **abonelik** ve bir **kaynak grubu**seçin. Sonra **Uygula**'yı seçin.

   > [!NOTE]
   > Listelenen [bir abonelik yoksa, bir aboneliği kiracınızla ilişkilendirebilirsiniz](../fundamentals/active-directory-how-subscriptions-associated-directory.md). Ya da, **henüz bir aboneliğiniz yoksa**bağlantıyı seçerek yeni bir abonelik ekleyebilirsiniz.

    ![Bir abonelik ve kaynak grubu seçin](media/external-identities-pricing/link-subscription-resource.png)

Bu adımları tamamladıktan sonra Azure aboneliğiniz, varsa Azure doğrudan veya Kurumsal Anlaşma ayrıntılarına göre faturalandırılır.

## <a name="next-steps"></a>Sonraki adımlar

En son fiyatlandırma bilgileri için bkz. [Azure Active Directory fiyatlandırması](https://azure.microsoft.com/pricing/details/active-directory/).
