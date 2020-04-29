---
author: dcurwin
ms.service: backup
ms.topic: include
ms.date: 03/12/2020
ms.author: dacurwin
ms.openlocfilehash: 024f70a21b36bffa662ba6f568adc8efbd0a909d
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81539349"
---
Artık verilerinizi bir sanal ağ içindeki sunuculardan kurtarma hizmetleri kasanıza güvenli bir şekilde yedeklemek için [Özel uç noktaları](https://docs.microsoft.com/azure/private-link/private-endpoint-overview) kullanabilirsiniz. Özel uç nokta, kasanızın VNET adres alanından bir IP kullanır. Sanal ağın ve kasadaki kaynaklarınız arasındaki ağ trafiği, sanal ağınız üzerinden ve Microsoft omurga ağında özel bir bağlantı üzerinden dolaşır. Bu, genel İnternet 'ten etkilenme olasılığını ortadan kaldırır. Özel uç noktalar, Azure sanal makinelerinizin içinde çalışan SQL ve SAP HANA veritabanlarınızı yedeklemek ve geri yüklemek için kullanılabilir. Ayrıca, MARS Aracısı kullanılarak şirket içi sunucularınız için de kullanılabilir.

Azure VM yedeklemesi internet bağlantısı gerektirmez ve bu nedenle ağ yalıtımına izin vermek için özel uç noktalar gerektirmez.

>[!NOTE]
> Bu özellik şu anda sınırlı kullanılabilirliğe sahiptir ve Orta Batı ABD, Orta Güney ABD, Batı ABD 2 ve Doğu ABD (izlenecek diğer Azure bölgelerinde kullanılabilirlik) kullanılabilir. Azure Backup için özel uç noktaları kullanmaya ilgileniyorsanız lütfen [Bu anketi](https://forms.microsoft.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbR0H3_nezt2RNkpBCUTbWEapUQk5EQ1QxRzVOWDNDS1Y1Q0xLTkdLQ0U0RC4u) doldurun ve bize azbackupnetsec@microsoft.com e-posta gönderin. Bu özelliği kullanma özelliği, Azure Backup hizmetinden onaylanma tabidir.
