---
title: include dosyası
description: include dosyası
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 07/30/2018
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: e4d20cd39d2a843ee1ab57a412ac668b3495fdb1
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "67188195"
---
>[!NOTE]
>1 Temmuz 2018 tarihinden itibaren TLS 1.0 ve 1.1 desteği Azure VPN Gateway'den kaldırılıyor. VPN Gateway, yalnızca TLS 1.2’yi destekleyecektir. Desteği korumak için [TLS1.2 desteğini etkinleştirmek için güncelleştirmelere](#tls1)bakın.

Ayrıca, 1 Temmuz 2018 tarihinde TLS için aşağıdaki eski algoritmalar da amortismana alınacaktır:

* RC4 (Rivest Şifrelemesi 4)
* DES (Veri Şifreleme Algoritması)
* 3DES (Üçlü Veri Şifreleme Algoritması)
* MD5 (İleti Özeti 5)

### <a name="how-do-i-enable-support-for-tls-12-in-windows-7-and-windows-81"></a><a name="tls1"></a>Windows 7 ve Windows 8.1'de TLS 1.2 desteğini nasıl etkinleştirebilirim?

[!INCLUDE [tls 1.2](vpn-gateway-tls-include.md)]
