---
title: include dosyası
description: include dosyası
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 10/28/2020
ms.author: cherylmc
ms.openlocfilehash: f37876a0b6b7e8b520b5b6428381ebaf7b5b5c24
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2021
ms.locfileid: "93042584"
---
Sürüm istemcinin mimarisiyle eşleştiği sürece, her bir Windows istemci bilgisayarında aynı VPN istemcisi yapılandırma paketini kullanabilirsiniz. Desteklenen istemci işletim sistemlerinin listesi için [VPN Gateway SSS](../articles/vpn-gateway/vpn-gateway-vpn-faq.md#P2S)konusunun Noktadan siteye bölümüne bakın.

>[!NOTE]
>Bağlanmak istediğiniz Windows istemci bilgisayarında yönetici haklarına sahip olmanız gerekir.
>

Sertifika kimlik doğrulaması için yerel Windows VPN istemcisini yapılandırmak için aşağıdaki adımları kullanın:

1. Windows bilgisayarın mimarisine karşılık gelen VPN istemcisi yapılandırma dosyalarını seçin. 64 bit işlemci mimarisi için 'VpnClientSetupAmd64' yükleyici paketini seçin. 32 bit işlemci mimarisi için 'VpnClientSetupX86' yükleyici paketini seçin. 
1. Yüklemek için pakete çift tıklayın. Bir SmartScreen açılır penceresi görürseniz **Daha fazla bilgi**’ye ve ardından **Yine de çalıştır**’a tıklayın.
1. İstemci bilgisayarda **ağ ayarları** ' na gidin ve **VPN**' ye tıklayın. VPN bağlantısı, bağlandığı sanal ağın adını gösterir.
1. Bağlanmayı denemeden önce, istemci bilgisayara bir istemci sertifikası yüklediğinizi doğrulayın. Yerel Azure sertifika kimlik doğrulaması türü kullanılırken kimlik doğrulaması için bir istemci sertifikası gereklidir.