---
title: Azure'da IBM DB2 pureScale
description: Bu makalede, Azure'da IBM DB2 pureScale ortamını çalıştırmak için bir mimari gösteriyoruz.
author: njray
manager: edprice
editor: edprice
ms.service: virtual-machines-linux
ms.subservice: workloads
ms.workload: infrastructure-services
ms.topic: article
ms.date: 11/09/2018
ms.author: edprice
ms.openlocfilehash: d8309a69c9c38610fa7bea3fee202a60d836980c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78945046"
---
# <a name="ibm-db2-purescale-on-azure"></a>Azure'da IBM DB2 pureScale

IBM DB2 pureScale ortamı, Linux işletim sistemlerinde yüksek kullanılabilirlik ve ölçeklenebilirlik sağlayan Azure için bir veritabanı kümesi sağlar. Bu makalede, Azure'da DB2 pureScale çalıştırmak için bir mimari gösterilmektedir.

## <a name="overview"></a>Genel Bakış

İşletmeler uzun kendi online işlem işleme (OLTP) ihtiyaçlarını karşılamak için geleneksel ilişkisel veritabanı yönetim sistemi (RDBMS) platformları kullandık. Günümüzde pek çok kişi, kapasiteyi artırmak, maliyetleri azaltmak ve sabit bir operasyonel maliyet yapısını korumak için ana bilgisayar tabanlı veritabanı ortamlarını Azure'a geçiriyor. Göç genellikle eski bir platformu modernize ilk adımdır. 

Son zamanlarda, bir kurumsal müşteri z/OS'de çalışan IBM DB2 ortamını Azure'da IBM DB2 pureScale'e yeniden ev sahipliği yaptı. Db2 pureScale veritabanı küme çözümü, Linux işletim sistemlerinde yüksek kullanılabilirlik ve ölçeklenebilirlik sağlar. Müşteri, Db2 pureScale'i yüklemeden önce Azure'da büyük ölçekli bir sistemde tek bir sanal makinede (VM) bağımsız, ölçeklendirilen bir örnek olarak Db2'yi başarıyla çalıştırdı. 

Orijinal ortamla aynı olmasa da, Linux'taki IBM DB2 pureScale, ana bilgisayarda Paralel Sysplex yapılandırmasında çalışan z/OS için IBM DB2'ye benzer yüksek kullanılabilirlik ve ölçeklenebilirlik özellikleri sunar. Bu senaryoda, küme iSCSI üzerinden paylaşılan bir depolama kümesine bağlanır. Biz GlusterFS dosya sistemi, ücretsiz, ölçeklenebilir, açık kaynak dağıtılmış dosya sistemi özellikle bulut depolama için optimize kullanılır. Ancak IBM artık bu çözümü desteklemez. IBM'den desteğinizi korumak için desteklenen iSCSI uyumlu bir dosya sistemi kullanmanız gerekir. Microsoft, Depolama Alanları Doğrudan (S2D) bir seçenek olarak sunuyor

Bu makalede, bu Azure geçişi için kullanılan mimari açıklanmaktadır. Müşteri yapılandırmayı test etmek için Red Hat Linux 7.4'u kullandı. Bu sürüm Azure Marketi'nden edinilebilir. Bir Linux dağıtımı seçmeden önce, şu anda desteklenen sürümleri doğruladığından emin olun. Ayrıntılar için [IBM DB2 pureScale](https://www.ibm.com/support/knowledgecenter/SSEPGG) ve [GlusterFS](https://docs.gluster.org/en/latest/)belgelerine bakın.

Bu makale, DB2 uygulama planınız için bir başlangıç noktasıdır. İş gereksinimleriniz farklı olacaktır, ancak aynı temel model geçerlidir. Bu mimari deseni Azure'daki çevrimiçi analitik işleme (OLAP) uygulamaları için de kullanabilirsiniz.

Bu makalede, z/OS veritabanı için IBM DB2'nin Linux üzerinde çalışan IBM DB2 pureScale'e taşınması için yapılan farklar ve olası geçiş görevleri kapsamaz. Ayrıca DB2 z/OS'den DB2 pureScale'e geçmek için boyutlandırma tahminleri ve iş yükü analizleri sağlamaz. 

Ortamınız için en iyi DB2 pureScale mimarisine karar vermenize yardımcı olmak için, boyutlandırmayı tam olarak tahmin etmenizi ve bir hipotez belirlemenizi öneririz. Kaynak sistemde, db2 z/OS Paralel Sysplex'i veri paylaşım mimarisi, Bağlantı Tesisi yapılandırması ve dağıtılmış veri tesisi (DDF) kullanım istatistikleri ile göz önünde bulundurun.

> [!NOTE]
> Bu makalede, DB2 geçişine bir yaklaşım açıklanır, ancak başkaları da vardır. Örneğin, DB2 pureScale, sanallaştırılmış şirket içi ortamlarda da çalıştırılabilir. IBM, Çeşitli yapılandırmalarda Microsoft Hyper-V'de DB2'yi destekler. Daha fazla bilgi için IBM Bilgi [Merkezi'ndeki DB2 pureScale sanallaştırma mimarisine](https://www.ibm.com/support/knowledgecenter/en/SSEPGG_11.1.0/com.ibm.db2.luw.qb.server.doc/doc/r0061462.html) bakın.

## <a name="architecture"></a>Mimari

Azure'da yüksek kullanılabilirlik ve ölçeklenebilirliği desteklemek için, DB2 pureScale için ölçeklendirilebilir, paylaşılan bir veri mimarisi kullanabilirsiniz. Müşteri geçişi aşağıdaki örnek mimariyi kullandı.

![Depolama ve ağ gösteren Azure sanal makinelerde DB2 pureScale](media/db2-purescale-on-azure/pureScaleArchitecture.png "Depolama ve ağ gösteren Azure sanal makinelerde DB2 pureScale")


Diyagram, DB2 pureScale kümesi için gereken mantıksal katmanları gösterir. Bunlar, istemci, yönetim, önbelleğe alma, veritabanı altyapısı ve paylaşılan depolama için sanal makineleri içerir. 

Veritabanı motor düğümlerine ek olarak, diyagram küme önbelleğe alma tesisleri (CFs) için kullanılan iki düğüm içerir. Veritabanı altyapısı nın kendisi için en az iki düğüm kullanılır. PureScale kümesine ait bir DB2 sunucusuna üye denir. 

Küme, ölçeklendirilen depolama ve yüksek kullanılabilirlik sağlamak için iSCSI üzerinden üç düğümlü paylaşılan depolama kümesine bağlanır. DB2 pureScale, Linux çalıştıran Azure sanal makinelerine yüklenir.

Bu yaklaşım, kuruluşunuzun boyutu ve ölçeği için değiştirebileceğiniz bir şablondur. Aşağıdakilere dayanır:

-   İki veya daha fazla veritabanı üyesi en az iki CF düğümüyle birleştirilir. Düğümler, paylaşılan erişimi denetlemek için paylaşılan bellek ve genel kilit yöneticisi (GLM) hizmetleri için etkin üyelerden gelen ortak erişimi ve çekişmeyi kilitlemek için genel bir arabellek havuzunu (GBP) yönetir. Bir CF düğümü birincil, diğeri ikincil, başarısız CF düğümü olarak görev eder. Ortamda tek bir hata noktasını önlemek için, Bir DB2 pureScale kümesi en az dört düğüm gerektirir.

-   Yüksek performanslı paylaşılan depolama alanı (diyagramda P30 boyutunda gösterilir). Her düğüm bu depolama alanını kullanır.

-   Veri üyeleri ve paylaşılan depolama için yüksek performanslı ağ.

### <a name="compute-considerations"></a>Hesaplama hususları

Bu mimari, Azure sanal makinelerinde uygulama, depolama ve veri katmanları çalıştırın. [Dağıtım kurulum komut dosyaları](https://aka.ms/db2onazure) aşağıdakileri oluşturur:

-   Bir DB2 pureScale kümesi. Azure'da gereksinim duyduğunuz işlem kaynaklarının türü kurulumunuza bağlıdır. Genel olarak, iki yaklaşım kullanabilirsiniz:

    -   Küçük ve orta ölçekli örneklerin paylaşılan depolama alanına eriştiği çok düğümlü, yüksek performanslı bilgi işlem (HPC) tarzı bir ağ kullanın. Bu HPC yapılandırma türü için, Azure bellek için optimize edilmiş E serisi veya depolama için optimize edilmiş L serisi [sanal makineler](https://docs.microsoft.com/azure/virtual-machines/windows/sizes) gerekli bilgi işlem gücünü sağlar.

    -   Veri motorları için daha az büyük sanal makine örneği kullanın. Büyük durumlarda, bellek için optimize edilmiş en büyük [M serisi](https://azure.microsoft.com/pricing/details/virtual-machines/series/) sanal makineler ağır bellek içi iş yükleri için idealdir. DB2'yi çalıştırmak için kullanılan mantıksal bölümün (LPAR) boyutuna bağlı olarak özel bir örneğe ihtiyacınız olabilir.

-   DB2 CF, E serisi veya L serisi gibi bellek için optimize edilmiş sanal makineler kullanır.

-   Linux çalıştıran Standart\_DS4\_v2 sanal makineleri kullanan paylaşılan bir depolama kümesi.

-   Yönetim jumpbox Linux\_çalışan bir\_Standart DS2 v2 sanal makinedir.  Alternatif, sanal ağınızdaki tüm VM'ler için güvenli bir RDP/SSH deneyimi sağlayan bir hizmet olan Azure Bastion'dur.

-   İstemci,\_Windows çalıştıran (sınama için kullanılan) standart ds3\_v2 sanal makinedir.

-   *İsteğe bağlı*. Tanık sunucusu. Bu sadece Db2 pureScale bazı önceki sürümleri ile gereklidir. Bu örnekte\_Linux çalıştıran standart ds3\_v2 sanal makine (DB2 pureScale için kullanılır) kullanılır.

> [!NOTE]
> Bir DB2 pureScale küme en az iki DB2 örneği gerektirir. Ayrıca bir önbellek örneği ve kilit yöneticisi örneği gerektirir.

### <a name="storage-considerations"></a>Depolama hakkında dikkat edilmesi gerekenler

Oracle RAC gibi, DB2 pureScale yüksek performanslı blok G /Ç, ölçeklendirme veritabanıdır. İhtiyaçlarınıza uygun en büyük [Azure premium SSD](disks-types.md) seçeneğini kullanmanızı öneririz. Daha küçük depolama seçenekleri geliştirme ve test ortamları için uygun olabilir, üretim ortamları ise genellikle daha fazla depolama kapasitesine ihtiyaç duyar. Örnek mimari, IOPS'nin boyut ve fiyata oranı nedeniyle [P30](https://azure.microsoft.com/pricing/details/managed-disks/) kullanır. Boyutundan bağımsız olarak, en iyi performans için Premium Depolama'yı kullanın.

DB2 pureScale, tüm verilerin tüm küme düğümlerinden erişilebildiği paylaşılan her şey mimarisini kullanır. Premium depolama, isteğe bağlı olarak veya özel durumlarda birden çok örnekte paylaşılmalıdır.

Büyük bir DB2 pureScale küme 200 terabayt (TB) veya daha fazla prim paylaşılan depolama, 100.000 IOPS ile gerektirebilir. DB2 pureScale, Azure'da kullanabileceğiniz bir iSCSI blok arabirimini destekler. iSCSI arabirimi, S2D veya başka bir araçla uygulayabileceğiniz paylaşılan bir depolama kümesi gerektirir. Bu tür bir çözüm, Azure'da sanal depolama alanı ağı (vSAN) aygıtı oluşturur. DB2 pureScale, sanal makineler arasında veri paylaşmak için kullanılan kümelenmiş dosya sistemini yüklemek için vSAN'ı kullanır.

### <a name="networking-considerations"></a>Ağ konusunda dikkat edilmesi gerekenler

IBM, DB2 pureScale kümesindeki tüm üyeler için InfiniBand ağ lamanızı önerir. DB2 pureScale, MÜMKÜN olan yerlerde, CF'ler için uzaktan doğrudan bellek erişimi (RDMA) da kullanır.

Kurulum sırasında, tüm sanal makineleri içeren bir Azure [kaynak grubu](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview) oluşturursunuz. Genel olarak, kaynakları yaşamlarına ve bunları kimin yöneteceğine göre gruplarsınız. Bu mimarideki sanal makineler [hızlandırılmış ağ](https://azure.microsoft.com/blog/maximize-your-vm-s-performance-with-accelerated-networking-now-generally-available-for-both-windows-and-linux/)gerektirir. Sanal makineye tek köklü G/Ç sanallaştırma (SR-IOV) aracılığıyla tutarlı, ultra düşük ağ gecikmesi sağlayan bir Azure özelliğidir.

Her Azure sanal makinesi alt ağları olan sanal bir ağa dağıtılır: ana, Gluster FS ön uç (gfsfe), Gluster FS arka uç (bfsbe), DB2 pureScale (db2be) ve DB2 pureScale ön uç (db2fe). Yükleme komut dosyası, ana alt ağdaki sanal makinelerde birincil [NIC'leri](https://docs.microsoft.com/azure/virtual-machines/linux/multiple-nics) de oluşturur.

Sanal ağ daki ağ trafiğini kısıtlamak ve alt ağları yalıtmak için [ağ güvenlik gruplarını](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg) kullanın.

Azure'da, DB2 pureScale depolama için ağ bağlantısı olarak TCP/IP'yi kullanmanız gerekir.

## <a name="next-steps"></a>Sonraki adımlar

-   [Bu mimariyi Azure'da dağıtma](deploy-ibm-db2-purescale-azure.md)
