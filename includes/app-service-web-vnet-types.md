---
author: ccompy
ms.service: app-service-web
ms.topic: include
ms.date: 04/15/2020
ms.author: ccompy
ms.openlocfilehash: c31a5aaa9866a4ce97cd3cd59a8e363834f70587
ms.sourcegitcommit: 7e04a51363de29322de08d2c5024d97506937a60
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/14/2020
ms.locfileid: "81312821"
---
* İzole dışında tüm fiyatlandırma planlarını destekleyen çok kiracılı sistemler.
* VNet'inize dağılan ve Yalıtılmış fiyatlandırma planı uygulamalarını destekleyen Uygulama Hizmet Ortamı.

VNet Tümleştirme özelliği çok kiracılı uygulamalarda kullanılır. Uygulamanız App [Service][ASEintro]Environment'daysa, zaten bir VNet'teyse ve aynı VNet'teki kaynaklara ulaşmak için VNet Tümleştirme özelliğinin kullanılması gerekmez. Tüm ağ özellikleri hakkında daha fazla bilgi için [Uygulama Hizmeti ağ özelliklerine][Networkingfeatures]bakın.

VNet Tümleştirme, uygulamanızın VNet'inizdeki kaynaklara erişmesini sağlar, ancak Uygulamanıza VNet'ten gelen özel erişim izni vermez. Özel site erişimi, bir uygulamayı yalnızca Azure sanal ağındaki gibi özel bir ağdan erişilebilir hale getirmek anlamına gelir. VNet Tümleştirme, yalnızca uygulamanızdan VNet'inize giden aramaları yapmak için kullanılır. VNet Tümleştirme özelliği, aynı bölgede VNet ve diğer bölgelerdeki VNet ile kullanıldığında farklı şekilde kullanılır. VNet Tümleştirme özelliğinin iki çeşidi vardır:

* **Bölgesel VNet Tümleştirmesi**: Aynı bölgedeki Azure Kaynak Yöneticisi sanal ağlarına bağlandığınızda, entegre olduğunuz VNet'te özel bir alt ağınız olmalıdır.
* **Ağ Geçidi gerekli VNet Tümleştirmesi**: Diğer bölgelerdeki VNet'e veya aynı bölgedeki klasik bir sanal ağa bağlandığınızda, hedef VNet'te bir Azure Sanal Ağ ağ geçidine ihtiyacınız vardır.

VNet Tümleştirme özellikleri:

* Standart, Premium, PremiumV2 veya Elastik Premium fiyatlandırma planı gerektirir.
* TCP ve UDP'yi destekleyin.
* Azure Uygulama Hizmeti uygulamaları ve işlev uygulamalarıyla çalışın.

VNet Tümleştirmesi'nin desteklemediği bazı şeyler vardır, şunları gibi:

* Bir sürücü monte.
* Etkin Dizin entegrasyonu.
* Netbıos.

Ağ geçidi gerekli VNet Tümleştirmesi, kaynaklara yalnızca hedef VNet'teki veya hedef VNet'e bağlı ağlarda eşleme veya VPN'lerle erişim sağlar. Ağ geçidi gerekli VNet Tümleştirmesi, Azure ExpressRoute bağlantılarında kullanılabilen kaynaklara erişim emamesi veya hizmet bitiş noktalarıyla çalışmasını sağlamaz.

Kullanılan sürümden bağımsız olarak, VNet Tümleştirme seçeneğinize VNet' inizdeki kaynaklara erişimi verir, ancak uygulamanıza VNet' ten gelen özel erişime izin vermez. Özel site erişimi, uygulamanızı azure vnet'in içinde olduğu gibi yalnızca özel bir ağdan erişilebilir hale getirmek anlamına gelir. VNet Tümleştirme, yalnızca uygulamanızdan VNet'inize giden aramaları yapmak içindir.

<!--Links-->
[ASEintro]: https://docs.microsoft.com/azure/app-service/environment/intro
[Networkingfeatures]: https://docs.microsoft.com/azure/app-service/networking-features
