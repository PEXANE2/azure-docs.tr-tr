---
title: "Azure'da SAP: Azure VM'lerle Desteklenen Senaryolar"
description: Azure Sanal Makineler SAP iş yüküyle senaryoları destekledi
services: virtual-machines-linux,virtual-machines-windows
documentationcenter: ''
author: msjuergent
manager: bburns
editor: ''
tags: azure-resource-manager
keywords: SAP
ms.assetid: d7c59cc1-b2d0-4d90-9126-628f9c7a5538
ms.service: virtual-machines-linux
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 03/11/2020
ms.author: juergent
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 564c648a550b41017ffc684ca19ff03612fc63d3
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79137637"
---
# <a name="sap-workload-on-azure-virtual-machine-supported-scenarios"></a>Azure sanal makine destekli senaryolarda SAP iş yükü
Azure'da SAP NetWeaver, `Hybris` Business one veya S/4HANA sistem mimarisinin tasarlanması, ölçeklenebilir, verimli ve yüksek kullanılabilir bir dağıtıma ulaşmak için kullanılacak çeşitli mimariler ve araçlar için birçok farklı fırsat sunar. Kullanılan işletim sistemine veya DBMS'ye bağlı olsa da, kısıtlamalar vardır. Ayrıca, şirket içinde desteklenen tüm senaryolar Azure'da aynı şekilde desteklenmez. Bu belge, yalnızca Azure VM'leri kullanarak desteklenen yüksek kullanılabilirlik dışı yapılandırmalar ve yüksek kullanılabilirlik yapılandırmaları ve mimarileri aracılığıyla yol açacaktır. [HANA Büyük Örnekleri](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-overview-architecture)ile desteklenen senaryolar [için, HANA Büyük Örnekleri için desteklenen makaleyi kontrol edin.](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-supported-scenario) 


## <a name="2-tier-configuration"></a>2 Katmanlı yapılandırma
SAP 2 Katmanlı yapılandırma, SAP DBMS'nin ve aynı sunucu veya VM biriminde çalışan uygulama katmanının birleştirilmiş katmanından oluşturulmuş olarak kabul edilir. İkinci katman kullanıcı arabirimi katmanı olarak kabul edilir. 2 Katmanlı yapılandırma durumunda, DBMS ve SAP uygulama katmanı Azure VM'nin kaynaklarını paylaşır. Sonuç olarak, farklı bileşenleri, kaynaklar için rekabet etmeyen bir şekilde yapılandırmanız gerekir. Ayrıca VM'nin kaynaklarına aşırı abone olmamak için dikkatli olmanız gerekir. Böyle bir yapılandırma, ilgili farklı Azure bileşenlerinin [Azure Hizmet Düzeyi anlaşmaları](https://azure.microsoft.com/support/legal/sla/) dışında yüksek kullanılabilirlik sağlamaz.

Böyle bir yapılandırmanın grafik gösterimi aşağıdaki gibi görünebilir:

![Basit 2 Katmanlı yapılandırma](./media/sap-planning-supported-configurations/two-tier-simple-configuration.png)

Bu tür yapılandırmalar, üretim ve üretim dışı durumlar için SQL Server, Oracle, Db2, maxDB ve SAP ASE'nin DBMS sistemleri için Windows, Red Hat, SUSE ve Oracle Linux ile desteklenir. Sap HANA için DBMS olarak, bu tür yapılandırmalar yalnızca üretim dışı servis talepleri için desteklenir. Buna [Azure HANA Büyük Örneklerinin](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-overview-architecture) dağıtım durumu da dahildir.
Azure'da desteklenen tüm OS/DBMS birleşimleri için bu tür yapılandırma lar desteklenir. Ancak, DBMS ve SAP bileşenlerinin yapılandırmasını, DBMS ve SAP bileşenlerinin bellek ve CPU kaynakları için rekabet edemeyecek ve böylece fiziksel kullanılabilir kaynakları aşacak şekilde ayarlamanız zorunludur. Bu, DBMS'nin ayırmasına izin verilen belleği kısıtlayarak yapılmalıdır. Ayrıca, uygulama örneklerinde SAP Genişletilmiş Bellek'i sınırlamanız gerekir. Ayrıca, bileşenlerin CPU kaynaklarını en üst düzeye çıkarmadığından emin olmak için VM'nin CPU tüketimini genel olarak izlemeniz gerekir. 

> [!NOTE]
> Üretim SAP sistemleri için, bu belgede daha sonra açıklandığı gibi ek yüksek kullanılabilirlik ve nihai olağanüstü durum kurtarma yapılandırmaları öneririz


## <a name="3-tier-configuration"></a>3 Katmanlı yapılandırma
Bu tür yapılandırmalarda SAP uygulama katmanını ve DBMS katmanını farklı VM'lere ayırırsınız. Bunu genellikle daha büyük sistemler ve SAP uygulama katmanının kaynakları üzerinde daha esnek olma nedenleriyle yaparsınız. En basit kurulumda, ilgili farklı Azure bileşenlerinin [Azure Hizmet Düzeyi anlaşmaları](https://azure.microsoft.com/support/legal/sla/) dışında yüksek kullanılabilirlik yoktur. 

Grafik gösterimi gibi görünür:

![Basit 2 Katmanlı yapılandırma](./media/sap-planning-supported-configurations/three-tier-simple-configuration.png)

Bu tür yapılandırma, üretim ve üretim dışı durumlar için SQL Server, Oracle, Db2, SAP HANA, maxDB ve SAP ASE'nin DBMS sistemleri için Windows, Red Hat, SUSE ve Oracle Linux'ta desteklenir. Bu, Azure HANA Büyük Örnekleri için varsayılan dağıtım [yapılandırmasıdır.](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-overview-architecture) Basitleştirme için, SAP uygulama katmanındaki SAP Merkezi Hizmetleri ve SAP iletişim örnekleri arasında ayrım yapmadık. Bu basit 3 Katmanlı yapılandırmada SAP Central Services için yüksek kullanılabilirlik koruması olmaz.

> [!NOTE]
> Üretim SAP sistemleri için, bu belgede daha sonra açıklandığı gibi ek yüksek kullanılabilirlik ve nihai olağanüstü durum kurtarma yapılandırmaları öneririz


## <a name="multiple-dbms-instances-per-vm-or-hana-large-instance-unit"></a>VM veya HANA Büyük Örnek birimi başına birden fazla DBMS örneği
Bu yapılandırma türünde, Azure VM veya HANA Büyük Örnek birimi başına birden çok DBMS örneğini barındırarsınız. Motivasyon korumak için daha az işletim sistemleri ve bu azaltılmış maliyetler ile olabilir. Diğer motivasyonlar, daha büyük bir VM veya HANA Büyük Örnek biriminin kaynaklarını birden çok DBMS örneği arasında paylaşarak daha fazla esnekliğe ve daha verimliliğe sahip olmak olabilir. Şimdiye kadar bu yapılandırmalar çoğunlukla üretim dışı sistemler için gösteriyordu.

Böyle bir yapılandırma gibi görünebilir:

![Bir birimde birden çok DBMS örneği](./media/sap-planning-supported-configurations/multiple-database-instances.png)

Bu tür DBMS dağıtımı şu lar için desteklenir:

- Windows'da SQL Server
- IBM Db2. Makalede birden [fazla örnek (Linux, UNIX)](https://www.ibm.com/support/knowledgecenter/en/SSEPGG_10.5.0/com.ibm.db2.luw.admin.dbobj.doc/doc/c0004904.html) ayrıntıları bulun
- Oracle için. Ayrıntılar için [SAP destek notu #1778431](https://launchpad.support.sap.com/#/notes/1778431) ve ilgili SAP notlarına bakın
- SAP HANA için, bir VM'de birden çok örnek, SAP bu dağıtım yöntemini MCOS olarak adlandırır, desteklenir. Ayrıntılar için SAP makalesine bakın [Çoklu SAP HANA Sistemleri Tek Ana Bilgisayarda (MCOS)](https://help.sap.com/viewer/eb3777d5495d46c5b2fa773206bbfb46/2.0.02/
- /b2751fd43bec41a9a14e01913f1edf18.html)

Tek bir ana bilgisayarda birden çok veritabanı örneği çalıştırıldığında, farklı örneklerin kaynaklar için rekabet etmediğinden ve bu nedenle VM'nin fiziksel kaynak sınırlarını aştığından emin olmanız gerekir. Bu, özellikle VM'yi paylaşan örneklerden herhangi birinin ayırabileceği belleği kapatmanız gereken bellek için geçerlidir. Bu, farklı veritabanı örneklerinin yararlanabileceği CPU kaynakları için de geçerli olabilir. Bahsedilen tüm DBMS'lerin, örnek düzeyinde bellek ayırma ve CPU kaynaklarını sınırlamaya izin veren yapılandırmaları vardır.
Azure VM'leri için böyle bir yapılandırma için destek olması için, farklı örnekler tarafından yönetilen veritabanlarının veri ve günlük günlük dosyaları için kullanılan disklerin veya birimlerin ayrı olması beklenir. Veya başka bir deyişle, farklı DBMS örneği tarafından yönetilen veritabanlarının veri veya günlük dosyalarının aynı diskleri veya birimleri paylaşması gerekmez. 

HANA Büyük Örnekleri için disk yapılandırması yapılandırılmış teslim edilir ve [HANA Büyük Örnekleri için Desteklenen senaryolarda](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-supported-scenario#single-node-mcos)ayrıntılı. 

> [!NOTE]
> Üretim SAP sistemleri için, bu belgede daha sonra açıklandığı gibi ek yüksek kullanılabilirlik ve nihai olağanüstü durum kurtarma yapılandırmaları öneririz. Birden çok DBMS örneğine sahip VM'ler, bu belgede daha sonra açıklanan yüksek kullanılabilirlik yapılandırmalarıyla desteklenmez.


## <a name="multiple-sap-dialog-instances-in-one-vm"></a>Bir VM'de birden çok SAP İletişim örneği
Birçok durumda, birden çok iletişim örnekleri çıplak metal sunucularda ve hatta özel bulutlarda çalışan VM'lerde dağıtılır. Bu tür yapılandırmaların nedeni, belirli SAP iletişim örneklerini belirli iş yükü, iş işlevselliği veya iş yükü türlerine uyarlamaktı. Bu örnekleri ayrı VM'lere ayırmamanın nedeni işletim sistemi bakım ve işlemlerinin çabasıydı. Veya birçok durumda, VM'nin ev sahibi veya operatörünün vm'başına aylık ücret istemesi ve yönetilmesi durumunda maliyetler. Azure'da, Windows, Red Hat, SUSE ve Oracle Linux işletim sistemlerinde üretim ve üretim dışı amaçlar için desteklenen tek bir VM içinde birden fazla SAP iletişim örneğini barındırma senaryosu. Windows ve modern Linux çekirdeklerinde bulunan SAP çekirdek parametresi PHYS_MEMSIZE, tek bir VM'de birden çok SAP Application Server örneği çalışıyorsa ayarlanmalıdır. Sap genişletilmiş Belleğin otomatik büyümesinin uygulandığı Windows gibi işletim sistemlerinde SAP Genişletilmiş Bellek genişlemesini sınırlaması da önerilir. Bu SAP profil parametresi `em/max_size_MB`ile yapılabilir.

Azure VM'lerde birden çok SAP iletişim örneğinin çalıştırıldığı 3 Katmanlı yapılandırmada şu şekilde görünebilir:

![Bir birimde birden çok DBMS örneği](./media/sap-planning-supported-configurations/multiple-dialog-instances.png)

Basitleştirme için, SAP uygulama katmanındaki SAP Merkezi Hizmetleri ve SAP iletişim örnekleri arasında ayrım yapmadık. Bu basit 3 Katmanlı yapılandırmada SAP Central Services için yüksek kullanılabilirlik koruması olmaz. Üretim sistemleri için SAP Central Services'ı korumasız bırakmak önerilmez. SAP Merkezi Örnekleri ve bu tür çoklu SID yapılandırmalarının yüksek kullanılabilirliği etrafında ki çoklu SID yapılandırmaları hakkındaki ayrıntılar için bu belgenin sonraki bölümlerine bakın.

## <a name="high-availability-protection-for-the-sap-dbms-layer"></a>SAP DBMS katmanı için yüksek kullanılabilirlik koruması
SAP üretim sistemlerini dağıtmaya baktığınızda, sıcak bekleme tipi yüksek kullanılabilirlik yapılandırmalarını göz önünde bulundurmanız gerekir. Özellikle, tam performansı ve ölçeklenebilirliği geri alabilmek için verilerin belleğe yüklenmesi gereken SAP HANA'da, Azure hizmet iyileştirmesi yüksek kullanılabilirlik için ideal bir ölçü değildir. 

Genel olarak Microsoft, docs.microsoft.com'da SAP iş yükü bölümü altında açıklanan yalnızca yüksek kullanılabilirlik yapılandırmalarını ve yazılım paketlerini destekler. Aynı deyimi SAP [not#1928533](https://launchpad.support.sap.com/#/notes/1928533)okuyabilirsiniz. Microsoft, SAP iş yüküyle birlikte Microsoft tarafından belgelenmeyen diğer yüksek kullanılabilirlik üçüncü taraf yazılım çerçeveleri için destek sağlamaz. Bu gibi durumlarda, yüksek kullanılabilirlik çerçevesinin üçüncü taraf tedarikçisi, müşteri olarak sizin tarafınıza dahil edilmesi gereken yüksek kullanılabilirlik yapılandırması için destekleyici taraftır. İstisnalar bu makalede belirtilmeye devam edilmiştir. 

Genel olarak Microsoft, Azure VM'lerde veya HANA Büyük Örnekler birimlerinde sınırlı sayıda yüksek kullanılabilirlik yapılandırmalarını destekler. HANA Büyük Örnekleri desteklenen senaryolar [için, HANA Büyük Örnekleri için desteklenen senaryolar](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-supported-scenario)belgeyi okuyun.

Azure VM'ler için aşağıdaki yüksek kullanılabilirlik yapılandırmaları DBMS düzeyinde desteklenir:

- SAP HANA Sistem Çoğaltma Linux Pacemaker SUSE ve Red Hat dayalı. Ayrıntılı makalelere bakın:
    - [SUSE Linux Enterprise Server'da Azure VM'lerde SAP HANA'nın yüksek kullanılabilirliği](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-high-availability)
    - [Red Hat Enterprise Linux'ta Azure VM'lerde SAP HANA'nın yüksek kullanılabilirliği](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-high-availability-rhel)
- SUSE ve Red Hat'teki [Azure NetApp Dosyalarını](https://azure.microsoft.com/services/netapp/) kullanarak SAP HANA ölçeklendirin n+m yapılandırmaları. Ayrıntılar bu makalelerde listelenmiştir:
    - [SUSE Linux Enterprise Server'da Azure NetApp Dosyalarını kullanarak Azure VM'lerde bekleme düğümü içeren bir SAP HANA ölçeklendirme sistemi dağıtın}](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-scale-out-standby-netapp-files-suse)
    - [Red Hat Enterprise Linux'ta Azure NetApp Dosyalarını kullanarak Azure VM'lerinde bekleme düğümü içeren bir SAP HANA ölçeklendirme sistemi dağıtma](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-scale-out-standby-netapp-files-rhel)
- Windows Scale-Out Dosya Hizmetleri'ni temel alan SQL Server Failover kümesi. Üretim sistemleri için öneri kümeleme yerine SQL Server Always On kullanmak olsa da. SQL Server Always Her zaman ayrı depolama alanı kullanarak daha iyi kullanılabilirlik sağlar. Ayrıntılar bu makalede açıklanmıştır: 
    - [Azure sanal makinelerde SQL Server failover küme örneğini yapılandırma](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-portal-sql-create-failover-cluster)
- SQL Server Always On, Azure'daki SQL Server için Windows işletim sistemi ile desteklenir. Bu, Azure'daki sql server örnekleri için varsayılan öneridir. Ayrıntılar bu makalelerde açıklanmıştır:
    - [Azure sanal makinelerde SQL Server Always On kullanılabilirlik gruplarına tanıtın.](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-portal-sql-availability-group-overview)
    - [Farklı bölgelerdeki Azure sanal makinelerinde Her Zaman Açık kullanılabilirlik grubunu yapılandırın.](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-portal-sql-availability-group-dr)
    - [Azure'daki Her Zaman Kullanılabilirlik grubu için bir yük dengeleyicisi yapılandırın.](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-portal-sql-alwayson-int-listener)
- Windows ve Oracle Linux için Oracle Data Guard. Oracle Linux için ayrıntılar bu makalede bulunabilir:
    - [Azure Linux sanal makinesinde Oracle Data Guard uygulamasını](https://docs.microsoft.com/azure/virtual-machines/workloads/oracle/configure-oracle-dataguard)
- Ibm Db2 HADR Üzerinde SUSE ve RHEL Pacemaker kullanarak SUSE ve RHEL için ayrıntılı belgeler burada verilmiştir:
    - [Pacemaker ile SUSE Linux Enterprise Server'da Azure VM'lerde IBM Db2 LUW'un yüksek kullanılabilirliği](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/dbms-guide-ha-ibm)
    - [Red Hat Enterprise Linux Server üzerinde Azure VM’lerindeki IBM Db2 LUW’a yönelik yüksek kullanılabilirlik](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-rhel-ibm-db2-luw)
- Sap ASE ve SAP maxDB yapılandırması bu belgelerde ayrıntılı olarak:
    - [SAP iş yükü için SAP ASE Azure Sanal Makineler DBMS dağıtımı](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/dbms_guide_sapase)
    - [Azure VM'lerde SAP MaxDB, liveCache ve Content Server dağıtımı](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/dbms_guide_maxdb)
- HANA Büyük Örnekler yüksek kullanılabilirlik senaryoları ayrıntılı olarak:
    - [Yüksek kullanılabilirlik için STONITH ile HANA Büyük Örnekleri- HSR için desteklenen senaryolar](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-supported-scenario#hsr-with-stonith-for-high-availability)
    - [HANA Büyük Örnekleri için desteklenen senaryolar - Ana bilgisayar otomatik failover (1+1)](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-supported-scenario#host-auto-failover-11)

> [!IMPORTANT]
> Yukarıda açıklanan senaryoların hiçbiri için, tek bir VM'de birden çok DBMS örneğinin yapılandırmalarını destekliyoruz. Her durumda, VM başına yalnızca bir veritabanı örneği dağıtılabilir ve açıklanan yüksek kullanılabilirlik yöntemleriyle korunabilir. Aynı Windows veya Pacemaker failover kümesi altında birden çok DBMS örneğinin korunması şu anda **desteklenmez.** Ayrıca Oracle Data Guard, yalnızca VM dağıtım servis talepleri başına tek bir örnek için desteklenir. 

Çeşitli veritabanı sistemleri, tek bir DBMS örneği altında birden çok veritabanını barındırmaya olanak sağlar. SAP HANA örneğinde olduğu gibi, birden çok veritabanı birden çok veritabanı kapsayıcısında (MDC) barındırılabilir. Bu çok veritabanı yapılandırmalarının tek bir başarısız küme kaynağı içinde çalıştığı durumlarda, bu yapılandırmalar desteklenir. Desteklenmeyen yapılandırmalar, birden çok küme kaynağının gerekli olduğu durumlardır. Birden çok SQL Server Kullanılabilirlik Grubu tanımlayacağınız yapılandırmalara gelince, tek bir SQL Server örneği altında.


![DBMS HA yapılandırması](./media/sap-planning-supported-configurations/database-high-availability-configuration.png)

DBMS an/veya işletim sistemlerine bağlı olarak, Azure yük dengeleyicisi gibi bileşenler çözüm mimarisinin bir parçası olarak gerekli olabilir veya olmayabilir. 

Özellikle maxDB için depolama yapılandırmasının farklı olması gerekir. maxDB'de veri ve günlük dosyalarının yüksek kullanılabilirlik yapılandırmaları için paylaşılan depolama alanında bulunması gerekir. Yalnızca maxDB durumunda, paylaşılan depolama yüksek kullanılabilirlik için desteklenir. Düğüm başına diğer tüm DBMS ayrı depolama yığınları için desteklenen tek disk yapılandırmaları vardır.

Diğer yüksek kullanılabilirlik çerçevelerinin bulunduğu ve Microsoft Azure'da da çalıştırılabildiği bilinmektedir. Ancak, Microsoft bu çerçeveleri sınamadı. Bu çerçevelerle yüksek kullanılabilirlik yapılandırmanızı oluşturmak istiyorsanız, aşağıdakiler için bu yazılımın sağlayıcısıyla çalışmanız gerekir:

- Dağıtım mimarisi geliştirme
- Mimarinin dağıtımı
- Mimarinin desteklenmesi

> [!IMPORTANT]
> Microsoft Azure Marketi, Azure yerel depolama alanının üzerinde depolama çözümleri sağlayan çeşitli yumuşak cihazlar sunar. Bu yumuşak cihazlar, teorik olarak bekleme düğümü gerektiren SAP HANA ölçeklendirme dağıtımlarında teorik olarak kullanılabilen NFS paylaşımları oluşturmak için de kullanılabilir. Çeşitli nedenlerden dolayı, bu depolama yumuşak cihazların hiçbiri Microsoft ve SAP tarafından Azure'daki DBMS dağıtımları için desteklenmez. DBMS'nin Kobİ hisselerine dağıtımları şu anda hiç desteklenmez. NFS hisselerinde DBMS'nin [dağıtımı, Azure NetApp Dosyalarındaki](https://azure.microsoft.com/services/netapp/)NFS 4.1 paylaşımları ile sınırlıdır.


## <a name="high-availability-for-sap-central-service"></a>SAP Merkez Servisi için Yüksek Kullanılabilirlik
SAP Merkezi Hizmetleri, SAP yapılandırmanızın ikinci tek hata noktasıdır. Sonuç olarak, bu Merkezi Hizmetler süreçlerini de korumanız gerekir. SAP iş yükü için desteklenen ve belgelenen teklif şöyledir:

- Windows Failover Cluster Server sapmnt ve global aktarım dizini için Windows Scale-out Dosya Hizmetleri'ni kullanır. Ayrıntılar makalede açıklanmıştır:
    - [Azure'da dosya paylaşımı kullanarak bir Windows failover kümesinde SAP ASCS/SCS örneğini kümeleme](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-high-availability-guide-wsfc-file-share)
    - [SAP ASCS/SCS örnekleri için Windows failover kümesi ve dosya paylaşımı kullanarak Azure altyapılarını SAP yüksek kullanılabilirliği için hazırlama](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-high-availability-infrastructure-wsfc-file-share)
- Windows Failover Cluster Server, sapmnt ve global aktarım dizini için [Azure NetApp Dosyalarını](https://azure.microsoft.com/services/netapp/) temel alan Kobİ paylaşımını kullanır. Ayrıntılar makalede listelenmiştir:
    - [SAP uygulamaları için Azure NetApp Files(SMB) ile Windows'daki Azure VM'lerde SAP NetWeaver için yüksek kullanılabilirlik](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-windows-netapp-files-smb)
- Windows Failover Cluster Server `Datakeeper`SIOS dayalı . Microsoft tarafından belgelenmiş olsa da, Bu çözümü kullanırken SIOS desteğiile etkileşimkurabileceğiniz için SIOS ile bir destek ilişkisine ihtiyacınız vardır. Ayrıntılar makalede açıklanmıştır:
    - [Azure'da bir küme paylaşılan disk kullanarak bir Windows failover kümesinde SAP ASCS/SCS örneğini kümeleme](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-high-availability-guide-wsfc-shared-disk)
    - [SAP ASCS/SCS için Windows failover kümesi ve paylaşılan disk kullanarak Azure altyapısını SAP HA için hazırlayın](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-high-availability-infrastructure-wsfc-shared-disk)
- Pacemaker SUSE işletim sistemi iki SUSE VMs kullanarak ve `drdb` dosya çoğaltma için son derece kullanılabilir NFS payı oluşturma ile. Ayrıntılar makalede belgelenmiştir
    - [SAP uygulamaları için SUSE Linux Enterprise Server'da Azure VM'lerde SAP NetWeaver için yüksek kullanılabilirlik](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse)
    - [SUSE Linux Enterprise Server'da Azure VM'lerde NFS için yüksek kullanılabilirlik](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse-nfs)
- [Azure NetApp Files](https://azure.microsoft.com/services/netapp/)tarafından sağlanan NFS paylaşımlarından yararlanan Pacemaker SUSE işletim sistemi. Ayrıntılar belgelenmiştir
    - [SAP uygulamaları için Azure NetApp Dosyaları ile SUSE Linux Enterprise Server'da Azure VM'lerde SAP NetWeaver için yüksek kullanılabilirlik](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse-netapp-files)
- Pacemaker Red Hat işletim sistemi nfs payı `glusterfs` ile bir küme üzerinde barındırılan. Ayrıntılar makalelerde bulunabilir
    - [Red Hat Enterprise Linux'ta SAP NetWeaver için Azure Sanal Makineler yüksek kullanılabilirlik](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-rhel)
    - [`GlusterFS`SAP NetWeaver için Red Hat Enterprise Linux'ta Azure VM'lerde](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-rhel-glusterfs)
- NFS paylaşımı ile Red Hat işletim sistemi pacemaker [Azure NetApp Files](https://azure.microsoft.com/services/netapp/)üzerinde barındırılan . Ayrıntılar makalede açıklanmıştır
    - [Azure Sanal Makineler, SAP uygulamaları için Azure NetApp Dosyaları ile Red Hat Enterprise Linux'ta SAP NetWeaver için yüksek kullanılabilirlik](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-rhel-netapp-files)

Listelenen çözümler arasında, `Datakeeper` ürünü desteklemek ve sorunlar durumunda doğrudan SIOS ile etkileşimde bulunmak için SIOS ile bir destek ilişkisine ihtiyacınız vardır. Windows, Red Hat ve/veya SUSE OS'yi lisanslayız şekline bağlı olarak, listelenen yüksek kullanılabilirlik yapılandırmalarının tam desteğine sahip olmak için işletim sistemi sağlayıcınızla bir destek sözleşmesi ne kadar süreniz olması da gerekebilir.

Yapılandırma aşağıdaki gibi görüntülenebilir:

![DBMS ve ASCS HA yapılandırması](./media/sap-planning-supported-configurations/high-available-3-tier-configuration.png)

Grafiklerin sağ tarafında, kullanılabilir SAP Merkezi Hizmetleri gösterilir. SAP Central hizmetlerinin bir sorun durumunda başarısız olabilecek bir küme çerçevesi ile korunmasına ek olarak, sapmnt ve küresel aktarım dizininin tek bir VM varlığından bağımsız olarak kullanılabilir. Windows Failover Cluster Server ve Pacemaker gibi çözümlerden bazıları, trafiği sağlıklı bir düğüme yönlendirmek veya yönlendirmek için bir Azure yük dengeleyicisi gerektirecektir.

Gösterilen listede, Oracle Linux işletim sisteminden bahsedilmemiştir. Oracle Linux bir küme çerçevesi olarak Pacemaker desteklemiyor. SAP sisteminizi Oracle Linux'ta dağıtmak istiyorsanız ve Oracle Linux için yüksek kullanılabilirlik çerçevesine ihtiyacınız varsa, üçüncü taraf tedarikçilerle çalışmanız gerekir. Tedarikçilerden biri, Azure'da SAP tarafından desteklenen Linux için Koruma Paketi'ne sahip SIOS'tur. Daha fazla bilgi için SAP not #1662610 - Daha fazla bilgi [için Linux için SIOS Protection Suite için destek ayrıntılarını](https://launchpad.support.sap.com/#/notes/1662610) okuyun.



### <a name="supported-storage-with-the-sap-central-services-scenarios-listed-above"></a>Yukarıda listelenen SAP Merkezi Hizmetler senaryoları ile desteklenen depolama
Yalnızca Azure depolama türlerinin bir alt kümesi, SAP Merkezi Hizmetleri küme senaryolarımızdaki kullanım için bu kaliteyi yüksek oranda kullanılabilir NFS veya Kobİ paylaşımları sağladığından, desteklenen depolama türlerinin bir listesi

- Windows Scale-out File Server'a sahip Windows Failover Cluster Server, Azure NetApp Dosyaları hariç tüm azure depolama türlerinde dağıtılabilir. Ancak, öneri, iş ve IOPS'deki üstün hizmet düzeyi anlaşmaları nedeniyle Premium Depolama'dan yararlanmaktır.
- Azure NetApp Dosyalarında Kobİ'ye sahip Windows Failover Cluster Server, Azure NetApp Dosyalarında desteklenir. Azure Dosyası hizmetlerinde Kobİ paylaşımları şu anda **desteklenmez.**
- SIOS `Datakeeper` tabanlı windows paylaşılan diske sahip Windows Failover Cluster Server, Azure NetApp Dosyaları hariç tüm azure depolama türlerinde dağıtılabilir. Ancak, öneri, iş ve IOPS'deki üstün hizmet düzeyi anlaşmaları nedeniyle Premium Depolama'dan yararlanmaktır.
- Azure NetApp Dosyalarında NFS paylaşımlarını kullanan SUSE veya Red Hat Pacemaker Azure NetApp Dosyalarında desteklenir. 
- İki VM arasında `drdb` yapılandırma kullanan SUSE Pacemaker, Azure NetApp Dosyaları dışında yerel Azure depolama türleri kullanılarak desteklenir. Ancak, öneri, iş ve IOPS'deki üstün hizmet düzeyi anlaşmaları nedeniyle Premium Depolama'dan yararlanmaktır.
- NFS paylaşımını sağlamak için kullandığı `glusterfs` Red Hat Pacemaker, Azure NetApp Dosyaları dışında yerel Azure depolama türleri kullanılarak desteklenir. Ancak, öneri, iş ve IOPS'deki üstün hizmet düzeyi anlaşmaları nedeniyle Premium Depolama'dan yararlanmaktır.

> [!IMPORTANT]
> Microsoft Azure Marketi, Azure yerel depolama alanının üzerinde depolama çözümleri sağlayan çeşitli yumuşak cihazlar sunar. Bu yumuşak cihazlar NFS veya Kobİ hisseleri oluşturmak için de teorik olarak kümelenmiş SAP Merkezi Hizmetleri üzerinde de kullanılabilir oluşturmak için kullanılabilir. Bu çözümler, SAP iş yükü için Microsoft tarafından doğrudan desteklenmez. NFS veya Kobİ payınızı oluşturmak için böyle bir çözüm kullanmaya karar verirseniz, SAP Merkezi Hizmet yapılandırmasına yönelik desteğin, yazılımı niçin depolama yumuşak cihazında bulunan üçüncü taraf tarafından sağlanması gerekir.


## <a name="multi-sid-sap-central-services-failover-clusters"></a>Multi-SID SAP Merkezi Hizmetleri kümeler üzerinde başarısız
Sap, büyük SAP manzaralarında gereken VM sayısını azaltmak için, başarısız küme yapılandırmasında birden çok farklı SAP sisteminin SAP Merkezi Hizmetleri örneklerini çalıştırmaya olanak tanır. 30 veya daha fazla NetWeaver veya S/4HANA üretim sistemine sahip olduğunuz durumları düşünün. Çoklu SID kümeleme olmadan, bu yapılandırmalar 30 veya daha fazla Windows veya Pacemaker failover küme yapılandırmalarında 60 veya daha fazla VM gerektirir. DBMS failover kümeleri dışında gerekli. Bir failover küme yapılandırmasında iki düğüm üzerinde birden çok SAP merkezi hizmeti dağıtmak, VM sayısını önemli ölçüde azaltabilir. Ancak, tek bir iki düğüm küme yapılandırması üzerinde birden çok SAP Central hizmetleri örnekleri dağıtmada bazı dezavantajları da vardır. Küme yapılandırmasındaki tek bir VM'nin sorunları birden çok SAP sistemine uygulanır. Küme yapılandırmasında çalışan konuk işletim sistemi üzerinde bakım, birden çok üretim SAP sistemleri etkilendiği için daha fazla koordinasyon gerektirir. SAP LaMa gibi araçlar, sistem klonlama işlemlerinde çoklu SID kümeleme işlemini desteklemiyor.

Azure'da, ENSA1 ve ENSA2 işletim sistemine sahip Windows işletim sistemi için çok SID küme yapılandırması desteklenir. Öneri, eski Enqueue Çoğaltma Hizmeti mimarisini (ENSA1) yeni mimariyle (ENSA2) tek bir çoklu SID kümeüzerinde birleştirmek değildir. Böyle bir mimari ile ilgili ayrıntılar makalelerde belgelenmiştir

- [Windows Server Failover Clustering ve Azure'da paylaşılan disk ile SAP ASCS/SCS örneği çoklu SID yüksek kullanılabilirlik](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-ascs-ha-multi-sid-wsfc-shared-disk) 
- [Windows Server Failover Clustering ve Azure'da dosya paylaşımı ile SAP ASCS/SCS örneği çoklu SID yüksek kullanılabilirlik](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-ascs-ha-multi-sid-wsfc-file-share) 

SUSE için, Pacemaker tabanlı bir multi-SID küme de desteklenir. Şimdiye kadar yapılandırma için desteklenir:

- En fazla beş SAP ASCS/SCS örneği
- Eski enqueue çoğaltma sunucusu buz mimarisi (ENSA1)
- İki düğüm Pacemaker küme yapılandırmaları

Yapılandırma, [SAP uygulamaları multi-SID kılavuzu için SUSE Linux Enterprise Server'daki Azure VM'lerde SAP NetWeaver için yüksek kullanılabilirlikte](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse-multi-sid) belgelenmiştir

Enqueue Çoğaltma sunucusu şematik olarak görünen bir çoklu SID kümesi

![DBMS ve ASCS HA yapılandırması](./media/sap-planning-supported-configurations/high-available-multi-system-configuration.png)


## <a name="sap-hana-scale-out-scenarios"></a>SAP HANA ölçeklendirme senaryoları
SAP HANA ölçeklendirme senaryoları, [SAP HANA donanım dizininde](https://www.sap.com/dmc/exp/2014-09-02-hana-hardware/enEN/iaas.html#categories=Microsoft%20Azure)listelenen HANA sertifikalı Azure VM'lerinin bir alt kümesi için desteklenir. 'Kümeleme' sütununda 'Evet' ile işaretlenmiş tüm VM'ler OLAP veya S/4HANA ölçeklendirmesi için kullanılabilir. Bekleme değiştirilen konfigürasyonlar Azure Depolama türleri ile desteklenir: 

- /hana/log birimi için Azure Yazma hızlandırıcısı da dahil olmak üzere Azure Premium Depolama
- [Ultra disk](https://docs.microsoft.com/azure/virtual-machines/linux/disks-enable-ultra-ssd)
- [Azure NetApp Files](https://azure.microsoft.com/services/netapp/) 

Bekleme düğümü(ler) olan OLAP veya S/4HANA için SAP HANA ölçeklendirme yapılandırmaları yalnızca Azure NetApp Files'da paylaşılan NFS ile desteklenir.

Bekleme düğümü olan veya olmayan tam depolama yapılandırmaları hakkında daha fazla bilgi için makaleleri kontrol edin:

- [SAP HANA Azure sanal makine depolama alanı yapılandırmaları](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-vm-operations-storage) 
- [SUSE Linux Enterprise Server'da Azure NetApp Dosyalarını kullanarak Azure VM'lerinde bekleme düğümü içeren bir SAP HANA ölçeklendirme sistemi dağıtma](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-scale-out-standby-netapp-files-suse)
- [Red Hat Enterprise Linux'ta Azure NetApp Dosyalarını kullanarak Azure VM'lerinde bekleme düğümü içeren bir SAP HANA ölçeklendirme sistemi dağıtma](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-scale-out-standby-netapp-files-rhel)
- [SAP destek notu #2080991](https://launchpad.support.sap.com/#/notes/2080991)

HANA Büyük Örnekleri desteklenen HANA ölçeklendirme yapılandırmaları ayrıntıları için aşağıdaki belgeler geçerlidir:

- [HANA Büyük Örnekleri için desteklenen senaryolar bekleme ile ölçeklendirildi](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-supported-scenario#scale-out-with-standby)
- [HANA Büyük Örnekler için desteklenen senaryolar beklemeden ölçeklendirildi](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-supported-scenario#scale-out-without-standby)


## <a name="disaster-recovery-scenario"></a>Olağanüstü Durum Kurtarma Senaryosu
Desteklenen çeşitli olağanüstü durum kurtarma senaryoları vardır. Felaket mimarilerini, tam bir Azure bölgesinin şebeke dışına gidişini telafi etmesi gereken mimariler olarak tanımlıyoruz. Bu, SAP ortamınızı çalıştırmak için hedef olarak farklı bir Azure bölgesi olması için olağanüstü durum kurtarma hedefine ihtiyacımız olduğu anlamına gelir. DBMS katmanındaki ve DBMS olmayan katmandaki yöntemleri ve yapılandırmaları ayırıyoruz. 

### <a name="dbms-layer"></a>DBMS katmanı
DBMS katmanı için, Her Zaman Açık, Oracle Data Guard, Db2 HADR, SAP ASE Always-On veya HANA Sistem Çoğaltma gibi DBMS yerel çoğaltma mekanizmalarını kullanan yapılandırmalar desteklenir. Bu gibi durumlarda çoğaltma akışının, tek bir Azure bölgesinde dağıtılan tipik yüksek kullanılabilirlik senaryolarında olduğu gibi senkron yerine eşzamanlı olması zorunludur. Desteklenen bir DBMS olağanüstü durum kurtarma yapılandırmasının tipik bir örneği, [Azure bölgeleri arasında SAP HANA kullanılabilirliği](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-availability-across-regions#combine-availability-within-one-region-and-across-regions)makalesinde açıklanmıştır. Bu bölümdeki ikinci grafik, HANA içeren bir senaryoyu örnek olarak açıklar. SAP uygulamaları için desteklenen ana veritabanlarının tümü böyle bir senaryoda dağıtılabilir.

VM tam iş yükü trafiğini yaşamadığından, olağanüstü durum kurtarma bölgesinde daha küçük bir VM'nin hedef örnek olarak kullanılması desteklenir. Bunu yaparken, aşağıdaki hususları göz önünde bulundurmanız gerekir:

- Daha küçük VM türleri, küçük VM'lerden çok sayıda diskin eklenmesine izin vermez
- Daha küçük VM'ler daha az ağ ve depolama iş metoduna sahiptir
- Farklı VM'ler tek bir Azure Kullanılabilirlik Kümesi'nde toplandığında veya yeniden boyutlandırma M-Serisi ailesi ile Mv2 VM ailesi arasında gerçekleştiğinde VM aileleri arasında yeniden boyutlandırma sorun olabilir
- Veritabanı örneğinin CPU ve bellek tüketimi, bu değişiklikleri en az gecikmeyle verilere uygulamak için en az gecikme ve yeterli CPU ve bellek kaynağı yla değişikliklerin akışını alabilme  

Farklı VM boyutları sınırlamaları hakkında daha fazla bilgi [burada](https://docs.microsoft.com/azure/virtual-machines/linux/sizes) bulabilirsiniz 

DR hedefini dağıtmanın desteklenen bir diğer yöntemi de, üretim dışı bir SAP örneğinin üretim dışı DBMS örneğini çalıştıran bir VM'ye ikinci bir DBMS örneğinin yüklenmesidir. Dr senaryosunda ana örnek olarak çalışması gereken belirli hedef örnekleri için bellek, CPU kaynakları, ağ bant genişliği ve depolama bant genişliği ne gerektiğini anlamaya gerek bu yana bu biraz daha zor olabilir. Özellikle HANA'da, verilerin DR hedef örneğine önceden yüklenmemesi için paylaşılan bir ana bilgisayarda DR hedefi olarak işlev gösteren örneği yapılandırmanız önerilir.

HANA Büyük Örnek DR senaryoları için bu belgeleri denetleyin:

- [Depolama çoğaltma kullanarak DR ile tek düğüm](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-supported-scenario#single-node-with-dr-using-storage-replication)
- [Depolama çoğaltma kullanarak DR (çok amaçlı) ile tek düğüm](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-supported-scenario#single-node-with-dr-multipurpose-using-storage-replication)
- [Depolama çoğaltma kullanarak DR (çok amaçlı) ile tek düğüm](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-supported-scenario#single-node-with-dr-multipurpose-using-storage-replication)
- [Depolama çoğaltma ile HSR ve DR ile yüksek kullanılabilirlik](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-supported-scenario#high-availability-with-hsr-and-dr-with-storage-replication)
- [Depolama çoğaltma kullanarak DR ile ölçeklendirme](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-supported-scenario#scale-out-with-dr-using-storage-replication)
- [HSR kullanarak DR ile tek düğüm](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-supported-scenario#single-node-with-dr-using-hsr)
- [Dr tek düğüm HSR (maliyet optimize)](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-supported-scenario#single-node-hsr-to-dr-cost-optimized)
- [HSR ile yüksek kullanılabilirlik ve olağanüstü durum kurtarma](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-supported-scenario#high-availability-and-disaster-recovery-with-hsr)
- [HSR ile yüksek kullanılabilirlik ve olağanüstü durum kurtarma (maliyet optimize edilmiş)](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-supported-scenario#high-availability-and-disaster-recovery-with-hsr-cost-optimized)
- [HSR kullanarak DR ile ölçeklendirme](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-supported-scenario#scale-out-with-dr-using-hsr)

> [!NOTE]
> Azure [Site Kurtarma](https://azure.microsoft.com/services/site-recovery/) kullanımı SAP iş yükü altındaki DBMS dağıtımları için sınanmandı. Sonuç olarak, bu noktada SAP sistemlerinin DBMS katmanı için desteklenmez. Microsoft ve SAP tarafından listelenmeyen diğer çoğaltma yöntemleri desteklenmez. Sap sistemlerinin DBMS katmanını farklı Azure Bölgeleri arasında çoğaltmak için üçüncü taraf yazılımlarının kullanılması, yazılımın satıcısı tarafından desteklenmesi gerekir ve Microsoft ve SAP destek kanalları aracılığıyla desteklenmez. 
 
## <a name="non-dbms-layer"></a>DBMS olmayan katman
SAP uygulama katmanı ve gerekli olan nihai paylaşımlar veya depolama konumları için, iki ana senaryo müşteriler tarafından yararlanılır:

- İkinci Azure bölgesindeki olağanüstü durum kurtarma hedefleri herhangi bir üretim veya üretim dışı amaçlar için kullanılmamaktadır. Bu senaryoda, olağanüstü durum kurtarma hedefi olarak işlev gören VM'ler ideal olarak dağıtılamaz ve üretim SAP uygulama katmanının görüntülerindeki görüntü ve değişiklikler olağanüstü durum kurtarma bölgesine çoğaltılır. Böyle bir görevi gerçekleştirebilen bir işlev [Azure Site Kurtarma'dır.](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-move-overview) Azure Site Kurtarma, bunun gibi Azure'dan Azure'a çoğaltma senaryolarını destekler. 
- Olağanüstü durum kurtarma hedefleri, üretim dışı sistemler tarafından fiilen kullanılan VM'lerdir. Tüm SAP ortamı, genellikle bir bölgede üretim sistemleri ve başka bir bölgedeki üretim dışı sistemlerle iki farklı Azure bölgesine yayılır. Birçok müşteri dağıtımında, müşteri üretim sistemine eşdeğer bir üretim dışı sisteme sahiptir. Müşteri, üretim dışı sistemlere uygulama katmanına önceden yüklenmiş üretim uygulama örneklerine sahiptir. Bir arıza durumunda, üretim dışı örnekler kapatılır, üretim VM'lerinin sanal adları üretim dışı VM'lere taşınır (DNS'de yeni IP adresleri atadıktan sonra) ve önceden yüklenmiş üretim örnekleri başlatılır

### <a name="sap-central-services-clusters"></a>SAP Merkezi Hizmetler kümeleri
Paylaşılan diskler (Windows), Kobİ paylaşımları (Windows) veya NFS paylaşımlarını kullanan SAP Merkezi Hizmetler kümelerini çoğaltmak biraz daha zordur. Windows tarafında, Windows Depolama Çoğaltma olası bir çözümdür. Linux rsync üzerinde uygulanabilir bir çözümdür.



## <a name="non-supported-scenario"></a>Desteklenmeyen senaryo
Azure mimarilerinde SAP iş yükü için desteklenmeyen bir senaryo listesi vardır. **Desteklenmeyen,** SAP ve Microsoft'un bu yapılandırmaları destekleyemeyecekleri ve bu tür mimarileri oluşturmak için yazılım sağlayan ilgili bir üçüncü tarafa ertelemeleri gerektiği anlamına gelir. Kategorilerden ikisi şunlardır:

- Depolama yumuşak aletleri: Azure pazarda sunulan bir dizi depolama yumuşak aletleri vardır. Satıcılardan bazıları, SAP yazılımıyla ilgili olarak Azure'da bu depolama yumuşak cihazlarının nasıl kullanılacağına ilişkin kendi belgeleri sunar. Bu tür depolama yumuşak aletleri içeren yapılandırmaları veya dağıtımları desteği bu depolama yumuşak aletleri satıcı tarafından sağlanmalıdır. Bu gerçek [SAP destek notunda](https://launchpad.support.sap.com/#/notes/2015553) da #2015553
- Yüksek Kullanılabilirlik çerçeveleri: Yalnızca Pacemaker ve Windows Server Failover Cluster, Azure'daki SAP iş yükü için yüksek kullanılabilirlik çerçeveleri desteklenir. Daha önce de belirtildiği gibi, `Datakeeper` SIOS çözümü Microsoft tarafından açıklanmıştır ve belgelenmiştir. Bununla birlikte, SIOS `Datakeeper` bileşenlerinin bu bileşenleri sağlayan satıcı olarak SIOS aracılığıyla desteklenmesi gerekir. SAP ayrıca çeşitli SAP notlarında diğer sertifikalı yüksek kullanılabilirlik çerçevelerini de listeledi. Bazıları Azure için üçüncü taraf satıcı tarafından da sertifikalandı. Bununla birlikte, bu ürünleri kullanan yapılandırmalar için destek ürün satıcısı tarafından sağlanmalıdır. Farklı satıcıların SAP destek süreçlerine farklı tümleştirmeleri var. Ürünü Azure'da dağıtılan SAP yapılandırmalarında kullanmaya karar vermeden önce, belirli satıcı için en iyi destek işleminin hangileri için en iyi şekilde çalıştığını açıklığa kavuşturmalısınız.
- Veritabanı dosyalarının paylaşılan disklerde bulunduğu paylaşılan disk kümeleri maxDB dışında desteklenmez. Diğer tüm veritabanı için desteklenen çözüm, yüksek kullanılabilirlik senaryolarını yapılandırmak için SMB veya NFS paylaşımı veya paylaşılan disk yerine ayrı depolama konumlarına sahip olmaktır

Desteklenmeyen diğer senaryolar şunlardır:

- NetWeaver, S/4HANA ve örneğin' de gösterildiği gibi SAP'nin ortak mimarisindeki SAP uygulama katmanı ile SAP DBMS katmanı `Hybris`arasında daha büyük bir ağ gecikmesi getiren dağıtım senaryoları. Buna aşağıdakiler dahildir:
    - Katmanlardan birini şirket içinde dağıtma, diğer katman ise Azure'da dağıtılır
    - Bir sistemin SAP uygulama katmanını DBMS katmanından farklı bir Azure bölgesinde dağıtma
    - Azure'da ve Azure'da diğer katmanda birlikte bulunan veri merkezlerinde, bu tür bir mimari desenlerin Azure yerel hizmeti tarafından sağlandığı durumlar dışında bir katmanı dağıtma
    - SAP uygulama katmanı ile DBMS katmanı arasında ağ sanal cihazları dağıtma
    - SAP DBMS katmanı veya SAP küresel aktarım dizini için Azure veri merkezinin ortak bulunan veri merkezlerinde barındırılan depolamadan yararlanma
    - İki farklı bulut satıcısıyla iki katmanı dağıtma. Örneğin, Oracle Cloud Infrastructure'da DBMS katmanını ve Azure'da uygulama katmanını dağıtma
- Çok ÖrnekLI HANA Pacemaker küme yapılandırmaları
- Windows'da desteklenen SAP veritabanları için ANF'de SOFS veya SMB aracılığıyla paylaşılan disklere sahip Windows Cluster yapılandırmaları. Bunun yerine, belirli veritabanlarının yerel yüksek kullanılabilirlik çoğaltma kullanımını öneririz ve ayrı depolama yığınları kullanın
- SAP HANA hariç ANF'nin üstünden NFS hisselerinde bulunan veritabanı dosyalarıyla Linux'ta desteklenen SAP veritabanlarının dağıtımı
- Oracle DBMS'nin Windows ve Oracle Linux'tan başka herhangi bir konuk işletim sistemi üzerinde dağıtımı. Ayrıca bakınız [SAP destek notu #2039619](https://launchpad.support.sap.com/#/notes/2039619)

Test etmediğimiz ve bu nedenle liste yle ilgili hiçbir deneyimimiz olmadığı senaryo(lar) :

- Azure Site Kurtarma çoğaltma DBMS katmanı VMs. Sonuç olarak, olası olağanüstü durum kurtarma yapılandırması için veritabanı nın yerel asynchronous çoğaltma işlevini kullanmanızı öneririz
 

## <a name="next-steps"></a>Sonraki Adımlar
[SAP NetWeaver için Azure Sanal Makineler planlaması ve uygulamasında](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/planning-guide) sonraki adımları okuyun




  



