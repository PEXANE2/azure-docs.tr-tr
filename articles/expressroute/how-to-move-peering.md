---
title: "Azure ExpressRoute: Ortak bir bakışla Microsoft'a bakma"
description: Bu makalede, public peering'inizi ExpressRoute'da Microsoft'a yönlendiren lere taşıma adımları gösterilmektedir.
services: expressroute
author: cherylmc
ms.service: expressroute
ms.topic: article
ms.date: 12/12/2019
ms.author: cherylmc
ms.openlocfilehash: 48ecfcc0d6241e7926892a3ca1c9925b0dc07241
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75436851"
---
# <a name="move-a-public-peering-to-microsoft-peering"></a>Genel eşlemeden Microsoft eşlemesine geçiş yapma

Bu makale, genel bir bakış yapılandırmasını, kesinti olmadan Microsoft'a taşımanıza yardımcı olur. ExpressRoute, Azure depolama ve Azure SQL Veritabanı gibi Azure PaaS hizmetleri için rota filtreleriyle Microsoft eşlemesinin kullanılmasını sağlar. Artık Microsoft PaaS ve SaaS hizmetlerine erişmek için tek bir yönlendirme etki alanına ihtiyacınız vardır. Kullanmak istediğiniz Azure bölgeleri için PaaS hizmeti ön eklerini tanıtmak için rota filtreleri kullanabilirsiniz.

Azure genel bakışlarının her BGP oturumuyla ilişkili 1 NAT IP adresi vardır. Microsoft'un bakışları, kendi NAT ayırmalarınızı yapılandırmanıza ve seçici önek reklamlar için rota filtrelerini kullanmanıza olanak tanır. Public Peering, Connectivity'nin WAN'ınızdan Microsoft Azure hizmetlerine her zaman başlatıldığı tek yönlü bir hizmettir. Microsoft Azure hizmetleri, bu yönlendirme etki alanı üzerinden ağınıza bağlantı başlatamaz.

Herkese açık bir şekilde bakma özelliği etkinleştirildikten sonra, tüm Azure hizmetlerine bağlanabilirsiniz. Rotaların reklamını yaptığımız hizmetleri seçici olarak seçmene izin vermiyoruz. Microsoft'un bakışları, WAN'ınızla birlikte Microsoft Azure hizmetinden bağlantı başlatılabilen çift yönlü bir bağlantı olsa da. Etki alanlarını yönlendirme ve bakma hakkında daha fazla bilgi için [ExpressRoute devreleri ve yönlendirme etki alanları](expressroute-circuit-peerings.md)hakkında bilgi.

## <a name="before-you-begin"></a><a name="before"></a>Başlamadan önce

Microsoft'un bakışlarına bağlanmak için NAT'yi ayarlamanız ve yönetmeniz gerekir. Bağlantı sağlayıcınız NAT'yi yönetilen bir hizmet olarak ayarlayabilir ve yönetebilir. Microsoft'un baktığında Azure PaaS ve Azure SaaS hizmetlerine erişmeyi planlıyorsanız, NAT IP havuzunu doğru boyutlandırmanız önemlidir. ExpressRoute için NAT hakkında daha fazla bilgi için [Microsoft'un bakışları için NAT gereksinimlerine](expressroute-nat.md#nat-requirements-for-microsoft-peering)bakın. Azure ExpressRoute (Microsoft peering) aracılığıyla Microsoft'a bağlandığınızda, Microsoft'a birden çok bağlantınız vardır. Bağlantıların biri, var olan İnternet bağlantınız diğeri de ExpressRoute aracılığıyla olan bağlantı. Microsoft’a yönelik trafiğin bir kısmı İnternet üzerinden gidip ExpressRoute üzerinden dönebilir ya da tam tersi gerçekleşebilir.

![Çift yönlü bağlantı](./media/how-to-move-peering/bidirectional-connectivity.jpg)

> [!Warning]
> Microsoft'a tanıtılan NAT IP havuzu İnternet'e tanıtılmamalıdır. Bu, diğer Microsoft hizmetlerine bağlantıyı keser.

Microsoft'u yapılandırmadan önce asimetrik yönlendirme ihtarları için [birden çok ağ yolu olan Asimetrik yönlendirmeye](https://docs.microsoft.com/azure/expressroute/expressroute-asymmetric-routing) bakın.

* Genel bakış kullanıyorsanız ve şu anda [Azure Depolama](../storage/common/storage-network-security.md) veya [Azure SQL Veritabanı'na](../sql-database/sql-database-vnet-service-endpoint-rule-overview.md)erişmek için kullanılan genel IP adresleri için IP Ağı kurallarına sahipseniz, Microsoft ile yapılandırılan NAT IP havuzunun Azure depolama hesabı veya Azure SQL hesabı nın ortak IP adresleri listesinde yer aldığından emin olmanız gerekir.<br>
* Microsoft'a herhangi bir kesinti olmadan bakmak için, bu makaledeki adımları sunulduğu sırada kullanın.

## <a name="1-create-microsoft-peering"></a><a name="create"></a>1. Microsoft'a bakış oluşturma

Microsoft'un bakışları oluşturulmamadıysa, Microsoft'un eşlemesi oluşturmak için aşağıdaki makalelerden birini kullanın. Bağlantı sağlayıcınız yönetilen katman 3 hizmetleri sunuyorsa, bağlantı sağlayıcısından devreniz için Microsoft'un eşlemesini etkinleştirmesini isteyebilirsiniz.

Katman 3 sizin yeriniz tarafından yönetiliyorsa, devam etmeden önce aşağıdaki bilgiler gereklidir:

* Birincil bağlantı için bir /30 alt ağı. Bu size ait ve bir RIR / IRR içinde kayıtlı bir geçerli ortak IPv4 ön eki olmalıdır. Microsoft yönlendiricisi için ikinci kullanılabilir IP'yi kullandığından, bu alt ağdan yönlendiricinize ilk kullanılabilir IP adresini atarsınız.<br>
* İkincil bağlantı için bir /30 alt ağı. Bu size ait ve bir RIR / IRR içinde kayıtlı bir geçerli ortak IPv4 ön eki olmalıdır. Microsoft yönlendiricisi için ikinci kullanılabilir IP'yi kullandığından, bu alt ağdan yönlendiricinize ilk kullanılabilir IP adresini atarsınız.<br>
* Bu eşlemenin kurulacağı geçerli bir VLAN kimliği. Bağlantı hattındaki başka bir eşlemenin aynı VLAN kimliğini kullanmadığından emin olun. Hem Birincil hem de İkincil bağlantılar için aynı VLAN Kimliğini kullanmanız gerekir.<br>
* Eşleme için AS numarası. 2 bayt ve 4 bayt AS numaralarını kullanabilirsiniz.<br>
* Tanıtılan önekler: BGP oturumunda tanıtmayı planladığınız tüm öneklerin bir listesini sağlamanız gerekir. Yalnızca ortak IP adresi ön ekleri kabul edilir. Bir önek kümesi göndermeyi planlıyorsanız, virgülle ayrılmış bir liste gönderebilirsiniz. Bu önekler size bir RIR / IRR içinde kaydedilmiş olmalıdır.<br>
* Yönlendirme Kayıt Defteri Adı: AS numarası ve öneklerinin kaydedildiği RIR / IRR’yi belirtebilirsiniz.

* **İsteğe Bağlı** - Müşteri ASN: Emsal AS numarasına kayıtlı olmayan önekleri reklamlıyorsanız, kayıtlı oldukları AS numarasını belirtebilirsiniz.<br>
* **İsteğe bağlı** - Kullanmayı seçerseniz MD5 karma.

Microsoft'un eşlemesini sağlamak için ayrıntılı yönergeler aşağıdaki makalelerde bulunabilir:

* [Azure portalLarını kullanarak Microsoft eşleme oluşturma](expressroute-howto-routing-portal-resource-manager.md#msft)<br>
* [Azure Powershell'i kullanarak Microsoft eşleme oluşturma](expressroute-howto-routing-arm.md#msft)<br>
* [Azure CLI'yi kullanarak Microsoft eşleme oluşturma](howto-routing-cli.md#msft)

## <a name="2-validate-microsoft-peering-is-enabled"></a><a name="validate"></a>2. Microsoft'un bakışlarını doğrulama etkin

Microsoft eşlemenin etkin olduğunu ve reklamı yapılandırılan genel öneklerin yapılandırılan durumda olduğunu doğrulayın.

* [Azure portalında](expressroute-howto-routing-portal-resource-manager.md#getmsft)<br>
* [Azure PowerShell](expressroute-howto-routing-arm.md#getmsft)<br>
* [Azure CLI](howto-routing-cli.md#getmsft)

## <a name="3-configure-and-attach-a-route-filter-to-the-circuit"></a><a name="routefilter"></a>3. Bir rota filtresini yapılandırma ve devreye iliştirme

Varsayılan olarak, yeni Microsoft eşlemeleri, devreye bir rota filtresi iliştirilene kadar öneklerin reklamını yapmaz. Bir rota filtresi kuralı oluşturduğunuzda, Azure PaaS hizmetleri için tüketmek istediğiniz Azure bölgeleri için hizmet toplulukları listesini belirtebilirsiniz. Bu, aşağıdaki ekran görüntüsünde gösterildiği gibi, rotaları gereksiniminize göre filtreleme esnekliği sağlar:

![Ortak bakışları birleştirme](./media/how-to-move-peering/routefilter.jpg)

Aşağıdaki makalelerden herhangi birini kullanarak rota filtrelerini yapılandırın:

* [Azure portalLarını kullanarak Microsoft eşleme için rota filtrelerini yapılandırma](how-to-routefilter-portal.md)<br>
* [Azure PowerShell'i kullanarak Microsoft eşlenetrasyon için rota filtrelerini yapılandırma](how-to-routefilter-powershell.md)<br>
* [Azure CLI kullanarak Microsoft eşlemesi için rota filtrelerini yapılandırma](how-to-routefilter-cli.md)

## <a name="4-delete-the-public-peering"></a><a name="delete"></a>4. Kamu bakan silme

Microsoft eşlemenin yapılandırıldığından ve tüketmek istediğiniz öneklerin Microsoft'un eşlemelerinde doğru şekilde reklamı nın yapıldığı doğrulandıktan sonra, ortak bakışları silebilirsiniz. Genel bakışları silmek için aşağıdaki makalelerden birini kullanın:

* [Azure PowerShell'i kullanarak Azure genel eşlemi silme](about-public-peering.md#powershell)
* [CLI kullanarak Azure ortak eşlemi silme](about-public-peering.md#cli)
  
## <a name="5-view-peerings"></a><a name="view"></a>5. Görünümleri görüntüleyin
  
Azure portalında tüm ExpressRoute devrelerinin ve eşlemelerinin listesini görebilirsiniz. Daha fazla bilgi için [bkz.](expressroute-howto-routing-portal-resource-manager.md#getmsft)

## <a name="next-steps"></a>Sonraki adımlar

ExpressRoute hakkında daha fazla bilgi için, bkz. [ExpressRoute SSS](expressroute-faqs.md).
