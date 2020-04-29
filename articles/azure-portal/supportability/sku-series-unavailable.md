---
title: Bölge veya SKU serisi kullanılamıyor-Azure
description: Bu bölge için seçili abonelik için bazı SKU serisi kullanılamıyor, bu durum abonelik yönetimi destek isteği gerektirebilir.
services: Azure Supportability
author: stevendotwang
ms.service: azure-supportability
ms.topic: article
ms.date: 01/27/2020
ms.author: xingwan
ms.openlocfilehash: b0f0762ded6804c0b0d90a19223c082f0fb8fd49
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "76843659"
---
# <a name="region-or-sku-unavailable"></a>Bölge veya SKU kullanılamıyor

Bu makalede, bir Azure aboneliği 'nin bir bölge veya VM SKU 'SU erişimi olmadığı sorununu çözme konusu açıklanır.

## <a name="symptoms"></a>Belirtiler

Bir sanal makineyi dağıttığınızda, aşağıdaki hata iletilerinden birini alırsınız:

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

Ayrılmış sanal makine örnekleri satın alırken, aşağıdaki hata iletilerinden birini alırsınız:

```
Message: Your subscription doesn’t support virtual machine reservation in <location>. Choose a 
different location. Supported locations are: <list of locations>  
```

```
Message: This size is currently unavailable in this location for this subscription
```

İşlem çekirdek kotasını artırmak için bir destek isteği oluştururken, bir bölge veya SKU ailesi seçim için kullanılamaz.

## <a name="solution"></a>Çözüm

Önce, iş gereksinimlerinizi karşılayan alternatif bir bölgeyi veya SKU 'YU düşünmeniz önerilir.

Uygun bir bölge veya SKU bulamıyorsanız, şu adımları izleyerek bir **Abonelik yönetimi** [destek isteği](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest) oluşturun:

1. [Azure Portal](https://portal.azure.com) menüsünde **Yardım + Destek**' i seçin. Ardından **Yeni destek isteği**' ni seçin.

1. **Temel bilgiler**Için, **sorun türü**için **Abonelik yönetimi**' ni seçin.

1. Bir **abonelik** seçin ve **Özet**' de kısa bir açıklama girin.

   ![Yeni destek isteğinin temelleri sekmesi](./media/SKU-series-unavailable/support-request-basics.png)

1. **Sorun türü**için **sorun türünü seç**' i seçin.

1. **Sorun türü seç**için bir seçenek belirleyin, örneğin, **aboneliğime veya kaynak** > sorunumu**Yukarıda listelenmeyen**bir seçeneğe tıklayın. **Kaydet**’i seçin.

   ![İstek için bir sorun belirtin](./media/SKU-series-unavailable/support-request-select-problem-type.png)

1. **İleri ' yi seçin:** olası çözümleri keşfetmeye yönelik çözümler. Gerekirse, Ileri ' yi seçin: devam etmek için **Ayrıntılar** .

1. İletişim bilgileriniz ile birlikte sağlayabilmeniz için ek bilgileri girin.

1. **İncele ve oluştur**’u seçin. Bilgilerinizi doğruladıktan sonra, isteği oluşturmak için **Oluştur** ' u seçin.

## <a name="send-us-your-suggestions"></a>Önerilerinizi bize gönderin

Her zaman geri bildirim ve önerilerle karşılaşıyoruz! [Önerilerinizi](https://feedback.azure.com/forums/266794-support-feedback)bize gönderin. Ayrıca, [Twitter](https://twitter.com/azuresupport) veya [MSDN forumlarında](https://social.msdn.microsoft.com/Forums/azure)bizimle iletişim sağlayabilirsiniz.

## <a name="learn-more"></a>Daha fazla bilgi edinin

[Azure desteği SSS](https://azure.microsoft.com/support/faq)
