---
title: Azure Güvenlik Duvarı kural işleme mantığı
description: Azure Güvenlik Duvarı kural işleme mantığı hakkında bilgi edinin
services: firewall-manager
author: vhorne
ms.service: firewall-manager
ms.topic: conceptual
ms.date: 10/24/2019
ms.author: victorh
ms.openlocfilehash: 74e58c316651a1604984ac14c70a3a65d46d6d9f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "73518209"
---
# <a name="azure-firewall-rule-processing-logic"></a>Azure Güvenlik Duvarı kural işleme mantığı

Azure Güvenlik Duvarı'nda NAT kuralları, ağ kuralları ve uygulama kuralları vardır. Kurallar kural türüne göre işlenir.

## <a name="network-rules-and-applications-rules"></a>Ağ kuralları ve uygulama kuralları

Ağ kuralları önce uygulanır, sonra uygulama kuralları. Kurallar sona eriyor. Bu nedenle, bir eşleşme ağ kurallarında bulunursa, uygulama kuralları işlenmez.  Ağ kuralı eşleşmesi yoksa ve paket protokolü HTTP/HTTPS ise bu durumda paket uygulama kuralları tarafından değerlendirilir. Hala eşleşme bulunamazsa, paket altyapı kuralı koleksiyonuna göre değerlendirilir. Ardından hala eşleşme yoksa paket varsayılan olarak reddedilir.

## <a name="nat-rules"></a>NAT kuralları

Gelen bağlantı, Öğretici'de açıklandığı gibi Hedef Ağ Adresi Çevirisi 'ni (DNAT) yapılandırarak [etkinleştirilebilir: Azure portalını kullanarak Azure Güvenlik Duvarı DNAT ile gelen trafiği filtreleyin.](../firewall/tutorial-firewall-dnat.md) Önce DNAT kuralları uygulanır. Bir eşleşme bulunursa, çevrilen trafiğe izin vermek için örtük karşılık gelen bir ağ kuralı eklenir. Bu davranışı, çevrilen trafikle eşleşen reddetme kuralları olan bir ağ kural koleksiyonunu açıkça ekleyerek geçersiz kılabilirsiniz. Bu bağlantılar için uygulama kuralı uygulanmaz.

## <a name="inherited-rules"></a>Devralınan kurallar

Bir üst ilkeden devralınan ağ kuralı koleksiyonları her zaman yeni ilkenizin bir parçası olarak tanımlanan ağ kuralı koleksiyonlarının üzerinde öncelikverilir. Aynı mantık uygulama kuralı koleksiyonları için de geçerlidir. Ancak, ağ kuralı koleksiyonları her zaman devralma ne olursa olsun uygulama kuralı koleksiyonları önce işlenir.

Varsayılan olarak, ilkeniz ana ilke tehdit istihbarat modu devralır. Tehdit İstihbarat modunuzu ilke ayarları sayfasında farklı bir değere ayarlayarak bunu geçersiz kılabilirsiniz. Yalnızca daha katı bir değerle geçersiz kılmak mümkündür. Örneğin, üst politikanız yalnızca *Uyarı olarak*ayarlanmışsa, bu yerel ilkeyi *Uyarı ve reddetmek*üzere yapılandırabilirsiniz, ancak kapatamazsınız.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Güvenlik Duvarı Yöneticisi Önizlemesi hakkında daha fazla bilgi edinin](overview.md)