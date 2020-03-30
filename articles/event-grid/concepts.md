---
title: Azure Olay Izgara kavramları
description: Azure Event Grid ve kavramlarını açıklar. Olay Izgara'nın birkaç önemli bileşenini tanımlar.
services: event-grid
author: spelluru
ms.service: event-grid
ms.topic: conceptual
ms.date: 08/03/2018
ms.author: spelluru
ms.openlocfilehash: 0821c749a6cb718e1b8abb74a2925bc041850eaf
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79265070"
---
# <a name="concepts-in-azure-event-grid"></a>Azure Etkinlik Izgarasındaki Kavramlar

Bu makalede, Azure Olay Grid'indeki ana kavramlar açıklanmaktadır.

## <a name="events"></a>Olaylar

Olay, sistemde olan bir şeyi tam olarak açıklayan en küçük bilgi miktarıdır. Her olayın ortak bilgileri vardır: olayın kaynağı, olayın gerçekleştiği saat ve benzersiz tanımlayıcı. Her olay aynı zamanda yalnızca belirli olay türüyle ilgili belirli bilgilere sahiptir. Örneğin, Azure Depolama'da oluşturulan yeni dosya hakkındaki olayda dosyayla ilgili `lastTimeModified` değeri gibi ayrıntılar vardır. Öte yandan, bir Event Hubs olayında Capture dosyasının URL'si bulunur. 

64 KB'ye kadar boyut landırma olayı Genel Kullanılabilirlik (GA) Hizmet Düzeyi Sözleşmesi (SLA) kapsamındadır. 1 MB'a kadar boyutlandırma etkinliği desteği şu anda önizlemededir. 64 KB üzerindeki olaylar 64-KB artışlarla ücretlendirilir. 


Bir etkinlikte gönderilen özellikler için [Azure Olay Izgara olay şemasına](event-schema.md)bakın.

## <a name="publishers"></a>Yayımcılar

Yayımcı, Olay Izgarası'na olay göndermeye karar veren kullanıcı veya kuruluştur. Microsoft, çeşitli Azure hizmetleri için etkinlikler yayımlar. Kendi uygulamanızdan olay yayımlayabilirsiniz. Hizmetleri Azure'ın dışında barındıran kuruluşlar Event Grid aracılığıyla olay yayımlayabilir.

## <a name="event-sources"></a>Olay kaynakları

Olay kaynağı, olayın gerçekleştiği yerdir. Her olay kaynağı bir veya birden çok olay türüyle ilişkilidir. Örneğin Azure Depolama, blob tarafından oluşturulan olayların olay kaynağıdır. IoT Hub, cihaz tarafından oluşturulan olayların olay kaynağıdır. Uygulamanız, sizin tanımladığınız özel olayların olay kaynağıdır. Event Grid'e olayları göndermek olay kaynaklarının sorumluluğundadır.

Desteklenen Olay Izgara kaynaklarından herhangi birini uygulama hakkında daha fazla bilgi için [Azure Olay Ağıt'ındaki Olay kaynaklarına](event-sources.md)bakın.

## <a name="topics"></a>Konular

Olay ızgarası konusu, kaynağın olayları gönderdiği bir bitiş noktası sağlar. Yayımcı olay ızgarası konusunu oluşturur ve olay kaynağının bir konuya mı yoksa birden fazla konuya mı ihtiyacı olacağına karar verir. Bir konu, ilgili olaylar topluluğu için kullanılır. Belirli türdeki olaylara yanıt vermek için, aboneler hangi konulara abone olunacağına karar verir.

Sistem konu başlıkları, Azure hizmetleri tarafından sağlanan yerleşik konu başlıklarıdır. Azure aboneliğinizde sistem konu başlıklarını görmezsiniz çünkü konu başlıkları yayımcıya aittir ama siz bunlara abone olabilirsiniz. Abone olmak için, kendisinden olayları almak istediğiniz kaynak hakkında bilgi sağlarsınız. Kaynağa erişiminiz olduğu sürece, o kaynağın olaylarına abone olabilirsiniz.

Özel konu başlıkları uygulamalar ve üçüncü taraf konu başlıklarıdır. Özel konu başlığı oluşturduğunuzda veya özel konu başlığına erişim için atandığınızda, aboneliğinizde özel konu başlığını görürsünüz.

Uygulamanızı tasarlarken, kaç konu oluşturabileceğinize karar verirken esnekliğe sahip siniz. Büyük çözümler için, ilgili olayların her kategorisi için özel bir konu oluşturun. Örneğin, kullanıcı hesaplarını değiştirme ve siparişleri işleme ile ilgili olaylar gönderen bir uygulamayı ele alalım. Herhangi bir olay işleyicisinin her iki olay kategorisini de istemesi pek olası değildir. İki özel konu başlığı oluşturun ve olay işleyicilerinin ilgilendikleri konu başlığına abone olmalarına izin verin. Küçük çözümler için, tüm olayları tek bir konuya göndermeyi tercih edebilirsiniz. Olay aboneleri istedikleri olay türleri için filtre uygulayabilirsiniz.

## <a name="event-subscriptions"></a>Olay abonelikleri

Abonelik, Olay Izgarası'na almak istediğiniz bir konuda hangi olayları anlattığını söyler. Aboneliği oluştururken, olayı işlemek için bir bitiş noktası sağlarsınız. Bitiş noktasına gönderilen olayları filtreleyebilirsiniz. Olay türüne veya konu desenine göre filtre uygulayabilirsiniz. Daha fazla bilgi için [Olay Izgara sıyrık şemasına](subscription-creation-schema.md)bakın.

Abonelik oluşturma örnekleri için bkz:

* [Event Grid için Azure CLI örnekleri](cli-samples.md)
* [Event Grid için Azure PowerShell örnekleri](powershell-samples.md)
* [Event Grid için Azure Resource Manager şablonları](template-samples.md)

Geçerli olay ızgarası aboneliklerinizi alma hakkında daha fazla bilgi için [Sorgu Olay Izgara aboneliklerine](query-event-subscriptions.md)bakın.

## <a name="event-subscription-expiration"></a>Olay aboneliğinin sona ermesi
Olay aboneliğinin süresi bu tarihte otomatik olarak sona erer. Yalnızca sınırlı bir süre için gerekli olan etkinlik abonelikleri için bir son kullanma tarihi ayarlayın ve bu abonelikleri temizleme konusunda endişelenmenize gerek yok. Örneğin, bir senaryoyu sınamak için bir olay aboneliği oluştururken, bir son kullanma tarihi ayarlamak isteyebilirsiniz. 

Bir son kullanma tarihi belirleme örneği için bkz. [gelişmiş filtrelerle Abone Ol.](how-to-filter-events.md#subscribe-with-advanced-filters)

## <a name="event-handlers"></a>Olay işleyicileri

Olay Izgarası açısından bakıldığında, olay işleyicisi olayın gönderildiği yerdir. İşleyici olayı işlemek için başka bir eylem de alır. Olay Izgara birkaç işleyici türlerini destekler. Desteklenen bir Azure hizmetini veya işleyici olarak kendi web hook'unuzu kullanabilirsiniz. Olay Izleyicitürüne bağlı olarak, Olay Izgara olay teslimini garanti etmek için farklı mekanizmaları izler. HTTP webhook olay işleyicileri için, işleyici bir durum kodu `200 – OK`döndürüne ne kadar olay yeniden denendi . Azure Depolama Sırası için, Sıra hizmeti ileti itme işlemlerini başarılı bir şekilde işleyene kadar olaylar yeniden denendir.

Desteklenen Olay Izgara işleyicilerinden herhangi birini uygulama hakkında bilgi için [Azure Olay Idamı'ndaki Olay işleyicilerine](event-handlers.md)bakın.

## <a name="security"></a>Güvenlik

Olay Izgara konulara abone olmak ve konuları yayımlamak için güvenlik sağlar. Abone yken, kaynak veya olay ızgarası konusu yla ilgili yeterli izinlere sahip olmalısınız. Yayımlarken, konu için bir SAS belirteci veya anahtar kimlik doğrulamanız olmalıdır. Daha fazla bilgi için [Olay Izgara güvenliği ve kimlik doğrulaması'na](security-authentication.md)bakın.

## <a name="event-delivery"></a>Olay teslimi

Olay Idamı, bir olayın abonenin bitiş noktası tarafından alındığını doğrulayamıyorsa, olayı yeniden sunar. Daha fazla bilgi için [Olay Izgara iletisi teslimi ve yeniden deneyin.](delivery-and-retry.md)

## <a name="batching"></a>Toplu İşleme

Özel bir konu kullanırken, olaylar her zaman bir dizi de yayımlanmalıdır. Bu, düşük verimli senaryolar için bir toplu iş olabilir, ancak yüksek hacimli kullanım örnekleri için, daha yüksek verimlilik elde etmek için yayımlama başına birkaç olayı bir araya getirebilmeniz önerilir. Toplu iş 1 MB'a kadar olabilir. Her olay yine de 64 KB (Genel Kullanılabilirlik) veya 1 MB (önizleme) büyük olmamalıdır.

> [!NOTE]
> 64 KB'ye kadar boyut landırma olayı Genel Kullanılabilirlik (GA) Hizmet Düzeyi Sözleşmesi (SLA) kapsamındadır. 1 MB'a kadar boyutlandırma etkinliği desteği şu anda önizlemededir. 64 KB üzerindeki etkinlikler 64 KB artışlarla ücretlendirilir. 

## <a name="next-steps"></a>Sonraki adımlar

* Event Grid’e giriş için bkz. [Event Grid hakkında](overview.md).
* Olay Ağıt'ı kullanmaya hızla başlamak için [Azure Olay Ağıtı ile özel etkinlikler oluştur ve yönlendir'e](custom-event-quickstart.md)bakın.
