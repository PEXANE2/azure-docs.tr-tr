---
title: Notification Hubs TLS güncelleştirmeleri
description: Azure Notification Hubs TLS desteği hakkında bilgi edinin.
services: notification-hubs
documentationcenter: .net
author: sethmanheim
manager: femila
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-multiple
ms.devlang: multiple
ms.topic: article
ms.date: 04/29/2020
ms.author: sethm
ms.reviewer: thsomasu
ms.lastreviewed: 01/28/2020
ms.openlocfilehash: 87a3627d7820f9f456ac08e2f20b70af961f817e
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87084254"
---
# <a name="transport-layer-security-tls"></a>Aktarım Katmanı Güvenliği (TLS)

Daha yüksek düzeyde güvenlik sağlamak için Notification Hubs, 1,0 **aralık 2020 2020** tarihinde TLS ve 1,1 desteğini devre dışı bırakacak. Bu eski protokoller zayıf şifreleme sağlar ve BEAST ve POOıDLE saldırılarına karşı savunmasız kalır. Android sürüm 5 veya üzerini çalıştıran cihazlara dağıtılan uygulamalar veya iOS sürüm 5 ya da üzeri, bu işletim sistemleri TLS 1,2 ' i destekledikleri ve istemci ve sunucu, bağlantı kurulduğunda protokolün en yüksek desteklenen sürümü üzerinde anlaşacak şekilde bu değişiklikten etkilenmez.

TLS 1,2 ' i destekleyen en uygun kitaplıkları ve TLS yığınlarını kullandıklarından emin olmak için Azure Notification Hubs kullanan tüm uygulamalarınızı incelemenizi öneririz.

## <a name="update-apps"></a>Uygulamaları güncelleştirme

İOS uygulamalarınızın, uygulama taşıma güvenliği (ATS) adlı Apple 'ın ağ güvenlik özelliğini kullanarak TLS 1,2 kullandığından emin olabilirsiniz. ATS, iOS 9,0 veya macOS 10,11 ' den daha eski SDK 'lar için kullanılamaz ve [Apple 'ın belgelerinden](https://developer.apple.com/documentation/security/preventing_insecure_network_connections)daha fazla bilgi edinebilirsiniz.

SSLSocket örnekleri kullanan Android uygulamaları için, her SSLSocket örneğinde etkinleştirilmiş protokolleri [Setenabledprotocols](https://developer.android.com/reference/javax/net/ssl/SSLSocket#setEnabledProtocols(java.lang.String%5B%5D))bölümünde belirtildiği gibi ayarlayın.

[TLS protokol uyumluluğu](https://support.globalsign.com/customer/portal/articles/2934392-tls-protocol-compatibility) Destek sayfasındaki tablo, işletim SISTEMLERININ uyumlu TLS sürümleriyle eşlenmenize yardımcı olur.

Daha fazla bilgi için bkz. [Windows 'DA TLS protokolleri için desteğe](/archive/blogs/kaushal/support-for-ssltls-protocols-on-windows)genel bakış.

## <a name="next-steps"></a>Sonraki adımlar

- [Notification Hubs genel bakış](notification-hubs-push-notification-overview.md)
