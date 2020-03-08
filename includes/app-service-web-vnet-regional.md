---
author: ccompy
ms.service: app-service-web
ms.topic: include
ms.date: 02/27/2020
ms.author: ccompy
ms.openlocfilehash: c731e2a7d4da1a66aabb50b335d526fbb6a0a302
ms.sourcegitcommit: bc792d0525d83f00d2329bea054ac45b2495315d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78671500"
---
Bölgesel VNet tümleştirmesini kullanmak, uygulamanızın erişmesini sağlar:

* aynı bölgedeki VNet 'teki kaynaklar 
* VNet 'lerdeki kaynaklar aynı bölgedeki VNet 'iniz ile eşlenmez
* hizmet uç noktası güvenliği sağlanmış hizmetler
* ExpressRoute bağlantıları genelindeki kaynaklar
* bağlı olduğunuz VNet 'teki kaynaklar
* ExpressRoute bağlantıları dahil eşlenen bağlantı genelindeki kaynaklar
* Özel uç noktalar 

Sanal ağlar ile VNet tümleştirmesi aynı bölgede kullanılırken, aşağıdaki Azure ağ özelliklerini kullanabilirsiniz:

* Ağ güvenlik grupları (NSG 'ler)-tümleştirme alt ağınıza yerleştirilmiş bir ağ güvenlik grubuyla giden trafiği engelleyebilirsiniz. Uygulama için gelen erişimi sağlamak üzere VNet tümleştirmesini kullanistemediğiniz için gelen kuralları uygulanmaz.
* Yol tabloları (UDRs)-istediğiniz yere giden trafik göndermek için tümleştirme alt ağına bir rota tablosu yerleştirebilirsiniz. 

Varsayılan olarak, uygulamanız RFC1918 trafiğini yalnızca sanal ağınıza yönlendirirsiniz. Tüm giden trafiğinizi sanal ağınıza yönlendirmek istiyorsanız WEBSITE_VNET_ROUTE_ALL uygulama ayarını uygulamanıza uygulayın. Uygulama ayarını yapılandırmak için:

1. Uygulama portalınızdaki yapılandırma kullanıcı arabirimine gidin. **Yeni uygulama ayarı** seçin
1. Ad alanına **WEBSITE_VNET_ROUTE_ALL** yazın ve değer alanına **1** yazın

   ![Uygulama ayarı sağla][4]

1. **Tamam**’ı seçin
1. **Kaydet**’i seçin

Tüm giden trafiğinizi sanal ağınıza yönlendirdiğinizde, tümleştirme alt ağınıza uygulanan NSG 'ler ve UDRs 'ye tabi olur. Tüm giden trafiğinizi sanal ağınıza yönlendirdiğinizde, trafiği başka bir yere göndermek için yollar sağlamadığınız müddetçe giden adresleriniz, uygulama özelliklerinde listelenen giden adresler olmaya devam eder. 

Aynı bölgedeki sanal ağlar ile VNet tümleştirmesi kullanımıyla ilgili bazı sınırlamalar vardır:

* Genel eşleme bağlantıları genelindeki kaynaklara ulaşılamıyor
* Özelliği yalnızca PremiumV2 App Service planlarını destekleyen daha yeni App Service ölçek birimlerinden kullanılabilir.
* Tümleştirme alt ağı yalnızca bir App Service planı tarafından kullanılabilir
* Özellik, App Service Ortamı yalıtılmış plan uygulamaları tarafından kullanılamaz
* Özellik, bir Kaynak Yöneticisi VNet 'te 32 adresi veya daha büyük bir/27 olan kullanılmayan bir alt ağ gerektiriyor
* Uygulama ve VNet aynı bölgede olmalıdır
* Tümleşik bir uygulamayla VNet 'i silemezsiniz. VNet 'i silmeden önce tümleştirmeyi kaldırın. 
* Yalnızca uygulamayla aynı abonelikte VNET 'ler ile tümleştirilebilir
* App Service planı başına yalnızca bir bölgesel VNet tümleştirmesi olabilir. Aynı App Service planındaki birden çok uygulama aynı VNet 'i kullanabilir. 
* Bölgesel VNet tümleştirmesi kullanan bir uygulama varken bir uygulamanın veya planın aboneliğini değiştiremezsiniz

Her plan örneği için bir adres kullanılır. Uygulamanızı beş örneğe ölçeklendirirseniz, beş adres kullanılır. Alt ağ boyutu atamadan sonra değiştirilemediğinden, uygulamanızın ulaşabileceği ölçeğe uyum sağlayacak kadar büyük bir alt ağ kullanmanız gerekir. 64 adresi olan bir/26 önerilen boyutdir. 64 adresi olan bir/26, 30 örneğe sahip Premium bir plana sahip olur. Bir planı yukarı veya aşağı ölçeklendirirseniz, kısa bir süre için birçok adrese iki kez ihtiyacınız vardır. 

Başka bir plandaki uygulamalarınızın zaten başka bir plandaki uygulamalara bağlı olan bir VNet 'e ulaşmasını istiyorsanız, önceden var olan VNet tümleştirmesi tarafından kullanılandan farklı bir alt ağ seçmeniz gerekir.  

Özelliği, Linux için önizlemededir. Özelliğin Linux formu yalnızca RFC 1918 adreslerine çağrı yapmayı destekler (10.0.0.0/8, 172.16.0.0/12, 192.168.0.0/16).

### <a name="web--function-app-for-containers"></a>Kapsayıcılar için Web/İşlev Uygulaması

Uygulamanızı Linux 'ta yerleşik görüntülerle barındırdıysanız bölgesel VNet tümleştirmesi ek değişiklik yapılmadan işe yarar. Kapsayıcılar için Web/İşlev Uygulaması kullanıyorsanız, VNet tümleştirmesini kullanmak için Docker görüntünüzü değiştirmeniz gerekir. Docker yansımanıza, sabit kodlanmış bir bağlantı noktası numarası kullanmak yerine ana Web sunucusunun dinleme bağlantı noktası olarak bağlantı noktası ortam değişkenini kullanın. Bağlantı noktası ortam değişkeni, kapsayıcı başlangıç saatinde platform tarafından otomatik olarak ayarlanır. SSH kullanıyorsanız, SSH Daemon 'ın bölgesel VNet tümleştirmesi kullanılırken SSH_PORT ortam değişkeni tarafından belirtilen bağlantı noktası numarasını dinlemek üzere yapılandırılması gerekir.  Linux üzerinde ağ geçidi gerekli VNet tümleştirmesi desteği yoktur. 

### <a name="service-endpoints"></a>Hizmet uç noktaları

Bölgesel VNet tümleştirmesi, hizmet uç noktalarını kullanmanıza olanak sağlar.  Hizmet uç noktalarını uygulamanızla birlikte kullanmak için, seçili bir sanal ağa bağlanmak üzere bölgesel VNet tümleştirmesini kullanın ve ardından tümleştirme için kullandığınız alt ağdaki hizmet uç noktalarını yapılandırın. 

### <a name="network-security-groups"></a>Ağ Güvenlik Grupları

Ağ güvenlik grupları, bir sanal ağdaki kaynaklara gelen ve giden trafiği engellemenize olanak tanır. Bölgesel VNet tümleştirmesinin kullanıldığı bir uygulama, VNet veya Internet 'teki kaynaklara giden trafiği engellemek için [ağ güvenlik grubunu][VNETnsg] kullanabilir. Ortak adreslere giden trafiği engellemek için WEBSITE_VNET_ROUTE_ALL uygulama ayarının 1 olarak ayarlanmış olması gerekir. VNet tümleştirmesi yalnızca uygulamanızdan giden trafiği etkilediği için bir NSG 'deki gelen kuralları uygulamanıza uygulanmaz. Uygulamanıza gelen trafiği denetlemek için erişim kısıtlamaları özelliğini kullanın. Tümleştirme alt ağına uygulanan bir NSG, tümleştirme alt ağına uygulanan rotalara bakılmaksızın etkili olacaktır. WEBSITE_VNET_ROUTE_ALL 1 olarak ayarlanmışsa ve tümleştirme alt ağınızda ortak adres trafiğini etkileyen bir yolunuz yoksa, tüm giden trafiğiniz tümleştirme alt ağına atanan NSG 'lere tabidir. WEBSITE_VNET_ROUTE_ALL ayarlanmamışsa, NSG 'ler yalnızca RFC1918 trafiğe uygulanır.

### <a name="routes"></a>Yollar

Yol tabloları, uygulamanızdaki giden trafiği istediğiniz yere yönlendirmenize olanak tanır. Varsayılan olarak, yol tabloları yalnızca RFC1918 hedef trafiğinizi etkiler.  WEBSITE_VNET_ROUTE_ALL 1 olarak ayarlarsanız tüm giden çağrılarınız etkilenecektir. Tümleştirme alt ağınızda ayarlanan yollar, gelen uygulama isteklerine yapılan yanıtları etkilemez. Ortak hedefler, güvenlik duvarı cihazları veya ağ geçitleri içerebilir. Şirket içinde tüm giden trafiği yönlendirmek isterseniz, tüm giden trafiği ExpressRoute ağ geçidize göndermek için bir yol tablosu kullanabilirsiniz. Trafiği bir ağ geçidine yönlendirdiğinizde, dış ağdaki yolları, yanıtları geri gönderecek şekilde ayarladığınızdan emin olun.

Sınır Ağ Geçidi Protokolü (BGP) yolları da uygulama trafiğinizi etkileyecektir. ExpressRoute ağ geçidine benzer bir şekilde BGP yollarınız varsa, uygulamanızın giden trafiği etkilenecektir. Varsayılan olarak BGP yolları yalnızca RFC1918 hedef trafiğinizi etkiler. WEBSITE_VNET_ROUTE_ALL 1 olarak ayarlanırsa, tüm giden trafik BGP rotalarınız tarafından etkilenebilir. 


<!--Image references-->
[4]: ../includes/media/web-sites-integrate-with-vnet/vnetint-appsetting.png

<!--Links-->
[VNETnsg]: https://docs.microsoft.com/azure/virtual-network/security-overview/