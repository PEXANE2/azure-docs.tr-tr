---
title: Azure VM 'lerinde SAP kullanmaya başlama | Microsoft Docs
description: Microsoft Azure sanal makinelerde (VM) çalışan SAP Çözümleri hakkında bilgi edinin
services: virtual-machines-linux
documentationcenter: ''
author: msjuergent
manager: bburns
editor: ''
tags: azure-resource-manager
keywords: ''
ms.assetid: ad8e5c75-0cf6-4564-ae62-ea1246b4e5f2
ms.service: virtual-machines-linux
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 10/26/2020
ms.author: juergent
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: e9b27224058b41e34995fc0ea8104301c426ea9a
ms.sourcegitcommit: 4cb89d880be26a2a4531fedcc59317471fe729cd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/27/2020
ms.locfileid: "92676069"
---
# <a name="use-azure-to-host-and-run-sap-workload-scenarios"></a>SAP iş yükü senaryolarını barındırmak ve çalıştırmak için Azure kullanın

Microsoft Azure kullandığınızda, görev açısından kritik SAP iş yüklerinizi ve senaryolarını ölçeklenebilir, uyumlu ve kurumsal olarak kanıtlanmış bir platformda güvenilir bir şekilde çalıştırabilirsiniz. Azure 'un ölçeklenebilirlik, esneklik ve maliyet tasarrufu olanakları elde edersiniz. Microsoft ve SAP arasındaki genişletilmiş iş ortaklığı sayesinde, SAP uygulamalarını Azure 'daki geliştirme ve test ve üretim senaryolarında çalıştırabilir ve tam olarak desteklenmektedir. SAP NetWeaver ile SAP S/4HANA, Linux 'ta SAP BI ve Windows 'a SAP HANA, bu konu başlığı altında yer aldık.

Azure üzerinde farklı DBMS ile SAP NetWeaver senaryolarını barındırmanın yanı sıra Azure 'da SAP BI gibi diğer SAP iş yükü senaryolarını de barındırabilirsiniz. 

SAP HANA için Azure 'un benzersizliği, Azure 'ı ayrı ayarlayan bir tekliftir. Azure, SAP HANA içeren daha fazla bellek ve CPU kaynak gerektiren SAP senaryolarına olanak tanımak için, müşterinin adanmış çıplak donanımlarının kullanımını sağlar. S/4HANA veya diğer SAP HANA iş yükü için 24 TB 'a kadar (120 TB genişleme) bellek gerektiren SAP HANA dağıtımlarını çalıştırmak için bu çözümü kullanın. 

Azure 'da SAP iş yükü senaryolarını barındırmak, kimlik tümleştirmesi ve çoklu oturum açma gereksinimleri de oluşturabilir. Bu durum, farklı SAP bileşenleri ve hizmet olarak yazılım (SaaS) veya hizmet olarak platform (PaaS) tekliflerini bağlamak için Azure Active Directory (Azure AD) kullandığınızda ortaya çıkabilir. Azure AD ve SAP varlıklarıyla ilgili tümleştirme ve çoklu oturum açma senaryolarından oluşan bir liste, "Azure AD SAP kimlik tümleştirmesi ve çoklu oturum açma" bölümünde açıklanmaktadır ve belgelenmiştir.

## <a name="changes-to-the-sap-workload-section"></a>SAP iş yükü bölümünde yapılan değişiklikler
Azure iş yükünde SAP bölümündeki belgelerde yapılan değişiklikler bu makalenin sonunda listelenmiştir. Değişiklik günlüğündeki girişler 180 gün boyunca tutulur.

## <a name="you-want-to-know"></a>Bilmeniz istediğiniz
Belirli sorularınız varsa, başlangıç sayfasının bu bölümündeki belirli belgelere veya akışlara yöneltireceğiz. Şunları bildirmek istiyorsunuz:

- Hangi SAP yazılım sürümleri ve işletim sistemi sürümleri için desteklenen Azure VM 'Leri ve HANA büyük örnek birimleri. Yanıtlar için [Azure dağıtımı Için HANGI SAP yazılımlarının desteklendiğini](./sap-supported-product-on-azure.md) ve bilgileri bulma işlemini okuyun
- Azure VM 'Leri ve HANA büyük örneklerle desteklenen SAP dağıtım senaryoları. Desteklenen senaryolar hakkında daha fazla bilgi belgelerde bulunabilir:
    - [Azure sanal makinesi üzerinde SAP iş yüküne yönelik desteklenen senaryolar](./sap-planning-supported-configurations.md)
    - [HANA büyük örneği için desteklenen senaryolar](./hana-supported-scenario.md)
- Azure Hizmetleri, Azure VM türleri ve Azure Depolama Hizmetleri farklı Azure bölgelerinde kullanılabilir, [bölgeye göre kullanılabilir site ürünlerini](https://azure.microsoft.com/global-infrastructure/services/) kontrol edin 
- Windows ve Paceyapıcısı 'nin desteklenme yanı sıra üçüncü taraf HA çerçevesi çalışıyor mu? [Sap destek notunun](https://launchpad.support.sap.com/#/notes/1928533) alt kısmına bakın #1928533
- Senaryoum için en iyi Azure depolama alanı nedir? [SAP iş yükü Için Azure depolama türlerini](./planning-guide-storage.md) okuyun

 
## <a name="sap-hana-on-azure-large-instances"></a>Azure’da SAP HANA (Büyük Örnekler)

Bir dizi belge, Azure 'daki SAP HANA (büyük örnekler) veya kısa, HANA büyük örnekleri için size yol gösterir. HANA büyük örnekleri hakkında daha fazla bilgi için, [Azure 'da SAP HANA belge genel bakış ve mimarisine (büyük örnekler)](./hana-overview-architecture.md) BAŞLAYıN ve Hana büyük örnek bölümündeki ilgili belgelere gidin



## <a name="sap-hana-on-azure-virtual-machines"></a>Azure sanal makineler 'de SAP HANA
Belgelerinin bu bölümü SAP HANA farklı yönlerini kapsamaktadır. Bir önkoşul olarak, Azure IaaS 'in temel hizmetlerini sağlayan Azure 'un asıl hizmetleri hakkında bilgi sahibi olmanız gerekir. Bu nedenle, Azure işlem, depolama ve ağ hakkında bilgi sahibi olmanız gerekir. Bu konuların birçoğu SAP NetWeaver ile ilgili [Azure planlama kılavuzunda](./planning-guide.md)ele alınır. 

 

## <a name="sap-netweaver-deployed-on-azure-virtual-machines"></a>Azure sanal makinelerinde dağıtılan SAP NetWeaver
Bu bölümde, Azure 'da SAP NetWeaver, SAP ve Iş için planlama ve dağıtım belgeleri listelenmiştir. Belgeler, Azure 'da SAP iş yükünün temel alınarak ve HANA olmayan veritabanlarının kullanımına odaklanmaktadır. Yüksek kullanılabilirlik için belgeler ve makaleler, Azure 'da yüksek kullanılabilirlik SAP HANA için de temel sahiptir

## <a name="sap-netweaver-and-s4hana-high-availability"></a>SAP NetWeaver ve S/4HANA yüksek kullanılabilirlik
SAP uygulama katmanının ve DBMS 'nin yüksek kullanılabilirliği, belge [Azure sanal makineleri için yüksek kullanılabilirliğe sahıp SAP NetWeaver](./sap-high-availability-guide-start.md) ile başlayan ayrıntılarla birlikte belgelenmiştir



## <a name="integrate-azure-ad-with-sap-services"></a>Azure AD 'yi SAP hizmetleriyle tümleştirme
Bu bölümde, SAP SaaS ve PaaS hizmetleri, NetWeaver ve Fiori ' nin birçoğu ile SSO 'yu yapılandırma hakkında bilgi edinebilirsiniz. 



## <a name="documentation-on-integration-of-azure-services-into-sap-components"></a>Azure hizmetlerinin SAP bileşenleriyle tümleştirilmesine yönelik belgeler
Bu bölümde, PowerBI tümleştirmesiyle ilgili belgeleri SAP veri kaynaklarına ve SAP BW Azure Data Factory tümleştirmesini bulabilirsiniz.



## <a name="change-log"></a>Değişiklik Günlüğü

- 10/26/2020: Azure Premium Depolama yapılandırması için bazı tabloları değiştirme [SAP HANA Azure sanal makine depolama yapılandırmalarında](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-vm-operations-storage) sağlanan ve veri bloğu aktarım hızını açıklığa kavuşturacak
- 10/22/2020: [SLES for SAP uygulamaları üzerindeki Azure VM](./high-availability-guide-suse.md)'lerinde SAP NW için ha 'DA, [ANF ile SLES](./high-availability-guide-suse-netapp-files.md)'deki Azure VM 'lerinde SAP NW 'de [sap NW için ha, net.IPv4.TCP_KEEPALIVE_TIME](./high-availability-guide-rhel.md) [Khel ile RHEL](./high-availability-guide-rhel-netapp-files.md) 'nin Azure VM 'lerinde  
- 10/16/2020: [SLES 'In pacemaker Ile Azure VM 'LERINDE IBM DB2 LUW 'ın ha](./dbms-guide-ha-ibm.md)'de değiştirilmesi [Azure VM 'lerinde SAP NW için, RHEL 'nin SAP uygulamaları için](./high-availability-guide-rhel.md)ha, RHEL ÜZERINDEKI Azure VM 'lerinde [IBM DB2 LUW](./high-availability-guide-rhel-ibm-db2-luw.md), RHEL 'de Azure VM 'lerinde SAP NW [için ha](./high-availability-guide-rhel-multi-sid.md), [ANF Ile RHEL üzerindeki Azure VM 'lerinde SAP NW](./high-availability-guide-rhel-netapp-files.md)için ha [SLES for SAP UYGULAMALARı üzerinde SAP NW için ha](./high-availability-guide-suse.md), SLES [Multi-SID kılavuzundaki Azure VM 'LERINDE SAP](./high-availability-guide-suse-multi-sid.md)'ye yönelik ha, [SLES 'de Azure VM 'Lerinde SAP NW için ha SAP uygulamaları için ANF](./high-availability-guide-suse-netapp-files.md), [SLES 'de Azure VM 'lerinde NFS için ha](./high-availability-guide-suse-nfs.md), [SLES üzerinde Azure VM 'lerinde SAP HANA ha](./sap-hana-high-availability.md), [RHEL üzerinde ANF ile SAP HANA ölçeği artırma](./sap-hana-high-availability-netapp-files-red-hat.md) , [RHEL üzerinde Azure VM 'lerinde SAP HANA ha](./sap-hana-high-availability-rhel.md), [RHEL üzerindeki Azure VM 'Lerinde pacemaker Ile genişleme HSR SAP HANA](./sap-hana-high-availability-scale-out-hsr-rhel.md)wsfc ve [paylaşılan disk Ile](./sap-high-availability-infrastructure-wsfc-shared-disk.md)SAP yoks/SCS, wsfc ve [Azure Paylaşılan](./sap-ascs-ha-multi-sid-wsfc-azure-shared-disk.md) disk ile SAP yoks/SCS için çok düzeyli ha Kılavuzu ve IKINCIL IP 'lerde Yük Dengeleme senaryolarında, kayan IP 'nin desteklenmeyen bir Ifade eklemek için çok [düzeyli ha Kılavuzu](./sap-ascs-ha-multi-sid-wsfc-shared-disk.md) için Azure altyapısını hazırlama 
- 10/16/2020: HANA büyük örneklerde [SAP HANA yedekleme ve geri yükleme](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-backup-restore) sırasında Hana büyük örneklerinin depolama anlık görüntülerini denetlemek için belgeler ekleme
- 10/15/2020: Azure 'da SAP BusinessObjects bı platformu, Azure 'da SAP BusinessObjects [bı platformu planlama ve uygulama kılavuzu](businessobjects-deployment-guide.md) , Azure 'da [Linux Için Azure ve SAP BusinessObjects bı platform dağıtım kılavuzu](businessobjects-deployment-guide-linux.md) ' na bakın
- 10/05/2020: [RHEL yapılandırma kılavuzunda Azure VM 'Lerinde pacemaker ile SAP HANA genişleme HSR](./sap-hana-high-availability-scale-out-hsr-rhel.md) sürümü
- 09/30/2020: [RHEL üzerinde Azure VM 'lerinde SAP HANA yüksek kullanılabilirlik](./sap-hana-high-availability-rhel.md), ha, RHEL [üzerinde anf ile SAP HANA ölçeği artırma](./sap-hana-high-availability-netapp-files-red-hat.md) ve RHEL 8,1 yönergelerini uyarlamak için [Azure 'Da RHEL üzerinde paceyapıcısı ayarlama](./high-availability-guide-rhel-pacemaker.md)
- 09/29/2020: [SAP uygulamalarıyla en iyi ağ gecikme süresi Için Azure yakınlık yerleşimi grupları](./sap-proximity-placement-scenarios.md) makalesindeki PPG kullanımı etrafında kısıtlamalar ve öneriler yapılıyor 
- 09/28/2020: [SAP HANA için Azure NetApp Files belge NFS v 4.1 birimleri](./hana-vm-operations-netapp.md) ile Azure NetApp Files kullanarak SAP HANA için yeni bir depolama işlemi Kılavuzu ekleniyor
- 09/23/2020: [hLi Için kullanılabilir SKU 'larda](./hana-available-skus.md) hLi için yeni sertifikalı SKU 'lar ekleyin 
- 09/20/2020: belgelerde yapılan değişiklikler [SAP iş yükü](./dbms_guide_general.md)için Azure sanal makineleri dbms dağıtımı [SQL Server](./dbms_guide_sqlserver.md), SAP NetWeaver IÇIN Azure sanal makineler DBMS dağıtımı, [Azure sanal makineler for SAP Iş yükü için Azure sanal makineleri, Azure sanal makineler, SAP Iş yüküne](./dbms_guide_oracle.md)yönelik [IBM DB2 Azure sanal makineler](./dbms_guide_ibm.md) ve SAP ikililerini farklı Azure disklerine ayırmayı sağlayan yeni yapılandırma önerisine uyum sağlar. Ayrıca, farklı kılavuzlara Ultra disk önerileri de ekleniyor.
- 09/08/2020: stonıth tanımlarını netleştirmek için [SLES 'teki Azure VM 'lerinde SAP HANA yüksek kullanılabilirlik](./sap-hana-high-availability.md) olarak değiştirin
- 09/03/2020: [SAP HANA Azure sanal makine depolama yapılandırmalarında](./hana-vm-operations-storage.md) , Ultra disk Ile 1 GB 'lik kapasite başına en az 2 IOPS olacak şekilde değiştirin
- 09/02/2020: [HLI Için kullanılabilir SKU](./hana-available-skus.md) 'LARDA, Hana sertifikalı SKU 'lar daha fazla saydam olacak şekilde değiştirin
- 08/28/2020: mımpo 'yi onarmak için [ANF Ile SLES 'Teki Azure VM 'LERINDE SAP NW Için ha](./high-availability-guide-suse-netapp-files.md) 'de değiştirin
- 08/25/2020: [wsfc ve paylaşılan disk Ile SAP ascs/SCS Için ha kılavuzunda](./sap-high-availability-guide-wsfc-shared-disk.md)değişiklik yaparken, wsfc ve paylaşılan DISK [Ile SAP ascs/SCS için Azure altyapısını hazırlayın](./sap-high-availability-infrastructure-wsfc-shared-disk.md) ve Azure PAYLAŞıLAN DISK ve belge SAP ERS2 mimarisini kullanma seçeneğini göstermek için [wsfc ve PAYLAŞıLAN disk Ile SAP NW ha 'yı yükledikten](./sap-high-availability-guide-wsfc-shared-disk.md) sonra
- 08/25/2020: [wsfc ve Azure Paylaşılan disk Ile SAP yoks/SCS için çok DÜZEYLI ha kılavuzunun](./sap-ascs-ha-multi-sid-wsfc-azure-shared-disk.md) yayını
- 08/25/2020: [wsfc ve Azure NetApp Files (SMB) Ile SAP ASCS/SCS Için ha kılavuzunda](./high-availability-guide-windows-netapp-files-smb.md)değiştirme wsfc ve [dosya paylaşımıyla](./sap-high-availability-infrastructure-wsfc-file-share.md)SAP yoks/SCS, wsfc ve [paylaşılan disk](./sap-ascs-ha-multi-sid-wsfc-shared-disk.md) ile SAP yoks/SCS için çok düzeyli ha Kılavuzu, wsfc ve paylaşılan disk ile SAP yoks/SCS IÇIN çok [düzeyli ha Kılavuzu ve WFC](./sap-ascs-ha-multi-sid-wsfc-file-share.md) ve paylaşılan disk ile SAP 
- 08/21/2020: I ve II türünde HLı birimler için kullanılabilir işletim sistemi olarak, [Hana büyük örnekleri Için uyumlu Işletim sistemlerine](./os-compatibility-matrix-hana-large-instance.md) yeni işletim sistemi sürümü ekleniyor
- 08/18/2020: [RHEL üzerinde ANF ile SAP HANA ölçeği Için ha](./sap-hana-high-availability-netapp-files-red-hat.md) sürümü
- 08/17/2020: SAP NetWeaver sistemlerini Şirket içinden Azure 'a taşımak için Azure Site Recovery kullanma hakkında bilgi ekleyin. [Azure sanal makineler planlama ve uygulama için, SAP NetWeaver](./planning-guide.md)
- 08/14/2020: [IBM DB2 Azure sanal makineleri için disk yapılandırma önerisi, SAP Için Azure sanal MAKINELER DBMS dağıtımı](./dbms_guide_ibm.md)
- 08/11/2020: RHEL 7,6, [Hana büyük örnekleri Için uyumlu Işletim sistemlerine](./os-compatibility-matrix-hana-large-instance.md) , ı türünde HLI birimler için kullanılabilir işletim sistemi olarak ekleniyor
- 08/10/2020: [Azure sanal makine depolama yapılandırmalarında SAP HANA](./hana-vm-operations-storage.md) SAP HANA depolama yapılandırması ve [Azure 'da SAP iş yükleri için bazı güncelleştirmeler yapma: planlama ve dağıtım denetim listesi](./sap-deployment-checklist.md)
- 08/04/2020: [Azure 'DA SLES 'de pacemaker 'ı ayarlama](./high-availability-guide-suse-pacemaker.md) ve [Azure 'daki RHEL 'de pacemaker](./high-availability-guide-rhel-pacemaker.md) 'ı ayarlama sırasında değişiklik yaparken, paceoluşturucu kümeleri için güvenilir ad çözümlemesinin önemini vurgulayın
- 08/04/2020: [SAP NW ha Ile WFCS 'de, dosya paylaşımıyla](./sap-high-availability-installation-wsfc-file-share.md)değiştirin, [paylaşılan disk ile wfcs 'de SAP NW ha](./sap-high-availability-installation-wsfc-shared-disk.md), [Azure VM](./high-availability-guide.md)'lerinde SAP NW için ha, [SLES](./high-availability-guide-suse.md)üzerinde SAP NW için ha, SLES 'deki [Azure VM 'Lerinde](./high-availability-guide-suse-netapp-files.md)SAP [SLES Multi-SID KıLAVUZUNDAKI Azure VM 'lerinde SAP NW Için ha](./high-availability-guide-suse-multi-sid.md), RHEL 'de Azure VM 'lerinde SAP [NetWeaver için yüksek kullanılabilirlik](./high-availability-guide-rhel.md), Azure VM 'lerinde RHEL 'de SAP [için,](./high-availability-guide-rhel-netapp-files.md) RHEL [Çoklu SID](./high-availability-guide-rhel-multi-sid.md) 'de Azure VM 'lerinde for SAP NW `enque/encni/set_so_keepalive`
- 23 Temmuz 2020: SAP HANA Büyük Örnekleri ayırmayı satın almadan önce bilmeniz gerekenleri açıklayan [bir Azure rezervasyon ile SAP HANA büyük örnekleri kaydetme](../../../cost-management-billing/reservations/prepay-hana-large-instances-reserved-capacity.md) ve satın alma işlemi yapma
- 16 Temmuz 2020: [dağıtım KıLAVUZUNDA](deployment-guide.md) SAP IÇIN yeni VM uzantısı yüklemek üzere Azure PowerShell nasıl kullanacağınızı açıkla
- 04 Temmuz 2020:  [SAP Çözümleri Için Azure Izleyici sürümü (Önizleme)](./azure-monitor-overview.md)
- 01 Temmuz 2020: Azure [sanal makine depolama yapılandırmalarını SAP HANA](./hana-vm-operations-storage.md) belgedeki Azure Premium Depolama patlaması işlevine göre daha ucuz depolama yapılandırması önerme 
- 24 Haziran 2020: Azure 'da yeni geliştirilmiş Azure sınır Aracısı ve cihazların Azure sınır aracısına göre daha dayanıklı STONITH yapılandırmasını serbest bırakmak için [Azure 'daki SLES 'de Paceyapıcısı ayarlama](./high-availability-guide-suse-pacemaker.md) sırasında değişiklik yapın 
- 24 Haziran 2020: [Azure 'daki RHEL 'nin](./high-availability-guide-rhel-pacemaker.md) daha dayanıklı STONITH yapılandırması yayınlamaması Için pacemaker ayarlama sırasında değişiklik
- 23 Haziran 2020: [SAP NetWeaver Kılavuzu planlama ve uygulama Için Azure sanal makinelerinde](./planning-guide.md) yapılan DEĞIŞIKLIKLER ve [SAP iş yükü Kılavuzu için Azure depolama türlerine](./planning-guide-storage.md) giriş
- 06/22/2020: [dağıtım KıLAVUZUNA](deployment-guide.md) SAP IÇIN yeni VM uzantısı için yükleme adımları ekleyin
- 16 Haziran 2020: SUSE genel bulut altyapısı 101 belgelerine bir bağlantı eklemek için [SAP ha senaryolarında Azure Standard ILB kullanarak VM 'ler Için genel uç nokta bağlantısında](./high-availability-guide-standard-load-balancer-outbound-connections.md) değişiklik yapın 
- 10 Haziran 2020: yeni HLI SKU 'ları HLI ve [SAP HANA (büyük örnekler) depolama mimarisi](./hana-storage-architecture.md) [Için kullanılabilir SKU 'lara](./hana-available-skus.md) ekleme
- 21 Mayıs 2020: [Azure 'DA SLES üzerinde Paceyapıcısı ayarlama](./high-availability-guide-suse-pacemaker.md) ve Azure ['da RHEL 'de PACEYAPıCıSı](./high-availability-guide-rhel-pacemaker.md) ayarlama, [SAP ha senaryolarında Azure Standard ILB kullanarak VM 'ler için genel uç nokta bağlantısına](./high-availability-guide-standard-load-balancer-outbound-connections.md) bağlantı ekleme  
- 19 Mayıs 2020: [SAP HANA Azure sanal makine depolama yapılandırmalarında](./hana-vm-operations-storage.md) , Hana ile ilgili birimler için LVM kullanırken kök birim grubunu kullanmak için önemli bir ileti ekleyin
- 19 Mayıs 2020: Hana büyük [örnekler Için uyumlu Işletim SISTEMLERINDE](/- azure/virtual-machines/workloads/sap/os-compatibility-matrix-hana-large-instance) Hana büyük örnek türü II için yeni desteklenen Işletim sistemi ekleme
- 12 Mayıs 2020: bağlantıları güncelleştirmek ve üçüncü taraf güvenlik duvarı yapılandırması için bilgi eklemek üzere [SAP ha senaryolarında Azure Standard ILB kullanarak VM 'ler Için genel uç nokta bağlantısında](./high-availability-guide-standard-load-balancer-outbound-connections.md) değişiklik yapın
- 11 Mayıs 2020: [SLES 'Teki Azure VM 'lerinde SAP HANA yüksek kullanılabilirlik](./sap-hana-high-availability.md) düzeyinde değişiklik yaparken, Netcat kaynağı için kaynak listesini 0 olarak ayarlama işlemi daha kolay yük devretmeye yol açar. 
- 05 Mayıs 2020: [Azure sanal makinelerindeki DEĞIŞIKLIKLER SAP NetWeaver için planlama ve uygulama;](./planning-guide.md) bu Gen2 DAĞıTıMLARıNıN Mv1 VM ailesi için kullanılabilir olduğunu ifade edebilir
- 24 Nisan 2020: [SLES 'de ANF Ile Azure VM 'lerinde bekleme düğümü ile SAP HANA genişleme](./sap-hana-scale-out-standby-netapp-files-suse.md)değişiklikleri Azure VM 'lerinde NetWeaver on [RHEL ile Azure](./sap-hana-scale-out-standby-netapp-files-rhel.md)VM 'lerinde, [SLES](./high-availability-guide-suse-netapp-files.md) 'deki Azure VM 'LERINDE, ANF birimleri için IP adreslerinin otomatik olarak atandığını açıklama eklemek için ANF ile Azure VM 'lerinde yüksek kullanılabilirlik ve [yüksek](./high-availability-guide-rhel-netapp-files.md) kullanılabilirlik içeren SAP HANA.
- 22 Nisan 2020: [SLES 'Deki Azure VM 'lerinde SAP HANA yüksek kullanılabilirlik](./sap-hana-high-availability.md) olarak değişiklik `is-managed` , kümeyi bakım moduna alma ile çakıştığından, yönergelerden meta özniteliği kaldırma
- 21 Nisan 2020: SQL Azure DB, SAP için desteklenen DBMS (Hybrsıs) Commerce platform 1811 ve üzeri sürümlerde Azure dağıtımları ve [SAP Sertifikaları ve Microsoft Azure üzerinde çalışan yapılandırma](./sap-certifications.md) [için SAP yazılımlarının desteklendiği](./sap-supported-product-on-azure.md) makalelerde eklendi
- 16 Nisan 2020: [Azure dağıtımları için](./sap-supported-product-on-azure.md) desteklenen SAP (hybru) ticaret platformu olarak SAP HANA eklendi ve [Microsoft Azure üzerinde çalışan SAP Sertifikaları ve yapılandırma](./sap-certifications.md)
- 13 Nisan 2020: [SAP asa Azure sanal makineler için](./dbms_guide_sapase.md) tam SAP asa sürüm NUMARALARıNA, SAP iş yükü için DBMS dağıtımına doğru
- 07 Nisan 2020: Cloud-netconfig-Azure yönergelerini açıklığa kavuşturacak [Azure 'DA SLES 'de Paceyapıcısı ayarlama](./high-availability-guide-suse-pacemaker.md) sırasında değişiklik
- 06 Nisan 2020: SAP HANA, Azure VM 'lerinde yer alan, [SLES ve Azure NetApp Files ile](./sap-hana-scale-out-standby-netapp-files-suse.md) Azure VM 'lerinde bekleme düğümü Ile SAP HANA değişiklik, NetApp [tr-](https://www.netapp.com/us/media/tr-4746.pdf) [4746](https://www.netapp.com/us/media/tr-4746.pdf)4435 başvurularını kaldırmak için [RHEL 'daki Azure NetApp Files](./sap-hana-scale-out-standby-netapp-files-rhel.md)
- 31 Mart 2020: [Azure VM 'Lerde yüksek düzeyde SAP HANA](./sap-hana-high-availability.md) [SAP HANA ve RHEL üzerindeki Azure](./sap-hana-high-availability-rhel.md) VM 'lerinde, Şeritli birimleri oluştururken Stripe boyutunu belirtme hakkında yönergeler eklemek için
- 27 Mart 2020: dosya sistemi bağlama seçeneklerini NetApp TR-4746 (eşitleme bağlama seçeneğini kaldır) olarak hizalamak için [ANF Ile SLES 'Deki Azure VM 'LERINDE SAP NW Için yüksek kullanılabilirlik](./high-availability-guide-suse-netapp-files.md) olarak değiştirin
- 26 Mart 2020: [SLES çok düzeyli kılavuzdaki Azure VM 'LERINDE SAP NetWeaver Için yüksek kullanılabilirlik](./high-availability-guide-suse-multi-sid.md) değişikliği yapın NetApp TR-4746 başvurusu ekleme
- 26 Mart 2020: [SLES for SAP Applications üzerinde SAP NetWeaver Için yüksek kullanılabilirlik](./high-availability-guide-suse.md)olarak değişiklik yapın ve SAP uygulamaları için [Azure NetApp Files, SLES 'teki Azure VM 'lerinde SAP NetWeaver için yüksek kullanılabilirlik](./high-availability-guide-suse-netapp-files.md)yapın [SLES](./high-availability-guide-suse-nfs.md)üzerinde Azure VM 'lerde yüksek KULLANıLABILIRLIK, [rhel çoklu SID kılavuzundaki](./high-availability-guide-suse-multi-sid.md)Azure [VM](./high-availability-guide-rhel-netapp-files.md) 'lerinde SAP NetWeaver için yüksek kullanılabilirlik, SAP uygulamaları için [RHEL 'de](./high-availability-guide-rhel.md) Azure VM 'lerde yüksek kullanılabilirlik ve güvenlik arka uç havuzu oluşturma yönergelerini açıklığa kavuşturma ve Azure VM 'lerde yüksek kullanılabilirlik Azure NetApp Files Azure Load Balancer
- 19 Mart 2020: belge hızlı başlangıcı 'nın ana düzeltmesi: Azure sanal makinelerine [SAP HANA yüklemek](./hana-get-started.md) Için [Azure sanal makinelerinde tek örnekli SAP HANA el ile yükleme](./hana-get-started.md)
- 17 Mart 2020: artık gerekli olmayan SBD yapılandırma ayarını kaldırmak için [Azure 'daki SUSE Linux Enterprise Server Paceyapıcısı ayarlama](./high-availability-guide-suse-pacemaker.md) sırasında değiştirin
- 16 2020 Mart: [Azure dağıtımları için desteklenen SAP yazılımının](./sap-supported-product-on-azure.md) SAP HANA IaaS sertifikalı platformunda sütun sertifikasyon senaryosunun açıklaması
- 03/11/2020: DBMS örnek desteği başına birden çok veritabanını netleştirmek için [Azure sanal makinesi desteklenen senaryolarda SAP iş](./sap-planning-supported-configurations.md) yükünde değişiklik yapın
- 11 Mart 2020:1. nesil ve 2. nesil VM 'Lerin yanı sıra [SAP NetWeaver için planlama ve uygulama Için Azure sanal makinelerinde](./planning-guide.md) değişiklik yapma
- 10 Mart 2020: [SAP HANA Azure sanal makine depolama yapılandırmalarında](./hana-vm-operations-storage.md) , ANF 'nin gerçek mevcut aktarım hızı sınırlarını açıklığa kavuşturacak şekilde değiştirin
- 09 Mart 2020: SAP [uygulamaları için SUSE Linux Enterprise Server Azure VM 'LERINDE SAP NetWeaver Için yüksek kullanılabilirlik](./high-availability-guide-suse.md)değişikliği, SAP uygulamaları için [Azure NetApp Files Ile SUSE Linux Enterprise Server Azure VM 'lerinde SAP NetWeaver için yüksek kullanılabilirlik](./high-availability-guide-suse-netapp-files.md) [SUSE LINUX ENTERPRISE Server üzerindeki Azure VM 'lerinde NFS için yüksek kullanılabilirlik](./high-availability-guide-suse-nfs.md), [azure 'Da SUSE Linux Enterprise Server üzerinde paceyapıcısı ayarlama, Azure](./high-availability-guide-suse-pacemaker.md)VM 'lerinde yüksek kullanılabilirliğe sahip Azure sanal makinelerinde SUSE Linux Enterprise Server, [Azure](./dbms-guide-ha-ibm.md)VM 'lerinde yüksek kullanılabilirlik ve kaynak Aracısı Azure-lb ile [High availability of SAP HANA on Azure VMs on SUSE Linux Enterprise Server](./sap-hana-high-availability.md) küme kaynaklarını güncelleştirmek için [yüksek kullanılabilirlik SUSE Linux Enterprise Server](./high-availability-guide-suse-multi-sid.md) SAP HANA 
- 05 Mart 2020: Azure bölgeleri ve Azure sanal makineleri için Azure sanal makineler 'de yapılan değişiklikleri ve içerik değişikliklerini, [SAP NetWeaver planlama ve uygulama Için](./planning-guide.md) yapılandırma
- 03/03/2020: SAP için [SLES 'Teki Azure VM 'LERINDE SAP NW 'yi](./high-availability-guide-suse-netapp-files.md) daha verimli ANF birimi düzenine göre değiştirmek için yüksek kullanılabilirlik olarak değiştirin
- 01 Mart 2020: [Azure sanal makinelerinde SAP HANA için](./sap-hana-backup-guide.md) Azure Backup hizmetini içerecek yedekleme Kılavuzu yeniden çalıştı. [SAP HANA Azure Backup dosya düzeyinde](./sap-hana-backup-file-level.md) daha az ve dar içerik ve disk anlık görüntüsü aracılığıyla yedekleme ile ilgili üçüncü bir belgeyi sildi. İçerik, Azure sanal makinelerinde SAP HANA için yedekleme kılavuzunda ele alınır 
- 27 Şubat 2020: [SLES for SAP uygulamaları](./high-availability-guide-suse.md)üzerindeki Azure VM 'lerinde SAP NW için yüksek kullanılabilirlik, SAP için [ANF](./high-availability-guide-suse-netapp-files.md) ve [SLES Multi-SID kılavuzundaki](./high-availability-guide-suse-multi-sid.md) Azure VM 'lerinde SAP NetWeaver için yüksek kullanılabilirlik, "başarısız" küme parametresini ayarlama
- 26 Şubat 2020: Azure 'da HANA için dosya sistemi seçimini netleştirmek üzere [Azure sanal makine depolama yapılandırmalarında SAP HANA](./hana-vm-operations-storage.md) değiştirin
- 26 Şubat 2020: [SAP Için yüksek kullanılabilirlik mimarisi ve senaryolarında](./sap-high-availability-architecture-scenarios.md) , RHEL çoklu SID kılavuzundaki Azure VM 'Lerinde SAP NETWEAVER için ha bağlantısını ekleme
- 26 Şubat 2020: [SLES for SAP uygulamaları ÜZERINDE SAP NW Için yüksek kullanılabilirlik](./high-availability-guide-suse.md)olarak değişiklik yapın ve SAP uygulamaları için [ANF ile SLES 'deki Azure VM 'lerinde SAP NW için yüksek](./high-availability-guide-suse-netapp-files.md)kullanılabilirlik, [Azure VM 'leri, RHEL üzerinde SAP NetWeaver Için yüksek](./high-availability-guide-rhel.md) kullanılabilirlik ve [Azure VM](./high-availability-guide-rhel-netapp-files.md) 'LERI, birden çok sıd yoks/ers kümesinin desteklenmediğini belirten Azure NetApp Files
- 26 Şubat 2020: SUSE Multi-SID küme Kılavuzu 'na bir bağlantı eklemek için  [RHEL çoklu SID kılavuzundaki Azure VM 'LERINDE SAP NetWeaver Için yüksek kullanılabilirlik](./high-availability-guide-rhel-multi-sid.md) yayını
- 02/25/2020: [SAP Için yüksek kullanılabilirlik mimarisi ve senaryolarında](./sap-high-availability-architecture-scenarios.md) , daha yeni ha makalelerine bağlantı eklemek için değiştirme
- 25 Şubat 2020: standart Azure Yük Dengeleyici ile genel uç noktaya erişimi açıklayan belgeyi işaret etmek için [SUSE Linux Enterprise Server üzerindeki Azure VM 'LERINDE IBM DB2 LUW 'ın yüksek kullanılabilirliğinde](./dbms-guide-ha-ibm.md) değişiklik yapın
- 21 Şubat 2020: [Azure sanal makineler ve SAP iş yükü Için Azure sanal MAKINELERI DBMS dağıtımı](./dbms_guide_sapase.md) makalesini tamamen gözden geçirme
- 21 Şubat 2020: [SAP HANA Azure sanal makine depolama yapılandırması](./hana-vm-operations-storage.md) 'nda,/Hana/Data için şerit boyutundaki yeni öneriyi ve g/ç zamanlayıcı ayarını ekleyerek değiştirin
- 21 Şubat 2020: S224 ve S224m ' nin yeni sertifikalı SKU 'Larını temsil etmesi için HANA büyük örnek belgelerindeki değişiklikler
- 21 Şubat 2020: [Azure VM 'LERINDE SAP NetWeaver için yüksek kullanılabilirliğe sahıp RHEL](./high-availability-guide-rhel.md) ve [Azure vm 'ler için Azure NetApp Files ile RHEL](./high-availability-guide-rhel-netapp-files.md) için yüksek kullanılabilirlik, sıraya alma sunucusu çoğaltma 2 mimarisi için küme kısıtlamalarını ayarlama (ENSA2)
- 20 Şubat 2020: SUSE Multi-SID küme Kılavuzu 'na bir bağlantı eklemek için [SLES Multi-SID kılavuzundaki Azure VM 'LERINDE SAP NetWeaver Için yüksek kullanılabilirlik](./high-availability-guide-suse-multi-sid.md) olarak değiştirin
- 13 Şubat 2020: [Azure sanal makinelerinde yapılan DEĞIŞIKLIKLER SAP NetWeaver için planlama ve uygulama](./planning-guide.md) yeni belgelerin bağlantılarını uygulayacak
- 13 Şubat 2020: yeni belge [SAP iş yükünü Azure sanal makinesi tarafından desteklenen senaryoya](./sap-planning-supported-configurations.md) eklendi
- 13 Şubat 2020: yeni belge eklendi [Azure dağıtımı için desteklenen SAP yazılımı](./sap-supported-product-on-azure.md)
- 13 Şubat 2020: standart Azure Yük Dengeleyici ile genel uç noktaya erişimi açıklayan belgeyi işaret etmek için [Red Hat Enterprise Linux sunucusundaki Azure VM 'LERINDE IBM DB2 LUW 'ın yüksek kullanılabilirliğinde](./high-availability-guide-rhel-ibm-db2-luw.md) değişiklik yapın
- 13 Şubat 2020: yeni VM türlerini, [Microsoft Azure çalışan SAP sertifikalarına ve yapılandırmalarına](./sap-certifications.md) ekleyin
- 13 Şubat 2020: yeni SAP destek notları [SAP iş yüklerini Azure 'Da ekleme: planlama ve dağıtım denetim listesi](./sap-deployment-checklist.md)
- 13 Şubat 2020: [Azure VM 'LERINDE SAP NetWeaver için yüksek kullanılabilirliğe sahıp RHEL](./high-availability-guide-rhel.md) ve Azure VM 'leri, küme kaynaklarını Red Hat zaman aşımı önerilerine hizalamak için [Azure NetApp Files ile RHEL üzerinde SAP NetWeaver üzerinde](./high-availability-guide-rhel-netapp-files.md) yüksek kullanılabilirlik
- 11 Şubat 2020: Azure [sanal makinelerine Azure büyük örnek geçişi üzerinde SAP HANA](./hana-large-instance-virtual-machine-migration.md) sürümü
- 07 Şubat 2020: örnek NSG ekran görüntüsünü güncelleştirmek için [SAP ha senaryolarında Azure Standard ILB kullanarak VM 'ler Için genel uç nokta bağlantısında](./high-availability-guide-standard-load-balancer-outbound-connections.md) değişiklik yapın
- 03 Şubat 2020: SLES 'deki küme düğümlerinin ana bilgisayar adlarında Dash kullanma uyarısını kaldırmak üzere SAP uygulamaları için [SLES 'Deki Azure VM](./high-availability-guide-suse.md) 'lerinde SAP NW için yüksek kullanılabilirlik ve SAP [NW için yüksek](./high-availability-guide-suse-netapp-files.md) kullanılabilirlik olarak değiştirin
- 28 Ocak 2020: SAP HANA küme kaynaklarının zaman aşımlarını Red Hat zaman aşımı önerilerine hizalamak için [RHEL üzerindeki Azure VM 'lerinde SAP HANA yüksek kullanılabilirlik](./sap-hana-high-availability-rhel.md) olarak değiştirin
- 17 Ocak 2020: mevcut VM 'Leri bir yakınlık yerleşimi grubuna taşıma bölümünü değiştirmek için, [SAP uygulamalarıyla en iyi ağ gecikmesi Için Azure yakınlık yerleştirme gruplarında](./sap-proximity-placement-scenarios.md) değişiklik yapın
- 17 Ocak 2020: Kullanılabilirlik Alanları arasındaki gecikme sürelerini otomatikleştiren prosedürü işaret eden [Azure kullanılabilirlik alanları Ile SAP iş yükü yapılandırmalarında](./sap-ha-availability-zones.md) değiştirin
- 16 Ocak 2020: işletim sistemi sürümlerini HANA IaaS donanım dizinine uyarlamak için [Azure 'da SAP HANA (büyük örnekler) yüklemek ve yapılandırmak](./hana-installation.md) için değiştirin
- 16 Ocak 2020: SAP sistemlerine yönelik yönergeler eklemek için [SLES Multi-SID kılavuzundaki Azure VM 'LERINDE SAP NetWeaver Için yüksek kullanılabilirlik](./high-availability-guide-suse-multi-sid.md) değişiklikleri, sıraya alma sunucu 2 mımarısı (ENSA2) kullanma
- 10 Ocak 2020: değişiklikler kalıcı hale getirme hakkında yönergeler eklemek için [SLES üzerinde Azure NetApp Files](./sap-hana-scale-out-standby-netapp-files-suse.md) ve Azure VM 'lerinde bekleme düğümüyle birlikte SAP HANA ölçek genişletme [Azure NetApp Files SAP HANA](./sap-hana-scale-out-standby-netapp-files-rhel.md) değişiklikler `nfs4_disable_idmapping` .
- 10 Ocak 2020: [Azure VM 'lerinde SAP NetWeaver Için yüksek kullanılabilirlik](./high-availability-guide-suse-netapp-files.md) , SAP uygulamaları için Azure NetApp Files ve Azure sanal makinelerinde Azure NetApp Files NFSv4 birimleri bağlama hakkında yönergeler eklemek üzere SAP uygulamaları için [Azure NetApp Files ile yüksek kullanılabilirlik](./high-availability-guide-rhel-netapp-files.md) sağlar.
- 23 Aralık 2019: [SLES Multi-SID kılavuzundaki Azure VM 'LERINDE SAP NetWeaver Için yüksek kullanılabilirlik](./high-availability-guide-suse-multi-sid.md) yayını
- 18 Aralık 2019: [RHEL üzerinde Azure NetApp Files Ile Azure VM 'lerinde bekleme düğümüyle SAP HANA genişleme](./sap-hana-scale-out-standby-netapp-files-rhel.md) sürümü
- 21 Kasım 2019: NFS KIMLIĞI eşleme yapılandırmasını basitleştirmek ve yönlendirmeyi kolaylaştırmak için önerilen birincil ağ arabirimini değiştirmek üzere [SUSE Linux Enterprise Server Azure NetApp Files Ile Azure VM 'lerinde bekleme düğümüyle SAP HANA ölçek](./sap-hana-scale-out-standby-netapp-files-suse.md) genişletme içindeki değişiklikler.
- 15 Kasım 2019: SAP [NetWeaver Için yüksek kullanılabilirliğe sahip SUSE Linux Enterprise Server](high-availability-guide-suse-netapp-files.md) , SAP uygulamaları için Azure NetApp Files ve SAP [NetWeaver için yüksek kullanılabilirlik ile Azure NetApp Files Red Hat Enterprise Linux SAP uygulamaları](high-availability-guide-rhel-netapp-files.md) için, yalnızca NFSv3 sürümü desteklenen bir bildirim ve kaldırma ifadesini açıklığa kavuşturacak.
- 12 Kasım 2019: [Windows 'ta Azure NetApp Files (SMB) Ile SAP NetWeaver Için yüksek kullanılabilirlik](high-availability-guide-windows-netapp-files-smb.md) yayını
- 8 Kasım 2019: [SUSE Linux Enterprise Server Azure VM 'Lerinde yüksek kullanılabilirlik SAP HANA](sap-hana-high-availability.md)değişiklikler, [Azure sanal makinelerinde (VM) SAP HANA sistem çoğaltması ayarlama](sap-hana-high-availability-rhel.md), Azure [sanal makineler for SAP NETWEAVER için yüksek kullanılabilirlik, SAP uygulamaları için SUSE Linux Enterprise Server](high-availability-guide-suse.md)Azure [sanal makineler SUSE Linux Enterprise Server on Azure NetApp Files ile SAP NetWeaver için yüksek kullanılabilirlik](high-availability-guide-suse-netapp-files.md), [Azure sanal makineler for SAP NetWeaver for Red Hat ENTERPRISE LINUX](high-availability-guide-rhel.md), Azure [sanal makineleri Azure NetApp Files Ile Red Hat Enterprise Linux üzerinde SAP NetWeaver için](high-availability-guide-rhel-netapp-files.md)yüksek [kullanılabilirlik, Azure](high-availability-guide-suse-nfs.md)sanal makineler SUSE Linux Enterprise Server [için Red Hat Enterprise Linux](high-availability-guide-rhel-glusterfs.md)  
- 8 Kasım 2019: şifreleme önerisini netleştirmek için [SAP iş yükü planlama ve dağıtım denetim listesi](sap-deployment-checklist.md) değişiklikleri  
- 4 Kasım 2019: [Azure 'daki SUSE Linux Enterprise Server Paceyapıcısı ayarlama](high-availability-guide-suse-pacemaker.md) içindeki değişiklikler, tek noktaya yayın yapılandırmasıyla doğrudan küme oluşturmak için