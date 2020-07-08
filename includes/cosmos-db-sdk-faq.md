---
author: SnehaGunda
ms.service: cosmos-db
ms.topic: include
ms.date: 11/09/2018
ms.author: sngun
ms.openlocfilehash: 99dddd86c9348c9791d3012b382298bb020e63c9
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "67188122"
---
**1. kullanıcılara devre dışı bırakma SDK 'sının nasıl bildirilmesi gerekir?**

Microsoft, desteklenen bir SDK 'ya sorunsuz bir geçiş yapılmasını kolaylaştırmak için devre dışı bırakma SDK 'sının destek sonuna 12 aylık öncelikli bildirim sağlayacaktır. Ayrıca, müşteriler çeşitli iletişim kanalları (Azure Yönetim Portalı, Geliştirici Merkezi, blog gönderisi ve atanan hizmet yöneticilerine doğrudan iletişim aracılığıyla bilgilendirilir.

**2. müşteriler 12 aylık dönemde "yapılacak" Kullanımdan kaldırılmış Azure Cosmos DB SDK kullanarak uygulamalar yazar mı?** 

Evet, müşteriler 12 ay yetkisiz kullanım süresi boyunca "yapılacak" Kullanımdan kaldırılmış Azure Cosmos DB SDK 'sını kullanarak, uygulamaları yazmak, dağıtmak ve değiştirmek için tam erişime sahip olur. 12 aylık kullanım süresi boyunca, müşterilerin uygun şekilde Azure Cosmos DB SDK 'nın daha yeni bir sürümüne geçirilmesi önerilir.

**3. müşteriler, 12 aylık bildirim süresinden sonra kullanımdan kaldırılan Azure Cosmos DB SDK kullanarak uygulamalar yazabilir ve değiştirebilir mi?**

12 aylık bildirim süresinden sonra SDK kullanımdan kaldırılacak. Kullanımdan kaldırılan bir SDK kullanan uygulamalar tarafından Azure Cosmos DB erişimine Azure Cosmos DB platformu izin verilmez. Ayrıca, Microsoft, kullanımdan kaldırılan SDK 'da müşteri desteği sağlamacaktır.

**4. müşterilerin desteklenmeyen Azure Cosmos DB SDK sürümü kullanan çalışan uygulamaları ne olur?**

Kullanımdan kaldırılan bir SDK sürümüyle Azure Cosmos DB hizmetine bağlanma girişimleri reddedilir. 

**5. yeni özellikler ve işlevler, kullanımdan kaldırılmayan tüm SDK 'lara uygulanacak mi?**

Yeni özellikler ve işlevler yalnızca yeni sürümlere eklenecektir. SDK 'nın eski, kullanımdan kaldırılmayan bir sürümünü kullanıyorsanız, Azure Cosmos DB için istekleriniz daha önce de çalışır, ancak yeni yetenekler için erişiminiz olmayacaktır.  

**6. uygulamamı bir kesme tarihinden önce güncelleştirediğimde ne yapmam gerekir?**

En son SDK 'ya mümkün olduğunca erken yükseltmeniz önerilir. Devre dışı bırakma için bir SDK etiketledikten sonra uygulamanızı güncellemek için 12 ay sahip olursunuz. Her nedenden dolayı, uygulama güncelleştirmenizi bu zaman diliminde tamamlamadıysanız, lütfen [Cosmos DB ekibine](mailto:askcosmosdb@microsoft.com) başvurarak, kesme tarihinden önce yardım isteyin.

