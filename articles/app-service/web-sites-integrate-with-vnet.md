---
title: Uygulamayı Azure sanal ağı ile tümleştirme
description: Uygulamaları Azure sanal ağlarla Azure App Service tümleştirme.
author: ccompy
ms.assetid: 90bc6ec6-133d-4d87-a867-fcf77da75f5a
ms.topic: article
ms.date: 02/27/2020
ms.author: ccompy
ms.custom: seodec18
ms.openlocfilehash: 76139716fe11536faa0ff792185ba1643801c641
ms.sourcegitcommit: 96dc60c7eb4f210cacc78de88c9527f302f141a9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2020
ms.locfileid: "77649073"
---
# <a name="integrate-your-app-with-an-azure-virtual-network"></a>Uygulamanızı bir Azure sanal ağıyla tümleştirin
Bu belgede Azure App Service sanal ağ tümleştirme özelliği ve [Azure App Service](https://go.microsoft.com/fwlink/?LinkId=529714)uygulamalarla nasıl ayarlanacağı açıklanmaktadır. [Azure sanal ağları][VNETOverview] (VNet), internet 'ten yönlendirilebilir olmayan bir ağa Azure kaynaklarınızın çoğunu yerleştirme izni verir.  

Azure App Service iki çeşitliliğe sahiptir. 

1. Yalıtılmış haricinde fiyatlandırma planlarının tam aralığını destekleyen çok kiracılı sistemler
2. Sanal ağınıza dağıtan ve yalıtılmış fiyatlandırma planı uygulamalarını destekleyen App Service Ortamı (ASE)

Bu belge, çok kiracılı App Service kullanımı için sanal ağ tümleştirme özelliğinden geçer. Uygulamanız [App Service ortamı][ASEintro]yer alıyorsa, zaten VNET 'te bulunur ve aynı VNET 'teki kaynaklara ulaşmak Için VNET tümleştirme özelliğinin kullanılmasını gerektirmez. Tüm App Service ağ özellikleriyle ilgili ayrıntılar için, [App Service ağ özelliklerini](networking-features.md) okuyun

VNet tümleştirmesi, Web uygulamanızın sanal ağınızdaki kaynaklara erişmesini sağlar, ancak Web uygulamanıza VNet 'ten gelen özel erişim izni vermez. Özel site erişimi, uygulamanızı yalnızca bir Azure sanal ağı içindeki gibi özel bir ağdan erişilebilir hale getirme anlamına gelir. VNet tümleştirmesi yalnızca uygulamanızdan VNet 'e giden çağrılar yapmak içindir. VNet tümleştirme özelliği, aynı bölgedeki sanal ağlar ve diğer bölgelerde VNET 'ler ile kullanıldığında farklı davranır. VNet tümleştirme özelliğinin iki çeşidi vardır.

1. Bölgesel VNet tümleştirmesi-aynı bölgedeki Kaynak Yöneticisi sanal ağlara bağlanırken, Tümleştirdiğiniz VNet 'te ayrılmış bir alt ağa sahip olmanız gerekir. 
2. Ağ Geçidi gerekli VNet tümleştirmesi-diğer bölgelerde sanal ağlara veya aynı bölgedeki bir klasik VNet 'e bağlanılırken hedef VNet 'te sağlanmış bir sanal ağ geçidi gerekir.

VNet tümleştirme özellikleri:

* Standart, Premium, PremiumV2 veya elastik Premium fiyatlandırma planı gerektir 
* TCP ve UDP desteği
* App Service uygulamalarla ve Işlev uygulamalarıyla çalışma

VNet tümleştirmesinin şunları dahil desteklemediği bazı şeyler vardır:

* sürücü bağlama
* AD tümleştirmesi 
* NetBios

Ağ Geçidi gerekli VNet tümleştirmesi yalnızca hedef VNet 'te veya eşleme ya da VPN 'Ler ile hedef VNet 'e bağlı ağlarda bulunan kaynaklara erişim sağlar. Ağ Geçidi gerekli VNet tümleştirmesi, ExpressRoute bağlantılarında kullanılabilir olan kaynaklara erişimi etkinleştirmez veya hizmet uç noktaları ile birlikte kullanılabilir. 

VNet tümleştirmesi, kullanılan sürümden bağımsız olarak, Web uygulamanıza sanal ağınızdaki kaynaklara erişmenizi sağlar, ancak sanal ağdan Web uygulamanıza gelen özel erişim izni vermez. Özel site erişimi, uygulamanızı yalnızca bir Azure sanal ağı içindeki gibi özel bir ağdan erişilebilir hale getirme anlamına gelir. VNet tümleştirmesi yalnızca uygulamanızdan VNet 'e giden çağrılar yapmak içindir. 

## <a name="enable-vnet-integration"></a>VNet tümleştirmesini etkinleştir 

1. App Service portalında ağ kullanıcı arabirimine gidin. VNet tümleştirmesi altında *"yapılandırmak için buraya tıklayın"* seçeneğini belirleyin. 

1. **VNET Ekle**' yi seçin.  

   ![VNet tümleştirmesini seçin][1]

1. Açılan listede, aynı bölgedeki ve diğer tüm bölgelerdeki tüm Kaynak Yöneticisi sanal ağların bir listesi olan, aboneliğinizdeki tüm Kaynak Yöneticisi sanal ağları yer alacak. Tümleştirilecek sanal ağı seçin.

   ![VNet 'i seçin][2]

   * VNet aynı bölgedeyse, yeni bir alt ağ oluşturun veya önceden varolan boş bir alt ağ seçin. 

   * Başka bir bölgedeki VNet seçmek için, sitenin etkin olduğu noktadan bir sanal ağ geçidinizin gerekir.

   * Klasik VNET ile tümleştirme için, VNet açılan listesine tıklanmak yerine, **Klasik VNET 'e bağlanmak için buraya tıklayın ' ı**seçin. İstediğiniz klasik sanal ağı seçin. Hedef VNet 'in, site etkin noktadan sonra sağlanan bir sanal ağ geçidi zaten olmalıdır.

    ![Klasik VNet seçin][3]
    
Tümleştirme sırasında uygulamanız yeniden başlatılır.  Tümleştirme tamamlandığında, ile tümleştirildiği VNet 'teki ayrıntıları görürsünüz. 

Uygulamanız VNet 'iniz ile tümleşik olduktan sonra, Azure DNS Özel Bölgeleri olmadığı müddetçe sanal ağınızın yapılandırıldığı aynı DNS sunucusunu kullanır. Şu anda Azure DNS Özel Bölgeleri ile VNet tümleştirmesini kullanamazsınız.

## <a name="regional-vnet-integration"></a>bölgesel VNet tümleştirmesi

Bölgesel VNet tümleştirmesini kullanmak, uygulamanızın erişmesini sağlar:

* aynı bölgedeki VNet 'teki kaynaklar 
* VNet 'lerdeki kaynaklar aynı bölgedeki VNet 'iniz ile eşlenmez
* hizmet uç noktası güvenliği sağlanmış hizmetler
* ExpressRoute bağlantıları genelindeki kaynaklar
* bağlı olduğunuz VNet 'teki kaynaklar
* ExpressRoute bağlantıları dahil eşlenen bağlantı genelindeki kaynaklar
* Özel uç noktalar 

Sanal ağlar ile VNet tümleştirmesi aynı bölgede kullanılırken, aşağıdaki Azure ağ özelliklerini kullanabilirsiniz:

* Ağ güvenlik grupları (NSG 'ler)-tümleştirme alt ağınıza yerleştirilmiş bir ağ güvenlik grubuyla giden trafiği engelleyebilirsiniz. Web uygulamanıza gelen erişimi sağlamak için VNet tümleştirmesini kullanmadığından, gelen kuralları uygulanmaz.
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
* Web uygulamasıyla aynı abonelikte yalnızca VNET 'ler ile tümleştirilebilir
* App Service planı başına yalnızca bir bölgesel VNet tümleştirmesi olabilir. Aynı App Service planındaki birden çok uygulama aynı VNet 'i kullanabilir. 
* Bölgesel VNet tümleştirmesi kullanan bir uygulama varken, bir uygulamanın veya App Service planının aboneliğini değiştiremezsiniz

Her bir App Service plan örneği için bir adres kullanılır. Uygulamanızı beş örneğe ölçeklendirirseniz, beş adres kullanılır. Alt ağ boyutu atamadan sonra değiştirilemediğinden, uygulamanızın ulaşabileceği ölçeğe uyum sağlayacak kadar büyük bir alt ağ kullanmanız gerekir. 64 adresi olan bir/26 önerilen boyutdir. 64 adresi olan bir/26, 30 örneğe sahip Premium App Service planına sahip olur. Bir App Service planını yukarı veya aşağı ölçeklendirirseniz, kısa bir süre için birçok adrese iki kez ihtiyacınız vardır. 

Daha önce başka bir App Service planında bulunan uygulamalar tarafından zaten bağlı olan bir VNet 'e erişmek için başka bir App Service planındaki uygulamalarınızın, önceden var olan VNet tümleştirmesi tarafından kullanılandan farklı bir alt ağ seçmeniz gerekir.  

Özelliği, Linux için önizlemededir. Özelliğin Linux formu yalnızca RFC 1918 adreslerine çağrı yapmayı destekler (10.0.0.0/8, 172.16.0.0/12, 192.168.0.0/16).

### <a name="web-app-for-containers"></a>Kapsayıcılar için Web App

Yerleşik görüntülerle Linux üzerinde App Service kullanıyorsanız, bölgesel VNet tümleştirmesi ek değişiklik yapılmadan işe yarar. Kapsayıcılar için Web App kullanıyorsanız, VNet tümleştirmesini kullanabilmeniz için Docker görüntünüzü değiştirmeniz gerekir. Docker yansımanıza, sabit kodlanmış bir bağlantı noktası numarası kullanmak yerine ana Web sunucusunun dinleme bağlantı noktası olarak bağlantı noktası ortam değişkenini kullanın. Bağlantı noktası ortam değişkeni, kapsayıcı başlangıç saatinde App Service platformu tarafından otomatik olarak ayarlanır. SSH kullanıyorsanız, SSH Daemon 'ın bölgesel VNet tümleştirmesi kullanılırken SSH_PORT ortam değişkeni tarafından belirtilen bağlantı noktası numarasını dinlemek üzere yapılandırılması gerekir.  Linux üzerinde ağ geçidi gerekli VNet tümleştirmesi desteği yoktur. 

### <a name="service-endpoints"></a>Hizmet uç noktaları

Bölgesel VNet tümleştirmesi, hizmet uç noktalarını kullanmanıza olanak sağlar.  Hizmet uç noktalarını uygulamanızla birlikte kullanmak için, seçili bir sanal ağa bağlanmak üzere bölgesel VNet tümleştirmesini kullanın ve ardından tümleştirme için kullandığınız alt ağdaki hizmet uç noktalarını yapılandırın. 

### <a name="network-security-groups"></a>Ağ Güvenlik Grupları

Ağ güvenlik grupları, bir sanal ağdaki kaynaklara gelen ve giden trafiği engellemenize olanak tanır. Bölgesel VNet tümleştirmesi kullanan bir Web uygulaması, VNet veya Internet 'teki kaynaklara giden trafiği engellemek için [ağ güvenlik grubunu][VNETnsg] kullanabilir. Ortak adreslere giden trafiği engellemek için WEBSITE_VNET_ROUTE_ALL uygulama ayarının 1 olarak ayarlanmış olması gerekir. VNet tümleştirmesi yalnızca uygulamanızdan giden trafiği etkilediği için bir NSG 'deki gelen kuralları uygulamanıza uygulanmaz. Web uygulamanıza gelen trafiği denetlemek için erişim kısıtlamaları özelliğini kullanın. Tümleştirme alt ağına uygulanan bir NSG, tümleştirme alt ağına uygulanan rotalara bakılmaksızın etkili olacaktır. WEBSITE_VNET_ROUTE_ALL 1 olarak ayarlanmışsa ve tümleştirme alt ağınızda ortak adres trafiğini etkileyen bir yolunuz yoksa, tüm giden trafiğiniz tümleştirme alt ağına atanan NSG 'lere tabidir. WEBSITE_VNET_ROUTE_ALL ayarlanmamışsa, NSG 'ler yalnızca RFC1918 trafiğe uygulanır.

### <a name="routes"></a>Yollar

Yol tabloları, uygulamanızdaki giden trafiği istediğiniz yere yönlendirmenize olanak tanır. Varsayılan olarak, yol tabloları yalnızca RFC1918 hedef trafiğinizi etkiler.  WEBSITE_VNET_ROUTE_ALL 1 olarak ayarlarsanız tüm giden çağrılarınız etkilenecektir. Tümleştirme alt ağınızda ayarlanan yollar, gelen uygulama isteklerine yapılan yanıtları etkilemez. Ortak hedefler, güvenlik duvarı cihazları veya ağ geçitleri içerebilir. Şirket içinde tüm giden trafiği yönlendirmek isterseniz, tüm giden trafiği ExpressRoute ağ geçidize göndermek için bir yol tablosu kullanabilirsiniz. Trafiği bir ağ geçidine yönlendirdiğinizde, dış ağdaki yolları, yanıtları geri gönderecek şekilde ayarladığınızdan emin olun.

Sınır Ağ Geçidi Protokolü (BGP) yolları da uygulama trafiğinizi etkileyecektir. ExpressRoute ağ geçidine benzer bir şekilde BGP yollarınız varsa, uygulamanızın giden trafiği etkilenecektir. Varsayılan olarak BGP yolları yalnızca RFC1918 hedef trafiğinizi etkiler. WEBSITE_VNET_ROUTE_ALL 1 olarak ayarlanırsa, tüm giden trafik BGP rotalarınız tarafından etkilenebilir. 

### <a name="how-regional-vnet-integration-works"></a>Bölgesel VNet tümleştirmesinin çalışması

App Service uygulamalar çalışan rollerinde barındırılır. Temel ve daha yüksek fiyatlandırma planları, aynı çalışanlar üzerinde çalışan başka müşteri iş yükleri bulunmayan ayrılmış barındırma planlardır. Bölgesel VNet tümleştirmesi, yetkilendirilmiş alt ağdaki adreslerle sanal arabirimler bağlayarak işe yarar. Kimden adresi sanal ağınız içinde olduğundan, VNET 'iniz içindeki bir VM 'de olduğu gibi, sanal ağınız içindeki veya bu sanal nesnelerin çoğuna erişebilir. Ağ uygulamasının VNet 'iniz üzerinde bir VM çalıştırmasının dışında ve bu özellik kullanılırken bazı ağ özellikleri henüz kullanılamaz.

![Bölgesel VNet tümleştirmesinin çalışması][5]

Bölgesel VNet tümleştirmesi etkinleştirildiğinde, uygulamanız yine de normal olarak aynı kanallar üzerinden İnternet 'e giden çağrılar yapmaya devam edecektir. Uygulama özellikleri portalında listelenen giden adresler hala uygulamanız tarafından kullanılan adreslerdir. Uygulamanız için değişiklikler, hizmet uç noktası güvenliği sağlanmış hizmetler veya RFC 1918 adresleri için de sanal ağınıza gider. WEBSITE_VNET_ROUTE_ALL 1 olarak ayarlanırsa, tüm giden trafik sanal ağınıza gönderilebilir. 

Özelliği, çalışan başına yalnızca bir sanal arabirimi destekler.  Çalışan başına bir sanal arabirim, App Service plan başına bir bölgesel VNet tümleştirmesi anlamına gelir. Aynı App Service planındaki tüm uygulamalar aynı VNet tümleştirmesini kullanabilir, ancak ek bir sanal ağa bağlanmak için bir uygulamaya ihtiyacınız varsa, başka bir App Service planı oluşturmanız gerekir. Kullanılan sanal arabirim, müşterilerin doğrudan erişimine sahip olduğu bir kaynak değildir.

Bu teknolojinin nasıl çalıştığı doğası nedeniyle, VNet tümleştirmeyle kullanılan trafik ağ Izleyicisi veya NSG akış günlüklerinde gösterilmez.  

## <a name="gateway-required-vnet-integration"></a>Ağ Geçidi gerekli VNet tümleştirmesi

Ağ Geçidi gerekli VNet tümleştirmesi, başka bir bölgedeki VNet 'e veya klasik VNet 'e bağlanmayı destekler. Ağ Geçidi gerekli VNet tümleştirmesi: 

* Bir uygulamanın tek seferde yalnızca 1 VNet 'e bağlanmasını sağlar
* App Service bir plan dahilinde tümleştirilecek en fazla beş VNET sağlar 
* Bir App Service planı tarafından kullanılabilecek toplam sayıyı etkilemeden bir App Service planında birden çok uygulama tarafından kullanılmasını sağlar.  Aynı App Service planında aynı VNet 'i kullanan altı uygulamanız varsa, bu, kullanılmakta olan 1 VNet olarak sayılır. 
* Ağ geçidinde SLA nedeniyle% 99,9 SLA 'sını destekler
* Uygulamalarınızın VNet 'in yapılandırıldığı DNS 'yi kullanmasına olanak sağlar
* , Uygulamaya bağlanmadan önce SSTP Noktadan siteye VPN ile yapılandırılmış bir sanal ağ yolu tabanlı ağ geçidi gerektirir. 

Ağ Geçidi gerekli VNet tümleştirmesini kullanamazsınız:

* Linux uygulamaları ile
* ExpressRoute ile bağlantılı VNet ile 
* Hizmet uç noktalarına güvenli kaynaklara erişmek için
* Hem ExpressRoute hem de site/siteden siteye VPN 'lere işaret eden bir birlikte bulunma ağ geçidiyle

### <a name="set-up-a-gateway-in-your-vnet"></a>Sanal ağınız için bir ağ geçidi ayarlama ###

Bir ağ geçidi oluşturmak için:

1. Sanal ağınız için [bir ağ geçidi alt ağı oluşturun][creategatewaysubnet] .  

1. [VPN ağ geçidini oluşturun][creategateway]. Rota tabanlı VPN türü seçin.

1. [Noktayı site adresleri olarak ayarlayın][setp2saddresses]. Ağ Geçidi temel SKU 'da değilse, ıKEV2 'nin site yapılandırması noktasında devre dışı bırakılması ve SSTP 'nin seçilmesi gerekir. Noktadan siteye adres alanı, RFC 1918 adres blokları, 10.0.0.0/8, 172.16.0.0/12, 192.168.0.0/16 biçiminde olmalıdır

Yalnızca App Service VNet tümleştirmesiyle kullanmak üzere ağ geçidini oluşturuyorsanız, bir sertifikayı karşıya yüklemeniz gerekmez. Ağ geçidinin oluşturulması 30 dakika sürebilir. Ağ Geçidi sağlanıncaya kadar uygulamanızı VNet ile tümleştirimeyeceksiniz. 

### <a name="how-gateway-required-vnet-integration-works"></a>Ağ Geçidi gerekli VNet tümleştirmesinin çalışması

Ağ Geçidi, Noktadan siteye VPN teknolojisine yerleşik olarak oluşturulan VNet tümleştirmesini gerektirir. Noktadan siteye VPN 'Ler, yalnızca uygulamayı barındıran sanal makineye ağ erişimini sınırlandırır. Uygulamalar, Karma Bağlantılar veya VNet tümleştirmesi aracılığıyla yalnızca İnternet 'e trafik gönderecek şekilde kısıtlanır. Uygulamanız, ağ geçidi gerekli VNet tümleştirmesi kullanmak üzere portalla yapılandırıldığında, ağ geçidinde ve uygulama tarafında sertifika oluşturmak ve atamak için sizin adınıza karmaşık bir anlaşma yönetilir. Nihai sonuç, uygulamalarınızı barındırmak için kullanılan çalışanların seçili VNet 'teki sanal ağ geçidine doğrudan bağlanmasını sağlar. 

![Ağ Geçidi gerekli VNet tümleştirmesinin çalışması][6]

### <a name="accessing-on-premises-resources"></a>Şirket içi kaynaklara erişme

Uygulamalar, siteden siteye bağlantıları olan VNET 'ler ile tümleştirerek şirket içi kaynaklara erişebilir. Ağ Geçidi gerekli VNet tümleştirmesini kullanıyorsanız, şirket içi VPN ağ geçidi yollarınızı Noktadan siteye adres bloklarında güncelleştirmeniz gerekir. Siteden siteye VPN ilk kez ayarlandığında, yapılandırmak için kullanılan betikler yolları doğru şekilde ayarlayabilmelidir. Siteden siteye VPN 'nizi oluşturduktan sonra Noktadan siteye adresleri eklerseniz, rotaları el ile güncelleştirmeniz gerekir. Bu şekilde, ağ geçidi başına farklılık gösteren ve burada açıklanmayan Ayrıntılar açıklanmaktadır. Siteden siteye VPN bağlantısıyla BGP ile yapılandırılmış olamaz.

Bölgesel VNet tümleştirme özelliğinin sanal ağınıza ve şirket içi ağa ulaşması için ek bir yapılandırma gerekmez. Sanal ortamınızı ExpressRoute veya siteden siteye VPN kullanarak şirket içi olarak bağlamanız yeterlidir. 

> [!NOTE]
> Ağ Geçidi gerekli VNet tümleştirme özelliği, bir uygulamayı ExpressRoute ağ geçidi olan bir VNet ile tümleştirmez. ExpressRoute ağ geçidi, birlikte [bulunma modunda][VPNERCoex] yapılandırılmış olsa bile VNET tümleştirmesi çalışmaz. Kaynaklara bir ExpressRoute bağlantısı aracılığıyla erişmeniz gerekiyorsa, bölgesel VNet tümleştirme özelliğini veya sanal ağınız üzerinde çalışan bir [App Service ortamı][ASE]kullanabilirsiniz. 
> 
> 

### <a name="peering"></a>Eşleme

Bölgesel VNet tümleştirmesiyle eşleme kullanıyorsanız, ek yapılandırma yapmanız gerekmez. 

Ağ Geçidi gereken VNet tümleştirmesini eşleme ile kullanıyorsanız, birkaç ek öğe yapılandırmanız gerekir. Eşleme 'yi uygulamanızla çalışacak şekilde yapılandırmak için:

1. Uygulamanızın bağlandığı VNet 'e bir eşleme bağlantısı ekleyin. Eşleme bağlantısı eklenirken **sanal ağ erişimine Izin ver** ' i etkinleştirin ve **iletilen trafiğe** izin ver ' i işaretleyin ve **ağ geçidi aktarımına izin verin**.
1. Bağlı olduğunuz VNet 'e eşlenmekte olan VNet üzerinde bir eşleme bağlantısı ekleyin. Hedef VNet 'e eşleme bağlantısı eklenirken **sanal ağ erişimine Izin ver** ' i etkinleştirin ve **iletilen trafiğe** izin ver ' i işaretleyin ve **uzak ağ geçitlerine izin**verin.
1. Portalda App Service plan > Ağ > VNet tümleştirme Kullanıcı arabirimine gidin.  Uygulamanızın bağlandığı sanal ağı seçin. Yönlendirme bölümünde, uygulamanızın bağlı olduğu VNet ile ilişkilendirilen VNet 'in adres aralığını ekleyin.  

## <a name="managing-vnet-integration"></a>VNet tümleştirmesini yönetme 

VNet ile bağlanma ve bağlantı kesilmesi uygulama düzeyindedir. Birden çok uygulama genelinde VNet tümleştirmesini etkileyebilecek işlemler App Service plan düzeyidir. App > Ağ > VNet tümleştirme Portalı ' ndan VNet 'iniz hakkında ayrıntılı bilgi edinebilirsiniz. ASP > Ağ > VNet tümleştirme portalındaki ASP düzeyinde benzer bilgiler görebilirsiniz.

VNet tümleştirmenize ilişkin uygulama görünümünde gerçekleştirebileceğiniz tek işlem, uygulamanızın bağlı olduğu VNet 'ten bağlantısını kesmesidir. Uygulamanızın VNet bağlantısını kesmek için **bağlantıyı kes**' i seçin. VNet bağlantısı kesildiğinde uygulamanız yeniden başlatılacak. Bağlantı kesilmesi VNet 'i değiştirmez. Alt ağ veya ağ geçidi kaldırılmaz. Daha sonra sanal ortamınızı silmek istiyorsanız, öncelikle uygulamanızın VNet bağlantısını kesmeniz ve ağ geçitleri gibi kaynakları silmeniz gerekir. 

ASP VNet tümleştirmesi Kullanıcı arabirimi, ASP 'inizdeki uygulamalar tarafından kullanılan tüm VNet tümleştirmelerini gösterir. Her VNet hakkındaki ayrıntıları görmek için ilgilendiğiniz VNet 'e tıklayın. Ağ Geçidi gerekli VNet tümleştirmesi için burada gerçekleştirebileceğiniz iki eylem vardır.

* **Ağ eşitleme**. Ağ eşitleme işlemi yalnızca ağ geçidine bağımlı VNet tümleştirme özelliği için geçerlidir. Bir eşitleme ağı işleminin gerçekleştirilmesi, sertifikalarınızın ve ağ bilgilerinizin eşitlenmiş olmasını sağlar. VNet 'nizin DNS 'sini ekler veya değiştirirseniz, bir **eşitleme ağı** işlemi gerçekleştirmeniz gerekir. Bu işlem, bu VNet 'i kullanan tüm uygulamaları yeniden başlatacak.
* **Rota Ekle** Yolların eklenmesi, giden trafiği sanal ağınıza yönlendirir. 

**Ağ Geçidi gerekli VNET tümleştirme yönlendirmesi** VNet 'iniz üzerinde tanımlanan yollar uygulamanızdan sanal ağınıza giden trafiği yönlendirmek için kullanılır. VNet 'e ek giden trafik göndermeniz gerekiyorsa, bu adres bloklarını buraya ekleyebilirsiniz. Bu özellik yalnızca ağ geçidi gerekli VNet tümleştirmesi ile kullanılabilir. Ağ Geçidi gerekli VNet tümleştirmesinin bölgesel VNet tümleştirmeyle yaptıkları şekilde kullanılması durumunda yol tabloları uygulama trafiğinizi etkilemez.

**Ağ Geçidi gerekli VNET tümleştirme sertifikaları** Ağ Geçidi gereken VNet tümleştirmesi etkinleştirildiğinde, bağlantının güvenliğini sağlamak için gerekli bir sertifika alışverişi vardır. Sertifikaların yanı sıra, DNS yapılandırması, rotalar ve ağı tanımlayan diğer benzer şeylerdir.
Sertifikalar veya ağ bilgileri değiştirilirse "ağı Eşitle" seçeneğine tıklamanız gerekir. "Ağ Eşitle" seçeneğine tıkladığınızda, uygulamanız ve VNet 'iniz arasındaki bağlantıda kısa bir kesinti oluşmasına neden olursunuz. Uygulamanız yeniden başlatılana karşın bağlantı kaybı sitenizin düzgün şekilde çalışmamasına neden olabilir. 

## <a name="pricing-details"></a>Fiyatlandırma ayrıntıları
Bölgesel VNet tümleştirme özelliğinin, ASP fiyatlandırma katmanı ücretleri dışında kullanılmak üzere ek ücret yoktur.

Ağ Geçidi gerekli VNet tümleştirme özelliğinin kullanımına yönelik üç ilgili ücret vardır:

* ASP fiyatlandırma katmanı ücretleri-uygulamalarınızın standart, Premium veya PremiumV2 App Service bir planda olması gerekir. Bu maliyetler hakkında daha fazla ayrıntı için şu adımları görebilirsiniz: [App Service fiyatlandırması][ASPricing]. 
* Veri aktarımı maliyetleri-VNet aynı veri merkezinde olsa bile veri çıkışı için ücret uygulanır. Bu ücretler [veri aktarımı fiyatlandırma ayrıntılarında][DataPricing]açıklanmaktadır. 
* VPN Gateway maliyetler-Noktadan siteye VPN için gereken VNet ağ geçidinin maliyeti vardır. Ayrıntılar [VPN Gateway fiyatlandırma][VNETPricing] sayfasıdır.

## <a name="troubleshooting"></a>Sorun giderme
Özelliği kolayca ayarlanabilir, bu da deneyiminizin ücretsiz olacağı anlamına gelmez. İstenen uç noktanıza erişmede sorun yaşamanız gerekir, bu, uygulama konsolundan bağlantıyı test etmek için kullanabileceğiniz bazı yardımcı programlar vardır. Kullanabileceğiniz iki konsol vardır. Birisi kudu konsoludur ve diğeri Azure portal konsoludur. Uygulamanızın kudu konsoluna ulaşmak için Araçlar-> kudu ' ye gidin. Ayrıca, Kudo konsoluna [SiteName]. scm. azurewebsites. net adresinden ulaşabilirsiniz. Web sitesi yüklendiğinde, hata ayıklama konsolu sekmesine gidin. Daha sonra Azure portal barındırılan konsola ulaşmak için Araçlar-> konsoluna gidin. 

#### <a name="tools"></a>Araçlar
**Ping**, **nslookup**ve **tracert** araçları, güvenlik kısıtlamaları nedeniyle konsolda çalışmaz. Void 'yi dolduracak şekilde iki ayrı araç eklenmiştir. DNS işlevselliğini test etmek için, nameresolver. exe adlı bir araç ekledik. Söz dizimi aşağıdaki gibidir:

    nameresolver.exe hostname [optional: DNS Server]

Uygulamanızın bağımlı olduğu ana bilgisayar adlarını denetlemek için **nameresolver** kullanabilirsiniz. Bu şekilde, DNS 'niz ile yanlış yapılandırılmış herhangi bir şey varsa veya belki DNS sunucunuza erişiminiz yoksa test edebilirsiniz. Uygulamanızın konsolda kullanacağı DNS sunucusunu, WEBSITE_DNS_SERVER ve WEBSITE_DNS_ALT_SERVER ortam değişkenlerine bakarak görebilirsiniz.

Sonraki araç, bir konak ve bağlantı noktası birleşimine TCP bağlantısı için test etmenizi sağlar. Bu araca, **tcma** adı verilir ve sözdizimi şöyledir:

    tcpping.exe hostname [optional: port]

**Tcter** yardımcı programı, belirli bir konağa ve bağlantı noktasına ulaşabilseniz size bildirir. Yalnızca şu durumlarda başarı gösterebilir: konak ve bağlantı noktası birleşimine dinleme yapan bir uygulama varsa ve uygulamanızdan belirtilen konağa ve bağlantı noktasına ağ erişimi varsa.

#### <a name="debugging-access-to-vnet-hosted-resources"></a>VNet barındırılan kaynaklara erişim hatası ayıklama
Uygulamanızın belirli bir konağa ve bağlantı noktasına ulaşmasını engelleyebilen birçok şey vardır. Çoğu zaman üç işlemlerden biridir:

* **Bir güvenlik duvarı bu şekilde yapılır.** Bu şekilde bir güvenlik duvarınız varsa, TCP zaman aşımına uğrayacaktır. Bu durumda TCP zaman aşımı 21 saniyedir. Bağlantıyı sınamak için **tcma** aracını kullanın. TCP zaman aşımları, güvenlik duvarlarının ötesinde çok sayıda şey olabilir ancak buradan başlayabilir. 
* **DNS erişilebilir değil.** DNS sunucusu başına DNS zaman aşımı üç saniyedir. İki DNS sunucunuz varsa, zaman aşımı 6 saniyedir. DNS çalışıp çalışmadığını görmek için nameresolver kullanın. Sanal ağınızın yapılandırıldığı DNS 'i kullanmayan için nslookup 'ı kullanamıyoruz. Erişilemezse, DNS erişimi engelleyen bir güvenlik duvarına veya NSG 'ye sahip olabilirsiniz veya bu durumda olabilir.

Bu öğeler sorunlarınızı yanıtlamazsanız, şunun gibi şeyler için göz atın: 

**bölgesel VNet tümleştirmesi**
* hedef, RFC1918 olmayan bir adres değil ve 1 olarak ayarlanmamış WEBSITE_VNET_ROUTE_ALL yok
* Tümleştirme alt ağınızdan çıkış engelliyor bir NSG var
* ExpressRoute veya VPN üzerinden gitecekseniz, şirket içi ağ geçidiniz trafiği Azure 'a yeniden yönlendirmek üzere yapılandırılmış mı? VNet 'iniz içindeki uç noktalara ulaşabilseniz de şirket içi değil, rotalarınızı denetleyin.
* Tümleştirme alt ağında temsili ayarlamak için yeterli izinlere sahip misiniz? Bölgesel VNet tümleştirme yapılandırması sırasında, tümleştirme alt ağınız Microsoft. Web 'e Temsilcili olur. VNet tümleştirme Kullanıcı arabirimi, alt ağı Microsoft. Web 'e otomatik olarak devredebilir. Hesabınız temsilci seçmek için yeterli ağ izinlerine sahip değilse, alt ağı devretmek için tümleştirme alt ağlarınızın özniteliklerini ayarlayabilen bir kişiye ihtiyacınız olacaktır. Tümleştirme alt ağını el ile atamak için Azure sanal ağ alt ağı Kullanıcı arabirimine gidin ve Microsoft. Web için temsilci belirleyin. 

**Ağ Geçidi gerekli VNet tümleştirmesi**
* , RFC 1918 aralıklarında (10.0.0.0-10.255.255.255/172.16.0.0-172.31.255.255/192.168.0.0-192.168.255.255) Noktadan siteye adres aralığıdır mi?
* Ağ Geçidi portalda mi çalışıyor? Ağ geçidinizin kapalıysa, daha sonra geri getirin.
* Sertifikalar eşitlenmiş olarak gösteriliyor mu veya ağ yapılandırmasının değiştiğini kuşkulu mı?  Sertifikalarınız eşitlenmemiş veya VNet yapılandırmanızda ASPs 'niz ile eşitlenmemiş bir değişiklik yapıldığından şüphelenirseniz, "ağa eşitleme" düğmesine basın.
* VPN üzerinden gitecekseniz, trafiği Azure 'a yönlendirmek için yapılandırılmış şirket içi ağ geçidi mi? VNet 'iniz içindeki uç noktalara ulaşabilseniz de şirket içi değil, rotalarınızı denetleyin.
* hem site hem de ExpressRoute 'a işaret eden bir birlikte bulunma ağ geçidi kullanmayı deniyor musunuz? VNet tümleştirmesinde birlikte bulunma ağ geçitleri desteklenmez 

Belirli bir konağa erişimi engelleyip engellemediğini göremediği için, ağ sorunlarını ayıklama işlemi bir zorluk mıdır: bağlantı noktası birleşimi. Bazı nedenler şunlardır:

* ana bilgisayarınızda, Noktadan siteye IP aralığına olan uygulama bağlantı noktasına erişimi engellediği bir güvenlik duvarınız var. Çakışan alt ağlar genellikle genel erişim gerektirir.
* hedef ana bilgisayarınız çalışmıyor
* uygulamanız çalışmıyor
* yanlış IP veya ana bilgisayar adı vardı
* Uygulamanız, beklediğinizden farklı bir bağlantı noktasını dinliyor. Uç nokta konağında "netstat-Aon" öğesini kullanarak, işlem KIMLIĞINIZI dinleme bağlantı noktasıyla eşleştirebilirsiniz. 
* ağ güvenlik gruplarınız, Noktadan siteye IP aralığına, uygulama ana bilgisayarınıza ve bağlantı noktasına erişimi engelleyecek şekilde yapılandırılır.

Uygulamanızın gerçekten kullanacağı adresi bilmiyorsanız unutmayın. Tümleştirme alt ağında veya Noktadan siteye adres aralığında herhangi bir adres olabilir, bu nedenle tüm adres aralığından erişime izin vermeniz gerekir. 

Ek hata ayıklama adımları şunlardır:

* VNet 'iniz içindeki bir VM 'ye bağlanın ve kaynak konağınız üzerinde bağlantı noktasını buradan ulaşmaya çalışın. TCP erişimini test etmek için PowerShell komut **testi-NetConnection**komutunu kullanın. Söz dizimi aşağıdaki gibidir:

      test-netconnection hostname [optional: -Port]

* bir VM üzerinde bir uygulama açın ve **tcup** kullanarak uygulamadan konsoldan bu konağa ve bağlantı noktasına erişimi test edin

#### <a name="on-premises-resources"></a>Şirket içi kaynaklar ####

Uygulamanız Şirket içindeki bir kaynağa ulaşamadıysanız, sanal ağınızdan kaynağa ulaşabilseniz kontrol edin. TCP erişimini denetlemek için **Test-NetConnection** PowerShell komutunu kullanın. VM 'niz şirket içi kaynağına ulaşamadıysa, VPN veya ExpressRoute bağlantınız düzgün şekilde yapılandırılmamış olabilir.

VNet barındırılan VM 'niz şirket içi sisteminize ulaşılırsa ancak uygulamanız bu nedenle, nedeni aşağıdakilerden biri olabilir:

* rotalarınız, alt ağlarınız ile yapılandırılmaz veya şirket içi ağ geçidinizdeki site adres aralıklarını işaret etmez
* ağ güvenlik gruplarınız, Noktadan siteye IP aralığınız için erişimi engelliyor
* Şirket içi güvenlik duvarları, Noktadan siteye IP aralığından trafiği engelliyor
* bölgesel VNet tümleştirme özelliğini kullanarak, RFC 1918 olmayan bir adrese ulaşmaya çalışıyorsunuz


## <a name="automation"></a>Otomasyon

Bölgesel VNet tümleştirmesi için CLı desteği vardır. Aşağıdaki komutlara erişmek için [Azure CLI 'Yı yüklemelisiniz][installCLI]. 

        az webapp vnet-integration --help

        Group
            az webapp vnet-integration : Methods that list, add, and remove virtual network integrations
            from a webapp.
                This command group is in preview. It may be changed/removed in a future release.
        Commands:
            add    : Add a regional virtual network integration to a webapp.
            list   : List the virtual network integrations on a webapp.
            remove : Remove a regional virtual network integration from webapp.

        az appservice vnet-integration --help

        Group
            az appservice vnet-integration : A method that lists the virtual network integrations used in an
            appservice plan.
                This command group is in preview. It may be changed/removed in a future release.
        Commands:
            list : List the virtual network integrations used in an appservice plan.

Ağ Geçidi gerekli VNet tümleştirmesi için, PowerShell kullanarak App Service bir Azure sanal ağı ile tümleştirebilirsiniz. Bir çalıştırma için, bkz. bir [uygulamayı Azure sanal ağına bağlama Azure App Service](https://gallery.technet.microsoft.com/scriptcenter/Connect-an-app-in-Azure-ab7527e3).


<!--Image references-->
[1]: ./media/web-sites-integrate-with-vnet/vnetint-app.png
[2]: ./media/web-sites-integrate-with-vnet/vnetint-addvnet.png
[3]: ./media/web-sites-integrate-with-vnet/vnetint-classic.png
[4]: ./media/web-sites-integrate-with-vnet/vnetint-appsetting.png
[5]: ./media/web-sites-integrate-with-vnet/vnetint-regionalworks.png
[6]: ./media/web-sites-integrate-with-vnet/vnetint-gwworks.png


<!--Links-->
[VNETOverview]: https://azure.microsoft.com/documentation/articles/virtual-networks-overview/ 
[AzurePortal]: https://portal.azure.com/
[ASPricing]: https://azure.microsoft.com/pricing/details/app-service/
[VNETPricing]: https://azure.microsoft.com/pricing/details/vpn-gateway/
[DataPricing]: https://azure.microsoft.com/pricing/details/data-transfers/
[V2VNETP2S]: https://azure.microsoft.com/documentation/articles/vpn-gateway-howto-point-to-site-rm-ps/
[ASEintro]: environment/intro.md
[ILBASE]: environment/create-ilb-ase.md
[V2VNETPortal]: ../vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal.md
[VPNERCoex]: ../expressroute/expressroute-howto-coexist-resource-manager.md
[ASE]: environment/intro.md
[creategatewaysubnet]: ../vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal.md#creategw
[creategateway]: https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal#creategw
[setp2saddresses]: https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal#addresspool
[VNETnsg]: https://docs.microsoft.com/azure/virtual-network/security-overview/
[VNETRouteTables]: https://docs.microsoft.com/azure/virtual-network/manage-route-table/
[installCLI]: https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest/
