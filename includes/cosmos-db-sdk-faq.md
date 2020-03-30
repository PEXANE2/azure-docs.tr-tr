---
author: SnehaGunda
ms.service: cosmos-db
ms.topic: include
ms.date: 11/09/2018
ms.author: sngun
ms.openlocfilehash: 99dddd86c9348c9791d3012b382298bb020e63c9
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "67188122"
---
**1. Müşteriler emekli olan SDK'dan nasıl haberdar edilecektir?**

Microsoft, desteklenen bir SDK'ya sorunsuz bir geçişi kolaylaştırmak için emekli olan SDK'nın desteğinin sonuna 12 aylık avans bildirimi sağlayacaktır. Ayrıca, müşteriler çeşitli iletişim kanalları aracılığıyla bilgilendirilecektir – Azure Yönetim Portalı, Geliştirici Merkezi, blog yazısı ve atanan hizmet yöneticilerine doğrudan iletişim.

**2. Müşteriler 12 aylık dönemde "olacak" emekli azure cosmos DB SDK kullanarak uygulamaları yazabilir mi?** 

Evet, müşteriler 12 aylık yetkisiz kullanım süresi boyunca "hazır" emekli Azure Cosmos DB SDK'yı kullanarak uygulamaları yazara, dağıtmaya ve değiştirebilmek için tam erişime sahip olacaktır. 12 aylık yetkisiz kullanım süresi boyunca, müşterilerin Azure Cosmos DB SDK'nın daha yeni desteklenen sürümüne uygun şekilde geçiş etmeleri önerilir.

**3. Müşteriler, 12 aylık bildirim döneminden sonra emekli bir Azure Cosmos DB SDK kullanarak uygulamaları yazar ve değiştirebilir mi?**

12 aylık bildirim süresinden sonra SDK emekliye ayrıldı. Emekli bir SDK kullanan bir uygulama tarafından Azure Cosmos DB'ye herhangi bir erişime Azure Cosmos DB platformu tarafından izin verilmez. Ayrıca, Microsoft emekli SDK'da müşteri desteği sağlamaz.

**4. Desteklenmeyen Azure Cosmos DB SDK sürümünü kullanan Müşterinin çalışan uygulamalarına ne olur?**

Azure Cosmos DB hizmetine emekli bir SDK sürümüyle bağlanmaya yönelik tüm girişimler reddedilir. 

**5. Emekli olmayan tüm SDK'lara yeni özellikler ve işlevler uygulanacak mı?**

Yeni özellikler ve işlevler yalnızca yeni sürümlere eklenir. SDK'nın eski, kullanımdan kaldırılmış bir sürümünü kullanıyorsanız, Azure Cosmos DB'ye istekleriniz önceki gibi çalışmaya devam eder, ancak yeni özelliklere erişmezsiniz.  

**6. Başvurumu bir kesme tarihinden önce güncelleyemezsem ne yapmalıyım?**

Mümkün olduğunca erken en son SDK'ya yükseltmenizi öneririz. Emeklilik için bir SDK etiketlendikten sonra başvurunuzu güncellemek için 12 ayınız olacaktır. Herhangi bir nedenle, bu zaman dilimi içinde uygulama güncellemenizi tamamlayamıyorsanız, lütfen [Cosmos DB Ekibi'ne](mailto:askcosmosdb@microsoft.com) başvurun ve kesme tarihinden önce yardım isteyin.

