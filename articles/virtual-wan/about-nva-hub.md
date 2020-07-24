---
title: "Azure sanal WAN: hub 'da ağ sanal gereci hakkında"
description: Bu makalede, sanal WAN hub 'ında ağ sanal cihazları hakkında bilgi edineceksiniz.
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: conceptual
ms.date: 07/06/2020
ms.author: scottnap
Customer intent: As someone with a networking background, I want to learn about Network Virtual Appliances in the Virtual WAN hub.
ms.openlocfilehash: ad7c7fb5111ce700a5725336b7c9db788e178c4c
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87100078"
---
# <a name="about-network-virtual-appliance-in-an-azure-virtual-wan-hub-preview"></a>Bir Azure sanal WAN hub 'ında ağ sanal gereci hakkında (Önizleme)

Azure sanal WAN, müşteri şirket Içi ekipmanlarını (CPE) sanal hub 'daki bir Azure VPN ağ geçidine bağlamayı kolaylaştıran bir Otomasyon oluşturmak için ağ ortaklarıyla çalıştı. Azure, müşterilerin bir üçüncü taraf ağ sanal gereci (NVA) doğrudan sanal hub 'a dağıtmasını sağlamak üzere Select Networking partner ile çalışmaktadır. Bu, kendi şubelerini sanal hub 'daki aynı marka NVA 'ya bağlamak isteyen müşterilerin, özel uçtan uca SD-WAN özelliğinden yararlanabilmeleri için bu özellikleri sağlar.

Barnetcuda Networks, sanal WAN hub 'ına doğrudan [Barrampada CloudGen WAN](https://www.barracuda.com/products/cloudgenwan) ürünüyle dağıtılabilecek bir NVA teklifi sağlayan ilk iş ortağıdır. Azure daha fazla iş ortağıyla birlikte çalışarak diğer tekliflerin izlediğini görmenizi bekler.

> [!NOTE]
> Yalnızca sanal WAN hub 'ına dağıtılabilecek olan NVA teklifleri sanal WAN hub 'ına dağıtılabilir. Azure 'da rastgele bir sanal ağa dağıtılamaz.

## <a name="how-does-it-work"></a><a name="how"></a>Nasıl çalışır?

Doğrudan Azure sanal WAN hub 'ına dağıtılabilecek olan NVA 'lar, özellikle sanal hub 'da kullanılmak üzere tasarlanmıştır. NVA teklifi, yönetilen bir uygulama olarak Azure Marketi 'nde yayımlanır ve müşteriler teklifi doğrudan Azure Market 'ten dağıtabilir veya Azure portal aracılığıyla teklifi sanal hub 'dan dağıtabilir.

:::image type="content" source="./media/about-nva-hub/high-level-process.png" alt-text="İşleme genel bakış":::

Her ortağın NVA teklifi, dağıtım gereksinimlerine bağlı olarak biraz farklı bir deneyim ve işlevlere sahiptir. Ancak, sanal WAN hub 'ında NVA için tüm iş ortağı tekliflerinden ortak olan bazı şeyler vardır.

* Azure Marketi aracılığıyla sunulan bir yönetilen uygulama deneyimi.
* NVA altyapısı birim tabanlı kapasite ve faturalandırma.
* Sistem durumu ölçümleri Azure Izleyici üzerinden ortaya çıkmış.

### <a name="managed-application"></a><a name="managed"></a>Yönetilen uygulama

Sanal WAN hub 'ına dağıtılabilecek olan tüm NVA tekliflerinin Azure Marketi 'nde kullanılabilen **yönetilen bir uygulaması** olacaktır. Yönetilen uygulamalar iş ortaklarının şunları yapmasına izin verir:

* NVA için özel bir dağıtım deneyimi oluşturun.
* Sanal WAN hub 'ında NVA 'yı doğrudan oluşturmalarına izin veren özelleştirilmiş bir Kaynak Yöneticisi şablonu sağlayın.
* Yazılım lisanslama maliyetlerini doğrudan veya Azure Marketi aracılığıyla faturanız.
* Özel özellikleri ve kaynak ölçümlerini kullanıma sunun.

NVA Iş ortakları, Gereç dağıtımına, yapılandırma lisanslarına ve yönetim ihtiyaçlarına bağlı olarak farklı kaynaklar oluşturabilir. Bir müşteri, sanal WAN hub 'ında tüm yönetilen uygulamalar gibi bir NVA oluşturduğunda, aboneliklerinde iki kaynak grubu oluşturulur.

* **Müşteri kaynak grubu** -bu, yönetilen uygulama için bir uygulama yer tutucusu içerir. İş ortakları bunu, burada seçtikleri müşteri özelliklerini göstermek için kullanabilir.
* **Yönetilen kaynak grubu** -müşteriler, yönetilen uygulamanın yayımcısı tarafından denetlenerek bu kaynak grubundaki kaynakları doğrudan yapılandıramaz veya değiştiremezler. Bu kaynak grubu **Networkvirtualapplisları** kaynağını içerir.

:::image type="content" source="./media/about-nva-hub/managed-app.png" alt-text="Yönetilen uygulama kaynak grupları":::

### <a name="nva-infrastructure-units"></a><a name="units"></a>NVA altyapı birimleri

Sanal WAN hub 'ında bir NVA oluşturduğunuzda, dağıtmak istediğiniz NVA altyapı birimi sayısını seçmeniz gerekir. Bir **NVA altyapı birimi** , sanal WAN hub 'ındaki bir NVA için toplam bant genişliği kapasitesinin bir birimidir. Bir **NVA altyapı birimi** , kapasite ve boyutlandırma hakkında düşündüğmeniz AÇıSıNDAN bir VPN [ölçek birimi](pricing-concepts.md#scale-unit) ile benzerdir.

* 1 NVA altyapısı birimi, bu NVA 'ya gelen tüm dal site bağlantıları için 500 MB/sn 'lik toplam bant genişliğini temsil eder.
* Azure, belirli bir NVA sanal hub dağıtımı için 1-80 NVA altyapı biriminden destekler.
* Her iş ortağı desteklenen tüm NVA altyapı birimi yapılandırmalarının alt kümesi olan farklı NVA altyapı birimi paketleri sunabilir.

VPN ölçek birimlerine benzer şekilde, *1 NVA altyapı birimi = 500 MB/sn*seçerseniz, her biri en fazla 500 Mbps hızında olacak şekilde, artıklık için iki örnek oluşturulur. Örneğin, her biri dalda 10 Mbps olan beş dala sahipseniz baş uçta 50 Mbps bir toplama işlemi yapmanız gerekir. NVA 'nın toplam kapasitesini planlama, hub 'a dal sayısını desteklemek için gereken kapasite değerlendirdikten sonra yapılmalıdır.

## <a name="network-virtual-appliance-configuration-process"></a><a name="configuration"></a>Ağ sanal gereç yapılandırma işlemi

İş ortakları, dağıtım sürecinin bir parçası olarak NVA 'yı otomatik olarak yapılandıran bir deneyim sağlamaya çalıştı. NVA sanal hub 'a sağlandıktan sonra, NVA için gerekli olabilecek ek yapılandırma NVA iş ortakları portalı veya yönetim uygulaması aracılığıyla yapılmalıdır. NVA 'ya doğrudan erişim kullanılamıyor.

## <a name="site-and-connection-resources-with-nvas"></a><a name="resources"></a>NVA 'lar ile site ve bağlantı kaynakları

Azure VPN Gateway yapılandırmalarının aksine, şube sitelerinizi sanal WAN hub 'ında NVA 'ınıza bağlamak için site kaynakları, **siteden siteye bağlantı** kaynakları veya **Noktadan siteye bağlantı** **kaynakları oluşturmanız gerekmez** . Bu, NVA iş ortağı aracılığıyla yönetilir.

Sanal WAN hub 'ınızı Azure sanal ağlarınıza bağlamak için yine de hub-VNet bağlantıları oluşturmanız gerekir.

## <a name="supported-regions"></a><a name="regions"></a>Desteklenen bölgeler

Sanal hub 'daki NVA aşağıdaki bölgelerde önizleme için kullanılabilir:

|Coğrafi bölge | Azure bölgeleri|
|---|---|
| Kuzey Amerika| Batı ABD, Orta Güney ABD, Doğu ABD 2   |
| Güney Amerika | Brezilya Güney |
| Avrupa | Batı Avrupa, UK Güney|
|  Orta Doğu | BAE Kuzey |
| Asya | Doğu Japonya |
| Avustralya | Doğu Avustralya |

## <a name="faq"></a>SSS

### <a name="i-am-a-network-appliance-partner-and-want-to-get-our-nva-in-the-hub--can-i-join-this-partner-program"></a>Bir ağ gereci iş ortalıyorum ve merkezinde NVA 'yı almak istiyorum.  Bu iş ortağı programına katılabilir miyim?

Ne yazık ki, şu anda yeni iş ortağı tekliflerini şirket içi olarak kapayoruz. Kasım ayında bizimle tekrar göz atın!

### <a name="can-i-deploy-any-nva-from-azure-marketplace-into-the-virtual-wan-hub"></a>Azure Marketi 'nden bir NVA 'yı sanal WAN hub 'ına dağıtabilir miyim?

Hayır. Şu anda, sanal WAN hub 'ına yalnızca [Barraycuda CloudGen WAN](https://aka.ms/BarracudaMarketPlaceOffer) dağıtılabilir.

### <a name="what-is-the-cost-of-the-nva"></a>NVA 'nın maliyeti nedir?

Barbcuda CloudGen WAN NVA için Barrampada bir lisans satın almanız gerekir. Lisanslama hakkında daha fazla bilgi için bkz. [Barbcuda 'ın CloudGen WAN sayfası](https://www.barracuda.com/products/cloudgenwan). Ayrıca, kullandığınız NVA altyapı birimleri ve kullandığınız diğer kaynaklar için de Microsoft 'un ücretlerine tabi olursunuz. Daha fazla bilgi için bkz. [fiyatlandırma kavramları](pricing-concepts.md).

### <a name="can-i-deploy-an-nva-to-a-basic-hub"></a>Bir NVA 'yı temel bir hub 'a dağıtabilir miyim?

Hayır. Bir NVA dağıtmak istiyorsanız, standart bir hub kullanmanız gerekir.

### <a name="can-i-deploy-an-nva-into-a-secure-hub"></a>Bir NVA 'yı güvenli bir hub 'a dağıtabilir miyim?

Evet. Barbcuda CloudGen WAN, Azure Güvenlik Duvarı ile bir hub 'a dağıtılabilir.

### <a name="can-i-connect-any-cpe-device-in-my-branch-office-to-barracuda-cloudgen-wan-nva-in-the-hub"></a>Şube ofisindeki herhangi bir CPE cihazını hub 'daki Barnetcuda CloudGen WAN NVA 'ya bağlayabilir miyim?

Hayır. Barbcuda CloudGen WAN yalnızca Barbcuda CPE cihazlarıyla uyumludur. CloudGen WAN gereksinimleri hakkında daha fazla bilgi edinmek için bkz. [Barbcuda 'ın CloudGen WAN sayfası](https://www.barracuda.com/products/cloudgenwan).

### <a name="what-routing-scenarios-are-supported-with-nva-in-the-hub"></a>Hub 'da NVA ile hangi yönlendirme senaryoları desteklenir?

Sanal WAN tarafından desteklenen tüm yönlendirme senaryoları, NVA 'lar ile birlikte desteklenir.

## <a name="next-steps"></a>Sonraki adımlar

Sanal WAN hakkında daha fazla bilgi edinmek için bkz. [sanal WAN genel bakış](virtual-wan-about.md) makalesi.