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
ms.openlocfilehash: 0e12ad66c635632b29b70000b6e227ddcbb5357b
ms.sourcegitcommit: efefce53f1b75e5d90e27d3fd3719e146983a780
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/01/2020
ms.locfileid: "80502468"
---
GatewaySubnet'te 0.0.0.0/0 hedefi olan kullanıcı tanımlı rotalar ve NSG'ler **desteklenmez.** Bu yapılandırmayla oluşturulan ağ geçitleri oluşturulması engellenir. Ağ geçitleri düzgün çalışması için yönetim denetleyicilerine erişim gerektirir. [BGP Rota Yayılımı,](https://docs.microsoft.com/azure/virtual-network/virtual-networks-udr-overview#border-gateway-protocol]) ağ geçidinin kullanılabilirliğini sağlamak için GatewaySubnet'te "Etkin" olarak ayarlanmalıdır. Bu devre dışı bırakılacak şekilde ayarlanmışsa, ağ geçidi çalışmaz.
