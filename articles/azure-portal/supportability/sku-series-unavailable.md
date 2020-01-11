---
title: SKU serisi kullanılamıyor | Microsoft Docs
description: Bu bölge için seçili abonelik için bazı SKU serisi kullanılamıyor.
services: Azure Supportability
author: stevendotwang
ms.service: azure-supportability
ms.topic: article
ms.date: 11/09/2017
ms.author: xingwan
ms.openlocfilehash: e317ae1ad88cf162f1d55a06d19e7b3b0b88ce60
ms.sourcegitcommit: 8e9a6972196c5a752e9a0d021b715ca3b20a928f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/11/2020
ms.locfileid: "75896733"
---
# <a name="region-or-sku-unavailable"></a>Bölge veya SKU kullanılamıyor
Bu makalede, bir Azure aboneliği 'nin bir bölge veya VM SKU 'SU erişimi olmadığı sorununu çözme konusu açıklanır.

## <a name="symptoms"></a>Belirtiler

### <a name="when-deploying-a-virtual-machine-you-receive-one-of-the-following-error-messages"></a>Bir sanal makineyi dağıttığınızda, aşağıdaki hata iletilerinden birini alırsınız:
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

### <a name="when-purchasing-reserved-virtual-machine-instances-you-receive-one-of-the-following-error-messages"></a>Ayrılmış sanal makine örnekleri satın alırken, aşağıdaki hata iletilerinden birini alırsınız:

```
Message: Your subscription doesn’t support virtual machine reservation in <location>. Choose a 
different location. Supported locations are: <list of locations>  
```

```
Message: This size is currently unavailable in this location for this subscription
```

### <a name="when-creating-a-support-request-to-increase-compute-core-quota-a-region-or-a-sku-family-is-not-available-for-selection"></a>İşlem çekirdek kotasını artırmak için bir destek isteği oluştururken, bir bölge veya SKU ailesi seçim için kullanılamaz.

## <a name="solution"></a>Çözüm
Önce, iş gereksinimlerinizi karşılayan alternatif bir bölgeyi veya SKU 'YU düşünmeniz önerilir. Uygun bir bölge veya SKU bulamıyorsanız aşağıdaki adımları izleyerek bir "Abonelik Yönetimi" [destek isteği](https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest) oluşturun:


- Temel bilgiler sayfasında, sorun türü ' nü "Abonelik Yönetimi" olarak seçin, aboneliği seçin ve "Ileri" ye tıklayın.

![Temel Bilgiler dikey penceresi](./media/SKU-series-unavailable/BasicsSubMgmt.png)


-   Sorun sayfasında, sorun türünü "diğer genel sorular" olarak seçin.
- Ayrıntılar bölümünde:
  - Lütfen sanal makineler dağıtmayı veya ayrılmış sanal makine örneklerini satın almayı düşünüyorsanız belirtin
  - Lütfen dağıtmayı veya satın almayı planladığınız sanal makine örneklerinin bölge, SKU 'SU ve sayısını belirtin


![Sorun](./media/SKU-series-unavailable/ProblemSubMgmt.png)

-   İletişim ayrıntılarınızı girin ve "Oluştur" a tıklayın.

![İletişim Bilgileri](./media/SKU-series-unavailable/ContactInformation.png)

## <a name="feedback"></a>Geri Bildirim
Geri bildirim ve önerilere her zaman açıktır! [Önerilerinizi](https://feedback.azure.com/forums/266794-support-feedback)bize gönderin. Ayrıca, [Twitter](https://twitter.com/azuresupport) veya [MSDN forumları](https://social.msdn.microsoft.com/Forums/azure)aracılığıyla bizimle iletişim sağlayabilirsiniz.

## <a name="learn-more"></a>Daha fazla bilgi
[Azure desteği SSS](https://azure.microsoft.com/support/faq)

