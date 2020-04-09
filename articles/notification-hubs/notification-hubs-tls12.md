---
title: Bildirim Hub'ları TLS güncelleştirmeleri
description: Azure Bildirim Hub'larında TLS desteği hakkında bilgi edinin.
services: notification-hubs
documentationcenter: .net
author: sethmanheim
manager: femila
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-multiple
ms.devlang: multiple
ms.topic: article
ms.date: 04/07/2020
ms.author: sethm
ms.reviewer: thsomasu
ms.lastreviewed: 01/28/2020
ms.openlocfilehash: 4da96df50e961f4291029a37e883fdcf88c6c87f
ms.sourcegitcommit: d187fe0143d7dbaf8d775150453bd3c188087411
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80885761"
---
# <a name="transport-layer-security-tls"></a>Aktarım Katmanı Güvenliği (TLS)

Daha yüksek bir güvenlik düzeyi sağlamak için Bildirim Hub'ları 31 Mayıs 2020'de (30 Nisan 2020 tarihinden itibaren uzatılmış) TLS 1.0 ve 1.1 sürümleri için desteği devre dışı kalacaktır. Bu eski protokoller zayıf şifreleme sunar ve BEAST ve POODLE saldırılarına karşı savunmasızdır. Android sürüm 5 veya daha büyük veya daha büyük çalıştıran cihazlara dağıtılan uygulamalar, bu işletim sistemleri TLS 1.2'yi desteklediğinden ve istemci ve sunucu bağlantı da protokolün karşılıklı olarak desteklenen en yüksek sürümünü müzakere edeceği için bu değişiklikten etkilenmez.

TLS 1.2'yi destekleyen en uygun kitaplıkları ve TLS yığınlarını kullandıklarından emin olmak için Azure Bildirim Hub'larını kullanan tüm uygulamalarınızı gözden geçirmenizi öneririz.

## <a name="update-apps"></a>Uygulamaları güncelleştirme

iOS uygulamalarınızın Apple'ın App Transport Security (ATS) adlı ağ güvenliği özelliğini kullanarak TLS 1.2'yi kullandığından emin olabilirsiniz. ATS, iOS 9.0 veya macOS 10.11'den daha eski SDK'lar için kullanılamaz ve [apple belgelerinden](https://developer.apple.com/documentation/security/preventing_insecure_network_connections)bu konuda daha fazla bilgi edinebilirsiniz.

SSLSocket örneklerini kullanan Android uygulamaları için, [setEnabledProtocols'ta](https://developer.android.com/reference/javax/net/ssl/SSLSocket#setEnabledProtocols(java.lang.String%5B%5D))belirtildiği gibi her SSLSocket örneğinde etkin protokoller ayarlayın.

[TLS Protokolü Uyumluluk](https://support.globalsign.com/customer/portal/articles/2934392-tls-protocol-compatibility) destek sayfasındaki tablo, uyumlu TLS sürümleriyle işletim sistemlerinin haritalanmasına yardımcı olur.

Daha fazla bilgi [için, Windows'daki TLS protokolleri desteğine](https://docs.microsoft.com/archive/blogs/kaushal/support-for-ssltls-protocols-on-windows)genel bakış bölümüne bakın.

## <a name="next-steps"></a>Sonraki adımlar

- [Bildirim Hub'larına genel bakış](notification-hubs-push-notification-overview.md)