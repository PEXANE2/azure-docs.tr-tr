---
title: Azure AD Etki Alanı Hizmetleri için ağ planlaması ve bağlantılar | Microsoft Dokümanlar
description: Azure Active Directory Domain Services'ı çalıştırDığınızda bağlantı için kullanılan bazı sanal ağ tasarımı konuları ve kaynakları hakkında bilgi edinin.
services: active-directory-ds
author: iainfoulds
manager: daveba
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.topic: conceptual
ms.date: 03/30/2020
ms.author: iainfou
ms.openlocfilehash: 69f8cd0f78a45c6c5e53368edc5902c4b6695701
ms.sourcegitcommit: 27bbda320225c2c2a43ac370b604432679a6a7c0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/31/2020
ms.locfileid: "80408824"
---
# <a name="virtual-network-design-considerations-and-configuration-options-for-azure-ad-domain-services"></a>Azure AD Etki Alanı Hizmetleri için sanal ağ tasarımı konuları ve yapılandırma seçenekleri

Azure Active Directory Domain Services (AD DS), diğer uygulamalara ve iş yüklerine kimlik doğrulama ve yönetim hizmetleri sağladığından, ağ bağlantısı önemli bir bileşendir. Doğru yapılandırılmış sanal ağ kaynakları olmadan, uygulamalar ve iş yükleri Azure AD DS tarafından sağlanan özelliklerle iletişim kuramaz ve kullanamaz. Azure AD DS'nin uygulamalarınıza ve iş yüklerinizi gerektiği gibi sunabileceğinden emin olmak için sanal ağ gereksinimlerinizi planlayın.

Bu makalede, Azure REKLAM DS'yi desteklemek için bir Azure sanal ağının tasarım konuları ve gereksinimleri sıralanmaktadır.

## <a name="azure-virtual-network-design"></a>Azure sanal ağ tasarımı

Ağ bağlantısı sağlamak ve uygulamaların ve hizmetlerin Azure AD DS'ye karşı kimlik doğrulaması sağlamasına izin vermek için bir Azure sanal ağı ve alt ağı kullanırsınız. İdeal olarak, Azure AD DS'si kendi sanal ağına dağıtılmalıdır. Yönetim VM veya hafif uygulama iş yüklerini barındırmak için aynı sanal ağa ayrı bir uygulama alt ağı ekleyebilirsiniz. Azure AD DS sanal ağına bakan daha büyük veya karmaşık uygulama iş yükleri için ayrı bir sanal ağ genellikle en uygun tasarımdır. Sanal ağ ve alt ağ için aşağıdaki bölümlerde belirtilen gereksinimleri karşılamanız koşuluyla, diğer tasarım seçenekleri geçerlidir.

Azure AD DS için sanal ağı tasarlarken aşağıdaki hususlar geçerlidir:

* Azure AD DS'nin sanal ağınızla aynı Azure bölgesine dağıtılması gerekir.
    * Şu anda, Azure AD kiracı başına yalnızca bir Azure AD DS yönetilen etki alanı dağıtabilirsiniz. Azure AD DS yönetilen etki alanı tek bir bölgeye dağıtılır. [Azure AD DS'yi destekleyen](https://azure.microsoft.com/global-infrastructure/services/?products=active-directory-ds&regions=all)bir bölgede sanal ağ oluşturduğunuzdan veya seçtiğinizden emin olun.
* Diğer Azure bölgelerinin ve uygulama iş yüklerinizi barındıran sanal ağların yakınlığını göz önünde bulundurun.
    * Gecikme süresini en aza indirmek için, temel uygulamalarınızı Azure AD DS yönetilen etki alanınızın sanal ağ alt ağına yakın veya aynı bölgede tutun. Azure sanal ağları arasında sanal ağ veya sanal özel ağ (VPN) bağlantıları kullanabilirsiniz. Bu bağlantı seçenekleri aşağıdaki bölümde ele alınmıştır.
* Sanal ağ, Azure AD DS tarafından sağlananlar dışında DNS hizmetlerine güvenemez.
    * Azure AD DS kendi DNS hizmetini sağlar. Sanal ağ bu DNS hizmet adreslerini kullanacak şekilde yapılandırılmalıdır. Ek ad alanları için ad çözümlemesi koşullu iletmeler kullanılarak gerçekleştirilebilir.
    * VM'ler de dahil olmak üzere diğer DNS sunucularından gelen sorguları yönlendirmek için özel DNS sunucu ayarlarını kullanamazsınız. Sanal ağdaki kaynaklar Azure AD DS tarafından sağlanan DNS hizmetini kullanmalıdır.

> [!IMPORTANT]
> Hizmeti etkinleştirdikten sonra Azure AD DS'yi farklı bir sanal ağa taşıyamazsınız.

Azure AD DS yönetilen etki alanı, Azure sanal ağındaki bir alt ağa bağlanır. Azure AD DS için aşağıdaki hususlargöz önünde bulundurularak bu alt ağı tasarla:

* Azure AD DS'nin kendi alt ağına dağıtılması gerekir. Varolan bir alt ağ veya ağ geçidi alt ağı kullanmayın.
* Azure AD DS yönetilen etki alanının dağıtımı sırasında bir ağ güvenlik grubu oluşturulur. Bu ağ güvenlik grubu, doğru hizmet iletişimi için gerekli kuralları içerir.
    * Varolan bir ağ güvenlik grubunu kendi özel kurallarınız ile oluşturmayın veya kullanmayın.
* Azure AD DS 3-5 IP adresi gerektirir. Alt net IP adres aralığınızın bu adres sayısını sağlayabileceğinden emin olun.
    * Kullanılabilir IP adreslerinin kısıtlanması, Azure AD Etki Alanı Hizmetlerinin iki etki alanı denetleyicisi tutmasını engelleyebilir.

Aşağıdaki örnek diyagram, Azure AD DS'nin kendi alt ağına sahip olduğu, dış bağlantı için bir ağ geçidi alt ağı olduğu ve uygulama iş yüklerinin sanal ağ içinde bağlı bir alt ağda bulunduğu geçerli bir tasarımı özetlemektedir:

![Önerilen alt ağ tasarımı](./media/active-directory-domain-services-design-guide/vnet-subnet-design.png)

## <a name="connections-to-the-azure-ad-ds-virtual-network"></a>Azure AD DS sanal ağına bağlantılar

Önceki bölümde belirtildiği gibi, Azure'da yalnızca tek bir sanal ağda azure AD Etki Alanı Hizmetleri yönetilen bir etki alanı oluşturabilirsiniz ve Azure AD kiracı başına yalnızca bir yönetilen etki alanı oluşturulabilir. Bu mimariye bağlı olarak, uygulama iş yüklerinizi barındıran bir veya daha fazla sanal ağı Azure AD DS sanal ağınıza bağlamanız gerekebilir.

Aşağıdaki yöntemlerden birini kullanarak diğer Azure sanal ağlarında barındırılan uygulama iş yüklerini bağlayabilirsiniz:

* Sanal ağ eşleme
* Sanal özel ağ (VPN)

### <a name="virtual-network-peering"></a>Sanal ağ eşleme

Sanal ağ eşleme, Aynı bölgedeki iki sanal ağı Azure omurga ağı üzerinden birbirine bağlayan bir mekanizmadır. Küresel sanal ağ eşlemi, Azure bölgeleri arasında sanal ağı bağlayabilir. Bir kez bakıldıktan sonra, iki sanal ağ, VM'ler gibi kaynakların özel IP adreslerini kullanarak birbirleriyle doğrudan iletişim kurmasına izin verebiliyor. Sanal ağ eşlemeyi kullanmak, diğer sanal ağlarda dağıtılan uygulama iş yükleri ile Azure AD DS yönetilen bir etki alanı dağıtmanızı sağlar.

![Eşleme kullanarak sanal ağ bağlantısı](./media/active-directory-domain-services-design-guide/vnet-peering.png)

Daha fazla bilgi için azure [sanal ağ izleme genel bakışına](../virtual-network/virtual-network-peering-overview.md)bakın.

### <a name="virtual-private-networking-vpn"></a>Sanal Özel Ağ (VPN)

Sanal ağı başka bir sanal ağa (VNet-to-VNet), sanal ağı şirket içi bir konuma yapılandırabileceğiniz şekilde bağlayabilirsiniz. Her iki bağlantı da IPsec/IKE kullanarak güvenli bir tünel oluşturmak için vpn ağ geçidi kullanır. Bu bağlantı modeli, Azure AD DS'yi azure sanal ağına dağıtmanıza ve şirket içi konumlara veya diğer bulutlara bağlanmanıza olanak tanır.

![VPN Ağ Geçidi kullanarak sanal ağ bağlantısı](./media/active-directory-domain-services-design-guide/vnet-connection-vpn-gateway.jpg)

Sanal özel ağ kullanma hakkında daha fazla bilgi için [Azure portalını kullanarak VNet'ten VNet'e VPN ağ geçidi bağlantısını yapılandır'ı](../vpn-gateway/vpn-gateway-howto-vnet-vnet-resource-manager-portal.md)okuyun.

## <a name="name-resolution-when-connecting-virtual-networks"></a>Sanal ağları bağlarken ad çözünürlüğü

Azure AD Etki Alanı Hizmetleri sanal ağına bağlı sanal ağların genellikle kendi DNS ayarları vardır. Sanal ağları bağladığınızda, Azure AD DS yönetilen etki alanı tarafından sağlanan hizmetleri çözmek için bağlanan sanal ağ için ad çözümlemeyi otomatik olarak yapılandırmaz. Bağlanan sanal ağlardaki ad çözümlemesi, uygulama iş yüklerinin Azure AD Etki Alanı Hizmetlerini bulmasını sağlayacak şekilde yapılandırılmalıdır.

DNS sunucusunda bağlanan sanal ağları destekleyen koşullu DNS iletmelerini kullanarak veya Azure AD Etki Alanı Hizmeti sanal ağındaki aynı DNS IP adreslerini kullanarak ad çözümlemesini etkinleştirebilirsiniz.

## <a name="network-resources-used-by-azure-ad-ds"></a>Azure AD DS tarafından kullanılan ağ kaynakları

Azure AD DS yönetilen etki alanı, dağıtım sırasında bazı ağ kaynakları oluşturur. Bu kaynaklar, Azure AD DS yönetilen etki alanının başarılı çalışması ve yönetimi için gereklidir ve el ile yapılandırılmamalıdır.

| Azure kaynağı                          | Açıklama |
|:----------------------------------------|:---|
| Ağ arabirim kartı                  | Azure AD DS, Yönetilen etki alanını Windows Server'da Azure VM olarak çalışan iki etki alanı denetleyicisi (DCs) üzerinde barındırıyor. Her VM, sanal ağ alt ağınıza bağlanan bir sanal ağ arabirimine sahiptir. |
| Dinamik standart genel IP adresi      | Azure AD DS, standart bir SKU genel IP adresi kullanarak eşitleme ve yönetim hizmetiyle iletişim kurar. Ortak IP adresleri hakkında daha fazla bilgi için [Azure'da IP adresi türleri ve ayırma yöntemlerine](../virtual-network/virtual-network-ip-addresses-overview-arm.md)bakın. |
| Azure standart yük dengeleyicisi            | Azure AD DS, ağ adresi çevirisi (NAT) ve yük dengeleme (güvenli LDAP ile kullanıldığında) için standart bir SKU yük dengeleyicisi kullanır. Azure yük dengeleyicileri hakkında daha fazla bilgi için azure [yük bakiyesi nedir?](../load-balancer/load-balancer-overview.md) |
| Ağ adresi çevirisi (NAT) kuralları | Azure AD DS, yük dengeleyicisi üzerinde üç NAT kuralı oluşturur ve kullanır : güvenli HTTP trafiği için bir kural ve powershell remoting'i güvenli hale almak için iki kural. |
| Yük dengeleyici kuralları                     | Azure AD DS yönetilen bir etki alanı TCP bağlantı noktası 636'da güvenli LDAP için yapılandırıldığında, trafiği dağıtmak için yük dengeleyicisi üzerinde üç kural oluşturulur ve kullanılır. |

> [!WARNING]
> Yük bakiyesini veya kuralları el ile yapılandırmak gibi Azure AD DS tarafından oluşturulan ağ kaynaklarından hiçbirini silmeyin veya değiştirmeyin. Ağ kaynaklarından herhangi birini siler veya değiştirirseniz, Bir Azure AD DS hizmet kesintisi oluşabilir.

## <a name="network-security-groups-and-required-ports"></a>Ağ güvenlik grupları ve gerekli bağlantı noktaları

Ağ [güvenlik grubu (NSG),](../virtual-network/virtual-networks-nsg.md) bir Azure sanal ağında ağ trafiğinin trafiğe girmesine izin veren veya reddeden kuralların bir listesini içerir. Hizmetin kimlik doğrulama ve yönetim işlevleri sağlamasına izin veren bir dizi kural içeren Azure AD DS'yi dağıttığınızda bir ağ güvenlik grubu oluşturulur. Bu varsayılan ağ güvenlik grubu, Azure AD DS yönetilen etki alanınızın dağıtılan sanal ağ alt ağıyla ilişkilidir.

Azure AD DS'nin kimlik doğrulama ve yönetim hizmetleri sağlaması için aşağıdaki ağ güvenliği grubu kuralları gereklidir. Azure AD DS yönetilen etki alanınızın dağıtılan sanal ağ alt ağı için bu ağ güvenlik grubu kurallarını düzenlemeyin veya silmeyin.

| Bağlantı noktası numarası | Protokol | Kaynak                             | Hedef | Eylem | Gerekli | Amaç |
|:-----------:|:--------:|:----------------------------------:|:-----------:|:------:|:--------:|:--------|
| 443         | TCP      | AzureActiveDirectoryDomainServices | Herhangi biri         | İzin Ver  | Evet      | Azure AD kiracınızla eşitleme. |
| 3389        | TCP      | Corpnetsaw                         | Herhangi biri         | İzin Ver  | Evet      | Etki alanınızın yönetimi. |
| 5986        | TCP      | AzureActiveDirectoryDomainServices | Herhangi biri         | İzin Ver  | Evet      | Etki alanınızın yönetimi. |
| 636         | TCP      | Herhangi biri                                | Herhangi biri         | İzin Ver  | Hayır       | Yalnızca güvenli LDAP 'yi (LDAPS) yapılandırdığınızda etkinleştirilir. |

> [!WARNING]
> Bu ağ kaynaklarını ve yapılandırmalarını el ile düzenlemeyin. Yanlış yapılandırılmış bir ağ güvenlik grubunu veya kullanıcı tanımlı bir rota tablosunu Azure AD DS'nin dağıtıldığı alt ağla ilişkilendirdiğinizde, Microsoft'un etki alanına hizmet verme ve yönetme yeteneğini bozabilirsiniz. Azure AD kiracınız ile Azure AD DS yönetilen etki alanınız arasındaki eşitleme de kesintiye uğrar.
>
> *AllowVnetInBound*için varsayılan kurallar , *AllowAzureLoadBalancerInBound*, *DenyAllInBound*, *AllowVnetOutBound*, *AllowInternetOutBound*, ve *DenyAllOutBound* ağ güvenlik grubu için de vardır. Bu varsayılan kuralları düzenlemayın veya silmeyin.
>
> Azure SLA' sı, Azure AD DS' nin etki alanınızı güncelleştirmesini ve yönetmesini engelleyen, yanlış yapılandırılmış bir ağ güvenlik grubu ve/veya kullanıcı tanımlı rota tablolarının uygulandığı dağıtımlar için geçerli değildir.

### <a name="port-443---synchronization-with-azure-ad"></a>Port 443 - Azure AD ile senkronizasyon

* Azure AD kiracınızı Azure AD DS yönetilen etki alan adınız ile senkronize etmek için kullanılır.
* Bu bağlantı noktasına erişmeden, Azure AD DS yönetilen etki alanınız Azure AD kiracınızla eşitleme yapamaz. Kullanıcılar, parolalarındaki değişiklikler Azure AD DS yönetilen etki alanınızla eşitlenmeyebileceği için oturum açamayabilir.
* Bu bağlantı noktasına IP adreslerine gelen erişim, **AzureActiveDirectoryDomainServices** hizmet etiketi kullanılarak varsayılan olarak kısıtlanır.
* Bu bağlantı noktasından giden erişimi kısıtlamayın.

### <a name="port-3389---management-using-remote-desktop"></a>Port 3389 - uzak masaüstü kullanarak yönetim

* Azure AD DS yönetilen etki alanınızdaki etki alanı denetleyicilerine uzak masaüstü bağlantıları için kullanılır.
* Varsayılan ağ güvenlik grubu kuralı, trafiği daha da kısıtlamak için *CorpNetSaw* hizmet etiketini kullanır.
    * Bu hizmet etiketi, Yalnızca Microsoft kurumsal ağındaki güvenli erişim iş istasyonlarının Azure AD DS yönetilen etki alanında uzak masaüstünü kullanmasına izin verir.
    * Erişime yalnızca yönetim veya sorun giderme senaryoları gibi iş gerekçesiyle izin verilir.
* Bu kural *Reddet*olarak ayarlanabilir ve yalnızca gerektiğinde *İzin Ver* olarak ayarlanabilir. Çoğu yönetim ve izleme görevi PowerShell remoting kullanılarak gerçekleştirilir. RDP yalnızca Microsoft'un gelişmiş sorun giderme için yönetilen etki alanınıza uzaktan bağlanması gereken nadir durumlarda kullanılır.

> [!NOTE]
> Bu ağ güvenlik grubu kuralını yönetmeye çalışırsanız, portaldan *CorpNetSaw* hizmet etiketini el ile seçemezsiniz. *CorpNetSaw* hizmet etiketini kullanan bir kuralı el ile yapılandırmak için Azure PowerShell veya Azure CLI'yi kullanmanız gerekir.

### <a name="port-5986---management-using-powershell-remoting"></a>Port 5986 - PowerShell remoting kullanarak yönetim

* Azure AD DS yönetilen etki alanınızda PowerShell remotingini kullanarak yönetim görevlerini gerçekleştirmek için kullanılır.
* Bu bağlantı noktasına erişim olmadan, Azure AD DS yönetilen etki alanınız güncelleştirilemez, yapılandırılamaz, yedeklenemez veya izlenemez.
* Kaynak Yöneticisi tabanlı sanal ağ kullanan Azure AD DS yönetilen etki alanları için, bu bağlantı noktasına gelen erişimi *AzureActiveDirectoryDomainServices* hizmet etiketiyle sınırlandırabilirsiniz.
    * Klasik tabanlı bir sanal ağ kullanarak eski Azure AD DS yönetilen etki alanları için, aşağıdaki kaynak IP adresleri için bu bağlantı noktasına gelen erişimi kısıtlayabilirsiniz: *52.180.183.8*, *23.101.0.70*, *52.225.184.198*, *52.179.126.223*, *13.74.249.156*, *52.187.117.83*, *52.161.13.95*, *104.40.156.18*ve *104.40.87.209*.

    > [!NOTE]
    > 2017 yılında Azure AD Etki Alanı Hizmetleri, bir Azure Kaynak Yöneticisi ağında barındırılabilmek için kullanılabilir hale geldi. O zamandan beri, Azure Kaynak Yöneticisi'nin modern özelliklerini kullanarak daha güvenli bir hizmet oluşturabildik. Azure Kaynak Yöneticisi dağıtımları klasik dağıtımların tamamen yerini aldığı için, Azure AD DS klasik sanal ağ dağıtımları 1 Mart 2023'te kullanımdan kaldırılacaktır.
    >
    > Daha fazla bilgi için [resmi amortisman bildirimine](https://azure.microsoft.com/updates/we-are-retiring-azure-ad-domain-services-classic-vnet-support-on-march-1-2023/) bakın

## <a name="user-defined-routes"></a>Kullanıcı tanımlı yollar

Kullanıcı tanımlı rotalar varsayılan olarak oluşturulmaz ve Azure AD DS'nin düzgün çalışması için gerekli değildir. Rota tablolarını kullanmanız gerekiyorsa, *0.0.0.0* rotasında değişiklik yapmaktan kaçının. Bu rotadaki değişiklikler Azure AD Etki Alanı Hizmetlerini bozar ve yönetilen etki alanını desteklenmeyen bir duruma sokar.

Ayrıca, gelen trafiği ilgili Azure hizmet etiketlerinde yer alan IP adreslerinden Azure AD Etki Alanı Hizmetleri alt ağına yönlendirmeniz gerekir. Hizmet etiketleri ve bunların ilişkili IP adresi hakkında daha fazla bilgi için Azure [IP Aralıkları ve Hizmet Etiketleri - Genel Bulut'](https://www.microsoft.com/en-us/download/details.aspx?id=56519)a bakın.

> [!CAUTION]
> Bu Azure veri merkezi IP aralıkları önceden haber verilmeden değişebilir. En son IP adreslerine sahip olduğunuzu doğrulamak için işlemlere sahip olduğundan emin olun.

## <a name="next-steps"></a>Sonraki adımlar

Azure AD DS tarafından kullanılan ağ kaynakları ve bağlantı seçeneklerinden bazıları hakkında daha fazla bilgi için aşağıdaki makalelere bakın:

* [Azure sanal ağ eşleme](../virtual-network/virtual-network-peering-overview.md)
* [Azure VPN ağ geçitleri](../vpn-gateway/vpn-gateway-about-vpn-gateway-settings.md)
* [Azure ağ güvenlik grupları](../virtual-network/security-overview.md)
