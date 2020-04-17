---
author: dcurwin
ms.service: backup
ms.topic: include
ms.date: 03/12/2020
ms.author: dacurwin
ms.openlocfilehash: 024f70a21b36bffa662ba6f568adc8efbd0a909d
ms.sourcegitcommit: b55d7c87dc645d8e5eb1e8f05f5afa38d7574846
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81539349"
---
Artık verilerinizi sanal ağ daki sunuculardan Kurtarma Hizmetleri kasanıza güvenli bir şekilde yedeklemek için [Özel Uç Noktaları'nı](https://docs.microsoft.com/azure/private-link/private-endpoint-overview) kullanabilirsiniz. Özel bitiş noktası, kasanız için VNET adres alanından bir IP kullanır. Sanal ağ içindeki kaynaklarınız ile kasa arasındaki ağ trafiği sanal ağınız ve Microsoft omurga ağınızdaki özel bir bağlantı üzerinden hareket eder. Bu, genel internetten maruz kalma ortadan kaldırır. Azure VM'lerinizin içinde çalışan SQL ve SAP HANA veritabanlarınızı yedeklemek ve geri almak için özel Uç Noktalar kullanılabilir. Mars aracısını kullanarak şirket içi sunucularınız için de kullanılabilir.

Azure VM yedeklemesi internet bağlantısı gerektirmez ve bu nedenle ağ yalıtımına izin vermek için Özel Bitiş Noktaları gerekmez.

>[!NOTE]
> Bu özellik şu anda sınırlı kullanılabilirliktedir ve Batı Orta ABD, Güney Orta ABD, Batı ABD 2 ve Doğu ABD'de kullanılabilir (takip edilebilecek diğer Azure bölgelerinde kullanılabilir). Azure Yedekleme için Özel Bitiş azbackupnetsec@microsoft.com Noktalarını kullanmak istiyorsanız lütfen bu [anketi](https://forms.microsoft.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbR0H3_nezt2RNkpBCUTbWEapUQk5EQ1QxRzVOWDNDS1Y1Q0xLTkdLQ0U0RC4u) doldurun ve bize e-posta gönderin. Bu özelliği kullanabilme özelliği Azure Yedekleme hizmetinin onayına tabidir.
