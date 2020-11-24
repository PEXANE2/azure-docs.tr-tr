---
title: include dosyası
description: include dosyası
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 09/28/2019
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: cf9d4c3fd96df83361e7d9aa89ba702d37265ec6
ms.sourcegitcommit: c95e2d89a5a3cf5e2983ffcc206f056a7992df7d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/24/2020
ms.locfileid: "95554812"
---
GatewaySubnet üzerinde 0.0.0.0/0 hedefi ve NSG 'ler içeren Kullanıcı tanımlı rotalar **desteklenmez**. Bu yapılandırmayla oluşturulan ağ geçitlerinin oluşturulması engellenir. Ağ geçitlerinin düzgün çalışması için yönetim denetleyicilerine erişimi olması gerekir. Ağ geçidinin kullanılabilirliğini sağlamak için, GatewaySubnet üzerinde [BGP yol yayılması](../articles/virtual-network/virtual-networks-udr-overview.md#border-gateway-protocol) "Enabled" olarak ayarlanmalıdır. Devre dışı olarak ayarlanırsa, ağ geçidi çalışmaz.