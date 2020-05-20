---
author: dcurwin
ms.service: backup
ms.topic: include
ms.date: 03/12/2020
ms.author: dacurwin
ms.openlocfilehash: b8941bb4fa77cb0008cb7271681e972bc21d6588
ms.sourcegitcommit: fdec8e8bdbddcce5b7a0c4ffc6842154220c8b90
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/19/2020
ms.locfileid: "83659525"
---
Artık verilerinizi bir sanal ağ içindeki sunuculardan kurtarma hizmetleri kasanıza güvenli bir şekilde yedeklemek için [Özel uç noktaları](https://docs.microsoft.com/azure/private-link/private-endpoint-overview) kullanabilirsiniz. Özel uç nokta, kasanızın VNET adres alanından bir IP kullanır. Sanal ağın ve kasadaki kaynaklarınız arasındaki ağ trafiği, sanal ağınız üzerinden ve Microsoft omurga ağında özel bir bağlantı üzerinden dolaşır. Bu, genel İnternet 'ten etkilenme olasılığını ortadan kaldırır. Özel uç noktalar, Azure sanal makinelerinizin içinde çalışan SQL ve SAP HANA veritabanlarınızı yedeklemek ve geri yüklemek için kullanılabilir. Ayrıca, MARS Aracısı kullanılarak şirket içi sunucularınız için de kullanılabilir.

Azure VM yedeklemesi internet bağlantısı gerektirmez ve bu nedenle ağ yalıtımına izin vermek için özel uç noktalar gerektirmez.

[Azure Backup için](https://docs.microsoft.com/azure/backup/private-endpoints)özel uç noktalar hakkında daha fazla bilgi edinin.
