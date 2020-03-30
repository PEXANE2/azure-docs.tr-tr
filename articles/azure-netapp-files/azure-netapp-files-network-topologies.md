---
title: Azure NetApp Dosyaları ağ planlaması için yönergeler | Microsoft Dokümanlar
description: Azure NetApp Dosyalarını kullanarak etkili bir ağ mimarisi tasarlamanıza yardımcı olabilecek yönergeleri açıklar.
services: azure-netapp-files
documentationcenter: ''
author: ram-kakani
manager: ''
editor: ''
ms.assetid: ''
ms.service: azure-netapp-files
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 05/08/2019
ms.author: b-juche
ms.openlocfilehash: 8e6a1c3472c6b20b27cf181edbeeb96ab71eb58d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "73242486"
---
# <a name="guidelines-for-azure-netapp-files-network-planning"></a>Azure NetApp Files ağ planlaması yönergeleri

Ağ mimarisi planlaması, herhangi bir uygulama altyapısı tasarlamanın önemli bir unsurudur. Bu makale, Azure NetApp Dosyaları'nın zengin özelliklerinden yararlanmak için iş yükleriniz için etkili bir ağ mimarisi tasarlamanıza yardımcı olur.

Azure NetApp Dosyaları birimleri, Azure Sanal Ağınızda temsilci alt ağı adı verilen özel amaçlı bir [alt ağda](https://docs.microsoft.com/azure/virtual-network/virtual-network-manage-subnet) yer almak üzere tasarlanmıştır. Bu nedenle, birimlere doğrudan VNet'inizden, aynı bölgedeki baktım VNet'lerden veya gerektiğinde sanal ağ ağ geçidi (ExpressRoute veya VPN Ağ Geçidi) üzerinden şirket içinde erişebilirsiniz. Alt ağ Azure NetApp Dosyaları'na adanmıştır ve diğer Azure hizmetlerine veya Internet'e bağlantı yoktur.

## <a name="considerations"></a>Dikkat edilmesi gerekenler  

Azure NetApp Files ağı için plan yaparken dikkat edilmesi gereken birkaç hususu anlamalısınız.

### <a name="constraints"></a>Kısıtlamalar

Aşağıdaki özellikler şu anda Azure NetApp Dosyaları için desteklenmez: 

* Devredilen alt ağa uygulanan ağ güvenlik grupları (NSGs)
* Azure NetApp dosyaları alt ağı olarak adres önekine sahip kullanıcı tanımlı rotalar (ÜDR)
* Azure NetApp Dosyaları arabirimindeki Azure ilkeleri (örneğin, özel adlandırma ilkeleri)
* Azure NetApp Dosyaları trafiği için yük bakiyeleri

Aşağıdaki ağ kısıtlamaları Azure NetApp Dosyaları için geçerlidir:

* Azure NetApp Files'a sahip bir VNet'te (eşlenen VNet'ler dahil) kullanılan IP sayısı 1000'i geçemez. Müşteri ölçeği taleplerini karşılamak için bu limiti artırmak için çalışıyoruz. Bu arada, daha fazla IP'ye ihtiyacınız varsa, kullanım durumunuz ve gerekli limitinizle destek ekibimize ulaşın.
* Her Azure Sanal Ağında (VNet), Azure NetApp Dosyalarına yalnızca bir alt ağ devredilebilir.


### <a name="supported-network-topologies"></a>Desteklenen ağ topolojileri

Aşağıdaki tabloda Azure NetApp Files tarafından desteklenen ağ topolojileri açıklanmaktadır.  Ayrıca desteklenmeyen topolojiler için geçici çözüm açıklar. 

|    Topolojiler    |    Desteklenir    |     Geçici çözüm    |
|-------------------------------------------------------------------------------------------------------------------------------|--------------------|-----------------------------------------------------------------------------|
|    Yerel bir VNet'te sesbirimine bağlantı    |    Evet    |         |
|    Eşlenen bir VNet'teki ses birimine bağlantı (Aynı bölge)    |    Evet    |         |
|    Bir eşlenen VNet'te sese bağlantı (Çapraz bölge veya küresel bakış)    |    Hayır    |    None    |
|    ExpressRoute ağ geçidi üzerinden bir birime bağlantı    |    Evet    |         |
|    ExpressRoute ağ geçidi ve ağ geçidi geçişiyle birlikte VNet üzerinden konuşan vnet'te şirket içi bir birime bağlantı    |    Evet    |        |
|    VPN ağ geçidi üzerinden konuşan vnet'te şirket içi bir birime bağlantı    |    Evet    |         |
|    VPN ağ geçidi üzerinden bir kollu VNet'te şirket içi bir birime bağlantı ve ağ geçidi geçişi ile VNet'e bakma    |    Evet    |         |


## <a name="virtual-network-for-azure-netapp-files-volumes"></a>Azure NetApp Dosyaları birimleri için sanal ağ

Bu bölümde, sanal ağ planlama konusunda size yardımcı olan kavramlar açıklanmaktadır.

### <a name="azure-virtual-networks"></a>Azure sanal ağları

Azure NetApp Dosyaları birimini sağlamadan önce bir Azure sanal ağı (VNet) oluşturmanız veya aboneliğinizde zaten var olan bir ağ kullanmanız gerekir. VNet, birimin ağ sınırını tanımlar.  Sanal ağlar oluşturma hakkında daha fazla bilgi için [Azure Sanal Ağ belgelerine](https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview)bakın.

### <a name="subnets"></a>Alt ağlar

Alt ağlar, sanal ağı, içinde bulunan Azure kaynakları tarafından kullanılabilir ayrı adres boşluklarına böler.  Azure NetApp Dosyaları [birimleri, temsilci alt ağı](https://docs.microsoft.com/azure/virtual-network/virtual-network-manage-subnet)olarak adlandırılan özel amaçlı bir alt ağda bulunur. 

Alt ağ delegasyonu, alt ağda hizmete özel kaynaklar oluşturmak için Azure NetApp Files hizmetine açık izinler verir.  Hizmeti dağıtılırken benzersiz bir tanımlayıcı kullanır. Bu durumda, Azure NetApp Dosyalarına bağlantı sağlamak için bir ağ arabirimi oluşturulur.

Yeni bir VNet kullanıyorsanız, bir alt net'i [Azure NetApp Dosyalarına Devretme](azure-netapp-files-delegate-subnet.md)yönergelerini izleyerek bir alt ağ oluşturabilir ve alt ağı Azure NetApp Dosyalarına devredebilirsiniz. Ayrıca, zaten diğer hizmetlere devredilmedi varolan boş bir alt net idamı da atayabilirsiniz.

VNet başka bir VNet ile eşliyse, VNet adres alanını genişletemezsiniz. Bu nedenle, vnet adres alanı içinde yeni temsilci alt net oluşturulması gerekir. Adres alanını genişletmeniz gerekiyorsa, adres alanını genişletmeden önce VNet eşlemasını silmeniz gerekir.

### <a name="udrs-and-nsgs"></a>ÜD'ler ve NSG'ler

Kullanıcı tanımlı rotalar (ÜDRs) ve Ağ güvenlik grupları (NSG'ler) Azure NetApp Dosyaları için devredilen alt ağlarda desteklenmez.

Geçici çözüm olarak, Azure NetApp Dosyaları'na ve tarafından devredilen alt ağa gelen trafiğe izin veren veya reddeden diğer alt ağlara NSG uygulayabilirsiniz.  

## <a name="azure-native-environments"></a>Azure yerel ortamlar

Aşağıdaki diyagram, Azure'un yerel ortamını göstermektedir:

![Azure'a özgü ağ ortamı](../media/azure-netapp-files/azure-netapp-files-network-azure-native-environment.png)

### <a name="local-vnet"></a>Yerel VNet

Temel senaryo, aynı VNet'teki sanal bir makineden (VM) azure netapp files birimi oluşturmak veya bu dosyalara bağlanmaktır. Yukarıdaki diyagramdaki VNet 2 için, Birim 1 devralınan bir alt ağda oluşturulur ve varsayılan alt ağda VM 1'e monte edilebilir.

### <a name="vnet-peering"></a>VNet eşlemesi

Aynı bölgede birbirinin kaynaklarına erişilmesi gereken ek VNet'leriniz varsa, Azure altyapısı üzerinden güvenli bağlantı sağlamak için [VNet eşlemesini](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview) kullanarak VNet'ler bağlanabilir. 

Yukarıdaki diyagramda VNet 2 ve VNet 3'ü göz önünde bulundurun. VM 1'in VM 2 veya Volume 2'ye bağlanması gerekiyorsa veya VM 2'nin VM 1 veya Volume 1'e bağlanması gerekiyorsa, VNet'in VNet 2 ile VNet 3 arasında eşlemesini etkinleştirmeniz gerekir. 

Ayrıca, VNet 1'in VNet 2 ile ve VNet 2'nin aynı bölgede VNet 3 ile görüntülendiği bir senaryo düşünün. VNet 1'in kaynakları VNet 2'deki kaynaklara bağlanabilir, ancak VNet 1 ve VNet 3'e bakılmadığı sürece VNet 3'teki kaynaklara bağlanamaz. 

Yukarıdaki diyagramda, VM 3 Cilt 1'e bağlanabilse de, VM 4 Cilt 2'ye bağlanamaz.  Bunun nedeni, konuşan VNets'in bakılmamasın ve _transit yönlendirmenin VNet'e bakma üzerinde desteklenmemesidir._

## <a name="hybrid-environments"></a>Karma ortamlar

Aşağıdaki diyagram karma ortamı göstermektedir: 

![Karma ağ ortamı](../media/azure-netapp-files/azure-netapp-files-network-hybrid-environment.png)

Karma senaryoda, şirket içi veri merkezlerinden gelen uygulamaların Azure'daki kaynaklara erişmeleri gerekir.  Veri merkezinizi Azure'a genişletmek mi, ister Azure yerel hizmetlerini kullanmak ister olağanüstü durum kurtarma için mi istediğiniz iveci dir. Birden çok kaynağın siteden siteye VPN veya ExpressRoute aracılığıyla Azure'daki kaynaklara şirket içinde nasıl bağlanılacak hakkında bilgi almak için [VPN Ağ Geçidi planlama seçeneklerine](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways?toc=%2fazure%2fvirtual-network%2ftoc.json#planningtable) bakın.

Azure hub VNet, karma hub konuşan bir topolojide şirket içi ağınıza merkezi bir bağlantı noktası görevi görür. Sözcüler hub ile bakan VNets vardır ve iş yüklerini yalıtmak için kullanılabilir.

Yapılandırmaya bağlı olarak, şirket içi kaynakları hub'daki ve sözcüdeki kaynaklara bağlayabilirsiniz.

Yukarıda gösterildiği topolojide, şirket içi ağ Azure'daki bir hub VNet'e bağlıdır ve aynı bölgede VNet hub'ına bakan 2 kollu VNet vardır.  Bu senaryoda, Azure NetApp Files birimleri için desteklenen bağlantı seçenekleri aşağıdaki gibidir:

* Şirket içi kaynaklar VM 1 ve VM 2, siteden siteye VPN veya ExpressRoute devresi üzerinden hub'daki Cilt 1'e bağlanabilir. 
* Şirket içi kaynaklar VM 1 ve VM 2, siteden siteye VPN ve bölgesel Vnet eşlemeleri üzerinden Cilt 2 veya Cilt 3'e bağlanabilir.
* VNet hub'ındaki VM 3, kollu VNet 1'de Volume 2'ye ve vnet 2'de Hacim 3'e bağlanabilir.
* VM 4 spoke VNet 1 ve VM 5 spoke VNet 2 hub VNet Hacim 1 bağlanabilir.

VM 4, spoke VNet 1'de VNet 2'de Cilt 3'e bağlanamaz. Ayrıca, VM 5 spoke VNet2'de VNet 1'de Cilt 2'ye bağlanamaz. Bu durum, sözcü VNet'lerin bakılmadığını ve _transit yönlendirmenin VNet'e bakma üzerinde desteklenmemesi_nedeniyle durum böyledir.

## <a name="next-steps"></a>Sonraki adımlar

[Azure NetApp Files için bir alt ağı temsilci olarak belirleme](azure-netapp-files-delegate-subnet.md)
