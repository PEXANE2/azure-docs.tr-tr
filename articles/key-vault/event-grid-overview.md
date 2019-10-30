---
title: Azure Event Grid ile Key Vault izleme
description: Key Vault olaylarına abone olmak için Azure Event Grid kullanma
services: media-services
author: msmbaldwin
manager: rkarlin
ms.service: key-vault
ms.topic: article
ms.date: 10/25/2019
ms.author: mbaldwin
ms.openlocfilehash: 1fa554e03188c4d8d6227a6d2c0a560c3080b0fe
ms.sourcegitcommit: d47a30e54c5c9e65255f7ef3f7194a07931c27df
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/29/2019
ms.locfileid: "73033504"
---
# <a name="monitoring-key-vault-with-azure-event-grid-preview"></a>Azure Event Grid ile Key Vault izleme (Önizleme)

Event Grid ile tümleştirme Key Vault Şu anda önizleme aşamasındadır. Anahtar Kasası 'nda depolanan bir gizli dizi durumu değiştiğinde kullanıcılara bildirim gönderilmesini sağlar. Bir durum değişikliği, süresi dolacak (30 gün sonra), süresi dolan bir gizli dizi veya yeni sürümü bulunan gizli anahtar olarak tanımlanır. Üç gizli dizi türü (anahtar, sertifika ve gizli) için bildirimler desteklenir.

Uygulamalar, karmaşık kod veya pahalı ve verimsiz yoklama Hizmetleri gerekmeden modern sunucusuz mimariler kullanarak bu olaylara tepki verebilir. Olaylar, [Azure işlevleri](https://azure.microsoft.com/services/functions/), [Azure Logic Apps](https://azure.microsoft.com/services/logic-apps/), hatta kendi web kancasına dahil olmak üzere olay işleyicisine [Azure Event Grid](https://azure.microsoft.com/services/event-grid/) gönderilir ve yalnızca kullandığınız kadar ödersiniz. Fiyatlandırma hakkında bilgi için bkz. [Event Grid fiyatlandırması](https://azure.microsoft.com/pricing/details/event-grid/).

## <a name="key-vault-events-and-schemas"></a>Key Vault olayları ve şemaları

Olay Kılavuzu, olay iletilerini abonelere yönlendirmek için [olay abonelikleri](../event-grid/concepts.md#event-subscriptions) kullanır. Key Vault olaylar, verilerdeki değişikliklere yanıt vermek için gereken tüm bilgileri içerir. EventType özelliği "Microsoft. Keykasası" ile başladığı için bir Key Vault olayını tanımlayabilirsiniz.

Daha fazla bilgi için bkz. [Key Vault olay şeması](../event-grid/event-schema-key-vault.md).

> [!NOTE]
> Olaylar yalnızca, abonelik ayarlandıktan sonra oluşturulan gizli sürümler (tüm üç tür) için tetiklenir.
>
> Mevcut gizli dizileri için yeni sürümler oluşturmanız gerekir.

## <a name="practices-for-consuming-events"></a>Olayları tüketen uygulamalar

Key Vault olaylarını işleyen uygulamalar, önerilen birkaç uygulamayı izlemelidir:

* Aynı olay işleyicisine olayları yönlendirmek için birden çok abonelik yapılandırılabilir. Olayların belirli bir kaynaktan olduğunu varsaymamak, ancak beklediğiniz anahtar kasasından geldiğinden emin olmak için iletinin konusunu denetlemek önemlidir.
* Benzer şekilde, eventType için hazırlanmakta olan bir olay olduğunu ve aldığınız tüm olayların istediğiniz tür olacağını kabul edin.
* Anladığınızı alanları yoksayın.  Bu uygulama, gelecekte eklenebilecek yeni özelliklere dayanıklı tutmaya yardımcı olur.
* Olayları belirli bir olayla sınırlamak için "konu" önekini ve sonek eşleşmelerini kullanın.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Key Vault genel bakış](key-vault-overview.md)
- [Azure Event Grid genel bakış](../event-grid/overview.md)
- [Nasıl yapılır: Key Vault olaylarını Automation runbook 'A yönlendirme (Önizleme)](event-grid-tutorial.md).
- [Azure Key Vault için Azure Event Grid olay şeması (Önizleme)](../event-grid/event-schema-key-vault.md)
- [Azure Otomasyonu’na genel bakış](../automation/index.yml)
