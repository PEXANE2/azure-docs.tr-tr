---
title: SAP iş yükü planlama ve dağıtım denetim listesi | Microsoft Dokümanlar
description: SAP iş yükü dağıtımlarını Azure'a planlamak ve iş yüklerini dağıtmak için denetim listesi
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
ms.date: 02/13/2020
ms.author: juergent
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 7b8c1b0bcc74d73f1f869972488ba7c5dfe610d8
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80060059"
---
# <a name="sap-workloads-on-azure-planning-and-deployment-checklist"></a>Azure'da SAP iş yükleri: planlama ve dağıtım denetim listesi

Bu denetim listesi, SAP NetWeaver, S/4HANA ve Hybris uygulamalarını hizmet olarak Azure altyapısına taşıması için tasarlanmıştır. Proje süresince, bir müşteri ve/veya SAP iş ortağı kontrol listesini gözden geçirmelidir. Denetimlerin çoğunun projenin başında ve planlama aşamasında tamamlandığını unutmayın. Dağıtım yapıldıktan sonra, dağıtılan Azure altyapısıveya SAP yazılım sürümlerindeki basit değişiklikler karmaşık hale gelebilir.

Projeniz sırasında önemli kilometre taşlarında denetim listesini gözden geçirin. Bunu yapmak, küçük sorunları büyük sorunlara dönüşmeden önce algılamanızı sağlar. Ayrıca, gerekli değişiklikleri yeniden tasarlamak ve test etmek için yeterli zamanınız olur. Bu denetim listesinin tamamolduğunu düşünmeyin. Durumunuza bağlı olarak, çok daha fazla denetim gerçekleştirmeniz gerekebilir.

Denetim listesi, Azure'dan bağımsız görevleri içermez. Örneğin, SAP uygulama arabirimleri Azure platformuna veya bir barındırma sağlayıcısına geçiş sırasında değişir.

Bu denetim listesi, zaten dağıtılan sistemler için de kullanılabilir. Yazma Hızlandırıcı ve Kullanılabilirlik Bölgeleri gibi yeni özellikler ve dağıttığınızdan beri yeni VM türleri eklenmiş olabilir. Bu nedenle, Azure platformundaki yeni özelliklerden haberdar olduğunuzdan emin olmak için denetim listesini düzenli aralıklarla gözden geçirmeniz yararlı olur.

## <a name="project-preparation-and-planning-phase"></a>Proje hazırlama ve planlama aşaması
Bu aşamada, SAP iş yükünüzün Azure platformuna geçişini planlarsınız. En azından, bu aşamada aşağıdaki belgeleri oluşturmanız ve geçişin aşağıdaki öğelerini tanımlamanız ve tartışmanız gerekir:

1. Üst düzey tasarım belgesi. Bu belge şunları içermelidir:
    - SAP bileşenlerinin ve uygulamalarının geçerli envanteri ve Azure için hedef uygulama envanteri.
    - İlgili tarafların sorumluluklarını ve görevlerini tanımlayan bir sorumluluk atama matrisi (RACI). Yüksek bir düzeyde başlayın ve planlama ve ilk dağıtımlar boyunca daha ayrıntılı düzeylerde çalışın.
    - Üst düzey bir çözüm mimarisi.
    - Hangi Azure bölgelerine dağıtılanın. Azure [bölgeleri listesine](https://azure.microsoft.com/global-infrastructure/regions/)bakın. Her bölgede hangi hizmetlerin kullanılabildiği hakkında bilgi edinmek için [bölgeye göre sunulan Ürünler'e](https://azure.microsoft.com/global-infrastructure/services/)bakın.
    - Şirket içinde Azure'a bağlanmak için ağ mimarisi. [Azure için Sanal Veri Merkezi planını](https://docs.microsoft.com/azure/architecture/vdc/)tanımaya başlayın.
    - Azure'da yüksek etkili iş verilerini çalıştırmak için güvenlik ilkeleri. Veri güvenliği hakkında bilgi edinmek için [Azure güvenlik belgeleriyle](https://docs.microsoft.com/azure/security/)başlayın.
2.  Teknik tasarım belgesi. Bu belge şunları içermelidir:
    - Çözüm için bir blok diyagramı.
    - Azure'da işlem, depolama ve ağ bileşenlerinin boyutlandırılması. Azure VM'lerin SAP boyutlandırması [için SAP destek notunun #1928533](https://launchpad.support.sap.com/#/notes/1928533)bakın.
    - İş sürekliliği ve felaket kurtarma mimarisi.
    - OS, DB, çekirdek ve SAP destek paketi sürümleri hakkında ayrıntılı bilgi. SAP NetWeaver veya S/4HANA tarafından desteklenen her işletim sistemi sürümüazure VM'lerde desteklenir olması şart değildir. Aynı DBMS sürümleri için de geçerlidir. SAP ve Azure desteğini sağlamak için SAP sürümlerini, DBMS sürümlerini ve işletim sistemi sürümlerini hizalamak için aşağıdaki kaynakları denetleyin ve gerekirse yükseltme SAP sürümlerini ve işletim sistemi sürümlerini denetleyin. SAP ve Microsoft'tan tam destek almak için SÜRÜM kombinasyonlarının SAP ve Azure tarafından desteklenmesi gerekir. Gerekirse, bazı yazılım bileşenlerini yükseltmeyi planlamanız gerekir. Desteklenen SAP, OS ve DBMS yazılımları hakkında daha fazla ayrıntı burada belgelenmiştir:
        - [SAP destek notu #1928533.](https://launchpad.support.sap.com/#/notes/1928533) Bu not, Azure VM'lerinde desteklenen en az işletim sistemi sürümlerini tanımlar. Ayrıca, HANA olmayan veritabanlarının çoğu için gereken minimum veritabanı sürümlerini tanımlar. Son olarak, SAP destekli Azure VM türleri için SAP boyutlandırmasını sağlar.
        - [SAP destek notu #2015553.](https://launchpad.support.sap.com/#/notes/2015553) Bu not, Azure depolama alanı yla ilgili destek ilkelerini ve Microsoft ile gereken destek ilişkisini tanımlar.
        - [SAP destek notu #2039619.](https://launchpad.support.sap.com/#/notes/2039619) Bu not, Azure için Oracle destek matrisini tanımlar. Oracle, SAP iş yükleri için Azure'da konuk işletim sistemleri olarak yalnızca Windows ve Oracle Linux'u destekler. Bu destek deyimi, SAP örneklerini çalıştıran SAP uygulama katmanı için de geçerlidir. Ancak Oracle, Pacemaker aracılığıyla Oracle Linux'taki SAP Central Services için yüksek kullanılabilirliği desteklemez. Oracle Linux'ta ASCS için yüksek kullanılabilirliğe ihtiyacınız varsa, Linux için SIOS Protection Suite'i kullanmanız gerekir. Ayrıntılı SAP belgelendirme verileri için, #1662610 SAP destek notu [- Linux için SIOS Protection Suite için destek ayrıntılarına](https://launchpad.support.sap.com/#/notes/1662610)bakın. Windows için SAP Central Services için SAP destekli Windows Server Failover Kümeleme çözümü, DBMS katmanı olarak Oracle ile birlikte desteklenir.
        - [SAP destek notu #2235581.](https://launchpad.support.sap.com/#/notes/2235581) Bu not, farklı işletim sistemi sürümlerinde SAP HANA için destek matrisi sağlar.
        - SAP HANA destekli Azure VM'ler ve [HANA Büyük Örnekleri](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-overview-architecture) SAP web [sitesinde](https://www.sap.com/dmc/exp/2014-09-02-hana-hardware/enEN/iaas.html#categories=Microsoft%20Azure)listelenmiştir.
        - [SAP Ürün Kullanılabilirlik Matrisi](https://support.sap.com/en/).
        - [SAP destek notu #2555629 - SAP HANA 2.0 Dinamik Katmanlama - Hipervizör ve Bulut Desteği](https://launchpad.support.sap.com/#/notes/2555629)
        - [SAP destek notu #1662610 - Linux için SIOS Protection Suite için destek ayrıntıları](https://launchpad.support.sap.com/#/notes/1662610)
        - SAP'ye özgü diğer ürünler için SAP notları.     
    - SAP üretim sistemleri için sıkı üç katmanlı tasarımlar öneririz. ASCS ve/veya DBMS ve/veya uygulama sunucularının tek bir VM'de birleştirilmesinizi önermiyoruz. SAP Central Services için çoklu SID küme yapılandırmalarının kullanılması, Azure'daki Windows konuk işletim sistemlerinde desteklenir. Ancak bu yapılandırma, Azure'daki Linux işletim sistemlerinde SAP Central Services için desteklenmez. Windows konuk işletim sistemi senaryosu için belgeleri şu makalelerde bulabilirsiniz:
        - [Windows Server Failover Clustering ve Azure'da paylaşılan disk ile SAP ASCS/SCS örneği çoklu SID yüksek kullanılabilirlik](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-ascs-ha-multi-sid-wsfc-shared-disk)
        - [Windows Server Failover Clustering ve Azure'da dosya paylaşımı ile SAP ASCS/SCS örneği çoklu SID yüksek kullanılabilirlik](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-ascs-ha-multi-sid-wsfc-file-share)
    - Yüksek kullanılabilirlik ve olağanüstü durum kurtarma mimarisi.
        - RTO ve RPO'ya dayalı olarak, yüksek kullanılabilirlik ve olağanüstü durum kurtarma mimarisinin nasıl görünmesi gerektiğini tanımlayın.
        - Bir bölge içinde yüksek kullanılabilirlik için, istediğiniz DBMS'nin Azure'da neler sunduğunu kontrol edin. Çoğu DBMS paketi, üretim sistemleri için önerdiğimiz senkron sıcak bekleme yöntemleri sunar. Sap iş yükleri ve ilgili belgeler [için Azure Sanal Makineler DBMS dağıtımıiçin Dikkate Alınmasıyla](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/dbms_guide_general) başlayan farklı veritabanları için SAP ile ilgili belgeleri de denetleyin.
           Örneğin [SQL Server için açıklanan](https://docs.microsoft.com/sql/sql-server/failover-clusters/windows/always-on-failover-cluster-instances-sql-server?view=sql-server-2017)DBMS katmanı için paylaşılan bir disk yapılandırması ile Windows Server Failover Kümeleme'nin kullanılması desteklenmez. Bunun yerine, şu gibi çözümler kullanın:
           - [SQL Server Always On](https://docs.microsoft.com/azure/virtual-machines/windows/sqlclassic/virtual-machines-windows-classic-ps-sql-alwayson-availability-groups)
           - [Oracle Data Guard](https://docs.microsoft.com/azure/virtual-machines/workloads/oracle/configure-oracle-dataguard)
           - [HANA Sistem Çoğaltma](https://help.sap.com/viewer/6b94445c94ae495c83a19646e7c3fd56/2.0.01/en-US/b74e16a9e09541749a745f41246a065e.html)
        - Azure bölgelerinde olağanüstü durum kurtarma için, farklı DBMS satıcıları tarafından sunulan çözümleri gözden geçirin. Çoğu eşzamanlı çoğaltma veya günlük nakliye destekler.
        - SAP uygulama katmanı için, üretim dağıtımlarınızın kopyaları olan iş regresyon test sistemlerini aynı Azure bölgesinde mi yoksa DR bölgenizde mi çalıştıracağınız konusunda belirleyin. İkinci durumda, bu iş regresyon sistemini üretim dağıtımlarınız için DR hedefi olarak hedefleyebilirsiniz.
        - Üretim dışı sistemleri DR sitesine yerleştirmemeye karar verirseniz, SAP uygulama katmanını Azure DR bölgesine çoğaltmayöntemi olarak Azure Site Kurtarma'yı inceleyin. Daha fazla bilgi için, [çok katmanlı SAP NetWeaver uygulaması dağıtımı için olağanüstü durum kurtarma ayarlama'ya](https://docs.microsoft.com/azure/site-recovery/site-recovery-sap)bakın.
        - [Azure Kullanılabilirlik Bölgeleri'ni](https://docs.microsoft.com/azure/availability-zones/az-overview)kullanarak birleştirilmiş HADR yapılandırması kullanmaya karar verirseniz, Kullanılabilirlik Bölgeleri'nin kullanılabildiği Azure bölgelerini edin. Ayrıca, iki Kullanılabilirlik Bölgesi arasındaki artan ağ gecikmeleri tarafından getirilebilecek kısıtlamaları da dikkate alın.  
3.  Tüm SAP arabirimlerinin (SAP ve SAP dışı) envanteri.
4.  Vakıf hizmetlerinin tasarımı. Bu tasarım aşağıdaki öğeleri içermelidir:
    - Aktif Dizin ve DNS tasarımı.
    - Azure içindeki ağ topolojisi ve farklı SAP sistemlerinin atanması.
    - [Azure'da](https://docs.microsoft.com/azure/role-based-access-control/overview) altyapı ve SAP uygulamalarını yöneten ekipler için rol tabanlı erişim yapısı.
    - Kaynak grubu topolojisi.
    - [Etiketleme stratejisi](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags#tags-and-billing).
    - VM'ler ve diğer altyapı bileşenleri ve/veya mantıksal adlar için adlandırma kuralları.
5.  Microsoft Premier Destek sözleşmesi. Microsoft Teknik Hesap Yöneticinizi (TAM) tanımlayın. SAP destek gereksinimleri için SAP [destek notu #2015553](https://launchpad.support.sap.com/#/notes/2015553)bakın.
6.  Azure aboneliklerinin sayısı ve abonelikler için temel kota. Gerektiğinde [Azure abonelikkotalarını artırmak için destek isteklerini açın.](https://docs.microsoft.com/azure/azure-portal/supportability/resource-manager-core-quotas-request)
7.  SAP verilerini Azure'a geçirmek için veri azaltma ve veri geçiş planı. SAP NetWeaver sistemleri için SAP'nin, büyük miktarda ki verilerin hacmini nasıl sınırlandırılabilmek için yönergeleri vardır. SAP ERP sistemlerinde veri yönetimi yle ilgili [sap kılavuzuna](https://wiki.scn.sap.com/wiki/download/attachments/247399467/DVM_%20Guide_7.2.pdf?version=1&modificationDate=1549365516000&api=v2) bakın. İçeriğin bir kısmı genel olarak NetWeaver ve S/4HANA sistemleri için de geçerlidir.
8.  Otomatik dağıtım yaklaşımı. Azure'daki altyapı dağıtımlarının otomasyonunun amacı, deterministik bir şekilde dağıtmak ve deterministik sonuçlar elde etmektir. Birçok müşteri PowerShell veya CLI tabanlı komut dosyaları kullanır. Ancak, SAP için Azure altyapısını dağıtmak ve hatta SAP yazılımLarını yüklemek için kullanabileceğiniz çeşitli açık kaynak teknolojileri vardır. GitHub'da örnekler bulabilirsiniz:
    - [Azure Bulutunda Otomatik SAP Dağıtımları](https://github.com/Azure/sap-hana)
    - [SAP HANA Kurulumu](https://github.com/AzureCAT-GSI/SAP-HANA-ARM)
9.  Müşteri, sistem entegratörü, Microsoft ve diğer ilgili taraflar olarak aranıza düzenli bir tasarım ve dağıtım gözden geçirme cadence tanımlayın.

 
## <a name="pilot-phase-strongly-recommended"></a>Pilot faz (şiddetle tavsiye edilir)
 
Proje planlama ve hazırlık sırasında bir pilot çalıştırabilirsiniz. Ayrıca planlama ve hazırlık aşamasında yapılan yaklaşımları ve tasarımları test etmek için pilot aşamayı kullanabilirsiniz. Ve pilot aşamayı genişleterek bunu konseptin gerçek bir kanıtı haline getirebilirsiniz.

Pilot dağıtım sırasında tam bir HADR çözümü ve güvenlik tasarımı ayarlamanızı ve doğrulamanızı öneririz. Bazı müşteriler bu aşamada ölçeklenebilirlik testleri gerçekleştirir. Diğer müşteriler SAP kum havuzu sistemlerinin dağıtımlarını pilot aşama olarak kullanır. Pilot için Azure'a geçirmek istediğiniz bir sistemi zaten belirlediğinizi varsayıyoruz.

1. Azure'a veri aktarımına en iyi şekilde aktarın. En uygun seçim, belirli senaryoya son derece bağlıdır. ExpressRoute devresi yeterli bant genişliğine sahipse, şirket içi Azure [ExpressRoute](https://azure.microsoft.com/services/expressroute/) üzerinden aktarma en hızlıyoldur. Diğer senaryolarda, internet üzerinden aktarma daha hızlıdır.
2. Veri dışa aktarma ve alma içeren heterojen bir SAP platformu geçişi için, dışa aktarma ve alma aşamalarını test edin ve optimize edin. SQL Server'ın hedef platform olduğu büyük geçişler için [öneriler](https://techcommunity.microsoft.com/t5/Running-SAP-Applications-on-the/SAP-OS-DB-Migration-to-SQL-Server-8211-FAQ-v6-2-April-2017/ba-p/368070)bulabilirsiniz. Birleşik sürüm yükseltmesi gerekmezseniz Geçiş İzleyicisi/SWPM kullanabilirsiniz. Geçişişlemini SAP sürüm yükseltmesiyle birleştirdiğinizde [SAP DMO](https://blogs.sap.com/2013/11/29/database-migration-option-dmo-of-sum-introduction/) işlemini kullanabilirsiniz. Bunu yapmak için, kaynak ve hedef DBMS platform kombinasyonu için belirli gereksinimleri karşılamanız gerekir. Bu [işlem, SUM 2.0 SP03 Veritabanı Geçiş Seçeneği (DMO)](https://launchpad.support.sap.com/#/notes/2631152)belgelenmiştir.
   1.  Kaynağa dışa aktarın, dosya yüklemesini Azure'a dışa aktarın ve performansı aktarın. Dışa aktarma ve alma arasındaki çakışma maksimize.
   2.  Altyapı boyutlandırma amacıyla hedef ve hedef platformlarda veritabanının hacmini değerlendirin.
   3.  Zamanlamayı doğrulayın ve optimize edin.
1. Teknik doğrulama.
   1. VM türleri.
        - SAP destek notlarında, SAP HANA donanım dizininde ve SAP PAM'deki kaynakları yeniden gözden geçirin. Azure için desteklenen VM'lerde, bu VM türleri için desteklenen işletim sistemi sürümlerinde ve desteklenen SAP ve DBMS sürümlerinde değişiklik olmadığından emin olun.
        - Uygulamanızın boyutlandırmasını ve Azure'da dağıttığınız altyapıyı yeniden doğrulayın. Varolan uygulamaları taşıyorsanız, genellikle kullandığınız altyapıdan ve [SAP kıyaslama web sayfasından](https://www.sap.com/dmc/exp/2018-benchmark-directory/#/sd) gerekli SAPS'yi elde edebilir ve [sap destek notu #1928533](https://launchpad.support.sap.com/#/notes/1928533)listelenen SAPS numaralarıyla karşılaştırabilirsiniz. Ayrıca [saps derecelendirme bu makaleyi](https://techcommunity.microsoft.com/t5/Running-SAP-Applications-on-the/SAPS-ratings-on-Azure-VMs-8211-where-to-look-and-where-you-can/ba-p/368208) akılda tutun.
        - Planlama aşamasında seçtiğiniz VM türlerinin maksimum depolama verimi ve ağ verimi açısından Azure VM'lerinizin boyutlandırmasını değerlendirin ve test edin. Verileri burada bulabilirsiniz:
           -  [Azure'daki Windows sanal makineleri için boyutlar.](https://docs.microsoft.com/azure/virtual-machines/windows/sizes?toc=%2fazure%2fvirtual-network%2ftoc.json) Boyutlandırma için maksimum *önbelleğe edilmemiş disk masını* göz önünde bulundurmak önemlidir.
           -  [Azure'daki Linux sanal makineleri için boyutlar.](https://docs.microsoft.com/azure/virtual-machines/linux/sizes?toc=%2fazure%2fvirtual-network%2ftoc.json) Boyutlandırma için maksimum *önbelleğe edilmemiş disk masını* göz önünde bulundurmak önemlidir.
   2. Depolama’yı seçin.
        - En azından, SAP uygulama katmanlarını temsil eden VM'ler ve performansa duyarlı olmayan DBMS'lerin dağıtımı için [Azure Standart SSD depolama alanı](https://docs.microsoft.com/azure/virtual-machines/windows/disks-types#standard-ssd) kullanın.
        - Genel olarak, [Azure Standart HDD disklerinin](https://docs.microsoft.com/azure/virtual-machines/windows/disks-types#standard-hdd)kullanılmasını önermiyoruz.
        - Performansa duyarlı tüm DBMS VM'ler için [Azure Premium Depolama'yı](https://docs.microsoft.com/azure/virtual-machines/windows/disks-types#premium-ssd) kullanın.
        - [Azure yönetilen diskleri](https://azure.microsoft.com/services/managed-disks/)kullanın.
        - M Serisi ile DBMS günlük sürücüleri için Azure Yazma Hızlandırıcısı'nı kullanın. Yazma Hızlandırıcı'da belirtildiği gibi Yazma Hızlandırıcı limitlerine ve kullanımına dikkat [edin.](https://docs.microsoft.com/azure/virtual-machines/linux/how-to-enable-write-accelerator)
        - Farklı DBMS türleri için, [genel SAP ile ilgili DBMS belgelerini](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/dbms_guide_general) ve genel belgenin işaret ettiği DBMS'ye özgü belgeleri denetleyin.
        - SAP HANA hakkında daha fazla bilgi için [Azure'daki SAP HANA altyapı yapılandırmaları ve işlemlerine](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-vm-operations)bakın.
        - Aygıt kimliğini kullanarak Azure veri disklerini asla Azure Linux VM'ye monte etme. Bunun yerine, evrensel olarak benzersiz tanımlayıcıyı (UUID) kullanın. Örneğin, Azure veri disklerini monte etmek için grafik araçlarını kullanırken dikkatli olun. UUID'nin diskleri monte etmek için kullanıldığından emin olmak için /etc/fstab'daki girişleri iki kez kontrol edin. [Bu makalede](https://docs.microsoft.com/azure/virtual-machines/linux/attach-disk-portal#connect-to-the-linux-vm-to-mount-the-new-disk)daha fazla ayrıntı bulabilirsiniz.
   3. Ağ.
        - Sanal ağ altyapınızı ve SAP uygulamalarınızın farklı Azure sanal ağları arasında veya içinde dağıtımını test edin ve değerlendirin.
        -  Hub ve kollu sanal ağ mimarisi yaklaşımını veya tek bir Azure sanal ağındaki mikro segmentasyon yaklaşımını değerlendirin. Bu değerlendirmeyi temel alan:
               1. [Bakan Azure sanal ağları](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview)arasındaki veri alışverişinin maliyetleri. Maliyetler hakkında daha fazla bilgi için [Sanal Ağ fiyatlandırması](https://azure.microsoft.com/pricing/details/virtual-network/)için bkz.
               2. Bir alt ağı sanal ağ içinde yalıtmak için ağ güvenlik grubunu değiştirmek yerine Azure sanal ağları arasındaki bakışbağlantısının hızlı bir şekilde kesilmesinin avantajları. Bu değerlendirme, sanal ağın bir alt ağında barındırılan uygulamaların veya VM'lerin güvenlik riski haline geldiğindeki durumlar içindir.
                3. Şirket içi, dış dünya ve Azure'da inşa ettiğiniz sanal veri merkezi arasındaki ağ trafiğinin merkezi günlüğe kaydetme ve denetleme.
        - SAP uygulama katmanı ile SAP DBMS katmanı arasındaki veri yolunu değerlendirin ve test edin.
            -  Azure [ağı sanal cihazlarının](https://azure.microsoft.com/solutions/network-appliances/) SAP uygulaması ile SAP NetWeaver, Hybris veya S/4HANA'ya dayalı SAP sistemlerinin DBMS katmanı arasındaki iletişim yoluna yerleştirilmesi desteklenmez.
            -  SAP uygulama katmanının ve SAP DBMS'nin, bakmayan farklı Azure sanal ağlarına yerleştirilmesi desteklenmez.
            -  SAP uygulama katmanı ile SAP DBMS katmanı arasındaki yolları tanımlamak için [uygulama güvenlik grubu ve ağ güvenliği grubu kurallarını](https://docs.microsoft.com/azure/virtual-network/security-overview) kullanabilirsiniz.
        - SAP uygulama katmanında ve SAP DBMS katmanında kullanılan VM'lerde [Azure Hızlandırılmış Ağ](https://azure.microsoft.com/blog/maximize-your-vm-s-performance-with-accelerated-networking-now-generally-available-for-both-windows-and-linux/) özelliğinin etkin olduğundan emin olun. Azure'da Hızlandırılmış Ağ'ı desteklemek için farklı işletim sistemi düzeylerine ihtiyaç duyulduğunu unutmayın:
            - Windows Server 2012 R2 veya üzeri.
            - SUSE Linux 12 SP3 veya sonrası.
            - RHEL 7.4 veya sonrası.
            - Oracle Linux 7.5. RHCKL çekirdeğini kullanıyorsanız, 3.10.0-862.13.1.el7 sürümü gereklidir. Oracle UEK çekirdeğini kullanıyorsanız, sürüm 5 gereklidir.
        - SAP destek notlarına göre SAP uygulama katmanı VM'ler ve DBMS VM'leri arasındaki ağ gecikmesini [#500235 test edin](https://launchpad.support.sap.com/#/notes/500235) ve [#1100926.](https://launchpad.support.sap.com/#/notes/1100926/E) [SAP destek notu #1100926'ndeki](https://launchpad.support.sap.com/#/notes/1100926/E)ağ gecikme kılavuzuna göre sonuçları değerlendirin. Ağ gecikmesi orta veya iyi aralıkta olmalıdır. Bu [makalede](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-network-architecture#networking-architecture-for-hana-large-instance)belirtildiği gibi, VM'ler ve HANA Büyük Örnek birimleri arasındaki trafik için özel durumlar geçerlidir.
        - ILB dağıtımlarının Doğrudan Sunucu İadesi'ni kullanacak şekilde ayarlandıklarından emin olun. Bu ayar, Azure ILB'leri DBMS katmanındaki yüksek kullanılabilirlik yapılandırmaları için kullanıldığında gecikme süresini azaltır.
        - Azure Yük Dengeleyicisini Linux konuk işletim sistemleriyle birlikte kullanıyorsanız, Linux ağ parametresi **net.ipv4.tcp_timestamps'nin** **0**olarak ayarlı olup olmadığını kontrol edin. Bu [öneri, SAP not#2382421](https://launchpad.support.sap.com/#/notes/2382421)eski sürümlerindeki önerilerle çakışma. SAP notu artık azure yük dengeleyicileriyle çalışmak için bu parametrenin **0** olarak ayarlanması gerektiğini belirtmek için güncelleştirildi.
        - En iyi ağ gecikmesini elde etmek için [Azure yakınlık yerleşim gruplarını](https://docs.microsoft.com/azure/virtual-machines/linux/co-location) kullanmayı düşünün. Daha fazla bilgi [için, SAP uygulamalarında en iyi ağ gecikmesi için Azure yakınlık yerleşim gruplarına](sap-proximity-placement-scenarios.md)bakın.
   4. Yüksek kullanılabilirlik ve olağanüstü durum kurtarma dağıtımları.
        - SAP uygulama katmanını belirli bir Azure Kullanılabilirlik Bölgesi tanımlamadan dağıtıysanız, SAP iletişim örneklerini veya tek bir SAP sisteminin ara yazılım örneklerini çalıştıran tüm VM'lerin [kullanılabilirlik kümesinde](https://docs.microsoft.com/azure/virtual-machines/windows/manage-availability)dağıtıldığınızdan emin olun.
        - SAP Merkezi Hizmetleri ve DBMS için yüksek kullanılabilirlik gerekmezseniz, bu VM'leri SAP uygulama katmanıyla aynı kullanılabilirlik kümesine dağıtabilirsiniz.
        - Pasif çoğaltma kullanarak SAP Merkezi Hizmetlerini ve DBMS katmanını yüksek kullanılabilirlik için korursanız, SAP Central Services için iki düğümayrı bir kullanılabilirlik kümesine ve iki DBMS düğümlerini başka bir kullanılabilirlik kümesine yerleştirin.
        - Azure Kullanılabilirlik Bölgeleri'ne dağıtım yaptığınızda, kullanılabilirlik kümelerini kullanamazsınız. Ancak, etkin ve pasif Merkezi Hizmetler düğümlerini iki farklı Kullanılabilirlik Bölgesine dağıttığınızdan emin olmanız gerekir. Aralarında en düşük gecikme gecikmesi olan Kullanılabilirlik Bölgeleri'ni kullanın.
          Kullanılabilirlik Bölgeleri arasında DBMS ve SAP Merkezi Hizmetler katmanı için Windows veya Pacemaker başarısız kümeleri oluşturma durumunda [Azure Standart Yük Dengeleyicisi](https://docs.microsoft.com/azure/load-balancer/load-balancer-standard-availability-zones) kullanmanız gerektiğini unutmayın. Zonal dağıtımlar için [Temel Yük Dengeleyici'ni](https://docs.microsoft.com/azure/load-balancer/load-balancer-overview) kullanamazsınız.
   5. Zaman sonu ayarları.
        - Enqueue sunucusu ile SAP iş süreçleri arasında bağlantı sonu olmadığından emin olmak için SAP örneklerinin SAP NetWeaver geliştirici izlerini denetleyin. Bu iki kayıt defteri parametrelerini ayarlayarak bu bağlantı sonları önleyebilirsiniz:
            - HKLM\System\CurrentControlSet\Services\Tcpip\Parameters\KeepAliveTime = 120000. Daha fazla bilgi için [KeepAliveTime'a](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-2000-server/cc957549(v=technet.10))bakın.
            - HKLM\System\CurrentControlSet\Services\Tcpip\Parameters\KeepAliveInterval = 120000. Daha fazla bilgi için [KeepAliveInterval'a](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-2000-server/cc957548(v=technet.10))bakın.
        - Şirket içi SAP GUI arabirimleri ve Azure'da dağıtılan SAP uygulama katmanları arasındaki GUI zaman zaman larını önlemek için, bu parametrelerin default.pfl veya örnek profilinde ayarlanıp ayarlanmadığını denetleyin:
            - rdisp/keepalive_timeout = 3600
            - rdisp/keepalive = 20
        - SAP enqueue işlemi ile SAP iş süreçleri arasındaki kurulan bağlantıların kesintiye uğramasını önlemek için **enque/encni/set_so_keepalive** parametresini **doğru**ayarlamanız gerekir. Ayrıca bakınız [SAP notu #2743751.](https://launchpad.support.sap.com/#/notes/2743751)  
        - Windows failover kümesi yapılandırması kullanıyorsanız, yanıt vermeyen düğümlerde tepki verme zamanının Azure için doğru ayarlandığınızdan emin olun. Makaleyi [Tuning Failover Cluster Ağ Eşikleri](https://techcommunity.microsoft.com/t5/Failover-Clustering/Tuning-Failover-Cluster-Network-Thresholds/ba-p/371834) parametreleri ve bunların failover hassasiyetlerini nasıl etkilediğini listeler. Küme düğümlerinin aynı alt ağda olduğunu varsayarsak, şu parametreleri değiştirmelisiniz:
            - SameSubNetDelay = 2000
            - SameSubnetThreshold = 15
            - YönlendirmeTarih boyu = 30
    6. İşletim Sistemi Ayarları veya Yamaları
        - SAP'de HANA çalıştırmak için şu notları ve belgeleri okuyun:
            -   [SAP destek notu #2814271 - SAP HANA Yedekleme, Azure'da Checksum Hatası ile başarısız oldu](https://launchpad.support.sap.com/#/notes/2814271)
            -   [SAP destek notu #2753418 - Zamanlayıcı Geri Dönüş Nedeniyle Potansiyel Performans Düşüşü](https://launchpad.support.sap.com/#/notes/2753418)
            -   [SAP destek notu #2791572 - Azure'da Hyper-V için Eksik VDSO Desteği Nedeniyle Performans Bozulması](https://launchpad.support.sap.com/#/notes/2791572)
            -   [SAP destek notu #2382421 - HANA ve OS Düzeyinde Ağ Yapılandırmasını Optimize Etme](https://launchpad.support.sap.com/#/notes/2382421)
            -   [SAP destek notu #2694118 - Red Hat Enterprise Linux HA Eklentisi Azure'da](https://launchpad.support.sap.com/#/notes/2694118)
            -   [SAP destek notu #1984787 - SUSE LINUX Enterprise Server 12: Kurulum notları](https://launchpad.support.sap.com/#/notes/1984787)
            -   [SAP destek notu #2002167 - Red Hat Enterprise Linux 7.x: Kurulum ve Yükseltme](https://launchpad.support.sap.com/#/notes/0002002167)
            -   [SAP destek notu #2292690 - SAP HANA DB: RHEL 7 için önerilen işletim sistemi ayarları](https://launchpad.support.sap.com/#/notes/0002292690)
            -   [SAP destek notu #2772999 - Red Hat Enterprise Linux 8.x: Kurulum ve Yapılandırma](https://launchpad.support.sap.com/#/notes/2772999)
            -   [SAP destek notu #2777782 - SAP HANA DB: RHEL 8 için önerilen Işletim Sistemi Ayarları](https://launchpad.support.sap.com/#/notes/2777782)
            -   [SAP destek notu #2578899 - SUSE Linux Enterprise Server 15: Kurulum Notu](https://launchpad.support.sap.com/#/notes/2578899)
            -   [SAP destek notu #2455582 - Linux: GCC 6.x ile derlenen SAP uygulamalarını çalıştırmak](https://launchpad.support.sap.com/#/notes/0002455582)
            -    [SAP destek notu #2729475 - SAP HANA için sertifikalı Azure VM'lerde HWCCT Hatası "Hypervisor desteklenmiyor"](https://launchpad.support.sap.com/#/notes/2729475)
1. Yüksek kullanılabilirliğinizi ve olağanüstü durum kurtarma prosedürlerinizi test edin.
   1. VM'leri (Windows konuk işletim sistemleri) kapatarak veya işletim sistemlerini panik moduna sokarak (Linux konuk işletim sistemleri) başarısız durumları simüle edin. Bu adım, başarısız yapılandırmalarınızın tasarladığı gibi çalışıp çalışmadığını anlamanıza yardımcı olur.
   1. Bir başarısızın ne kadar süreceğini ölçün. Süreler çok uzunsa, aşağıdakileri göz önünde bulundurun:
        - SUSE Linux için, başarısızlığı hızlandırmak için Azure Fence aracısı yerine SBD aygıtları kullanın.
        - SAP HANA için, verilerin yeniden yüklenmesi çok uzun sürüyorsa, daha fazla depolama bant genişliği sağlamayı düşünün.
   3. Yedekleme/geri yükleme dizinizi ve zamanlamanızı test edin ve gerekirse düzeltmeler yapın. Yedekleme sürelerinin yeterli olduğundan emin olun. Ayrıca geri yükleme ve zaman geri yükleme etkinliklerini test etmek gerekir. RTO'nuzun bir veritabanına veya VM geri yükleme işlemine dayandığı her yerde geri yükleme sürelerinin RTO SLA'larınızın içinde olduğundan emin olun.
   4. Bölgeler arası DR işlevselliğini ve mimarisini test edin.
1. Güvenlik kontrolleri.
   1. Azure rol tabanlı erişim denetimi (RBAC) mimarinizin geçerliliğini test edin. Amaç, farklı takımların erişimini ve izinlerini ayırmak ve sınırlamaktır. Örneğin, SAP Basis ekip üyeleri Sanal M'leri dağıtabilmeli ve Azure Depolama'dan belirli bir Azure sanal ağına disk atayabilir. Ancak SAP Basis ekibi kendi sanal ağlarını oluşturamamalı veya varolan sanal ağların ayarlarını değiştirememelidir. Ağ ekibinin üyeleri, SAP uygulaması ve DBMS VM'lerinin çalıştırıldığı sanal ağlara VM dağıtamamalıdır. Bu ekibin üyeleri VM'lerin özniteliklerini değiştiremez, hatta VM'leri veya diskleri silemez.  
   1.  Ağ [güvenlik grubunun ve ASC](https://docs.microsoft.com/azure/virtual-network/security-overview) kurallarının beklendiği gibi çalıştığını doğrulayın ve korunan kaynakları koruyun.
   1.  Şifrelenmiş olması gereken tüm kaynakların şifrelenmiş olduğundan emin olun. Sertifikaları yedeklemek, bu sertifikaları depolamak ve bunlara erişmek ve şifrelenmiş varlıkları geri yüklemek için işlemleri tanımlayın ve uygulayın.
   1.  İşletim sistemi desteği açısından mümkün olduğunda işletim sistemi diskleri için [Azure Disk Şifreleme'yi](https://docs.microsoft.com/azure/security/azure-security-disk-encryption-faq) kullanın.
   1.  Çok fazla şifreleme katmanı kullanmadığınızdan emin olun. Bazı durumlarda, aynı sunucudaki farklı diskleri veya bileşenleri korumak için Azure Disk Şifrelemesi'ni DBMS Saydam Veri Şifreleme yöntemlerinden biriyle birlikte kullanmak mantıklıdır.  Örneğin, bir SAP DBMS sunucusunda, Azure Disk Şifrelemesi (ADE) işletim sistemi önyükleme diskinde (işletim sistemi ADE'yi destekliyorsa) ve DBMS veri kalıcılığı dosyaları tarafından kullanılmayan veri diski(ler) etkinleştirilebilir.  DBMS TDE şifreleme anahtarlarını tutan diskte ADE kullanmak buna bir örnektir.
1. Performans testi. SAP'de SAP izleme ve ölçümleri temel alınerek şu karşılaştırmaları yapın:
   - Geçerli olduğu durumlarda, en iyi 10 çevrimiçi raporu geçerli uygulamanızla karşılaştırın.
   - Geçerli olduğu durumlarda, en iyi 10 toplu iş için geçerli uygulama karşılaştırın.
   - Arayüzler aracılığıyla veri aktarımlarını SAP sistemine karşılaştırın. Aktarımın şirket içi azure'dan Azure'a geçiş yaptığını bildiğiniz arayüzlere odaklanın.


## <a name="non-production-phase"></a>Üretim dışı faz 
Bu aşamada, başarılı bir pilot veya kavram kanıtı (POC) sonrasında, üretim dışı SAP sistemlerini Azure'a dağıtmaya başladığınızı varsayıyoruz. POC sırasında öğrendiğiniz ve deneyimlediğiniz her şeyi bu dağıtıma dahil edin. POC'lar için listelenen tüm ölçütler ve adımlar bu dağıtım için de geçerlidir.

Bu aşamada, genellikle geliştirme sistemlerini, birim test sistemlerini ve iş regresyon test sistemlerini Azure'a dağıtırsınız. Bir SAP uygulama satırındaki en az bir üretim dışı sistemin gelecekteki üretim sisteminin sahip olacağı yüksek kullanılabilirlik yapılandırması na sahip olduğunu öneririz. Bu aşamada tamamlamanız gereken bazı ek adımlar şunlardır:  

1.  Sistemleri eski platformdan Azure'a taşımadan önce, CPU kullanımı, depolama verisi ve IOPS verileri gibi kaynak tüketim verilerini toplayın. Özellikle dbms katman birimlerinden bu verileri toplamak, ama aynı zamanda uygulama katmanı birimlerinden toplamak. Ayrıca ağ ve depolama gecikmesi ölçün.
2.  Sistemlerinizin kullanılabilirlik kullanım süresi modellerini kaydedin. Amaç, üretim dışı sistemlerin tüm gün, her gün kullanılabilir olması gerekip gerekmediğini veya bir hafta veya ayın belirli aşamalarında kapatılabilen üretim dışı sistemlerin olup olmadığını belirlemektir.
3.  Azure'daki VM'leriniz için kendi işletim sistemi resimlerinizi mi oluşturmak istediğinizi veya Azure Paylaşılan Resim Galerisi'nden bir resim kullanmak isteyip istemediğinizi test edin ve belirleyin. Paylaşılan Resim Galerisi'nden bir resim kullanıyorsanız, işletim sistemi satıcınızla olan destek sözleşmesini yansıtan bir resim kullandığınızdan emin olun. Bazı işletim sistemi satıcıları için Paylaşılan Resim Galerisi kendi lisans resimlerinizi getirmenize olanak tanır. Diğer işletim sistemi görüntüleri için destek, Azure tarafından teklif edilen fiyata dahildir. Kendi işletim sistemi resimlerinizi oluşturmaya karar verirseniz, bu makalelerde belgeler bulabilirsiniz:
    -   [Azure'da dağıtılan bir Windows VM'nin genelleştirilmiş görüntüsünü oluşturma](https://docs.microsoft.com/azure/virtual-machines/windows/capture-image-resource)
    -   [Azure'da dağıtılan bir Linux VM'nin genelleştirilmiş görüntüsünü oluşturma](https://docs.microsoft.com/azure/virtual-machines/linux/capture-image)
3.  Paylaşılan Resim Galerisi'ndeki SUSE ve Red Hat Linux görüntülerini kullanıyorsanız, Paylaşılan Resim Galerisi'ndeki Linux satıcıları tarafından sağlanan SAP görüntülerini kullanmanız gerekir.
4.  Microsoft destek anlaşmaları için SAP destek gereksinimlerini yerine getirdiğinden emin olun. [SAP destek notuna #2015553](https://launchpad.support.sap.com/#/notes/2015553)bakın. HANA Büyük Örnekleri [için, Onboarding gereksinimlerine](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-onboarding-requirements)bakın.
4.  En iyi kapalı kalma sürelerini seçebilmeniz için doğru kişilerin [planlı bakım bildirimleri](https://azure.microsoft.com/blog/a-new-planned-maintenance-experience-for-your-virtual-machines/) aldığından emin olun.
5.  Dağıtımlarınıza uygulanabilecek yeni işlevler için [Kanal 9](https://channel9.msdn.com/) gibi kanallardaki Azure sunularını sık sık denetleyin.
6.  Yeni VM SK'ler ve yeni desteklenen işletim sistemi ve DBMS sürümleri için [destek notu #1928533](https://launchpad.support.sap.com/#/notes/1928533)gibi Azure ile ilgili SAP notlarını kontrol edin. Yeni VM türlerinin fiyatlandırmasını eski VM türleri ile karşılaştırın, böylece VM'leri en iyi fiyat/performans oranıyla dağıtabilirsiniz.
7.  SAP destek notlarını, SAP HANA donanım dizinini ve SAP PAM'i yeniden denetleyin. Azure için desteklenen VM'lerde, bu VM'lerde desteklenen işletim sistemi sürümlerinde ve desteklenen SAP ve DBMS sürümlerinde değişiklik olmadığından emin olun.
8.  Azure'da HANA sertifikalı yeni SUK'lar için [SAP web sitesini](https://www.sap.com/dmc/exp/2014-09-02-hana-hardware/enEN/iaas.html#categories=Microsoft%20Azure) kontrol edin. Yeni STU'ların fiyatlandırmasını kullanmayı planladığınız fiyatlarla karşılaştırın. Sonuç olarak, en iyi fiyat/performans oranına sahip olanları kullanmak için gerekli değişiklikleri yapın.
9.  Dağıtım komut dosyalarınızı yeni VM türlerini kullanacak ve kullanmak istediğiniz yeni Azure özelliklerini birleştirir.
10. Altyapının dağıtımından sonra, SAP destek notları [#500235](https://launchpad.support.sap.com/#/notes/500235) ve [#1100926](https://launchpad.support.sap.com/#/notes/1100926/E)göre, SAP uygulama katmanı VM'ler ve DBMS VM'ler arasındaki ağ gecikmesini test edin ve değerlendirin. [SAP destek notu #1100926'ndeki](https://launchpad.support.sap.com/#/notes/1100926/E)ağ gecikme kılavuzuna göre sonuçları değerlendirin. Ağ gecikmesi orta veya iyi aralıkta olmalıdır. Bu [makalede](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-network-architecture#networking-architecture-for-hana-large-instance)belirtildiği gibi, VM'ler ve HANA Büyük Örnek birimleri arasındaki trafik için özel durumlar geçerlidir. SAP iş yükleri ve [SAP HANA altyapı yapılandırmaları ve Azure'daki işlemler](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-vm-operations) [için Azure Sanal Makineler DBMS dağıtımıiçin Dikkate Alınması'nda](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/dbms_guide_general#azure-network-considerations) belirtilen kısıtlamaların hiçbiri dağıtımınız için geçerli olmadığından emin olun.
11. [SAP uygulamalarında en iyi ağ gecikmesi için Azure yakınlık yerleşim gruplarında](sap-proximity-placement-scenarios.md)açıklandığı gibi, VM'lerinizin doğru [Azure yakınlık yerleşim grubuna](https://docs.microsoft.com/azure/virtual-machines/linux/co-location)dağıtıldığından emin olun.
11. İş yükünü uygulamadan önce kavram aşamasının kanıtı için listelenen diğer tüm denetimleri gerçekleştirin.
12. İş yükü de geçerli olduğu için, sistemlerin kaynak tüketimini Azure'a kaydedin. Bu tüketimi eski platformunuzdaki kayıtlarla karşılaştırın. Büyük farklılıklarınız olduğunu görürseniz, gelecekteki dağıtımların VM boyutlandırmayı ayarlayın. VM'lerin küçültüldüğünüzde, depolama alanı ve ağ bant genişliklerinin de azalacağını unutmayın.
    - [Azure'daki Windows sanal makinesi boyutları](https://docs.microsoft.com/azure/virtual-machines/windows/sizes?toc=%2fazure%2fvirtual-network%2ftoc.json)
    - [Azure'daki Linux sanal makinesi boyutları](https://docs.microsoft.com/azure/virtual-machines/linux/sizes?toc=%2fazure%2fvirtual-network%2ftoc.json) 
13. Sistem kopyalama işlevselliği ve işlemleri ile denemeler. Amaç, proje ekiplerinin yeni sistemleri hızlı bir şekilde alabilmeleri için bir geliştirme sistemini veya test sistemini kopyalamanızı kolaylaştırmaktır. Bu görevler için [SAP LaMa'yı](https://wiki.scn.sap.com/wiki/display/ATopics/SAP+Landscape+Management+%28SAP+LaMa%29+at+a+Glance) kullanmayı düşünün.
14. Görev ayrımına sahip olduğundan emin olmak için ekibinizin Azure rol tabanlı erişimini, izinlerini ve işlemlerini optimize edin ve duruma getirin. Aynı zamanda, tüm ekiplerin görevlerini Azure altyapısında gerçekleştirebilmesini sağlayın.
15. Personelinizin bu görevleri yerine getirmesini sağlamak için yüksek kullanılabilirlik ve olağanüstü durum kurtarma yordamlarını egzersiz yapın, test edin ve belgelayın. Eksiklikleri belirleyin ve dağıtımlarınıza entegre ettiğiniz yeni Azure işlevlerini uyarla.

 
## <a name="production-preparation-phase"></a>Üretim hazırlama aşaması 
Bu aşamada, üretim dışı dağıtımlarınızda deneyimlediğiniz ve öğrendiklerinizi toplayın ve gelecekteki üretim dağıtımlarına uygulayın. Ayrıca, geçerli barındırma konumunuz ile Azure arasındaki veri aktarım çalışmalarını da hazırlamanız gerekir.

1.  Azure'a geçmeden önce üretim sistemlerinizin gerekli SAP sürüm yükseltmelerini tamamlayın.
1.  Üretim sisteminin geçişinden sonra yapılması gereken fonksiyonel ve iş testleri konusunda işletme sahipleriyle aynı fikire gel.
1.  Bu testlerin geçerli barındırma konumundaki kaynak sistemlerle tamamlandığından emin olun. Sistem Azure'a taşındıktan sonra ilk kez test yapmaktan kaçının.
1.  Üretim sistemlerini Azure'a geçirme işlemini test edin. Tüm üretim sistemlerini aynı zaman diliminde Azure'a taşıymıyorsanız, aynı barındırma konumunda olması gereken üretim sistemleri grupları oluşturun. Veri geçişini test edin. Sık karşılaşılan bazı yöntemler şunlardır:
    - SQL Server Always On, HANA System Replication veya Log gönderimi ile birlikte yedekleme/geri yükleme gibi DBMS yöntemlerini kullanarak Azure'daki veritabanı içeriğini tohumlayıp senkronize edin.
    - Daha küçük veritabanları için yedekleme/geri yükleme kullanın.
    - Heterojen geçişler gerçekleştirmek için SAP SWPM'ye entegre edilmiş SAP Geçiş İzleyicisi'ni kullanın.
    - Geçişinizi bir SAP sürüm yükseltmesiyle birleştirmeniz gerekiyorsa [SAP DMO](https://blogs.sap.com/2013/11/29/database-migration-option-dmo-of-sum-introduction/) işlemini kullanın. Kaynak DBMS ve hedef DBMS'nin tüm kombinasyonlarının desteklenmediğini unutmayın. DMO'nun farklı sürümleri için belirli SAP destek notlarında daha fazla bilgi bulabilirsiniz. Örneğin, [SUM 2.0 SP04 Veritabanı Geçiş Seçeneği (DMO).](https://launchpad.support.sap.com/#/notes/2644872)
    - Yedeklemeleri veya SAP dışa aktarma dosyalarını taşımanız gerektiğinde veri aktarım veri aktarım ın internet üzerinden mi yoksa ExpressRoute üzerinden mi daha iyi olduğunu test edin. Verileri Internet üzerinden taşıyorsanız, gelecekteki üretim sistemleri için yerinde olması gereken ağ güvenlik grubu/uygulama güvenlik grubu kurallarının bazılarını değiştirmeniz gerekebilir.
1.  Sistemleri eski platformunuzdan Azure'a taşımadan önce kaynak tüketimi verilerini toplayın. Yararlı veriler CPU kullanımı, depolama verisi ve IOPS verilerini içerir. Özellikle dbms katman birimlerinden bu verileri toplamak, ama aynı zamanda uygulama katmanı birimlerinden toplamak. Ayrıca ağ ve depolama gecikmesi ölçün.
1.  SAP destek notlarını ve gerekli işletim sistemi ayarlarını, SAP HANA donanım dizinini ve SAP PAM'i yeniden denetleyin. Azure için desteklenen VM'lerde, bu VM'lerde desteklenen işletim sistemi sürümlerinde ve desteklenen SAP ve DBMS sürümlerinde değişiklik olmadığından emin olun.
1.  VM türleri ve Azure işlevleri hakkında aldığınız en son kararları dikkate almak için dağıtım komut dosyalarını güncelleştirin.
1.  Altyapı ve uygulamaları dağıttıktan sonra aşağıdakileri doğrulayın:
    - Doğru VM türleri, doğru öznitelikler ve depolama boyutları ile dağıtıldı.
    - VM'ler doğru ve istenilen işletim sistemi sürümlerive yamaları üzerinde ve tek düze.
    - VM'ler gerektiği gibi ve tek tip bir şekilde sertleştirilmiştir.
    - Doğru uygulama sürümleri ve yamalar yüklendi ve dağıtıldı.
    - VM'ler planlandığı gibi Azure kullanılabilirlik kümelerine dağıtıldı.
    - Azure Premium Depolama, gecikmeye duyarlı diskler için veya [%99,9'luk tek VM SLA'sının](https://azure.microsoft.com/support/legal/sla/virtual-machines/v1_8/) gerekli olduğu durumlarda kullanılır.
    - Azure Yazma Hızlandırıcısı doğru şekilde dağıtılır.
        - VM'ler içinde depolama alanları veya şerit kümelerinin Yazma Hızlandırıcısı gerektiren diskler arasında doğru şekilde inşa edildiğinden emin olun.
        - [Linux'ta yazılım RAID yapılandırmasını kontrol edin.](https://docs.microsoft.com/azure/virtual-machines/linux/configure-raid)
        - [Azure'daki Linux VM'lerde LVM yapılandırmasını kontrol edin.](https://docs.microsoft.com/azure/virtual-machines/linux/configure-lvm)
    - [Azure yönetilen diskler](https://azure.microsoft.com/services/managed-disks/) yalnızca kullanılır.
    - VM'ler doğru kullanılabilirlik kümelerine ve Kullanılabilirlik Bölgelerine dağıtıldı.
    - [Azure Hızlandırılmış Ağ,](https://azure.microsoft.com/blog/maximize-your-vm-s-performance-with-accelerated-networking-now-generally-available-for-both-windows-and-linux/) SAP uygulama katmanında ve SAP DBMS katmanında kullanılan VM'lerde etkinleştirilir.
    - SAP uygulaması ile SAP NetWeaver, Hybris veya S/4HANA'ya dayalı SAP sistemlerinin DBMS katmanı arasındaki iletişim yolunda hiçbir [Azure ağı sanal cihazı](https://azure.microsoft.com/solutions/network-appliances/) bulunmaz.
    - Uygulama güvenlik grubu ve ağ güvenliği grubu kuralları, iletişimin istenilen ve planlandığı şekilde engellenmesine ve gerektiğinde iletişimin engellenmesine olanak tanır.
    - Zaman sonu ayarları, daha önce açıklandığı gibi doğru şekilde ayarlanır.
    - VM'ler, [SAP uygulamalarıyla en iyi ağ gecikmesi için Azure yakınlık yerleşim gruplarında](sap-proximity-placement-scenarios.md)açıklandığı gibi doğru [Azure yakınlık yerleşim grubuna](https://docs.microsoft.com/azure/virtual-machines/linux/co-location)dağıtılır.
    - SAP uygulama katmanı VM'ler ve DBMS VM'ler arasındaki ağ gecikmesi, [SAP](https://launchpad.support.sap.com/#/notes/500235) destek notlarında açıklandığı gibi test edilir ve #500235 ve [#1100926.](https://launchpad.support.sap.com/#/notes/1100926/E) [SAP destek notu #1100926'ndeki](https://launchpad.support.sap.com/#/notes/1100926/E)ağ gecikme kılavuzuna göre sonuçları değerlendirin. Ağ gecikmesi orta veya iyi aralıkta olmalıdır. Bu [makalede](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-network-architecture#networking-architecture-for-hana-large-instance)belirtildiği gibi, VM'ler ve HANA Büyük Örnek birimleri arasındaki trafik için özel durumlar geçerlidir.
    - Şifreleme gerektiğinde ve uygun şifreleme yöntemi ile uygulanmıştır.
    - Arabirimler ve diğer uygulamalar yeni dağıtılan altyapıyı bağlayabilir.
1.  Planlanan Azure bakımına tepki vermek için bir oyun kitabı oluşturun. Planlı bakım için sistemlerin ve VM'lerin yeniden başlatılması gereken sırayı belirleyin.
    

## <a name="go-live-phase"></a>Go-live aşaması
Go-live aşamasında, önceki aşamalarda geliştirdiğiniz oyun kitaplarını takip ettiğinizden emin olun. Test ettiğiniz ve uyguladığınız adımları uygulayın. Yapılandırmalarda ve işlemlerde son dakika değişikliklerini kabul etmeyin. Ayrıca şu adımları tamamlayın:

1. Azure portal izleme ve diğer izleme araçlarının çalıştığını doğrulayın. Windows için Windows Performance Monitor (perfmon) ve Linux için SAR öneririz.
    - CPU sayaçları.
        - Ortalama CPU süresi, toplam (tüm CPU'lar)
        - Ortalama işlemci süresi, her bir işlemci (M128 VM'lerde 128 işlemci)
        - CPU çekirdeği süresi, her bir işlemci
        - CPU kullanıcı süresi, her bir işlemci
    - Bellek.
        - Boş bellek
        - Bellek sayfası in/second
        - Bellek sayfası çıktı/saniye
    - Disk.
        - Disk KBps, tek tek disk başına okuyun
        - Disk okur/saniye, tek tek disk başına
        - Disk mikrosaniye/okuma, tek tek disk başına okuma
        - Disk KBps yazma, tek tek disk başına
        - Disk yazma/saniye, tek tek disk başına
        - Disk mikrosaniye/okuma, tek tek disk başına yazma
    - Ağ.
        - Ağ paketleri içinde/saniyede
        - Ağ paketleri çıkış/saniye
        - Ağ KB in/second
        - Ağ KB çıkış/saniye
1.  Veri geçişinden sonra, işletme sahipleriyle anlaştığınız tüm doğrulama testlerini gerçekleştirin. Doğrulama testi sonuçlarını yalnızca özgün kaynak sistemler için sonuçlar olduğunda kabul edin.
1.  Arabirimlerin çalışıp çalışmadığını ve diğer uygulamaların yeni dağıtılan üretim sistemleriyle iletişim kurup kuramayacağını denetleyin.
1.  SAP işlem STMS ile taşıma ve düzeltme sistemini kontrol edin.
1.  Sistem üretim için serbest bırakıldıktan sonra veritabanı yedeklemelerini gerçekleştirin.
1.  Sistem üretim için serbest bırakıldıktan sonra SAP uygulama katmanı VM'leri için VM yedeklemeleri gerçekleştirin.
1.  Geçerli go-live aşamasının bir parçası olmayan ancak bu go-live aşamasında Azure'a taşıdığınız SAP sistemleriyle iletişim kuran SAP sistemleri için, SM51'deki ana bilgisayar ad arabelleği sıfırlamanız gerekir. Bunu yapmak, Azure'a taşıdığınız uygulama örneklerinin adlarıyla ilişkili eski önbelleğe alınmış IP adreslerini kaldırır.  


## <a name="post-production"></a>Post prodüksiyon
Bu aşama, sistemi izlemek, işletmek ve yönetmekle ilgilidir. SAP açısından bakıldığında, eski barındırma konumunuzda tamamlamanız gereken olağan görevler geçerlidir. Azure'a özel bu görevleri de tamamlayın:

1. Yüksek şarj lı sistemler için Azure faturalarını gözden geçirin.
2. VM tarafında ve depolama tarafında fiyat/performans verimliliğini optimize edin.
3. Sistemleri kapatabileceğiniz saatleri optimize edin.  


## <a name="next-steps"></a>Sonraki adımlar
Şu makalelere bakın:

- [SAP NetWeaver için Azure Sanal Makineler planlaması ve uygulaması](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/planning-guide)
- [SAP NetWeaver için Azure Sanal Makineler dağıtımı](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/deployment-guide)
- [SAP iş yükleri için Azure Sanal Makineler DBMS dağıtımında dikkat edilmesi gerekenler](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/dbms_guide_general)

