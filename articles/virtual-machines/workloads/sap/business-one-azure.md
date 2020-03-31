---
title: Azure Sanal Makinelerde SAP Business One | Microsoft Dokümanlar
description: Azure'da SAP Business One.
services: virtual-machines-linux,virtual-machines-windows
documentationcenter: ''
author: msjuergent
manager: patfilot
editor: ''
tags: azure-resource-manager
keywords: ''
ms.service: virtual-machines-linux
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 07/15/2018
ms.author: juergent
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 18409f93ab50f7d031ec78a55b9eaf8ad1b85a49
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "70101406"
---
# <a name="sap-business-one-on-azure-virtual-machines"></a>Azure Sanal Makineler'de SAP Business One
Bu belge, SAP Business One'ı Azure Sanal Makinelere dağıtmak için kılavuz sağlar. Belgeler, SAP için Business one yükleme belgelerinin yerini alamaz. Belgeler, Azure altyapısının Business One uygulamalarını çalıştırması için temel planlama ve dağıtım yönergelerini kapsamalıdır.

Business One iki farklı veritabanını destekler:
- SQL Server - sap note #928839 - [Microsoft SQL Server için Sürüm Planlama](https://launchpad.support.sap.com/#/notes/928839)
- SAP HANA - SAP HANA için tam SAP Business One destek matrisi için, [SAP Ürün Kullanılabilirlik](https://support.sap.com/pam) Matrisi'ni kullanıma

SQL Server ile ilgili olarak, [SAP NetWeaver için Azure Sanal Makineler DBMS dağıtımında](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/dbms-guide) belgelenen temel dağıtım konuları geçerlidir. SAP HANA için bu belgede dikkate alınacak hususlar belirtilmiştir.

## <a name="prerequisites"></a>Ön koşullar
Bu kılavuzu kullanmak için aşağıdaki Azure bileşenleri hakkında temel bilgiye ihtiyacınız vardır:

- [Windows'da Azure sanal makineleri](https://docs.microsoft.com/azure/virtual-machines/windows/tutorial-manage-vm)
- [Linux'ta Azure sanal makineleri](https://docs.microsoft.com/azure/virtual-machines/linux/tutorial-manage-vm)
- [PowerShell ile Azure ağ ve sanal ağ yönetimi](https://docs.microsoft.com/azure/virtual-machines/windows/tutorial-virtual-network)
- [CLI ile Azure ağ ve sanal ağlar](https://docs.microsoft.com/azure/virtual-machines/linux/tutorial-virtual-network)
- [Azure CLI ile Azure disklerini yönetme](https://docs.microsoft.com/azure/virtual-machines/linux/tutorial-manage-disks)

Yalnızca bir iş ile ilgileniyor olsanız bile, [AZURE Sanal Makinelerin SAP NetWeaver için planlanması ve uygulanması](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/planning-guide) iyi bir bilgi kaynağı olabilir.

Sap Business One'ı dağıtan örnek olarak şu varsayım:

- VM gibi belirli bir altyapıya SAP HANA yüklemeye aşina
- SAP Business One uygulamasını Azure VM'ler gibi bir altyapıya yüklemeye aşina
- SAP Business One işletimine aşina ve seçilen DBMS sistemi
- Azure'da altyapı dağıtma ya da dağıtma hakkında bilgi

Tüm bu alanlar bu belgede yer alacaktır.

Azure belgelerinin yanı sıra, Business One'a atıfta bulunan veya iş bir için SAP'nin ana Notları olan ana SAP Notları'ndan haberdar olmalısınız:

- [528296 - SAP Business One Bültenleri ve İlgili Ürünler Için Genel Genel Bakış Notu](https://launchpad.support.sap.com/#/notes/528296)
- [2216195 - SAP Business One 9.2 için Sürüm Güncellemeler Notu, SAP HANA için sürüm](https://launchpad.support.sap.com/#/notes/2216195)
- [2483583 - SAP Business One için Merkez Not 9,3](https://launchpad.support.sap.com/#/notes/2483583)
- [2483615 - SAP Business One için Sürüm Güncellemeler Notu 9.3](https://launchpad.support.sap.com/#/notes/2483615)
- [2483595 - SAP Business One için Toplu Not 9.3 Genel Konular](https://launchpad.support.sap.com/#/notes/2483595)
- [2027458 - SAP Business One'ın SAP HANA İle İlgili Konuları için Kolektif Danışmanlık Notu, SAP HANA için sürüm](https://launchpad.support.sap.com/#/notes/2027458)


## <a name="business-one-architecture"></a>Business One Mimarlık
Business One iki katmanı olan bir uygulamadır:

- 'Şişman' istemcisi olan bir istemci katmanı
- Kiracı için veritabanı şemasını içeren bir veritabanı katmanı

İstemci kısmında hangi bileşenlerin çalıştığını ve sunucu bölümünde hangi parçaların çalıştığını sap [Business One Administrator's Guide'da](https://help.sap.com/http.svc/rc/879bd9289df34a47af838e67d74ea302/9.3/en-US/AdministratorGuide_SQL.pdf) daha iyi bir genel bakış belgelenmiştir 

İstemci katmanı ile DBMS katmanı arasında ağır gecikme açısından kritik etkileşim olduğundan, Azure'da dağıtılırken her iki aşamanın da Azure'da bulunması gerekir. kullanıcıların daha sonra Business One istemci bileşenleri için bir RDS hizmeti çalıştıran bir veya birden çok VM içine RDS olması olağandır.

### <a name="sizing-vms-for-sap-business-one"></a>SAP Business One için BOYUTlandırma VM'leri

İstemci VM(ler) boyutlandırma ile ilgili olarak, kaynak gereksinimleri SAP tarafından belge [SAP Business One Donanım Gereksinimleri Kılavuzu'nda](https://help.sap.com/http.svc/rc/011000358700000244612011e/9.3/en-US/B1_Hardware_Requirements_Guide.pdf)belgelenir. Azure için, belgenin 2.4 bölümünde belirtilen gereksinimlere odaklanmanız ve hesaplamanız gerekir.

Business One istemci bileşenlerini ve DBMS ana bilgisayarını barındırmak için Azure sanal makineleri olduğundan, yalnızca SAP NetWeaver tarafından desteklenen VM'lere izin verilir. SAP NetWeaver desteklenen Azure VM'lerinin listesini bulmak [için SAP Note #1928533'ı](https://launchpad.support.sap.com/#/notes/1928533)okuyun.

SAP HANA'nın Business One için DBMS arka ucu olarak çalıştırıldığı, [HANA certifeid IaaS platform listesinde](https://www.sap.com/dmc/exp/2014-09-02-hana-hardware/enEN/iaas.html#categories=Microsoft%20Azure%23SAP%20Business%20One) HANA'da Business için listelenen sadece VM'ler HANA için desteklenmektedir. Business One istemci bileşenleri, SAP HANA'nın DBMS sistemi olarak bu güçlü kısıtlamadan etkilenmez.

### <a name="operating-system-releases-to-use-for-sap-business-one"></a>SAP Business One için kullanılacak işletim sistemi bültenleri

Prensip olarak, her zaman en son işletim sistemi sürümlerini kullanmak en iyisidir. Özellikle Linux alanında, yeni Azure işlevi Suse ve Red Hat'in daha yeni sürümleriyle tanıtıldı. Windows tarafında, Windows Server 2016'nın kullanılması önerilir.


## <a name="deploying-infrastructure-in-azure-for-sap-business-one"></a>SAP Business One için Azure'da altyapı dağıtma
Sonraki birkaç bölümde, SAP'yi dağıtmak için önemli olan altyapı parçaları.

### <a name="azure-network-infrastructure"></a>Azure ağ altyapısı
Azure'da dağıtmanız gereken ağ altyapısı, tek bir Business One sistemini kendiniz için dağıtıp dağıtmadığınıza bağlıdır. İster müşteriler için düzinelerce Business One sistemine ev sahipliği yapan bir ev sahibi olun. Ayrıca, Azure'a nasıl bağlanıp bağlanmadığınızkonusunda tasarımda küçük değişiklikler de olabilir. Farklı olanaklardan geçerek, Azure'a VPN bağlantısına sahip olduğunuz ve Active Directory'nizi [VPN](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-plan-design) veya [ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-introduction) aracılığıyla Azure'a genişlettiğiniz bir tasarım.

![Business One ile basit ağ yapılandırması](./media/business-one-azure/simple-network-with-VPN.PNG)

Sunulan basitleştirilmiş yapılandırma, yönlendirmeyi denetlemeye ve sınırlamaya izin veren birkaç güvenlik örneği sunar. Bu ile başlar 

- Müşteri şirket içi taraftaki yönlendirici/güvenlik duvarı.
- Bir sonraki örnek, SAP Business'ınızı bir yapılandırmanızda çalıştırdığınız Azure VNet için yönlendirme ve güvenlik kurallarını tanıtmak için kullanabileceğiniz [Azure Ağ Güvenlik Grubu'dur.](https://docs.microsoft.com/azure/virtual-network/security-overview)
- Business One istemcisi kullanıcılarının veritabanını çalıştıran Business One sunucusunu da görebilmesi için, Business one istemcisini barındıran VM'yi ve iş bir sunucusunu VNet içindeki iki farklı alt ağda ayırmanız gerekir.
- İş bir sunucusuna erişimi sınırlamak için iki farklı alt ağiçin yeniden atanan Azure NSG'yi kullanırsınız.

Azure ağ yapılandırmasının daha gelişmiş bir sürümü, [hub ve spoke mimarisinin](https://docs.microsoft.com/azure/architecture/reference-architectures/hybrid-networking/hub-spoke)Azure belgelenmiş en iyi uygulamalarını temel almaktadır. Hub ve spoke'nin mimari deseni, ilk basitleştirilmiş yapılandırmayı şu şekilde değiştirir:


![Business One ile hub ve spoke yapılandırması](./media/business-one-azure/hub-spoke-network-with-VPN.PNG)

Kullanıcıların Azure'a özel bir bağlantı olmadan internet üzerinden bağlandığı durumlarda, Azure'daki ağın tasarımı, [Azure ile Internet arasındaki DMZ](https://docs.microsoft.com/azure/architecture/reference-architectures/dmz/secure-vnet-dmz)için Azure başvuru mimarisinde belgelenen ilkelerle uyumlu olmalıdır.

### <a name="business-one-database-server"></a>Business One veritabanı sunucusu
Veritabanı türü için SQL Server ve SAP HANA kullanılabilir. DBMS'den bağımsız olarak, azure VM'lerde ve ilgili ağ ve depolama konularında DBMS dağıtımları hakkında genel bir anlayış elde etmek [için, SAP iş yükü için Azure Sanal Makineler DBMS dağıtımı](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/dbms_guide_general) için gereken bilgileri okumanız gerekir.

Zaten belirli ve genel veritabanı belgelerinde vurgulanmış olsa da, kendinizi aşina yapmalısınız:

- [Azure'daki Windows sanal makinelerinin kullanılabilirliğini yönetin](https://docs.microsoft.com/azure/virtual-machines/windows/manage-availability) ve [Azure'daki Linux sanal makinelerinin kullanılabilirliğini yönetin](https://docs.microsoft.com/azure/virtual-machines/linux/manage-availability)
- [Sanal Makineler için SLA](https://azure.microsoft.com/support/legal/sla/virtual-machines/v1_8/)

Bu belgeler, depolama türleri ve yüksek kullanılabilirlik yapılandırmaseçimi ne karar vermenize yardımcı olur.

Prensipte şunları yapmalısın:

- Standart HDD'ler üzerinden Premium SSD'leri kullanın. Kullanılabilir disk türleri hakkında daha fazla bilgi edinmek için makalemize bir [disk türü seçin](../../windows/disks-types.md)
- Azure Yönetilen diskleri yönetilmeyen diskler üzerinde kullanma
- Disk yapılandırmanızla yapılandırılan yeterli IOPS ve G/Ç iş sahibi olduğunuzdan emin olun
- Uygun maliyetli depolama yapılandırması için /hana/veri ve /hana/log hacmini birleştirin


#### <a name="sql-server-as-dbms"></a>DBMS olarak SQL Server
SQL Server'ı Business One için DBMS olarak dağıtmak [için, SAP NetWeaver için SQL Server Azure Sanal Makineler DBMS dağıtımı](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/dbms_guide_sqlserver)na gidin. 

SQL Server için DBMS tarafı için kaba boyutlandırma tahminleri şunlardır:

| Kullanıcı sayısı | Sanal çekirdek | Bellek | Örnek VM türleri |
| --- | --- | --- | --- |
| 20'ye kadar | 4 | 16 GB | D4s_v3, E4s_v3 |
| 40'a kadar | 8 | 32 GB | D8s_v3, E8s_v3 |
| 80'e kadar | 16 | 64 GB | D16s_v3, E16s_v3 |
| 150'ye kadar | 32 | 128 GB | D32s_v3, E32s_v3 |

Yukarıda listelenen boyutlandırma ile başlamak için bir fikir vermelidir. Daha az veya daha fazla kaynağa ihtiyacınız olabilir, bu durumda azure'da bir uyarlama kolaydır. VM türleri arasında bir değişiklik, VM'nin yeniden başlatılmasıyla mümkündür.

#### <a name="sap-hana-as-dbms"></a>SAP HANA - DBMS
Aşağıdaki bölümlerde SAP HANA'yı DBMS olarak kullanarak [Azure işlemleri kılavuzundaki BELGE SAP HANA'nın](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-vm-operations)dikkatlerini takip etmelisiniz.

Azure'da Business One veritabanı olarak SAP HANA çevresinde yüksek kullanılabilirlik ve olağanüstü durum kurtarma yapılandırmaları için, [Azure sanal makineleri için SAP HANA yüksek kullanılabilirlik](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-availability-overview) belgelerini ve bu belgeden işaret edilen belgeleri okumalısınız.

SAP HANA yedekleme ve geri yükleme stratejileri için, [Azure Sanal Makineler'de SAP HANA için](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-backup-guide) belge Yedekleme kılavuzunu ve bu belgeden işaret edilen belgeleri okumalısınız.

 
### <a name="business-one-client-server"></a>Business One istemci sunucusu
Bu bileşenler için depolama konuları birincil endişe değildir. yine de, güvenilir bir platforma sahip olmak istiyorum. Bu nedenle, bu VM için, temel VHD için bile Azure Premium Depolama'yı kullanmanız gerekir. [SAP Business One Donanım Gereksinimleri Kılavuzu'nda](https://help.sap.com/http.svc/rc/011000358700000244612011e/9.3/en-US/B1_Hardware_Requirements_Guide.pdf)verilen verilerle VM'yi boyutlandırma. Azure için, belgenin 2.4 bölümünde belirtilen gereksinimlere odaklanmanız ve hesaplamanız gerekir. Gereksinimleri hesaplarken, sizin için ideal VM'yi bulmak için bunları aşağıdaki belgelerle karşılaştırmanız gerekir:

- [Azure'daki Windows sanal makinesi boyutları](https://docs.microsoft.com/azure/virtual-machines/windows/sizes)
- [SAP Not #1928533](https://launchpad.support.sap.com/#/notes/1928533)

Gereken CPU ve bellek sayısını Microsoft tarafından belgelenenlerle karşılaştırın. Ayrıca VM'leri seçerken ağ iş buzunu aklınızda bulundurun.








