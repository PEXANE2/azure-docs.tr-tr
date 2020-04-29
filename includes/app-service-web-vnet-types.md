---
author: ccompy
ms.service: app-service-web
ms.topic: include
ms.date: 04/15/2020
ms.author: ccompy
ms.openlocfilehash: c31a5aaa9866a4ce97cd3cd59a8e363834f70587
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81312821"
---
* Yalıtılmış haricinde fiyatlandırma planlarının tam aralığını destekleyen çok kiracılı sistemler.
* Sanal ağınıza dağıtan ve yalıtılmış fiyatlandırma planı uygulamalarını destekleyen App Service Ortamı.

VNET tümleştirme özelliği çok kiracılı uygulamalarında kullanılır. Uygulamanız [App Service ortamı][ASEintro]yer alıyorsa, zaten VNET 'te bulunur ve aynı VNET 'teki kaynaklara ulaşmak Için VNET tümleştirme özelliğinin kullanılmasını gerektirmez. Tüm ağ özellikleri hakkında daha fazla bilgi için bkz. [App Service ağ özellikleri][Networkingfeatures].

VNet tümleştirmesi, uygulamanızın VNet 'teki kaynaklara erişmesini sağlar, ancak sanal ağdan uygulamanıza gelen özel erişim vermez. Özel site erişimi, bir uygulamayı yalnızca bir Azure sanal ağı içindeki gibi özel bir ağdan erişilebilir hale getirme anlamına gelir. VNet tümleştirmesi yalnızca uygulamanızdan sanal ağınıza giden çağrılar yapmak için kullanılır. VNET tümleştirme özelliği, aynı bölgedeki VNet ile ve diğer bölgelerde VNet ile kullanıldığında farklı davranır. VNet tümleştirme özelliğinin iki çeşidi vardır:

* **Bölgesel VNET tümleştirmesi**: aynı bölgedeki Azure Resource Manager sanal ağlara bağlandığınızda, Tümleştirdiğiniz VNET 'te ayrılmış bir alt ağa sahip olmanız gerekir.
* **Ağ Geçidi-gerekli VNET tümleştirmesi**: başka bölgelerdeki VNET 'e veya aynı bölgedeki klasik bir sanal ağa bağlandığınızda hedef VNET 'te sağlanan bir Azure sanal ağ geçidi gerekir.

VNet tümleştirme özellikleri:

* Standart, Premium, PremiumV2 veya elastik Premium fiyatlandırma planı gerektirir.
* TCP ve UDP desteği.
* Azure App Service uygulamalar ve işlev uygulamalarıyla çalışın.

VNet tümleştirmesinin desteklemediği bazı şeyler vardır, örneğin:

* Sürücü bağlama.
* Active Directory tümleştirme.
* Tanımlaya.

Ağ Geçidi gerekli VNet tümleştirmesi, yalnızca hedef VNet 'te veya eşleme ya da VPN 'Ler ile hedef VNet 'e bağlı ağlarda bulunan kaynaklara erişim sağlar. Ağ Geçidi gerekli VNet tümleştirmesi, Azure ExpressRoute bağlantılarında bulunan kaynaklara erişimi etkinleştirmez veya hizmet uç noktaları ile birlikte kullanılabilir.

VNet tümleştirmesi, kullanılan sürümden bağımsız olarak uygulamanızın VNet 'teki kaynaklara erişmesini sağlar, ancak sanal ağdan uygulamanıza gelen özel erişim vermez. Özel site erişimi, uygulamanızı Azure VNet içindeki gibi özel bir ağdan erişilebilir hale getirme anlamına gelir. VNet tümleştirmesi yalnızca uygulamanızdan VNet 'e giden çağrılar yapmak içindir.

<!--Links-->
[ASEintro]: https://docs.microsoft.com/azure/app-service/environment/intro
[Networkingfeatures]: https://docs.microsoft.com/azure/app-service/networking-features
