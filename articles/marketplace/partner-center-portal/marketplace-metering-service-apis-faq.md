---
title: Ölçüm hizmeti API 'Leri SSS-Microsoft ticari Market
description: Microsoft AppSource ve Azure Market 'te SaaS teklifleri için ölçüm hizmeti API 'Leri hakkında sık sorulan sorular.
author: dsindona
ms.author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 06/01/2020
ms.openlocfilehash: 6c1cc8c08fa4dc09d033cf4752a81f01419e8f2c
ms.sourcegitcommit: 537c539344ee44b07862f317d453267f2b7b2ca6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/11/2020
ms.locfileid: "84694935"
---
# <a name="marketplace-metered-billing-apis---faq"></a>Market ölçülen faturalandırma API 'Leri-SSS

Müşteri bir SaaS hizmetine abone olduktan veya yönetilen uygulamalar planına sahip Azure uygulamasına, tarifeli faturalandırma ile, kullanılan her faturalandırma boyutu için tüketimi takip edersiniz.  Tüketim, müşteri tarafından seçilen dönem için ayarlanan dahil edilen miktarları aşarsa, hizmetiniz kullanım olaylarını Microsoft 'a yayacaktır.

## <a name="for-both-saas-offers-and-managed-apps"></a>SaaS teklifleri ve yönetilen uygulamalar için

### <a name="how-often-is-it-expected-to-emit-usage"></a>Ne sıklıkta kullanım yayma bekleniyor?

İdeal olarak, yalnızca önceki saat içinde kullanım için kullanım süresi boyunca, son saat için kullanımı her saat için bir süre yayma bekliyorduk.

### <a name="is-there-a-maximal-period-between-one-emission-and-the-next-one"></a>Bir duyun ve sonraki bir değer arasında bir büyük dönem var mı?

Böyle bir sınırlama yoktur. Yalnızca kullanım sırasında ortaya çıkar. Örneğin, abonelik ömrü başına yalnızca bir kullanım birimi göndermeniz gerekiyorsa, bunu yapabilirsiniz.

### <a name="what-is-the-maximum-delay-between-the-time-an-event-occurs-and-the-time-a-usage-event-is-emitted-to-microsoft"></a>Bir olayın gerçekleştiği zaman arasındaki en uzun gecikme ve kullanım olayının Microsoft 'a nasıl yayıldığını nelerdir?

En ideal olarak, kullanım olayı, son saat içinde gerçekleşen olaylar için saatte bir yayınlanır. Ancak gecikme beklenmektedir. İzin verilen en fazla gecikme 24 saattir, bu da kullanım olaylarının kabul edilmemesini sağlayacak. En iyi uygulama saatlik kullanım toplamaktır ve bu süre sonunda tek bir olay olarak oluşturulur.

Örneğin, bir gün içinde kullanım olayı 1 PM 'de gerçekleşirse, bu olayla ilişkili kullanım olayını oluşturmak için bir sonraki gün 1 PM 'ye kadar olur.  Sistem yayma kullanımının düşük olması durumunda, bu durum, kullanım gerçekleştiği zaman aralığı için kullanım olayını kurtarabilir ve daha sonra bir uygunluk kaybı olmadan gönderebilir.

Gerçek kullanımdan sonra 24 saat geçmişse, daha sonraki kullanım olaylarıyla tüketilen birimleri de yayabilirsiniz.  Ancak, bu uygulama, son müşteri için faturalandırma olayı raporlarının güvenilirlik düzeyini etkileyebilir.  Bir gün/hafta/ay boyunca ölçüm e-görevi gönderilmesini önlemenize önerilir.  Müşterinin gerçek kullanımını anlamak ve kullanım olaylarıyla ilgili olarak ortaya çıkan sorunları veya soruları çözümlemek daha zor olacaktır.

Her saat kullanım göndermenin başka bir nedeni de, Yayımcı günlük/haftalık/aylık egörev olayı göndermeden önce kullanıcının aboneliği iptal ettiğini önmaktır.

### <a name="what-happens-when-you-send-more-than-one-usage-event-in-the-same-hour"></a>Aynı saat içinde birden fazla kullanım olayı gönderdiğinizde ne olur?

Tek saatlik Aralık için yalnızca bir kullanım olayı kabul edilir. Saat aralığı 0 ' dan başlar ve dakika 59 ' de sona erer.  Aynı saat için birden fazla kullanım olayı yayıldıysanız, sonraki kullanım olayları yineleme olarak bırakılır.

### <a name="what-happens-when-the-customer-cancels-the-purchase-within-the-time-allowed-by-the-cancellation-policy"></a>Müşteri, iptal ilkesi tarafından izin verilen süre içinde satınalmayı iptal ettiğinde ne olur?

Sabit fiyat tutarı ücretlendirilmeyecektir ancak fazla kullanım kullanımı olur.

### <a name="can-custom-meter-plans-be-used-for-one-time-payments"></a>Özel ölçüm planları, tek seferlik ödemeler için kullanılabilir mi?

Evet, bir tek seferlik ödeme birimi olarak özel bir boyut tanımlayabilir ve her müşteri için yalnızca bir kez yayabilirsiniz.

### <a name="can-custom-meter-plans-be-used-to-tiered-pricing-model"></a>Fiyatlandırma modelinin katmanlı olması için özel ölçüm planları kullanılabilir mi?

Evet, tek bir fiyat katmanını temsil eden her özel boyut için uygulanabilir.

Örneğin, contoso, ilk 1000 e-postalar için e-posta başına $0,5 $0,4, 1000 ve 5000 e-postalar arasında her e-posta için ve yukarıdaki $0,2 e-postaları için e-posta başına Üç e-posta fiyatlandırma katmanına karşılık gelen üç özel boyut tanımlayabilirler. İlk boyutun birim sayısını, e-posta sayısı 1000 ' ün altında kaldığında, son e-posta sayısı 1000 ve 5000 ve son olarak, 5000 e-postaları için üçüncü boyutun birimleri arasında olduğunda, İkinci boyutun birimleri.

### <a name="what-happens-if-the-marketplace-metering-service-has-an-outage"></a>Market ölçüm hizmeti bir kesinti yaşmışsa ne olur?

ISV özel bir ölçüm gönderiyor ve bir hata alırsa (genellikle benzer olaylar hata olmadan önce kabul edildiğinde), ISV 'nin beklemesi ve egörevi yeniden denemesi gerekir.

Hata devam ederse, bu özel ölçümü bir sonraki saat (miktarı biriktir) olarak yeniden gönderin. Hata olmayan bir yanıt alınana kadar bu işleme devam edin.

## <a name="for-saas-offers-only"></a>Yalnızca SaaS teklifleri için

### <a name="what-happens-when-you-emit-usage-for-a-saas-subscription-that-has-been-unsubscribed-already"></a>Zaten abone olunan bir SaaS aboneliği için kullanım yaydığınızda ne olur?

Bir SaaS aboneliği silindikten sonra Market platformuna yayılan kullanım olayları kabul edilmez.

Kullanım yalnızca abone durumundaki abonelikler için (, veya içindeki abonelikler için değil `PendingFulfillmentStart` `Suspended` ) kullanılabilir `Unsubscribed` .

Tek özel durum, SaaS aboneliğinin iptalinden önceki süre için raporlama kullanımdır.

Örneğin, müşteri SaaS aboneliğini bugün 3:12 ' de iptal etti. 5 pm, Yayımcı, bu SaaS aboneliği için bugün 6 ila dün ile 3 pm arasındaki süreyi kullanım süresini yayabilir.

### <a name="can-you-get-a-list-of-all-saas-subscriptions-including-active-and-unsubscribed-subscriptions"></a>Etkin ve aboneliği kaldırma abonelikleri dahil olmak üzere tüm SaaS aboneliklerinin bir listesini alabilir misiniz?

Evet, tüm SaaS aboneliklerinin bir listesini içerdiği için [abonelikleri al LISTE API](https://docs.microsoft.com/azure/marketplace/partner-center-portal/pc-saas-fulfillment-api-v2#subscription-api) 'sini çağırdığınızda. Her bir SaaS aboneliğine yönelik yanıttaki durum alanı, aboneliğin etkin veya aboneliği kaldırma olduğunu yakalar.

### <a name="are-the-start-and-end-dates-of-saas-subscription-term-and-overage-usage-emission-connected"></a>SaaS Abonelik döneminin başlangıç ve bitiş tarihleri ve fazla kullanım kullanımı emismidir?

Fazla kullanım olayları, *abone olunmuş* durumdaki mevcut SaaS aboneliği için herhangi bir zamanda kullanılabilir. Bu, yayımcı planında tanımlanan ilkeye göre kullanım olaylarını yayma yayımcısının sorumluluğundadır. Fazla kullanım, SaaS aboneliğinin teriminde tanımlanan tarihlere göre hesaplanmalıdır. 

Örneğin, yayımcı aylık düz hızda $100 için 1000 e-postaları içeren bir SaaS planı tanımlıyorsa, 1000 yukarıdaki her e-posta, özel boyut aracılığıyla $1 olarak faturalandırılır.

Müşteri aboneliği satın alır ve 6 Ocak tarihinde etkinleşdiğinde, sabit fiyata dahil edilen 1000 e-postası bu günde bir başlangıç olarak sayılır. Bu nedenle, 5 Şubat ayının (aboneliğin ilk ayının sonuna kadar) yalnızca 900 e-postaların gönderilmesi halinde müşteri, bu aboneliğin yalnızca ilk ayında sabit fiyat üzerinden ödeme yapar ve fazla kullanım olayı yayımcı tarafından 6 Ocak ve 5 Şubat arasında yayılır. 6 Şubat 'ta abonelik otomatik olarak yenilenir ve sayım yeniden başlatılır. 15 Şubat 'ta müşteriye ulaşılan 1000 e-posta gönderilir. 5 Mart 'a kadar gönderilen e-postaların geri kalanı, yayımcı tarafından oluşturulan fazla kullanım olaylarına göre fazla kullanım (e-posta başına $1) olarak ücretlendirilecektir.

## <a name="next-steps"></a>Sonraki adımlar

- Daha fazla bilgi için bkz. [Market ölçüm hizmeti API 'leri](./marketplace-metering-service-apis.md).
