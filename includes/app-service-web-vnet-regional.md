---
author: ccompy
ms.service: app-service-web
ms.topic: include
ms.date: 04/15/2020
ms.author: ccompy
ms.openlocfilehash: f7208307df51ecefb76f9adaedea59b327cdc19e
ms.sourcegitcommit: 5e49f45571aeb1232a3e0bd44725cc17c06d1452
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/17/2020
ms.locfileid: "81604859"
---
Bölgesel VNet Tümleştirme'yi kullanmak uygulamanızın şu şekilde erişimini sağlar:

* Uygulamanızla aynı bölgedeki bir VNet'teki kaynaklar.
* VNet'teki kaynaklar, uygulamanızın entegre olduğu VNet'e bakar.
* Hizmet bitiş noktası güvenli hizmetler.
* Azure ExpressRoute bağlantılarında kaynaklar.
* Entegre olduğunuz VNet'teki kaynaklar.
* Azure ExpressRoute bağlantılarını içeren eşli bağlantılar arasındaki kaynaklar.
* Özel uç noktalar 

Aynı bölgedeki VNet'lerle VNet Tümleştirme'yi kullandığınızda, aşağıdaki Azure ağ özelliklerini kullanabilirsiniz:

* **Ağ güvenlik grupları (NSGs)**: Tümleştirme alt ağınıza yerleştirilen bir NSG ile giden trafiği engelleyebilirsiniz. Gelen kurallar geçerli değildir, çünkü uygulamanıza gelen erişimi sağlamak için VNet Tümleştirmesini kullanamazsınız.
* **Rota tabloları (ÜDR' ler)**: Giden trafiği istediğiniz yere göndermek için tümleştirme alt ağına bir rota tablosu yerleştirebilirsiniz.

Varsayılan olarak, uygulamanız VNet'inize yalnızca RFC1918 trafiğini yönlendirir. Tüm giden trafiğinizi VNet'inize yönlendirmek istiyorsanız, uygulama ayarını WEBSITE_VNET_ROUTE_ALL uygulamanız için uygulayın. Uygulama ayarını yapılandırmak için:

1. Uygulama portalınızdaki **Configuration** UI'ye gidin. **Yeni uygulama ayarını**seçin.
1. **Ad** kutusuna **WEBSITE_VNET_ROUTE_ALL** girin ve **Değer** kutusuna **1** girin.

   ![Uygulama ayarını sağlama][4]

1. **Tamam'ı**seçin.
1. **Kaydet**’i seçin.

Tüm giden trafiğinizi VNet'inize yönlendirirseniz, bu trafik, entegrasyon alt ağınıza uygulanan NSG'lere ve ÜD'lere bağlıdır. Tüm giden trafiğinizi VNet'inize yönlendirdiğinizde, trafiği başka bir yere gönderecek rotalar sağlamadığınız sürece, giden adresleriniz hala uygulama özelliklerinizde listelenen giden adreslerdir.

Aynı bölgede VNet'lerle VNet Tümleştirmesi kullanmanın bazı sınırlamaları vardır:

* Genel bakış bağlantıları üzerinden kaynaklara erişemezsiniz.
* Bu özellik yalnızca PremiumV2 Uygulama Hizmeti planlarını destekleyen yeni Azure App Service ölçek birimlerinden edinilebilir.
* Tümleştirme alt ağı yalnızca bir Uygulama Hizmeti planı tarafından kullanılabilir.
* Bu özellik, Uygulama Hizmeti Ortamında bulunan Yalıtılmış plan uygulamaları tarafından kullanılamaz.
* Özellik, bir Azure Kaynak Yöneticisi VNet'te 32 adresli veya daha büyük bir /27 olan kullanılmayan bir alt ağ gerektirir.
* Uygulama ve VNet aynı bölgede olmalıdır.
* Entegre bir uygulamayla bir VNet'i silemezsiniz. VNet'i silmeden önce tümleştirmeyi kaldırın.
* VNets ile yalnızca uygulamayla aynı abonelikte entegre olabilirsiniz.
* Uygulama Hizmeti planı başına yalnızca bir bölgesel VNet Tümleştirme'niz olabilir. Aynı Uygulama Hizmeti planındaki birden çok uygulama aynı VNet'i kullanabilir.
* Bölgesel VNet Tümleştirmesi kullanan bir uygulama varken bir uygulamanın veya planın aboneliğini değiştiremezsiniz.
* Uygulamanız Azure DNS Özel Bölgeleri'ndeki adresleri yapılandırma değişiklikleri olmadan çözemez

Her plan örneği için bir adres kullanılır. Uygulamanızı beş örnekle ölçeklendirseniz, beş adres kullanılır. Atamadan sonra alt ağ boyutu değiştirilemediğinden, uygulamanızın ulaşabileceği ölçeklere uyacak kadar büyük bir alt ağ kullanmanız gerekir. 64 adresli bir /26 önerilen boyutdur. 64 adresli bir /26, 30 örnekli bir Premium planı barındırır. Bir planı yukarı veya aşağı ölçeklendirdiğinizde, kısa bir süre için iki kat daha fazla adrese ihtiyacınız vardır.

Uygulamalarınızın başka bir plandaki uygulamalara zaten bağlı olan bir VNet'e ulaşmasını istiyorsanız, önceden varolan VNet Tümleştirmesi tarafından kullanılandan farklı bir alt ağ seçin.

Özellik Linux için önizleme. Bu özelliğin Linux formu yalnızca RFC 1918 adreslerine (10.0.0.0/8, 172.16.0.0/12, 192.168.0.0/16) arama yapmayı destekler.

### <a name="web-or-function-app-for-containers"></a>Kapsayıcılar için Web veya İşlev Uygulaması

Uygulamanızı Linux'ta yerleşik görüntülerle barındırArsanız, bölgesel VNet Tümleştirmesi ek değişiklikler yapmadan çalışır. Kapsayıcılar için Web veya İşlev Uygulaması kullanıyorsanız, Docker resminizi VNet Tümleştirmesi'ni kullanmak üzere değiştirmeniz gerekir. Docker görüntünüzde, sabit kodlu bir bağlantı noktası numarası kullanmak yerine ANA web sunucusunun dinleme bağlantı noktası olarak PORT ortamı değişkenini kullanın. PORT ortamı değişkeni, kapsayıcı başlatma zamanında platform tarafından otomatik olarak ayarlanır. SSH kullanıyorsanız, Bölgesel VNet Tümleştirmesi'ni kullandığınızda SSH daemon' un, SSH_PORT çevre değişkeni tarafından belirtilen bağlantı noktası numarasını dinleyecek şekilde yapılandırılmalıdır. Linux'ta ağ geçidi için gerekli VNet Entegrasyonu için destek yoktur.

### <a name="service-endpoints"></a>Hizmet uç noktaları

Bölgesel VNet Tümleştirmesi, hizmet bitiş noktalarını kullanmanızı sağlar. Uygulamanızla hizmet bitiş noktalarını kullanmak için, seçili bir VNet'e bağlanmak için bölgesel VNet Tümleştirmesini kullanın ve ardından tümleştirme için kullandığınız alt ağdaki hedef servisle hizmet bitiş noktalarını yapılandırın. Daha sonra hizmet bitiş noktaları üzerinden bir hizmete erişmek istiyorsanız:

1. web uygulamanızla bölgesel VNet Entegrasyonunu yapılandırma
1. hedef servise gidin ve tümleştirme için kullanılan alt ağa karşı hizmet bitiş noktalarını yapılandırın

### <a name="network-security-groups"></a>Ağ güvenlik grupları

VNet'teki kaynaklara gelen ve giden trafiği engellemek için ağ güvenlik gruplarını kullanabilirsiniz. Bölgesel VNet Tümleştirmesi kullanan bir uygulama, VNet'inizdeki veya internetteki kaynaklara giden trafiği engellemek için bir [ağ güvenlik grubu][VNETnsg] kullanabilir. Ortak adreslere gelen trafiği engellemek için uygulama ayarını WEBSITE_VNET_ROUTE_ALL 1 olarak ayarlamanız gerekir. VNet Tümleştirmesi uygulamanızdan yalnızca giden trafiği etkilediği için NSG'deki gelen kurallar uygulamanız için geçerli değildir.

Uygulamanıza gelen trafiği denetlemek için Erişim Kısıtlamaları özelliğini kullanın. Tümleştirme alt ağınıza uygulanan bir NSG, tümleştirme alt ağınıza uygulanan rotalardan bağımsız olarak etkindir. WEBSITE_VNET_ROUTE_ALL 1 olarak ayarlanmışsa ve tümleştirme alt ağınızdaki genel adres trafiğini etkileyen herhangi bir rotanız yoksa, giden trafiğinizin tümü yine de tümleştirme alt ağınıza atanan NSG'lere tabidir. WEBSITE_VNET_ROUTE_ALL ayarlı değilse, NSG'ler yalnızca RFC1918 trafiğine uygulanır.

### <a name="routes"></a>Yollar

Giden trafiği uygulamanızdan istediğiniz yere yönlendirmek için rota tablolarını kullanabilirsiniz. Varsayılan olarak, rota tabloları yalnızca RFC1918 hedef trafiğinizi etkiler. WEBSITE_VNET_ROUTE_ALL 1 olarak ayarlarsanız, tüm giden çağrılarınız etkilenir. Tümleştirme alt ağınızda ayarlanan rotalar gelen uygulama isteklerine verilen yanıtları etkilemez. Sık kullanılan hedefler arasında güvenlik duvarı aygıtları veya ağ geçitleri yer alabilir.

Tüm giden trafiği şirket içinde yönlendirmek istiyorsanız, tüm giden trafiği ExpressRoute ağ geçidinize göndermek için bir rota tablosu kullanabilirsiniz. Trafiği bir ağ geçidine yönlendirin, yanıtları geri göndermek için dış ağdaki yolları ayarladığınızdan emin olun.

Kenarlık Ağ Geçidi Protokolü (BGP) yolları da uygulama trafiğinizi etkiler. ExpressRoute ağ geçidi gibi bir şeyden BGP rotalarınız varsa, uygulamagiden trafiğiniz etkilenir. Varsayılan olarak, BGP rotaları yalnızca RFC1918 hedef trafiğinizi etkiler. WEBSITE_VNET_ROUTE_ALL 1 olarak ayarlanmışsa, tüm giden trafik BGP rotalarınızdan etkilenebilir.

### <a name="azure-dns-private-zones"></a>Azure DNS Özel Bölgeler 

Uygulamanız VNet'inizle entegre olduktan sonra, VNet'inizin yapılandırıldığının aynısını kullanır. Varsayılan olarak, uygulamanız Azure DNS Özel Bölgeleri ile çalışmaz. Azure DNS Özel Bölgeleri ile çalışmak için aşağıdaki uygulama ayarlarını eklemeniz gerekir:

1. değeri 168.63.129.16 olan WEBSITE_DNS_SERVER 
1. değeri 1 olan WEBSITE_VNET_ROUTE_ALL

Bu ayarlar, uygulamanızın Azure DNS özel bölgelerini kullanmasını sağlamanın yanı sıra, uygulamanızdan gelen tüm çağrılarınızı VNet'inize gönderir.

### <a name="private-endpoints"></a>Özel uç noktalar

[Özel Bitiş Noktaları'na][privateendpoints]arama yapmak istiyorsanız, Azure DNS Özel Bölgeleri ile tümleştirmeniz veya uygulamanız tarafından kullanılan DNS sunucusundaki özel bitiş noktasını yönetmeniz gerekir. 

<!--Image references-->
[4]: ../includes/media/web-sites-integrate-with-vnet/vnetint-appsetting.png

<!--Links-->
[VNETnsg]: https://docs.microsoft.com/azure/virtual-network/security-overview/
[privateendpoints]: https://docs.microsoft.com/azure/app-service/networking/private-endpoint
