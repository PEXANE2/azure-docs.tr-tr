---
author: dcurwin
ms.service: backup
ms.topic: include
ms.date: 03/12/2020
ms.author: dacurwin
ms.openlocfilehash: fd7124ad83a446d7dde39a836c337a97b12a0a0a
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2021
ms.locfileid: "95555687"
---
Artık verilerinizi bir sanal ağ içindeki sunuculardan kurtarma hizmetleri kasanıza güvenli bir şekilde yedeklemek için [Özel uç noktaları](../articles/private-link/private-endpoint-overview.md) kullanabilirsiniz. Özel uç nokta, kasanızın VNET adres alanından bir IP kullanır. Sanal ağın ve kasadaki kaynaklarınız arasındaki ağ trafiği, sanal ağınız üzerinden ve Microsoft omurga ağında özel bir bağlantı üzerinden dolaşır. Bu, genel İnternet 'ten etkilenme olasılığını ortadan kaldırır. Özel uç noktalar, Azure sanal makinelerinizin içinde çalışan SQL ve SAP HANA veritabanlarınızı yedeklemek ve geri yüklemek için kullanılabilir. Ayrıca, MARS Aracısı kullanılarak şirket içi sunucularınız için de kullanılabilir.

Azure VM yedeklemesi internet bağlantısı gerektirmez ve bu nedenle ağ yalıtımına izin vermek için özel uç noktalar gerektirmez.

[Azure Backup için](../articles/backup/private-endpoints.md)özel uç noktalar hakkında daha fazla bilgi edinin.