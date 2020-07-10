---
author: ccompy
ms.service: app-service-web
ms.topic: include
ms.date: 06/08/2020
ms.author: ccompy
ms.openlocfilehash: 926a1867a77b543057fa1de170cdb64ccfefe7cb
ms.sourcegitcommit: ec682dcc0a67eabe4bfe242fce4a7019f0a8c405
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/09/2020
ms.locfileid: "86218160"
---
Bölgesel VNet tümleştirmesini kullanmak, uygulamanızın erişmesini sağlar:

* Uygulamanızla aynı bölgedeki bir VNet 'teki kaynaklar.
* Sanal ağlardaki VNet 'lerdeki kaynaklar, uygulamanızın tümleştirildiği VNet 'e tümleştirilir.
* Hizmet uç noktası güvenliği sağlanmış hizmetler.
* Azure ExpressRoute bağlantıları genelindeki kaynaklar.
* Tümleştirmiş olduğunuz VNet 'teki kaynaklar.
* Azure ExpressRoute bağlantıları içeren, eşlenen bağlantılar arasındaki kaynaklar.
* Özel uç noktalar 

VNet ile VNet tümleştirmesini aynı bölgede kullandığınızda, aşağıdaki Azure ağ özelliklerini kullanabilirsiniz:

* **Ağ güvenlik grupları (NSG 'ler)**: Tümleştirme alt ağınıza yerleştirilmiş bir NSG ile giden trafiği engelleyebilirsiniz. Uygulama için gelen erişimi sağlamak üzere VNet tümleştirmesini kullanamadığından gelen kuralları uygulanmaz.
* **Yol tabloları (UDRs)**: istediğiniz yere giden trafik göndermek için tümleştirme alt ağına bir rota tablosu yerleştirebilirsiniz.

Varsayılan olarak, uygulamanız yalnızca sanal ağınıza RFC1918 trafiği yönlendirir. Tüm giden trafiğinizi sanal ağınıza yönlendirmek istiyorsanız WEBSITE_VNET_ROUTE_ALL uygulama ayarını uygulamanıza uygulayın. Uygulama ayarını yapılandırmak için:

1. Uygulama portalınızdaki **yapılandırma** Kullanıcı arabirimine gidin. **Yeni uygulama ayarı**'nı seçin.
1. **Ad** kutusuna **WEBSITE_VNET_ROUTE_ALL** girin ve **değer** kutusuna **1** yazın.

   ![Uygulama ayarı sağla][4]

1. **Tamam**’ı seçin.
1. **Kaydet**’i seçin.

Tüm giden trafiğinizi sanal ağınıza yönlendirdiğinizde, tümleştirme alt ağınıza uygulanan NSG 'ler ve UDRs 'ye tabidir. Tüm giden trafiğinizi sanal ağınıza yönlendirdiğinizde, trafiği başka bir yere göndermek için yollar sağlamazsanız, giden adresleriniz hala uygulama özelliklerinde listelenen giden adreslerdir.

Aynı bölgedeki sanal ağlar ile VNet tümleştirmesi kullanımıyla ilgili bazı sınırlamalar vardır:

* Genel eşleme bağlantıları genelindeki kaynaklara ulaşıamazsınız.
* Özelliği yalnızca PremiumV2 App Service planlarını destekleyen daha yeni Azure App Service ölçek birimlerinden kullanılabilir. Bu, *uygulamanızın bir PremiumV2 fiyatlandırma katmanında çalıştırılması gerektiğini*, yalnızca PremiumV2 seçeneğinin kullanılabildiği bir App Service planında çalışması gerektiğini (Yani bu VNET tümleştirme özelliğinin de kullanılabilir olduğu daha yeni bir ölçek birimi olduğunu gösterir) unutmayın.
* Tümleştirme alt ağı yalnızca bir App Service planı tarafından kullanılabilir.
* Özelliği, bir App Service Ortamı olan yalıtılmış plan uygulamaları tarafından kullanılamaz.
* Bu özellik, bir Azure Resource Manager VNet 'te 32 adresi veya daha büyük bir/27 veya daha büyük olmayan kullanılmayan bir alt ağ gerektirir.
* Uygulama ve VNet aynı bölgede olmalıdır.
* Tümleşik bir uygulamayla VNet 'i silemezsiniz. VNet 'i silmeden önce tümleştirmeyi kaldırın.
* Yalnızca uygulama ile aynı abonelikte VNET 'ler ile tümleştirilebilir.
* App Service planı başına yalnızca bir bölgesel VNet tümleştirmesi olabilir. Aynı App Service planındaki birden çok uygulama aynı VNet 'i kullanabilir.
* Bölgesel VNet tümleştirmesi kullanan bir uygulama varken, bir uygulamanın veya planın aboneliğini değiştiremezsiniz.
* Uygulamanız, yapılandırma değişiklikleri olmadan Azure DNS Özel Bölgeleri adresleri çözemez

Her plan örneği için bir adres kullanılır. Uygulamanızı beş örneğe ölçeklendirirseniz, beş adres kullanılır. Alt ağ boyutu atamadan sonra değiştirilemediğinden, uygulamanızın ulaşabileceği ölçeğe uyum sağlayacak kadar büyük bir alt ağ kullanmanız gerekir. 64 adresi olan bir/26 önerilen boyutdir. 64 adresi olan bir/26, 30 örnek içeren bir Premium planı karşılar. Bir planı yukarı veya aşağı ölçeklendirirseniz, kısa bir süre için birçok adrese iki kez ihtiyacınız vardır.

Farklı bir plandaki uygulamalarınızın, başka bir plandaki uygulamalar tarafından zaten bağlı olan bir VNet 'e ulaşmasını istiyorsanız, önceden var olan VNet tümleştirmesi tarafından kullanılandan farklı bir alt ağ seçin.

Özelliği, hem Windows hem de Linux Web Apps için tam olarak desteklenmektedir. Tüm davranışlar Windows Uygulamaları ve Linux uygulamaları arasında aynı şekilde davranır.

### <a name="service-endpoints"></a>Hizmet uç noktaları

Bölgesel VNet tümleştirmesi, hizmet uç noktalarını kullanmanıza olanak sağlar. Hizmet uç noktalarını uygulamanızla birlikte kullanmak için, seçili bir sanal ağa bağlanmak üzere bölgesel VNet tümleştirmesini kullanın ve ardından tümleştirme için kullandığınız alt ağdaki hedef hizmetle birlikte hizmet uç noktalarını yapılandırın. Daha sonra hizmet uç noktaları üzerinden bir hizmete erişmek istiyorsanız:

1. Web uygulamanızla bölgesel VNet tümleştirmesini yapılandırma
1. hedef hizmete gidin ve hizmet uç noktalarını tümleştirme için kullanılan alt ağa karşı yapılandırın

### <a name="network-security-groups"></a>Ağ güvenlik grupları

Ağ güvenlik gruplarını, bir sanal ağdaki kaynaklara gelen ve giden trafiği engellemek için kullanabilirsiniz. Bölgesel VNet tümleştirmesi kullanan bir uygulama, VNet veya Internet 'teki kaynaklara giden trafiği engellemek için bir [ağ güvenlik grubu][VNETnsg] kullanabilir. Ortak adreslere giden trafiği engellemek için WEBSITE_VNET_ROUTE_ALL uygulama ayarının 1 olarak ayarlanmış olması gerekir. VNet tümleştirmesi uygulamanızdan yalnızca giden trafiği etkilediği için NSG 'deki gelen kuralları uygulamanıza uygulanmaz.

Uygulamanıza gelen trafiği denetlemek için erişim kısıtlamaları özelliğini kullanın. Tümleştirme alt ağına uygulanan bir NSG, tümleştirme alt ağına uygulanan rotalara bakılmaksızın etkili olur. WEBSITE_VNET_ROUTE_ALL 1 olarak ayarlanmışsa ve tümleştirme alt ağınızda ortak adres trafiğini etkileyen bir yolunuz yoksa, tüm giden trafiğiniz hala tümleştirme alt ağına atanmış olan NSG 'lere tabidir. WEBSITE_VNET_ROUTE_ALL ayarlanmamışsa, NSG 'ler yalnızca RFC1918 trafiğe uygulanır.

### <a name="routes"></a>Yollar

Uygulama, giden trafiği uygulamanızdaki her yere yönlendirmek için yol tabloları ' nı kullanabilirsiniz. Varsayılan olarak, yol tabloları yalnızca RFC1918 hedef trafiğinizi etkiler. WEBSITE_VNET_ROUTE_ALL 1 olarak ayarlarsanız tüm giden çağrılarınız etkilenir. Tümleştirme alt ağınızda ayarlanan yollar, gelen uygulama isteklerine olan yanıtları etkilemez. Ortak hedefler, güvenlik duvarı cihazları veya ağ geçitleri içerebilir.

Şirket içinde tüm giden trafiği yönlendirmek isterseniz, tüm giden trafiği ExpressRoute ağ geçidize göndermek için bir yol tablosu kullanabilirsiniz. Trafiği bir ağ geçidine yönlendirdiğinizde, dış ağdaki yolları, yanıtları geri gönderecek şekilde ayarladığınızdan emin olun.

Sınır Ağ Geçidi Protokolü (BGP) rotaları de uygulama trafiğinizi etkiler. ExpressRoute ağ geçidine benzer bir şekilde BGP yollarınız varsa, uygulamanızın giden trafiği etkilenecektir. Varsayılan olarak BGP yolları yalnızca RFC1918 hedef trafiğinizi etkiler. WEBSITE_VNET_ROUTE_ALL 1 olarak ayarlanırsa, tüm giden trafik BGP rotalarınız tarafından etkilenebilir.

### <a name="azure-dns-private-zones"></a>Azure DNS Özel Bölgeleri 

Uygulamanız VNet 'iniz ile tümleştirdikten sonra, sanal ağınızın yapılandırıldığı aynı DNS sunucusunu kullanır. Varsayılan olarak, uygulamanız Azure DNS Özel Bölgeleri çalışmaz. Azure DNS Özel Bölgeleri çalışmak için aşağıdaki uygulama ayarlarını eklemeniz gerekir:

1. 168.63.129.16 değeri ile WEBSITE_DNS_SERVER 
1. Değer 1 ile WEBSITE_VNET_ROUTE_ALL

Bu ayarlar, uygulamanızın özel bölgeler Azure DNS kullanmasını olanaklı hale getirecek ek olarak, uygulamanızdaki tüm giden çağrılarınızı sanal ağınıza gönderir.

### <a name="private-endpoints"></a>Özel uç noktalar

[Özel uç noktalara][privateendpoints]çağrılar yapmak istiyorsanız, Azure DNS özel bölgeleri ile tümleştirmeniz veya uygulamanız tarafından kullanılan DNS sunucusunda özel uç noktasını yönetmeniz gerekir. 

<!--Image references-->
[4]: ../includes/media/web-sites-integrate-with-vnet/vnetint-appsetting.png

<!--Links-->
[VNETnsg]: https://docs.microsoft.com/azure/virtual-network/security-overview/
[privateendpoints]: https://docs.microsoft.com/azure/app-service/networking/private-endpoint
