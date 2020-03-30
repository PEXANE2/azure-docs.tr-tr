---
author: ccompy
ms.service: app-service-web
ms.topic: include
ms.date: 02/27/2020
ms.author: ccompy
ms.openlocfilehash: da43e639779dd4946cb014be777e3a6db0366e24
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78671486"
---
* İzole dışında tüm fiyatlandırma planlarını destekleyen çok kiracılı sistemler
* VNet'inize dağıtan ve Yalıtılmış fiyatlandırma planı uygulamalarını destekleyen Uygulama Hizmet Ortamı (ASE)

Bu belge, çok kiracılı uygulamalarda kullanılmak üzere olan VNet Tümleştirme özelliğinden geçer. Uygulamanız App [Service][ASEintro]Environment'daysa, zaten bir VNet'teyse ve aynı VNet'teki kaynaklara ulaşmak için VNet Tümleştirme özelliğinin kullanılması gerekmez. Tüm ağ özellikleri hakkında ayrıntılı bilgi için [App Service ağ özelliklerini][Networkingfeatures] okuyun

VNet Tümleştirme, uygulamanızın sanal ağınızdaki kaynaklara erişmesini sağlar, ancak VNet'ten uygulamanız için gelen özel erişim izni vermez. Özel site erişimi, uygulamayı yalnızca Azure sanal ağı gibi özel bir ağdan erişilebilir hale getirmek anlamına gelir. VNet Tümleştirme, yalnızca uygulamanızdan VNet'inize giden aramaları yapmak içindir. VNet Tümleştirme özelliği, aynı bölgede VNet'lerle ve diğer bölgelerdeki VNet'lerle kullanıldığında farklı şekilde kullanılır. VNet Tümleştirme özelliğiiki varyasyona sahiptir.

* Bölgesel VNet Tümleştirmesi - Aynı bölgedeki Kaynak Yöneticisi VNet'e bağlanırken, entegre olduğunuz VNet'te özel bir alt ağınız olmalıdır. 
* Ağ Geçidi gerekli VNet Tümleştirmesi - Diğer bölgelerdeki VNet'lere veya aynı bölgedeki klasik VNet'e bağlanırken hedef VNet'te bir Sanal Ağ ağ geçidine ihtiyacınız vardır.

VNet Tümleştirme özellikleri:

* Standart, Premium, PremiumV2 veya Elastik Premium fiyatlandırma planı gerektirir 
* Destek TCP ve UDP
* Uygulama Hizmeti uygulamaları yla ve İşlev uygulamalarıyla çalışma

VNet Tümleştirmesi'nin desteklemediği bazı şeyler vardır:

* bir sürücü montaj
* AD tümleştirmesi 
* NetBios

Ağ Geçidi gerekli VNet Tümleştirmesi yalnızca hedef VNet'teki veya hedef VNet'e bağlı ağlardaki kaynaklara bakma veya VPN'lerle erişim sağlar. Ağ Geçidi gerekli VNet Tümleştirme expressroute bağlantıları arasında kullanılabilir kaynaklara erişim sağlamaz veya hizmet bitiş noktaları ile çalışır. 

Kullanılan sürümden bağımsız olarak, VNet Tümleştirme uygulamanızın sanal ağınızdaki kaynaklara erişmesini sağlar, ancak uygulamanız için sanal ağdan gelen özel erişim izni vermez. Özel site erişimi, uygulamanızı yalnızca Azure sanal ağındaki gibi özel bir ağdan erişilebilir hale getirmek anlamına gelir. VNet Tümleştirme, yalnızca uygulamanızdan VNet'inize giden aramaları yapmak içindir. 

<!--Links-->
[ASEintro]: https://docs.microsoft.com/azure/app-service/environment/intro
[Networkingfeatures]: https://docs.microsoft.com/azure/app-service/networking-features