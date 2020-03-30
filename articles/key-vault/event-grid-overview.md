---
title: Azure Olay Izgarası ile Anahtar Kasası İzleme
description: Key Vault etkinliklerine abone olmak için Azure Olay Kılavuz'u'nun kullanımını
services: key-vault
author: msmbaldwin
manager: rkarlin
ms.service: key-vault
ms.subservice: general
ms.topic: tutorial
ms.date: 11/12/2019
ms.author: mbaldwin
ms.openlocfilehash: 039b7e57c1f98368ab33f9f17e19a741dc9eb5f4
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "78184919"
---
# <a name="monitoring-key-vault-with-azure-event-grid-preview"></a>Azure Olay Izgarasıyla Anahtar Kasası'nı İzleme (önizleme)

Olay Grid ile Key Vault tümleştirmesi şu anda önizlemede. Anahtar kasasında saklanan bir sırrın durumu değiştiğinde kullanıcılara bilgi verilmesine olanak tanır. Durum değişikliği, süresi dolmak üzere olan bir sır (son kullanma tarihinden itibaren 30 gün içinde), süresi dolmuş bir sır veya yeni sürümü bulunan bir sır olarak tanımlanır. Her üç gizli tür (anahtar, sertifika ve gizli) için bildirimler desteklenir.

Uygulamalar karmaşık kod veya pahalı ve verimsiz yoklama hizmetleri gerek kalmadan, modern sunucusuz mimarileri kullanarak bu olaylara tepki verebilir. Etkinlikler Azure [Etkinlik Izgarası](https://azure.microsoft.com/services/event-grid/) aracılığıyla [Azure İşlevleri,](https://azure.microsoft.com/services/functions/) [Azure Mantık Uygulamaları](https://azure.microsoft.com/services/logic-apps/)ve hatta kendi Webhook'unuz gibi etkinlik işleyicilerine itilir ve yalnızca kullandığınız kadar ını ödler. Fiyatlandırma hakkında daha fazla bilgi için [Olay Ağı fiyatlandırması'na](https://azure.microsoft.com/pricing/details/event-grid/)bakın.

## <a name="key-vault-events-and-schemas"></a>Anahtar Vault olaylar ve şemalar

Olay [ızgarası,](../event-grid/concepts.md#event-subscriptions) olay iletilerini abonelere yönlendirmek için olay aboneliklerini kullanır. Anahtar Vault olayları, verilerinizdeki değişikliklere yanıt vermek için gereken tüm bilgileri içerir. EventType özelliği "Microsoft.KeyVault" ile başladığı için bir Key Vault olayını tanımlayabilirsiniz.

Daha fazla bilgi için [Key Vault olay şemasına](../event-grid/event-schema-key-vault.md)bakın.

> [!WARNING]
> Bildirim olayları yalnızca sırların, anahtarların ve sertifikaların yeni sürümlerinde tetiklenir ve bu bildirimleri almak için önce anahtar kasanızdaki etkinliğe abone olmalısınız.
> 
> Sertifikalar üzerinde bildirim olayları yalnızca sertifikanız için belirlediğiniz ilkeye göre otomatik olarak yenilendiğinde alırsınız.

## <a name="practices-for-consuming-events"></a>Etkinlikleri tüketme uygulamaları

Anahtar Kasa olaylarını işleyen uygulamalar önerilen birkaç uygulamayı izlemelidir:

* Olayları aynı olay işleyicisine yönlendirmek için birden çok abonelik yapılandırılabilir. Olayların belirli bir kaynaktan geldiğini varsaymamak, ancak beklediğiniz anahtar kasasından geldiğinden emin olmak için iletinin konusunu kontrol etmek önemlidir.
* Benzer şekilde, eventType'ın işlemeye hazır olup olmadığını denetleyin ve aldığınız tüm olayların beklediğiniz türler olacağını varsaymayın.
* Anlamadığınız alanları yoksay.  Bu uygulama, gelecekte eklenebilir yeni özelliklere karşı esnek tutmanıza yardımcı olacaktır.
* Olayları belirli bir olayla sınırlamak için "özne" öneki ve sonek eşleşmelerini kullanın.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Key Vault'a genel bakış](key-vault-overview.md)
- [Azure Event Grid’e genel bakış](../event-grid/overview.md)
- Nasıl yapılır: [Route Key Vault Events to Automation Runbook (önizleme)](event-grid-tutorial.md).
- Nasıl yapılsın: [Önemli bir kasa gizli değiştiğinde e-posta alın](event-grid-logicapps.md)
- [Azure Anahtar Kasası için Azure Olay Izgara olay şeması (önizleme)](../event-grid/event-schema-key-vault.md)
- [Azure Otomasyonu’na genel bakış](../automation/index.yml)
