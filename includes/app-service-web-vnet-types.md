---
author: ccompy
ms.service: app-service-web
ms.topic: include
ms.date: 02/27/2020
ms.author: ccompy
ms.openlocfilehash: da43e639779dd4946cb014be777e3a6db0366e24
ms.sourcegitcommit: bc792d0525d83f00d2329bea054ac45b2495315d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78671486"
---
* Yalıtılmış haricinde fiyatlandırma planlarının tam aralığını destekleyen çok kiracılı sistemler
* Sanal ağınıza dağıtan ve yalıtılmış fiyatlandırma planı uygulamalarını destekleyen App Service Ortamı (ASE)

Bu belge, çok kiracılı uygulamalarda kullanılmak üzere VNet tümleştirme özelliği aracılığıyla gider. Uygulamanız [App Service ortamı][ASEintro]yer alıyorsa, zaten VNET 'te bulunur ve aynı VNET 'teki kaynaklara ulaşmak Için VNET tümleştirme özelliğinin kullanılmasını gerektirmez. Tüm ağ özellikleriyle ilgili ayrıntılar için [App Service ağ özelliklerini][Networkingfeatures] okuyun

VNet tümleştirmesi, uygulamanızın sanal ağınızdaki kaynaklara erişmesini sağlar, ancak sanal ağdan uygulamanıza gelen özel erişim vermez. Özel site erişimi, uygulamayı yalnızca bir Azure sanal ağı içindeki gibi özel bir ağdan erişilebilir hale getirme anlamına gelir. VNet tümleştirmesi yalnızca uygulamanızdan VNet 'e giden çağrılar yapmak içindir. VNet tümleştirme özelliği, aynı bölgedeki sanal ağlar ve diğer bölgelerde VNET 'ler ile kullanıldığında farklı davranır. VNet tümleştirme özelliğinin iki çeşidi vardır.

* Bölgesel VNet tümleştirmesi-aynı bölgedeki Kaynak Yöneticisi sanal ağlara bağlanırken, Tümleştirdiğiniz VNet 'te ayrılmış bir alt ağa sahip olmanız gerekir. 
* Ağ Geçidi gerekli VNet tümleştirmesi-diğer bölgelerde sanal ağlara veya aynı bölgedeki bir klasik VNet 'e bağlanılırken hedef VNet 'te sağlanmış bir sanal ağ geçidi gerekir.

VNet tümleştirme özellikleri:

* Standart, Premium, PremiumV2 veya elastik Premium fiyatlandırma planı gerektir 
* TCP ve UDP desteği
* App Service uygulamalarla ve Işlev uygulamalarıyla çalışma

VNet tümleştirmesinin şunları dahil desteklemediği bazı şeyler vardır:

* sürücü bağlama
* AD tümleştirmesi 
* NetBios

Ağ Geçidi gerekli VNet tümleştirmesi yalnızca hedef VNet 'te veya eşleme ya da VPN 'Ler ile hedef VNet 'e bağlı ağlarda bulunan kaynaklara erişim sağlar. Ağ Geçidi gerekli VNet tümleştirmesi, ExpressRoute bağlantılarında kullanılabilir olan kaynaklara erişimi etkinleştirmez veya hizmet uç noktaları ile birlikte kullanılabilir. 

VNet tümleştirmesi, kullanılan sürümden bağımsız olarak, uygulamanıza sanal ağınızdaki kaynaklara erişmenizi sağlar, ancak sanal ağdan uygulamanıza gelen özel erişim vermez. Özel site erişimi, uygulamanızı yalnızca bir Azure sanal ağı içindeki gibi özel bir ağdan erişilebilir hale getirme anlamına gelir. VNet tümleştirmesi yalnızca uygulamanızdan VNet 'e giden çağrılar yapmak içindir. 

<!--Links-->
[ASEintro]: https://docs.microsoft.com/azure/app-service/environment/intro
[Networkingfeatures]: https://docs.microsoft.com/azure/app-service/networking-features