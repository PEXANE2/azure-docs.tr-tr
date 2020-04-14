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
ms.openlocfilehash: 97fde67c3ac7649418ed0239a2c7aa4f1a4b3f96
ms.sourcegitcommit: 8dc84e8b04390f39a3c11e9b0eaf3264861fcafc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/13/2020
ms.locfileid: "81275052"
---
GatewaySubnet'te 0.0.0.0/0 hedefi olan kullanıcı tanımlı rotalar ve NSG'ler **desteklenmez.** Bu yapılandırmayla oluşturulan ağ geçitleri oluşturulması engellenir. Ağ geçitleri düzgün çalışması için yönetim denetleyicilerine erişim gerektirir. [BGP Rota Yayılımı,](https://docs.microsoft.com/azure/virtual-network/virtual-networks-udr-overview#border-gateway-protocol) ağ geçidinin kullanılabilirliğini sağlamak için GatewaySubnet'te "Etkin" olarak ayarlanmalıdır. Bu devre dışı bırakılacak şekilde ayarlanmışsa, ağ geçidi çalışmaz.
