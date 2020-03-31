---
author: ccompy
ms.service: app-service-web
ms.topic: include
ms.date: 02/27/2020
ms.author: ccompy
ms.openlocfilehash: c731e2a7d4da1a66aabb50b335d526fbb6a0a302
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78671500"
---
Bölgesel VNet Tümleştirme'yi kullanmak uygulamanızın şu şekilde erişimini sağlar:

* entegre ettiğiniz aynı bölgedeki VNet'teki kaynaklar 
* VNet'teki kaynaklar, aynı bölgede bulunan VNet'inize baktı
* hizmet bitiş noktası güvenli hizmetler
* ExpressRoute bağlantıları arasındaki kaynaklar
* bağlı olduğunuz VNet'teki kaynaklar
* ExpressRoute bağlantıları da dahil olmak üzere eşli bağlantılar arasındaki kaynaklar
* özel uç noktalar 

Aynı bölgedeki VNet'lerle VNet Tümleştirme'yi kullanırken aşağıdaki Azure Ağ özelliklerini kullanabilirsiniz:

* Ağ Güvenlik Grupları (NSGs) - Tümleştirme alt ağınıza yerleştirilen bir Ağ Güvenlik Grubu ile giden trafiği engelleyebilirsiniz. Uygulamanıza gelen erişimi sağlamak için VNet Tümleştirmesini kullanamadığınız için gelen kurallar geçerli değildir.
* Rota Tabloları (ÜDR' ler) - Giden trafiği istediğiniz yere göndermek için tümleştirme alt ağına bir rota tablosu yerleştirebilirsiniz. 

Varsayılan olarak, uygulamanız RFC1918 trafiğini yalnızca VNet'inize yönlendirir. Tüm giden trafiğinizi VNet'inize yönlendirmek istiyorsanız, uygulama ayarını WEBSITE_VNET_ROUTE_ALL uygulamanız için uygulayın. Uygulama ayarını yapılandırmak için:

1. Uygulama portalınızdaki Configuration UI'ye gidin. **Yeni uygulama ayarını** seçin
1. Ad alanında **WEBSITE_VNET_ROUTE_ALL** ve Değer alanına **1** yazın

   ![Uygulama ayarını sağlama][4]

1. **Tamam'ı** seçin
1. **Kaydet**’i seçin

Tüm giden trafiğinizi VNet'inize yönlendirirseniz, bu trafik entegrasyon alt ağınıza uygulanan NSG'lere ve ÜD'lere tabi olacaktır. Tüm giden trafiğinizi VNet'inize yönlendirdiğinizde, trafiği başka bir yere gönderecek rotalar sağlamadığınız sürece, giden adresleriniz uygulama özelliklerinizde listelenen giden adresler olmaya devam eder. 

Aynı bölgede VNet'lerle VNet Tümleştirmesi kullanmanın bazı sınırlamaları vardır:

* Küresel eşleme bağlantıları üzerinden kaynaklara erişemezsiniz
* Bu özellik yalnızca PremiumV2 App Service planlarını destekleyen yeni Uygulama Hizmeti ölçek birimlerinden edinilebilir.
* Tümleştirme alt ağı yalnızca bir Uygulama Hizmeti planı tarafından kullanılabilir
* Bu özellik, Uygulama Hizmeti Ortamında bulunan Yalıtılmış plan uygulamaları tarafından kullanılamaz
* Özellik, Bir Kaynak Yöneticisi VNet'te 32 adresli veya daha büyük bir /27 olan kullanılmayan bir alt ağ gerektirir
* Uygulama ve VNet aynı bölgede olmalıdır
* Entegre bir uygulamayla bir VNet'i silemezsiniz. VNet'i silmeden önce tümleştirmeyi kaldırın. 
* VNets ile yalnızca uygulamayla aynı abonelikte entegre olabilirsiniz
* Uygulama Hizmeti planı başına yalnızca bir bölgesel VNet Tümleştirme'niz olabilir. Aynı Uygulama Hizmeti planındaki birden çok uygulama aynı VNet'i kullanabilir. 
* Bölgesel VNet Tümleştirmesi kullanan bir uygulama varken bir uygulamanın veya planın aboneliğini değiştiremezsiniz

Her plan örneği için bir adres kullanılır. Uygulamanızı beş örnekle ölçeklendirseniz, beş adres kullanılır. Atamadan sonra alt ağ boyutu değiştirilemeyeceğinden, uygulamanızın ulaşabileceği ölçeklere uyacak kadar büyük bir alt ağ kullanmanız gerekir. 64 adresli bir /26 önerilen boyutdur. 64 adresli bir /26, 30 örnekli bir Premium planı barındıracaktır. Bir planı yukarı veya aşağı ölçeklendirdiğinizde, kısa bir süre için iki kat daha fazla adrese ihtiyacınız vardır. 

Uygulamalarınızın başka bir plandaki uygulamalarla zaten bağlı olan bir VNet'e ulaşmasını istiyorsanız, önceden varolan VNet Tümleştirmesi tarafından kullanılandan farklı bir alt ağ seçmeniz gerekir.  

Özellik Linux için önizleme. Bu özelliğin Linux formu yalnızca RFC 1918 adreslerine (10.0.0.0/8, 172.16.0.0/12, 192.168.0.0/16) arama yapmayı destekler.

### <a name="web--function-app-for-containers"></a>Kapsayıcılar için Web / Fonksiyon Uygulaması

Uygulamanızı Linux'ta yerleşik görüntülerle barındırArsanız, bölgesel VNet Tümleştirmesi ek değişiklikler yapmadan çalışır. Kapsayıcılar için Web / Fonksiyon Uygulaması kullanıyorsanız, VNet Tümleştirmesi'ni kullanmak için docker resminizi değiştirmeniz gerekir. Docker görüntünüzde, sabit kodlu bir bağlantı noktası numarası kullanmak yerine ANA web sunucusunun dinleme bağlantı noktası olarak PORT ortamı değişkenini kullanın. PORT ortamı değişkeni, kapsayıcı başlatma zamanında platform tarafından otomatik olarak ayarlanır. SSH kullanıyorsanız, bölgesel VNet tümleştirmesini kullanırken SSH daemon'u SSH_PORT ortam değişkeni tarafından belirtilen bağlantı noktası numarasını dinleyecek şekilde yapılandırılmalıdır.  Linux'ta gerekli VNet Entegrasyonu için ağ geçidi desteği yoktur. 

### <a name="service-endpoints"></a>Hizmet Uç Noktaları

Bölgesel VNet Tümleştirmesi, hizmet bitiş noktalarını kullanmanızı sağlar.  Uygulamanızla hizmet uç noktalarını kullanmak için, seçili bir VNet'e bağlanmak için bölgesel VNet Tümleştirmesini kullanın ve ardından tümleştirme için kullandığınız alt ağdaki hizmet uç noktalarını yapılandırın. 

### <a name="network-security-groups"></a>Ağ Güvenlik Grupları

Ağ Güvenlik Grupları, VNet'teki kaynaklara gelen ve giden trafiği engellemenize olanak tanır. Bölgesel VNet Tümleştirmesini kullanan bir uygulama, VNet'inizdeki veya internetteki kaynaklara giden trafiği engellemek için [Ağ Güvenliği Grubu'nu][VNETnsg] kullanabilir. Ortak adreslere gelen trafiği engellemek için uygulama ayarını WEBSITE_VNET_ROUTE_ALL 1 olarak ayarlamanız gerekir. VNet Entegrasyonu yalnızca uygulamanızdan gelen trafiği etkilediği için NSG'deki gelen kurallar uygulamanız için geçerli değildir. Uygulamanıza gelen trafiği denetlemek için Erişim Kısıtlamaları özelliğini kullanın. Tümleştirme alt ağınıza uygulanan bir NSG, tümleştirme alt ağınıza uygulanan rotalardan bağımsız olarak geçerli olacaktır. WEBSITE_VNET_ROUTE_ALL 1 olarak ayarlanmışsa ve tümleştirme alt ağınızdaki genel adres trafiğini etkileyen herhangi bir rotanız yoksa, giden trafiğinizin tümü yine de tümleştirme alt ağınıza atanan NSG'lere tabi olacaktır. WEBSITE_VNET_ROUTE_ALL ayarlanmasaydı, NSG'ler yalnızca RFC1918 trafiğine uygulanırdı.

### <a name="routes"></a>Yollar

Rota Tabloları, giden trafiği uygulamanızdan istediğiniz yere yönlendirmenize olanak tanır. Varsayılan olarak, rota tabloları yalnızca RFC1918 hedef trafiğinizi etkiler.  WEBSITE_VNET_ROUTE_ALL 1 olarak ayarlarsanız, tüm giden çağrılarınız etkilenir. Tümleştirme alt ağınızda ayarlanan rotalar gelen uygulama isteklerine verilen yanıtları etkilemez. Sık kullanılan hedefler arasında güvenlik duvarı aygıtları veya ağ geçitleri yer alabilir. Tüm giden trafiği şirket içinde yönlendirmek istiyorsanız, tüm giden trafiği ExpressRoute ağ geçidinize göndermek için bir rota tablosu kullanabilirsiniz. Trafiği bir ağ geçidine yönlendirin, yanıtları geri göndermek için dış ağdaki yolları ayarladığınızdan emin olun.

Kenarlık Ağ Geçidi Protokolü (BGP) yolları da uygulama trafiğinizi etkiler. ExpressRoute ağ geçidi gibi bir şeyden BGP rotalarınız varsa, uygulamagiden trafiğiniz etkilenir. Varsayılan olarak, BGP rotaları yalnızca RFC1918 hedef trafiğinizi etkiler. WEBSITE_VNET_ROUTE_ALL 1 olarak ayarlanmışsa, tüm giden trafik BGP rotalarınızdan etkilenebilir. 


<!--Image references-->
[4]: ../includes/media/web-sites-integrate-with-vnet/vnetint-appsetting.png

<!--Links-->
[VNETnsg]: https://docs.microsoft.com/azure/virtual-network/security-overview/