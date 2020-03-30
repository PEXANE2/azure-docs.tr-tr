---
title: VPN Gateway Classic Kaynak Yöneticisi Geçiş | Microsoft Dokümanlar
description: Bu sayfa, KAYNAK Yöneticisi geçişi için VPN Ağ Geçidi Klasik genel bir bakış sağlar.
documentationcenter: na
services: vpn-gateway
author: amsriva
manager: rossort
editor: amsriva
ms.assetid: caa8eb19-825a-4031-8b49-18fbf3ebc04e
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/06/2020
ms.author: amsriva
ms.openlocfilehash: c1a75630c6419816b048495ee87d24c81979af16
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77064776"
---
# <a name="vpn-gateway-classic-to-resource-manager-migration"></a>KAYNAK Yöneticisi geçişine VPN Ağ Geçidi klasiği
VPN Ağ Geçitleri artık klasik kaynak yöneticisi dağıtım modeline geçirilebilir. Azure Kaynak Yöneticisi özellikleri [ve avantajları](../azure-resource-manager/management/overview.md)hakkında daha fazla bilgi edinebilirsiniz. Bu makalede, klasik dağıtımlardan daha yeni Kaynak Yöneticisi tabanlı modele nasıl geçirilir ayrıntılarıyla anlatılmaktadır. 

VPN Ağ Geçitleri, Klasik'ten Kaynak Yöneticisi'ne VNet geçişinin bir parçası olarak geçirilir. Bu geçiş bir seferde bir VNet yapılır. Geçiş için araçlar veya ön koşullar açısından ek bir gereklilik yoktur. Geçiş adımları varolan VNet geçişiyle aynıdır ve [IaaS kaynakları geçiş sayfasında](../virtual-machines/windows/migration-classic-resource-manager-ps.md)belgelenmiştir. Geçiş sırasında veri yolu kapalı kalma süresi yoktur ve bu nedenle varolan iş yükleri geçiş sırasında şirket içi bağlantı kaybı olmadan çalışmaya devam eder. GEÇIŞ işlemi sırasında VPN ağ geçidiyle ilişkili genel IP adresi değişmez. Bu, geçiş tamamlandıktan sonra şirket içi yönlendiricinizi yeniden yapılandırmanız gerekmeyeceğini zedebeder.  

Kaynak Yöneticisi'ndeki model klasik modelden farklıdır ve sanal ağ ağ geçitleri, yerel ağ ağ geçitleri ve bağlantı kaynaklarından oluşur. Bunlar VPN ağ geçidinin kendisini temsil eder, yerel site nin yerinde adres alanı ve ikisi arasındaki bağlantı sırasıyla temsil eder. Geçiş tamamlandıktan sonra ağ geçitleriniz klasik modelde kullanılamaz ve sanal ağ ağ ağ geçitleri, yerel ağ ağ geçitleri ve bağlantı nesneleri üzerindeki tüm yönetim işlemleri Kaynak Yöneticisi modeli kullanılarak gerçekleştirilmelidir.

## <a name="supported-scenarios"></a>Desteklenen senaryolar
En yaygın VPN bağlantı senaryoları kaynak yöneticisi geçiş klasik tarafından karşılanır. Desteklenen senaryolar şunlardır -

* Nokta dan siteye bağlantı
* Şirket içinde konuma bağlı VPN Ağ Geçidi ile siteden siteye bağlantı
* VPN ağ geçitlerini kullanan iki VNet arasındaki VNet-VNet bağlantısı
* Aynı şirket konumuna bağlı birden fazla VNets
* Çok siteli bağlantı
* Zorunlu tünelleme etkin VNets

Desteklenmeyen senaryolar şunlardır:  

* Hem ExpressRoute Ağ Geçidi hem de VPN Ağ Geçidi'ne sahip VNet şu anda desteklenmez.
* VM uzantılarının şirket içi sunuculara bağlı olduğu aktarım senaryoları. Transit VPN bağlantı sınırlamaları aşağıda ayrıntılı olarak açıklanmıştır.

> [!NOTE]
> Kaynak Yöneticisi modelindeki CIDR doğrulaması klasik modeldekinden daha katıdır. Geçişten önce, verilen klasik adres aralıklarının geçişe başlamadan önce geçerli CIDR biçimine uygun olduğundan emin olun. CIDR herhangi bir ortak CIDR doğrulayıcıları kullanılarak doğrulanabilir. VNet veya geçersiz CIDR aralıkları ile yerel siteler geçirildiğinde başarısız duruma neden olur.
> 
> 

## <a name="vnet-to-vnet-connectivity-migration"></a>VNet-VNet bağlantı geçişi
VNet vnet vnet klasik bağlı VNet yerel bir site gösterimi oluşturarak elde edildi. Müşterilerin birbirine bağlanması gereken iki VNet'i temsil eden iki yerel site oluşturması gerekiyordu. Bunlar daha sonra iki VNets arasında bağlantı kurmak için IPsec tünel kullanarak ilgili VNets bağlı. Bir VNet'teki herhangi bir adres aralığı değişikliğinin ilgili yerel site gösteriminde de korunması gerektiğinden, bu modelin yönetilebilirlik zorlukları vardır. Kaynak Yöneticisi modelinde bu geçici çözüme artık gerek yoktur. İki VNet arasındaki bağlantı, Bağlantı kaynağındaki 'Vnet2Vnet' bağlantı türü kullanılarak doğrudan elde edilebilir. 

![VNet'in VNet'e geçiş ekranı.](./media/vpn-gateway-migration/migration1.png)

VNet geçişi sırasında, geçerli VNet'in VPN ağ geçidine bağlı varlığın başka bir VNet olduğunu algılarız ve her iki VNet'in geçişi tamamlandıktan sonra diğer VNet'i temsil eden iki yerel siteyi artık göremeyeceğinizden emin oluruz. İki VPN ağ geçidi, iki yerel site ve aralarındaki iki bağlantının klasik modeli, iki VPN ağ geçidi ve vnet2Vnet tipi iki bağlantıile Resource Manager modeline dönüştürülür.

## <a name="transit-vpn-connectivity"></a>Transit VPN bağlantısı
VPN ağ geçitlerini, bir VNet için şirket içi bağlantının şirket içi doğrudan şirket içi bağlantıya bağlanarak elde edilebildiği bir topolojide yapılandırabilirsiniz. Bu, ilk VNet'teki örneklerin şirket içi kaynaklara doğrudan şirket içi bağlı bağlı VNet'teki VPN ağ geçidine geçiş yoluyla şirket içi kaynaklara bağlı olduğu transit VPN bağlantısıdır. Bu yapılandırmayı klasik dağıtım modelinde gerçekleştirmek için, hem bağlı VNet'i hem de şirket içi adres alanını temsil eden önekleri toplayan yerel bir site oluşturmanız gerekir. Bu temsili yerel site daha sonra transit bağlantı sağlamak için VNet'e bağlanır. Bu klasik model, şirket içi adres aralığındaki herhangi bir değişikliğin VNet'in toplamını temsil eden yerel sitede ve şirket içinde de sürdürülmesi gerektiğinden, benzer yönetilebilirlik zorluklarına da sahiptir. Bağlı ağ geçitleri öneklerde manuel değişiklik yapılmadan güzergahları şirket içinde öğrenebildiği için Kaynak Yöneticisi destekli ağ geçitlerinde BGP desteğinin başlatılması yönetilebilirliği kolaylaştırır.

![Geçiş yönlendirme senaryosunun ekran görüntüsü.](./media/vpn-gateway-migration/migration2.png)

VNet'i yerel sitelere gerek kalmadan VNet bağlantısına dönüştürdüğünüziçin, geçiş senaryosu dolaylı olarak şirket içi bağlantıya bağlı olan VNet'in şirket içi bağlantısını kaybeder. Bağlantı kaybı, geçiş tamamlandıktan sonra aşağıdaki iki şekilde azaltılabilir - 

* BGP'yi birbirine bağlı VPN ağ geçitlerinde ve şirket içinde etkinleştirin. VNet ağ geçitleri arasında rotalar öğrenildive reklamı olduğundan BGP'yi etkinleştirmek başka bir yapılandırma değişikliği olmadan bağlantı geri yüklenir. BGP seçeneğinin yalnızca Standart ve daha yüksek SPU'larda kullanılabildiğini unutmayın.
* Etkilenen VNet'ten şirket içi konumu temsil eden yerel ağ ağ geçidine açık bir bağlantı kurun. Bu, IPsec tünelini oluşturmak ve yapılandırmak için şirket içi yönlendiricide yapılandırmayı değiştirmeyi de gerektirir.

## <a name="next-steps"></a>Sonraki adımlar
VPN ağ geçidi geçiş desteği hakkında bilgi edindikten sonra, başlamak için [klasikten Kaynak Yöneticisi'ne IaaS kaynaklarının platform destekli geçişine](../virtual-machines/windows/migration-classic-resource-manager-ps.md) gidin.

