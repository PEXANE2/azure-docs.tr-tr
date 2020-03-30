---
title: Azure Fonksiyonları coğrafi felaket kurtarma ve yüksek kullanılabilirlik
description: Artıklık için coğrafi bölgeleri kullanma ve Azure İşlevlerinde başarısız olması.
ms.assetid: 9058fb2f-8a93-4036-a921-97a0772f503c
ms.topic: conceptual
ms.date: 08/29/2019
ms.openlocfilehash: 481a716bd6ced5c304da41c70fdcfc687b76661d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79080238"
---
# <a name="azure-functions-geo-disaster-recovery"></a>Azure Fonksiyonları coğrafi felaket kurtarma

Tüm Azure bölgeleri veya veri merkezleri kapalı kalma süresi yle karşılaştığında, bilgi işlemin farklı bir bölgede işlemeye devam etmesi çok önemlidir.  Bu makalede, olağanüstü durum kurtarma için izin vermek için işlevleri dağıtmak için kullanabileceğiniz bazı stratejiler açıklanacaktır.

## <a name="basic-concepts"></a>Temel kavramlar

Azure İşlevler belirli bir bölgede çalışır.  Daha yüksek kullanılabilirlik elde etmek için, aynı işlevleri birden çok bölgeye dağıtabilirsiniz.  Birden çok bölgede işlevleriniz *etkin/etkin* desen veya *etkin/pasif* desende çalışıyor olabilir.  

* Etkin/etkin. Her iki bölge de etkin ve olay alıyor (yinelenen veya dönüşümlü olarak). Azure Ön Kapı ile birlikte HTTPS işlevleri için etkin/etkin önerilir.
* Aktif/pasif. Bir bölge etkin ve olay alırken, ikincil bir bölge boşta.  Başarısız olmak gerektiğinde, ikincil bölge etkinleştirilir ve işlemeyi devralır.  Bu, Servis Veri Servisi ve Olay Hub'ları gibi HTTP olmayan işlevler için önerilir.

Çok bölgeli dağıtımlar hakkında daha fazla bilgi için birden çok bölgede uygulamaların nasıl [çalıştırılabildiğini](https://docs.microsoft.com/azure/architecture/reference-architectures/app-service-web-app/multi-region) okuyun.

## <a name="activeactive-for-https-functions"></a>HTTPS işlevleri için etkin/etkin

Işlevlerin etkin/etkin dağıtımlarını elde etmek için, her iki bölge arasındaki olayları koordine edebilen bazı bileşen gerektirir.  HTTPS işlevleri için bu koordinasyon [Azure Ön Kapı](../frontdoor/front-door-overview.md)kullanılarak gerçekleştirilir.  Azure Ön Kapı, birden çok bölgesel işlev arasında https isteklerini yönlendirebilir ve yuvarlayabilir.  Ayrıca periyodik olarak her bitiş noktasının durumunu denetler.  Bölgesel bir işlev sistem durumu denetimlerine yanıt vermeyi durdurursa, Azure Ön Kapı bu işlevi döndürme dışına çıkarır ve trafiği yalnızca sağlıklı işlevlere iletir.  

![Azure Ön Kapı ve İşlev Için Mimari](media/functions-geo-dr/front-door.png)  

## <a name="activeactive-for-non-https-functions"></a>HTTPS olmayan işlevler için etkin/etkin

YINE de HTTPS olmayan işlevler için etkin/etkin dağıtımlar elde edebilirsiniz.  Ancak, iki bölgenin birbirleriyle nasıl etkileşimde bulunacağını veya eşgüdüm kuracağını göz önünde bulundurmanız gerekir.  Aynı işlev uygulamasını her biri aynı Hizmet Veri Servisi kuyruğunda tetikleyen iki bölgeye dağıtırsanız, bu sırayı iptal etmede rakip tüketiciler olarak hareket ederler.  Bu, her iletinin yalnızca örneklerden biri tarafından işlendiği anlamına gelir, ancak tek Hizmet Veri Yolu'nda hala tek bir hata noktası olduğu anlamına gelir.  İki Servis Veri Servisi kuyruğu (biri birincil bölgede, biri ikincil bölgede) dağıtırsanız ve iki işlev uygulaması bölge sırasını işaret ederse, şimdi sıra iletilerinin iki bölge arasında nasıl dağıtıldığı yla ilgili sorun gelir.  Genellikle bu, her yayımcının *her iki* bölgeye de bir ileti yayımlamaya çalıştığı ve her iletinin her iki etkin işlev uygulaması tarafından işlendiği anlamına gelir.  Bu, etkin/etkin bir desen oluştururken, bilgi işlem yinelemesi ve verilerin ne zaman ve nasıl biraraya gelinip birleştirilmesi yle ilgili başka zorluklar da oluşturur.  Bu nedenlerden dolayı, https olmayan tetikleyicilerin etkin/pasif deseni kullanması önerilir.

## <a name="activepassive-for-non-https-functions"></a>HTTPS olmayan işlevler için etkin/pasif

Etkin/pasif, yalnızca tek bir işlevin her iletiyi işlemesi için bir yol sağlar, ancak bir felaket durumunda ikincil bir bölgeye geçememesi için bir mekanizma sağlar.  Azure İşgünişleri, [Azure Hizmet Veri Yolu geo- recovery](../service-bus-messaging/service-bus-geo-dr.md) ve Azure Olay Hub' larının günde [kurtarma](../event-hubs/event-hubs-geo-dr.md)sınırının yanı sır çerçeves

Azure Olay Hub'larını örnek olarak kullanarak etkin/pasif desen aşağıdakileri içerir:

* Azure Olay Hub'ı hem birincil hem de ikincil bir bölgeye dağıtıldı.
* Birincil ve ikincil Olay Hub'ını eşleştirmek için coğrafi felaket özelliği etkinleştirildi.  Bu, olay hub'larına bağlanmak ve bağlantı bilgilerini değiştirmeden birincilden ikincile geçmek için kullanabileceğiniz bir "takma ad" da oluşturur.
* İşlev uygulamaları hem birincil hem de ikincil bir bölgeye dağıtılır.
* İşlev uygulamaları, ilgili olay hub'ı için *doğrudan* (takma ad olmayan) bağlantı dizesini tetikler. 
* Olay hub'ına yayıncılar diğer ad bağlantı dizesini yayımlamalıdır. 

![Etkin-pasif örnek mimari](media/functions-geo-dr/active-passive.png)

Başarısız olmadan önce, paylaşılan diğer ada gönderen yayımcılar birincil olay merkezine yönlendirecektir.  Birincil işlev uygulaması yalnızca birincil etkinlik merkezini dinlemektir.  İkincil fonksiyon uygulaması pasif ve boşta olacaktır.  Başarısız bir işlem başlatılır başlatılmaz, paylaşılan diğer ada gönderen yayımcılar artık ikincil olay merkezine yönlendirilir.  İkincil işlev uygulaması artık etkin hale gelecek ve otomatik olarak tetiklemeye başlayacak.  İkincil bir bölgeye etkili bir hata tamamen olay merkezinden yönlendirilebilir ve işlevler yalnızca ilgili olay hub'ı etkin olduğunda etkin hale gelir.

[Servis Veri Servisi](../service-bus-messaging/service-bus-geo-dr.md) ve etkinlik [merkezleri](../event-hubs/event-hubs-geo-dr.md)ile başarısız olmak için bilgi ve hususlar hakkında daha fazla bilgi edinin.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Ön Kapı Oluşturma](../frontdoor/quickstart-create-front-door.md)
* [Olay Hub'ları dikkate alınmadan başarısız oldu](../event-hubs/event-hubs-geo-dr.md#considerations)
