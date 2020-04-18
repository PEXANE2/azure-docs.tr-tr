---
title: Azure'da SAP HANA altyapı yapılandırmaları ve işlemleri | Microsoft Dokümanlar
description: Azure sanal makinelerde dağıtılan SAP HANA sistemleri için işlem kılavuzu.
services: virtual-machines-linux,virtual-machines-windows
documentationcenter: ''
author: msjuergent
manager: bburns
editor: ''
tags: azure-resource-manager
keywords: ''
ms.service: virtual-machines-linux
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 10/01/2019
ms.author: juergent
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 7c4f3ec2727d06528eab788a2a24a6190fe26533
ms.sourcegitcommit: 5e49f45571aeb1232a3e0bd44725cc17c06d1452
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/17/2020
ms.locfileid: "81606137"
---
# <a name="sap-hana-infrastructure-configurations-and-operations-on-azure"></a>Azure'da SAP HANA altyapı yapılandırmaları ve işlemleri
Bu belge, Azure altyapısının yapılandırılması ve Azure yerel sanal makinelerde (VM) dağıtılan SAP HANA sistemlerinin işletimi için kılavuz sağlar. Belge ayrıca M128s VM SKU için SAP HANA ölçeklendirmeiçin yapılandırma bilgilerini de içerir. Bu belge, aşağıdaki içeriği içeren standart SAP belgelerinin yerine geçmeamacını almıştır:

- [SAP yönetim kılavuzu](https://help.sap.com/viewer/6b94445c94ae495c83a19646e7c3fd56/2.0.02/330e5550b09d4f0f8b6cceb14a64cd22.html)
- [SAP kurulum kılavuzları](https://service.sap.com/instguides)
- [SAP notları](https://service.sap.com/notes)

## <a name="prerequisites"></a>Ön koşullar
Bu kılavuzu kullanmak için aşağıdaki Azure bileşenleri hakkında temel bilgiye ihtiyacınız vardır:

- [Azure sanal makineleri](https://docs.microsoft.com/azure/virtual-machines/linux/tutorial-manage-vm)
- [Azure ağ ve sanal ağlar](https://docs.microsoft.com/azure/virtual-machines/linux/tutorial-virtual-network)
- [Azure Depolama](https://docs.microsoft.com/azure/virtual-machines/linux/tutorial-manage-disks)

Azure'daki SAP NetWeaver ve diğer SAP bileşenleri hakkında daha fazla bilgi edinmek için [Azure belgelerinin](https://docs.microsoft.com/azure/) [Azure'daki SAP](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/get-started) bölümüne bakın.

## <a name="basic-setup-considerations"></a>Temel kurulum konuları
Aşağıdaki bölümlerde, SAP HANA sistemlerini Azure VM'lerinde dağıtmak için temel kurulum konuları açıklanmaktadır.

### <a name="connect-into-azure-virtual-machines"></a>Azure sanal makinelerine bağlanın
[Azure sanal makineler planlama kılavuzunda](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/planning-guide)belgelendirildikçe, Azure SANAL MM'lere bağlanmak için iki temel yöntem vardır:

- Jump VM'de veya SAP HANA çalıştıran VM'de internet ve ortak uç noktalar üzerinden bağlanın.
- [VPN](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal) veya Azure [ExpressRoute](https://azure.microsoft.com/services/expressroute/)üzerinden bağlanın.

Üretim senaryoları için VPN veya ExpressRoute üzerinden siteden siteye bağlantı gereklidir. Sap yazılımının kullanıldığı üretim senaryolarını besleyen üretim dışı senaryolar için de bu tür bir bağlantı gereklidir. Aşağıdaki resim, bir site arası bağlantı örneğini gösterir:

![Site içi bağlantı](media/virtual-machines-shared-sap-planning-guide/300-vpn-s2s.png)


### <a name="choose-azure-vm-types"></a>Azure VM türlerini seçin
Üretim senaryoları için kullanılabilecek Azure VM türleri [IAAS için SAP belgelerinde](https://www.sap.com/dmc/exp/2014-09-02-hana-hardware/enEN/iaas.html)listelenir. Üretim dışı senaryolar için daha çeşitli yerel Azure VM türleri kullanılabilir.

>[!NOTE]
> Üretim dışı senaryolar [için, SAP not#1928533'nde](https://launchpad.support.sap.com/#/notes/1928533)listelenen VM türlerini kullanın. Üretim senaryoları için Azure VM'lerinin kullanımı için SAP HANA sertifikalı VM'leri SAP tarafından yayınlanan [Sertifikalı IaaS Platformları listesinde](https://www.sap.com/dmc/exp/2014-09-02-hana-hardware/enEN/iaas.html#categories=Microsoft%20Azure)denetleyin.

Aşağıdakileri kullanarak Azure'da VM'leri dağıtın:

- Azure portalı.
- Azure PowerShell cmdlets.
- The Azure CLI.

Ayrıca, AZURE VM hizmetlerinde tam yüklü bir SAP HANA [platformını SAP Bulut platformu](https://cal.sap.com/)üzerinden dağıtabilirsiniz. Yükleme işlemi, [Azure'da SAP S/4HANA veya BW/4HANA'da](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/cal-s4h) veya [burada](https://github.com/AzureCAT-GSI/SAP-HANA-ARM)yayımlanan otomasyonla açıklanmıştır.

>[!IMPORTANT]
> M208xx_v2 VM'leri kullanmak için Azure VM resim galerisinden Linux resminizi seçerken dikkatli olmanız gerekir. Ayrıntıları okumak için, [makaleBellek optimize sanal makine boyutları](../../mv2-series.md)okuyun.
> 


### <a name="storage-configuration-for-sap-hana"></a>SAP HANA için depolama yapılandırması
Azure'da SAP HANA ile kullanılacak depolama yapılandırmaları ve depolama türleri için [SAP HANA Azure sanal makine depolama yapılandırmaları](./hana-vm-operations-storage.md) belgesini okuyun


### <a name="set-up-azure-virtual-networks"></a>Azure sanal ağlarını ayarlama
VPN veya ExpressRoute üzerinden Azure'a siteden siteye bağlantınız olduğunda, SANAL Ağ Geçidi üzerinden VPN veya ExpressRoute devresine bağlanan en az bir Azure sanal ağınız olmalıdır. Basit dağıtımlarda Sanal Ağ Geçidi, SAP HANA örneklerini de barındıran Azure sanal ağının (VNet) bir alt ağında dağıtılabilir. SAP HANA'yı yüklemek için Azure sanal ağında iki ek alt ağ oluşturursunuz. Bir alt ağ, SAP HANA örneklerini çalıştırmak için VM'leri barındırıyor. Diğer alt ağ, SAP HANA Studio'yu, diğer yönetim yazılımLarını veya uygulama yazılımınızı barındırmak için Jumpbox veya Management VM'leri çalıştırAr.

> [!IMPORTANT]
> İşlevsellik dışında, ancak performans nedenleriyle daha da önemlisi, [Azure Ağı Sanal Cihazları'nı](https://azure.microsoft.com/solutions/network-appliances/) SAP uygulaması ile SAP NetWeaver, Hybris veya S/4HANA tabanlı SAP sisteminin DBMS katmanı arasındaki iletişim yolunda yapılandırmak için desteklenmez. SAP uygulama katmanı ile DBMS katmanı arasındaki iletişimin doğrudan olması gerekir. Bu kısıtlama, [ASG ve NSG](https://docs.microsoft.com/azure/virtual-network/security-overview) kuralları doğrudan iletişime izin veremediği sürece Azure ASG ve NSG kurallarını içermez. NV'lerin desteklenmediği diğer senaryolar, [SUSE Linux Enterprise Server'daki SUSE Linux Enterprise Server'da SAP uygulamalarında SAP NetWeaver için yüksek kullanılabilirlikte](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse)açıklandığı şekilde Linux Pacemaker küme düğümlerini temsil eden Azure VM'leri ile SBD aygıtları arasındaki iletişim yollarındadır. Veya Azure VM'leri ile Windows Server SOFS arasındaki iletişim yollarında, [Azure'da bir dosya paylaşımı kullanarak Windows failover kümesinde bir SAP ASCS/SCS örneğinde](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-high-availability-guide-wsfc-file-share)açıklandığı şekilde ayarlanır. İletişim yollarında nvas kolayca iki iletişim ortakları arasında ağ gecikme iki katına, SAP uygulama katmanı ve DBMS katmanı arasındaki kritik yollarda iş geleni kısıtlayabilir. Müşterilerle gözlenen bazı senaryolarda, NV'ler Pacemaker Linux kümelerinin, Linux Pacemaker küme düğümleri arasındaki iletişimin bir NVA üzerinden SBD aygıtlarıyla iletişim kurması gerektiğinde başarısız lığa neden olabilir.  
> 

> [!IMPORTANT]
> **DESTEKLENMEYEN** bir diğer tasarım da SAP uygulama katmanının ve DBMS katmanının birbirinden [farklı](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview) Azure sanal ağlarına ayrılmasıdır. Sap uygulama katmanını ve DBMS katmanını, farklı Azure sanal ağları kullanmak yerine, bir Azure sanal ağındaki alt ağları kullanarak ayırmanız önerilir. Öneriye uymamaya karar verirseniz ve bunun yerine iki katmanı farklı sanal ağa ayırırsanız, iki sanal ağın [bakılması](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview)gerekir. [İki bakan](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview) Azure sanal ağı arasındaki ağ trafiğinin aktarım maliyetlerine tabi olduğunu unutmayın. SAP uygulama katmanı ile DBMS katmanı arasında değiş tokuş edilen birçok Terabayttaki büyük veri hacmiyle, SAP uygulama katmanı ve DBMS katmanı iki eşlenen Azure sanal ağı arasında ayrılmışsa önemli maliyetler birikebilir. 

SAP HANA'yı çalıştırmak için VM'leri yüklediğinizde, VM'lerin aşağıdakilere ihtiyacı vardır:

- İki sanal NIC yüklü: bir NIC yönetim alt ağına bağlanmak için ve bir NIC şirket içi ağdan veya diğer ağlardan Azure VM'deki SAP HANA örneğine bağlanmak için.
- Her iki sanal NIC için dağıtılan statik özel IP adresleri.

> [!NOTE]
> Statik IP adreslerini Azure araçları yla tek tek vNIC'lere atamalısınız. Konuk işletim sistemi içindeki statik IP adreslerini bir vNIC'e atamamalısınız. Azure Yedekleme Hizmeti gibi bazı Azure hizmetleri, en azından birincil vNIC'nin statik IP adreslerine göre değil, DHCP olarak ayarlanmış olmasına dayanır. Ayrıca bkz. belge [Sorun Giderme Azure sanal makine](https://docs.microsoft.com/azure/backup/backup-azure-vms-troubleshoot#networking)yedeklemesi. Bir VM'ye birden çok statik IP adresi atamanız gerekiyorsa, vm'ye birden çok vNI atamanız gerekir.
>
>

Ancak, kalıcı dağıtımlar için Azure'da sanal bir veri merkezi ağ mimarisi oluşturmanız gerekir. Bu mimari, şirket içi bağlanan Azure VNet Ağ Geçidi'nin şirket içinde ayrı bir Azure VNet'e ayrılmasını önerir. Bu ayrı VNet, şirket içinde veya internete giden tüm trafiği barındırmalıdır. Bu yaklaşım, bu ayrı hub VNet'te Azure'daki sanal veri merkezine giren denetim ve günlük trafiği için yazılım dağıtmanıza olanak tanır. Böylece, Azure dağıtımınıza gelen ve giden trafikle ilgili tüm yazılımları ve yapılandırmaları barındıran bir VNet'iniz vardır.

Azure [Sanal Veri Merkezi: Ağ Perspektifi](https://docs.microsoft.com/azure/architecture/vdc/networking-virtual-datacenter) ve [Azure Sanal Veri Merkezi ve Kurumsal Denetim Düzlemi](https://docs.microsoft.com/azure/architecture/vdc/) makaleleri, sanal veri merkezi yaklaşımı ve ilgili Azure VNet tasarımı hakkında daha fazla bilgi verir.


>[!NOTE]
>Azure VNet [eşlemesini](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview) kullanarak hub VNet ile konuşan VNet arasında akan trafik ek [maliyetlere](https://azure.microsoft.com/pricing/details/virtual-network/)tabidir. Bu maliyetlere bağlı olarak, Katı bir hub ve konuşan ağ tasarımı nı çalıştırmakla VNet'i atlamak için 'sözcülere' bağlandığınız birden çok [Azure ExpressRoute Ağ Geçidi](https://docs.microsoft.com/azure/expressroute/expressroute-about-virtual-network-gateways) çalıştırmak arasında ödün vermeyi düşünebilirsiniz. Ancak Azure ExpressRoute Ağ Geçitleri de ek [maliyetler](https://azure.microsoft.com/pricing/details/vpn-gateway/) sunar. Ayrıca, ağ trafiği günlüğe kaydetme, denetleme ve izleme için kullandığınız üçüncü taraf yazılımlar için ek maliyetlerle de karşılaşabilirsiniz. Bir tarafta VNet eşleme yoluyla veri alışverişi maliyetlerine ve ek Azure ExpressRoute Ağ Geçitleri ve ek yazılım lisansları tarafından oluşturulan maliyetlere bağlı olarak, VNet yerine alt ağları yalıtım birimi olarak kullanarak bir VNet içinde mikro segmentasyona karar verebilirsiniz.


IP adresleri atamak için farklı yöntemlere genel bakış için [Azure'da IP adresi türleri ve ayırma yöntemlerine](https://docs.microsoft.com/azure/virtual-network/virtual-network-ip-addresses-overview-arm)bakın. 

SAP HANA çalıştıran VM'ler için atanan statik IP adresleriyle çalışmanız gerekir. Nedeni HANA başvuru IP adresleri için bazı yapılandırma öznitelikleri olmasıdır.

[Azure Ağ Güvenlik Grupları (NSG'ler),](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg) SAP HANA örneğine veya atlama kutusuna yönlendirilen trafiği yönlendirmek için kullanılır. NSG'ler ve sonunda [Uygulama Güvenlik Grupları](https://docs.microsoft.com/azure/virtual-network/security-overview#application-security-groups) SAP HANA alt ağı ve Yönetim alt ağıyla ilişkilidir.

Aşağıdaki resim, sap HANA için bir hub ve kollu VNet mimarisi aşağıdaki kaba bir dağıtım şeması genel bir bakış gösterir:

![SAP HANA için kaba dağıtım şeması](media/hana-vm-operations/hana-simple-networking-dmz.png)

SAP HANA'yı sitebağlantısı olmadan Azure'da dağıtmak için, SAP HANA örneğini genel internetten korumak ve ileri bir proxy'nin arkasına gizlemek istersiniz. Bu temel senaryoda, dağıtım ana bilgisayar adlarını çözmek için Azure yerleşik DNS hizmetlerine dayanır. Genel kullanıma açık IP adreslerinin kullanıldığı daha karmaşık bir dağıtımda, Azure yerleşik DNS hizmetleri özellikle önemlidir. Azure'da Azure VNet mimarinize yönlendirmeyi kontrol etmek, yönlendirmeyi izlemek için Azure NSG'leri ve [Azure NV'lerini](https://azure.microsoft.com/solutions/network-appliances/) kullanın. Aşağıdaki resim, sap HANA'yı bir hub'da siteden siteye bağlantısı olmadan dağıtmak için kaba bir şemayı gösterir ve VNet mimarisini seslendirmektedir:
  
![Sap HANA için siteden siteye bağlantısı olmayan kaba dağıtım şeması](media/hana-vm-operations/hana-simple-networking-dmz.png)
 

Hub ve kollu VNet mimarisi olmadan Internet'ten erişimi denetlemek ve izlemek için Azure NVAs'ların nasıl kullanılacağına ilişkin başka bir açıklama, [yüksek kullanılabilir ağ sanal cihazlarını dağıt'ta](https://docs.microsoft.com/azure/architecture/reference-architectures/dmz/nva-ha)bulunabilir.


## <a name="configuring-azure-infrastructure-for-sap-hana-scale-out"></a>SAP HANA ölçeği için Azure altyapıyapılandırma
OLAP ölçeklendirme veya S/4HANA ölçeklendirmeiçin onaylı Azure VM türlerini öğrenmek için [SAP HANA donanım dizinine](https://www.sap.com/dmc/exp/2014-09-02-hana-hardware/enEN/iaas.html#categories=Microsoft%20Azure)bakın. 'Kümeleme' sütunundaki bir onay işareti ölçeklendirme desteğini gösterir. Uygulama türü, OLAP ölçeklendirmesinin veya S/4HANA ölçeğinin desteklenip desteklenmediğini gösterir. VM'lerin her biri için ölçeklendirme yle onaylanan düğümlerle ilgili ayrıntılar için SAP HANA donanım dizininde listelenen belirli VM SKU'daki girişlerin ayrıntılarını kontrol edin.

Azure VM'lerde ölçeklendirme yapılandırmaları dağıtmak için en az işletim sistemi sürümleri, SAP HANA donanım dizininde listelenen belirli VM SKU'daki girişlerin ayrıntılarını denetleyin. N-düğüm OLAP ölçeklendirme yapılandırmasının bir düğümü ana düğüm olarak işlev görür. Sertifika sınırına kadar olan diğer düğümler işçi düğümü olarak hareket eder. Ek bekleme düğümleri sertifikalı düğüm sayısına dahil etmez

>[!NOTE]
> SAP HANA'nın bekleme düğümüyle Azure VM ölçeklendirmedağıtımları yalnızca [Azure NetApp Files](https://azure.microsoft.com/services/netapp/) depolama sını kullanarak mümkündür. Sap HANA sertifikalı başka hiçbir Azure depolama, SAP HANA bekleme düğümlerinin yapılandırmasına izin verir
>

/hana/shared için [Azure NetApp Dosyaları'nın](https://azure.microsoft.com/services/netapp/)kullanımını da öneririz. 

Ölçeklendirme yapılandırmasındaki tek bir düğüm için tipik bir temel tasarım şu şekilde görünecektir:

![Tek bir düğümün ölçeklendirme temelleri](media/hana-vm-operations/scale-out-basics-anf-shared.PNG)

SAP HANA ölçeklendirmesi için bir VM düğümünün temel yapılandırması aşağıdaki gibi görünür:

- **/hana/shared**için Azure NetApp Files aracılığıyla sağlanan yerel NFS hizmetini kullanırsınız. 
- Diğer tüm disk birimleri farklı düğümler arasında paylaşılmaz ve NFS'yi temel almaz. Paylaşılan **olmayan /hana/data** ve **/hana/log** ile ölçeklendirilen HANA yüklemeleri için kurulum yapılandırmaları ve adımları daha sonra bu belgede verilmiştir. Kullanılabilen HANA sertifikalı depolama için SAP [HANA Azure sanal makine depolama yapılandırmaları makalesini](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-vm-operations-storage)kontrol edin.


Birimleri veya diskleri boyutlandırma, işçi düğümleri sayısına bağlı olarak gerekli boyutu için [belge SAP HANA TDI Depolama Gereksinimleri,](https://www.sap.com/documents/2015/03/74cdb554-5a7c-0010-82c7-eda71af511fa.html)kontrol etmek gerekir. Belge, birimin gerekli kapasitesini elde etmek için uygulamanız gereken bir formül yayımlar

Bir ölçeklendirme SAP HANA VM için tek düğüm yapılandırmagrafiklerinde görüntülenen diğer tasarım ölçütleri VNet veya daha iyi alt net yapılandırmasıdır. SAP, hana düğümleri arasındaki iletişimden trafiğe bakan istemcinin/uygulamanın ayrılmasını şiddetle önerir. Grafiklerde gösterildiği gibi, bu hedefe VM'ye iki farklı vNI'nin eklenmesiyle ulaşılır. Her iki vNIC'in de farklı alt ağlarında olması, iki farklı IP adresi vardır. Ardından NSG'leri veya kullanıcı tanımlı yolları kullanarak yönlendirme kurallarıyla trafik akışını kontrol elabilirsiniz.

Özellikle Azure'da, belirli vNIC'lerde hizmet kalitesini ve kotaları uygulamak için hiçbir araç ve yöntem yoktur. Sonuç olarak, istemci/uygulamanın karşı karşıya olduğu ve düğüm içi iletişimin ayrılması, bir trafik akışını diğerine öncelik lendirmek için herhangi bir fırsat açmaz. Bunun yerine ayırma, ölçeklendirme yapılandırmalarının düğüm içi iletişimini korumada bir güvenlik önlemi olmaya devam eder.  

>[!NOTE]
>SAP, ağ trafiğini istemci/uygulama tarafına ve bu belgede açıklandığı şekilde düğüm içi trafiğe ayırmayı önerir. Bu nedenle son grafiklerde gösterildiği gibi yerine bir mimari koyarak önerilir. Ayrıca, öneriden sapan gereksinimler için güvenlik ve uyumluluk ekibinize başvurun 
>

Ağ açısından bakıldığında, gereken minimum ağ mimarisi aşağıdaki gibi görünür:

![Tek bir düğümün ölçeklendirme temelleri](media/hana-vm-operations/overview-scale-out-networking.png)



### <a name="installing-sap-hana-scale-out-n-azure"></a>SAP HANA scale-out n Azure'u yükleme
Ölçeklendirme sap yapılandırması yükleme, kaba adımları gerçekleştirmeniz gerekir:

- Yeni bir Azure VNet altyapıyı dağıtma veya uyarlama
- Yeni VM'leri Azure Yönetilen Premium Depolama, Ultra disk birimleri ve/veya NFS birimlerini ANF'ye dayalı olarak dağıtma
- - Ağ yönlendirmesini, örneğin, VM'ler arasındaki düğüm içi iletişimin bir [NVA](https://azure.microsoft.com/solutions/network-appliances/)üzerinden yönlendirilemediğinden emin olmak için uyarla. 
- SAP HANA ana düğüm'üne yükleyin.
- SAP HANA ana düğümün yapılandırma parametrelerini uyarlama
- SAP HANA işçi düğümlerinin kurulumuna devam edin

#### <a name="installation-of-sap-hana-in-scale-out-configuration"></a>SAP HANA'nın ölçeklendirilen yapılandırmada kurulumu
Azure VM altyapınız dağıtılırken ve diğer tüm hazırlıklar yapıldığından, SAP HANA ölçeklendirme yapılandırmalarını aşağıdaki adımlara yüklemeniz gerekir:

- SAP HANA ana düğüm'üne SAP belgelerine göre yükleme
- Azure Premium Depolama veya Ultra disk depolamasını paylaşılan /hana/veri ve /hana/log diskleriyle kullanmak durumunda, global.ini dosyasını değiştirmeniz ve global.ini dosyasına 'basepath_shared = hayır' parametresini eklemeniz gerekir. Bu parametre, SAP HANA'nın düğümler arasında 'paylaşılan' **/hana/veri** ve **/hana/log** birimleri olmadan ölçeklendirme de çalışmasını sağlar. Ayrıntılar [SAP Note #2080991'nde](https://launchpad.support.sap.com/#/notes/2080991)belgelenmiştir. /hana/data ve /hana/log için ANF'ye dayalı NFS hacimleri kullanıyorsanız, bu değişikliği yapmanız gerekmez
- Global.ini parametredeki nihai değişiklikten sonra SAP HANA örneğini yeniden başlatın
- Ek alt düğümler ekleyin. Ayrıca <https://help.sap.com/viewer/6b94445c94ae495c83a19646e7c3fd56/2.0.00/en-US/0d9fe701e2214e98ad4f8721f6558c34.html>bakınız. Yükleme sırasında veya daha sonra yerel hdblcm kullanarak SAP HANA inter-node iletişim için iç ağ belirtin. Daha ayrıntılı belgeler için [SAP Note #2183363'a](https://launchpad.support.sap.com/#/notes/2183363)da bakın. 

SUSE Linux'ta bekleme düğümü içeren bir SAP HANA ölçeklendirme sistemi kurmak için ayrıntılar, [SUSE Linux Enterprise Server'da Azure NetApp Dosyalarını kullanarak Azure VM'lerde bekleme düğümü içeren bir SAP HANA ölçeklendirme sistemini dağıt'ta](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-scale-out-standby-netapp-files-suse)ayrıntılı olarak açıklanmıştır. Red Hat için eşdeğer belgeler makalede, [Red Hat Enterprise Linux'taki Azure NetApp Dosyalarını kullanarak Azure VM'lerinde bekleme düğümü olan bir SAP HANA ölçeklendirme sistemi dağıtın'](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-scale-out-standby-netapp-files-rhel)da bulunabilir. 


## <a name="sap-hana-dynamic-tiering-20-for-azure-virtual-machines"></a>Azure sanal makineleri için SAP HANA Dinamik Katmanlama 2.0

Azure M serisi VM'lerde SAP HANA sertifikalarına ek olarak, SAP HANA Dynamic Tiering 2.0 Microsoft Azure'da da desteklenir (daha aşağıya sap HANA Dinamik Katmanlama dokümantasyon bağlantılarına bakın). Örneğin, bir Azure Sanal Makine'nin içindeki SAP HANA Kokpit aracılığıyla ürünü yüklemeveya çalıştırma konusunda bir fark yoktur, ancak Azure'da resmi destek için zorunlu olan birkaç önemli öğe vardır. Bu önemli noktalar aşağıda açıklanmıştır. Makale boyunca, "DT 2.0" kısaltması tam adı Dinamik Katmanlama 2.0 yerine kullanılacaktır.

SAP HANA Dynamic Tiering 2.0 SAP BW veya S4HANA tarafından desteklenmez. Ana kullanım örnekleri şu anda yerli HANA uygulamalarıdır.


### <a name="overview"></a>Genel Bakış

Aşağıdaki resimde, Microsoft Azure'daki DT 2.0 desteğine ilişkin genel bir bakış yer almaktadır. Resmi sertifikaya uymak için uyulması gereken bir dizi zorunlu gereklilik vardır:

- DT 2.0 özel bir Azure VM'ye yüklenmelidir. SAP HANA'nın çalıştığı aynı VM'de çalışmayabilir
- SAP HANA ve DT 2.0 VM'ler aynı Azure Vnet içinde dağıtılmalıdır
- SAP HANA ve DT 2.0 VM'ler Azure hızlandırılmış ağ etkinleştirilmiş olarak dağıtılmalıdır
- DT 2.0 VM'ler için depolama türü Azure Premium Depolama olmalıdır
- Birden çok Azure diski DT 2.0 VM'ye eklenmelidir
- Azure diskleri arasında şeritleme kullanarak bir yazılım baskını / çizgili hacim (lvm veya mdadm yoluyla) oluşturmak için gereklidir

Daha fazla bilgi aşağıdaki bölümlerde açıklanacaktır.

![SAP HANA DT 2.0 Mimariye Genel Bakış](media/hana-vm-operations/hana-data-tiering.png)



### <a name="dedicated-azure-vm-for-sap-hana-dt-20"></a>SAP HANA DT 2.0 için özel Azure VM

Azure IaaS'da DT 2.0 yalnızca özel bir VM'de desteklenir. Hana örneğinin çalıştığı aynı Azure VM'de DT 2.0 çalışmasına izin verilmez. Başlangıçta iki VM türleri SAP HANA DT 2.0 çalıştırmak için kullanılabilir:

- M64-32ms 
- E32sv3 

VM türü [açıklamasına buradan](https://docs.microsoft.com/azure/virtual-machines/linux/sizes-memory) bakınız

Maliyetlerden tasarruf etmek için "sıcak" verileri boşaltmak üzere olan DT 2.0'ın temel fikri göz önüne alındığında, ilgili VM boyutlarını kullanmak mantıklıdır. Olası kombinasyonları ile ilgili olsa da sıkı bir kural yoktur. Belirli müşteri iş yüküne bağlıdır.

Önerilen yapılandırmalar olacaktır:

| SAP HANA VM tipi | DT 2.0 VM tipi |
| --- | --- | 
| M128ms | M64-32ms |
| M128'ler | M64-32ms |
| M64ms | E32sv3 |
| M64'ler | E32sv3 |


Desteklenen DT 2.0 VM'lere (M64-32ms ve E32sv3) sahip SAP HANA sertifikalı M serisi VM'lerin tüm kombinasyonları mümkündür.


### <a name="azure-networking-and-sap-hana-dt-20"></a>Azure ağ ve SAP HANA DT 2.0

DT 2.0'ın özel bir VM'ye yüklenmesi, DT 2.0 VM ile en az 10 Gb SAP HANA VM arasında ağ işbirliği gerektirir. Bu nedenle, tüm VM'leri aynı Azure Vnet'in içine yerleştirmek ve Azure hızlandırılmış ağ olmasını sağlamak zorunludur.

Azure hızlandırılmış ağ hakkında ek bilgilere [buradan](https://docs.microsoft.com/azure/virtual-network/create-vm-accelerated-networking-cli) bakın

### <a name="vm-storage-for-sap-hana-dt-20"></a>SAP HANA DT 2.0 için VM Depolama

DT 2.0 en iyi uygulama kılavuzuna göre, disk IO iş artışı fiziksel çekirdek başına en az 50 MB/sn olmalıdır. DT 2.0 için desteklenen iki Azure VM türü için spec baktığımızda VM için maksimum disk IO verim sınırı gibi görünüyor:

- E32sv3 : 768 MB/sn (kalibre edilmemiş) yani fiziksel çekirdek başına 48 MB/sn
- M64-32ms : Fiziksel çekirdek başına 62,5 MB/sn oranı anlamına gelen 1000 MB/sn (kalibresiz)

DT 2.0 VM'ye birden fazla Azure diski eklemek ve VM başına disk çıktısı maksimum sınırını elde etmek için işletim sistemi düzeyinde bir yazılım baskını (şeritleme) oluşturmak gerekir. Tek bir Azure diski, bu konuda maksimum VM sınırına ulaşmak için iş elde etmeyi sağlayamaz. Azure Premium depolama nın DT 2.0'ı çalıştırması zorunludur. 

- Kullanılabilir Azure disk türleri ile ilgili ayrıntılara [buradan](../../windows/disks-types.md) ulaşabilirsiniz
- Mdadm üzerinden yazılım baskını oluşturma ile ilgili [ayrıntılara buradan](https://docs.microsoft.com/azure/virtual-machines/linux/configure-raid) ulaşabilirsiniz
- LVM'yi maksimum iş hacmi için çizgili bir hacim oluşturmak üzere yapılandırma yla ilgili [ayrıntılara buradan](https://docs.microsoft.com/azure/virtual-machines/linux/configure-lvm) bulabilirsiniz

Boyut gereksinimlerine bağlı olarak, bir VM'nin maksimum iş bölümüne ulaşmak için farklı seçenekler vardır. Üst VM iş hacmi limitini elde etmek için her DT 2.0 VM türü için olası veri hacmi disk yapılandırmaları aşağıda veda edilmiştir. E32sv3 VM, daha küçük iş yükleri için bir giriş düzeyi olarak düşünülmelidir. Bu m64-32ms VM yeniden boyutlandırmak için gerekli olabilir yeterince hızlı olmadığı ortaya çıkması durumunda.
M64-32ms VM çok fazla belleğe sahip olduğu için, IO yükü özellikle okunan yoğun iş yükleri için sınıra ulaşamayabilir. Bu nedenle, müşteriye özgü iş yüküne bağlı olarak şerit kümesindeki daha az disk yeterli olabilir. Ancak güvenli tarafta olmak için aşağıdaki disk yapılandırmaları maksimum verimi garanti etmek için seçilmiştir:


| VM SKU | Disk Config 1 | Disk Config 2 | Disk Config 3 | Disk Config 4 | Disk Config 5 | 
| ---- | ---- | ---- | ---- | ---- | ---- | 
| M64-32ms | 4 x P50 -> 16 TB | 4 x P40 -> 8 TB | 5 x P30 -> 5 TB | 7 x P20 -> 3,5 TB | 8 x P15 -> 2 TB | 
| E32sv3 | 3 x P50 -> 12 TB | 3 x P40 -> 6 TB | 4 x P30 -> 4 TB | 5 x P20 -> 2,5 TB | 6 x P15 -> 1,5 TB | 


Özellikle iş yükü okuma yoğun olduğunda, veritabanı yazılımının veri hacimleri için önerilen Azure ana bilgisayar önbelleğini "salt okunur" olarak açmak için IO performansını artırabilir. Hareket günlüğü için Azure ana bilgisayar disk önbelleği "yok" olmalıdır. 

Günlük hacminin boyutuile ilgili olarak önerilen bir başlangıç noktası veri boyutunun% 15 sezgisel olduğunu. Günlük hacminin oluşturulması, maliyet ve iş hacmi gereksinimlerine bağlı olarak farklı Azure disk türleri kullanılarak gerçekleştirilebilir. Günlük hacmi için yüksek G/Ç iş hacmi gereklidir.  VM tipi M64-32ms kullanılması durumunda [Yazma Hızlandırıcı'yı](https://docs.microsoft.com/azure/virtual-machines/linux/how-to-enable-write-accelerator)etkinleştirmek zorunludur. Azure Yazma Hızlandırıcı, işlem günlüğü için en iyi disk yazma gecikmesi sağlar (yalnızca M serisi için kullanılabilir). VM türü başına maksimum disk sayısı gibi olsa da dikkate alınması gereken bazı öğeler vardır. Yazma Hızlandırıcısı ile ilgili ayrıntılara [buradan](https://docs.microsoft.com/azure/virtual-machines/windows/how-to-enable-write-accelerator) ulabilirsiniz


Günlük hacmini boyutlandırma yla ilgili birkaç örnek aşağıda verilmiştir:

| veri hacmi boyutu ve disk türü | günlük hacmi ve disk türü config 1 | günlük hacmi ve disk türü config 2 |
| --- | --- | --- |
| 4 x P50 -> 16 TB | 5 x P20 -> 2,5 TB | 3 x P30 -> 3 TB |
| 6 x P15 -> 1,5 TB | 4 x P6 -> 256 GB | 1 x P15 -> 256 GB |


SAP HANA ölçeğinde olduğu gibi, /hana/paylaşılan dizin SAP HANA VM ve DT 2.0 VM arasında paylaşılmalıdır. Sap HANA ölçeğinde, son derece kullanılabilir bir NFS sunucusu olarak hareket eden özel VM'ler kullanılarak aynı mimari önerilir. Paylaşılan bir yedekleme hacmi sağlamak için, aynı tasarım kullanılabilir. Ancak, HA'nın gerekli olup olmayacağını veya yedek sunucu olarak hareket etmek için yeterli depolama kapasitesine sahip özel bir VM'i kullanmanın yeterli olup olmadığını niçin yeterli olacağı müşteriye kalmıştır.



### <a name="links-to-dt-20-documentation"></a>DT 2.0 belgelerine bağlantılar 

- [SAP HANA Dinamik Katmanlama yükleme ve güncelleme kılavuzu](https://help.sap.com/viewer/88f82e0d010e4da1bc8963f18346f46e/2.0.03/en-US)
- [SAP HANA Dinamik Katmanlama eğitimleri ve kaynakları](https://help.sap.com/viewer/fb9c3779f9d1412b8de6dd0788fa167b/2.0.03/en-US)
- [SAP HANA Dinamik Katmanlama PoC](https://blogs.sap.com/2017/12/08/sap-hana-dynamic-tiering-delivering-on-low-tco-with-impressive-performance/)
- [SAP HANA 2.0 SPS 02 dinamik katmanlama geliştirmeleri](https://blogs.sap.com/2017/07/31/sap-hana-2.0-sps-02-dynamic-tiering-enhancements/)




## <a name="operations-for-deploying-sap-hana-on-azure-vms"></a>Azure VM'lerde SAP HANA dağıtma işlemleri
Aşağıdaki bölümlerde, AZURE VM'lerinde SAP HANA sistemlerinin dağıtılmasıyla ilgili bazı işlemler açıklanmaktadır.

### <a name="back-up-and-restore-operations-on-azure-vms"></a>Azure VM'lerde işlemleri yedekleme ve geri yükleme
Aşağıdaki belgeler, SAP HANA dağıtımınızı nasıl yedekleyip geri yükleyeceğimize açıklanmıştır:

- [SAP HANA yedeklemesine genel bakış](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-backup-guide)
- [SAP HANA dosya düzeyinde yedekleme](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-backup-file-level)
- [SAP HANA depolama anlık görüntü kıyaslamı](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-backup-storage-snapshots)


### <a name="start-and-restart-vms-that-contain-sap-hana"></a>SAP HANA içeren VM'leri başlatın ve yeniden başlatın
Azure genel bulutunun önemli bir özelliği, yalnızca bilgi işlem dakikalarınız için ücretlendirilmenizdir. Örneğin, SAP HANA çalıştıran bir VM'i kapattığınızda, bu süre içinde yalnızca depolama maliyetleri için faturalandırılırsınız. İlk dağıtımınızda VM'leriniz için statik IP adresleri belirttiğinizde başka bir özellik kullanılabilir. SAP HANA'sı olan bir VM'yi yeniden başlattığınızda, VM önceki IP adresleriyle yeniden başlatılır. 


### <a name="use-saprouter-for-sap-remote-support"></a>SAP uzaktan desteği için SAProuter'ı kullanın
Şirket içi konumlarınız ile Azure arasında siteden siteye bağlantınız varsa ve SAP bileşenlerini çalıştırıyorsanız, büyük olasılıkla SAProuter'ı zaten çalıştırMış sınızdır. Bu durumda, uzaktan destek için aşağıdaki öğeleri tamamlayın:

- SAProuter yapılandırmasında SAP HANA'yı barındıran VM'nin özel ve statik IP adresini koruyun.
- Hana VM'yi barındıran alt netin NSG'sini TCP/IP bağlantı noktası 3299 üzerinden trafiğe izin verecek şekilde yapılandırın.

Azure'a internet üzerinden bağlanıyorsanız ve SAP HANA'lı VM için bir SAP yönlendiriciniz yoksa, bileşeni yüklemeniz gerekir. SAProuter'ı Yönetim alt ağına ayrı bir VM'ye yükleyin. Aşağıdaki resim, SAP HANA'yı siteden siteye bağlantısı olmadan ve SAProuter ile dağıtmak için kaba bir şemayı gösterir:

![Sap HANA için siteden siteye bağlantı ve SAProuter olmadan kaba dağıtım şeması](media/hana-vm-operations/hana-simple-networking-saprouter.png)

SAProuter'ı Jumpbox VM'nize değil, ayrı bir VM'ye yüklediğinizden emin olun. Ayrı VM statik bir IP adresi olmalıdır. SAProuter'ınızı SAP tarafından barındırılan SAProuter'a bağlamak için BIR IP adresi için SAP ile iletişime geçin. (SAP tarafından barındırılan SAProuter, VM'nize yüklediğiniz SAProuter örneğinin muadilidir.) SAProuter örneğini yapılandırmak için SAP'nin IP adresini kullanın. Yapılandırma ayarlarında, gerekli tek bağlantı noktası TCP bağlantı noktası 3299'dur.

SAProuter üzerinden uzaktan destek bağlantılarının nasıl kurulup sürdürüldi hakkında daha fazla bilgi için [SAP belgelerine](https://support.sap.com/en/tools/connectivity-tools/remote-support.html)bakın.

### <a name="high-availability-with-sap-hana-on-azure-native-vms"></a>Azure yerel VM'lerinde SAP HANA ile yüksek kullanılabilirlik
SUSE Linux Enterprise Server veya Red Hat çalıştırıyorsanız, STONITH aygıtları ile bir Pacemaker kümesi oluşturabilirsiniz. Aygıtları, HANA Sistem Çoğaltma ve otomatik arıza ile eşzamanlı çoğaltma kullanan bir SAP HANA yapılandırması ayarlamak için kullanabilirsiniz. 'Sonraki adımlar' bölümünde listelenen daha fazla bilgi için.

## <a name="next-steps"></a>Sonraki Adımlar
Listelenen makaleleri tanıma
- [SAP HANA Azure sanal makine depolama alanı yapılandırmaları](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-vm-operations-storage)
- [SUSE Linux Enterprise Server'da Azure NetApp Dosyalarını kullanarak Azure VM'lerinde bekleme düğümü içeren bir SAP HANA ölçeklendirme sistemi dağıtma](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-scale-out-standby-netapp-files-suse)
- [Red Hat Enterprise Linux'ta Azure NetApp Dosyalarını kullanarak Azure VM'lerinde bekleme düğümü içeren bir SAP HANA ölçeklendirme sistemi dağıtma](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-scale-out-standby-netapp-files-rhel)
- [SUSE Linux Enterprise Server'da Azure VM'lerde SAP HANA'nın yüksek kullanılabilirliği](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-high-availability)
- [Red Hat Enterprise Linux'ta Azure VM'lerde SAP HANA'nın yüksek kullanılabilirliği](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-high-availability-rhel)

 

