---
title: Azure VM'lerde SAP ile başlayın | Microsoft Dokümanlar
description: Microsoft Azure'da sanal makinelerde (VM) çalışan SAP çözümleri hakkında bilgi edinin
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
ms.date: 04/06/2020
ms.author: juergent
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: df7461443fc9e89209545e8a2dfb9ad0addf3f3e
ms.sourcegitcommit: 441db70765ff9042db87c60f4aa3c51df2afae2d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2020
ms.locfileid: "80757143"
---
# <a name="use-azure-to-host-and-run-sap-workload-scenarios"></a>SAP iş yükü senaryolarını barındırmak ve çalıştırmak için Azure'u kullanın

Microsoft Azure'u kullandığınızda, görev açısından kritik SAP iş yüklerinizi ve senaryolarınızı ölçeklenebilir, uyumlu ve kurumsal olarak kanıtlanmış bir platformda güvenilir bir şekilde çalıştırabilirsiniz. Azure'un ölçeklenebilirliğini, esnekliğini ve maliyet tasarruflarını elde elabilirsiniz. Microsoft ve SAP arasındaki genişletilmiş ortaklıkla, SAP uygulamalarını Azure'da geliştirme, test ve üretim senaryolarında çalıştırabilir ve tam olarak destekleyebilirsiniz. SAP NetWeaver'dan SAP S/4HANA'ya, SAP BI Linux'tan Windows'a ve SAP HANA'dan SQL'e kadar sizi kapsıyoruz.

Azure'daki farklı DBMS ile SAP NetWeaver senaryolarını barındırmanın yanı sıra, Azure'daki SAP BI gibi diğer SAP iş yükü senaryolarını da barındırabilirsiniz. 

SAP HANA için Azure'un benzersizliği, Azure'u diğerlerinden ayıran bir tekliftir. Daha fazla bellek ve SAP HANA içeren CPU kaynak gerektiren SAP senaryolarının barındırılmasını etkinleştirmek için Azure, müşteriye özel çıplak metal donanım kullanımını sunar. S/4HANA veya diğer SAP HANA iş yükü için 24 TB'a (120-TB ölçeklendirme) kadar bellek gerektiren SAP HANA dağıtımlarını çalıştırmak için bu çözümü kullanın. 

AZURE'da SAP iş yükü senaryolarını barındırmak, kimlik tümleştirmesi ve tek oturum açma gereksinimleri de oluşturabilir. Bu durum, farklı SAP bileşenlerini ve HIZMET olarak SAP yazılımlarını (SaaS) veya hizmet olarak platform (PaaS) tekliflerini bağlamak için Azure Active Directory'yi (Azure AD) kullandığınızda oluşabilir. Azure AD ve SAP varlıklarıyla bu tür tümleştirme ve tek oturum açma senaryolarının listesi "AAD SAP kimlik tümleştirmesi ve tek oturum açma" bölümünde açıklanır ve belgelenir.

## <a name="changes-to-the-sap-workload-section"></a>SAP iş yükü bölümündeki değişiklikler
Azure iş yükü bölümünde SAP'deki belgelerdeki değişiklikler bu makalenin sonunda listelenir. Değişiklik günlüğündeki girişler yaklaşık 180 gün saklanır.

## <a name="you-want-to-know"></a>Bilmek istiyorsun.
Belirli sorularınız varsa, başlangıç sayfasının bu bölümünde sizi belirli belgelere veya akışlara yönlendireceğiz. Bilmek istiyorsun:

- Hangi Azure VM'leri ve HANA Büyük Örnek birimleri hangi SAP yazılımının yayımladığı ve hangi işletim sistemi sürümleri için desteklenir. Belgeyi okuyun Yanıtlar ve bilgileri bulma işlemi [için Azure dağıtımı için](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-supported-product-on-azure) desteklenen SAP yazılımı
- Hangi SAP dağıtım senaryoları Azure VM'leri ve HANA Büyük Örnekleri ile desteklenir. Desteklenen senaryolar hakkındaki bilgiler belgelerde bulunabilir:
    - [Azure sanal makine destekli senaryolarda SAP iş yükü](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-planning-supported-configurations)
    - [HANA Büyük Örnek için desteklenen senaryolar](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-supported-scenario)
- Farklı Azure bölgelerinde hangi Azure Hizmetleri, Azure VM türleri ve Azure depolama hizmetleri [kullanılabilir, bölgeye göre kullanılabilen](https://azure.microsoft.com/global-infrastructure/services/) site Ürünlerini kontrol edin 

 
## <a name="sap-hana-on-azure-large-instances"></a>Azure’da SAP HANA (Büyük Örnekler)

Bir dizi belge sizi Azure'da SAP HANA 'ya (Büyük Örnekler) veya kısaca HANA Büyük Örnekleri'ne yönlendirir. HANA Büyük Örnekleri hakkında bilgi için [BELGE Genel Bakış ve Azure SAP HANA mimarisi ile başlar (Büyük Örnekler)](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-overview-architecture) ve HANA Büyük Örnek bölümünde ilgili belgeler üzerinden gidin



## <a name="sap-hana-on-azure-virtual-machines"></a>Azure sanal makinelerde SAP HANA
Belgelerin bu bölümü SAP HANA'nın farklı yönlerini kapsamaktadır. Ön koşul olarak, Azure IaaS'ın temel hizmetlerini sağlayan Azure'un temel hizmetlerini biliyor olmalısınız. Bu nedenle, Azure bilgi işlem, depolama ve ağ bilgisine ihtiyacınız var. Bu konuların çoğu SAP NetWeaver ile ilgili [Azure planlama kılavuzunda](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/planning-guide)işlenir. 

Azure'da HANA hakkında bilgi için aşağıdaki makalelere ve alt makalelerine bakın:

- [Azure'da SAP HANA altyapı yapılandırmaları ve işlemleri](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-vm-operations)
- [Azure sanal makineleri için SAP HANA yüksek kullanılabilirlik](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-availability-overview)
- [Azure sanal makinelerde SAP HANA'nın yüksek kullanılabilirliği](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-high-availability)
- [Azure sanal makinelerde SAP HANA için yedekleme kılavuzu](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-backup-guide)


 

## <a name="sap-netweaver-deployed-on-azure-virtual-machines"></a>SAP NetWeaver Azure sanal makinelerde dağıtıldı
Bu bölümde, SAP NetWeaver ve Business One için Azure'daki planlama ve dağıtım belgeleri listelenmiştir. Belgeler, Azure'da SAP iş yüküne sahip HANA olmayan veritabanlarının temellerine ve kullanımına odaklanır. Yüksek kullanılabilirlik için belgeler ve makaleler de Azure'da HANA yüksek kullanılabilirlik için temel vardır, gibi:

- [Azure planlama kılavuzu.](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/planning-guide) 
- [Azure sanal makinelerde SAP Business One](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/business-one-azure)
- [Site Kurtarma'yı kullanarak çok katmanlı SAP NetWeaver uygulama dağıtımını koruyun](https://docs.microsoft.com/azure/site-recovery/site-recovery-sap)
- [Azure için SAP LaMa bağlayıcısı](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/lama-installation)

Azure'daki BIR SAP iş yükü altında HANA olmayan veritabanları hakkında bilgi için bkz:

- [SAP iş yükü için Azure Sanal Makineler DBMS dağıtımında dikkat edilmesi gerekenler](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/dbms_guide_general)
- [SAP NetWeaver için SQL Server Azure Sanal Makineler DBMS dağıtımı](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/dbms_guide_sqlserver)
- [SAP iş yükü için Oracle Azure Sanal Makineler DBMS dağıtımı](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/dbms_guide_oracle)
- [SAP iş yükü için IBM DB2 Azure Sanal Makineler DBMS dağıtımı](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/dbms_guide_ibm)
- [SAP iş yükü için SAP ASE Azure Sanal Makineler DBMS dağıtımı](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/dbms_guide_sapase)
- [Azure VM'lerde SAP MaxDB, Canlı Önbellek ve İçerik Sunucusu dağıtımı](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/dbms_guide_maxdb)

Azure'daki SAP HANA veritabanları hakkında bilgi için "Azure sanal makinelerde SAP HANA" bölümüne bakın.

Azure'da bir SAP iş yükünün yüksek kullanılabilirliği hakkında bilgi için bkz:

- [SAP NetWeaver için Azure Sanal Makineler yüksek kullanılabilirlik](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-high-availability-guide-start)

Bu belge, diğer çeşitli mimari ve senaryo belgelerine işaret ediyor. Daha sonraki senaryo belgelerinde, farklı yüksek kullanılabilirlik yöntemlerinin dağıtımını ve yapılandırmasını açıklayan ayrıntılı teknik belgelere bağlantılar sağlanır. SAP NetWeaver iş yükü için yüksek kullanılabilirlik nasıl oluşturulup yapılandırılabildiğini gösteren farklı belgeler Linux ve Windows işletim sistemlerini kapsar.


Azure Etkin Dizin (Azure AD) ve SAP hizmetleri ve tek oturum açma arasındaki tümleştirme hakkında bilgi için bkz:

- [Öğretici: Müşteri için SAP Bulutile Azure Active Directory entegrasyonu](https://docs.microsoft.com/azure/active-directory/saas-apps/sap-customer-cloud-tutorial?toc=%2fazure%2fvirtual-machines%2fworkloads%2fsap%2ftoc.json)
- [Öğretici: SAP Bulut Platformu Kimlik Kimlik Doğrulaması ile Azure Active Directory entegrasyonu](https://docs.microsoft.com/azure/active-directory/saas-apps/sap-hana-cloud-platform-identity-authentication-tutorial?toc=%2fazure%2fvirtual-machines%2fworkloads%2fsap%2ftoc.json)
- [Öğretici: SAP Bulut Platformu ile Azure Active Directory entegrasyonu](https://docs.microsoft.com/azure/active-directory/saas-apps/sap-hana-cloud-platform-tutorial?toc=%2fazure%2fvirtual-machines%2fworkloads%2fsap%2ftoc.json)
- [Öğretici: SAP NetWeaver ile Azure Active Directory tümleştirme](https://docs.microsoft.com/azure/active-directory/saas-apps/sap-netweaver-tutorial?toc=%2fazure%2fvirtual-machines%2fworkloads%2fsap%2ftoc.json)
- [Öğretici: SAP Business ByDesign ile Azure Active Directory tümleştirme](https://docs.microsoft.com/azure/active-directory/saas-apps/sapbusinessbydesign-tutorial?toc=%2fazure%2fvirtual-machines%2fworkloads%2fsap%2ftoc.json)
- [Öğretici: SAP HANA ile Azure Active Directory entegrasyonu](https://docs.microsoft.com/azure/active-directory/saas-apps/saphana-tutorial?toc=%2fazure%2fvirtual-machines%2fworkloads%2fsap%2ftoc.json)
- [S/4HANA ortamınız: Azure AD ile Fiori Launchpad SAML tek oturum açma](https://blogs.sap.com/2017/02/20/your-s4hana-environment-part-7-fiori-launchpad-saml-single-sing-on-with-azure-ad/)

Azure hizmetlerinin SAP bileşenlerine entegrasyonu hakkında bilgi için bkz:

- [Power BI Desktop'ta SAP HANA Kullanma](https://docs.microsoft.com/power-bi/desktop-sap-hana)
- [DirectQuery ve SAP HANA](https://docs.microsoft.com/power-bi/desktop-directquery-sap-hana)
- [Power BI Desktop'ta SAP BW Bağlayıcısı'nı Kullanma](https://docs.microsoft.com/power-bi/desktop-sap-bw-connector) 
- [Azure Data Factory, SAP HANA ve Business Warehouse veri tümleştirmesi sunuyor](https://azure.microsoft.com/blog/azure-data-factory-offer-sap-hana-and-business-warehouse-data-integration)


## <a name="change-log"></a>Değişiklik Günlüğü
- 04/06/2020: NetApp [TR-4435'e](https://www.netapp.com/us/media/tr-4746.pdf) yapılan başvuruları kaldırmak için [SLES'teki Azure NetApp Dosyaları ile Azure VM'lerde](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-scale-out-standby-netapp-files-suse) ve [RHEL'deki Azure NetApp Dosyaları ile SAP HANA ölçeğinde standby düğümlü](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-scale-out-standby-netapp-files-rhel) SAP HANA ölçeğindeki değişiklikler [(TR-4746](https://www.netapp.com/us/media/tr-4746.pdf)ile değiştirildi)
- 31/03/2020: Çizgili hacimler oluştururken şerit boyutunu nasıl belirteceğinize ilişkin talimatlar eklemek için [SLES'teki Azure VM'lerde SAP HANA'nın Yüksek kullanılabilirlik durumu](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-high-availability) ve [RHEL'deki Azure VM'lerde SAP HANA'nın yüksek kullanılabilirliği](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-high-availability-rhel)
- 27/03/2020: Dosya sistemi montaj seçeneklerini NetApp TR-4746 ile hizalamak [için SLES'teki Azure VM'lerde SAP NW'nin YÜKSEK kullanılabilirlik teki değişikliği, SAP uygulamaları için ANF](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse-netapp-files) ile (eşitleme montaj seçeneğini kaldırın)
- 26.03.2020: NetApp TR-4746'ya referans eklemek [için SLES multi-SID kılavuzunda Azure VM'lerde SAP NetWeaver](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse-multi-sid) için yüksek kullanılabilirlik değişikliği
- 26/03/2020: [SAP uygulamaları için SLES'te Azure VM'lerde SAP NetWeaver](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse)için yüksek kullanılabilirlik değişikliği , SAP uygulamaları için Azure [NetApp Dosyalarında Azure VM'lerde SAP NetWeaver için yüksek kullanılabilirlik](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse-netapp-files), [SLES'te Azure VM'lerde NFS'ler için yüksek kullanılabilirlik](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse-nfs), [SAP için yüksek kullanılabilirlik SLES multi-SID kılavuzunda Azure VM'lerde NetWeaver](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse-multi-sid), [SAP uygulamaları için RHEL'de Azure VM'lerde SAP NetWeaver için yüksek kullanılabilirlik](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-rhel) ve [SAP Uygulamaları için Azure NetApp Dosyaları ile RHEL'de AZURE NetWeaver için yüksek kullanılabilirlik](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-rhel-netapp-files) diyagramları güncellemek ve Azure Yük Bakiyesi arka uç havuzu oluşturma yönergelerini netleştirmek için
- 03/19/2020: Quickstart belgesinin büyük [revizyonu: Azure Sanal Makinelerde TEK ÖRNEKSAP HANA'nın](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-get-started) [Azure Sanal Makinelerde SAP HANA'nın Azure Sanal Makinelere Kurulumu](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-get-started) için manuel olarak kurulumu
- 03/17/2020: Artık gerekli olmayan SBD yapılandırma ayarını kaldırmak için [Azure'da SUSE Linux Enterprise Server'da Pacemaker](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse-pacemaker) Kurulumu nda Değişiklik
- 03/16/2020: Sap HANA IaaS sertifikalı platformda, Azure [dağıtımları için hangi SAP yazılımının desteklenirse](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-supported-product-on-azure) sütun sertifikasenaryosunun açıklanması
- 03/11/2020: DBMS örnek desteği başına birden fazla veritabanını netleştirmek için [Azure sanal makine destekli senaryolarda SAP iş yükündeki](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-planning-supported-configurations) değişiklik
- 03/11/2020: Nesil 1 ve Nesil 2 VM'leri açıklayan [SAP NetWeaver için Azure Sanal Makineler planlama](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/planning-guide) ve uygulamasında değişiklik
- 03/10/2020: ANF'nin gerçek mevcut iş verme limitlerini netleştirmek için [SAP HANA Azure sanal makine depolama yapılandırmalarında](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-vm-operations-storage) değişiklik
- 03/09/2020: [SAP uygulamaları için SUSE Linux Enterprise Server'da Azure VM'lerde SAP NetWeaver](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse)için yüksek kullanılabilirlik değişikliği , SAP uygulamaları için Azure [NetApp Dosyaları ile Azure VM'lerde SAP NetWeaver için yüksek kullanılabilirlik](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse-netapp-files), [SUSE Linux Enterprise Server'da Azure VM'lerde NFS için yüksek kullanılabilirlik , SUSE Linux Enterprise Server'da NFS için yüksek kullanılabilirlik](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse-nfs), [Ayarlama Azure'da SUSE Linux Enterprise Server'da Pacemaker](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse-pacemaker), [Ibm Db2 LUW'un Azure VM'lerde Pacemaker ile Yüksek kullanılabilirliği](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/dbms-guide-ha-ibm), [SUSE Linux Enterprise Server'da Azure VM'lerde SAP HANA'nın yüksek kullanılabilirliği](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-high-availability) ve [SLES multi-SID kılavuzunda SLES multi-SID kılavuzunda SAP NetWeaver için yüksek kullanılabilirlik](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse-multi-sid) 
- 03/05/2020: Azure Bölgeleri ve Azure Sanal makineleri için sap [NetWeaver için planlama ve uygulama değişiklikleri ve](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/planning-guide) Azure Sanal makineleri için yapı değişiklikleri
- 03/03/2020: Daha verimli ANF birim düzenine geçiş için [SAP uygulamaları için ANF ile SLES'teki Azure VM'lerde SAP NW'nin yüksek kullanılabilirlik](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse-netapp-files) değişimi
- 03/01/2020: [Azure Sanal Makinelerde SAP HANA için](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-backup-guide) Azure Yedekleme hizmetini içerecek şekilde Yeniden Düzenlenmiş Yedekleme kılavuzu. [SAP HANA Azure Yedekleme'de dosya düzeyinde](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-backup-file-level) azaltılmış ve yoğunlaştırılmış içerik ve disk anlık görüntüsü aracılığıyla yedekleme yle ilgili üçüncü bir belge yisildi. Azure Sanal Makinelerde SAP HANA yedekleme kılavuzunda içerik ele alınır 
- 27/02/2020: [SLES'teki Azure VM'lerde SAP NW'nin SAP uygulamalarında yüksek kullanılabilirlik](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse)değişimi, [SAP uygulamaları için ANF ile SLES'te Azure VM'lerde SAP NW](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse-netapp-files) için yüksek kullanılabilirlik ve [SLES multi-SID kılavuzunda SLES multi-SID kılavuzunda SAP NetWeaver için yüksek kullanılabilirlik](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse-multi-sid) "başarısız" kümesi parametresini ayarlamak için
- 26.02.2020: AZURE'da HANA için dosya sistemi seçimini netleştirmek için [SAP HANA Azure sanal makine depolama yapılandırmalarında](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-vm-operations-storage) değişiklik
- 26/02/2020: RHEL multi-SID kılavuzunda Azure VM'lerde SAP NetWeaver için HA bağlantısını içerecek şekilde [SAP için yüksek kullanılabilirlik mimarisi](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-high-availability-architecture-scenarios) ve senaryolarında değişiklik
- 26/02/2020: [SAP uygulamaları için SLES'teki Azure VM'lerde SAP NW'nin yüksek kullanılabilirlik](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse)değişimi , SAP uygulamaları için [ANF ile SLES'te SAP NW](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse-netapp-files)için yüksek [kullanılabilirlik, RHEL'de SAP NetWeaver için Azure VM'ler yüksek kullanılabilirlik](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-rhel) ve [Azure NetApp Dosyaları ile RHEL'de SAP NetWeaver için Azure NetApp'in yüksek kullanılabilirliği, multi-SID](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-rhel-netapp-files) ASCS/ERS kümekümesinin desteklenmediğini niçin kaldırma
- 26/02/2020: SUSE multi-SID küme kılavuzuna bağlantı eklemek [için RHEL multi-SID kılavuzunda Azure VM'lerde SAP NetWeaver için yüksek kullanılabilirlik](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-rhel-multi-sid) serbest bırakılması
- 25/02/2020: Sap'nin yeni HA makalelerine bağlantılar eklemek [için yüksek kullanılabilirlik mimarisi ve senaryolarında](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-high-availability-architecture-scenarios) değişiklik
- 25/02/2020: Standart Azure Yük dengeleyicisi ile ortak uç noktaya erişimi açıklayan belgeye işaret etmek için [SUSE Linux Enterprise Server'daki Azure VM'lerde IBM Db2 LUW'un Yüksek kullanılabilirlik durumu](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/dbms-guide-ha-ibm) değişikliği
- 21.02.2020: SAP iş [yükü için SAP ASE Azure Sanal Makineler DBMS dağıtımı](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/dbms_guide_sapase) makalesinin tam revizyonu
- 21/02/2020: [SAP HANA Azure sanal makine depolama](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-vm-operations-storage) yapılandırmasında/hana/veri için şerit boyutunda yeni bir öneriyi temsil edecek ve G/Ç zamanlayıcısının ayarını ekleme de
- 21.02.2020: HANA Büyük Örnek belgelerinde S224 ve S224m'nin yeni onaylı SK'larını temsil edecek değişiklikler
- 21/02/2020: ENqueue sunucu çoğaltma 2 mimarisi (ENSA2) mimarisi için küme kısıtlamalarını ayarlamak [için Azure NetApp Dosyaları ile RHEL'deki SAP NetWeaver](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-rhel-netapp-files) için RHEL ve Azure VM'lerde SAP NetWeaver için Yüksek Kullanılabilirlik Azure [VM'lerde değişiklik](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-rhel)
- 20/02/2020: SUSE multi-SID küme kılavuzuna bağlantı eklemek [için SLES multi-SID kılavuzunda Azure VM'lerde SAP NetWeaver](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse-multi-sid) için yüksek kullanılabilirlik değişikliği
- 02/13/2020: [SAP NetWeaver'ın](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/planning-guide) yeni belgelere bağlantılar uygulaması için Azure Sanal Makineleri planlaması ve uygulamasında yapılan değişiklikler
- 02/13/2020: Azure [sanal makine destekli senaryoda](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-planning-supported-configurations) yeni belge SAP iş yükü eklendi
- 13.02.2020: Yeni belge eklendi [Hangi SAP yazılımı Azure dağıtımı için desteklenir](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-supported-product-on-azure)
- 02/13/2020: Standart Azure Yük dengeleyicisi ile ortak bitiş noktasına erişimi açıklayan belgeye işaret etmek için [Red Hat Enterprise Linux Server'daki Azure VM'lerde IBM Db2 LUW'un yüksek kullanılabilirlik](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-rhel-ibm-db2-luw) durumu değişikliği
- 13/02/2020: Microsoft Azure'da [çalışan SAP sertifikalarına ve yapılandırmalarına](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-certifications) yeni VM türlerini ekleyin
- 02/13/2020: Azure'a yeni SAP destek notları ekleyin [SAP iş yükleri: planlama ve dağıtım denetim listesi](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-deployment-checklist)
- 02/13/2020: Küme kaynakları zaman larını Red Hat zaman önerileriyle hizalamak [için RHEL'deki SAP NetWeaver](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-rhel-netapp-files) için YÜKSEK Kullanılabilirlik RHEL ve Azure VM'ler için [YÜKSEK Kullanılabilirlik](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-rhel)
- 02/11/2020: [AZURE Büyük Örnek geçişinde SAP HANA'nın Azure Sanal Makinelere geçişinde](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-large-instance-virtual-machine-migration) serbest bırakılması
- 02/07/2020: Örnek NSG ekran görüntüsünü güncelleştirmek [için SAP HA senaryolarında Azure Standart ILB kullanan VM'ler için Genel uç nokta bağlantısında](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-standard-load-balancer-outbound-connections) değişiklik
- 02/03/2020: [SLES'teki Azure VM'lerde SAP NW'nin YÜKSEK kullanılabilirlik değişimi](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse) ve [SLES'teki SLES'teki Azure VM'lerde SAP NW için YÜKSEK kullanılabilirlik ve SAP uygulamaları için ANF ile SLES'te](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse-netapp-files) SAP uygulamaları için yüksek kullanılabilirlik, SLES'teki küme düğümlerinin ana bilgisayar adlarında tire kullanma uyarısını kaldırmak için
- 28/01/2020: SAP HANA küme kaynakları zaman larını Red Hat zaman önerileriyle hizalamak için [RHEL'deki Azure VM'lerde SAP HANA'nın yüksek kullanılabilirlik durumu](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-high-availability-rhel) değişikliği
- 01/17/2020: Sap [uygulamalarıyla en iyi ağ gecikmesi için Azure yakınlık yerleşim gruplarında](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-proximity-placement-scenarios) değişiklik yaparak mevcut VM'leri bir yakınlık yerleşim grubuna taşıma bölümünü değiştirme
- 01/17/2020: Kullanılabilirlik Bölgeleri arasındaki gecikme ölçümlerini otomatikleştiren yordamı işaret etmek için [Azure Kullanılabilirlik Bölgeleri ile SAP iş yükü yapılandırmalarında](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-ha-availability-zones) değişiklik
- 01/16/2020: OS sürümlerini HANA IaaS donanım dizinine uyarlamak için [Azure'da SAP HANA'nın (Büyük Örnekler) nasıl yüklenir ve yapılandırılabilen](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-installation)
- 01/16/2020: Enqueue server 2 mimarisini kullanarak SAP sistemleri için yönergeler eklemek için [SLES multi-SID kılavuzunda Azure VM'lerde SAP NetWeaver](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse-multi-sid) için yüksek kullanılabilirlik değişiklikleri
- 01/10/2020: [SLES'teki Azure NetApp Dosyaları ile Azure VM'lerde ve](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-scale-out-standby-netapp-files-suse) [RHEL'deki Azure NetApp Dosyaları ile Azure VM'lerde bekleme düğümü yle SAP HANA ölçeğindeki](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-scale-out-standby-netapp-files-rhel) değişiklikler, değişikliklerin nasıl kalıcı hale getirileceğe `nfs4_disable_idmapping` ilişkin talimatlar eklemek için.
- 01/10/2020: [SAP uygulamaları için Azure NetApp Dosyaları ile SLES'teki Azure VM'lerde SAP NetWeaver'ın yüksek kullanılabilirlik](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse-netapp-files) teki değişiklikler ve Azure Sanal [Makineler'de, SAP uygulamaları için Azure NetApp Dosyaları ile RHEL'deki SAP NetWeaver için yüksek kullanılabilirlik, Azure](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-rhel-netapp-files) NetApp Files NFSv4 ciltlerinin nasıl monte edileceğine ilişkin talimatlar eklemek için.
- 23.12.2019: [SLES multi-SID kılavuzunda AZURE VM'lerde SAP NetWeaver için yüksek kullanılabilirlik](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse-multi-sid) sürümü
- 12/18/2019: [RHEL'deki Azure NetApp Dosyaları ile Azure VM'lerde bekleme düğümü yle SAP HANA ölçeklendirmesinin](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-scale-out-standby-netapp-files-rhel) serbest bırakılması
- 21/11/2019: NFS ID eşleleştirme yapılandırmasını basitleştirmek ve yönlendirmeyi kolaylaştırmak için önerilen birincil ağ arabirimini değiştirmek için [SUSE Linux Enterprise Server'daki Azure NetApp Dosyaları ile Azure VM'lerde bekleme düğümü yle SAP HANA ölçeğindeki](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-scale-out-standby-netapp-files-suse) değişiklikler.
- 11/15/2019: Kapasite havuzu boyutu kısıtlamalarını netleştirmek ve yalnızca NFSv3 sürümünün desteklendirilmiş olduğu bildirimini kaldırmak için SAP uygulamaları için Red Hat Enterprise Linux'ta SAP [NetApp Dosyaları ile SUSE Linux Enterprise Server'da SAP NetWeaver](high-availability-guide-suse-netapp-files.md) için yüksek kullanılabilirlik temalı küçük değişiklikler ve [SAP uygulamaları için SUSE Enterprise Linux'ta Yüksek kullanılabilirlik.](high-availability-guide-rhel-netapp-files.md)
- 11/12/2019: [Azure NetApp Dosyaları (SMB) ile Windows'da SAP NetWeaver için Yüksek kullanılabilirlik](high-availability-guide-windows-netapp-files-smb.md) sürümü
- 11/08/2019: [SUSE Linux Enterprise Server'da Azure VM'lerde SAP HANA'nın yüksek kullanılabilirlik](sap-hana-high-availability.md)değişiklikleri, Azure sanal makinelerde SAP HANA Sistem Çoğaltma 'nı [ayarlama (VM'ler)](sap-hana-high-availability-rhel.md), [SAP uygulamaları için SUSE Linux Enterprise Server'da SAP NetWeaver için Azure Sanal Makineler yüksek kullanılabilirlik](high-availability-guide-suse.md), [SUSE Linux Enterprise Server'da SAP NetWeaver için Azure Sanal Makineler yüksek kullanılabilirlik Azure NetApp Files ile,](high-availability-guide-suse-netapp-files.md) [Azure Sanal Makineler, Red Hat Enterprise Linux'ta SAP NetWeaver için yüksek kullanılabilirlik](high-availability-guide-rhel.md), [Azure Sanal Makineler, Red Hat Enterprise Linux'ta SAP NetWeaver için yüksek kullanılabilirlik, Azure NetApp Dosyaları ile Red Hat Enterprise Linux'ta](high-availability-guide-rhel-netapp-files.md) [NFS için yüksek kullanılabilirlik, SUSE Linux Enterprise Server'da Azure VM'lerde NFS için yüksek kullanılabilirlik,](high-availability-guide-suse-nfs.md) [GlusterFS üzerinde Azure VM'lerde Red Hat Enterprise Linux'ta Sap NetWeaver için](high-availability-guide-rhel-glusterfs.md) Azure standart yük dengeleyicisini önermek için  
- 11/08/2019: Şifreleme önerisini netleştirmek için [SAP iş yükü planlaması ve dağıtım denetim listesindeki](sap-deployment-checklist.md) değişiklikler  
- 11/04/2019: Tek döküm yapılandırmaile kümeyi doğrudan oluşturmak için [Azure'da SUSE Linux Enterprise Server'da Pacemaker Kurulumunda](high-availability-guide-suse-pacemaker.md) Değişiklikler  
- 29.10.2019: [SAP yüksek kullanılabilirlik senaryolarında Azure Standart Yük Dengeleyicisini kullanan Sanal Makineler için Genel uç nokta bağlantısının](high-availability-guide-standard-load-balancer-outbound-connections.md) serbest bırakılması
- 25.10.2019: [SAP HANA Azure sanal makine depolama yapılandırmalarında](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-vm-operations-storage) ve [SAP HANA ölçeklendirmesinde, SUSE Linux Enterprise Server'daki Azure NetApp Dosyaları ile Azure VM'lerde standby düğümlü](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-scale-out-standby-netapp-files-suse) değişiklikler,/hana/paylaşılan birim için NFS protokolünü netleştirmek için
- 22.10.2019: [SAP uygulamaları için SUSE Linux Enterprise Server'da Azure VM'lerde SAP NetWeaver](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse)için yüksek kullanılabilirlik değişikliği , SAP uygulamaları için Azure [NetApp Dosyaları ile Azure VM'lerde SAP NetWeaver için yüksek kullanılabilirlik](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse-netapp-files), [Azure VM'lerde NFS için yüksek kullanılabilirlik SUSE Linux Enterprise Server'da](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse-nfs), [Azure'da SUSE Linux Enterprise Server'da Pacemaker'ı ayarlama](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse-pacemaker), [Pacemaker ile SUSE Linux Enterprise Server'da Azure VM'lerde IBM Db2 LUW'un yüksek kullanılabilirliği](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/dbms-guide-ha-ibm)ve Azure Yük Bakiyesi Algılama Sertleştirme için [Azure VM'lerde SAP HANA'nın yüksek kullanılabilirliği](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-high-availability)
- [SAP HANA Azure sanal makine depolama yapılandırmalarında](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-vm-operations-storage) ANF bölümünü ve üstbilgi bölümünü değiştirir
- 21.10.2019: [SLES'teki Azure NetApp Dosyaları ile Azure VM'lerde bekleme düğümü yle SAP HANA ölçeklendirmesinin](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-scale-out-standby-netapp-files-suse) serbest bırakılması
- 16.10.2019: Yedekleme ve [geri yükleme](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-backup-restore) de kırık bağlantıları düzeltmek
- 10/16/2019: [SUSE Linux Enterprise Server'da Ibm Db2 LUW'un Pacemaker ile SUSE Linux Enterprise Server'da yüksek kullanılabilirliğinde](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/dbms-guide-ha-ibm) SLES 12 SP3'ten SLES 12 SP4'e önerilen işletim sistemi değiştirme
- 10/11/2019: Ultra disk depolama yapılandırmalarında değişiklikler ve [SAP HANA Azure sanal makine depolama yapılandırmalarında](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-vm-operations-storage) ANF'nin kullanıma sunulması
- 10/01/2019: Daha fazla netlik elde etmek [için SAP uygulamalarıyla en iyi ağ gecikmesi için Azure yakınlık yerleşim gruplarının](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-proximity-placement-scenarios) grafik değişikliği
- 10/01/2019: /hana/shared için yüksek kullanılabilir NFS payı yla ilgili ifadeleri düzeltmek için [Azure'daki SAP HANA altyapı yapılandırmalarında ve işlemlerinde](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-vm-operations) değişiklik. 
- 28.09.2019: SBD'yi RHEL kümelerinde desteklenmez olarak açıklığa kavuşturmak için [Azure'da Red Hat Enterprise Linux'ta Pacemaker Kurulumunda](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-rhel-pacemaker) Değişiklik  
- 17.09.2019: NETWeaver Planlama ve Dağıtım Kılavuzunda SAP için VM Extension ile ilgili terimleri birleştirilmesi nde değişiklik  
- 22.08.2019: Özel rol oluşturma url'lerini güncellemek için [Azure'da SUSE Linux Enterprise Server'da Pacemaker Kurulumunda](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse-pacemaker) Değişiklikler  
- 08/16/2019: Azure'da [Red Hat Enterprise Linux'ta Pacemaker Kurulumu'nda](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-rhel-pacemaker) yapılan değişiklikler, müşterilere Azure çit aracısının yeni sürümüne güncelleniyorsa, özel roldeki eylemleri güncelleştirmelerini hatırlatmak için  
- 15.08.2019: [SAP HANA Azure sanal makine depolama yapılandırmalarında](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-vm-operations-storage) Ultra diskin Genel Kullanılabilirliğini yansıtacak değişiklikler (eski adıyla Ultra SSD)
- 08/01/2019: [Azure'da SUSE Linux Enterprise Server'da Pacemaker Kurulumunda](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse-pacemaker) Yapılan Değişiklikler, SLES 15 için özel değişiklikleri entegre etmek için 


