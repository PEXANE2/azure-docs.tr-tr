---
title: Müşterileri ve Temsilcili kaynakları görüntüleme ve yönetme
description: Azure tarafından atanan kaynak yönetimini kullanan bir hizmet sağlayıcı olarak, tüm Temsilcili müşteri kaynaklarınızı ve aboneliklerinizi Azure portal müşterilerime giderek görüntüleyebilirsiniz.
author: JnHs
ms.author: jenhayes
ms.service: lighthouse
ms.date: 07/11/2019
ms.topic: overview
manager: carmonm
ms.openlocfilehash: 8163bcb4189cbec545544b7d43ba7142d34e29dd
ms.sourcegitcommit: e0a1a9e4a5c92d57deb168580e8aa1306bd94723
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2019
ms.locfileid: "72286162"
---
# <a name="view-and-manage-customers-and-delegated-resources"></a>Müşterileri ve Temsilcili kaynakları görüntüleme ve yönetme

[Azure tarafından yetkilendirilen kaynak yönetimini](../concepts/azure-delegated-resource-management.md) kullanan hizmet sağlayıcıları, temsilcili müşteri kaynaklarını ve abonelikleri görüntülemek için [Azure Portal](https://portal.azure.com) **müşterilerim** sayfasını kullanabilir. Burada hizmet sağlayıcılarına ve müşterilere başvuracağız. birden çok kiracıyı yöneten kuruluşlar, yönetim deneyimlerini birleştirmek için aynı süreci kullanabilir.

Azure portal **müşterilerimiz** sayfasına erişmek Için, **tüm hizmetler**' i seçin, ardından **müşterilerimi** arayın ve seçin. Ayrıca, Azure portal üst kısmındaki arama kutusuna "My Customers" girerek de bulabilirsiniz.

**Müşterilerimin** yalnızca, abonelikleri veya kaynak grupları temsil eden müşteriler hakkındaki bilgileri gösterdiğini aklınızda bulundurun. Diğer müşterilerle çalışıyorsanız (örneğin, [bulut çözüm sağlayıcısı programı](https://docs.microsoft.com/partner-center/csp-overview)aracılığıyla), kaynakları Temsilcili kaynak yönetimine yerleştirmediğiniz sürece burada bu müşterilerle ilgili bilgileri görmezsiniz.

> [!NOTE]
> Müşterileriniz, Azure portal hizmet **sağlayıcılarına** giderek hizmet sağlayıcılarıyla ilgili bilgileri görüntüleyebilir. Daha fazla bilgi için bkz. [hizmet sağlayıcılarını görüntüleme ve yönetme](view-manage-service-providers.md).

## <a name="view-and-manage-customer-details"></a>Müşteri ayrıntılarını görüntüleme ve yönetme

Müşteri ayrıntılarını görüntülemek için **müşterilerimin** sol tarafında bulunan **müşteriler** ' i seçin.

Her müşteri için müşterinin adını, müşteri KIMLIĞINI (kiracı KIMLIĞI) ve görevlendirmede ilişkili teklifi görürsünüz. **Temsilciler** sütununda, temsilci olan aboneliklerin sayısını ve/veya atanan kaynak grubu sayısını görürsünüz.

Sayfanın üst kısmındaki filtreler, müşteri bilgilerinizi, tekliflerini veya anahtar sözcüklere göre filtrelemenizi ve gruplamanıza olanak tanır.

Bu sayfadan aşağıdaki bilgileri görüntüleyebilirsiniz:

- Bir müşteriyle ilişkili tüm abonelikleri, teklifleri ve temsilcileri görmek için müşterinin adını seçin.
- Bir teklif ve temsilcileri hakkında daha fazla ayrıntı görmek için teklif adını seçin.
- Temsil edilen abonelikler veya kaynak grupları için yer atamaları hakkında daha fazla ayrıntı görüntülemek için **Temsilciler** sütunundaki girişi seçin.

## <a name="view-delegations"></a>Temsilcileri görüntüle

Temsilciler, atanmış olan abonelik/kaynak grubunu ve ona erişimi olan kullanıcılar ve izinlerle birlikte gösterir. Bu bilgileri görüntülemek için, **müşterilerimiz** sayfasının sol tarafındaki **temsilci** seçme ' yi seçin.

Sayfanın üst kısmındaki filtreler, erişim atama bilgilerinizi sıralamanıza ve gruplandırarak belirli müşterilere, tekliflere veya anahtar sözcüklere göre filtrelemenize izin verir.

Her bir temsilciyle ilişkili kullanıcılar ve izinler, **rol atamaları** sütununda görünür. Her bir girişi, abonelik veya kaynak grubuna erişim izni verilen kullanıcıların, grupların ve hizmet sorumlularının tam listesini görüntülemek için seçebilirsiniz. Buradan, daha fazla ayrıntı edinmek için belirli bir Kullanıcı, Grup veya hizmet asıl adı seçebilirsiniz.

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
