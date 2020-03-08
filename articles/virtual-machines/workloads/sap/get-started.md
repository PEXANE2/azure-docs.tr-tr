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
ms.date: 03/05/2020
ms.author: juergent
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 4a02fb7ab7e42f2a56041638a3d883287da3eb42
ms.sourcegitcommit: bc792d0525d83f00d2329bea054ac45b2495315d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78674498"
---
# <a name="use-azure-to-host-and-run-sap-workload-scenarios"></a>SAP iş yükü senaryolarını barındırmak ve çalıştırmak için Azure kullanın

Microsoft Azure kullandığınızda, görev açısından kritik SAP iş yüklerinizi ve senaryolarını ölçeklenebilir, uyumlu ve kurumsal olarak kanıtlanmış bir platformda güvenilir bir şekilde çalıştırabilirsiniz. Azure 'un ölçeklenebilirlik, esneklik ve maliyet tasarrufu olanakları elde edersiniz. Microsoft ve SAP arasındaki genişletilmiş iş ortaklığı sayesinde, SAP uygulamalarını Azure 'daki geliştirme ve test ve üretim senaryolarında çalıştırabilir ve tam olarak desteklenmektedir. SAP NetWeaver ile SAP S/4HANA, Linux 'ta SAP BI ve Windows 'a SAP HANA, bu konu başlığı altında yer aldık.

Azure üzerinde farklı DBMS ile SAP NetWeaver senaryolarını barındırmanın yanı sıra Azure 'da SAP BI gibi diğer SAP iş yükü senaryolarını de barındırabilirsiniz. 

SAP HANA için Azure 'un benzersizliği, Azure 'ı ayrı ayarlayan bir tekliftir. Azure, SAP HANA içeren daha fazla bellek ve CPU kaynak gerektiren SAP senaryolarına olanak tanımak için, müşterinin adanmış çıplak donanımlarının kullanımını sağlar. S/4HANA veya diğer SAP HANA iş yükü için 24 TB 'a kadar (120-TB genişleme) bellek gerektiren SAP HANA dağıtımlarını çalıştırmak için bu çözümü kullanın. 

Azure 'da SAP iş yükü senaryolarını barındırmak, kimlik tümleştirmesi ve çoklu oturum açma gereksinimleri de oluşturabilir. Bu durum, farklı SAP bileşenleri ve hizmet olarak yazılım (SaaS) veya hizmet olarak platform (PaaS) tekliflerini bağlamak için Azure Active Directory (Azure AD) kullandığınızda ortaya çıkabilir. Azure AD ve SAP varlıklarıyla ilgili tümleştirme ve çoklu oturum açma senaryolarından oluşan bir liste, "AAD SAP kimlik tümleştirmesi ve çoklu oturum açma" bölümünde açıklanmaktadır ve belgelenmiştir.

## <a name="changes-to-the-sap-workload-section"></a>SAP iş yükü bölümünde yapılan değişiklikler
Azure iş yükünde SAP bölümündeki belgelerde yapılan değişiklikler bu makalenin sonunda listelenmiştir. Değişiklik günlüğündeki girişler 180 gün boyunca tutulur.

## <a name="you-want-to-know"></a>Bilmeniz istediğiniz
Belirli sorularınız varsa, başlangıç sayfasının bu bölümündeki belirli belgelere veya akışlara yöneltireceğiz. Şunları bildirmek istiyorsunuz:

- Hangi SAP yazılım sürümleri ve işletim sistemi sürümleri için desteklenen Azure VM 'Leri ve HANA büyük örnek birimleri. Yanıtlar için [Azure dağıtımı Için HANGI SAP yazılımlarının desteklendiğini](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-supported-product-on-azure) ve bilgileri bulma işlemini okuyun
- Azure VM 'Leri ve HANA büyük örneklerle desteklenen SAP dağıtım senaryoları. Desteklenen senaryolar hakkında daha fazla bilgi belgelerde bulunabilir:
    - [Azure sanal makinesi desteklenen senaryolarda SAP iş yükü](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-planning-supported-configurations)
    - [HANA büyük örneği için desteklenen senaryolar](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-supported-scenario)
- Azure Hizmetleri, Azure VM türleri ve Azure Storage farklı Azure bölgelerinde kullanılabilir, [bölgeye göre kullanılabilir site ürünlerini](https://azure.microsoft.com/global-infrastructure/services/) kontrol edin 

 
## <a name="sap-hana-on-azure-large-instances"></a>Azure’da SAP HANA (Büyük Örnekler)

Bir dizi belge, Azure 'daki SAP HANA (büyük örnekler) veya kısa, HANA büyük örnekleri için size yol gösterir. HANA büyük örnekler hakkında bilgi edinmek için, [Azure 'daki SAP HANA belge genel bakış ve mimarisiyle (büyük örnekler)](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-overview-architecture) başlar ve Hana büyük örnek bölümünde ilgili belgelerde ilerleyin



## <a name="sap-hana-on-azure-virtual-machines"></a>Azure sanal makineler 'de SAP HANA
Belgelerinin bu bölümü SAP HANA farklı yönlerini kapsamaktadır. Bir önkoşul olarak, Azure IaaS 'in temel hizmetlerini sağlayan Azure 'un asıl hizmetleri hakkında bilgi sahibi olmanız gerekir. Bu nedenle, Azure işlem, depolama ve ağ hakkında bilgi sahibi olmanız gerekir. Bu konuların birçoğu SAP NetWeaver ile ilgili [Azure planlama kılavuzunda](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/planning-guide)ele alınır. 

Azure 'da HANA hakkında daha fazla bilgi için aşağıdaki makalelere ve bunların alt makalelerine bakın:

- [Azure'da SAP HANA altyapısı yapılandırmaları ve işlemleri](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-vm-operations)
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
- [SAP iş yükü için IBM DB2 Azure Sanal Makineler DBMS dağıtımı](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/dbms_guide_ibm)
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
- [Öğretici: SAP NetWeaver ile tümleştirme Azure Active Directory](https://docs.microsoft.com/azure/active-directory/saas-apps/sap-netweaver-tutorial?toc=%2fazure%2fvirtual-machines%2fworkloads%2fsap%2ftoc.json)
- [Öğretici: SAP Business ByDesign ile Azure Active Directory tümleştirme](https://docs.microsoft.com/azure/active-directory/saas-apps/sapbusinessbydesign-tutorial?toc=%2fazure%2fvirtual-machines%2fworkloads%2fsap%2ftoc.json)
- [Öğretici: SAP HANA ile tümleştirme Azure Active Directory](https://docs.microsoft.com/azure/active-directory/saas-apps/saphana-tutorial?toc=%2fazure%2fvirtual-machines%2fworkloads%2fsap%2ftoc.json)
- [S/4HANA ortamınız: Fiori Launchpad SAML çoklu oturum açma Azure AD ile](https://blogs.sap.com/2017/02/20/your-s4hana-environment-part-7-fiori-launchpad-saml-single-sing-on-with-azure-ad/)

Azure hizmetlerini SAP bileşenleriyle tümleştirme hakkında daha fazla bilgi için bkz.:

- [Power BI Desktop’ta SAP HANA kullanma](https://docs.microsoft.com/power-bi/desktop-sap-hana)
- [DirectQuery ve SAP HANA](https://docs.microsoft.com/power-bi/desktop-directquery-sap-hana)
- [SAP BW Connector’ı Power BI Desktop’ta kullanma](https://docs.microsoft.com/power-bi/desktop-sap-bw-connector) 
- [Azure Data Factory, SAP HANA ve Business Warehouse veri tümleştirmesi sunuyor](https://azure.microsoft.com/blog/azure-data-factory-offer-sap-hana-and-business-warehouse-data-integration)


## <a name="change-log"></a>Değişiklik Günlüğü

- 03/05/2020: Azure sanal makinelerinde Azure bölgeleri ve Azure sanal makineleri için değişiklik ve içerik değişikliklerini, [SAP NetWeaver için planlama ve uygulamayı](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/planning-guide) yapılandırma
- 03/03/2020: SAP için [SLES 'Teki Azure VM 'LERINDE SAP NW 'yi](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse-netapp-files) daha verimli ANF birimi düzenine göre değiştirmek için yüksek kullanılabilirlik olarak değiştirin
- 03/01/2020: [Azure sanal makinelerinde SAP HANA Için Yedekleme kılavuzu](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-backup-guide) Azure Backup hizmeti içerecek şekilde yeniden çalıştı. [SAP HANA Azure Backup dosya düzeyinde](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-backup-file-level) daha az ve dar içerik ve disk anlık görüntüsü aracılığıyla yedekleme ile ilgili üçüncü bir belgeyi sildi. İçerik, Azure sanal makinelerinde SAP HANA için yedekleme kılavuzunda ele alınır 
- 02/27/2020: [SLES for SAP uygulamaları](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse)üzerindeki Azure VM 'lerinde SAP NW için yüksek kullanılabilirlik, SAP için [ANF](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse-netapp-files) ve [SLES Multi-SID kılavuzundaki](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse-multi-sid) Azure VM 'lerinde SAP NetWeaver için yüksek kullanılabilirlik, "başarısız" küme parametresini ayarlama
- 02/26/2020: Azure 'da HANA için dosya sistemi seçimini netleştirmek üzere [Azure sanal makine depolama yapılandırmalarının SAP HANA](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-vm-operations-storage) değiştirin
- 02/26/2020: [SAP Için yüksek kullanılabilirlik mimarisi ve senaryolarında](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-high-availability-architecture-scenarios) , RHEL çoklu SID kılavuzundaki Azure VM 'Lerinde SAP NETWEAVER için ha 'nın bağlantısını içermesini sağlamak için değiştirin
- 02/26/2020: [SLES for SAP Applications ÜZERINDE SAP NW Için yüksek kullanılabilirlik](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse)olarak değişiklik yapın ve SAP [uygulamaları için ANF ile SLES 'deki Azure VM 'lerinde SAP NW için](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse-netapp-files)yüksek kullanılabilirlik yapın, Azure VM 'leri, [RHEL üzerinde SAP NetWeaver Için yüksek](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-rhel) kullanılabilirlik ve Azure VM 'leri, çok düzeyli yoks/ers kümesinin desteklenmediğini belirten [Azure NetApp Files ile RHEL](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-rhel-netapp-files) üzerinde SAP NetWeaver için yüksek kullanılabilirlik
- 02/26/2020: SUSE Multi-SID küme Kılavuzu 'na bir bağlantı eklemek için [RHEL çoklu SID kılavuzundaki Azure VM 'LERINDE SAP NetWeaver Için yüksek kullanılabilirlik](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-rhel-multi-sid) yayını
- 02/25/2020: [SAP Için yüksek kullanılabilirlik mimarisi ve senaryolarında](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-high-availability-architecture-scenarios) , daha yeni ha makalelerine bağlantı eklemek için değiştirme
- 02/25/2020: standart Azure Yük Dengeleyici ile genel uç noktaya erişimi açıklayan belgeyi işaret etmek için [SUSE Linux Enterprise Server üzerindeki Azure VM 'LERINDE IBM DB2 LUW 'ın yüksek kullanılabilirliğinde](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/dbms-guide-ha-ibm) değişiklik yapın
- 02/21/2020: SAP [iş yükü Için SAP Ao Azure sanal MAKINELERI DBMS dağıtımı](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/dbms_guide_sapase) makalesini tamamen gözden geçirme
- 02/21/2020: [Azure sanal makine depolama yapılandırmasındaki SAP HANA](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-vm-operations-storage) ,/Hana/Data için Stripe boyutunda yeni öneriyi temsil edecek şekilde ve g/ç zamanlayıcı ayarı ekleyerek değiştirin
- 02/21/2020: S224 ve S224m 'in yeni sertifikalı SKU 'Larını temsil eden HANA büyük örnek belgelerindeki değişiklikler
- 02/21/2020: [Azure VM 'LERINDE SAP NetWeaver için yüksek kullanılabilirliğe sahıp RHEL](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-rhel) ve [Azure VM 'lerinde SAP NetWeaver](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-rhel-netapp-files) için yüksek kullanılabilirlik, sıraya alma sunucusu çoğaltma 2 mimarisi (ENSA2) için küme kısıtlamalarını ayarlamak için Azure NetApp Files
- 02/20/2020: SUSE Multi-SID küme Kılavuzu 'na bir bağlantı eklemek için [SLES Multi-SID kılavuzundaki Azure VM 'LERINDE SAP NetWeaver Için yüksek kullanılabilirlik](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse-multi-sid) olarak değiştirin
- 02/13/2020: [Azure sanal makinelerinde yapılan DEĞIŞIKLIKLER SAP NetWeaver için planlama ve uygulama](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/planning-guide) yeni belgelerin bağlantılarını uygulayacak
- 02/13/2020: [Azure sanal makinesi desteklenen bir senaryoda yeni belge SAP iş yükü](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-planning-supported-configurations) eklendi
- 02/13/2020: yeni belge eklendi [Azure dağıtımı için desteklenen SAP yazılımı](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-supported-product-on-azure)
- 02/13/2020: standart Azure Yük Dengeleyici ile genel uç noktaya erişimi açıklayan belgeyi işaret etmek için [Red Hat Enterprise Linux sunucusundaki Azure VM 'LERINDE IBM DB2 LUW 'ın yüksek kullanılabilirliğinde](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-rhel-ibm-db2-luw) değişiklik yapın
- 02/13/2020: yeni VM türlerini [SAP sertifikalarına ve Microsoft Azure üzerinde çalışan yapılandırmalara](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-certifications) ekleyin
- 02/13/2020: Azure 'da yeni SAP destek notları [SAP iş yükleri ekleme: planlama ve dağıtım denetim listesi](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-deployment-checklist)
- 02/13/2020: [Azure VM 'LERINDE SAP NetWeaver için yüksek kullanılabilirliğe sahıp RHEL](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-rhel) ve Azure VM 'leri, küme kaynaklarını Red Hat Timeout önerilerine hizalamak için [Azure NetApp Files ile RHEL üzerinde SAP NetWeaver için](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-rhel-netapp-files) yüksek kullanılabilirlik sağlar
- 02/11/2020: Azure [sanal makinelerine Azure büyük örnek geçişi üzerinde SAP HANA](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-large-instance-virtual-machine-migration) sürümü
- 02/07/2020: örnek NSG ekran görüntüsünü güncelleştirmek için [SAP ha senaryolarında Azure Standard ILB kullanarak VM 'ler Için genel uç nokta bağlantısında](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-standard-load-balancer-outbound-connections) değişiklik yapın
- 02/03/2020: SLES 'deki küme düğümlerinin ana bilgisayar adlarında Dash kullanma uyarısını kaldırmak üzere SAP uygulamaları için [SLES 'Deki Azure VM](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse) 'lerinde SAP NW 'de SAP NW için yüksek kullanılabilirlik ve [yüksek kullanılabilirlik](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse-netapp-files) olarak değiştirin
- 01/28/2020: SAP HANA küme kaynaklarını Red Hat zaman aşımı önerilerine hizalamak için [RHEL üzerindeki Azure VM 'lerinde SAP HANA yüksek kullanılabilirlik](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-high-availability-rhel) değişikliği yapın
- 01/17/2020: mevcut VM 'Leri bir yakınlık yerleşimi grubuna taşıma bölümünü değiştirmek için [SAP uygulamalarıyla en iyi ağ gecikmesi Için Azure yakınlık yerleştirme gruplarında](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-proximity-placement-scenarios) değişiklik yapın
- 01/17/2020: Kullanılabilirlik Alanları arasındaki gecikme sürelerini otomatikleştiren prosedürü işaret eden [Azure kullanılabilirlik alanları Ile SAP iş yükü yapılandırmalarında](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-ha-availability-zones) değiştirin
- 01/16/2020: işletim sistemi sürümlerini HANA IaaS donanım dizinine uyarlamak için [Azure 'da SAP HANA (büyük örnekler) yüklemek ve yapılandırmak](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-installation) için değiştirin
- 01/16/2020: [SLES çok düzeyli Azure VM 'lerinde SAP NetWeaver Için yüksek kullanılabilirliğe](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse-multi-sid) sahip değişiklikler sıraya alma sunucu 2 mımarısı (ENSA2) kullanarak SAP sistemlerine yönergeler eklemek için
- 01/10/2020: [Azure VM 'lerde, SLES üzerinde Azure NetApp Files olan SAP HANA ölçek genişletme Ile Azure VM](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-scale-out-standby-netapp-files-suse) 'lerinde değişiklik yapma ve `nfs4_disable_idmapping` değişikliklerin kalıcı hale getirme hakkında yönergeler eklemek için [rhel üzerinde Azure NetApp Files olan Azure](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-scale-out-standby-netapp-files-rhel) VM 'lerinde bekleyen bir düğüm ile SAP HANA ölçeği.
- 01/10/2020: SAP uygulamaları için Azure NetApp Files ve Azure sanal makinelerinde SAP [NetWeaver Için yüksek kullanılabilirliğe sahip olan Azure VM](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse-netapp-files) 'lerde, Azure NetApp Files NFSv4 birimleri bağlama hakkında yönergeler eklemek üzere SAP [uygulamaları için Azure NetApp Files olan RHEL 'de SAP NetWeaver 'ın yüksek kullanılabilirliği](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-rhel-netapp-files) .
- 12/23/2019: [SLES Multi-SID kılavuzundaki Azure VM 'LERINDE SAP NetWeaver Için yüksek kullanılabilirlik](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse-multi-sid) yayını
- 12/18/2019: [RHEL üzerinde Azure NetApp Files Ile Azure VM 'lerinde bekleme düğümüyle SAP HANA genişleme](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-scale-out-standby-netapp-files-rhel) sürümü
- 11/21/2019: NFS KIMLIĞI eşleme yapılandırmasını basitleştirmek ve yönlendirmeyi kolaylaştırmak için önerilen birincil ağ arabirimini değiştirmek üzere [SUSE Linux Enterprise Server Azure NetApp Files Ile Azure VM 'lerinde bekleme düğümüyle SAP HANA ölçek](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-scale-out-standby-netapp-files-suse) genişletme sırasında değişiklikler.
- 11/15/2019: SAP NetWeaver için [yüksek kullanılabilirliğe sahip SUSE Linux Enterprise Server, SAP uygulamaları için Azure NetApp Files](high-availability-guide-suse-netapp-files.md) ve SAP [NetWeaver için yüksek kullanılabilirliğe sahip Red Hat Enterprise Linux](high-availability-guide-rhel-netapp-files.md) SAP uygulamaları Için Azure NetApp Files ve yalnızca NFSv3 sürümünün desteklendiği ve kaldırma ifadesini açıklığa kavuşturacak şekilde destekler.
- 11/12/2019: [Azure NetApp Files (SMB) Ile Windows ÜZERINDE SAP NetWeaver Için yüksek kullanılabilirlik](high-availability-guide-windows-netapp-files-smb.md) yayını
- 11/08/2019: [SUSE Linux Enterprise Server üzerindeki Azure VM 'lerinde SAP HANA yüksek kullanılabilirlik](sap-hana-high-availability.md), [Azure sanal makinelerinde (VM) SAP HANA sistem çoğaltmasını ayarlama](sap-hana-high-availability-rhel.md), SAP [uygulamaları için SUSE Linux Enterprise Server üzerinde SAP NetWeaver için yüksek kullanılabilirlik](high-availability-guide-suse.md), Azure sanal makineler for the SUSE Linux Enterprise Server [on](high-availability-guide-suse-netapp-files.md)The SAP NetWeaver for the Azure NetApp Files [ ](high-availability-guide-rhel.md), Azure [sanal makineler Azure NetApp Files Red Hat ENTERPRISE LINUX on SAP NetWeaver için yüksek kullanılabilirlik](high-availability-guide-rhel-netapp-files.md), Azure [VM](high-availability-guide-suse-nfs.md)'lerinde Azure VM 'lerde yüksek kullanılabilirlik, Azure Standart yük dengeleyiciyi önermek için [SAP NetWeaver için Red Hat Enterprise Linux on](high-availability-guide-rhel-glusterfs.md) SUSE Linux Enterprise Server, Azure VM 'lerinde GlusterFS  
- 11/08/2019: şifreleme önerisini netleştirmek için [SAP iş yükü planlama ve dağıtım denetim listesi](sap-deployment-checklist.md) değişiklikleri  
- 11/04/2019: [Azure 'daki SUSE Linux Enterprise Server Paceyapıcısı ayarlama](high-availability-guide-suse-pacemaker.md) içindeki değişiklikler, tek noktaya yayın yapılandırmasıyla doğrudan küme oluşturmak için  
- 10/29/2019: [Azure Standart Load Balancer 'yi kullanarak sanal makineler Için genel uç nokta bağlantısının YAYıMLANMASı SAP yüksek kullanılabilirlik senaryolarında](high-availability-guide-standard-load-balancer-outbound-connections.md)
- 10/25/2019: [Azure sanal makine depolama yapılandırmalarının SAP HANA](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-vm-operations-storage) değişiklikler ve Azure VM 'lerinde,/Hana/Shared BIRIMI için NFS protokolünü açıklığa kavuşturmak için [SUSE Linux Enterprise Server Azure NetApp Files ile SAP HANA birlikte bekleme düğümüyle genişleme](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-scale-out-standby-netapp-files-suse)
- 10/22/2019: SAP [NetWeaver for Azure VM 'lerinde SAP Için yüksek kullanılabilirlik](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse)değişikliği, SAP uygulamaları için SUSE Linux Enterprise Server, SAP [uygulamaları için Azure NetApp Files ile SuSE Linux Enterprise Server üzerinde](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse-netapp-files)Azure VM 'lerde yüksek kullanılabilirlik, [SUSE Linux Enterprise Server üzerinde](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse-nfs)Azure VM 'lerde yüksek kullanılabilirlik, Azure ['daki SUSE Linux Enterprise Server](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse-pacemaker)Azure [VM](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/dbms-guide-ha-ibm)'lerinde yüksek kullanılabilirlik Azure yük dengeleyici algılama sağlamlaştırma için [SUSE Linux Enterprise Server Azure VM 'lerinde SAP HANA yüksek kullanılabilirliği](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-high-availability)
- [Azure sanal makine depolama yapılandırmalarında SAP HANA](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-vm-operations-storage) ANF bölümü ve üst bilgi bölümünü değiştirir
- 10/21/2019: [SLES 'de Azure NetApp Files Ile Azure VM 'lerinde bekleme düğümüyle SAP HANA genişleme](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-scale-out-standby-netapp-files-suse) sürümü
- 10/16/2019: [yedekleme ve geri yükleme](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-backup-restore) sırasında kopuk bağlantıları çözme
- 10/16/2019: WLES 12 SP3 'ün en düşük önerilen işletim sistemini, [SUSE Linux Enterprise Server pacemaker Ile Azure VM 'Lerinde yüksek kullanılabilirlik Ile](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/dbms-guide-ha-ibm) SLES 12 SP4 olarak değiştirin
- 10/11/2019: [SAP HANA Azure sanal makine depolama yapılandırmalarında](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-vm-operations-storage) , Ultra disk depolama yapılandırmalarının ve ANF 'ye giriş değişiklikleri
- 10/01/2019: daha fazla açıklık elde etmek için [SAP uygulamalarıyla en iyi ağ gecikmesi Için Azure yakınlık yerleştirme gruplarının](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-proximity-placement-scenarios) grafiklerde değişiklik yapın
- 10/01/2019: [Azure 'daki SAP HANA altyapı yapılandırmalarında ve işlemlerinde](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-vm-operations) ,/Hana/sharediçin yüksek düzeyde kullanılabilir NFS paylaşımındaki deyimleri düzeltmek üzere değiştirin. 
- 09/28/2019: RHEL kümelerinde, bir Chel kümelerinde SBD 'yi açıklığa kavuşturmaya yönelik olarak [Azure 'daki Red Hat Enterprise Linux Paceyapıcısı ayarlama](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-rhel-pacemaker) sırasında değişiklik  
- 09/17/2019: SAP için VM uzantısının etrafındaki terimleri bütünleştirme NetWeaver planlama ve dağıtım kılavuzunda değiştirin  
- 08/22/2019: özel rol oluşturmaya yönelik URL 'Leri güncelleştirmek için [Azure 'daki SUSE Linux Enterprise Server pacemaker ayarlama](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse-pacemaker) değişiklikleri  
- 08/16/2019: Azure ['da Red Hat Enterprise Linux](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-rhel-pacemaker) , Azure sınır aracısının yeni sürümüne güncelleştirilüyorsam, müşterilere özel roldeki eylemleri güncelleştirme  
- 08/15/2019: [SAP HANA Azure sanal makine depolama yapılandırmalarında](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-vm-operations-storage) , Ultra diskin genel kullanılabilirliğini yansıtacak değişiklikler (eski adıyla Ultra SSD)
- 08/01/2019: [Azure 'daki SUSE Linux Enterprise Server pacemaker 'ı ayarlamaya](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse-pacemaker) yönelik değişiklikler, SLES 15 için değişiklikleri özellikle tümleştirin 


