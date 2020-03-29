---
title: Bölge veya SKU serisi kullanılamıyor - Azure
description: Bazı SKU serileri, abonelik yönetimi destek isteği gerektirebilecek bu bölge için seçili abonelik için kullanılamaz.
services: Azure Supportability
author: stevendotwang
ms.service: azure-supportability
ms.topic: article
ms.date: 01/27/2020
ms.author: xingwan
ms.openlocfilehash: b0f0762ded6804c0b0d90a19223c082f0fb8fd49
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76843659"
---
# <a name="region-or-sku-unavailable"></a>Bölge veya SKU kullanılamıyor

Bu makalede, bir bölgeye veya VM SKU erişimi olmayan bir Azure aboneliği sorununun nasıl çözüleceği açıklanmaktadır.

## <a name="symptoms"></a>Belirtiler

Sanal bir makine dağıtırken, aşağıdaki hata iletilerinden birini alırsınız:

```
Code: SkuNotAvailable
Message: The requested size for resource '<resource>' is currently not available in location 
'<location>' zones '<zone>' for subscription '<subscriptionID>'. Please try another size or 
deploy to a different location or zones. See https://aka.ms/azureskunotavailable for details.
```

```
Message: Your subscription doesn’t support virtual machine creation in <location>. Choose a 
different location. Supported locations are <list of locations>
```

```
Code: NotAvailableForSubscription
Message: This size is currently unavailable in this location for this subscription
```

Ayrılmış Sanal Makine Örnekleri satın alırken, aşağıdaki hata iletilerinden birini alırsınız:

```
Message: Your subscription doesn’t support virtual machine reservation in <location>. Choose a 
different location. Supported locations are: <list of locations>  
```

```
Message: This size is currently unavailable in this location for this subscription
```

Bilgi işlem çekirdek kotasını artırmak için bir destek isteği oluştururken, bir bölge veya SKU ailesi seçim için kullanılamaz.

## <a name="solution"></a>Çözüm

Öncelikle iş ihtiyaçlarınızı karşılayan alternatif bir bölge veya SKU düşünmenizi öneririz.

Uygun bir bölge veya SKU bulamıyorsanız, aşağıdaki adımları izleyerek bir **Abonelik yönetimi** [destek isteği](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest) oluşturun:

1. Azure [portalı](https://portal.azure.com) menüsünden **Yardım + destek'i**seçin. Ardından **Yeni destek isteğini**seçin.

1. **Temel**Bilgiler'de, **Sorun türü**için **Abonelik yönetimini**seçin.

1. **Abonelik** seçin ve **Özet'e**kısa bir açıklama girin.

   ![Yeni destek isteğinin temelleri sekmesi](./media/SKU-series-unavailable/support-request-basics.png)

1. **Sorun türü için,** **sorun türünü seç'i**seçin.

1. **Sorun türünü seçin,** bir seçenek seçin, örneğin, **Aboneliğime veya kaynağıma** > erişemiyor**benim sorunum yukarıda listelenmez.** **Kaydet'i**seçin.

   ![İstek için bir sorun belirtin](./media/SKU-series-unavailable/support-request-select-problem-type.png)

1. **İleri'yi seçin:** Olası çözümleri keşfetmek için çözümler. Gerekirse, **İleri:Devam** edecek ayrıntılar'ı seçin.

1. İletişim bilgilerinizle birlikte sağlayabileceğiniz ek bilgileri girin.

1. **İncele ve oluştur**’u seçin. Bilgilerinizi doğruladıktan sonra, isteği oluşturmak için **Oluştur'u** seçin.

## <a name="send-us-your-suggestions"></a>Önerilerinizi bize gönderin

Biz her zaman geribildirim ve önerilere açığız! [Önerilerinizi](https://feedback.azure.com/forums/266794-support-feedback)bize gönderin. Ayrıca, [Bizimle Twitter](https://twitter.com/azuresupport) veya [MSDN forumlarında](https://social.msdn.microsoft.com/Forums/azure)etkileşimkurabilirsiniz.

## <a name="learn-more"></a>Daha fazla bilgi edinin

[Azure Destek SSS](https://azure.microsoft.com/support/faq)
