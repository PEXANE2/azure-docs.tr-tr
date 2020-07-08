---
title: Azure Güvenlik Duvarı kural işleme mantığı
description: Azure Güvenlik Duvarı kural işleme mantığı hakkında bilgi edinin
services: firewall-manager
author: vhorne
ms.service: firewall-manager
ms.topic: conceptual
ms.date: 06/30/2020
ms.author: victorh
ms.openlocfilehash: 70026173d1cb932d30a59ea2b876ef22217a81bc
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85563684"
---
# <a name="azure-firewall-rule-processing-logic"></a>Azure Güvenlik Duvarı kural işleme mantığı

Azure Güvenlik duvarında NAT kuralları, ağ kuralları ve uygulamalar kuralları vardır. Kurallar kural türüne göre işlenir.

## <a name="network-rules-and-applications-rules"></a>Ağ kuralları ve uygulama kuralları

Ağ kuralları önce uygulanır, sonra uygulama kuralları. Kurallar sonlandırılıyor. Bu nedenle, ağ kurallarında bir eşleşme bulunursa uygulama kuralları işlenmez.  Ağ kuralı eşleşmesi yoksa ve paket Protokolü HTTP/HTTPS ise, paket daha sonra uygulama kuralları tarafından değerlendirilir. Hala eşleşme bulunamazsa paket, altyapı kuralı koleksiyonuna göre değerlendirilir. Hala eşleşme yoksa, paket varsayılan olarak reddedilir.

## <a name="nat-rules"></a>NAT kuralları

Gelen bağlantı [, öğretici: Azure Güvenlik Duvarı ile gelen trafiği Azure Portal kullanarak](../firewall/tutorial-firewall-dnat.md)hedef ağ adresi çevirisi (DNAT) yapılandırılarak etkinleştirilebilir. DNAT kuralları önce uygulanır. Bir eşleşme bulunursa, çevrilmiş trafiğe izin veren örtülü olarak karşılık gelen bir ağ kuralı eklenir. Bu davranışı, çevrilen trafikle eşleşen reddetme kuralları olan bir ağ kural koleksiyonunu açıkça ekleyerek geçersiz kılabilirsiniz. Bu bağlantılar için uygulama kuralı uygulanmaz.

## <a name="inherited-rules"></a>Devralınan kurallar

Bir üst ilkeden devralınan ağ kuralı koleksiyonları, yeni ilkenizin kapsamında tanımlanan ağ kuralı koleksiyonlarının üzerine her zaman öncelik atanır. Aynı mantık, uygulama kuralı koleksiyonları için de geçerlidir. Bununla birlikte, devralma ne olursa olsun, ağ kuralı koleksiyonları her zaman uygulama kuralı koleksiyonlarından önce işlenir.

Varsayılan olarak, ilkeniz ana ilke tehdit zekası modunu devralır. Tehdit bilgileri modinizi ilke ayarları sayfasında farklı bir değere ayarlayarak bunu geçersiz kılabilirsiniz. Yalnızca daha sıkı bir değerle geçersiz kılmak mümkündür. Örneğin, üst ilke *yalnızca uyarı*olarak ayarlandıysa, bu yerel ilkeyi *uyarı ve reddetme*için yapılandırabilirsiniz, ancak kapatamaz.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Güvenlik Duvarı Yöneticisi hakkında daha fazla bilgi](overview.md)