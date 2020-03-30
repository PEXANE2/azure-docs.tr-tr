---
title: include dosyası
description: include dosyası
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: include
ms.date: 10/07/2019
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: f7167cbb26e69941cade01ab8c0b8d9dc633f0d2
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "72168368"
---
Özel IPsec ilkeleriyle çalışırken aşağıdaki gereksinimleri göz önünde bulundurun:

* **IKE** - IKE için, IKE Şifreleme'den herhangi bir parametrenin yanı sıra IKE Bütünlüğü'nden herhangi bir parametre ve DH Group'tan herhangi bir parametre seçebilirsiniz.
* **IPsec** - IPsec için, IPsec Şifreleme herhangi bir parametre, artı IPsec Bütünlüğü herhangi bir parametre, artı PFS seçebilirsiniz. IPsec Şifreleme veya IPsec Bütünlüğü için parametrelerden herhangi biri GCM ise, her iki ayar için parametreler GCM olmalıdır.

>[!NOTE]
> Özel IPsec ilkeleriyle yanıtlayıcı ve başlatıcı kavramı yoktur (Varsayılan IPsec ilkelerinden farklı olarak). Her iki taraf da (şirket içi ve Azure VPN ağ geçidi) IKE Phase 1 ve IKE Phase 2 için aynı ayarları kullanır. Hem IkEv1 hem de IKEv2 protokolleri desteklenir. Yalnızca yanıtlayıcı olarak Azure için destek yoktur.
>

**Kullanılabilir ayarlar ve parametreler**

| Ayar | Parametreler |
|--- |--- |
| IKE Şifreleme | AES256, AES192, AES128 |
| IKE Bütünlüğü | SHA384, SHA256, SHA1 |
| DH Grubu | DHGroup24, ECP384, ECP256, DHGroup14, DHGroup2048, DHGroup2 |
| IPsec Şifrelemesi | GCMAES256, GCMAES192, GCMAES128, AES256, AES192, AES128 |
| IPsec Bütünlüğü | GCMASE256, GCMAES192, GCMAES128, SHA256, SHA1 |
| PFS Grubu | PFS24, ECP384, ECP256, PFS2048, PFS2 |
