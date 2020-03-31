---
title: Müşterileri ve temsilci olarak atanmış kaynakları görüntüleme ve yönetme
description: Azure temsilci kaynak yönetimini kullanan bir hizmet sağlayıcısı olarak, Azure portalındaki müşterilerime giderek tüm temsilci leştirilmiş müşteri kaynaklarınızı ve aboneliklerinizi görüntüleyebilirsiniz.
ms.date: 01/22/2020
ms.topic: conceptual
ms.openlocfilehash: 0d4b3187066754e8a549f029623762df539b30b1
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76543435"
---
# <a name="view-and-manage-customers-and-delegated-resources"></a>Müşterileri ve temsilci olarak atanmış kaynakları görüntüleme ve yönetme

[Azure temsilci kaynak yönetimini](../concepts/azure-delegated-resource-management.md) kullanan hizmet sağlayıcıları, devredilen müşteri kaynaklarını ve abonelikleri görüntülemek için Azure [portalındaki](https://portal.azure.com) **Müşterilerim** sayfasını kullanabilir. Burada hizmet sağlayıcılara ve müşterilere atıfta bulunsak da, birden çok kiracıyı yöneten işletmeler yönetim deneyimlerini pekiştirmek için aynı işlemi kullanabilir.

Azure portalındaki **müşterilerim** sayfasına erişmek için **Tüm hizmetleri**seçin, ardından **müşterilerimi** arayın ve seçin. Azure portalının üst kısmındaki arama kutusuna "Müşterilerim" girerek de bulabilirsiniz.

**Müşterilerim** sayfasının en üst teki **Müşteriler** bölümünün yalnızca abonelik veya kaynak grupları devralan müşterilerle ilgili bilgileri gösterdiğini unutmayın. Diğer müşterilerle çalışıyorsanız (örneğin [Bulut Çözüm Sağlayıcısı programı](https://docs.microsoft.com/partner-center/csp-overview)gibi), Azure temsilci kaynak yönetimi için kaynaklarını gemiye binmedikçe **Müşteriler** bölümünde bu müşterilerle ilgili bilgileri görmezsiniz.

Sayfada daha düşük bir yerde, **Bulut Çözüm Sağlayıcısı (Önizleme)** adlı ayrı bir bölüm, [Microsoft Müşteri Sözleşmesi'ni (MCA) imzalayan](https://docs.microsoft.com/partner-center/confirm-customer-agreement) ve Azure planı [altında](https://docs.microsoft.com/partner-center/azure-plan-get-started)olan CSP müşterileriniz için fatura bilgilerini ve kaynaklarını gösterir. Daha fazla bilgi için Microsoft [İş Ortağı Sözleşmesi faturalandırma hesabınızla başlayın.](../../billing/mpa-overview.md) Azure temsilci kaynak yönetimi için bu tür CSP müşterilerinin de bu bölümde görüntülenip yerleştirilmediğini unutmayın. Benzer şekilde, Azure temsilci kaynak yönetimi için bu müşterilere binmeniz için bir CSP müşterisinin **müşterilerimin** **Bulut Çözüm Sağlayıcısı (Önizleme)** bölümünde görünmesi gerekmez.

> [!NOTE]
> Müşterileriniz, Azure portalında **Servis sağlayıcılarına** göz atarak hizmet sağlayıcıları hakkındaki bilgileri görüntüleyebilir. Daha fazla bilgi için [bkz.](view-manage-service-providers.md)

## <a name="view-and-manage-customer-details"></a>Müşteri ayrıntılarını görüntüleme ve yönetme

Müşteri ayrıntılarını görüntülemek **için, müşterilerim** sayfasının sol tarafındaki **Müşterileri** seçin.

Her müşteri için müşterinin adını, müşteri kimliğini (kiracı kimliğini) ve etkileşimle ilişkili teklifi görürsünüz. **Delegasyonlar** sütununda, devredilen abonelik lerin ve/veya temsilci kaynağı gruplarının sayısını görürsünüz.

> [!IMPORTANT]
> Bir temsilci görmek için, kullanıcılara onboarding işleminde [Reader](../../role-based-access-control/built-in-roles.md#reader) rolü (veya Reader erişimini içeren başka bir yerleşik rol) verilmiş olmalıdır.

Sayfanın üst kısmındaki filtreler, müşteri bilgilerinizi sıralamanıza ve gruplandırmanıza veya belirli müşterilere, tekliflere veya anahtar kelimelere göre filtrelemenize izin verir.

Aşağıdaki bilgileri bu sayfadan görüntüleyebilirsiniz:

- Müşteriyle ilişkili tüm abonelikleri, teklifleri ve temsilcilikleri görmek için müşterinin adını seçin.
- Teklif ve heyetleri hakkında daha fazla bilgi almak için teklif adını seçin.
- Devredilen abonelikler veya kaynak grupları için rol atamaları hakkında daha fazla ayrıntı görüntülemek **için, Temsilciler** sütunundaki girişi seçin.

## <a name="view-and-manage-delegations"></a>Heyetleri görüntüleme ve yönetme

Delegasyonlar, devredilen abonelik/kaynak grubunu, kullanıcılar ve ona erişimi olan izinlerle birlikte gösterir. Bu bilgileri görüntülemek **için, müşterilerim** sayfasının sol tarafında **Temsilciler'i** seçin.

Sayfanın üst kısmındaki filtreler, erişim atama bilgilerinizi sıralamanıza ve gruplandırmanıza veya belirli müşterilere, tekliflere veya anahtar kelimelere göre filtrelemenize izin verir.

### <a name="view-role-assignments"></a>Rol atamalarını görüntüle

Her temsilciyle ilişkili kullanıcılar ve izinler **Rol atamaları** sütununda görünür. Abonelik veya kaynak grubuna erişim izni verilen kullanıcıların, grupların ve hizmet ilkelerinin tam listesini görüntülemek için her girişi seçebilirsiniz. Buradan, daha fazla ayrıntı almak için belirli bir kullanıcı, grup veya hizmet ana adı seçebilirsiniz.

### <a name="remove-delegations"></a>Delegasyonları kaldırma

[Yönetilen Hizmetler Kaydı Atama Silme Rolü'ne](../../role-based-access-control/built-in-roles.md#managed-services-registration-assignment-delete-role) sahip kullanıcıları Azure temsilci kaynak yönetimi için bir müşteriye binerken eklediyseniz, bu kullanıcılar bu temsilciiçin satırda görünen çöp kutusu simgesini seçerek bir temsilciyi kaldırabilir. Bunu yaptıkları zaman, hizmet sağlayıcısının kiracısındaki hiçbir kullanıcı daha önce devredilen kaynaklara erişemez.


## <a name="work-in-the-context-of-a-delegated-subscription"></a>Devredilen abonelik bağlamında çalışma

Çalıştığınız dizinin iliğini değiştirmeden doğrudan Azure portalı içinde devralınan bir abonelik bağlamında çalışabilirsiniz. Bunu yapmak için:

1. Azure portalının üst kısmındaki **Dizin + Abonelik** simgesini seçin.
2. Genel **abonelik** filtresinde, yalnızca bu devralan abone aboneliği için kutunun seçildiğinden emin olun. Yalnızca belirli bir dizindeki abonelikleri göstermek için **Geçerli + temsilcidizin** açılır kutusunu kullanabilirsiniz. (Oturum açtığınızdaki dizini değiştirdiğinden **Switch dizinini** kullanmayın.)

Daha [sonra, kiracılar arası yönetim deneyimlerini](../concepts/cross-tenant-management-experience.md)destekleyen bir hizmete erişirseniz, hizmet seçtiğiniz devredilen aboneliğin bağlamına varsayılan olarak gelir. Yukarıdaki adımları izleyerek ve **Tüm ünü seç** kutusunu işaretleyerek (veya bunun yerine çalışmak için bir veya daha fazla abonelik seçerek) bunu değiştirebilirsiniz.

> [!NOTE]
> Tüm abonelik yerine bir veya daha fazla kaynak grubuna erişim izni verildiyse, bu kaynak grubunun ait olduğu aboneliği seçebilirsiniz. Daha sonra bu abonelik bağlamında çalışırsınız, ancak yalnızca belirlenen kaynak gruplarına erişebilirsiniz.

Ayrıca, bu hizmet in içinden abonelik veya kaynak grubunu seçerek, kiracılar arası yönetim deneyimlerini destekleyen hizmetler içinden devredilen aboneliklerle veya kaynak gruplarıyla ilgili işlevlere de erişebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

- Kiracılar [arası yönetim deneyimleri](../concepts/cross-tenant-management-experience.md)hakkında bilgi edinin.
- Azure portalındaki **Servis sağlayıcılarına** giderek müşterilerinizin hizmet sağlayıcılarını nasıl [görüntüleyip yönetebileceğini](view-manage-service-providers.md) öğrenin.
