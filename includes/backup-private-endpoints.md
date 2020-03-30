---
author: dcurwin
ms.service: backup
ms.topic: include
ms.date: 03/12/2020
ms.author: dacurwin
ms.openlocfilehash: a3bae0ce3e6ebcf64936d0ca4af4fb702e5ea404
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79137033"
---
Artık verilerinizi sanal ağ daki sunuculardan Kurtarma Hizmetleri kasanıza güvenli bir şekilde yedeklemek için [Özel Uç Noktaları'nı](https://docs.microsoft.com/azure/private-link/private-endpoint-overview) kullanabilirsiniz. Özel bitiş noktası, kasanız için VNET adres alanından bir IP kullanır. Sanal ağ içindeki kaynaklarınız ile kasa arasındaki ağ trafiği sanal ağınız ve Microsoft omurga ağınızdaki özel bir bağlantı üzerinden hareket eder. Bu, genel internetten maruz kalma ortadan kaldırır. Azure VM'lerinizin içinde çalışan SQL ve SAP HANA veritabanlarınızı yedeklemek ve geri almak için özel Uç Noktalar kullanılabilir. Mars aracısını kullanarak şirket içi sunucularınız için de kullanılabilir.

Azure VM yedeklemesi internet bağlantısı gerektirmez ve bu nedenle ağ yalıtımına izin vermek için Özel Bitiş Noktaları gerekmez.

>[!NOTE]
> Bu özellik şu anda erken kullanımdadır. Azure Yedekleme için Özel Bitiş azbackupnetsec@microsoft.com Noktalarını kullanmak istiyorsanız lütfen bu [anketi](https://forms.microsoft.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbR0H3_nezt2RNkpBCUTbWEapUQk5EQ1QxRzVOWDNDS1Y1Q0xLTkdLQ0U0RC4u) doldurun ve bize e-posta gönderin. Bu özelliği kullanabilme özelliği Azure Yedekleme hizmetinin onayına tabidir.
