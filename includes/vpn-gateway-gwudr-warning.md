---
title: dosya dahil etme
description: dosya dahil etme
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 09/28/2019
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 97fde67c3ac7649418ed0239a2c7aa4f1a4b3f96
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "81275052"
---
GatewaySubnet üzerinde 0.0.0.0/0 hedefi ve NSG 'ler içeren Kullanıcı tanımlı rotalar **desteklenmez**. Bu yapılandırmayla oluşturulan ağ geçitlerinin oluşturulması engellenir. Ağ geçitlerinin düzgün çalışması için yönetim denetleyicilerine erişimi olması gerekir. Ağ geçidinin kullanılabilirliğini sağlamak için, GatewaySubnet üzerinde [BGP yol yayılması](https://docs.microsoft.com/azure/virtual-network/virtual-networks-udr-overview#border-gateway-protocol) "Enabled" olarak ayarlanmalıdır. Devre dışı olarak ayarlanırsa, ağ geçidi çalışmaz.
