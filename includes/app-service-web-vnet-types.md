---
author: ccompy
ms.service: app-service-web
ms.topic: include
ms.date: 02/27/2020
ms.author: ccompy
ms.openlocfilehash: 41b7c7163ff56c848b1b66e606c06ba45d36e610
ms.sourcegitcommit: 7581df526837b1484de136cf6ae1560c21bf7e73
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/31/2020
ms.locfileid: "80419529"
---
* İzole dışında tüm fiyatlandırma planlarını destekleyen çok kiracılı sistemler.
* Sanal ağınıza dağılan ve Yalıtılmış fiyatlandırma planı uygulamalarını destekleyen Uygulama Hizmet Ortamı.

VNet Tümleştirme özelliği çok kiracılı uygulamalarda kullanılır. Uygulamanız App [Service][ASEintro]Environment'daysa, zaten sanal bir ağdadır ve aynı sanal ağdaki kaynaklara ulaşmak için VNet Tümleştirme özelliğinin kullanılması gerekmez. Tüm ağ özellikleri hakkında daha fazla bilgi için [Uygulama Hizmeti ağ özelliklerine][Networkingfeatures]bakın.

VNet Tümleştirme, uygulamanızın sanal ağınızdaki kaynaklara erişmesini sağlar, ancak uygulamanız için sanal ağdan gelen özel erişim izni vermez. Özel site erişimi, bir uygulamayı yalnızca Azure sanal ağındaki gibi özel bir ağdan erişilebilir hale getirmek anlamına gelir. VNet Tümleştirme, yalnızca uygulamanızdan sanal ağınıza giden aramaları yapmak için kullanılır. VNet Tümleştirme özelliği, aynı bölgedeki sanal ağlarla ve diğer bölgelerdeki sanal ağlarla kullanıldığında farklı şekilde kullanılır. VNet Tümleştirme özelliğinin iki çeşidi vardır:

* **Bölgesel VNet Tümleştirmesi**: Aynı bölgedeki Azure Kaynak Yöneticisi sanal ağlarına bağlandığınızda, entegre olduğunuz sanal ağda özel bir alt ağınız olmalıdır.
* **Ağ Geçidi gerekli VNet Tümleştirmesi**: Diğer bölgelerdeki sanal ağlara veya aynı bölgedeki klasik bir sanal ağa bağlandığınızda, hedef sanal ağda sağlanan bir Azure Sanal Ağ ağ geçidine ihtiyacınız vardır.

VNet Tümleştirme özellikleri:

* Standart, Premium, PremiumV2 veya Elastik Premium fiyatlandırma planı gerektirir.
* TCP ve UDP'yi destekleyin.
* Azure Uygulama Hizmeti uygulamaları ve işlev uygulamalarıyla çalışın.

VNet Tümleştirmesi'nin desteklemediği bazı şeyler vardır, şunları gibi:

* Bir sürücü monte.
* Etkin Dizin entegrasyonu.
* Netbıos.

Ağ geçidi gerekli VNet Tümleştirmesi, kaynaklara yalnızca hedef sanal ağdaki veya hedefleme sanal ağa bağlı ağlarda, eşleme veya VPN'lerle erişim sağlar. Ağ geçidi gerekli VNet Tümleştirmesi, Azure ExpressRoute bağlantılarında kullanılabilen kaynaklara erişim emamesi veya hizmet bitiş noktalarıyla çalışmasını sağlamaz.

Kullanılan sürümden bağımsız olarak, VNet Tümleştirme uygulamanız sanal ağınızdaki kaynaklara erişim sağlar, ancak sanal ağdan uygulamanız için gelen özel erişim izni vermez. Özel site erişimi, uygulamanızı yalnızca Azure sanal ağındaki gibi özel bir ağdan erişilebilir hale getirmek anlamına gelir. VNet Tümleştirme, yalnızca uygulamanızdan sanal ağınıza giden aramalar yapmak içindir.

<!--Links-->
[ASEintro]: https://docs.microsoft.com/azure/app-service/environment/intro
[Networkingfeatures]: https://docs.microsoft.com/azure/app-service/networking-features
