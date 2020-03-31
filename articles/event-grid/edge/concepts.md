---
title: Kavramlar - Azure Olay Izgara IoT Edge | Microsoft Dokümanlar
description: IoT Edge'deki Olay Izgara'daki kavramlar.
author: VidyaKukke
manager: rajarv
ms.author: vkukke
ms.reviewer: spelluru
ms.date: 10/03/2019
ms.topic: article
ms.service: event-grid
services: event-grid
ms.openlocfilehash: 73309e10e88c11e639e6ac6fd3bb061e1b5c685b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "72992554"
---
# <a name="event-grid-concepts"></a>Event Grid kavramları

Bu makalede, Azure Olay Grid'indeki ana kavramlar açıklanmaktadır.

## <a name="events"></a>Olaylar

Olay, sistemde olan bir şeyi tam olarak açıklayan en küçük bilgi miktarıdır. Her olayın ortak bilgileri vardır: olayın kaynağı, olayın gerçekleştiği saat ve benzersiz tanımlayıcı. Her olay aynı zamanda yalnızca belirli olay türüyle ilgili belirli bilgilere sahiptir. 1 MB'a kadar boyutlandırma etkinliği desteği şu anda önizlemededir.

Bir olaya dahil olan özellikler için [Azure Olay Ağı etkinliği şemasına](event-schemas.md)bakın.

## <a name="publishers"></a>Yayımcılar

Yayımcı, Olay Izgarası'na olay göndermeye karar veren kullanıcı veya kuruluştur. Kendi uygulamanızdan olay yayımlayabilirsiniz.

## <a name="event-sources"></a>Olay kaynakları

Olay kaynağı, olayın gerçekleştiği yerdir. Her olay kaynağı bir veya birden çok olay türüyle ilişkilidir. Örneğin Azure Depolama, blob tarafından oluşturulan olayların olay kaynağıdır. Uygulamanız, sizin tanımladığınız özel olayların olay kaynağıdır. Event Grid'e olayları göndermek olay kaynaklarının sorumluluğundadır.

## <a name="topics"></a>Konular

Olay ızgarası konusu, kaynağın olayları gönderdiği bir bitiş noktası sağlar. Yayımcı olay ızgarası konusunu oluşturur ve olay kaynağının bir konuya mı yoksa birden fazla konuya mı ihtiyacı olacağına karar verir. Bir konu, ilgili olaylar topluluğu için kullanılır. Belirli türdeki olaylara yanıt vermek için, aboneler hangi konulara abone olunacağına karar verir.

Uygulamanızı tasarlarken, kaç konu oluşturabileceğinize karar verme esnekliğine sahipsiniz. Büyük çözümler için, ilgili olayların her kategorisi için özel bir konu oluşturun. Örneğin, kullanıcı hesaplarını değiştirme ve siparişleri işleme ile ilgili olaylar gönderen bir uygulamayı ele alalım. Herhangi bir olay işleyicisinin her iki olay kategorisini de istemesi pek olası değildir. İki özel konu başlığı oluşturun ve olay işleyicilerinin ilgilendikleri konu başlığına abone olmalarına izin verin. Küçük çözümler için, tüm olayları tek bir konuya göndermeyi tercih edebilirsiniz. Olay aboneleri istedikleri olay türleri için filtre uygulayabilirsiniz.

Olay Izgara'sındaki konuların nasıl yönetilene ilişkin [REST API belgelerine](api.md) bakın.

## <a name="event-subscriptions"></a>Olay abonelikleri

Abonelik, Olay Izgarası'na almak istediğiniz bir konuda hangi olayları anlattığını söyler. Aboneliği oluştururken, olayı işlemek için bir bitiş noktası sağlarsınız. Bitiş noktasına gönderilen olayları filtreleyebilirsiniz. 

Olay Grid'deki aboneliklerin nasıl yönetilene ilişkin [REST API belgelerine](api.md) bakın.

## <a name="event-handlers"></a>Olay işleyicileri

Olay Izgarası açısından bakıldığında, olay işleyicisi olayın gönderildiği yerdir. İşleyici olayı işlemek için daha fazla eylem alır. Olay Izgara birkaç işleyici türlerini destekler. Desteklenen bir Azure hizmetini veya işleyici olarak kendi web kancanızı kullanabilirsiniz. Olay Izleyicitürüne bağlı olarak, Olay Izgara olay teslimini garanti etmek için farklı mekanizmaları izler. Hedef olay işleyicisi bir HTTP web kancasıysa, işleyici `200 – OK`' nin durum kodunu döndürdüğünde olay yeniden deneniyor. Kenar Hub'ı için, olay istisnasız teslim edilirse, başarılı olarak kabul edilir.

## <a name="security"></a>Güvenlik

Olay Izgara konulara abone olmak ve konuları yayımlamak için güvenlik sağlar. Daha fazla bilgi için [Olay Izgara güvenliği ve kimlik doğrulaması'na](security-authentication.md)bakın.

## <a name="event-delivery"></a>Olay teslimi

Olay Idamı, bir olayın abonenin bitiş noktası tarafından alındığını doğrulayamıyorsa, olayı yeniden sunar. Daha fazla bilgi için [Olay Izgara iletisi teslimi ve yeniden deneyin.](delivery-retry.md)

## <a name="batching"></a>Toplu İşleme

Özel bir konu kullanırken, olaylar her zaman bir dizi de yayımlanmalıdır. Düşük iş dizisi senaryoları için dizinin yalnızca bir değeri olacaktır. Yüksek hacimli kullanım örnekleri için, daha yüksek verimlilik elde etmek için yayımlama başına birkaç olayı bir araya getiremenizi öneririz. Toplu iş 1 MB'a kadar olabilir. Her olay yine de 1 MB'dan (önizleme) büyük olmamalıdır.