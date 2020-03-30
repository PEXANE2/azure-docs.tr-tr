---
title: Azure Site Kurtarma ile SQL Server için olağanüstü durum kurtarma ayarlama
description: Bu makalede, SQL Server ve Azure Site Kurtarma kullanarak SQL Server için olağanüstü durum kurtarma nasıl ayarlanır açıklanmaktadır.
services: site-recovery
author: sujayt
manager: rochakm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 08/02/2019
ms.author: sutalasi
ms.openlocfilehash: 429f46156da728bbc24108090eac8c04f68da71c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74084751"
---
# <a name="set-up-disaster-recovery-for-sql-server"></a>SQL Server için olağanüstü durum kurtarma yı ayarlama

Bu makalede, bir uygulamanın SQL Server arka ucunda korunmasına nasıl yardımcı açıklanmaktadır. Bunu, SQL Server iş sürekliliği ve olağanüstü durum kurtarma (BCDR) teknolojileri ve [Azure Site Kurtarma'nın](site-recovery-overview.md)bir birleşimini kullanarak yaparsınız.

Başlamadan önce, SQL Server olağanüstü durum kurtarma özelliklerini anladığınızdan emin olun. Bu özellikler şunları içerir:

* Yük devretme kümelemesi
* Her Zaman Kullanılabilirlik gruplarında
* Veritabanı yansıtma
* Günlük aktarma
* Etkin coğrafi çoğaltma
* Otomatik yük devretme grupları

## <a name="combining-bcdr-technologies-with-site-recovery"></a>BCDR teknolojilerini Site Kurtarma ile birleştirme

SQL Server örneklerini kurtarmak için BCDR teknolojisi seçiminiz, aşağıdaki tabloda açıklandığı gibi kurtarma süresi hedefiniz (RTO) ve kurtarma noktası hedefi (RPO) gereksinimlerine dayalı olmalıdır. Site Kurtarma'yı, tüm uygulamanızın kurtarılmasını düzenlemek için seçtiğiniz teknolojinin başarısız çalışmasıyla birleştirin.

Dağıtım türü | BCDR teknolojisi | SQL Server için Beklenen RTO | SQL Server için beklenen RPO |
--- | --- | --- | ---
SQL Server bir Hizmet olarak Azure altyapısında (IaaS) sanal makine (VM) veya şirket içinde.| [AlwaysOn kullanılabilirlik grubu](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server?view=sql-server-2017) | Birincil olarak ikincil çoğaltma yapmak için alınan süre. | İkincil yinelemeye çoğaltma asynchronous olduğundan, bazı veri kaybı var.
SQL Server, Azure IaaS VM'de veya şirket içinde.| [Failover kümeleme (Her zaman FCI açık)](https://docs.microsoft.com/sql/sql-server/failover-clusters/windows/windows-server-failover-clustering-wsfc-with-sql-server?view=sql-server-2017) | Düğümler arasında başarısız olmak için alınan süre. | Her Zaman FCI'da paylaşılan depolama alanı kullandığından, depolama örneğinin aynı görünümü failover'da kullanılabilir.
SQL Server, Azure IaaS VM'de veya şirket içinde.| [Veritabanı yansıtma (yüksek performans modu)](https://docs.microsoft.com/sql/database-engine/database-mirroring/database-mirroring-sql-server?view=sql-server-2017) | Sıcak bir bekleme sunucusu olarak ayna sunucu kullanan hizmet zorlamak için alınan süre. | Çoğaltma asynchronous olduğunu. Ayna veritabanı, ana veritabanının biraz gerisinde kalabilir. Gecikme genellikle küçüktür. Ancak, ana para veya ayna sunucusunun sistemi ağır bir yük altındaysa büyük olabilir.<br/><br/>Günlük nakliye veritabanı yansıtma için bir ek olabilir. Bu asynchronous veritabanı yansıtma için olumlu bir alternatif.
Sql, Azure'da hizmet olarak platform (PaaS) olarak.<br/><br/>Bu dağıtım türü, elastik havuzları ve Azure SQL Veritabanı sunucularını içerir. | Etkin coğrafi çoğaltma | 30 saniye sonra failover tetiklenir.<br/><br/>İkincil veritabanlarından biri için başarısız lık etkinleştirildiğinde, diğer tüm ikincil ler otomatik olarak yeni birincil veritabanına bağlanır. | Beş saniyelik RPO.<br/><br/>Etkin coğrafi çoğaltma, SQL Server'ın Always On teknolojisini kullanır. Anlık görüntü yalıtımı kullanarak birincil veritabanındaki taahhüt edilen hareketleri ikincil bir veritabanına eşzamanlı olarak çoğaltır.<br/><br/>İkincil verilerin hiçbir zaman kısmi hareketlere sahip olmaması garanti edilir.
Sql paas olarak Azure'da etkin coğrafi çoğaltma ile yapılandırıldı.<br/><br/>Bu dağıtım türü, bir SQL Veritabanı yönetilen örneği, elastik havuzları ve SQL Veritabanı sunucularını içerir. | Otomatik yük devretme grupları | RTO bir saat. | Beş saniyelik RPO.<br/><br/>Otomatik hata grupları, etkin coğrafi çoğaltmanın üstüne grup anlambilimini sağlar. Ama aynı eşzamanlı çoğaltma mekanizması kullanılır.
SQL Server, Azure IaaS VM'de veya şirket içinde.| Azure Site Kurtarma ile Çoğaltma | RTO genellikle 15 dakikadan daha kısadır. Daha fazla bilgi için [Site Kurtarma tarafından sağlanan RTO SLA'yı](https://azure.microsoft.com/support/legal/sla/site-recovery/v1_2/)okuyun. | Uygulama tutarlılığı için bir saat ve kilitlenme tutarlılığı için beş dakika. Daha düşük RPO arıyorsanız, diğer BCDR teknolojilerini kullanın.

> [!NOTE]
> Site Kurtarma ile SQL iş yüklerinin korunmasına yardımcı olurken birkaç önemli husus:
> * Site Kurtarma uygulama agnostik olduğunu. Site Kurtarma, desteklenen bir işletim sisteminde dağıtılan SQL Server sürümünün korunmasına yardımcı olabilir. Daha fazla bilgi edinmek için, çoğaltılan makinelerin [geri kazanımı için destek matrisine](vmware-physical-azure-support-matrix.md#replicated-machines) bakın.
> * Azure, Hyper-V, VMware veya fiziksel altyapıdaki herhangi bir dağıtım için Site Kurtarma'yı kullanmayı seçebilirsiniz. Site Kurtarma ile [bir SQL Server kümesinin korunmasına nasıl yardımcı olunailgili](#how-to-help-protect-a-sql-server-cluster) bu makalenin sonundaki kılavuzu izleyin.
> * Makinede gözlenen veri değiştirme oranının [Site Kurtarma sınırları](vmware-physical-azure-support-matrix.md#churn-limits)içinde olduğundan emin olun. Değişim oranı saniyede bayt yazma ile ölçülür. Windows çalıştıran makinelerde, Görev Yöneticisi'nde **Performans** sekmesini seçerek bu değişiklik oranını görüntüleyebilirsiniz. Her disk için yazma hızını gözlemleyin.
> * Site Kurtarma, Doğrudan Depolama Alanları'ndaki Failover Küme Örneklerinin çoğaltılmasıişlemini destekler. Daha fazla bilgi edinmek için Depolama [Alanları Doğrudan çoğaltmayı nasıl etkinleştirin.](azure-to-azure-how-to-enable-replication-s2d-vms.md)

## <a name="disaster-recovery-of-an-application"></a>Bir uygulamanın olağanüstü durum kurtarma

Site Kurtarma, kurtarma planları yardımıyla test başarısızlığı ve tüm uygulamanızın başarısızlığı sağlar.

Kurtarma planıtamamen ihtiyacınıza göre özelleştirilmiş olduğundan emin olmak için bazı ön koşullar vardır. Herhangi bir SQL Server dağıtımı genellikle bir Active Directory dağıtımı gerekir. Ayrıca, uygulama katmanınız için bağlantı gerekir.

### <a name="step-1-set-up-active-directory"></a>Adım 1: Etkin Dizin ayarlama

SQL Server'ın düzgün çalışması için ikincil kurtarma sitesinde Active Directory'yi ayarlayın.

* **Küçük işletme**: Az sayıda uygulamanız ve şirket içi site için tek bir etki alanı denetleyiciniz vardır. Sitenin tamamında başarısız olmak istiyorsanız, Site Kurtarma çoğaltmasını kullanın. Bu hizmet, etki alanı denetleyicisini ikincil veri merkezine veya Azure'a kopyalar.
* **Orta ve büyük kuruluş**: Ek etki alanı denetleyicileri ayarlamanız gerekebilir.
  - Çok sayıda uygulamanız varsa, Etkin Dizin ormanınvarsa ve uygulama veya iş yükü nedeniyle başarısız olmak istiyorsanız, ikincil veri merkezinde veya Azure'da başka bir etki alanı denetleyicisi ayarlayın.
  -  Uzak bir siteye kurtarmak için Her Zaman kullanılabilirlik gruplarını kullanıyorsanız, ikincil sitede veya Azure'da başka bir etki alanı denetleyicisi ayarlayın. Bu etki alanı denetleyicisi kurtarılan SQL Server örneği için kullanılır.

Bu makaledeki yönergeler, bir etki alanı denetleyicisinin ikincil konumda kullanılabildiğini varsayar. Daha fazla bilgi edinmek [için, Site Kurtarma ile Active Directory'nin korunmasına yardımcı olan](site-recovery-active-directory.md)yordamlara bakın.

### <a name="step-2-ensure-connectivity-with-other-tiers"></a>Adım 2: Diğer katmanlarla bağlantı sağlayın

Veritabanı katmanı hedef Azure bölgesinde çalışmaya devam ettikten sonra, uygulama ve web katmanlarıyla bağlantınız olduğundan emin olun. Test başarısızlığı ile bağlantıyı doğrulamak için gerekli adımları önceden atın.

Bağlantı yla ilgili uygulamalar için uygulamaları nasıl tasarlayabileceğinizi anlamak için aşağıdaki örneklere bakın:

* [Bulut felaketkurtarma için bir uygulama tasarla](../sql-database/sql-database-designing-cloud-solutions-for-disaster-recovery.md)
* [Elastik havuz Afet Kurtarma stratejileri](../sql-database/sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool.md)

### <a name="step-3-interoperate-with-always-on-active-geo-replication-and-auto-failover-groups"></a>Adım 3: Her Zaman Aasla, etkin coğrafi çoğaltma ve otomatik arıza gruplarıyla birlikte çalışma

BCDR teknolojileri Her Zaman Aç, etkin coğrafi çoğaltma ve otomatik hata grupları, hedef Azure bölgesinde çalışan SQL Server'ın ikincil kopyalarına sahiptir. Uygulama başarısız için ilk adım birincil olarak bu yineleme belirtmektir. Bu adım, ikincil bir etki alanı denetleyicisi zaten var varsayar. Otomatik hata yapmayı seçerseniz adım gerekli olmayabilir. Web ve uygulama katmanlarınız üzerinde yalnızca veritabanı nın başarısız olması tamamlandıktan sonra başarısız olun.

> [!NOTE]
> Sql makinelerinin Site Kurtarma ile korunmasına yardımcı olduysanız, bu makinelerden oluşan bir kurtarma grubu oluşturmanız ve bunların geri kazanım planına eklenmesi niz gerekir.

Uygulama ve web katmanı sanal makineleri ile [bir kurtarma planı oluşturun.](site-recovery-create-recovery-plans.md) Aşağıdaki adımlar veritabanı katmanının nasıl başarısız eklendirilebildiğini gösterir:

1. Hem [Kaynak Yöneticisi sanal makinede](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/asr-automation-recovery/scripts/ASR-SQL-FailoverAG.ps1) hem de klasik bir [sanal makinede](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/asr-automation-recovery/scripts/ASR-SQL-FailoverAGClassic.ps1)SQL Kullanılabilirlik Grubu üzerinden başarısız olmak için komut dosyalarını alın. Komut dosyalarını Azure Otomasyon hesabınıza aktarın.

    [!["Azure'a Dağıt" logosunun görüntüsü](https://azurecomcdn.azureedge.net/mediahandler/acomblog/media/Default/blog/c4803408-340e-49e3-9a1f-0ed3f689813d.png)](https://aka.ms/asr-automationrunbooks-deploy)

1. ASR-SQL-FailoverAG komut dosyasını kurtarma planının ilk grubunun ön eylemi olarak ekleyin.

1. Bir otomasyon değişkeni oluşturmak için komut dosyasında bulunan yönergeleri izleyin. Bu değişken kullanılabilirlik gruplarının adını sağlar.

### <a name="step-4-conduct-a-test-failover"></a>Adım 4: Bir test başarısız yapma

SQL Always On gibi bazı BCDR teknolojileri test başarısızlarını desteklemez. Yalnızca *bu tür teknolojileri kullanırken*aşağıdaki yaklaşımı öneririz.

1. Azure'da kullanılabilirlik grubu yinelemesini barındıran VM'de [Azure Yedekleme'yi](../backup/backup-azure-arm-vms.md) ayarlayın.

1. Kurtarma planının test başarısızlığı tetiklemeden önce, önceki adımda alınan yedeklemeden VM'yi kurtarın.

    ![Azure Yedekleme'den yapılandırmayı geri oluşturmak için ekran görüntüsü gösterme penceresi](./media/site-recovery-sql/restore-from-backup.png)

1. VM'de yedekten geri yüklenen [bir çoğunluk zorla.](https://docs.microsoft.com/sql/sql-server/failover-clusters/windows/force-a-wsfc-cluster-to-start-without-a-quorum#PowerShellProcedure)

1. Dinleyicinin IP adresini test başarısız ağında bulunan bir adres olarak güncelleştirin.

    ![Kurallar penceresi nin ekran görüntüsü ve IP adresi özellikleri iletişim kutusu](./media/site-recovery-sql/update-listener-ip.png)

1. Dinleyiciyi çevrimiçi duruma getirin.

    ![Sunucu adlarını ve durumlarını gösteren Content_AG etiketli pencerenin ekran görüntüsü](./media/site-recovery-sql/bring-listener-online.png)

1. Failover ağındaki yük bakiyeleyicisinin, her kullanılabilirlik grubu dinleyicisine karşılık gelen ön uç IP adresi havuzundan ve arka uç havuzundaki SQL Server VM ile bir IP adresi olduğundan emin olun.

     !["SQL-AlwaysOn-LB - Frontend IP Havuzu " başlıklı pencerenin ekran görüntüsü](./media/site-recovery-sql/create-load-balancer1.png)

    !["SQL-AlwaysOn-LB - Backend IP Pool " başlıklı pencerenin ekran görüntüsü](./media/site-recovery-sql/create-load-balancer2.png)

1. Daha sonraki kurtarma gruplarında, bu kurtarma planı için web katmanınızın ardından uygulama katmanınızın başarısızlığı ekleyin.

1. Uygulamanızın uçtan uca başarısız olmasını test etmek için kurtarma planının bir test başarısızlığı yapın.

## <a name="steps-to-do-a-failover"></a>Başarısız olmak için adımlar

Komut dosyasını Adım 3'e ekledikten ve Adım 4'te doğruladıktan sonra, Adım 3'te oluşturulan kurtarma planının başarısız olmasını sağlayabilirsiniz.

Uygulama ve web katmanları için başarısız adımlar hem test başarısız ve başarısız kurtarma planları aynı olmalıdır.

## <a name="how-to-help-protect-a-sql-server-cluster"></a>SQL Server kümesinin korunmasına nasıl yardımcı okadar yardımcı olur

SQL Server Standard edition veya SQL Server 2008 R2 çalıştıran bir küme için, SQL Server'ın korunmasına yardımcı olmak için Site Kurtarma çoğaltmasını kullanmanızı öneririz.

### <a name="azure-to-azure-and-on-premises-to-azure"></a>Azure'dan Azure'a ve Şirket İçlerinde Azure'a

Site Kurtarma, Azure bölgesine çoğaltma yaparken konuk küme desteği sağlamaz. SQL Server Standard sürümü de düşük maliyetli bir olağanüstü durum kurtarma çözümü sağlamaz. Bu senaryoda, SQL Server kümesini birincil konumda bağımsız bir SQL Server örneğine korumanızı ve ikincil konumda kurtarmanızı öneririz.

1. Birincil Azure bölgesinde veya şirket içi sitede ek bir bağımsız SQL Server örneğini yapılandırın.

1. Korumanıza yardımcı olmak istediğiniz veritabanları için bir ayna olarak hizmet vermek üzere örneği yapılandırın. Yansıtmayı yüksek güvenlik modunda yapılandırın.

1. [Azure,](azure-to-azure-tutorial-enable-replication.md) [Hyper-V](site-recovery-hyper-v-site-to-azure.md)veya [VMware VM'ler ve fiziksel sunucular](site-recovery-vmware-to-azure-classic.md)için birincil sitede Site Kurtarma'yı yapılandırın.

1. Yeni SQL Server örneğini ikincil siteye çoğaltmak için Site Kurtarma çoğaltmasını kullanın. Yüksek güvenlikli bir ayna kopyası olduğu için, birincil kümeyle senkronize edilir ancak Site Kurtarma çoğaltma kullanılarak çoğaltılır.

   ![Birincil site, Site Kurtarma ve Azure arasındaki ilişkiyi ve akışı gösteren standart bir kümenin görüntüsü](./media/site-recovery-sql/standalone-cluster-local.png)

### <a name="failback-considerations"></a>Failback hususlar

SQL Server Standart kümeleri için, planlanmamış bir hatadan sonra geri dönüş için bir SQL Server yedeklemesi ve geri yükleme sorabilirsiniz. Bu işlem ayna örneğinden orijinal kümeye, aynanın yeniden kurulması yla yapılır.

## <a name="frequently-asked-questions"></a>Sık sorulan sorular

### <a name="how-does-sql-server-get-licensed-when-used-with-site-recovery"></a>SQL Server, Site Kurtarma ile kullanıldığında nasıl lisanslanır?

SQL Server için Site Kurtarma çoğaltma Yazılım Güvencesi olağanüstü kurtarma avantajı kapsamındadır. Bu kapsam tüm Site Kurtarma senaryoları için geçerlidir: Azure olağanüstü durum kurtarma ve bölgeler arası Azure IaaS olağanüstü durum kurtarma için şirket içi. Daha fazlası için [Azure Site Kurtarma fiyatlandırması](https://azure.microsoft.com/pricing/details/site-recovery/) için bkz.

### <a name="will-site-recovery-support-my-sql-server-version"></a>Site Kurtarma SQL Server sürümümü destekleyecek mi?

Site Kurtarma uygulama agnostik olduğunu. Site Kurtarma, desteklenen bir işletim sisteminde dağıtılan SQL Server sürümünün korunmasına yardımcı olabilir. Daha fazla için, çoğaltılan makinelerin [geri kazanımı için destek matrisine](vmware-physical-azure-support-matrix.md#replicated-machines) bakın.

## <a name="next-steps"></a>Sonraki adımlar

* [Site Kurtarma mimarisi](site-recovery-components.md)hakkında daha fazla bilgi edinin.
* Azure'daki SQL Server için, ikincil bir Azure bölgesinde kurtarma için [yüksek kullanılabilirlik çözümleri](../virtual-machines/windows/sql/virtual-machines-windows-sql-high-availability-dr.md#azure-only-high-availability-solutions) hakkında daha fazla bilgi edinin.
* SQL Veritabanı için, ikinci bir Azure bölgesinde kurtarma için [iş sürekliliği](../sql-database/sql-database-business-continuity.md) ve [yüksek kullanılabilirlik](../sql-database/sql-database-high-availability.md) seçenekleri hakkında daha fazla bilgi edinin.
* Şirket içi SQL Server makineleri için Azure Sanal Makineleri'nde kurtarma için [yüksek kullanılabilirlik seçenekleri](../virtual-machines/windows/sql/virtual-machines-windows-sql-high-availability-dr.md#hybrid-it-disaster-recovery-solutions) hakkında daha fazla bilgi edinin.
