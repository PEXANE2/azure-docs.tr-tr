---
title: include dosyası
description: include dosyası
services: redis-cache
author: wesmc7777
ms.service: cache
ms.topic: include
ms.date: 11/05/2019
ms.author: wesmc
ms.custom: include file
ms.openlocfilehash: 3c064aa8e57a77b96161da06847f543816be1217
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "73719104"
---
1. Önbellek oluşturmak için [Azure portalında](https://portal.azure.com) oturum açın ve **kaynak oluştur'u**seçin. 
   
   ![Kaynak Oluştur'u seçin](media/redis-cache-create/create-a-resource.png)
   
1. **Yeni** **sayfada, Veritabanları'nı** seçin ve **ardından Redis için Azure Önbelleği'ni**seçin.
   
   ![Redis için Azure Önbelleğini seçin](media/redis-cache-create/redis-cache-new-cache-menu.png)
   
1. Yeni **Redis Önbellek** sayfasında, yeni önbelleğinizin ayarlarını yapılandırın.
   
   | Ayar      | Önerilen değer  | Açıklama |
   | ------------ |  ------- | -------------------------------------------------- |
   | **DNS adı** | Genel olarak benzersiz bir ad girin. | Önbellek adı, yalnızca sayılar, harfler veya tireler içeren 1 ile 63 karakter arasında bir dize olmalıdır. Ad bir sayı veya harfle başlayıp bitmelidir ve ardışık tireler içeremez. Önbellek örneğinizin *ana bilgisayar adı* * \<DNS adı>.redis.cache.windows.net*olacaktır. | 
   | **Abonelik** | Aşağı inin ve aboneliğinizi seçin. | Redis örneği için bu yeni Azure Önbelleğini oluşturmak için altında abonelik. | 
   | **Kaynak grubu** | Aşağı inip bir kaynak grubu seçin veya **yeni oluştur'u** seçin ve yeni bir kaynak grubu adı girin. | Önbelleğinizi ve diğer kaynakları oluşturmak için kaynak grubunun adını ada. Tüm uygulama kaynaklarınızı tek bir kaynak grubuna koyarak, bunları kolayca yönetebilir veya birlikte silebilirsiniz. | 
   | **Konum** | Aşağı inin ve bir konum seçin. | Önbelleğinizi kullanacak diğer hizmetlere yakın bir [bölge](https://azure.microsoft.com/regions/) seçin. |
   | **Fiyatlandırma katmanı** | Aşağı inin ve bir [Fiyatlandırma katmanı](https://azure.microsoft.com/pricing/details/cache/)seçin. |  Fiyatlandırma katmanı önbellek için kullanılabilen boyut, performans ve özellikleri belirler. Daha fazla bilgi [için Redis Genel Bakış için Azure Önbelleği'ne](../articles/azure-cache-for-redis/cache-overview.md)bakın. |
   
1. **Oluştur'u**seçin. 
   
   ![Redis için Azure Önbelleği Oluşturma](media/redis-cache-create/redis-cache-cache-create.png) 
   
   Önbelleğin oluşturması biraz zaman alır. Redis **Genel Bakış** sayfasındaki Azure Önbelleği'ndeki ilerlemeyi izleyebilirsiniz. **Durum** **Çalışan**olarak gösterdiğinde, önbellek kullanıma hazırdır.
   
   ![Redis için Azure Önbelleği oluşturuldu](media/redis-cache-create/redis-cache-cache-created.png)

