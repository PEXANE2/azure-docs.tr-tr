---
title: Pazar yeri ölçüm hizmeti API'leri - SSS | Azure Marketi
description: Azure Marketi'nde Bir SaaS teklifinin kullanımını yamit.
author: dsindona
ms.author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 07/11/2019
ms.openlocfilehash: 6e5b691a41ef283449f9eeeb90e9d01a91616146
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80275790"
---
# <a name="marketplace-metering-service-apis---faq"></a>Market ölçüm hizmeti API’leri - SSS

Bir Azure kullanıcısı tarifeli faturalandırma içeren bir SaaS hizmetine abone olduktan sonra, müşteri tarafından kullanılan her faturalandırma boyutu için tüketimi izlersiniz. Tüketim, müşteri tarafından seçilen dönem için ayarlanan dahil edilen miktarları aşarsa, hizmetiniz kullanım olaylarını Microsoft'a yayacaktır.

## <a name="emit-usage-events"></a>Kullanım olaylarını yaslar

>[!Note]
>Bu bölüm yalnızca, planlardan en az birinin teklifin yayımlandığı sırada tanımlanan ölçüm hizmet boyutlarına sahip olduğu SaaS teklifleri için geçerlidir.

![Kullanım olaylarını yaslar](media/isv-emits-usage-event.png)

Kullanım olayları yayan API sözleşmesi hakkında bilgi için [SaaS toplu iş etkinliği API'ye](./marketplace-metering-service-apis.md#batch-usage-event) bakın.

### <a name="how-often-is-it-expected-to-emit-usage"></a>Ne sıklıkta kullanım yontmak için bekleniyor?

İdeal olarak, yalnızca önceki saat içinde kullanım varsa, son bir saat için her saat kullanım yontmak için bekleniyor.

### <a name="what-is-the-maximum-delay-between-the-time-an-event-occurs-and-the-time-a-usage-event-is-emitted-to-microsoft"></a>Bir olayın oluştuğu saat ile bir kullanım olayının Microsoft'a yayDığı saat arasındaki en büyük gecikme nedir?

İdeal olarak, kullanım olayı son bir saat içinde meydana gelen olaylar için her saat yayılan. Ancak gecikmeler bekleniyor. İzin verilen maksimum gecikme 24 saattir ve bundan sonra kullanım olayları kabul edilmez.

Örneğin, bir kullanım olayı günde saat 13:00'te gerçekleşirse, bu olayla ilişkili bir kullanım olayı yontmak için bir sonraki gün saat 13:00'e kadar vaktiniz vardır. Bu, sistemin kullanım süresinin azalması durumunda, kullanım olayını kurtarabileceği ve kullanım olayını, sadakat kaybı olmaksızın, kullanım olayının gerçekleştiği saat aralığı na gönderebileceği anlamına gelir.

### <a name="what-happens-when-you-send-more-than-one-usage-event-on-the-same-hour"></a>Aynı saatte birden fazla kullanım olayı gönderirseniz ne olur?

Saat aralığı için yalnızca bir kullanım olayı kabul edilir. Saat aralığı 0.  Aynı saat aralığı için birden fazla kullanım olayı yayımlanırsa, sonraki kullanım olayları yineolarak bırakılır.

### <a name="what-happens-when-you-emit-usage-for-a-saas-subscription-that-has-been-unsubscribed-already"></a>Aboneliği iptal edilmiş bir SaaS aboneliği için kullanım yayırsanız ne olur?

SaaS aboneliği silindikten sonra pazar platformuna yayılan herhangi bir kullanım olayı kabul edilmez.

### <a name="can-you-get-a-list-of-all-saas-subscriptions-including-active-and-unsubscribed-subscriptions"></a>Etkin ve abonelikten çıkarılan abonelikler de dahil olmak üzere tüm SaaS aboneliklerinin listesini alabilir misiniz?

Evet, API'yi `GET /saas/subscriptions` aradiğinizde, tüm SaaS aboneliklerinin bir listesini içerir. Her SaaS aboneliğiiçin yanıttaki durum alanı, aboneliğin etkin olup olmadığını veya aboneliğinin iptal edilip edilemeyeceğini yakalar. Abonelikleri listele meçilesi, o anda en fazla 100 abonelik döndürür.

## <a name="next-steps"></a>Sonraki adımlar

- Daha fazla bilgi için [Market ölçüm hizmeti API'leri'ne](./marketplace-metering-service-apis.md) bakın.
