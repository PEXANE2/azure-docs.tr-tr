---
title: Müşterileri ve temsilci olarak atanmış kaynakları görüntüleme ve yönetme
description: Azure tarafından atanan kaynak yönetimini kullanan bir hizmet sağlayıcı olarak, tüm Temsilcili müşteri kaynaklarınızı ve aboneliklerinizi Azure portal müşterilerime giderek görüntüleyebilirsiniz.
ms.date: 01/22/2020
ms.topic: conceptual
ms.openlocfilehash: 0d4b3187066754e8a549f029623762df539b30b1
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "76543435"
---
# <a name="view-and-manage-customers-and-delegated-resources"></a>Müşterileri ve temsilci olarak atanmış kaynakları görüntüleme ve yönetme

[Azure tarafından yetkilendirilen kaynak yönetimini](../concepts/azure-delegated-resource-management.md) kullanan hizmet sağlayıcıları, temsilcili müşteri kaynaklarını ve abonelikleri görüntülemek için [Azure Portal](https://portal.azure.com) **müşterilerim** sayfasını kullanabilir. Burada hizmet sağlayıcılarına ve müşterilere başvuracağız. birden çok kiracıyı yöneten kuruluşlar, yönetim deneyimlerini birleştirmek için aynı süreci kullanabilir.

Azure portal **müşterilerimiz** sayfasına erişmek Için, **tüm hizmetler**' i seçin, ardından **müşterilerimi** arayın ve seçin. Ayrıca, Azure portal üst kısmındaki arama kutusuna "My Customers" girerek de bulabilirsiniz.

**Müşteriler** sayfasının en popüler **müşterileri** bölümünün yalnızca abonelik veya kaynak grupları için temsilci atanmış müşteriler hakkındaki bilgileri gösterdiğini aklınızda bulundurun. Diğer müşterilerle (örneğin, [bulut çözüm sağlayıcısı programı](https://docs.microsoft.com/partner-center/csp-overview)aracılığıyla) çalışıyorsanız, Azure tarafından atanan kaynak yönetimi için kaynaklarına eklendi sahip olmadığınız sürece **müşteriler** bölümünde bu müşterilerle ilgili bilgileri görmezsiniz.

Sayfada daha düşük olan **bulut çözümü sağlayıcısı (Önizleme)** adlı ayrı bir bölüm, [Microsoft Müşteri SÖZLEŞMESI 'ni (MCA) imzalayan](https://docs.microsoft.com/partner-center/confirm-customer-agreement) ve [Azure planı kapsamında](https://docs.microsoft.com/partner-center/azure-plan-get-started)olan CSP müşterileriniz için faturalandırma bilgilerini ve kaynakları gösterir. Daha fazla bilgi için bkz. [Microsoft Iş ortağı sözleşmesi Faturalandırma hesabınızı kullanmaya başlama](../../billing/mpa-overview.md). Bu tür CSP müşterilerinin, Azure tarafından atanan kaynak yönetimi için de eklendi sahip olup olmadığına bakılmaksızın bu bölümde göründüğünü unutmayın. Benzer şekilde, bir CSP müşterisi, Azure tarafından atanan kaynak yönetimine yönelik olarak bunları eklemek için **müşterilerimin** **bulut çözümü sağlayıcısı (Önizleme)** bölümünde görünmesini gerektirmez.

> [!NOTE]
> Müşterileriniz, Azure portal hizmet **sağlayıcılarına** giderek hizmet sağlayıcılarıyla ilgili bilgileri görüntüleyebilir. Daha fazla bilgi için bkz. [hizmet sağlayıcılarını görüntüleme ve yönetme](view-manage-service-providers.md).

## <a name="view-and-manage-customer-details"></a>Müşteri ayrıntılarını görüntüleme ve yönetme

Müşteri ayrıntılarını görüntülemek için **müşterilerimin** sol tarafında bulunan **müşteriler** ' i seçin.

Her müşteri için müşterinin adını, müşteri KIMLIĞINI (kiracı KIMLIĞI) ve görevlendirmede ilişkili teklifi görürsünüz. **Temsilciler** sütununda, temsilci olan aboneliklerin sayısını ve/veya atanan kaynak grubu sayısını görürsünüz.

> [!IMPORTANT]
> Bir temsilciyi görmek için kullanıcılara, ekleme sürecinde [okuyucu](../../role-based-access-control/built-in-roles.md#reader) rolü (veya okuyucu erişimi içeren başka bir yerleşik rol) verilmiş olması gerekir.

Sayfanın üst kısmındaki filtreler, müşteri bilgilerinizi, tekliflerini veya anahtar sözcüklere göre filtrelemenizi ve gruplamanıza olanak tanır.

Bu sayfadan aşağıdaki bilgileri görüntüleyebilirsiniz:

- Bir müşteriyle ilişkili tüm abonelikleri, teklifleri ve temsilcileri görmek için müşterinin adını seçin.
- Bir teklif ve temsilcileri hakkında daha fazla ayrıntı görmek için teklif adını seçin.
- Temsilci atanan abonelikler veya kaynak grupları için rol atamaları hakkında daha fazla ayrıntı görüntülemek için **Temsilciler** sütunundaki girişi seçin.

## <a name="view-and-manage-delegations"></a>Temsilcileri görüntüleyin ve yönetin

Temsilciler, atanmış olan abonelik/kaynak grubunu ve ona erişimi olan kullanıcılar ve izinlerle birlikte gösterir. Bu bilgileri görüntülemek için, **müşterilerimiz** sayfasının sol tarafındaki **temsilci** seçme ' yi seçin.

Sayfanın üst kısmındaki filtreler, erişim atama bilgilerinizi sıralamanıza ve gruplandırarak belirli müşterilere, tekliflere veya anahtar sözcüklere göre filtrelemenize izin verir.

### <a name="view-role-assignments"></a>Rol atamalarını görüntüle

Her bir temsilciyle ilişkili kullanıcılar ve izinler, **rol atamaları** sütununda görünür. Her bir girişi, abonelik veya kaynak grubuna erişim izni verilen kullanıcıların, grupların ve hizmet sorumlularının tam listesini görüntülemek için seçebilirsiniz. Buradan, daha fazla ayrıntı edinmek için belirli bir Kullanıcı, Grup veya hizmet asıl adı seçebilirsiniz.

### <a name="remove-delegations"></a>Temsilcileri kaldır

Azure tarafından atanan kaynak yönetimi için bir müşteri eklerken [yönetilen hizmetler kayıt ataması silme rolüne](../../role-based-access-control/built-in-roles.md#managed-services-registration-assignment-delete-role) sahip kullanıcılar eklediyseniz, bu kullanıcılar bu temsilcinin satırında görünen çöp kutusu simgesini seçerek bir temsilciyi kaldırabilir. Bunu yaparken, hizmet sağlayıcısının kiracısındaki hiçbir Kullanıcı daha önce atanmış kaynaklara erişemeyecektir.


## <a name="work-in-the-context-of-a-delegated-subscription"></a>Temsilci atanmış bir abonelik bağlamında çalışın

Azure portal içinde, üzerinde çalıştığınız dizini değiştirmeden, temsilci bir abonelik bağlamında doğrudan çalışabilirsiniz. Bunu yapmak için:

1. Azure portal üst tarafında bulunan **Dizin + abonelik** simgesini seçin.
2. **Genel abonelik** filtresinde, yalnızca bu temsilci abonelik için kutunun seçildiğinden emin olun. Yalnızca belirli bir dizin içindeki abonelikleri göstermek için **geçerli + temsilci Dizin** açılan kutusunu kullanabilirsiniz. (Bu, oturum açtığınız dizini değiştirdiği için **Dizin Değiştir** seçeneğini kullanmayın.)

Daha sonra, [çapraz kiracı yönetim deneyimlerini](../concepts/cross-tenant-management-experience.md)destekleyen bir hizmete eriştiğinizde, hizmet varsayılan olarak seçtiğiniz Temsilcili aboneliğin bağlamıdır. Yukarıdaki adımları izleyerek ve **Tümünü Seç** kutusunu işaretleyerek (veya bunun yerine içinde çalışacak bir veya daha fazla abonelik seçerek) bunu değiştirebilirsiniz.

> [!NOTE]
> Bir aboneliğin tamamına erişmek yerine bir veya daha fazla kaynak grubuna erişim verdiyseniz, bu kaynak grubunun ait olduğu aboneliği seçebilirsiniz. Daha sonra bu aboneliğin bağlamında çalışacaksınız, ancak yalnızca belirlenen kaynak gruplarına erişebileceksiniz.

Ayrıca, bu hizmetin içinden abonelik veya kaynak grubunu seçerek, şirketler arası yönetim deneyimlerini destekleyen hizmetler içinden Temsilcili abonelikler veya kaynak gruplarıyla ilgili işlevlere da erişebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

- [Çapraz kiracı yönetim deneyimleri](../concepts/cross-tenant-management-experience.md)hakkında bilgi edinin.
- Müşterilerinizin Azure portal **hizmet sağlayıcılarına** giderek [hizmet sağlayıcılarını nasıl görüntüleyip yönetebileceğini](view-manage-service-providers.md) öğrenin.
