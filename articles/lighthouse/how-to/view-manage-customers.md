---
title: Müşterileri ve temsilci olarak atanmış kaynakları görüntüleme ve yönetme
description: Azure Use ile bir hizmet sağlayıcı veya kuruluş olarak, Azure portal tüm temsilcilerimizi ve aboneliklerinizi, müşterilerime giderek görüntüleyebilirsiniz.
ms.date: 08/12/2020
ms.topic: how-to
ms.openlocfilehash: 165110556282e90bbacf4ce3b3c59e34b203a528
ms.sourcegitcommit: c28fc1ec7d90f7e8b2e8775f5a250dd14a1622a6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/13/2020
ms.locfileid: "88167207"
---
# <a name="view-and-manage-customers-and-delegated-resources"></a>Müşterileri ve temsilci olarak atanmış kaynakları görüntüleme ve yönetme

[Azure açık Thouse](../overview.md) kullanan hizmet sağlayıcıları, temsilcili müşteri kaynaklarını ve abonelikleri görüntülemek için [Azure Portal](https://portal.azure.com) **müşteriler** sayfasını kullanabilir. 

> [!TIP]
> Burada hizmet sağlayıcılarına ve müşterilere başvuracağız. [birden çok kiracıyı yöneten kuruluşlar](../concepts/enterprise.md) , yönetim deneyimlerini birleştirmek için aynı süreci kullanabilir.

Azure portal **müşterilerimiz** sayfasına erişmek Için, **tüm hizmetler**' i seçin, ardından **müşterilerimi** arayın ve seçin. Ayrıca, Azure portal üst kısmındaki arama kutusuna "My Customers" girerek de bulabilirsiniz.

**Müşteriler** sayfasının en popüler **müşterileri** bölümünün yalnızca abonelik veya kaynak grupları için temsilci atanmış müşteriler hakkındaki bilgileri gösterdiğini aklınızda bulundurun. Diğer müşterilerle (örneğin, [bulut çözümü sağlayıcısı programı](/partner-center/csp-overview)aracılığıyla) çalışıyorsanız, [kaynaklarını Azure açık bir şekilde eklendi](onboard-customer.md)kullanmadığınız sürece **müşteriler** bölümünde bu müşterilerle ilgili bilgileri görmezsiniz.

Sayfada daha düşük olan **bulut çözümü sağlayıcısı (Önizleme)** adlı ayrı bir bölüm, [Microsoft Müşteri SÖZLEŞMESI 'ni (MCA) imzalayan](/partner-center/confirm-customer-agreement) ve [Azure planı kapsamında](/partner-center/azure-plan-get-started)olan CSP müşterileriniz için faturalandırma bilgilerini ve kaynakları gösterir. Daha fazla bilgi için bkz. [Microsoft Iş ortağı sözleşmesi Faturalandırma hesabınızı kullanmaya başlama](../../cost-management-billing/understand/mpa-overview.md). Bu tür CSP müşterilerinin Ayrıca, Azure açık Thouse ile de eklendi etmeksizin bu bölümde göründüğünü unutmayın. Benzer şekilde, bir CSP müşterisi, Azure açık Thouse 'a eklemek için **müşterilerimin** **bulut çözümü sağlayıcısı (Önizleme)** bölümünde gözükmez.

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

Temsilciler, kendisine erişimi olan Kullanıcı ve izinlerle birlikte, temsilci seçilmiş olan abonelik veya kaynak grubunu gösterir. Bu bilgileri görüntülemek için, **müşterilerimiz** sayfasının sol tarafındaki **temsilci** seçme ' yi seçin.

Sayfanın üst kısmındaki filtreler, erişim atama bilgilerinizi sıralamanıza ve gruplandırarak belirli müşterilere, tekliflere veya anahtar sözcüklere göre filtrelemenize izin verir.

### <a name="view-role-assignments"></a>Rol atamalarını görüntüle

Her bir temsilciyle ilişkili kullanıcılar ve izinler, **rol atamaları** sütununda görünür. Her bir girişi, abonelik veya kaynak grubuna erişim izni verilen kullanıcıların, grupların ve hizmet sorumlularının tam listesini görüntülemek için seçebilirsiniz. Buradan, daha fazla ayrıntı edinmek için belirli bir Kullanıcı, Grup veya hizmet asıl adı seçebilirsiniz.

### <a name="remove-delegations"></a>Temsilcileri kaldır

Bir müşteriyi Azure Aydınlam 'ye eklerken [yönetilen hizmetler kayıt ataması silme rolü](../../role-based-access-control/built-in-roles.md#managed-services-registration-assignment-delete-role) ' ne sahipseniz, bu kullanıcılar bu temsilcinin satırında görünen çöp kutusu simgesini seçerek bir temsilciyi kaldırabilir. Bunu yaparken, hizmet sağlayıcısının kiracısındaki hiçbir Kullanıcı daha önce atanmış kaynaklara erişemeyecektir.

## <a name="work-in-the-context-of-a-delegated-subscription"></a>Temsilci atanmış bir abonelik bağlamında çalışın

Azure portal içinde, üzerinde çalıştığınız dizini değiştirmeden, temsilci bir abonelik bağlamında doğrudan çalışabilirsiniz. Bunun için:

1. Azure portal üst tarafında bulunan **Dizin + abonelik** simgesini seçin.
2. **Genel abonelik** filtresinde, yalnızca bu temsilci abonelik için kutunun seçildiğinden emin olun. Yalnızca belirli bir dizin içindeki abonelikleri göstermek için **geçerli + temsilci Dizin** açılan kutusunu kullanabilirsiniz. (Bu, oturum açtığınız dizini değiştirdiği için **Dizin Değiştir** seçeneğini kullanmayın.)

Daha sonra, [çapraz kiracı yönetim deneyimlerini](../concepts/cross-tenant-management-experience.md)destekleyen bir hizmete eriştiğinizde, hizmet varsayılan olarak seçtiğiniz Temsilcili aboneliğin bağlamıdır. Yukarıdaki adımları izleyerek ve **Tümünü Seç** kutusunu işaretleyerek (veya bunun yerine içinde çalışacak bir veya daha fazla abonelik seçerek) bunu değiştirebilirsiniz.

> [!NOTE]
> Bir aboneliğin tamamına erişmek yerine bir veya daha fazla kaynak grubuna erişim verdiyseniz, bu kaynak grubunun ait olduğu aboneliği seçebilirsiniz. Daha sonra bu aboneliğin bağlamında çalışacaksınız, ancak yalnızca belirlenen kaynak gruplarına erişebileceksiniz.

Ayrıca, bu hizmetin içinden abonelik veya kaynak grubunu seçerek, şirketler arası yönetim deneyimlerini destekleyen hizmetler içinden Temsilcili abonelikler veya kaynak gruplarıyla ilgili işlevlere da erişebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

- [Çapraz kiracı yönetim deneyimleri](../concepts/cross-tenant-management-experience.md)hakkında bilgi edinin.
- Müşterilerinizin Azure portal **hizmet sağlayıcılarına** giderek [hizmet sağlayıcılarını nasıl görüntüleyip yönetebileceğini](view-manage-service-providers.md) öğrenin.
