---
author: baanders
description: adımlara genel bakış için dosya ekleme ve Azure dijital TWINS kurulumu 'nda izin ön eki
ms.service: digital-twins
ms.topic: include
ms.date: 8/7/2020
ms.author: baanders
ms.openlocfilehash: b87d45a8be68a77df7b06ebd6e80562ccbe0e444
ms.sourcegitcommit: 98854e3bd1ab04ce42816cae1892ed0caeedf461
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/07/2020
ms.locfileid: "88009688"
---
>[!NOTE]
>Bu işlemler, Azure aboneliğindeki kaynakları ve Kullanıcı erişimini yönetmek için bir kullanıcı tarafından bir kullanıcı tarafından tamamlanacak şekilde tasarlanmıştır. Bazı adımlar daha düşük izinlerle tamamlanabilse de, bu izinlere sahip birisinin ortak işlemi, kullanılabilir bir örneği tamamen ayarlamak için gerekli olacaktır. [*Önkoşul: gerekli izinler*](#prerequisites-permission-requirements) bölümünde bunun hakkında daha fazla bilgi görüntüleyin.

Yeni bir Azure dijital TWINS örneği için tam kurulum üç bölümden oluşur:
1. **Örnek oluşturma**
2. **Kullanıcı erişim Izinlerini ayarlama**: Azure kullanıcılarının Azure dijital TWINS örneğinde, verileri ve verilerini yönetebilmek Için Azure *dijital TWINS sahibi (Önizleme)* rolünün olması gerekir. Bu adımda, Azure aboneliğinin sahibi/Yöneticisi, bu rolü Azure dijital TWINS örneğinizi yöneten kişiye atayacaktır. Bu, sizin veya kuruluşunuzdaki başka bir kullanıcı olabilir.
3. **İstemci uygulamaları için erişim Izinlerini ayarlama**: örneğinizle etkileşim kurmak için kullanılacak bir istemci uygulaması yazmak yaygındır. Bu istemci uygulamanın Azure dijital TWINS 'nize erişmesi için, istemci uygulamanın örnekte kimlik doğrulamak üzere kullanacağı [Azure Active Directory](../articles/active-directory/fundamentals/active-directory-whatis.md) bir *uygulama kaydı* ayarlamanız gerekir.

Devam etmek için bir Azure aboneliğine ihtiyacınız olacaktır. [Buradan](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)birini ücretsiz olarak ayarlayabilirsiniz.

## <a name="prerequisites-permission-requirements"></a>Önkoşullar: Izin gereksinimleri

Bu makaledeki tüm adımları tamamlayabilmeniz için, aboneliğinizde aşağıdaki izinlere sahip bir [rolün](../articles/role-based-access-control/rbac-and-directory-admin-roles.md) olması gerekir:
* Azure kaynaklarını oluşturma ve yönetme
* Azure kaynaklarına Kullanıcı erişimini yönetme (izin verme ve temsilci atama dahil)

Bu gereksinimi karşılayan ortak roller, *sahip*, *Hesap Yöneticisi*veya *Kullanıcı erişimi Yöneticisi* ile *katkıda bulunan*birleşimidir. Rol ve izinlerin, diğer rollere dahil edilen izinler dahil olmak üzere tüm açıklamaları için, Azure RBAC belgelerindeki [*Klasik abonelik yöneticisi rolleri, Azure rolleri ve Azure AD rolleri*](../articles/role-based-access-control/rbac-and-directory-admin-roles.md) ' ni ziyaret edin.

Aboneliğinizdeki rolü görüntülemek için Azure portal [abonelikler sayfasını](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade) ziyaret edin (Bu bağlantıyı kullanabilir veya Portal arama çubuğu ile *abonelikleri* arayabilirsiniz). Kullanmakta olduğunuz aboneliğin adını arayın *ve rolü rol sütununda görüntüleyin* :

:::image type="content" source="../articles/digital-twins/media/how-to-set-up-instance/portal/subscriptions-role.png" alt-text="Kullanıcı sahip olarak gösteren Azure portal Abonelikler sayfasının görünümü" lightbox="../articles/digital-twins/media/how-to-set-up-instance/portal/subscriptions-role.png":::

Değerin *katkıda*bulunduğunu veya yukarıda açıklanan gerekli izinlere sahip olmayan başka bir rolü fark ederseniz, aboneliğinizde bu *izinlere sahip olan* kullanıcıyla (abonelik sahibi veya hesap yöneticisi gibi) başvurabilirsiniz ve aşağıdaki yollarla devam edebilirsiniz:
* Sizin adınıza bu makaledeki adımları tamamlamaları isteyin
* Kendinize devam etmek için izinleriniz olacak şekilde abonelikte rolünüzü yükseltirse. Bunun uygun olup olmadığı, kuruluşunuza ve onun içindeki rolünüze bağlıdır.