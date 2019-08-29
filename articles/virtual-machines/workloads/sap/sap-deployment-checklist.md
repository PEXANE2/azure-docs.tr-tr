---
title: SAP iş yükü planlama ve dağıtım denetim listesi | Microsoft Docs
description: Azure 'da SAP iş yükünün planlanması ve dağıtımı için denetim listesi
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
ms.date: 07/15/2019
ms.author: juergent
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 2d6170dead4e8577cea6883ffea25b90ebe39b88
ms.sourcegitcommit: 44e85b95baf7dfb9e92fb38f03c2a1bc31765415
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/28/2019
ms.locfileid: "70079003"
---
# <a name="sap-workload-on-azure-planning-and-deployment-checklist"></a>Azure planlama ve dağıtım denetim listesi üzerinde SAP iş yükü 

Bu denetim listesi, SAP NetWeaver, S/4HANA ve Hybrsıs uygulamalarını bir hizmet olarak Azure altyapısına taşıyan müşteriler için tasarlanmıştır.  Bu denetim listesi, proje süresince bir müşteri ve/veya SAP iş ortağı tarafından incelenmelidir. Denetimlerin çoğunun projenin başlangıcında ve planlama aşamasında gerçekleştirildiği önemli bir öneme sahiptir. Dağıtım yapıldıktan sonra, dağıtılan Azure altyapısında veya SAP yazılım sürümlerindeki temel değişiklikler karmaşık hale gelebilir. Bu denetim listesini bir proje boyunca anahtar kilometre taşları kısmında gözden geçirin.  Küçük sorunlar, büyük sorunlar olmadan ve gerekli değişiklikleri yeniden mühendisler ve test etmek için yeterli zaman mevcut olmaya başlamadan önce algılanabilir. Talep listesinin tamamlanmadığına hiçbir yol yoktur. Tek durumunuza bağlı olarak, gerçekleştirilmesi gereken pek çok denetim olabilir. 

Birleştirilen denetim listesi, Azure 'dan bağımsız olan görevleri içermez.  Örnek: SAP uygulama arabirimleri, Azure genel bulutuna veya bir barındırma sağlayıcısına geçiş sırasında değişir.    

Bu denetim listesi, zaten dağıtılan sistemler için de kullanılabilir. Yazma Hızlandırıcısı, Kullanılabilirlik Alanları ve yeni VM türleri gibi yeni özellikler, dağıtıldıktan sonra eklenmiş olabilir.  Bu nedenle, Azure platformunda yeni özelliklerden haberdar olduğunuzdan emin olmak için denetim listesini düzenli aralıklarla gözden geçirmeniz yararlı olur. 

## <a name="project-preparation-and-planning-phase"></a>Proje hazırlama ve planlama aşaması
Bu aşamada, SAP iş yükünün Azure genel bulutuna geçirilmesi planlanmaktadır. Aşağıda belirtildiği gibi en düşük varlık ve öğe kümesi ve liste:

1. Üst düzey tasarım belgesi – Bu belge şunları içermelidir:
    1. Azure 'da SAP bileşenlerinin ve uygulamalarının geçerli envanteri ve hedef uygulama envanteri
    2. Dahil edilen farklı tarafların sorumluluklarını ve atamalarını tanımlayan bir sorumluluk atama matrisi (KCI) oluşturun ve bu matris üzerinden çalışın. Yüksek düzeyde başlayın ve daha fazla ve daha ayrıntılı düzeyler için planlama ve ilk dağıtımları işleme
    2. Üst düzey bir çözüm mimarisi
    3. Uygulamasına dağıtılacak Azure bölgelerinde karar. Azure bölgelerinin listesi için [Azure bölgelerini](https://azure.microsoft.com/global-infrastructure/regions/)inceleyin. Azure bölgelerinin her birinde kullanılabilen hizmetler için, [bölge başına kullanılabilen makale ürünlerini](https://azure.microsoft.com/global-infrastructure/services/) denetleyin
    4. Şirket içinden Azure 'a bağlanmak için ağ mimarisi. [Azure Için sanal veri merkezi şeması](https://docs.microsoft.com/azure/architecture/vdc/) hakkında bilgi sahibi olmak için başlatın
    5. Azure 'da yüksek iş etkisi verileri çalıştırmaya yönelik güvenlik ilkeleri. [Azure Güvenlik belgeleriyle](https://docs.microsoft.com/azure/security/) başlamak için malzeme okuma
2.  Teknik tasarım belgesi – şunları içerir:
    1.  Bir çözüm blok diyagramı 
    2.  Azure 'da işlem, depolama ve ağ bileşenlerini boyutlandırma. Azure VM 'lerinin SAP boyutlandırması için, SAP destek notuna bakın [#1928533](https://launchpad.support.sap.com/#/notes/1928533) 
    3.  İş sürekliliği ve olağanüstü durum kurtarma mimarisi
    4.  Ayrıntılı işletim sistemi, DB, çekirdek ve SAP destek paketi sürümleri. SAP NetWeaver veya S/4HANA tarafından desteklenen herhangi bir işletim sistemi sürümünün Azure VM 'lerinde desteklendiği belli değildir. Aynı, DBMS sürümleri için de geçerlidir. SAP sürümlerini, DBMS sürümlerini veya işletim sistemi sürümlerini SAP ve Azure tarafından desteklenen bir pencerede olacak şekilde yükseltmek ve gerekirse, aşağıdaki kaynakların hizalanması ve gerekli olması zorunludur. SAP ve Microsoft tarafından tam destek almak için SAP ve Azure tarafından desteklenen yayın birleşimleri dahilinde olmanız zorunludur. Gerekirse, bazı yazılım bileşenlerinden bazılarını yükseltmeyi planlamanız gerekir. Desteklenen SAP, OS ve DBMS yazılımları hakkında daha fazla ayrıntı şu konumlarda belgelenmiştir:
        1.  SAP destek notunun [#1928533](https://launchpad.support.sap.com/#/notes/1928533). Bu notta, Azure VM 'lerinde desteklenen en düşük işletim sistemi sürümleri tanımlanmaktadır. Ayrıca, HANA olmayan çoğu veritabanı için gereken en düşük veritabanı sürümlerini tanımlar. Bu notta, SAP tarafından desteklenen farklı Azure VM türlerinin SAP boyutlandırma de gösterilmektedir.
        2.  SAP destek notunun [#2039619](https://launchpad.support.sap.com/#/notes/2039619). Bu notta, Azure 'da Oracle desteği matrisi tanımlanmaktadır. Oracle 'ın yalnızca Windows ve Oracle Linux SAP iş yükü için Azure 'da Konuk işletim sistemi olarak desteklediğini unutmayın. Bu destek beyanı, SAP örnekleri çalıştıran SAP uygulama katmanı için de geçerlidir. Ancak, Oracle, pacemaker üzerinden Oracle Linux SAP Merkezi Hizmetleri için yüksek kullanılabilirliği desteklemez. Oracle Linux yoks için yüksek kullanılabilirlik gerekiyorsa, Linux için SIOS koruma paketi ' ni kullanmanız gerekir. Ayrıntılı SAP sertifika verileri için, SAP destek notunun [#1662610-Linux IÇIN SIOS koruma paketine yönelik destek ayrıntıları](https://launchpad.support.sap.com/#/notes/1662610)' na bakın. Windows için, SAP Merkezi Hizmetleri için desteklenen SAP Windows Yük devretme kümesi yük devretme çözümü, Oracle AS DBMS katmanı ile birlikte desteklenir. 
        3.  Farklı işletim sistemi sürümlerindeki SAP HANA destek matrisini almak için SAP destek notuna [#2235581](https://launchpad.support.sap.com/#/notes/2235581)
        4.  Desteklenen Azure VM 'Leri ve [Hana büyük örnekleri](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-overview-architecture) [burada](https://www.sap.com/dmc/exp/2014-09-02-hana-hardware/enEN/iaas.html#categories=Microsoft%20Azure) listelenmiştir SAP HANA
        5.  [SAP ürün kullanılabilirliği matrisi](https://support.sap.com/en/)
        6.  Diğer SAP özgü ürünler için diğer SAP notları  
    5.  SAP üretim sistemleri için katı 3 katmanlı tasarımlar yapmanızı öneririz. ASCS + APP Servers 'ın aynı VM üzerinde birleştirilmesi önerilmez.  SAP Merkezi Hizmetleri için çok SID küme yapılandırmalarının kullanılması, Azure 'da Konuk işletim sistemi olarak Windows ile desteklenir. SAP Merkezi Hizmetler çoklu SID küme yapılandırmalarına Azure 'da Linux işletim sistemleri de desteklenmez. Windows Konuk işletim sistemi örneğine yönelik belgeler şu şekilde bulunabilir:
        1.  [SAP ASCS/SCS örneği Windows Server Yük Devretme Kümelemesi ve paylaşılan disk ile Azure üzerinde çok düzeyli yüksek kullanılabilirlik](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-ascs-ha-multi-sid-wsfc-shared-disk)
        2.  [SAP ASCS/SCS örneği Windows Server Yük Devretme Kümelemesi ve dosya paylaşımıyla Azure 'da yüksek oranda kullanılabilirlik](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-ascs-ha-multi-sid-wsfc-file-share)
    6.  Yüksek kullanılabilirlik ve olağanüstü durum kurtarma mimarisi
        1.  Yüksek kullanılabilirlik ve olağanüstü durum kurtarma mimarisinin benzer şekilde görünmesi için, RTO ve RPO tabanlı ' i tanımlama
        2.  Aynı bölgedeki yüksek kullanılabilirlik için, istenen DBMS 'nin Azure 'da sunmasına ne olduğunu denetleyin. Çoğu DBMS, üretim sistemleri için önerdiğimiz, zaman uyumlu bir etkin bekleme, zaman uyumlu yöntemler sunar. Ayrıca, [SAP iş yükü ve ilgili belgeler Için Azure sanal MAKINELERI DBMS dağıtımı hakkında bazı hususlar](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/dbms_guide_general) ile başlayan farklı VERITABANLARıNA yönelik SAP ile ilgili belgelere bakın
            1.  Örneğin, [burada](https://docs.microsoft.com/sql/sql-server/failover-clusters/windows/always-on-failover-cluster-instances-sql-server?view=sql-server-2017) SQL Server açıklananlar gıbı, DBMS katmanının paylaşılan disk yapılandırmasıyla Windows Yük devretme kümesi hizmeti 'ni kullanmak desteklenmez. Bunun yerine, şunun gibi çözümler:
                1.  [SQL Server AlwaysOn](https://docs.microsoft.com/azure/virtual-machines/windows/sqlclassic/virtual-machines-windows-classic-ps-sql-alwayson-availability-groups) 
                2.  [Oracle Data Guard](https://docs.microsoft.com/azure/virtual-machines/workloads/oracle/configure-oracle-dataguard)
                3.  [HANA sistem çoğaltması](https://help.sap.com/viewer/6b94445c94ae495c83a19646e7c3fd56/2.0.01/en-US/b74e16a9e09541749a745f41246a065e.html)
        3.  Farklı Azure bölgelerinde olağanüstü durum kurtarma için, farklı DBMS satıcıları tarafından hangi olasılıkların sunulmakta olduğunu kontrol edin. Çoğu zaman uyumsuz çoğaltmayı veya günlük dağıtımını destekler
        4.  SAP uygulama katmanı için, aynı Azure bölgesindeki veya DR bölgesindeki üretim dağıtımlarınızın çoğaltmaları ideal olan iş gerileme test sistemlerinizi çalıştırıp çalıştırmayacağını tanımlayın. İkinci durumda, bu iş gerileme sistemini üretim için DR hedefi olarak hedefleyebilirsiniz
        5.  Üretim dışı sistemleri DR sitesine yerleştirmemeye karar verirseniz, SAP uygulama katmanını Azure DR bölgesine çoğaltmak için uygun bir yöntem olarak Azure Site Recovery bakın. Ayrıca bkz. [çok KATMANLı SAP NetWeaver uygulama dağıtımı için olağanüstü durum kurtarmayı ayarlama](https://docs.microsoft.com/azure/site-recovery/site-recovery-sap) 
        6.  Birleştirilmiş bir HA/DR yapılandırması kullanmaya karar verirseniz [Azure kullanılabilirlik alanları](https://docs.microsoft.com/azure/availability-zones/az-overview) , yukarıdaki kullanılabilirlik alanları kullanılabildiği Azure bölgelerini ve artan ağ gecikmelerinin tanıtılmasına neden olan kısıtlamaları öğrenerek kendi kendinize tanıdık getirin iki Kullanılabilirlik Alanları arasında  
3.  Müşterinin/ortağın tüm SAP arabirimlerinin (SAP ve SAP olmayan) envanterini oluşturması gerekir. 
4.  Foundation Services tasarımının tasarımı-bu tasarım, şunun gibi öğeler içerir
    1.  Active Directory ve DNS tasarımı
    2.  Azure içindeki ağ topolojisi ve farklı SAP sistemleri atama
    3.  Azure 'da altyapı ve SAP uygulamalarını yöneten farklı ekipleriniz için [rol tabanlı erişim](https://docs.microsoft.com/azure/role-based-access-control/overview) yapısı
    3.  Kaynak grubu topolojisi 
    4.  [Etiketleme stratejisi](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags#tags-and-billing)
    5.  VM 'Ler ve diğer altyapı bileşenleri ve/veya mantıksal adlar için adlandırma kuralı
5.  Microsoft Premier Destek Sözleşmesi – MS Technical Account Manager 'ı (TAM) belirler. SAP tarafından destek gereksinimleri için SAP destek notuna göz [#2015553](https://launchpad.support.sap.com/#/notes/2015553) 
6.  Farklı abonelikler için Azure abonelik ve çekirdek kota sayısını tanımlayın. Gerektiğinde [Azure aboneliklerinin kotalarını artırmak için destek Istekleri açın](https://docs.microsoft.com/azure/azure-supportability/resource-manager-core-quotas-request) 
7.  SAP verilerini Azure 'a geçirmek için veri azaltma ve veri geçişi planı. SAP NetWeaver sistemlerinde SAP, çok sayıda verinin sınırlı tutulması hakkında yönergeler içerir. SAP, SAP ERP sistemlerinde veri yönetimiyle ilgili [Bu profili yayımlamıştır](https://help.sap.com/http.svc/rc/2eb2fba8f8b1421c9a37a8d7233da545/7.0/en-US/Data_Management_Guide_Version_70E.PDF) . Ancak, bazı içerikler genel olarak NetWeaver ve S/4HANA sistemleri için de geçerlidir.
8.  Otomatikleştirilmiş dağıtım yaklaşımını tanımlayın ve belirleyin. Azure üzerinde altyapı dağıtımlarının arkasında Otomasyon hedefi, belirleyici bir şekilde dağıtılır ve belirleyici sonuçlar elde olmaktır. Birçok müşteri, Power Shell veya CLı tabanlı betikleri kullanır. Ancak SAP için Azure altyapısını dağıtmak ve hatta SAP yazılımı yüklemek için kullanılabilen çeşitli açık kaynaklı teknolojiler vardır. Örnek, GitHub 'da bulunabilir:
    1.  [Azure bulutta otomatik SAP dağıtımları](https://github.com/Azure/sap-hana)
    2.  [SAP HANA yükleme](https://github.com/AzureCAT-GSI/SAP-HANA-ARM)
9.  Müşteri, Sistem Tümleştirici, Microsoft ve diğer ilgili taraflar arasında bir normal tasarım ve dağıtım incelemesi temposunda tanımlayın

 
## <a name="pilot-phase-strongly-recommended"></a>Pilot aşaması (önemle önerilir)
 
Pilot, proje planlama ve hazırlığı için daha önce veya paralel olarak çalıştırılabilir. Aşama, planlama ve hazırlık aşamasında yapılan yaklaşımları ve tasarımı test etmek için de kullanılabilir. Pilot aşaması, gerçek bir kavram kanıtını uzatılaşmış olabilir. Bir tam HA/DR çözümünün ve bir pilot dağıtımı sırasında güvenlik tasarımının ayarlanması ve doğrulanması önerilir. Bazı müşteri durumlarında ölçeklenebilirlik testleri de bu aşamada gerçekleştirilebilir. Diğer müşteriler, SAP Sandbox sistemlerinin pilot aşama olarak dağıtımını kullanır. Bu nedenle, bir pilot çalıştırmaya yönelik olarak Azure 'a geçirmek istediğiniz bir sistem tanımlamış olursunuz.

1. Veri aktarımını Azure 'a iyileştirin. Müşteri taleplerine son derece bağlı, Express devresi 'ın yeterli bant genişliğine sahip olması durumunda Şirket içinden [Azure ExpressRoute](https://azure.microsoft.com/services/expressroute/) üzerinden aktarım hızlıydı. Diğer müşterilerle, internet iletişime 'dan daha hızlı geçeceğiz
2. SAP heterojen platform geçişi söz konusu olduğunda veritabanı verilerinin dışarı ve içeri aktarılmasını, test yapmayı ve dışarı aktarma ve içeri aktarma aşamalarını iyileştirmenizi içerir. Hedef platform olarak SQL Server içeren büyük geçişler için, öneriler [burada](https://techcommunity.microsoft.com/t5/Running-SAP-Applications-on-the/SAP-OS-DB-Migration-to-SQL-Server-8211-FAQ-v6-2-April-2017/ba-p/368070)bulunabilir. Geçişi bir SAP sürüm yükseltmesi ile birleştirdiğinizde ve belirli kaynak ve Hedef DBMS platformu kombinasyonlarını şu şekilde karşıladığınızda, geçiş Izleyici/SWPM 'nin yaklaşımını, birleştirilmiş bir sürüm yükseltmesi veya [SAP dmo](https://blogs.sap.com/2013/11/29/database-migration-option-dmo-of-sum-introduction/) işlemine gerek kalmaz Örneğin, [2,0 SP03 Sum for Database geçiş seçeneğinde (dmo)](https://launchpad.support.sap.com/#/notes/2631152)belgelenmiştir. 
   1.  Kaynağa aktarın, dosyayı karşıya yükleyin ve Azure 'a aktarın ve performansı Içeri aktarın.  Dışarı aktarma ve içeri aktarma arasındaki üst düzeye çıkarma
   2.  Altyapı boyutlandırılmasına yansıtmak için hedef ve hedef platform arasında veritabanı hacmini değerlendirin    
   3.  Zamanlamayı doğrulama ve iyileştirme 
3. Teknik doğrulama 
   1. Sanal Makine Türleri
      1.  Azure için desteklenen VM 'lerde, bu sanal makine türleri için desteklenen işletim sistemi yayınlarına ve desteklenen SAP ve DBMS yayınlarına hiçbir değişiklik olmadığından emin olmak için SAP destek notları, SAP HANA donanım dizini ve SAP PAM 'deki kaynakları yeniden doğrulayın
      2.  Uygulamanızın ve Azure 'da dağıttığınız altyapının boyutunu yeniden doğrulayın. Mevcut uygulamaları taşımak için çoğunlukla, kullandığınız altyapıdan ve [SAP kıyaslama Web sayfasından](https://www.sap.com/dmc/exp/2018-benchmark-directory/#/sd) gerekli SAPS 'yi türetebilir ve bunu sap destek Not#1928533 LISTELENEN SAPS numaralarıyla karşılaştırabilirsiniz. [](https://launchpad.support.sap.com/#/notes/1928533) Ayrıca [Bu makaleyi](https://techcommunity.microsoft.com/t5/Running-SAP-Applications-on-the/SAPS-ratings-on-Azure-VMs-8211-where-to-look-and-where-you-can/ba-p/368208) göz önünde bulundurun
      3.  Azure VM 'lerinizi, planlama aşamasında seçtiğiniz farklı VM türlerinin en fazla depolama alanı işleme ve ağ verimlilik ile ilgili olarak boyutlandırmayı değerlendirin ve test edin. Veriler şu şekilde bulunabilir:
          1.  [Azure 'Da Windows sanal makineleri Için boyutlar](https://docs.microsoft.com/azure/virtual-machines/windows/sizes?toc=%2fazure%2fvirtual-network%2ftoc.json). Boyut için **önbelleğe alınmamış maksimum disk aktarım hızını** göz önünde bulundurmanız önemlidir
          2.  [Azure 'Da Linux sanal makineleri Için boyutlar](https://docs.microsoft.com/azure/virtual-machines/linux/sizes?toc=%2fazure%2fvirtual-network%2ftoc.json) Boyut için **önbelleğe alınmamış maksimum disk aktarım hızını** göz önünde bulundurmanız önemlidir
   2. Depolama
      1.  SAP uygulama katmanlarını ve performansa duyarlı olmayan DBMS dağıtımı için en düşük VM 'Ler için [Azure Standart SSD depolama](https://docs.microsoft.com/azure/virtual-machines/windows/disks-types#standard-ssd) kullanın
      2.  [Azure Standart HDD disklerini](https://docs.microsoft.com/azure/virtual-machines/windows/disks-types#standard-hdd) genel olarak kullanmayı öneriyoruz
      2.  Uzaktan performansa duyarlı tüm DBMS VM 'Leri için [Azure Premium Depolama](https://docs.microsoft.com/azure/virtual-machines/windows/disks-types#premium-ssd) kullanma
      2.  [Azure yönetilen disklerini](https://azure.microsoft.com/services/managed-disks/) kullanma
      3.  D serisi olan DBMS günlük sürücüleri için Azure Yazma Hızlandırıcısı kullanın. [Yazma Hızlandırıcısı](https://docs.microsoft.com/azure/virtual-machines/linux/how-to-enable-write-accelerator) ' de belgelendiği gibi, yazma Hızlandırıcısı limitlerinin ve kullanımının farkında olun
      4.  Farklı DBMS türleri için, Genel SAP ile [ılgılı DBMS belgelerini](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/dbms_guide_general) ve genel belgeyi Işaret eden DBMS 'ye özgü belgeleri denetleyin
      5.  SAP HANA için, [Azure 'da SAP HANA altyapı yapılandırmalarında ve işlemlerinde](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-vm-operations) daha fazla ayrıntı belgelenmiştir
      6.  Azure veri disklerini, cihaz KIMLIĞI kullanarak bir Azure Linux sanal makinesine hiçbir şekilde bağlama. Bunun yerine, evrensel benzersiz tanımlayıcıyı (UUID) kullanın. Örneğin, Azure veri disklerini bağlamak için grafiksel araçlar kullanırken dikkatli olun. Disklerin UUID kullanılarak bağlandığından emin olmak için/etc/fstab içindeki girdileri çift işaretleyin
          1.  Daha fazla ayrıntı bulunabilir [burada](https://docs.microsoft.com/azure/virtual-machines/linux/attach-disk-portal#connect-to-the-linux-vm-to-mount-the-new-disk)
   3. Ağ
      1.  VNet altyapınızı ve farklı Azure sanal ağlarında veya içinde SAP uygulamalarınızın dağıtımını test edin ve değerlendirin
          1.  Tek bir Azure sanal ağı içindeki hub ve bağlı bileşen sanal ağ mimarisinin veya mikro segmentinin yaklaşımını temel alarak değerlendirin
              1.  Eşlenen [Azure sanal](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview)ağları arasındaki veri alışverişi nedeniyle maliyetler. Maliyetler için [sanal ağ fiyatlandırmasını](https://azure.microsoft.com/pricing/details/virtual-network/) denetleyin
              2.  Sanal ağ alt ağında barındırılan uygulamaların veya VM 'Lerin bir güvenlik riskine neden olduğu durumlarda bir sanal ağ içindeki bir alt ağı yalıtmak için, NSG 'yi değiştirme ile karşılaştırıldığında Azure sanal ağları arasındaki eşlemenin daha hızlı kesilme avantajı
              3.  Şirket içi, dış dünya ve Azure 'da oluşturduğunuz sanal veri merkezi arasındaki ağ trafiğini merkezi olarak günlüğe kaydetme ve denetleme
          2.  SAP uygulama katmanı ve SAP DBMS katmanı arasındaki veri yolunu değerlendirin ve test edin. 
              1.  SAP uygulaması ile SAP NetWeaver, Hybrsıs veya S/4HANA tabanlı SAP sistemlerinin DBMS katmanı arasındaki iletişim yolundaki [Azure ağ sanal](https://azure.microsoft.com/solutions/network-appliances/) gereçlerinin herhangi bir yeri desteklenmez
              2.  SAP uygulama katmanının ve SAP DBMS 'nin eşlenmiş olmayan farklı Azure sanal ağlarına yerleştirilmesi desteklenmez
              3.  SAP uygulama katmanı ve SAP DBMS katmanı arasındaki yolların tanımlanması için [Azure ASG ve NSG kuralları](https://docs.microsoft.com/azure/virtual-network/security-overview) desteklenir
          3.  SAP uygulama katmanında ve SAP DBMS katmanında kullanılan VM 'lerde [Azure hızlandırılmış ağ](https://azure.microsoft.com/blog/maximize-your-vm-s-performance-with-accelerated-networking-now-generally-available-for-both-windows-and-linux/) 'ın etkinleştirildiğinden emin olun. Azure 'da hızlandırılmış ağı desteklemek için farklı işletim sistemi düzeylerinin gerekli olduğunu aklınızda bulundurun:
              1.  Windows Server 2012 R2 veya daha yeni sürümler
              2.  SUSE Linux 12 SP3 veya daha yeni sürümleri
              3.  RHEL 7,4 veya daha yeni sürümler
              4.  Oracle Linux 7,5. RHCKL çekirdeğini kullanarak, sürümün 3.10.0-862.13.1. EL7 olması gerekir. Oracle UEK çekirdek sürümü 5 ' in kullanılması gerekir
          4.   Sap desteği notuna [#500235](https://launchpad.support.sap.com/#/notes/500235) ve sap destek notuna [#1100926](https://launchpad.support.sap.com/#/notes/1100926/E), SAP uygulama KATMANı VM 'si ve DBMS VM arasındaki ağ gecikmesini test edin ve değerlendirin. Sonuçları, SAP desteği notunun [#1100926](https://launchpad.support.sap.com/#/notes/1100926/E)ağ gecikme Kılavuzu ' na göre değerlendirin. Ağ gecikmesi orta ve iyi bir aralıkta olmalıdır. [Burada](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-network-architecture#networking-architecture-for-hana-large-instance) belgelendiği gibi, VM 'ler ve Hana büyük örnek birimleri arasındaki trafik için özel durumlar geçerlidir
          5.   ILB dağıtımlarının doğrudan sunucu dönüşü kullanacak şekilde ayarlandığından emin olun. Bu ayar, Azure ılbs 'ın, DBMS katmanında yüksek kullanılabilirlik yapılandırmalarında kullanıldığı durumlarda gecikme süresini azaltır
          6.   Linux konuk işletim sistemleriyle birlikte Azure Load Balancer kullanıyorsanız, **net. IPv4. TCP _Zaman damgaları** Linux ağ parametresinin **0**olarak ayarlandığından emin olun. SAP notunun eski sürümlerindeki önerilere karşı [#2382421](https://launchpad.support.sap.com/#/notes/2382421). SAP Note, parametrenin Azure yük dengeleyiciler ile birlikte çalışmak için 0 olarak ayarlanması gerektiğini yansıtacak şekilde güncelleştirilir.
          7.   En iyi ağ gecikmesini elde etmek için [SAP uygulamalarıyla en iyi ağ gecikme süresi Için Azure yakınlık yerleşimi grupları](sap-proximity-placement-scenarios.md) makalesinde açıklandığı gibi [Azure yakınlık yerleşimi grubunu](https://docs.microsoft.com/azure/virtual-machines/linux/co-location) kullanmayı göz önünde bulundurun.
   4. Yüksek kullanılabilirlik ve olağanüstü durum kurtarma dağıtımları. 
      1. SAP uygulama katmanını belirli bir Azure kullanılabilirlik bölgesi belirtmeden dağıtırsanız, SAP iletişim kutusu örneği veya tek bir SAP sisteminin ara yazılım örneklerini çalıştıran tüm VM 'Lerin bir [kullanılabilirlik kümesinde](https://docs.microsoft.com/azure/virtual-machines/windows/manage-availability)dağıtıldığından emin olun. 
         1.   SAP Merkezi Hizmetleri ve DBMS için yüksek kullanılabilirlik gerektirmeniz durumunda, bu VM 'Ler SAP uygulama katmanıyla aynı Kullanılabilirlik kümesine dağıtılabilir
      2. SAP merkezi hizmetlerini ve DBMS katmanını pasif yinelemelerle yüksek kullanılabilirlik için koruyorduğunuzda, SAP Merkezi Hizmetleri için iki düğüme ayrı bir kullanılabilirlik kümesinde ve farklı bir kullanılabilirlik kümesindeki iki DBMS düğümüne sahip olursunuz
      3. Azure Kullanılabilirlik Alanları dağıtırsanız, kullanılabilirlik kümelerinden yararlanamaz. Ancak, etkin ve pasif Merkezi Hizmetler düğümlerini, bölgeler arasındaki en küçük gecikme süresini gösteren iki farklı Kullanılabilirlik Alanları dağıttığınızdan emin olmanız gerekir.
         1.   DBMS ve SAP Merkezi Hizmetler katmanı için Kullanılabilirlik Alanları genelinde Windows veya pacemaker yük devretme kümeleri kurmak üzere [Azure Standart Load Balancer](https://docs.microsoft.com/azure/load-balancer/load-balancer-standard-availability-zones) kullanmanız gerektiğini aklınızda bulundurun. [Temel Load Balancer](https://docs.microsoft.com/azure/load-balancer/load-balancer-overview#skus) , bölgesel dağıtımları için kullanılamaz 
   5. Zaman aşımı ayarları
      1. Farklı SAP örneklerinin SAP NetWeaver geliştirici izlemelerini denetleyin ve sıraya alma sunucusu ile SAP çalışma işlemi arasında hiçbir bağlantı kesilmesinin Not olmadığından emin olun. Bu iki kayıt defteri parametresi ayarlanarak Bu bağlantı sonlarına kaçınılabilir:
         1.   HKLM\SYSTEM\CurrentControlSet\Services\Tcpip\Parameters\KeepAliveTime = 120000-Ayrıca [Bu makaleye](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-2000-server/cc957549(v=technet.10)) bakın
         2.   Hklm\system\currentcontrolset\services\tcpip\parameters\keepaliveınterval = 120000-Ayrıca [Bu makaleye](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-2000-server/cc957548(v=technet.10)) bakın 
      2. Azure 'da dağıtılan tek şirket içi dağıtılmış SAP GUI arabirimleri ve SAP uygulama katmanları arasındaki GUI zaman aşımlarını önlemek için, aşağıdaki parametrelerin varsayılan. PFL veya örnek profilinde ayarlanmış olup olmadığını denetleyin:
         1.   rdisp/keepalive_timeout = 3600
         2.   rdisp/KeepAlive = 20
      3. SAP sıraya alma işlemi ile SAP workprocesse arasında kurulan bağlantıların kesilmesini önlemek için, EnQue/encnı/set_so_keepalive parametresinin "true" olarak ayarlanması gerekir. Ayrıca bkz. [SAP Note #2743751](https://launchpad.support.sap.com/#/notes/2743751)  
      3. Windows Yük devretme kümesi yapılandırması kullanıyorsanız, yanıt vermeyen düğümlerde tepki vermeye yönelik sürenin Azure için doğru ayarlandığından emin olun. Microsoft makale [ayarlama yük devretme kümesi ağ eşikleri](https://techcommunity.microsoft.com/t5/Failover-Clustering/Tuning-Failover-Cluster-Network-Thresholds/ba-p/371834) , parametreleri ve bu etki yük devretme sensitivities etkisini listeler. Küme düğümlerinin aynı alt ağda olduğu varsayıldığında, aşağıdaki parametrelerin değiştirilmesi gerekir:
         1.   SameSubNetDelay = 2000
         2.   SameSubNetThreshold = 15
         3.   Routinggeçmişini = 30
4. Yüksek kullanılabilirlik ve olağanüstü durum kurtarma yordamlarınızı test edin
   1. Yük devretme yapılandırmalarının tasarlanan gibi çalışıp çalışmadığını anlamak için VM 'leri (Windows Konuk işletim sistemi) veya işletim sistemlerini panik moduna (Linux konuk işletim sistemi) girerek yük devretme durumlarının benzetimini yapın. 
   2. Yük devretme yürütmek için gereken zaman sayısını ölçer. Süreler çok uzun sürsün sonra şunları göz önünde bulundurun:
      1.   SUSE Linux için, yük devretmeyi hızlandırmak üzere Azure Geçiş Aracısı yerine SBD cihazları kullanın
      2.   SAP HANA için, verilerin yeniden yüklenmesi çok uzun sürerse daha fazla depolama bant genişliği sağlamaya yarar
   3. Yedekleme/geri yükleme sırasını ve zamanlamasını test edin ve gerekirse ayarlayın. Yalnızca yedekleme sürelerinin yeterli olduğundan emin olun. Ayrıca, geri yükleme testi yapın ve geri yükleme etkinliklerinde zamanlamayı gerçekleştirin. geri yükleme sürelerinin, RTO SLA 'larınızın bir veritabanı veya VM geri yükleme işlemini temel aldığından emin olun
   4. DR işlevselliği ve mimarisi bölgesinde test etme
5. Güvenlik denetimleri
   1.  Uygulanan Azure rol tabanlı erişim (RBAC) mimarisinin geçerliliğini test edin. Amaç, farklı takımların erişimini ve izinlerini ayırmak ve sınırlamak. Örnek olarak, SAP tabanlı ekip üyeleri VM 'Leri dağıtıp Azure depolama 'dan belirli bir Azure sanal ağına disk atayabilmelidir. Ancak SAP tabanlı ekibin kendi sanal ağlarını oluşturamayacak veya mevcut sanal ağların ayarlarını değiştirememelidir. Diğer taraftan, ağ ekibinin üyeleri SAP uygulaması ve DBMS VM 'lerinin çalıştığı sanal ağlarda VM 'Leri dağıtamayacak olmalıdır. Ya da ağ ekibinin üyeleri sanal makinelerin özniteliklerini değiştirebilmeli veya VM 'Leri ya da diskleri silebilir.  
   2.  [NSG ve ASC](https://docs.microsoft.com/azure/virtual-network/security-overview) kurallarının beklenen şekilde çalıştığını ve korunan kaynakları koruduğuna emin olun
   3.  Şifrelenmesi gereken tüm kaynakların şifrelendiğinden emin olun. Sertifikaları yedeklemek, depolamak ve bu sertifikalara erişmek ve şifrelenmiş varlıkları geri yüklemek için süreçler tanımlayın ve yürütün. 
   4.  Bir işletim sistemi destek noktasından mümkün olduğunda, [Azure disk şifrelemesi](https://docs.microsoft.com/azure/security/azure-security-disk-encryption-faq) 'ni ve/veya işletim sistemi diskleri için kullanın
   5.  Çok fazla şifreleme katmanı kullanıldığını kontrol edin. Azure disk şifrelemesi 'ni ve ardından DBMS Saydam veritabanı şifreleme yöntemlerinden en üst bir kısmını kullanmak için sınırlı bir anlam getirir
6. Performans Testi
   1.  SAP izleme ve ölçümlerine dayalı SAP 'de, ilk 10 çevrimiçi raporu, geçerli olduğu durumlarda geçerli uygulamayla karşılaştırın 
   2.  SAP izleme ve ölçümlerine dayalı SAP 'de, en iyi 10 toplu işi geçerli olduğu durumlarda geçerli uygulamayla karşılaştırın 
   3.  SAP izleme ve ölçümlerine dayalı SAP 'de, arabirimler aracılığıyla veri aktarımlarını SAP sistemine karşılaştırın. Aktarımın, Şirket içinden Azure 'a geçmek gibi farklı konumlar arasında gittiğini bildiğiniz arabirimlere odaklanın 


## <a name="non-production-phase"></a>Üretim dışı aşaması 
Bu aşamada, başarılı bir pilot veya PoC sonrasında, üretim dışı SAP sistemlerini Azure 'a dağıtmaya başladığınızı varsayalım. Kavramların kanıtı olan tüm dersleri ve deneyimlerin bu tür bir dağıtıma uyarlanmalıdır. PoC 'de listelenen tüm ölçüt ve adımlar bu tür dağıtımlarda de geçerlidir. Bu aşamada, genellikle geliştirme sistemlerini, birim testi sistemlerini ve iş regresyon testi sistemlerini Azure 'a dağıtırsınız. Bir SAP uygulama satırındaki en az bir üretim dışı sisteminin, gelecekteki üretim sistemi sahip olacak şekilde tam yüksek kullanılabilirlik yapılandırmasına sahip olması önerilir. Bu aşamada göz önünde bulundurmanız gereken ek adımlar şunlardır:  

1.  Sistemleri eski platformdan Azure 'a taşımadan önce CPU kullanımı, depolama aktarım hızı ve ıOPS verileri gibi kaynak tüketim verilerini toplayın. Özellikle de DBMS katman birimlerinden, ancak uygulama katmanı birimlerinden. Ayrıca ağ ve depolama gecikmesini ölçer.
2.  Sistemlerin kullanılabilirlik kullanım süresi desenlerini kaydedin. Amaç, üretim dışı sistemlerin çok fazla kullanılabilir olması gerekip gerekmediğini veya bir haftanın veya ayın belirli aşamaları sırasında kapatılabilir üretim dışı sistemler olup olmadığını sağlamaktır
3.  Test edin ve Azure 'da VM 'niz için kendi işletim sistemi görüntülerini oluşturmak isteyip istemediğinizi veya Azure görüntü Galerisi 'nden bir görüntü kullanmak isteyip istemediğinizi tanımlayın. Azure galerisinden bir görüntü kullanıyorsanız, işletim sistemi satıcınızla destek sözleşmesini yansıtan doğru görüntüyü aldığınızdan emin olun. Azure galerileri, bazı işletim sistemi satıcıları için kendi lisans görüntülerinizi getirmelerini sağlar. Diğer işletim sistemi görüntüleri için destek, Azure tarafından teklif edilen fiyata dahildir. Kendi işletim sistemi görüntülerinizi oluşturmaya karar verirseniz, bu makalelerdeki belgeleri bulabilirsiniz:
    1.  [Bu belgeye](https://docs.microsoft.com/azure/virtual-machines/windows/capture-image-resource) göre Azure 'da dağıtılan bir Windows sanal makinesinin genelleştirilmiş bir görüntüsünü oluşturabilirsiniz
    2.  [Bu belgeye](https://docs.microsoft.com/azure/virtual-machines/linux/capture-image) göre Azure 'da dağıtılan bir Linux sanal makinesinin genelleştirilmiş bir görüntüsünü oluşturabilirsiniz
3.  Azure VM Galerisi 'nden SUSE ve Red Hat Linux görüntülerini kullanıyorsanız, Azure VM galerisinde Linux satıcıları tarafından sunulan SAP görüntülerini kullanmanız gerekir
4.  SAP 'nin Microsoft destek sözleşmeleri ile ilgili destek gereksinimlerini karşıladığınızdan emin olun. Bilgi, SAP destek Not#2015553 bulunabilir. [](https://launchpad.support.sap.com/#/notes/2015553) HANA büyük örnekleri için belge [ekleme gereksinimlerine](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-onboarding-requirements) bakın
4.  Doğru kişilerin [Planlı bakım bildirimleri](https://azure.microsoft.com/blog/a-new-planned-maintenance-experience-for-your-virtual-machines/)almasını sağlayın, bu nedenle kapalı kalma süresini ve VM 'lerin yeniden başlatılmasını seçebilirsiniz
5.  Artmasıyla, dağıtımlarınıza uygulanabilecek yeni işlevler için [Channel9](https://channel9.msdn.com/) gibi kanallar hakkında Microsoft sunumlarının Azure belgelerini denetleyin
6.  Yeni VM SKU 'Ları veya yeni desteklenen işletim sistemi ve DBMS sürümü için destek Not [#1928533](https://launchpad.support.sap.com/#/notes/1928533) gibi Azure Ile ilgili SAP notlarını kontrol edin. Yeni VM türlerini fiyatlandırma sırasında daha eski VM türleriyle karşılaştırın, bu nedenle VM 'Leri en iyi fiyat/performans oranıyla dağıtabileceksiniz
7.  Azure için desteklenen VM 'lerde, bu VM 'lerde desteklenen işletim sistemi yayınlarına ve desteklenen SAP ve DBMS yayınlarına hiçbir değişiklik olmadığından emin olmak için SAP destek notları, SAP HANA donanım dizini ve SAP PAM 'deki kaynakları doğrulayın
8.  Azure [](https://www.sap.com/dmc/exp/2014-09-02-hana-hardware/enEN/iaas.html#categories=Microsoft%20Azure) 'DAKI yeni Hana sertifikalı SKU 'ları inceleyin ve fiyatları, planladıklarıyla karşılaştırın ve sonunda daha iyi fiyat performansına sahip birimleri almak için değiştirin 
9.  Dağıtım betiklerinizi yeni VM türlerini kullanacak şekilde uyarlayın ve Azure 'un kullanmak istediğiniz yeni özelliklerini ekleyin
10. Altyapı dağıtımından sonra, sap desteği notuna [#500235](https://launchpad.support.sap.com/#/notes/500235) ve sap destek notuna [#1100926](https://launchpad.support.sap.com/#/notes/1100926/E), SAP uygulama KATMANı VM 'si ve DBMS VM arasındaki ağ gecikmesini test edin ve değerlendirin. Sonuçları, SAP desteği notunun [#1100926](https://launchpad.support.sap.com/#/notes/1100926/E)ağ gecikme Kılavuzu ' na göre değerlendirin. Ağ gecikmesi orta ve iyi bir aralıkta olmalıdır. [Burada](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-network-architecture#networking-architecture-for-hana-large-instance)belgelendiği gibi, VM 'ler ve Hana büyük örnek birimleri arasındaki trafik için özel durumlar geçerlidir. [SAP iş yükü Için Azure sanal MAKINELERI DBMS dağıtımı](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/dbms_guide_general#azure-network-considerations) ve [Azure 'da altyapı yapılandırması ve Işlemler SAP HANA](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-vm-operations) için önemli olmayan kısıtlamaların olmadığından emin olun
11. [SAP uygulamalarıyla en iyi ağ gecikmesi Için Azure yakınlık yerleşimi grupları](sap-proximity-placement-scenarios.md) makalesinde açıklandığı gibi, sanal makinelerin doğru [Azure yakınlık yerleşimi grubu](https://docs.microsoft.com/azure/virtual-machines/linux/co-location) kullanılarak dağıtıldığından emin olun
11. İş yükünü uygulamadan önce kavram kanıtı aşamasında listelenen diğer tüm denetimleri gerçekleştirin
12. İş yükü geçerli olduğundan, bu sistemlerin kaynak tüketimini Azure 'da kaydedin ve eski platformınızdan aldığınız kayıtlarla karşılaştırın. Daha büyük farklılıklar olduğunu görürseniz gelecek dağıtımların sanal makine boyutunu ayarlayın. Bir sanal makinenin downboyutlandırma, depolama ve ağ bant genişlikleri de azaltılarak emin olun:
    1.  [Azure 'Da Windows sanal makineleri Için boyutlar](https://docs.microsoft.com/azure/virtual-machines/windows/sizes?toc=%2fazure%2fvirtual-network%2ftoc.json). 
    2.  [Azure'daki Linux sanal makinesi boyutları](https://docs.microsoft.com/azure/virtual-machines/linux/sizes?toc=%2fazure%2fvirtual-network%2ftoc.json) 
13. Sistem kopyalama işlevselliği ve işlemlerinde çalışın. Amaç, bir geliştirme sistemini veya bir test sistemini kopyalamanızı kolaylaştırırken, bu sayede proje ekiplerinin hızlı bir şekilde yeni sistemler alabilir. Bu gibi görevleri gerçekleştiren bir araç olarak [SAP](https://wiki.scn.sap.com/wiki/display/ATopics/SAP+Landscape+Management+%28SAP+LaMa%29+at+a+Glance) 'yi ele alalım.
14. Bir tarafta bir vergi ayrımı olduğundan emin olmak için ekibinizin Azure rol tabanlı erişim, izinler ve süreçlerinizi iyileştirin ve odaklanmak. Diğer taraftan, tüm takımların Azure altyapısında görevlerini gerçekleştirmesini sağlamak istersiniz.
15. Personelinizin bu görevleri yürütmesini sağlamak için yüksek kullanılabilirlik ve olağanüstü durum kurtarma yordamlarını uygulayın, test edin ve belgeleyin. Eksiklikleri belirleyip dağıtımlarınızla Tümleştirdiğiniz yeni Azure işlevlerini uyarlayın

 
## <a name="production-preparation-phase"></a>Üretim hazırlığı aşaması 
Bu aşamada, üretim dışı dağıtımlarınızın tüm deneyimlerini ve dersleri toplamak ve gelecekteki üretim dağıtımlarında uygulamak istiyorsunuz. Daha önce aşamaların yanı sıra, geçerli barındırma konumunuz ile Azure arasında veri aktarımı işini hazırlamanız gerekir. 

1.  Azure 'a geçmeden önce üretim sistemlerinizi gereken SAP sürüm yükseltmeleri aracılığıyla çalışın
2.  Üretim sisteminin geçişinden sonra gerçekleştirilmesi gereken işlevsel ve iş testlerinde iş sahiplerini kabul edin
    1.  Tüm bu testlerin geçerli barındırma konumundaki kaynak sistemlerle birlikte yürütüldüğünden emin olun. Sistem Azure 'a taşındıktan sonra testlerin ilk kez yürütülmesinden kaçınmak istiyorsunuz
2.  Azure 'da üretim geçiş işlemini test edin. Tüm üretim sistemlerini aynı zaman çerçevesinde Azure 'a taşımadığından, aynı barındırma konumunda olması gereken üretim sistemleri gruplarını oluşturun. Alıştırma ve test veri geçişi. Ortak yöntemler listesi şöyle:
    1.  Yedekleme/geri yükleme gibi DBMS yöntemlerini SQL Server AlwaysOn, HANA sistem çoğaltması veya Azure 'daki veritabanı içeriğini tohum ve eşitleme için günlük aktarma gibi kullanma
    2.  Daha küçük veritabanları için yedekleme/geri yükleme kullanın
    3.  Heterojen geçişler gerçekleştirmek için SAP SWPM aracına uygulanan SAP geçiş Izleyicisini kullanın
    4.  SAP yayın yükseltmesi ile birleştirmeniz gerekiyorsa [SAP dmo](https://blogs.sap.com/2013/11/29/database-migration-option-dmo-of-sum-introduction/) işlemini kullanın. Kaynak ve Hedef DBMS arasındaki tüm kombinasyonın desteklenmediğini aklınızda bulundurun. Farklı DMO sürümleri için belirli SAP desteği notlarında daha fazla bilgi bulabilirsiniz. Örneğin, [SUM 2,0 SP04 veritabanı geçiş seçeneği (dmo)](https://launchpad.support.sap.com/#/notes/2644872)
    5.  Yedeklemeleri veya SAP dışa aktarma dosyalarını taşımanız gerekebilmeniz durumunda internet üzerinden veya ExpressRoute üzerinden veri aktarımının daha iyi olup olmadığını test edin. Verileri Internet üzerinden taşırken, gelecekteki üretim sistemlerinde yer almanız gereken NSG/ASG güvenlik kurallarınızın bazılarını değiştirmeniz gerekebilir.
3.  Sistemleri eski platformdan Azure 'a taşımadan önce CPU kullanımı, depolama aktarım hızı ve ıOPS verileri gibi kaynak tüketim verilerini toplayın. Özellikle de DBMS katman birimlerinden, ancak uygulama katmanı birimlerinden. Ayrıca ağ ve depolama gecikmesini ölçer.
4.  Azure için desteklenen VM 'lerde, bu VM 'lerde desteklenen işletim sistemi yayınlarına ve desteklenen SAP ve DBMS yayınlarına hiçbir değişiklik olmadığından emin olmak için SAP destek notları, SAP HANA donanım dizini ve SAP PAM 'deki kaynakları doğrulayın 
4.  Dağıtım betiklerini, VM türleri ve Azure işlevleri üzerinde karar verdiğiniz en son değişikliklere uyarlayabilirsiniz
5.  Altyapı ve uygulama dağıtımı sonrasında, doğrulamak için bir dizi denetim aracılığıyla ilerleyin:
    1.  Doğru sanal makine türleri doğru öznitelikler ve depolama boyutları ile dağıtıldı
    2.  VM 'Lerin doğru ve istenen işletim sistemi yayınları ve düzeltme eklerinin üzerinde olup olmadığını ve tek düzen olduğunu kontrol edin
    3.  VM 'lerin sağlamlaştırılmasını gerekli ve Tekdüzen olarak işaretle
    4.  Doğru uygulama sürümlerinin ve düzeltme eklerinin yüklenip dağıtılmadığını denetleyin
    5.  VM 'Ler Azure kullanılabilirlik kümelerine planlandı olarak dağıtıldı
    6.  Gecikme süresine duyarlı diskler için Azure Premium Depolama kullanıldı veya [% 99,9 tek VM SLA 'sı](https://azure.microsoft.com/support/legal/sla/virtual-machines/v1_8/) gerekli
    7.  Doğru Azure Yazma Hızlandırıcısı dağıtımını denetleyin
        1.  VM, depolama alanları veya dizili kümeler içinde Azure Yazma Hızlandırıcısı desteği gerektiren diskler arasında doğru şekilde oluşturulduğundan emin olun
            1.  [Linux 'Ta yazılım RAID yapılandırma 'yi](https://docs.microsoft.com/azure/virtual-machines/linux/configure-raid) denetle
            2.  [Azure 'da bir LINUX VM 'de LVM 'Yi yapılandırma](https://docs.microsoft.com/azure/virtual-machines/linux/configure-lvm) ' yı denetleme
    8.  [Azure yönetilen diskler](https://azure.microsoft.com/services/managed-disks/) özel olarak kullanıldı
    9.  VM 'Ler doğru kullanılabilirlik kümelerine dağıtıldı ve Kullanılabilirlik Alanları
    10. SAP uygulama katmanında ve SAP DBMS katmanında kullanılan VM 'lerde [Azure hızlandırılmış ağ](https://azure.microsoft.com/blog/maximize-your-vm-s-performance-with-accelerated-networking-now-generally-available-for-both-windows-and-linux/) 'ın etkinleştirildiğinden emin olun
    11. SAP uygulaması ile SAP NetWeaver, Hybrsıs veya S/4HANA tabanlı SAP sistemlerinin DBMS katmanı arasındaki iletişim yolunda [Azure ağ sanal](https://azure.microsoft.com/solutions/network-appliances/) gereçlerinin yerleştirilmesi
    12. ASG ve NSG kuralları istenen ve planlanan şekilde iletişime izin verir ve gerektiğinde iletişimi engeller
    13. Daha önce açıklandığı gibi zaman aşımı ayarları doğru şekilde ayarlanmıştır
    14. [SAP uygulamalarıyla en iyi ağ gecikmesi Için Azure yakınlık yerleşimi grupları](sap-proximity-placement-scenarios.md) makalesinde açıklandığı gibi, sanal makinelerin doğru [Azure yakınlık yerleşimi grubu](https://docs.microsoft.com/azure/virtual-machines/linux/co-location) kullanılarak dağıtıldığından emin olun
    15. Sap desteği notuna [#500235](https://launchpad.support.sap.com/#/notes/500235) ve sap destek notuna [#1100926](https://launchpad.support.sap.com/#/notes/1100926/E), SAP uygulama KATMANı VM 'si ve DBMS VM arasındaki ağ gecikmesini test edin ve değerlendirin. Sonuçları, SAP desteği notunun [#1100926](https://launchpad.support.sap.com/#/notes/1100926/E)ağ gecikme Kılavuzu ' na göre değerlendirin. Ağ gecikmesi orta ve iyi bir aralıkta olmalıdır. [Burada](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-network-architecture#networking-architecture-for-hana-large-instance) belgelendiği gibi, VM 'ler ve Hana büyük örnek birimleri arasındaki trafik için özel durumlar geçerlidir
    15. Şifrelemenin gerektiğinde ve şifreleme yöntemiyle birlikte dağıtılıp dağıtılmadığını denetleyin
    16. Arabirimlerin ve diğer uygulamaların yeni dağıtılan altyapıyı bağlanıp bağlanamamadığını denetleyin
6.  Azure planlı bakım için yeniden işlem yapmaya yönelik bir PlayBook oluşturun. Planlı bakım durumunda yeniden başlatılacak sistem ve VM 'Lerin sırasını tanımlayın
    

## <a name="go-live-phase"></a>Canlı git aşaması
Go-Live aşamasında, daha önceki aşamalarda geliştirmiş olduğunuz PlayBook 'larınızı izlediğinizden emin olmanız gerekir. Test ve eğitilen adımları yürütün. Yapılandırmalarda ve işlemde geçen dakikalık değişiklikleri kabul etmeyin. Bunun yanı sıra, aşağıdaki ölçüleri uygular:

1. Azure portal izlemenin ve diğer izleme araçlarının çalıştığını doğrulayın.  Önerilen araçlar Perfmon (Windows) veya çın ÖIB (Linux): 
    1.  CPU sayaçları 
        1.  Ortalama CPU süresi – toplam (tüm CPU)
        2.  Ortalama CPU süresi – her ayrı işlemci (M128 VM 'de 128 işlemci)
        3.  CPU süresi çekirdeği – her ayrı işlemci
        4.  CPU süresi Kullanıcı – her ayrı işlemci
    5.  Bellek 
        1.  Boş bellek
        2.  Saniye cinsinden bellek sayfası
        3.  Bellek sayfa aşımı/sn
    4.  Disk 
        1.  Disk okuma KB/sn – ayrı disk başına 
        2.  Disk Okuma/sn – ayrı disk başına
        3.  Disk okuma/okuma – ayrı disk başına
        4.  Disk yazma KB/sn – ayrı disk başına 
        5.  Disk yazma/sn – ayrı disk başına
        6.  Disk yazma MS/Read – ayrı disk başına
    5.  Ağ 
        1.  Ağ paketleri/sn
        2.  Giden ağ paketi/sn
        3.  Ağ KB/sn
        4.  Ağ KB/sn 
2.  Verilerin geçişinden sonra, iş sahipleri ile üzerinde anlaştığınız tüm doğrulama testlerini gerçekleştirin. Yalnızca özgün kaynak sistemleri için sonuçları olan doğrulama test sonuçlarını kabul et
3.  Arabirimlerin çalışıp çalışmadığını ve diğer uygulamaların yeni dağıtılan üretim sistemleriyle iletişim kurup kuramayacağını denetleyin
4.  SAP işlem STMS aracılığıyla taşıma ve düzeltme sistemini denetleyin
5.  Sistem üretim için serbest bırakıldıktan sonra veritabanı yedeklemeleri gerçekleştirin
6.  Sistem üretime sunulduktan sonra SAP uygulama katmanı VM 'Leri için VM yedeklemeleri gerçekleştirme
7.  Geçerli Go-Live aşamasının parçası olmayan SAP sistemleri için, ancak bu Go-Live aşamasında Azure 'a taşıdığınız SAP sistemleriyle iletişim kurmak için SM51 içindeki konak adı arabelleğini sıfırlamanız gerekir. Bu adım, Azure 'a taşıdığınız uygulama örneklerinin adlarıyla ilişkili eski önbelleğe alınmış IP adreslerinden kurtul edilecek  


## <a name="post-production"></a>Üretim sonrası
Bu aşamada, sistemi izleme, çalıştırma ve yönetme hakkında bilgi sağlanır. Bir SAP bakış noktasından, eski barındırma konumunuzda gerçekleştirmeniz gereken olağan görevler geçerlidir. Yapmak istediğiniz Azure 'a özgü görevler şunlardır:

1. Yüksek ücretlendirme sistemleri için Azure faturalarını çözümleme
2. VM tarafında ve depolama tarafında fiyat/performans verimliliğini iyileştirin
3. En iyileştirme süresi sistemleri kapatılabilir  


## <a name="next-steps"></a>Sonraki Adımlar
Belgelere başvurun:

- [SAP NetWeaver için Azure sanal makineleri planlama ve uygulama](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/planning-guide)
- [SAP NetWeaver için Azure sanal makineler dağıtımı](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/deployment-guide)
- [SAP iş yükü için Azure sanal makineler DBMS dağıtımına yönelik konular](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/dbms_guide_general)

