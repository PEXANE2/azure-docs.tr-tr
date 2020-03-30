---
title: Azure Site Kurtarma ile VMware olağanüstü durum kurtarma
description: Bu makalede, Azure Site Kurtarma hizmetini kullanarak VMware VM'lerin Azure'a olağanüstü durum kurtarma sına genel bir bakış sunulmaktadır.
author: rayne-wiselman
ms.service: site-recovery
ms.topic: conceptual
ms.date: 11/12/2019
ms.author: raynew
ms.openlocfilehash: 589dda80d68fba73a729da4b6e59270cc09c18cb
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "73954384"
---
# <a name="about-disaster-recovery-of-vmware-vms-to-azure"></a>VMware VM'lerin Azure'a olağanüstü kurtarma sı hakkında

Bu makalede, [Azure Site Kurtarma](site-recovery-overview.md) hizmetini kullanarak şirket içi VMware VM'lerden Azure'a olağanüstü durum kurtarma genel bakışı sunulmaktadır.

## <a name="what-is-bcdr"></a>BCDR nedir?

İş sürekliliği ve olağanüstü durum kurtarma (BCDR) stratejisi, işinizi çalışır durumda tutmaya yardımcı olur. Planlanan kapalı kalma süresi ve beklenmeyen kesintiler sırasında BCDR verileri güvenli ve kullanılabilir tutar ve uygulamaların çalışmaya devam edilmesini sağlar. Azure, bölgesel eşleştirme ve yüksek kullanılabilirlik depolama gibi platform BCDR özelliklerine ek olarak, BCDR çözümünüzün ayrılmaz bir parçası olarak Kurtarma Hizmetleri sağlar. Kurtarma hizmetleri şunları içerir: 

- [Azure Yedekleme,](https://docs.microsoft.com/azure/backup/backup-introduction-to-azure-backup) şirket içi ve Azure VM verilerinizi yedekler. Bir dosyayı ve klasörleri, belirli iş yüklerini veya tüm VM'yi yedekleyebilirsiniz. 
- [Azure Site Kurtarma,](site-recovery-overview.md) şirket içi makinelerde çalışan uygulamalar ve iş yükleri veya Azure IaaS VM'leri için esneklik ve olağanüstü durum kurtarma sağlar. Site Kurtarma çoğaltmayı yönetir ve kesintiler olduğunda Azure'a başarısız olur. Ayrıca, Azure'dan birincil sitenize kurtarma işlemlerini de işler. 

## <a name="how-does-site-recovery-do-disaster-recovery"></a>Site Kurtarma olağanüstü durum kurtarma yı nasıl yapar?

1. Azure'u ve şirket içi sitenizi hazırladıktan sonra, şirket içi makineleriniz için çoğaltmayı ayarlar ve etkinleştirin.
2. Site Kurtarma, ilke ayarlarınıza uygun olarak makinenin ilk çoğaltmasını yönetir.
3. İlk çoğaltmadan sonra, Site Kurtarma delta değişikliklerini Azure'da çoğaltır. 
4. Her şey beklendiği gibi çoğalırken, bir felaket kurtarma tatbikatı çalıştırın.
    - Matkap, gerçek bir ihtiyaç ortaya çıktığında, arızanın beklendiği gibi çalışmasını sağlamaya yardımcı olur.
    - Matkap, üretim ortamınızı etkilemeden bir test hatası gerçekleştirir.
5. Bir kesinti oluşursa, Azure'da tam bir kesinti çalıştırın. Tek bir makine de başarısız olabilir veya aynı anda birden çok makine üzerinde başarısız bir kurtarma planı oluşturabilirsiniz.
6. Azure VM'leri, yönetilen disklerde veya depolama hesaplarındaki VM verilerinden oluşturulur. Kullanıcılar Azure VM'den uygulamalara ve iş yüklerine erişmeye devam edebilir
7. Şirket içi siteniz yeniden kullanılabilir olduğunda Azure'dan geri dönmezsiniz.
8. Geri başarısız olduktan ve birincil sitenizden bir kez daha çalıştıktan sonra, şirket içi VM'leri yeniden Azure'a çoğaltmaya başlarsınız.


## <a name="how-do-i-know-if-my-environment-is-suitable-for-disaster-recovery-to-azure"></a>Ortamımın Azure'a olağanüstü durum kurtarma için uygun olup olmadığını nasıl anlarım?

Site Kurtarma, desteklenen bir VMware VM veya fiziksel sunucuda çalışan tüm iş yükünü çoğaltabilir. Ortamınızda kontrol etmeniz gereken şeyler şunlardır:

- VMware VM'leri kopyalarsanız, VMware sanallaştırma sunucularının doğru sürümlerini mi çalıştırıyorsunuz? [Burada kontrol edin.](vmware-physical-azure-support-matrix.md#on-premises-virtualization-servers)
- Çoğaltmak istediğiniz makineler desteklenen bir işletim sistemi çalıştırıyor mu? [Burada kontrol edin.](vmware-physical-azure-support-matrix.md#replicated-machines)
- Linux olağanüstü durum kurtarma için, makineler desteklenen bir dosya sistemi / konuk depolama çalışıyor? [Buraya bakın](vmware-physical-azure-support-matrix.md#linux-file-systemsguest-storage)
- Çoğaltmak istediğiniz makineler Azure gereksinimlerine uygun mu? [Burada kontrol edin.](vmware-physical-azure-support-matrix.md#azure-vm-requirements)
- Ağ yapılandırmanız desteklendi mi? [Burada kontrol edin.](vmware-physical-azure-support-matrix.md#network)
- Depolama yapılandırmanız desteklendi mi? [Burada kontrol edin.](vmware-physical-azure-support-matrix.md#storage)


## <a name="what-do-i-need-to-set-up-in-azure-before-i-start"></a>Başlamadan önce Azure'da ne ayarlamam gerekir?

Azure'da aşağıdakileri hazırlamanız gerekir:

1. Azure hesabınızın Azure'da VM oluşturma izinlerine sahip olduğunu doğrulayın.
2. Azure VM'lerinin depolama hesaplarından veya yönetilen disklerden oluşturulduklarında birleşeceği bir Azure ağı oluşturun.
3. Site Kurtarma için bir Azure Kurtarma Hizmetleri kasası ayarlayın. Kasa Azure portalında bulunur ve Site Kurtarma dağıtımınızı dağıtmak, yapılandırmak, düzenlemek, izlemek ve sorun gidermek için kullanılır.

*Daha fazla yardım mı gerekiyor?*

[Hesabınızı doğrulayarak,](tutorial-prepare-azure.md#verify-account-permissions) [ağ](tutorial-prepare-azure.md#set-up-an-azure-network)oluşturarak ve kasa oluşturarak Azure'u nasıl [ayarlayayınız](tutorial-prepare-azure.md#create-a-recovery-services-vault)öğrenin.



## <a name="what-do-i-need-to-set-up-on-premises-before-i-start"></a>Başlamadan önce şirket içinde ne ayarlamam gerekiyor?

Şirket içinde yapmanız gerekenler şunlardır:

1. Birkaç hesap ayarlamanız gerekir:

    - VMware VM'leri kopyalıyorsanız, VM'leri otomatik olarak keşfetmek için Site Kurtarma'nın vCenter Server veya vSphere ESXi ana bilgisayarlarına erişmesi için bir hesap gereklidir.
    - Çoğaltmak istediğiniz her fiziksel makineye veya VM'ye Site Kurtarma Mobilitehizmeti aracısını yüklemek için bir hesap gereklidir.

2. Bunu daha önce yapmadıysanız VMware altyapınızın uyumluluğunu kontrol etmiş olmanız gerekir.
3. Bir arızadan sonra Azure VM'lere bağlanabildiğinizden emin olun. RDP'yi şirket içi Windows makinelerinde veya Linux makinelerinde SSH'de ayarlarsınız.

*Daha fazla yardım mı gerekiyor?*
- [Otomatik keşif](vmware-azure-tutorial-prepare-on-premises.md#prepare-an-account-for-automatic-discovery) ve [Mobilite hizmetinin kurulumu](vmware-azure-tutorial-prepare-on-premises.md#prepare-an-account-for-mobility-service-installation)için hesap hazırlayın.
- VMware ayarlarınızın uyumlu olduğundan [doğrulayın.](vmware-azure-tutorial-prepare-on-premises.md#check-vmware-requirements)
- [Prepare](vmware-azure-tutorial-prepare-on-premises.md#prepare-to-connect-to-azure-vms-after-failover) Başarısız olduktan sonra Azure'a bağlanın.
- Başarısız olduktan sonra Azure VM'leri için IP adresleme ayarlama konusunda daha ayrıntılı yardım almak istiyorsanız, [bu makaleyi okuyun.](concepts-on-premises-to-azure-networking.md)

## <a name="how-do-i-set-up-disaster-recovery"></a>Olağanüstü durum kurtarmayı nasıl ayarlıyorum?

Azure ve şirket içi altyapınızı uygulamaya açtıktan sonra olağanüstü durum kurtarma yı ayarlayabilirsiniz.

1. Dağıtmanız gereken bileşenleri anlamak için [VMware'i Azure mimarisine](vmware-azure-architecture.md)ve [fizikselden Azure mimarisine](physical-azure-architecture.md)inceleyin. Birkaç bileşen vardır, bu nedenle hepsinin birbirine nasıl uyduğunu anlamak önemlidir.
2. **Kaynak ortamı**: Dağıtımın ilk adımı olarak çoğaltma kaynak ortamınızı ayarlarsınız. Neyi çoğaltmak istediğinizi ve nerede çoğaltmak istediğinizi belirtirsiniz.
3. **Configuration server**: Şirket içi kaynak ortamınızda bir yapılandırma sunucusu ayarlamanız gerekir:
    - Yapılandırma sunucusu tek bir şirket içi makinedir. VMware olağanüstü durum kurtarma için, indirilebilir ovf şablonundan dağıtılabilir bir VMware VM olarak dağıtmanızı öneririz.
    - Yapılandırma sunucusu şirket içi ve Azure arasındaki iletişimi koordine eder
    - Yapılandırma sunucusu makinesinde çalışan diğer bileşenlerden birkaçı.
        - İşlem sunucusu, çoğaltma verilerini Azure'daki önbellek depolama hesabına alır, optimize eder ve gönderir. Ayrıca çoğaltmak istediğiniz makinelerde Mobilite hizmetinin otomatik olarak yüklenmesini işler ve VMware sunucularında VM'lerin otomatik olarak keşfini gerçekleştirir.
        - Ana hedef sunucu, Azure'dan yeniden çalışma sırasında çoğaltma verilerini işler.
    - Kurulum, yapılandırma sunucusunun kasaya kaydedilmesi, MySQL Server ve VMware PowerCLI'nin indirilmesi ve otomatik bulma ve Mobilite hizmet yüklemesi için oluşturulan hesapların belirtilmesi içerir.
4. **Hedef ortamı**: Azure aboneliğinizi ve ağ ayarlarınızı belirterek hedef Azure ortamınızı ayarlarsınız.
5. **Çoğaltma ilkesi**: Çoğaltmanın nasıl oluştuğunu belirtirsiniz. Ayarlar, kurtarma noktalarının ne sıklıkta oluşturulduğunu ve depolandığını ve uygulama tutarlı anlık görüntü oluşturulup oluşturulmayacağını içerir.
6. **Çoğaltmayı etkinleştirin.** Şirket içi makineler için çoğaltmayı etkinleştirin. Mobilite hizmetini yüklemek için bir hesap oluşturduysanız, bir makine için çoğaltmayı etkinleştirdiğinizde bu hesap yüklenir. 

*Daha fazla yardım mı gerekiyor?*

- Bu adımlarıhızlı bir walkthrough için, bizim [VMware öğretici](vmware-azure-tutorial.md)deneyebilirsiniz, ve [fiziksel sunucu walkthrough](physical-azure-disaster-recovery.md).
- Kaynak ortamınızı ayarlama hakkında [daha fazla bilgi edinin.](vmware-azure-set-up-source.md)
- Yapılandırma sunucusu gereksinimleri ve VMware çoğaltma için bir OVF şablonu ile yapılandırma sunucusu ayarlama [hakkında bilgi edinin.](vmware-azure-deploy-configuration-server.md) Herhangi bir nedenle şablon kullanamıyorsanız veya fiziksel sunucuları çoğaltıyorsanız, [bu yönergeleri kullanın.](physical-azure-set-up-source.md#set-up-the-source-environment)
- Hedef ayarları hakkında [daha fazla bilgi edinin.](vmware-azure-set-up-target.md)
- Çoğaltma ilkesi ayarlama hakkında [daha fazla bilgi alın.](vmware-azure-set-up-replication.md)
- [Çoğaltmayı](vmware-azure-enable-replication.md) nasıl etkinleştiracağınızı öğrenin ve diskleri çoğaltmadan [hariç takın.](vmware-azure-exclude-disk.md)


## <a name="something-went-wrong-how-do-i-troubleshoot"></a>Bir şeyler ters gitti, nasıl sorun giderebilirim?

- İlk adım olarak, çoğaltılan öğelerin, işlerin ve altyapı sorunlarının durumunu doğrulamak ve hataları tanımlamak için [dağıtımınızı izlemeyi](site-recovery-monitor-and-troubleshoot.md) deneyin.
- İlk çoğaltmayı tamamlayamıyorsanız veya devam eden çoğaltma beklendiği gibi çalışmıyorsa, sık karşılaşılan hatalar ve sorun giderme ipuçları için [bu makaleyi gözden geçirin.](vmware-azure-troubleshoot-replication.md)
- Çoğaltmak istediğiniz makinelerde Mobilite hizmetinin otomatik olarak yüklenmesiyle ilgili sorunlar yaşıyorsanız, [bu makaledeki](vmware-azure-troubleshoot-push-install.md)yaygın hataları gözden geçirin.
- Failover beklendiği gibi çalışmıyorsa, bu [makalede](site-recovery-failover-to-azure-troubleshoot.md)sık karşılaşılan hataları denetleyin.
- Geri ödeme çalışmıyorsa, sorununuzun [bu makalede](vmware-azure-troubleshoot-failback-reprotect.md)görünüp görünmediğini denetleyin.



## <a name="next-steps"></a>Sonraki adımlar

Çoğaltma şimdi yerinde, bu failover beklendiği gibi çalıştığından emin olmak için [bir felaket kurtarma matkap çalıştırmalısınız.](tutorial-dr-drill-azure.md) 
