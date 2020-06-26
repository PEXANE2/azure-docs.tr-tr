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
ms.date: 06/24/2020
ms.author: juergent
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: fd170d505286d57f383af50da282e120167fe499
ms.sourcegitcommit: bf8c447dada2b4c8af017ba7ca8bfd80f943d508
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/25/2020
ms.locfileid: "85367368"
---
# <a name="use-azure-to-host-and-run-sap-workload-scenarios"></a>SAP iş yükü senaryolarını barındırmak ve çalıştırmak için Azure kullanın

Microsoft Azure kullandığınızda, görev açısından kritik SAP iş yüklerinizi ve senaryolarını ölçeklenebilir, uyumlu ve kurumsal olarak kanıtlanmış bir platformda güvenilir bir şekilde çalıştırabilirsiniz. Azure 'un ölçeklenebilirlik, esneklik ve maliyet tasarrufu olanakları elde edersiniz. Microsoft ve SAP arasındaki genişletilmiş iş ortaklığı sayesinde, SAP uygulamalarını Azure 'daki geliştirme ve test ve üretim senaryolarında çalıştırabilir ve tam olarak desteklenmektedir. SAP NetWeaver ile SAP S/4HANA, Linux 'ta SAP BI ve Windows 'a SAP HANA, bu konu başlığı altında yer aldık.

Azure üzerinde farklı DBMS ile SAP NetWeaver senaryolarını barındırmanın yanı sıra Azure 'da SAP BI gibi diğer SAP iş yükü senaryolarını de barındırabilirsiniz. 

SAP HANA için Azure 'un benzersizliği, Azure 'ı ayrı ayarlayan bir tekliftir. Azure, SAP HANA içeren daha fazla bellek ve CPU kaynak gerektiren SAP senaryolarına olanak tanımak için, müşterinin adanmış çıplak donanımlarının kullanımını sağlar. S/4HANA veya diğer SAP HANA iş yükü için 24 TB 'a kadar (120 TB genişleme) bellek gerektiren SAP HANA dağıtımlarını çalıştırmak için bu çözümü kullanın. 

Azure 'da SAP iş yükü senaryolarını barındırmak, kimlik tümleştirmesi ve çoklu oturum açma gereksinimleri de oluşturabilir. Bu durum, farklı SAP bileşenleri ve hizmet olarak yazılım (SaaS) veya hizmet olarak platform (PaaS) tekliflerini bağlamak için Azure Active Directory (Azure AD) kullandığınızda ortaya çıkabilir. Azure AD ve SAP varlıklarıyla ilgili tümleştirme ve çoklu oturum açma senaryolarından oluşan bir liste, "AAD SAP kimlik tümleştirmesi ve çoklu oturum açma" bölümünde açıklanmaktadır ve belgelenmiştir.

## <a name="changes-to-the-sap-workload-section"></a>SAP iş yükü bölümünde yapılan değişiklikler
Azure iş yükünde SAP bölümündeki belgelerde yapılan değişiklikler bu makalenin sonunda listelenmiştir. Değişiklik günlüğündeki girişler 180 gün boyunca tutulur.

## <a name="you-want-to-know"></a>Bilmeniz istediğiniz
Belirli sorularınız varsa, başlangıç sayfasının bu bölümündeki belirli belgelere veya akışlara yöneltireceğiz. Şunları bildirmek istiyorsunuz:

- Hangi SAP yazılım sürümleri ve işletim sistemi sürümleri için desteklenen Azure VM 'Leri ve HANA büyük örnek birimleri. Yanıtlar için [Azure dağıtımı Için HANGI SAP yazılımlarının desteklendiğini](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-supported-product-on-azure) ve bilgileri bulma işlemini okuyun
- Azure VM 'Leri ve HANA büyük örneklerle desteklenen SAP dağıtım senaryoları. Desteklenen senaryolar hakkında daha fazla bilgi belgelerde bulunabilir:
    - [Azure sanal makinesi üzerinde SAP iş yüküne yönelik desteklenen senaryolar](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-planning-supported-configurations)
    - [HANA büyük örneği için desteklenen senaryolar](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-supported-scenario)
- Azure Hizmetleri, Azure VM türleri ve Azure Depolama Hizmetleri farklı Azure bölgelerinde kullanılabilir, [bölgeye göre kullanılabilir site ürünlerini](https://azure.microsoft.com/global-infrastructure/services/) kontrol edin 
- Windows ve Paceyapıcısı 'nin desteklenme yanı sıra üçüncü taraf HA çerçevesi çalışıyor mu? [Sap destek notunun](https://launchpad.support.sap.com/#/notes/1928533) alt kısmına bakın #1928533

 
## <a name="sap-hana-on-azure-large-instances"></a>Azure’da SAP HANA (Büyük Örnekler)

Bir dizi belge, Azure 'daki SAP HANA (büyük örnekler) veya kısa, HANA büyük örnekleri için size yol gösterir. HANA büyük örnekleri hakkında daha fazla bilgi için, [Azure 'da SAP HANA belge genel bakış ve mimarisine (büyük örnekler)](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-overview-architecture) BAŞLAYıN ve Hana büyük örnek bölümündeki ilgili belgelere gidin



## <a name="sap-hana-on-azure-virtual-machines"></a>Azure sanal makineler 'de SAP HANA
Belgelerinin bu bölümü SAP HANA farklı yönlerini kapsamaktadır. Bir önkoşul olarak, Azure IaaS 'in temel hizmetlerini sağlayan Azure 'un asıl hizmetleri hakkında bilgi sahibi olmanız gerekir. Bu nedenle, Azure işlem, depolama ve ağ hakkında bilgi sahibi olmanız gerekir. Bu konuların birçoğu SAP NetWeaver ile ilgili [Azure planlama kılavuzunda](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/planning-guide)ele alınır. 

Azure 'da HANA hakkında daha fazla bilgi için aşağıdaki makalelere ve bunların alt makalelerine bakın:

- [Azure'da SAP HANA altyapı yapılandırmaları ve işlemleri](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-vm-operations)
- [Azure sanal makineleri için yüksek kullanılabilirlik SAP HANA](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-availability-overview)
- [Azure sanal makinelerinde SAP HANA yüksek kullanılabilirliği](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-high-availability)
- [Azure sanal makinelerinde SAP HANA için yedekleme Kılavuzu](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-backup-guide)


 

## <a name="sap-netweaver-deployed-on-azure-virtual-machines"></a>Azure sanal makinelerinde dağıtılan SAP NetWeaver
Bu bölümde, Azure 'da SAP NetWeaver ve Iş için planlama ve dağıtım belgeleri listelenmiştir. Belgeler, Azure 'da SAP iş yükünün temel alınarak ve HANA olmayan veritabanlarının kullanımına odaklanmaktadır. Yüksek kullanılabilirlik için belgeler ve makaleler Ayrıca, Azure 'da HANA yüksek kullanılabilirliğe yönelik temel bir temelidir; örneğin:

- [Azure Planlama Kılavuzu](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/planning-guide). 
- [Azure sanal makinelerde SAP Business One](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/business-one-azure)
- [Site Recovery kullanarak çok katmanlı SAP NetWeaver uygulama dağıtımını koruma](https://docs.microsoft.com/azure/site-recovery/site-recovery-sap)
- [Azure için SAP LaMa bağlayıcısı](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/lama-installation)

Azure 'da bir SAP iş yükü altında HANA olmayan veritabanları hakkında bilgi için bkz.:

- [SAP iş yükü için Azure sanal makineler DBMS dağıtımına yönelik konular](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/dbms_guide_general)
- [SAP NetWeaver için Azure sanal makineler DBMS dağıtımı SQL Server](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/dbms_guide_sqlserver)
- [SAP iş yükü için Oracle Azure Sanal Makineler DBMS dağıtımı](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/dbms_guide_oracle)
- [IBM DB2 Azure sanal makineleri SAP iş yükü için DBMS dağıtımı](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/dbms_guide_ibm)
- [SAP iş yükü için SAP ASE Azure Sanal Makineler DBMS dağıtımı](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/dbms_guide_sapase)
- [Azure VM 'lerinde SAP MaxDB, canlı önbellek ve Içerik sunucusu dağıtımı](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/dbms_guide_maxdb)

Azure 'daki SAP HANA veritabanları hakkında daha fazla bilgi için, "Azure sanal makinelerinde SAP HANA" bölümüne bakın.

Azure 'da SAP iş yükünün yüksek kullanılabilirliği hakkında daha fazla bilgi için bkz.:

- [Azure sanal makineler SAP NetWeaver için yüksek kullanılabilirlik](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-high-availability-guide-start)

Bu belge, diğer çeşitli mimari ve senaryo belgelerini gösterir. Sonraki senaryo belgelerinde, farklı yüksek kullanılabilirliğe sahip yöntemlerin dağıtımını ve yapılandırmasını açıklayan ayrıntılı teknik belgelerin bağlantıları verilmiştir. SAP NetWeaver iş yükü için Linux ve Windows işletim sistemleri için yüksek kullanılabilirlik oluşturmayı ve yapılandırmayı gösteren farklı belgeler.


Azure Active Directory (Azure AD) ile SAP Hizmetleri ve çoklu oturum açma arasında tümleştirme hakkında daha fazla bilgi için bkz.:

- [Öğretici: müşteri için SAP bulutu ile Azure Active Directory tümleştirme](https://docs.microsoft.com/azure/active-directory/saas-apps/sap-customer-cloud-tutorial?toc=%2fazure%2fvirtual-machines%2fworkloads%2fsap%2ftoc.json)
- [Öğretici: SAP Cloud Platform Identity Authentication ile tümleştirme Azure Active Directory](https://docs.microsoft.com/azure/active-directory/saas-apps/sap-hana-cloud-platform-identity-authentication-tutorial?toc=%2fazure%2fvirtual-machines%2fworkloads%2fsap%2ftoc.json)
- [Öğretici: SAP Cloud Platform ile tümleştirme Azure Active Directory](https://docs.microsoft.com/azure/active-directory/saas-apps/sap-hana-cloud-platform-tutorial?toc=%2fazure%2fvirtual-machines%2fworkloads%2fsap%2ftoc.json)
- [Öğretici: SAP NetWeaver ile Azure Active Directory tümleştirme](https://docs.microsoft.com/azure/active-directory/saas-apps/sap-netweaver-tutorial?toc=%2fazure%2fvirtual-machines%2fworkloads%2fsap%2ftoc.json)
- [Öğretici: SAP Business ByDesign ile Azure Active Directory tümleştirme](https://docs.microsoft.com/azure/active-directory/saas-apps/sapbusinessbydesign-tutorial?toc=%2fazure%2fvirtual-machines%2fworkloads%2fsap%2ftoc.json)
- [Öğretici: SAP HANA ile tümleştirme Azure Active Directory](https://docs.microsoft.com/azure/active-directory/saas-apps/saphana-tutorial?toc=%2fazure%2fvirtual-machines%2fworkloads%2fsap%2ftoc.json)
- [S/4HANA ortamınız: Fiori Launchpad SAML çoklu oturum açma Azure AD ile](https://blogs.sap.com/2017/02/20/your-s4hana-environment-part-7-fiori-launchpad-saml-single-sing-on-with-azure-ad/)

Azure hizmetlerini SAP bileşenleriyle tümleştirme hakkında daha fazla bilgi için bkz.:

- [Power BI Desktop'ta SAP HANA Kullanma](https://docs.microsoft.com/power-bi/desktop-sap-hana)
- [DirectQuery ve SAP HANA](https://docs.microsoft.com/power-bi/desktop-directquery-sap-hana)
- [Power BI Desktop'ta SAP BW Bağlayıcısı'nı Kullanma](https://docs.microsoft.com/power-bi/desktop-sap-bw-connector) 
- [Azure Data Factory, SAP HANA ve Business Warehouse veri tümleştirmesi sunar](https://azure.microsoft.com/blog/azure-data-factory-offer-sap-hana-and-business-warehouse-data-integration)


## <a name="change-log"></a>Değişiklik Günlüğü

- 06/24/2020: Azure 'da, Azure sınır aracısına bağlı yeni geliştirilmiş Azure sınır Aracısı ve cihazlar için daha dayanıklı STONITH yapılandırması yayınlamak üzere [Azure 'DA SLES 'de pacemaker ayarlama](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse-pacemaker) sırasında değişiklik yapın 
- 06/24/2020: [Azure 'daki RHEL üzerinde Paceyapıcısı ayarlama](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-rhel-pacemaker) sırasında değişiklik yaparken daha dayanıklı bir yapılandırma yayınlanın
- 06/23/2020: SAP [NetWeaver Guide planlama ve uygulama Için Azure sanal makinelerinde](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/planning-guide) yapılan DEĞIŞIKLIKLER ve [SAP iş yükü Kılavuzu için Azure depolama türlerini](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/planning-guide-storage) giriş
- 06/22/2020: [dağıtım KıLAVUZUNA](deployment-guide.md) SAP IÇIN yeni VM uzantısı için yükleme adımları ekleyin
- 06/16/2020: SUSE genel bulut altyapısı 101 belgelerine bir bağlantı eklemek için [SAP ha senaryolarında Azure Standard ILB kullanarak VM 'ler Için genel uç nokta bağlantısında](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-standard-load-balancer-outbound-connections) değişiklik yapın 
- 06/10/2020: HLI ve [SAP HANA (büyük örnekler) depolama mimarisi](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-storage-architecture) [için kullanılabilir SKU](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-available-skus) 'Lara yeni hLi SKU 'lar ekleme
- 05/21/2020: Azure ['DA SLES 'de Paceyapıcısı ayarlama](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse-pacemaker) ve [SAP ha senaryolarında Azure Standard ILB kullanarak VM 'ler için genel uç nokta bağlantısına](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-standard-load-balancer-outbound-connections) bir bağlantı eklemek için [Azure 'Daki RHEL 'de paceyapıcısı](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-rhel-pacemaker) ayarlama sırasında değişiklik yapın  
- 05/19/2020: [SAP HANA Azure sanal makine depolama yapılandırmalarında](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-vm-operations-storage) , Hana ile ilgili birimler için LVM kullanırken kök birim grubunu kullanmak için önemli bir ileti ekleyin
- 05/19/2020: [HANA büyük örnekler için uyumlu Işletim sistemleri] içinde HANA büyük örnek türü II için yeni desteklenen işletim sistemi ekleyin (https://docs.microsoft.com/
- Azure/sanal makineler/iş yükleri/sap/işletim sistemi-uyumluluk-matris-Hana-büyük-örnek)
- 05/12/2020: bağlantıları güncelleştirmek ve üçüncü taraf güvenlik duvarı yapılandırması için bilgi eklemek üzere [SAP ha senaryolarında Azure Standard ILB kullanarak VM 'ler Için genel uç nokta bağlantısında](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-standard-load-balancer-outbound-connections) değişiklik yapın
- 05/11/2020: [SLES 'Teki Azure VM 'lerinde SAP HANA yüksek kullanılabilirlik](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-high-availability) düzeyinde değişiklik yaparken, Netcat kaynağı için kaynak sayısını 0 olarak ayarlayın. Bu, daha kolay yük devretmeye yol açar 
- 05/05/2020: [Azure sanal makinelerindeki DEĞIŞIKLIKLER SAP NetWeaver için planlama ve uygulama;](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/planning-guide) bu Gen2 DAĞıTıMLARıNıN Mv1 VM ailesi için kullanılabilir olduğunu ifade ediyor
- 04/24/2020: [SLES 'de ANF Ile Azure VM 'lerinde bekleyen bir düğüm ile SAP HANA genişleme](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-scale-out-standby-netapp-files-suse)değişiklikleri Azure VM 'lerinde NetWeaver on [RHEL ile Azure](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-scale-out-standby-netapp-files-rhel)VM 'lerinde, [SLES](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse-netapp-files) 'deki Azure VM 'LERINDE, ANF birimleri için IP adreslerinin otomatik olarak atandığını açıklama eklemek için ANF ile Azure VM 'lerinde yüksek kullanılabilirlik ve [yüksek](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-rhel-netapp-files) kullanılabilirlik içeren SAP HANA.
- 04/22/2020: [SLES 'Teki Azure VM 'lerinde SAP HANA yüksek kullanılabilirlik](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-high-availability) olarak değişiklik `is-managed` , kümeyi bakım moduna alma veya hizmetten çıkarma konusunda çakışmalar nedeniyle meta özniteliği yönergelerden kaldırma
- 04/21/2020: SQL Azure DB, SAP için desteklenen DBMS (Hybrsıs) Commerce platform 1811 ve üzeri sürümlerde Azure dağıtımları ve [SAP Sertifikaları ve Microsoft Azure üzerinde çalışan yapılandırma](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-certifications) [için SAP yazılımının desteklendiği](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-supported-product-on-azure) makalelerde eklenmiştir
- 04/16/2020: Azure dağıtımları için desteklenen SAP (Hybrsıs) ticaret platformu olarak SAP HANA eklendi ve [Microsoft Azure üzerinde çalışan SAP Sertifikaları ve yapılandırmaların](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-certifications) [desteklendiği yazılım](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-supported-product-on-azure)
- 04/13/2020: SAP asa Azure sanal makineler için tam SAP asa sürüm numaralarına, [SAP iş yükü IÇIN DBMS dağıtımına](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/dbms_guide_sapase) doğru
- 04/07/2020: Cloud-netconfig-Azure yönergelerini açıklığa kavuşturacak [Azure 'DA SLES 'de Paceyapıcısı ayarlama](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse-pacemaker) sırasında değişiklik yapın
- 04/06/2020: [Azure VM 'lerinde, SLES üzerinde Azure NetApp Files olan SAP HANA ölçek](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-scale-out-standby-netapp-files-suse) genişletme ve Azure VM 'lerinde Azure NetApp Files olan Azure sanal makinelerinde bulunan ve [olan Azure VM 'lerinde genişleme düğümü ile SAP HANA](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-scale-out-standby-netapp-files-rhel) değişiklik, netapp [tr-](https://www.netapp.com/us/media/tr-4746.pdf) [4746](https://www.netapp.com/us/media/tr-4746.pdf)4435 başvurularını kaldırmak
- 03/31/2020: [Azure VM 'Lerde yüksek kullanılabilirlik üzerinde SAP HANA](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-high-availability) ve Azure VM 'lerde yüksek kullanılabilirlik, şeritli birimler oluştururken Stripe boyutunun nasıl belirtilmesinde yönergeler eklemek için [rhel üzerinde SAP HANA](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-high-availability-rhel)
- 03/27/2020: dosya sistemi bağlama seçeneklerini NetApp TR-4746 (eşitleme bağlama seçeneğini kaldır) olarak hizalamak için [SLES 'Deki Azure VM 'LERINDE SAP NW Için yüksek kullanılabilirlik](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse-netapp-files) olarak değiştirin
- 03/26/2020: [SLES çok düzeyli kılavuzdaki Azure VM 'LERINDE SAP NetWeaver Için yüksek kullanılabilirlik](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse-multi-sid) değişikliği yapın NetApp TR-4746 başvurusu eklemek için
- 03/26/2020: SLES for SAP Applications üzerinde SAP [NetWeaver Için yüksek kullanılabilirlik](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse)olarak değişiklik yapın ve SAP [uygulamaları için Azure NetApp Files, SLES 'teki Azure VM 'lerinde SAP NetWeaver için yüksek kullanılabilirlik](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse-netapp-files)yapın SLES [High availability for NFS on Azure VMs on SLES](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse-nfs)'de Azure VM 'lerde yüksek KULLANıLABILIRLIK, [SLES Multi-SID kılavuzundaki](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse-multi-sid)Azure VM 'lerinde SAP NetWeaver için yüksek kullanılabilirlik, SAP uygulamaları için [RHEL 'de Azure](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-rhel) VM 'Lerde yüksek kullanılabilirlik ve Azure VM 'lerinde SAP NetWeaver için [yüksek](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-rhel-netapp-files) kullanılabilirlik ve arka uç havuzu oluşturma yönergelerini açıklığa kavuşturma Azure NetApp Files Azure Load Balancer
- 03/19/2020: belge hızlı başlangıcı 'nın ana düzeltmesi: Azure sanal makinelerine [SAP HANA yüklemek](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-get-started) Için [Azure sanal makinelerinde tek örnekli SAP HANA el ile yükleme](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-get-started)
- 03/17/2020: artık gerekli olmayan SBD yapılandırma ayarını kaldırmak için [Azure 'daki SUSE Linux Enterprise Server Paceyapıcısı ayarlama](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse-pacemaker) sırasında değişiklik
- 03/16/2020: [Azure dağıtımları için desteklenen SAP yazılımının desteklendiği](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-supported-product-on-azure) SAP HANA IaaS sertifikalı platformda sütun sertifikasyon senaryosunun açıklaması
- 03/11/2020: DBMS örnek desteği başına birden çok veritabanını netleştirmek için [Azure sanal makinesi desteklenen senaryolarda SAP iş](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-planning-supported-configurations) yükünde değişiklik yapın
- 03/11/2020:1. nesil ve 2. nesil VM 'Ler [Için Azure sanal makinelerinde planlama ve uygulama NetWeaver](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/planning-guide)
- 03/10/2020: ANF 'nin gerçek mevcut aktarım hızı sınırlarını netleştirmek için [SAP HANA Azure sanal makine depolama yapılandırmalarında](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-vm-operations-storage) değiştirin
- 03/09/2020: SAP [NetWeaver for azure SUSE Linux Enterprise Server VM 'lerinde SAP Için yüksek kullanılabilirlik](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse)değişikliği, SAP [uygulamaları için Azure NetApp Files Ile SUSE Linux Enterprise Server Azure VM 'lerinde SAP NetWeaver için yüksek kullanılabilirlik](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse-netapp-files), [SUSE LINUX ENTERPRISE Server üzerindeki Azure VM 'lerinde NFS için yüksek kullanılabilirlik](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse-nfs), [azure 'da SUSE Linux Enterprise Server üzerinde paceyapıcısı](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse-pacemaker) [ayarlama,](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-high-availability) Azure vm 'lerinde yüksek kullanılabilirliğe sahip Azure sanal makinelerinde SUSE Linux Enterprise Server, [Azure](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/dbms-guide-ha-ibm)VM 'lerinde yüksek kullanılabilirlik ve kaynak Aracısı Azure-lb ile küme kaynaklarını güncelleştirmek için [yüksek kullanılabilirlik SUSE Linux Enterprise Server](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse-multi-sid) SAP HANA 
- 03/05/2020: Azure sanal makinelerinde Azure bölgeleri ve Azure sanal makineleri için değişiklik ve içerik değişikliklerini, [SAP NetWeaver için planlama ve uygulamayı](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/planning-guide) yapılandırma
- 03/03/2020: SAP için [SLES 'Teki Azure VM 'LERINDE SAP NW 'yi](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse-netapp-files) daha verimli ANF birimi düzenine göre değiştirmek için yüksek kullanılabilirlik olarak değiştirin
- 03/01/2020: [Azure sanal makinelerinde SAP HANA Için Yedekleme kılavuzu](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-backup-guide) Azure Backup hizmeti içerecek şekilde yeniden çalıştı. [SAP HANA Azure Backup dosya düzeyinde](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-backup-file-level) daha az ve dar içerik ve disk anlık görüntüsü aracılığıyla yedekleme ile ilgili üçüncü bir belgeyi sildi. İçerik, Azure sanal makinelerinde SAP HANA için yedekleme kılavuzunda ele alınır 
- 27 Şubat 2020: [SLES for SAP uygulamaları](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse)üzerindeki Azure VM 'lerinde SAP NW için yüksek kullanılabilirlik, SAP için [ANF](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse-netapp-files) ve [SLES Multi-SID kılavuzundaki](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse-multi-sid) Azure VM 'lerinde SAP NetWeaver için yüksek kullanılabilirlik, "başarısız" küme parametresini ayarlama
- 26 Şubat 2020: Azure 'da HANA için dosya sistemi seçimini netleştirmek üzere [Azure sanal makine depolama yapılandırmalarında SAP HANA](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-vm-operations-storage) değiştirin
- 26 Şubat 2020: [SAP Için yüksek kullanılabilirlik mimarisi ve senaryolarında](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-high-availability-architecture-scenarios) , RHEL çoklu SID kılavuzundaki Azure VM 'Lerinde SAP NETWEAVER için ha bağlantısını ekleme
- 26 Şubat 2020: [SLES for SAP uygulamaları ÜZERINDE SAP NW Için yüksek kullanılabilirlik](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse)olarak değişiklik yapın ve SAP uygulamaları için [ANF ile SLES 'deki Azure VM 'lerinde SAP NW için yüksek](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse-netapp-files)kullanılabilirlik, [Azure VM 'leri, RHEL üzerinde SAP NetWeaver Için yüksek](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-rhel) kullanılabilirlik ve [Azure VM](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-rhel-netapp-files) 'LERI, birden çok sıd yoks/ers kümesinin desteklenmediğini belirten Azure NetApp Files
- 26 Şubat 2020: SUSE Multi-SID küme Kılavuzu 'na bir bağlantı eklemek için [RHEL çoklu SID kılavuzundaki Azure VM 'LERINDE SAP NetWeaver Için yüksek kullanılabilirlik](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-rhel-multi-sid) yayını
- 02/25/2020: [SAP Için yüksek kullanılabilirlik mimarisi ve senaryolarında](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-high-availability-architecture-scenarios) , daha yeni ha makalelerine bağlantı eklemek için değiştirme
- 25 Şubat 2020: standart Azure Yük Dengeleyici ile genel uç noktaya erişimi açıklayan belgeyi işaret etmek için [SUSE Linux Enterprise Server üzerindeki Azure VM 'LERINDE IBM DB2 LUW 'ın yüksek kullanılabilirliğinde](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/dbms-guide-ha-ibm) değişiklik yapın
- 21 Şubat 2020: [Azure sanal makineler ve SAP iş yükü Için Azure sanal MAKINELERI DBMS dağıtımı](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/dbms_guide_sapase) makalesini tamamen gözden geçirme
- 21 Şubat 2020: [SAP HANA Azure sanal makine depolama yapılandırması](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-vm-operations-storage) 'nda,/Hana/Data için şerit boyutundaki yeni öneriyi ve g/ç zamanlayıcı ayarını ekleyerek değiştirin
- 21 Şubat 2020: S224 ve S224m ' nin yeni sertifikalı SKU 'Larını temsil etmesi için HANA büyük örnek belgelerindeki değişiklikler
- 21 Şubat 2020: [Azure VM 'LERINDE SAP NetWeaver için yüksek kullanılabilirliğe sahıp RHEL](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-rhel) ve [Azure vm 'ler için Azure NetApp Files ile RHEL](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-rhel-netapp-files) için yüksek kullanılabilirlik, sıraya alma sunucusu çoğaltma 2 mimarisi için küme kısıtlamalarını ayarlama (ENSA2)
- 20 Şubat 2020: SUSE Multi-SID küme Kılavuzu 'na bir bağlantı eklemek için [SLES Multi-SID kılavuzundaki Azure VM 'LERINDE SAP NetWeaver Için yüksek kullanılabilirlik](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse-multi-sid) olarak değiştirin
- 13 Şubat 2020: [Azure sanal makinelerinde yapılan DEĞIŞIKLIKLER SAP NetWeaver için planlama ve uygulama](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/planning-guide) yeni belgelerin bağlantılarını uygulayacak
- 13 Şubat 2020: yeni belge [SAP iş yükünü Azure sanal makinesi tarafından desteklenen senaryoya](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-planning-supported-configurations) eklendi
- 13 Şubat 2020: yeni belge eklendi [Azure dağıtımı için desteklenen SAP yazılımı](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-supported-product-on-azure)
- 13 Şubat 2020: standart Azure Yük Dengeleyici ile genel uç noktaya erişimi açıklayan belgeyi işaret etmek için [Red Hat Enterprise Linux sunucusundaki Azure VM 'LERINDE IBM DB2 LUW 'ın yüksek kullanılabilirliğinde](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-rhel-ibm-db2-luw) değişiklik yapın
- 13 Şubat 2020: yeni VM türlerini, [Microsoft Azure çalışan SAP sertifikalarına ve yapılandırmalarına](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-certifications) ekleyin
- 13 Şubat 2020: yeni SAP destek notları [SAP iş yüklerini Azure 'Da ekleme: planlama ve dağıtım denetim listesi](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-deployment-checklist)
- 13 Şubat 2020: [Azure VM 'LERINDE SAP NetWeaver için yüksek kullanılabilirliğe sahıp RHEL](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-rhel) ve Azure VM 'leri, küme kaynaklarını Red Hat zaman aşımı önerilerine hizalamak için [Azure NetApp Files ile RHEL üzerinde SAP NetWeaver üzerinde](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-rhel-netapp-files) yüksek kullanılabilirlik
- 11 Şubat 2020: Azure [sanal makinelerine Azure büyük örnek geçişi üzerinde SAP HANA](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-large-instance-virtual-machine-migration) sürümü
- 07 Şubat 2020: örnek NSG ekran görüntüsünü güncelleştirmek için [SAP ha senaryolarında Azure Standard ILB kullanarak VM 'ler Için genel uç nokta bağlantısında](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-standard-load-balancer-outbound-connections) değişiklik yapın
- 03 Şubat 2020: SLES 'deki küme düğümlerinin ana bilgisayar adlarında Dash kullanma uyarısını kaldırmak üzere SAP uygulamaları için [SLES 'Deki Azure VM](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse) 'lerinde SAP NW için yüksek kullanılabilirlik ve SAP [NW için yüksek](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse-netapp-files) kullanılabilirlik olarak değiştirin
- 28 Ocak 2020: SAP HANA küme kaynaklarının zaman aşımlarını Red Hat zaman aşımı önerilerine hizalamak için [RHEL üzerindeki Azure VM 'lerinde SAP HANA yüksek kullanılabilirlik](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-high-availability-rhel) olarak değiştirin
- 17 Ocak 2020: mevcut VM 'Leri bir yakınlık yerleşimi grubuna taşıma bölümünü değiştirmek için, [SAP uygulamalarıyla en iyi ağ gecikmesi Için Azure yakınlık yerleştirme gruplarında](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-proximity-placement-scenarios) değişiklik yapın
- 17 Ocak 2020: Kullanılabilirlik Alanları arasındaki gecikme sürelerini otomatikleştiren prosedürü işaret eden [Azure kullanılabilirlik alanları Ile SAP iş yükü yapılandırmalarında](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-ha-availability-zones) değiştirin
- 16 Ocak 2020: işletim sistemi sürümlerini HANA IaaS donanım dizinine uyarlamak için [Azure 'da SAP HANA (büyük örnekler) yüklemek ve yapılandırmak](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-installation) için değiştirin
- 16 Ocak 2020: SAP sistemlerine yönelik yönergeler eklemek için [SLES Multi-SID kılavuzundaki Azure VM 'LERINDE SAP NetWeaver Için yüksek kullanılabilirlik](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse-multi-sid) değişiklikleri, sıraya alma sunucu 2 mımarısı (ENSA2) kullanma
- 10 Ocak 2020: değişiklikler kalıcı hale getirme hakkında yönergeler eklemek için [SLES üzerinde Azure NetApp Files](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-scale-out-standby-netapp-files-suse) ve Azure VM 'lerinde bekleme düğümüyle birlikte SAP HANA ölçek genişletme [Azure NetApp Files SAP HANA](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-scale-out-standby-netapp-files-rhel) değişiklikler `nfs4_disable_idmapping` .
- 10 Ocak 2020: [Azure VM 'lerinde SAP NetWeaver Için yüksek kullanılabilirlik](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse-netapp-files) , SAP uygulamaları için Azure NetApp Files ve Azure sanal makinelerinde Azure NetApp Files NFSv4 birimleri bağlama hakkında yönergeler eklemek üzere SAP uygulamaları için [Azure NetApp Files ile yüksek kullanılabilirlik](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-rhel-netapp-files) sağlar.
- 23 Aralık 2019: [SLES Multi-SID kılavuzundaki Azure VM 'LERINDE SAP NetWeaver Için yüksek kullanılabilirlik](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse-multi-sid) yayını
- 18 Aralık 2019: [RHEL üzerinde Azure NetApp Files Ile Azure VM 'lerinde bekleme düğümüyle SAP HANA genişleme](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-scale-out-standby-netapp-files-rhel) sürümü
- 21 Kasım 2019: NFS KIMLIĞI eşleme yapılandırmasını basitleştirmek ve yönlendirmeyi kolaylaştırmak için önerilen birincil ağ arabirimini değiştirmek üzere [SUSE Linux Enterprise Server Azure NetApp Files Ile Azure VM 'lerinde bekleme düğümüyle SAP HANA ölçek](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-scale-out-standby-netapp-files-suse) genişletme içindeki değişiklikler.
- 15 Kasım 2019: SAP [NetWeaver Için yüksek kullanılabilirliğe sahip SUSE Linux Enterprise Server](high-availability-guide-suse-netapp-files.md) , SAP uygulamaları için Azure NetApp Files ve SAP [NetWeaver için yüksek kullanılabilirlik ile Azure NetApp Files Red Hat Enterprise Linux SAP uygulamaları](high-availability-guide-rhel-netapp-files.md) için, yalnızca NFSv3 sürümü desteklenen bir bildirim ve kaldırma ifadesini açıklığa kavuşturacak.
- 12 Kasım 2019: [Windows 'ta Azure NetApp Files (SMB) Ile SAP NetWeaver Için yüksek kullanılabilirlik](high-availability-guide-windows-netapp-files-smb.md) yayını
- 8 Kasım 2019: [SUSE Linux Enterprise Server Azure VM 'Lerinde yüksek kullanılabilirlik SAP HANA](sap-hana-high-availability.md)değişiklikler, [Azure sanal makinelerinde (VM) SAP HANA sistem çoğaltması ayarlama](sap-hana-high-availability-rhel.md), Azure [sanal makineler for SAP NETWEAVER için yüksek kullanılabilirlik, SAP uygulamaları için SUSE Linux Enterprise Server](high-availability-guide-suse.md)Azure [sanal makineler SUSE Linux Enterprise Server on Azure NetApp Files ile SAP NetWeaver için yüksek kullanılabilirlik](high-availability-guide-suse-netapp-files.md), [Azure sanal makineler for SAP NetWeaver for Red Hat ENTERPRISE LINUX](high-availability-guide-rhel.md), Azure [sanal makineleri Azure NetApp Files Ile Red Hat Enterprise Linux üzerinde SAP NetWeaver için](high-availability-guide-rhel-netapp-files.md)yüksek [kullanılabilirlik, Azure](high-availability-guide-suse-nfs.md)sanal makineler SUSE Linux Enterprise Server [için Red Hat Enterprise Linux](high-availability-guide-rhel-glusterfs.md)  
- 8 Kasım 2019: şifreleme önerisini netleştirmek için [SAP iş yükü planlama ve dağıtım denetim listesi](sap-deployment-checklist.md) değişiklikleri  
- 4 Kasım 2019: [Azure 'daki SUSE Linux Enterprise Server Paceyapıcısı ayarlama](high-availability-guide-suse-pacemaker.md) içindeki değişiklikler, tek noktaya yayın yapılandırmasıyla doğrudan küme oluşturmak için  


