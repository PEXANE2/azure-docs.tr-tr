---
author: cephalin
ms.service: app-service
ms.topic: include
ms.date: 04/15/2020
ms.author: cephalin
ms.openlocfilehash: 0fccb1ed092190f88d197c9bf2cd17678a4d1731
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "83649032"
---
Azure VM 'Leri, depolama, ağ bağlantıları, Web çerçeveleri, yönetim ve tümleştirme özellikleri dahil olmak üzere App Service platform bileşenleri, etkin olarak güvenli ve sağlamlaştırılmış hale getirilir. App Service, şu emin olmak için sürekli olarak vinen uyumluluk denetimlerinden geçer:

- Uygulama kaynaklarınızın diğer müşterilerin Azure kaynaklarından [güvenliği](https://github.com/projectkudu/kudu/wiki/Azure-Web-App-sandbox) sağlanır.
- Yeni keşfedilen güvenlik açıklarını gidermek için [sanal makine örnekleri ve çalışma zamanı yazılımı düzenli olarak güncelleştirilir](../articles/app-service/overview-patch-os-runtime.md) . 
- Uygulamanız ve diğer Azure kaynakları (örneğin, [SQL veritabanı](https://azure.microsoft.com/services/sql-database/)) arasındaki gizli dizileri (bağlantı dizeleri gibi) Iletişim, Azure 'da kalır ve herhangi bir ağ sınırları boyunca geçmez. Gizli diziler her zaman depolandığında şifrelenir.
- [Karma bağlantı](../articles/app-service/app-service-hybrid-connections.md)gibi App Service bağlantısı özellikleri üzerinden tüm iletişimler şifrelenir. 
- Azure PowerShell, Azure CLı, Azure SDK 'Ları, REST API 'Ler gibi uzak yönetim araçları ile kurulan bağlantılar, tümüyle şifrelenir.
- 24 saatlik tehdit yönetimi, altyapıyı ve platformu kötü amaçlı yazılım, dağıtılmış hizmet reddi (DDoS), ortadaki adam (MITD) ve diğer tehditlere karşı korur.

Azure 'da altyapı ve platform güvenliği hakkında daha fazla bilgi için bkz. [Azure Güven Merkezi](https://azure.microsoft.com/overview/trusted-cloud/).