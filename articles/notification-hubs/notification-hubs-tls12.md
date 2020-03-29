---
title: Bildirim Hub'ları TLS güncelleştirmeleri
description: Azure Bildirim Hub'larında TLS desteği hakkında bilgi edinin.
services: notification-hubs
documentationcenter: .net
author: sethmanheim
manager: femila
editor: jwargo
ms.assetid: ''
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-multiple
ms.devlang: multiple
ms.topic: article
ms.date: 01/30/2020
ms.author: sethm
ms.reviewer: jowargo
ms.lastreviewed: 01/28/2020
ms.openlocfilehash: 87309e20efd9d6f8bd1a659451e5a603e6b95bc8
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76908533"
---
# <a name="transport-layer-security-tls"></a>Aktarım Katmanı Güvenliği (TLS)

Daha yüksek bir güvenlik düzeyi sağlamak için Bildirim Hub'ları 30 Nisan 2020'de TLS 1.0 ve 1.1 sürümleri için desteği devre dışı katacak. Bu eski protokoller zayıf şifreleme sunar ve BEAST ve POODLE saldırılarına karşı savunmasızdır. Android sürüm 5 veya daha büyük veya daha büyük veya iOS sürüm 5 veya daha büyük çalıştıran cihazlara dağıtılan uygulamalar, bu işletim sistemleri TLS 1.2'yi desteklediğinden ve istemci ve sunucu nun karşılıklı olarak desteklenen en yüksek sürümünü müzakere edeceği için bu değişiklikten etkilenmez. bağlantı üzerine protokol.

TLS 1.2'yi destekleyen en uygun kitaplıkları ve TLS yığınlarını kullandıklarından emin olmak için Azure Bildirim Hub'larını kullanan tüm uygulamalarınızı gözden geçirmenizi öneririz.

## <a name="update-apps"></a>Uygulamaları güncelleştirme

iOS uygulamalarınızın Apple'ın App Transport Security (ATS) adlı ağ güvenliği özelliğini kullanarak TLS 1.2'yi kullandığından emin olabilirsiniz. ATS, iOS 9.0 veya macOS 10.11'den daha eski SDK'lar için kullanılamaz ve [apple belgelerinden](https://developer.apple.com/documentation/security/preventing_insecure_network_connections)bu konuda daha fazla bilgi edinebilirsiniz.

SSLSocket örneklerini kullanan Android uygulamaları için, [setEnabledProtocols'ta](https://developer.android.com/reference/javax/net/ssl/SSLSocket#setEnabledProtocols(java.lang.String%5B%5D))belirtildiği gibi her SSLSocket örneğinde etkin protokoller ayarlayın.

[TLS Protokolü Uyumluluk](https://support.globalsign.com/customer/portal/articles/2934392-tls-protocol-compatibility) destek sayfasındaki tablo, uyumlu TLS sürümleriyle işletim sistemlerinin haritalanmasına yardımcı olur.

Daha fazla bilgi [için, Windows'daki TLS protokolleri desteğine](https://docs.microsoft.com/archive/blogs/kaushal/support-for-ssltls-protocols-on-windows)genel bakış bölümüne bakın.

## <a name="next-steps"></a>Sonraki adımlar

- [Bildirim Hub'larına genel bakış](notification-hubs-push-notification-overview.md)