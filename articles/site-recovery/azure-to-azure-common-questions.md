---
title: Azure Site Kurtarma ile Azure VM olağanüstü durum kurtarma ile ilgili sık sorulan sorular
description: Bu makalede, Azure Site Kurtarma kullandığınızda Azure VM olağanüstü durum kurtarma ile ilgili sık sorulan soruları yanıtlar.
author: sideeksh
manager: rochakm
ms.date: 04/29/2019
ms.topic: conceptual
ms.openlocfilehash: 3d71301534d56ef8eca68951c8c9f9a1570b3a6d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80282265"
---
# <a name="common-questions-azure-to-azure-disaster-recovery"></a>Sık sorulan sorular: Azure'dan Azure'a olağanüstü durum kurtarma

Bu makalede, [Azure Site Kurtarma'yı](site-recovery-overview.md)kullandığınızda azure vm'lerinin başka bir Azure bölgesine olağanüstü durum kurtarma ile ilgili sık sorulan soruları yanıtlar.

## <a name="general"></a>Genel

### <a name="how-is-site-recovery-priced"></a>Site Kurtarma nasıl fiyatlandırılır?

[VM'ler için Azure Site Kurtarma fiyatlandırmasının](https://azure.microsoft.com/blog/know-exactly-how-much-it-will-cost-for-enabling-dr-to-your-azure-vm/)gözden geçirilmesi.

### <a name="how-does-the-free-tier-for-azure-site-recovery-work"></a>Azure Site Kurtarma için ücretsiz katman nasıl çalışır?

Azure Site Kurtarma ile korunan her örnek, korumanın ilk 31 günü için ücretsizdir. Bu dönemden sonra, her örnek için koruma, [Azure Sanal Makineleri için Azure Site Kurtarma fiyatlandırmasındaki](https://azure.microsoft.com/blog/know-exactly-how-much-it-will-cost-for-enabling-dr-to-your-azure-vm/)fiyatlara bağlıdır.

### <a name="during-the-first-31-days-will-i-incur-any-other-azure-charges"></a>İlk 31 gün boyunca başka azure ücretlerine maruz kalacak mıyım?

Evet. Azure Site Kurtarma, korumalı bir örneğin ilk 31 günü boyunca ücretsiz olsa da, Azure Depolama, depolama işlemleri ve veri aktarımları için ücrete tabi olabilirsiniz. Kurtarılan bir Sanal Makine de Azure bilgi işlem ücretlerine tabi olabilir. Azure Site Kurtarma [fiyatlandırması](https://azure.microsoft.com/pricing/details/site-recovery)ile ilgili tüm ayrıntıları öğrenin.

### <a name="what-are-the-best-practices-for-azure-virtual-machines-disaster-recovery"></a>Azure Sanal Makineler olağanüstü durum kurtarma için en iyi uygulamalar nelerdir?

1. [Azure'dan Azure mimarisine anlama](azure-to-azure-architecture.md)
1. [Desteklenen ve desteklenmeyen yapılandırmaları gözden geçirme](azure-to-azure-support-matrix.md)
1. [Azure VM'leri için olağanüstü durum kurtarma ayarlama](azure-to-azure-how-to-enable-replication.md)
1. [Yük devretme testi çalıştırma](azure-to-azure-tutorial-dr-drill.md)
1. [Üzerinde başarısız ve birincil bölgeye geri başarısız](azure-to-azure-tutorial-failover-failback.md)

### <a name="how-is-capacity-ensured-in-the-target-region"></a>Hedef bölgede kapasite nasıl sağlanır?

Site Kurtarma ekibi ve Azure kapasite yönetimi ekibi yeterli altyapı kapasitesi için plan sağlar. Bir başarısızlık başlattığınızda, takımlar ayrıca Site Kurtarma tarafından korunan VM örneklerinin hedef bölgeye dağıtılmasını sağlamaya da yardımcı olur.

## <a name="replication"></a>Çoğaltma

### <a name="can-i-replicate-vms-enabled-through-azure-disk-encryption"></a>Azure disk şifrelemesi aracılığıyla etkinleştirilen VM'leri çoğaltabilir miyim?

Evet. Site Kurtarma, Azure Disk Şifrelemesi etkinleştirilmiş VM'lerin olağanüstü kurtarma sını destekler. Çoğaltmayı etkinleştirdiğinizde, Azure gerekli tüm disk şifreleme anahtarlarını ve sırlarını kaynak bölgeden kullanıcı bağlamında hedef bölgeye kopyalar. Uygun izinlere sahip değilseniz, güvenlik yöneticiniz anahtarları ve sırları kopyalamak için bir komut dosyası kullanabilir.

- Site Kurtarma, Windows çalıştıran Azure VM'leri için Azure Disk Şifrelemesini destekler.
- Site Kurtarma, Azure Etkin Dizin (Azure AD) gerektiren bir şema içeren Azure Disk Şifreleme sürümü 0.1'i destekler. Site Kurtarma, Azure AD gerektirmeyen sürüm 1.1'i de destekler. [Azure disk şifrelemesi için uzantı şeması hakkında daha fazla bilgi edinin.](../virtual-machines/extensions/azure-disk-enc-windows.md#extension-schema)
  - Azure Disk Şifreleme sürüm 1.1 için, Windows VM'lerini yönetilen disklere sahip kullanmanız gerekir.
  - Şifreli VM'ler için çoğaltmayı etkinleştirme hakkında [daha fazla bilgi edinin.](azure-to-azure-how-to-enable-replication-ade-vms.md)

### <a name="can-i-replicate-vms-to-another-subscription"></a>VM'leri başka bir aboneliğe çoğaltabilir miyim?

Evet, Azure VM'lerini aynı Azure AD kiracısı içinde farklı bir abonelikle çoğaltabilirsiniz.

Çoğaltma sırasında başka bir abonelik seçerek [abonelikler arasında](https://azure.microsoft.com/blog/cross-subscription-dr) olağanüstü durum kurtarma yapılandırma.

### <a name="can-i-replicate-zone-pinned-azure-vms-to-another-region"></a>Bölge sabitlenmiş Azure VM'lerini başka bir bölgeye çoğaltabilir miyim?

Evet, [bölge sabitlenmiş VM'leri](https://azure.microsoft.com/blog/disaster-recovery-of-zone-pinned-azure-virtual-machines-to-another-region) başka bir bölgeye çoğaltabilirsiniz.

### <a name="can-i-exclude-disks"></a>Diskleri hariç tutabilir miyim?

Evet, PowerShell'i kullanarak koruma sırasında diskleri hariç tutabilirsiniz. Daha fazla bilgi [için, diskleri çoğaltmadan nasıl dışlarız.](azure-to-azure-exclude-disks.md)

### <a name="can-i-add-new-disks-to-replicated-vms-and-enable-replication-for-them"></a>Çoğaltılan VM'lere yeni diskler ekleyebilir ve bunlar için çoğaltmayı etkinleştirebilir miyim?

Evet, çoğaltılan VM'lere yeni diskler eklemek ve bunlar için çoğaltmayı etkinleştirmek, yönetilen disklere sahip Azure VM'leri için desteklenir. Çoğaltma için etkin leştirilmiş bir Azure VM'ine yeni bir disk eklediğinizde, VM için çoğaltma durumu bir uyarı gösterir. Bu uyarı, VM'deki bir veya daha fazla diskin korunmak için kullanılabildiğini belirtir. Eklenen diskler için çoğaltmayı etkinleştirebilirsiniz.

- Eklenen diskler için koruma yı etkinleştiriseniz, uyarı ilk çoğaltmadan sonra kaybolur.
- Disk için çoğaltmayı etkinleştiremezseniz, uyarıyı kapatabilirsiniz.
- Eklenen disk ve çoğaltma etkin leştirilmiş bir VM üzerinde başarısız olursanız, çoğaltma noktaları vardır. Çoğaltma noktaları, kurtarma için kullanılabilir diskleri gösterir.

Örneğin, bir VM'nin tek bir diski olduğunu ve yeni bir disk ekleyeceğinivarsa yalım. Diski eklemeden önce oluşturulmuş bir çoğaltma noktası olabilir. Bu çoğaltma noktası, "2 diskin 1'inden" oluştuğunu gösterir.

Site Kurtarma, çoğaltılan bir VM'den bir diskin "sıcak kaldırma"yı desteklemez. Bir VM diskini kaldırırsanız, VM için çoğaltmayı devre dışı bırakıp yeniden etkinleştirmeniz gerekir.

### <a name="how-often-can-i-replicate-to-azure"></a>Azure'a ne sıklıkta çoğaltabilirim?

Azure VM'lerini başka bir Azure bölgesine kopyalarken çoğaltma işlemi süreklidir. Daha fazla bilgi için [Azure'dan Azure'a çoğaltma mimarisine](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-architecture#replication-process)bakın.

### <a name="can-i-replicate-virtual-machines-within-a-region-i-need-this-functionality-to-migrate-vms"></a>Bir bölgedeki sanal makineleri çoğaltabilir miyim? VM'leri geçirmek için bu işlevselliğe ihtiyacım var.

Bir bölgedeki VM'leri çoğaltmak için Azure'dan Azure'a disk kurtarma çözümünü kullanamazsınız.

### <a name="can-i-replicate-vm-instances-to-any-azure-region"></a>VM örneklerini herhangi bir Azure bölgesine çoğaltabilir miyim?

Site Kurtarma'yı kullanarak, aynı coğrafi küme içindeki herhangi iki bölge arasındaki VM'leri çoğaltabilir ve kurtarabilirsiniz. Coğrafi kümeler veri gecikmesi ve egemenlik göz önünde bulundurularak tanımlanır. Daha fazla bilgi için Site Kurtarma [bölge destek matrisine](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-support-matrix#region-support)bakın.

### <a name="does-site-recovery-require-internet-connectivity"></a>Site Kurtarma internet bağlantısı gerektirir mi?

Hayır, Site Kurtarma internet bağlantısı gerektirmez. Ancak [Azure VM olağanüstü durum kurtarma ağ](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-about-networking#outbound-connectivity-for-ip-address-ranges)belirtildiği gibi, Site Kurtarma URL'leri ve IP aralıkları erişim gerektirir.

### <a name="can-i-replicate-an-application-that-has-a-separate-resource-group-for-separate-tiers"></a>Ayrı katmanlar için ayrı bir kaynak grubu olan bir uygulamayı çoğaltabilir miyim?

Evet, uygulamayı çoğaltabilir ve olağanüstü durum kurtarma yapılandırmasını ayrı bir kaynak grubunda da tutabilirsiniz.

Örneğin, uygulamanız ayrı bir kaynak grubunda her kademenin uygulamasına, veritabanına ve web'ine sahipse, tüm katmanları korumak için [çoğaltma sihirbazını](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-how-to-enable-replication#enable-replication) üç kez seçmeniz gerekir. Site Kurtarma bu üç katmanı üç farklı kaynak grubuna kopyalar.

## <a name="replication-policy"></a>Çoğaltma ilkesi

### <a name="what-is-a-replication-policy"></a>Çoğaltma ilkesi nedir?

Çoğaltma ilkesi, kurtarma noktalarının bekletme geçmişinin ayarlarını tanımlar. İlke, uygulama tutarlı anlık görüntü sıklığı da tanımlar. Varsayılan olarak, Azure Site Kurtarma varsayılan ayarları ile yeni bir çoğaltma ilkesi oluşturur:

- Kurtarma noktalarının bekletme geçmişi için 24 saat.
- Uygulama tutarlı anlık görüntü sıklığı için 60 dakika.

[Çoğaltma ayarları hakkında daha fazla bilgi edinin.](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-tutorial-enable-replication#configure-replication-settings)

### <a name="what-is-a-crash-consistent-recovery-point"></a>Çarpışma tutarlı kurtarma noktası nedir?

Kilitlenme tutarlı kurtarma noktası, anlık görüntü sırasında güç kablosunu sunucudan çekmiş gibi disk teki verilere sahiptir. Kilitlenme tutarlı kurtarma noktası anlık görüntü alındığında bellekte olan hiçbir şey içermez.

Günümüzde, çoğu uygulama kilitlenme tutarlı anlık görüntülerinden iyi kurtarılabilir. Kilitlenme tutarlı kurtarma noktası genellikle dosya sunucuları, DHCP sunucuları ve yazdırma sunucuları gibi veritabanı olmayan işletim sistemleri ve uygulamaları için yeterlidir.

### <a name="what-is-the-frequency-of-crash-consistent-recovery-point-generation"></a>Çarpışma tutarlı kurtarma noktası oluşturma sıklığı nedir?

Site Kurtarma her 5 dakikada bir kilitlenme tutarlı kurtarma noktası oluşturur.

### <a name="what-is-an-application-consistent-recovery-point"></a>Uygulama tutarlı kurtarma noktası nedir?

Uygulama tutarlı kurtarma noktaları, uygulama tutarlı anlık görüntülerden oluşturulur. Uygulama tutarlı kurtarma noktaları, bellekteki ve işlemdeki tüm işlemlerdeki verileri yakalarken, kilitlenme tutarlı anlık görüntülerle aynı verileri yakalar.

Ek içerikleri nedeniyle, uygulama tutarlı anlık görüntüler en çok dahil olan ve en uzun süre yitiren görüntülerdir. SQL Server gibi veritabanı işletim sistemleri ve uygulamaları için uygulama tutarlı kurtarma noktaları öneririz.

### <a name="what-is-the-impact-of-application-consistent-recovery-points-on-application-performance"></a>Uygulama tutarlı kurtarma noktalarının uygulama performansı üzerindeki etkisi nedir?

Uygulama tutarlı kurtarma noktaları bellekte ve işlemde tüm verileri yakalar. Kurtarma noktaları bu verileri yakaladığından, uygulamayı sorgulamak için Windows'taki Toplu Gölge Kopyalama Hizmeti gibi çerçeve gerektirirler. Yakalama işlemi sık sayılsa, iş yükü zaten meşgulken performansı etkileyebilir. Veritabanı olmayan iş yükleri için uygulama tutarlı kurtarma noktaları için düşük frekans kullanmanızı önermiyoruz. Veritabanı iş yükü için bile 1 saat yeterlidir.

### <a name="what-is-the-minimum-frequency-of-application-consistent-recovery-point-generation"></a>Uygulama tutarlı kurtarma noktası oluşturma nın minimum sıklığı nedir?

Site Kurtarma, en az 1 saatlik sıklıkta uygulama tutarlı bir kurtarma noktası oluşturabilir.

### <a name="how-are-recovery-points-generated-and-saved"></a>Kurtarma noktaları nasıl oluşturulur ve kaydedilir?

Site Kurtarma'nın kurtarma noktalarını nasıl oluşturduğunu anlamak için çoğaltma ilkesinin bir örneğini görelim. Bu çoğaltma ilkesi, 24 saatlik bekletme penceresi ve 1 saatlik uygulama tutarlı frekans anlık görüntüsü ile bir kurtarma noktası vardır.

Site Kurtarma her 5 dakikada bir kilitlenme tutarlı kurtarma noktası oluşturur. Bu frekansı değiştiremezsin. Son bir saat için, 12 kilitlenme tutarlı nokta ve 1 uygulama tutarlı nokta arasından seçim yapabilirsiniz. Zaman ilerledikçe, Site Kurtarma son saatin ötesindeki tüm kurtarma noktalarını budar ve saatte yalnızca 1 kurtarma noktası kaydeder.

Aşağıdaki ekran görüntüsü örneği göstermektedir. Ekran görüntüsünde:

- Son bir saat içinde, 5 dakikalık bir frekans ile kurtarma noktaları vardır.
- Son bir saatin ötesinde, Site Kurtarma yalnızca 1 kurtarma noktası tutar.

   ![Oluşturulan kurtarma noktalarılistesi](./media/azure-to-azure-troubleshoot-errors/recoverypoints.png)

### <a name="how-far-back-can-i-recover"></a>Ne kadar geri dönebilirim?

Kullanabileceğiniz en eski kurtarma noktası 72 saattir.

### <a name="i-have-a-replication-policy-of-24-hours-what-will-happen-if-a-problem-prevents-site-recovery-from-generating-recovery-points-for-more-than-24-hours-will-my-previous-recovery-points-be-lost"></a>24 saatlik bir çoğaltma politikam var. Bir sorun Site Kurtarma'nın 24 saatten fazla kurtarma noktaları oluşturmasını engellerse ne olur? Önceki iyileşme puanlarım kaybolacak mı?

Hayır, Site Kurtarma önceki tüm kurtarma noktaları tutar. Kurtarma noktalarının bekletme penceresine bağlı olarak, Site Kurtarma yalnızca yeni noktalar oluşturursa en eski noktanın yerini alır. Sorun nedeniyle, Site Kurtarma yeni kurtarma noktaları oluşturamaz. Yeni kurtarma noktaları olana kadar, bekletme penceresine ulaştıktan sonra tüm eski noktalar kalır.

### <a name="after-replication-is-enabled-on-a-vm-how-do-i-change-the-replication-policy"></a>VM'de çoğaltma etkinleştirildikten sonra çoğaltma ilkesini nasıl değiştirebilirim?

Site **Kurtarma Vault** > **Site Kurtarma Altyapı** > **Çoğaltma ilkeleri**gidin. Yapmak istediğiniz ilkeyi seçin ve değişiklikleri kaydedin. Herhangi bir değişiklik varolan tüm çoğaltmalar için de geçerli olacaktır.

### <a name="are-all-the-recovery-points-a-complete-copy-of-the-vm-or-a-differential"></a>Tüm kurtarma noktaları VM'nin veya diferansiyelin tam bir kopyası mı?

Oluşturulan ilk kurtarma noktası tam kopyasına sahiptir. Ardışık kurtarma noktalarında delta değişiklikleri vardır.

### <a name="does-increasing-the-retention-period-of-recovery-points-increase-the-storage-cost"></a>Kurtarma noktalarının bekletme süresini artırmak depolama maliyetini artırır mı?

Evet, bekletme süresini 24 saatten 72 saate çıkarsanız, Site Kurtarma kurtarma noktalarını ek 48 saat kaydeder. Eklenen süre depolama ücretlerine neden olur. Örneğin, tek bir kurtarma noktası, ayda 0,16 TL'lik GB başına maliyeti yle 10 GB'lık delta değişikliklere sahip olabilir. Ek ücretler 1,60 $ × 48 aylık olacaktır.

## <a name="multi-vm-consistency"></a>Çoklu VM tutarlılığı

### <a name="what-is-multi-vm-consistency"></a>Çoklu VM tutarlılığı nedir?

Multi-VM tutarlılığı, kurtarma noktasının çoğaltılan tüm sanal makinelerde tutarlı olmasını sağlar.

Site Kurtarma, tüm makinelerin çoğaltma grubunu oluşturan bir **Multi-VM tutarlılık** seçeneği sağlar.

Sanal makineler üzerinde başarısız olduğunuzda, kilitlenme tutarlı ve uygulama tutarlı kurtarma noktaları paylaştılar.

[Çoklu VM tutarlılığını sağlamak](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-tutorial-enable-replication#enable-replication-for-a-vm)için öğreticiye gidin.

### <a name="can-i-fail-over-a-single-virtual-machine-within-a-multi-vm-consistency-replication-group"></a>Çoklu VM tutarlılık çoğaltma grubu içinde tek bir sanal makine üzerinde başarısız olabilir miyim?

**Multi-VM tutarlılık** seçeneğini seçtiğinizde, uygulamanın bir grup içindeki tüm sanal makinelere bağımlı olduğunu belirtirsiniz. Tek bir sanal makine nin üzerinden geçilen arızaya izin verilmez.

### <a name="how-many-virtual-machines-can-i-replicate-as-a-part-of-a-multi-vm-consistency-replication-group"></a>Çoklu VM tutarlılık çoğaltma grubunun bir parçası olarak kaç sanal makine çoğaltabilirim?

Bir çoğaltma grubunda 16 sanal makineyi birlikte çoğaltabilirsiniz.

### <a name="when-should-i-enable-multi-vm-consistency"></a>Çoklu VM tutarlılığını ne zaman etkinleştirmeliyim?

Çoklu VM tutarlılığı CPU yoğun olduğundan, etkinleştirme iş yükü performansını etkileyebilir. Multi-VM tutarlılığını yalnızca makineler aynı iş yükünü çalıştırıyorsa ve birden çok makinede tutarlılığa ihtiyacınız varsa kullanın. Örneğin, bir uygulamada iki SQL Server örneği ve iki web sunucunuz varsa, yalnızca SQL Server örnekleri için çoklu VM tutarlılığına sahip olmalısınız.

### <a name="can-you-add-an-already-replicating-vm-to-a-replication-group"></a>Zaten çoğaltma grubuna zaten çoğalan bir VM ekleyebilir misiniz?

Çoğaltmayı etkinleştirirken yeni bir çoğaltma grubuna VM ekleyebilirsiniz. Çoğaltmayı etkinleştirirken varolan bir çoğaltma grubuna vm de ekleyebilirsiniz. Ancak, zaten çoğaltma vm yeni bir çoğaltma grubuna veya varolan çoğaltma grubuna ekleyemezsiniz.

## <a name="failover"></a>Yük devretme

### <a name="how-is-capacity-ensured-in-the-target-region-for-azure-vms"></a>Azure VM'ler için hedef bölgede kapasite nasıl sağlanır?

Site Kurtarma ekibi ve Azure kapasite yönetimi ekibi yeterli altyapı kapasitesi için plan sağlar. Bir başarısızlık başlattığınızda, takımlar ayrıca Site Kurtarma tarafından korunan VM örneklerinin hedef bölgeye dağıtılmasını sağlamaya da yardımcı olur.

### <a name="is-failover-automatic"></a>Yük devretme işlemi otomatik midir?

Yük devretme işlemi otomatik değildir. Portalda tek bir tıklamayla başarısız olmaya başlayabilir veya [powershell'i](azure-to-azure-powershell.md) kullanarak bir başarısızı tetikleyebilirsiniz.

### <a name="can-i-keep-a-public-ip-address-after-a-failover"></a>Bir hatadan sonra herkese açık bir IP adresi tutabilir miyim?

Bir arızadan sonra üretim uygulamasının genel IP adresini tutamazsınız.

Başarısız olma işleminin bir parçası olarak bir iş yükü getirdiyseniz, iş yüküne bir Azure genel IP kaynağı atamanız gerekir. Azure genel IP kaynağının hedef bölgede kullanılabilmesi gerekir. Azure genel IP kaynağını el ile atayabilir veya kurtarma planıyla otomatikleştirebilirsiniz. Başarısız olduktan sonra genel IP adreslerini nasıl [ayarlayatılamayı](concepts-public-ip-address-with-site-recovery.md#public-ip-address-assignment-using-recovery-plan)öğrenin.

### <a name="can-i-keep-a-private-ip-address-during-a-failover"></a>Bir arıza sırasında özel bir IP adresi tutabilir miyim?

Evet, özel bir IP adresi tutabilirsiniz. Varsayılan olarak, Azure VM'leri için olağanüstü durum kurtarmayı etkinleştirdiğinizde, Site Kurtarma kaynak kaynak ayarlarını temel alan hedef kaynakları oluşturur. Statik IP adresleriyle yapılandırılan Azure Sanal Makineler için Site Kurtarma, kullanımda değilse hedef VM için aynı IP adresini sağlamaya çalışır.
[Failover sırasında IP adreslerini tutma](site-recovery-retain-ip-azure-vm-failover.md)hakkında bilgi edinin.

### <a name="after-a-failover-why-is-the-server-assigned-a-new-ip-address"></a>Bir arıza dan sonra, neden sunucuya yeni bir IP adresi atanır?

Site Kurtarma, başarısız olduğu sırada IP adresini sağlamaya çalışır. Başka bir sanal makine bu adresi alıyorsa, Site Kurtarma bir sonraki kullanılabilir IP adresini hedef olarak ayarlar.

[Sanal ağlar için ağ eşleme ve IP adresleme ayarlama](azure-to-azure-network-mapping.md#set-up-ip-addressing-for-target-vms)hakkında daha fazla bilgi edinin.

### <a name="what-are-latest-lowest-rpo-recovery-points"></a>En **Son (en düşük RPO)** kurtarma noktaları nelerdir?

**En Son (en düşük RPO)** seçeneği, önce Site Kurtarma'ya gönderilen tüm verileri işler. Hizmet verileri işledikten sonra, VM'ye geçmeden önce her VM için bir kurtarma noktası oluşturur. Bu seçenek en düşük kurtarma noktası hedefi (RPO) sağlar. Failover'dan sonra oluşturulan VM, hatanın tetiklediği andan itibaren Site Kurtarma'ya çoğaltılan tüm verileri vardır.

### <a name="do-latest-lowest-rpo-recovery-points-have-an-impact-on-failover-rto"></a>**En son (en düşük RPO)** kurtarma noktalarının RTO üzerinde başarısız olması üzerinde bir etkisi var mı?

Evet. Site Kurtarma, başarısız olmadan önce bekleyen tüm verileri işler, bu nedenle bu seçenek diğer seçeneklerle karşılaştırıldığında daha yüksek bir kurtarma süresi hedefine (RTO) sahiptir.

### <a name="what-does-the-latest-processed-option-in-recovery-points-mean"></a>Kurtarma noktalarındaki **en son işlenmiş** seçenek ne anlama gelir?

**En son işlenen** seçenek, plandaki tüm VM'ler üzerinde Site Kurtarma'nın işlendiği en son kurtarma noktasına kadar başarısız olur. Belirli bir VM için en son kurtarma noktasını görmek için VM ayarlarında **En Son Kurtarma Noktaları'na** bakın. İşlenmemiş verileri işlemek için zaman harcandığından, bu seçenek düşük bir RTO sağlar.

### <a name="what-happens-if-my-primary-region-experiences-an-unexpected-outage"></a>Birincil bölgemde beklenmedik bir kesinti olursa ne olur?

Kesintiden sonra bir arıza tetikleyebilir. Site Kurtarma, başarısızlığı sağlamak için birincil bölgeden bağlantı ya da bağlantıya ihtiyaç duymaz.

### <a name="what-is-an-rto-of-a-vm-failover"></a>VM başarısızlığı ndan rto nedir?

Site Kurtarma 2 saat bir [RTO SLA](https://azure.microsoft.com/support/legal/sla/site-recovery/v1_2/)vardır. Ancak, çoğu zaman, Site Kurtarma dakika içinde sanal makineler üzerinde başarısız olur. VM'yi gündeme getirmek için gereken zamanı gösteren başarısız işlere giderek RTO'yu hesaplayabilirsiniz. Kurtarma planı RTO için bir sonraki bölüme bakın.

## <a name="recovery-plans"></a>Kurtarma planları

### <a name="what-is-a-recovery-plan"></a>Kurtarma planı nedir?

Site Kurtarma'daki bir kurtarma planı, VM'lerin başarısız kurtarma sını yönetir. Kurtarmanın tutarlı bir şekilde doğru, yinelenebilir ve otomatik hale alınmasına yardımcı olur. Kurtarma planı aşağıdaki gereksinimleri giderer:

- Birlikte başarısız olan bir sanal makine grubunu tanımlama
- Uygulamanın doğru bir şekilde ortaya çıkması için sanal makineler arasındaki bağımlılıkların tanımlanması
- Sanal makinelerin başarısız lığı dışındaki görevleri yerine getirmek için özel el ile işlemlerle birlikte kurtarmayı otomatikleştirmek

Kurtarma [planları oluşturma hakkında](site-recovery-create-recovery-plans.md)daha fazla bilgi edinin.

### <a name="how-is-sequencing-achieved-in-a-recovery-plan"></a>Kurtarma planında sıralama nasıl sağlanır?

Kurtarma planında, sıralamayı gerçekleştirmek için birden çok grup oluşturabilirsiniz. Her grup aynı anda başarısız olur. Aynı grubun parçası olan sanal makineler birlikte başarısız olur ve ardından başka bir grup tarafından izlenir. Kurtarma planı kullanarak bir uygulamayı nasıl modellendireceklerini öğrenmek için kurtarma [planları hakkında'ya](recovery-plan-overview.md#model-apps)bakın.

### <a name="how-can-i-find-the-rto-of-a-recovery-plan"></a>Kurtarma planının RTO'yu nasıl bulabilirim?

Kurtarma planının RTO'yu denetlemek için, kurtarma planı için bir test başarısızlığı yapın ve **Site Kurtarma işlerine**gidin.
Aşağıdaki örnekte, iş **SAPTestRecoveryPlan**bakın. İş tüm sanal makineler üzerinde başarısız ve belirtilen eylemleri yapmak için 8 dakika ve 59 saniye sürdü.

![Site Kurtarma işleri listesi](./media/azure-to-azure-troubleshoot-errors/recoveryplanrto.PNG)

### <a name="can-i-add-automation-runbooks-to-the-recovery-plan"></a>Kurtarma planına otomasyon runbook'ları ekleyebilir miyim?

Evet, Azure Otomasyon runbook'larını kurtarma planınıza entegre edebilirsiniz. [Azure Otomasyon uyrularını ekleme](site-recovery-runbook-automation.md)hakkında daha fazla bilgi edinin.

## <a name="reprotection-and-failback"></a>Yeniden koruma ve geri dönüş

### <a name="i-failed-over-from-the-primary-region-to-a-disaster-recovery-region-are-vms-in-a-dr-region-protected-automatically"></a>Birincil bölgeden felaket kurtarma bölgesine kadar başarısız oldum. DR bölgesindeki VM'ler otomatik olarak korunuyor mu?

Hayır. Azure VM'leri bir bölgeden diğerine [geçemediğinizi,](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-tutorial-failover-failback) VM'ler DR bölgesinde korumasız bir durumda başlar. Birincil bölgeye VM'leri geri başarısız olmak için ikincil bölgedeki VM'leri [yeniden korumanız](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-how-to-reprotect) gerekir.

### <a name="at-the-time-of-reprotection-does-site-recovery-replicate-complete-data-from-the-secondary-region-to-the-primary-region"></a>Yeniden koruma sırasında, Site Kurtarma ikincil bölgeden birincil bölgeye tam verileri çoğaltır mı?

Duruma göre değişir. Kaynak bölge VM varsa, yalnızca kaynak disk ve hedef disk arasındaki değişiklikler eşitlenir. Site Kurtarma diskleri karşılaştırarak diferansiyelleri hesaplamak ve daha sonra verileri aktarır. Bu işlem genellikle birkaç saat sürer. Yeniden koruma sırasında ne olduğu hakkında daha fazla bilgi için [bkz.](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-how-to-reprotect#what-happens-during-reprotection)

### <a name="how-much-time-does-it-take-to-fail-back"></a>Yeniden çalışma ne kadar zaman alır?

Yeniden korumadan sonra, geri dönüş birincil bölgeden ikincil bir bölgeye başarısız olmak için gereken süreyle aynı miktarda alır.

## <a name="capacity"></a><a name="capacity"></a>Kapasite

### <a name="how-is-capacity-ensured-in-the-target-region-for-azure-vms"></a>Azure VM'ler için hedef bölgede kapasite nasıl sağlanır?

Site Kurtarma ekibi ve Azure kapasite yönetimi ekibi yeterli altyapı kapasitesi için plan sağlar. Bir başarısızlık başlattığınızda, takımlar ayrıca Site Kurtarma tarafından korunan VM örneklerinin hedef bölgeye dağıtılmasını sağlamaya da yardımcı olur.

### <a name="does-site-recovery-work-with-reserved-instances"></a>Site Kurtarma ayrılmış örneklerle çalışır mı?

Evet, olağanüstü durum kurtarma bölgesinde [ayrılmış Azure VM'leri](https://azure.microsoft.com/pricing/reserved-vm-instances/) satın alabilirsiniz ve Site Kurtarma başarısız işlemleri bunları kullanır. Ek yapılandırma gerekmez.

## <a name="security"></a>Güvenlik

### <a name="is-replication-data-sent-to-the-site-recovery-service"></a>Çoğaltılan veriler Site Recovery hizmetine gönderilir mi?

Hayır, Site Kurtarma çoğaltılan verileri engellemez ve Sanal Bilgisayarlarınızda ne yle ilgili herhangi bir bilgi yoktur. Yalnızca çoğaltma ve yük devretme işlemlerini düzenlemek için gereken meta veriler Site Recovery hizmetine gönderilir.

Site Kurtarma ISO 27001:2013, 27018, HIPAA ve DPA sertifikalıdır. Hizmet SOC2 ve FedRAMP JAB değerlendirmelergeçiyor.

### <a name="does-site-recovery-encrypt-replication"></a>Site Recovery çoğaltma işlemini şifreleyebilir mi?

Evet, hem aktarım şifreleme hem de [Azure'da kalan şifreleme](https://docs.microsoft.com/azure/storage/storage-service-encryption) desteklenir.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure'dan Azure'a destek gereksinimlerini gözden geçirin.](azure-to-azure-support-matrix.md)
- [Azure'dan Azure'a çoğaltma'yı ayarlayın.](azure-to-azure-tutorial-enable-replication.md)
- Bu makaleyi okuduktan sonra sorularınız varsa, [bunları Azure Kurtarma Hizmetleri forumuna](https://social.msdn.microsoft.com/Forums/azure/home?forum=hypervrecovmgr)gönderin.
