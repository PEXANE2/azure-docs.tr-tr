---
author: ccompy
ms.service: app-service-web
ms.topic: include
ms.date: 02/27/2020
ms.author: ccompy
ms.openlocfilehash: e0db3ce7d31b838ca6f7d566083a33ee215d3399
ms.sourcegitcommit: 7581df526837b1484de136cf6ae1560c21bf7e73
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/31/2020
ms.locfileid: "80419563"
---
Bölgesel VNet Tümleştirme'yi kullanmak uygulamanızın şu şekilde erişimini sağlar:

* Entegre ettiğiniz aynı bölgedeki sanal ağdaki kaynaklar.
* Sanal ağlardaki kaynaklar, aynı bölgede bulunan sanal ağınıza bakar.
* Hizmet bitiş noktası güvenli hizmetler.
* Azure ExpressRoute bağlantılarında kaynaklar.
* Bağlı olduğunuz sanal ağdaki kaynaklar.
* Azure ExpressRoute bağlantılarını içeren eşli bağlantılar arasındaki kaynaklar.
* Özel uç noktalar.

Aynı bölgedeki sanal ağlarla VNet Tümleştirme'yi kullandığınızda, aşağıdaki Azure ağ özelliklerini kullanabilirsiniz:

* **Ağ güvenlik grupları (NSGs)**: Tümleştirme alt ağınıza yerleştirilen bir NSG ile giden trafiği engelleyebilirsiniz. Gelen kurallar geçerli değildir, çünkü uygulamanıza gelen erişimi sağlamak için VNet Tümleştirmesini kullanamazsınız.
* **Rota tabloları (ÜDR' ler)**: Giden trafiği istediğiniz yere göndermek için tümleştirme alt ağına bir rota tablosu yerleştirebilirsiniz.

Varsayılan olarak, uygulamanız sanal ağınıza yalnızca RFC1918 trafiğini yönlendirir. Tüm giden trafiğinizi sanal ağınıza yönlendirmek istiyorsanız, uygulama ayarını uygulamanız WEBSITE_VNET_ROUTE_ALL uygulayın. Uygulama ayarını yapılandırmak için:

1. Uygulama portalınızdaki **Configuration** UI'ye gidin. **Yeni uygulama ayarını**seçin.
1. **Ad** kutusuna **WEBSITE_VNET_ROUTE_ALL** girin ve **Değer** kutusuna **1** girin.

   ![Uygulama ayarını sağlama][4]

1. **Tamam'ı**seçin.
1. **Kaydet'i**seçin.

Tüm giden trafiğinizi sanal ağınıza yönlendirirseniz, bu trafik, tümleştirme alt ağınıza uygulanan NSG'lere ve ÜD'lere bağlıdır. Tüm giden trafiğinizi sanal ağınıza yönlendirdiğinizde, trafiği başka bir yere gönderecek rotalar sağlamadığınız sürece, giden adresleriniz hala uygulama özelliklerinizde listelenen giden adreslerdir.

VNet Tümleştirmesinin aynı bölgedeki sanal ağlarla kullanılmasında bazı sınırlamalar vardır:

* Genel bakış bağlantıları üzerinden kaynaklara erişemezsiniz.
* Bu özellik yalnızca PremiumV2 Uygulama Hizmeti planlarını destekleyen yeni Azure App Service ölçek birimlerinden edinilebilir.
* Tümleştirme alt ağı yalnızca bir Uygulama Hizmeti planı tarafından kullanılabilir.
* Bu özellik, Uygulama Hizmeti Ortamında bulunan Yalıtılmış plan uygulamaları tarafından kullanılamaz.
* Özellik, bir Azure Kaynak Yöneticisi sanal ağında 32 adresli veya daha büyük bir /27 olan kullanılmayan bir alt ağ gerektirir.
* Uygulama ve sanal ağ aynı bölgede olmalıdır.
* Entegre bir uygulamayla sanal ağı silemezsiniz. Sanal ağı silmeden önce tümleştirmeyi kaldırın.
* Sanal ağlarla yalnızca uygulamayla aynı abonelikte tümleştirebilirsiniz.
* Uygulama Hizmeti planı başına yalnızca bir bölgesel VNet Tümleştirme'niz olabilir. Aynı Uygulama Hizmeti planındaki birden çok uygulama aynı sanal ağı kullanabilir.
* Bölgesel VNet Tümleştirmesi kullanan bir uygulama varken bir uygulamanın veya planın aboneliğini değiştiremezsiniz.

Her plan örneği için bir adres kullanılır. Uygulamanızı beş örnekle ölçeklendirseniz, beş adres kullanılır. Atamadan sonra alt ağ boyutu değiştirilemediğinden, uygulamanızın ulaşabileceği ölçeklere uyacak kadar büyük bir alt ağ kullanmanız gerekir. 64 adresli bir /26 önerilen boyutdur. 64 adresli bir /26, 30 örnekli bir Premium planı barındırır. Bir planı yukarı veya aşağı ölçeklendirdiğinizde, kısa bir süre için iki kat daha fazla adrese ihtiyacınız vardır.

Uygulamalarınızın başka bir plandaki uygulamalara zaten bağlı olan sanal bir ağa ulaşmasını istiyorsanız, önceden varolan VNet Tümleştirmesi tarafından kullanılandan farklı bir alt ağ seçin.

Özellik Linux için önizleme. Bu özelliğin Linux formu yalnızca RFC 1918 adreslerine (10.0.0.0/8, 172.16.0.0/12, 192.168.0.0/16) arama yapmayı destekler.

### <a name="web-or-function-app-for-containers"></a>Kapsayıcılar için Web veya İşlev Uygulaması

Uygulamanızı Linux'ta yerleşik görüntülerle barındırArsanız, bölgesel VNet Tümleştirmesi ek değişiklikler yapmadan çalışır. Kapsayıcılar için Web veya İşlev Uygulaması kullanıyorsanız, Docker resminizi VNet Tümleştirmesi'ni kullanmak üzere değiştirmeniz gerekir. Docker görüntünüzde, sabit kodlu bir bağlantı noktası numarası kullanmak yerine ANA web sunucusunun dinleme bağlantı noktası olarak PORT ortamı değişkenini kullanın. PORT ortamı değişkeni, kapsayıcı başlatma zamanında platform tarafından otomatik olarak ayarlanır. SSH kullanıyorsanız, Bölgesel VNet Tümleştirmesi'ni kullandığınızda SSH daemon' un, SSH_PORT çevre değişkeni tarafından belirtilen bağlantı noktası numarasını dinleyecek şekilde yapılandırılmalıdır. Linux'ta ağ geçidi için gerekli VNet Entegrasyonu için destek yoktur.

### <a name="service-endpoints"></a>Hizmet uç noktaları

Bölgesel VNet Tümleştirmesi ile hizmet bitiş noktalarını kullanabilirsiniz. Uygulamanızla hizmet bitiş noktalarını kullanmak için, seçili bir sanal ağa bağlanmak için bölgesel VNet Tümleştirmesini kullanın. Ardından, tümleştirme için kullandığınız alt ağdaki hizmet uç noktalarını yapılandırın.

### <a name="network-security-groups"></a>Ağ güvenlik grupları

Sanal ağdaki kaynaklara gelen ve giden trafiği engellemek için ağ güvenlik gruplarını kullanabilirsiniz. Bölgesel VNet Tümleştirmesi kullanan bir uygulama, sanal ağınızdaki veya internetteki kaynaklara giden trafiği engellemek için bir [ağ güvenlik grubu][VNETnsg] kullanabilir. Ortak adreslere gelen trafiği engellemek için uygulama ayarını WEBSITE_VNET_ROUTE_ALL 1 olarak ayarlamanız gerekir. VNet Tümleştirmesi uygulamanızdan yalnızca giden trafiği etkilediği için NSG'deki gelen kurallar uygulamanız için geçerli değildir.

Uygulamanıza gelen trafiği denetlemek için Erişim Kısıtlamaları özelliğini kullanın. Tümleştirme alt ağınıza uygulanan bir NSG, tümleştirme alt ağınıza uygulanan rotalardan bağımsız olarak etkindir. WEBSITE_VNET_ROUTE_ALL 1 olarak ayarlanmışsa ve tümleştirme alt ağınızdaki genel adres trafiğini etkileyen herhangi bir rotanız yoksa, giden trafiğinizin tümü yine de tümleştirme alt ağınıza atanan NSG'lere tabidir. WEBSITE_VNET_ROUTE_ALL ayarlı değilse, NSG'ler yalnızca RFC1918 trafiğine uygulanır.

### <a name="routes"></a>Yollar

Giden trafiği uygulamanızdan istediğiniz yere yönlendirmek için rota tablolarını kullanabilirsiniz. Varsayılan olarak, rota tabloları yalnızca RFC1918 hedef trafiğinizi etkiler. WEBSITE_VNET_ROUTE_ALL 1 olarak ayarlarsanız, tüm giden çağrılarınız etkilenir. Tümleştirme alt ağınızda ayarlanan rotalar gelen uygulama isteklerine verilen yanıtları etkilemez. Sık kullanılan hedefler arasında güvenlik duvarı aygıtları veya ağ geçitleri yer alabilir.

Tüm giden trafiği şirket içinde yönlendirmek istiyorsanız, tüm giden trafiği ExpressRoute ağ geçidinize göndermek için bir rota tablosu kullanabilirsiniz. Trafiği bir ağ geçidine yönlendirin, yanıtları geri göndermek için dış ağdaki yolları ayarladığınızdan emin olun.

Kenarlık Ağ Geçidi Protokolü (BGP) yolları da uygulama trafiğinizi etkiler. ExpressRoute ağ geçidi gibi bir şeyden BGP rotalarınız varsa, uygulamagiden trafiğiniz etkilenir. Varsayılan olarak, BGP rotaları yalnızca RFC1918 hedef trafiğinizi etkiler. WEBSITE_VNET_ROUTE_ALL 1 olarak ayarlanmışsa, tüm giden trafik BGP rotalarınızdan etkilenebilir.


<!--Image references-->
[4]: ../includes/media/web-sites-integrate-with-vnet/vnetint-appsetting.png

<!--Links-->
[VNETnsg]: https://docs.microsoft.com/azure/virtual-network/security-overview/