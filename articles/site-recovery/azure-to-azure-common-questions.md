---
title: Azure Site Recovery ile Azure VM olağanüstü durum kurtarma hakkında sık sorulan sorular
description: Bu makalede Azure Site Recovery kullandığınızda Azure VM olağanüstü durum kurtarma hakkında sık sorulan sorular yanıtlanmaktadır.
author: sideeksh
manager: rochakm
ms.date: 04/29/2019
ms.topic: conceptual
ms.openlocfilehash: 99fed1d2b1246e4c099f275708f694e5d7ea2f22
ms.sourcegitcommit: b07964632879a077b10f988aa33fa3907cbaaf0e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/13/2020
ms.locfileid: "77190834"
---
# <a name="common-questions-azure-to-azure-disaster-recovery"></a>Sık sorulan sorular: Azure 'dan Azure 'a olağanüstü durum kurtarma

Bu makalede, [Azure Site Recovery](site-recovery-overview.md)kullandığınızda Azure VM 'lerinin olağanüstü durum kurtarma hakkında sık sorulan sorular ve diğer Azure bölgelerine yanıt verir.

## <a name="general"></a>Genel

### <a name="how-is-site-recovery-priced"></a>Site Recovery nasıl fiyatlandırılır?

[VM 'ler için Azure Site Recovery fiyatlandırmasını](https://azure.microsoft.com/blog/know-exactly-how-much-it-will-cost-for-enabling-dr-to-your-azure-vm/)gözden geçirin.

### <a name="how-does-the-free-tier-for-azure-site-recovery-work"></a>Azure Site Recovery için ücretsiz katman nasıl çalışır?

Azure Site Recovery ile korunan her örnek, ilk 31 gün koruma için ücretsizdir. Bu dönemden sonra, her bir örnek için koruma, [Azure sanal makineleri için Azure Site Recovery fiyatlandırmasına](https://azure.microsoft.com/blog/know-exactly-how-much-it-will-cost-for-enabling-dr-to-your-azure-vm/)göre ücretlendirilir.

### <a name="during-the-first-31-days-will-i-incur-any-other-azure-charges"></a>İlk 31 gün boyunca diğer Azure ücretlerine tabi mıyım?

Evet. Korunan bir örneğin ilk 31 gün içinde Azure Site Recovery boş olsa da, Azure depolama, depolama işlemleri ve veri aktarımları için ücret ödemeniz istenebilir. Kurtarılan bir sanal makine de Azure işlem ücretlerine neden olabilir. Fiyatlandırma hakkında [Azure Site Recovery fiyatlandırmayla](https://azure.microsoft.com/pricing/details/site-recovery)ilgili tüm ayrıntıları alın.

### <a name="what-are-the-best-practices-for-azure-virtual-machines-disaster-recovery"></a>Azure sanal makineler olağanüstü durum kurtarma için en iyi uygulamalar nelerdir?

1. [Azure 'dan Azure 'a mimariyi anlama](azure-to-azure-architecture.md)
1. [Desteklenen ve desteklenmeyen konfigürasyonları gözden geçirin](azure-to-azure-support-matrix.md)
1. [Azure VM 'Leri için olağanüstü durum kurtarmayı ayarlama](azure-to-azure-how-to-enable-replication.md)
1. [Yük devretme testi çalıştırma](azure-to-azure-tutorial-dr-drill.md)
1. [Yük devretme ve birincil bölgeye dönme](azure-to-azure-tutorial-failover-failback.md)

### <a name="how-is-capacity-ensured-in-the-target-region"></a>Hedef bölgede kapasite nasıl belirlenir?

Site Recovery Team ve Azure kapasite yönetimi ekibi, yeterli altyapı kapasitesi için plan yapın. Yük devretme başlattığınızda takımlar, Site Recovery tarafından korunan sanal makine örneklerinin hedef bölgeye dağıtılmasını sağlamaya de yardımcı olur.

## <a name="replication"></a>Çoğaltma

### <a name="can-i-replicate-vms-enabled-through-azure-disk-encryption"></a>Azure disk şifrelemesi aracılığıyla etkinleştirilen VM 'Leri çoğaltabilir miyim?

Evet. Site Recovery, Azure Disk Şifrelemesi etkin olan VM 'Lerde olağanüstü durum kurtarmayı destekler. Çoğaltmayı etkinleştirdiğinizde, Azure tüm gerekli disk şifreleme anahtarlarını ve gizli dizileri kaynak bölgesinden Kullanıcı bağlamındaki hedef bölgeye kopyalar. Uygun izinleriniz yoksa güvenlik yöneticiniz, anahtar ve gizli dizileri kopyalamak için bir komut dosyası kullanabilir.

- Site Recovery, Windows çalıştıran Azure VM 'Leri için Azure disk şifrelemeyi destekler.
- Site Recovery, Azure Active Directory (Azure AD) gerektiren bir şemaya sahip Azure disk şifreleme sürümü 0,1 ' ü destekler. Site Recovery Ayrıca, Azure AD gerektirmeyen 1,1 sürümünü de destekler. [Azure disk şifrelemesi için şemaların serileştirilmesi uzantısı hakkında daha fazla bilgi edinin](../virtual-machines/extensions/azure-disk-enc-windows.md#extension-schemata).
  - Azure disk şifrelemesi sürüm 1,1 için, yönetilen disklerle Windows VM 'Leri kullanmanız gerekir.
  - Şifrelenmiş VM 'Ler için çoğaltmayı etkinleştirme hakkında [daha fazla bilgi edinin](azure-to-azure-how-to-enable-replication-ade-vms.md) .

### <a name="can-i-replicate-vms-to-another-subscription"></a>VM 'Leri başka bir aboneliğe çoğaltabilir miyim?

Evet, Azure VM 'Leri aynı Azure AD kiracısı içindeki farklı bir aboneliğe çoğaltabilirsiniz.

Çoğaltma sırasında başka bir abonelik seçerek, [abonelikler arasında](https://azure.microsoft.com/blog/cross-subscription-dr) olağanüstü durum kurtarmayı yapılandırın.

### <a name="can-i-replicate-zone-pinned-azure-vms-to-another-region"></a>Bölgeye sabitlenmiş Azure sanal makinelerini başka bir bölgeye çoğaltabilir miyim?

Evet, [bölgeye sabitlenmiş VM 'leri](https://azure.microsoft.com/blog/disaster-recovery-of-zone-pinned-azure-virtual-machines-to-another-region) başka bir bölgeye çoğaltabilirsiniz.

### <a name="can-i-exclude-disks"></a>Diskleri hariç tutabilir miyim?

Evet, PowerShell kullanarak diskleri koruma sırasında dışlayabilirsiniz. Daha fazla bilgi için bkz. [diskleri çoğaltmanın dışında tutma](azure-to-azure-exclude-disks.md).

### <a name="can-i-add-new-disks-to-replicated-vms-and-enable-replication-for-them"></a>Çoğaltılan VM 'lere yeni diskler ekleyebilir ve bu disklere yönelik çoğaltmayı etkinleştirebilir miyim?

Evet, çoğaltılan VM 'lere yeni diskler eklemek ve bunların çoğaltılmasını etkinleştirmek, yönetilen disklere sahip Azure sanal makinelerinde desteklenir. Çoğaltma için etkinleştirilen bir Azure VM 'ye yeni bir disk eklediğinizde, sanal makine için çoğaltma durumu bir uyarı gösterir. Bu uyarı, VM 'deki bir veya daha fazla diskin koruma için kullanılabilir olduğunu belirtir. Eklenen diskler için çoğaltmayı etkinleştirebilirsiniz.

- Eklenen diskler için korumayı etkinleştirirseniz, uyarı ilk çoğaltmadan sonra kaybolacaktır.
- Disk için çoğaltmayı etkinleştirmezseniz, uyarıyı kapatabilirsiniz.
- Eklenmiş bir disk ve çoğaltma etkin olan bir VM 'nin yükünü devretmek, çoğaltma noktaları vardır. Çoğaltma noktalarında, kurtarma için kullanılabilen diskler gösterilir. 

Örneğin, bir VM 'nin tek bir diski olduğunu ve yeni bir tane ekleneceğini varsayalım. Diski eklemeden önce oluşturulmuş bir çoğaltma noktası olabilir. Bu çoğaltma noktası, "1/2 diskten" oluştuğunu gösterir.

Site Recovery, çoğaltılan bir VM 'den bir diskin "Hot Remove" öğesini desteklemez. VM diskini kaldırırsanız VM için çoğaltmayı devre dışı bırakıp yeniden etkinleştirmeniz gerekir.

### <a name="how-often-can-i-replicate-to-azure"></a>Azure 'a ne sıklıkta çoğaltma yapabilirim?

Azure VM 'lerini başka bir Azure bölgesine çoğaltdığınızda çoğaltma işlemi sürekli olur. Daha fazla bilgi için bkz. [Azure 'Dan Azure 'a çoğaltma mimarisi](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-architecture#replication-process).

### <a name="can-i-replicate-virtual-machines-within-a-region-i-need-this-functionality-to-migrate-vms"></a>Bir bölgedeki sanal makineleri çoğaltabilir miyim? VM 'Leri geçirmek için bu işlevselliğe ihtiyacım var.

Bir bölgedeki VM 'Leri çoğaltmak için Azure 'dan Azure 'a disk kurtarma çözümü kullanamazsınız.

### <a name="can-i-replicate-vm-instances-to-any-azure-region"></a>Sanal makine örneklerini herhangi bir Azure bölgesine çoğaltabilir miyim?

Site Recovery kullanarak, aynı coğrafi küme içindeki iki bölge arasında VM 'Leri çoğaltıp kurtarabilirsiniz. Coğrafi kümeler, veri gecikmesi ve sogemenlik göz önünde bulundurularak tanımlanmıştır. Daha fazla bilgi için Site Recovery [bölge desteği matrisine](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-support-matrix#region-support)bakın.

### <a name="does-site-recovery-require-internet-connectivity"></a>Site Recovery internet bağlantısı gerektiriyor mu?

Hayır, Site Recovery Internet bağlantısı gerektirmez. Ancak, [Azure VM olağanüstü durum kurtarma 'daki ağlarda](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-about-networking#outbound-connectivity-for-ip-address-ranges)belirtildiği gibi Site Recovery URL 'ler ve IP aralıkları için erişim gerektirir.

### <a name="can-i-replicate-an-application-that-has-a-separate-resource-group-for-separate-tiers"></a>Ayrı katmanlar için ayrı bir kaynak grubuna sahip olan bir uygulamayı çoğaltabilir miyim?

Evet, uygulamayı çoğaltabilir ve olağanüstü durum kurtarma yapılandırmasını aynı şekilde ayrı bir kaynak grubunda tutabilirsiniz.

Örneğin, uygulamanız ayrı bir kaynak grubunda her bir katmanın uygulamasına, veritabanına ve Web 'e sahipse, tüm katmanları korumak için [Çoğaltma Sihirbazı 'nı](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-how-to-enable-replication#enable-replication) üç kez seçmeniz gerekir. Site Recovery, bu üç katmanı üç farklı kaynak grubuna çoğaltacaktır.

## <a name="replication-policy"></a>Çoğaltma ilkesi

### <a name="what-is-a-replication-policy"></a>Çoğaltma ilkesi nedir?

Çoğaltma İlkesi, kurtarma noktalarının bekletme geçmişine yönelik ayarları tanımlar. İlke, uygulamayla tutarlı anlık görüntülerin sıklığını da tanımlar. Varsayılan olarak, Azure Site Recovery varsayılan ayarları ile yeni bir çoğaltma ilkesi oluşturur:

- Kurtarma noktalarının bekletme geçmişi için 24 saat.
- uygulamayla tutarlı anlık görüntülerin sıklığı için 60 dakika.

[Çoğaltma ayarları hakkında daha fazla bilgi edinin](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-tutorial-enable-replication#configure-replication-settings).

### <a name="what-is-a-crash-consistent-recovery-point"></a>Kilitlenme ile tutarlı bir kurtarma noktası nedir?

Kilitlenme ile tutarlı bir kurtarma noktası, anlık görüntü sırasında sunucudan güç kablosunu çektiği gibi disk üzerindeki verilere sahiptir. Kilitlenme ile tutarlı kurtarma noktası, anlık görüntü çekilirken bellekte olan herhangi bir şeyi içermez.

Günümüzde, çoğu uygulama kilitlenme ile tutarlı anlık görüntülerden iyi bir şekilde kurtarabilir. Kilitlenme ile tutarlı bir kurtarma noktası, genellikle veritabanı olmayan işletim sistemleri ve dosya sunucuları, DHCP sunucuları ve yazdırma sunucuları gibi uygulamalar için yeterlidir.

### <a name="what-is-the-frequency-of-crash-consistent-recovery-point-generation"></a>Kilitlenme ile tutarlı kurtarma noktası oluşturma sıklığı nedir?

Site Recovery 5 dakikada bir çökme ile tutarlı bir kurtarma noktası oluşturur.

### <a name="what-is-an-application-consistent-recovery-point"></a>Uygulamayla tutarlı kurtarma noktası nedir?

Uygulamayla tutarlı kurtarma noktaları uygulamayla tutarlı anlık görüntülerden oluşturulur. Uygulamayla tutarlı kurtarma noktaları aynı verileri çökme ile tutarlı anlık görüntülerle yakalar, ayrıca bellekteki verileri ve işlemdeki tüm işlemleri de yakalarsınız.

Ek içerikleri nedeniyle, uygulamayla tutarlı anlık görüntüler en çok söz konusu ve en uzun sürer. Veritabanı işletim sistemleri ve SQL Server gibi uygulamalar için uygulamayla tutarlı kurtarma noktaları öneririz.

### <a name="what-is-the-impact-of-application-consistent-recovery-points-on-application-performance"></a>Uygulamayla tutarlı kurtarma noktalarının uygulama performansı üzerindeki etkisi nedir?

Uygulamayla tutarlı kurtarma noktaları, bellekteki ve işlemdeki tüm verileri yakalar. Kurtarma noktaları bu verileri yakaladığı için, uygulamayı sessiz bir şekilde açmak için Windows üzerinde Birim Gölge Kopyası Hizmeti gibi bir çerçeve gerektirir. Yakalama işlemi sık sık ise, iş yükü zaten meşgul olduğunda performansı etkileyebilir. Veritabanı olmayan iş yükleri için uygulamayla tutarlı kurtarma noktaları için düşük bir sıklık kullanmanızı önermiyoruz. Veritabanı iş yükü için bile 1 saat yeterlidir.

### <a name="what-is-the-minimum-frequency-of-application-consistent-recovery-point-generation"></a>Uygulamayla tutarlı kurtarma noktası oluşturma işlemi için en düşük sıklık nedir?

Site Recovery, en az 1 saatlik bir sıklıkta uygulamayla tutarlı bir kurtarma noktası oluşturabilir.

### <a name="how-are-recovery-points-generated-and-saved"></a>Kurtarma noktaları nasıl oluşturulup kaydedilir?

Site Recovery kurtarma noktaları oluşturma hakkında bilgi edinmek için bir çoğaltma ilkesi örneği görelim. Bu çoğaltma ilkesinde, 24 saatlik bir bekletme penceresi ve 1 saatlik uygulamayla tutarlı sıklık anlık görüntüsüne sahip bir kurtarma noktası bulunur.

Site Recovery 5 dakikada bir çökme ile tutarlı bir kurtarma noktası oluşturur. Bu sıklığı değiştiremezsiniz. Son bir saat için, 12 çökme ile tutarlı nokta ve 1 uygulamayla tutarlı nokta arasından seçim yapabilirsiniz. Zaman ilerledikçe Site Recovery, tüm kurtarma noktalarını son saatin ötesinde ayıklar ve yalnızca saat başına 1 kurtarma noktası kaydeder.

Aşağıdaki ekran görüntüsünde örnek gösterilmektedir. Ekran görüntüsünde:

- Son saat içinde, 5 dakikalık bir sıklıkta kurtarma noktaları vardır.
- Son saatin ötesinde Site Recovery yalnızca 1 kurtarma noktası tutar.

   ![Oluşturulan kurtarma noktalarının listesi](./media/azure-to-azure-troubleshoot-errors/recoverypoints.png)

### <a name="how-far-back-can-i-recover"></a>Ne kadar geri kurtarabilirim?

Kullanabileceğiniz en eski kurtarma noktası 72 saattir.

### <a name="i-have-a-replication-policy-of-24-hours-what-will-happen-if-a-problem-prevents-site-recovery-from-generating-recovery-points-for-more-than-24-hours-will-my-previous-recovery-points-be-lost"></a>24 saat olan bir çoğaltma ilkem var. Bir sorun Site Recovery, 24 saatten uzun bir bir kurtarma noktası oluşturmasını engelliyorsa ne olur? Önceki kurtarma noktalarım kaybedilir mi?

Hayır, Site Recovery önceki tüm kurtarma noktalarınızı tutacaktır. Kurtarma noktalarının bekletme penceresine bağlı olarak, Site Recovery en eski noktayı yalnızca yeni noktaları oluşturursa değiştirir. Sorun nedeniyle, Site Recovery yeni kurtarma noktası üretemiyor. Yeni kurtarma noktaları olana kadar, bekletme penceresine erişduktan sonra tüm eski noktaların kalması gerekir.

### <a name="after-replication-is-enabled-on-a-vm-how-do-i-change-the-replication-policy"></a>VM 'de çoğaltma etkinleştirildikten sonra, çoğaltma ilkesini nasıl değiştirebilirim?

Altyapı > **çoğaltma ilkelerine** **Site Recovery** **Site Recovery kasa** > gidin. Düzenlemek istediğiniz ilkeyi seçin ve değişiklikleri kaydedin. Tüm değişiklikler, mevcut tüm çoğaltmalar için de geçerli olacaktır.

### <a name="are-all-the-recovery-points-a-complete-copy-of-the-vm-or-a-differential"></a>Tüm kurtarma noktaları VM 'nin ya da bir Farklıdan oluşan tüm bir kopyası mı?

Oluşturulan ilk kurtarma noktasının tamamen kopyası vardır. Tüm ardışık kurtarma noktalarında delta değişiklikleri vardır.

### <a name="does-increasing-the-retention-period-of-recovery-points-increase-the-storage-cost"></a>Kurtarma noktalarının bekletme döneminin artırılması depolama maliyetini arttırır mi?

Evet, bekletme süresini 24 saat ile 72 saate artırırsanız Site Recovery, kurtarma noktalarını ek 48 saat boyunca kaydeder. Eklenen süre, depolama ücretlerine tabi olacaktır. Örneğin, tek bir kurtarma noktası, ayda $0,16 GB başına maliyet ile 10 GB 'lik Delta değişikliklere sahip olabilir. Ek ücretler ayda $1,60 × 48 olacaktır.

## <a name="multi-vm-consistency"></a>Çoklu VM tutarlılığı

### <a name="what-is-multi-vm-consistency"></a>Çoklu VM tutarlılığı nedir?

Çoklu VM tutarlılığı, kurtarma noktasının tüm çoğaltılan sanal makinelerde tutarlı olmasını sağlar.

Site Recovery, tüm makinelerin çoğaltma grubunu oluşturan **Çoklu VM tutarlılığı** seçeneğini sağlar.

Sanal makinelerin yükünü devretmek için, paylaşılan kilitlenme ile tutarlı ve uygulamayla tutarlı kurtarma noktalarına sahip olurlar.

[Çoklu VM tutarlılığını etkinleştirmek](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-tutorial-enable-replication#enable-replication-for-a-vm)için Öğreticiye gidin.

### <a name="can-i-fail-over-a-single-virtual-machine-within-a-multi-vm-consistency-replication-group"></a>Çoklu VM tutarlılığı çoğaltma grubundaki tek bir sanal makinenin yükünü devreder miyim?

**Çoklu VM tutarlılığı** seçeneğini belirlediğinizde, uygulamanın bir grup içindeki tüm sanal makinelere bağımlılığı olduğunu siz belirteceğiz. Tek sanal makineye yük devretmeye izin verilmiyor.

### <a name="how-many-virtual-machines-can-i-replicate-as-a-part-of-a-multi-vm-consistency-replication-group"></a>Çoklu VM tutarlılığı çoğaltma grubunun bir parçası olarak kaç sanal makine çoğaltırım?

Bir çoğaltma grubunda, 16 sanal makineyi birlikte çoğaltabilirsiniz.

### <a name="when-should-i-enable-multi-vm-consistency"></a>Çoklu VM tutarlılığını ne zaman etkinleştirmem gerekir?

Çoklu VM tutarlılığı CPU yoğun olduğundan, bunun etkinleştirilmesi iş yükü performansını etkileyebilir. Çoklu VM tutarlılığı yalnızca makineler aynı iş yükünü çalıştırıyorsa ve birden çok makine arasında tutarlılık gerekiyorsa kullanın. Örneğin, bir uygulamada iki SQL Server örneğe ve iki Web sunucusuna sahipseniz, yalnızca SQL Server örnekleri için çoklu VM tutarlılığı olmalıdır.

## <a name="failover"></a>Yük devretme

### <a name="how-is-capacity-ensured-in-the-target-region-for-azure-vms"></a>Azure VM 'Leri için hedef bölgede kapasite nasıl belirlenir?

Site Recovery Team ve Azure kapasite yönetimi ekibi, yeterli altyapı kapasitesi için plan yapın. Yük devretme başlattığınızda takımlar, Site Recovery tarafından korunan sanal makine örneklerinin hedef bölgeye dağıtılmasını sağlamaya de yardımcı olur.

### <a name="is-failover-automatic"></a>Yük devretme işlemi otomatik midir?

Yük devretme işlemi otomatik değildir. Portalda tek bir tıklama ile yük devretme başlatabilir veya [PowerShell](azure-to-azure-powershell.md) 'i kullanarak yük devretmeyi tetikleyebilirsiniz.

### <a name="can-i-keep-a-public-ip-address-after-a-failover"></a>Yük devretmeden sonra genel IP adresini tutabilir miyim?

Yük devretmeden sonra üretim uygulamasının genel IP adresini saklayamıyorum.

Yük devretme sürecinin bir parçası olarak bir iş yükünü aldığınızda, iş yüküne bir Azure genel IP kaynağı atamanız gerekir. Azure genel IP kaynağı, hedef bölgede kullanılabilir olmalıdır. Azure genel IP kaynağını el ile atayabilir veya bir kurtarma planıyla otomatikleştirebilmeniz gerekir. [Yük devretmeden sonra genel IP adreslerini ayarlamayı](concepts-public-ip-address-with-site-recovery.md#public-ip-address-assignment-using-recovery-plan)öğrenin.  

### <a name="can-i-keep-a-private-ip-address-during-a-failover"></a>Yük devretme sırasında özel bir IP adresini tutabilir miyim?

Evet, özel bir IP adresi tutabilirsiniz. Varsayılan olarak, Azure VM 'Leri için olağanüstü durum kurtarmayı etkinleştirdiğinizde Site Recovery kaynak kaynak ayarlarına dayalı olarak hedef kaynakları oluşturur. Statik IP adresleriyle yapılandırılmış Azure sanal makineleri için Site Recovery, kullanımda değilse hedef VM için aynı IP adresini sağlamaya çalışır.
[Yük devretme SıRASıNDA IP adreslerini tutma](site-recovery-retain-ip-azure-vm-failover.md)hakkında bilgi edinin.

### <a name="after-a-failover-why-is-the-server-assigned-a-new-ip-address"></a>Yük devretme işleminden sonra sunucu neden yeni bir IP adresi atamış?

Site Recovery, yük devretme sırasında IP adresini sağlamaya çalışır. Başka bir sanal makine bu adresi alıyorsa, Site Recovery hedef olarak bir sonraki kullanılabilir IP adresini ayarlar.

[Sanal ağlar için ağ eşlemesi ve IP adresleme ayarlama](azure-to-azure-network-mapping.md#set-up-ip-addressing-for-target-vms)hakkında daha fazla bilgi edinin.

### <a name="what-are-latest-lowest-rpo-recovery-points"></a>**En son (en düşük RPO)** kurtarma noktası nedir?

**En son (en düşük RPO)** seçeneği ilk olarak Site Recovery gönderilen tüm verileri işler. Hizmet verileri işleyerek, VM 'ye yük devrettikten önce her VM için bir kurtarma noktası oluşturur. Bu seçenek en düşük kurtarma noktası hedefini (RPO) sağlar. Yük devretmeden sonra oluşturulan VM, yük devretme tetiklendiğinde Site Recovery ' a çoğaltılan tüm verileri içerir.

### <a name="do-latest-lowest-rpo-recovery-points-have-an-impact-on-failover-rto"></a>**En son (en düşük RPO)** kurtarma noktalarının yük devretme RTO üzerinde bir etkisi var mı?

Evet. Site Recovery, yük devretmeye başlamadan önce tüm bekleyen verileri işler, bu nedenle bu seçeneğin diğer seçeneklerle karşılaştırıldığında daha yüksek bir kurtarma süresi hedefi (RTO) vardır.

### <a name="what-does-the-latest-processed-option-in-recovery-points-mean"></a>Kurtarma noktalarında **en son işlenen** seçeneği ne anlama geliyor?

**Son işlenen** seçeneği, plandaki tüm VM 'lerin üzerinde Site Recovery işlenen en son kurtarma noktasına devredilmelidir. Belirli bir sanal makine için en son kurtarma noktasını görmek üzere VM ayarlarındaki **en son kurtarma noktalarını** kontrol edin. İşlenmemiş verileri işlemek için zaman harcanmadığından, bu seçenek düşük bir RTO sağlar.

### <a name="what-happens-if-my-primary-region-experiences-an-unexpected-outage"></a>Birincil bölgem beklenmeyen bir kesinti yaşıyorsa ne olur?

Kesinti sonrasında yük devretme tetikleyebilirsiniz. Site Recovery yük devretmeyi yapmak için birincil bölgeden bağlantı gerektirmez.

### <a name="what-is-an-rto-of-a-vm-failover"></a>VM yük devretmesinin RTO nedir?

Site Recovery, [RTO 2 saatlık SLA 'ya](https://azure.microsoft.com/support/legal/sla/site-recovery/v1_2/)sahiptir. Ancak, çoğu zaman Site Recovery sanal makineler üzerinde birkaç dakika içinde başarısız olur. Yük devretme işlerine giderek RTO 'ı hesaplayabilirsiniz, bu da VM 'yi getirmek için geçen süreyi gösterir. Kurtarma planı RTO için bir sonraki bölüme bakın.

## <a name="recovery-plans"></a>Kurtarma planları

### <a name="what-is-a-recovery-plan"></a>Kurtarma planı nedir?

Site Recovery bir kurtarma planı, VM 'lerin yük devretme kurtarmasını düzenler. Kurtarmaya sürekli olarak doğru, yinelenebilir ve otomatik hale getirme konusunda yardımcı olur. Kurtarma planı aşağıdaki ihtiyaçları ele alınmaktadır:

- Birlikte yük devretme yapan bir sanal makine grubu tanımlama
- Uygulamanın doğru olması için sanal makineler arasındaki bağımlılıkları tanımlama
- Sanal makinelerin yük devretmesi dışında görevler elde etmek için el ile yapılan özel eylemlerle birlikte kurtarmayı otomatikleştirme

[Kurtarma planlarını oluşturma hakkında](site-recovery-create-recovery-plans.md)daha fazla bilgi edinin.

### <a name="how-is-sequencing-achieved-in-a-recovery-plan"></a>Bir kurtarma planında sıralama nasıl sağlanır?

Bir kurtarma planında sıralama elde etmek için birden çok grup oluşturabilirsiniz. Her grup, bir seferde devredildi. Aynı grubun parçası olan sanal makineler, ardından başka bir grup tarafından birlikte yük devreder. Bir kurtarma planı kullanarak bir uygulamanın nasıl modelleyeceğinizi öğrenmek için bkz. [kurtarma planları hakkında](recovery-plan-overview.md#model-apps).

### <a name="how-can-i-find-the-rto-of-a-recovery-plan"></a>Bir kurtarma planının RTO 'ı nasıl bulabilirim?

Kurtarma planının RTO 'ı denetlemek için kurtarma planı için bir yük devretme testi yapın ve **Site Recovery işler**' e gidin.
Aşağıdaki örnekte, **Saptestrecoveryplan**işi ' ne bakın. İş, tüm sanal makinelerin yükünü devretmek ve belirtilen eylemleri yapmak için 8 dakika ve 59 saniye sürdü.

![Site Recovery işlerin listesi](./media/azure-to-azure-troubleshoot-errors/recoveryplanrto.PNG)

### <a name="can-i-add-automation-runbooks-to-the-recovery-plan"></a>Kurtarma planına Otomasyon Runbook 'ları ekleyebilir miyim?

Evet, Azure Otomasyonu runbook 'larını kurtarma planınızla tümleştirebilirsiniz. [Azure Otomasyonu runbook 'ları ekleme](site-recovery-runbook-automation.md)hakkında daha fazla bilgi edinin.

## <a name="reprotection-and-failback"></a>Yeniden koruma ve yeniden çalışma

### <a name="i-failed-over-from-the-primary-region-to-a-disaster-recovery-region-are-vms-in-a-dr-region-protected-automatically"></a>Birincil bölgeden bir olağanüstü durum kurtarma bölgesine yük devretdim. Bir DR bölgesindeki VM 'Ler otomatik olarak korunuyor mu?

Hayır. Azure VM 'lerinden bir bölgeden diğerine [Yük devretmek](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-tutorial-failover-failback) Için sanal makineler, korumasız BIR durumdaki Dr bölgesinde başlatılır. VM 'Leri birincil bölgeye yeniden devretmek için ikincil bölgedeki VM 'Leri [yeniden korumanız](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-how-to-reprotect) gerekir.

### <a name="at-the-time-of-reprotection-does-site-recovery-replicate-complete-data-from-the-secondary-region-to-the-primary-region"></a>Yeniden koruma sırasında, tüm verileri ikincil bölgeden birincil bölgeye çoğaltarak Site Recovery.

Duruma bağlıdır. Kaynak bölgesi VM 'si varsa, yalnızca kaynak disk ve hedef disk arasındaki değişiklikler eşitlenir. Site Recovery, diskleri karşılaştırarak farklıları hesaplar ve ardından verileri aktarır. Bu işlem genellikle birkaç saat sürer. Yeniden koruma sırasında ne olacağı hakkında daha fazla bilgi için bkz. [Azure VM örnekleri yük devretmesinin birincil bölgeye yeniden koruma](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-how-to-reprotect#what-happens-during-reprotection).

### <a name="how-much-time-does-it-take-to-fail-back"></a>Yeniden başarısızlık süresi ne kadar sürer?

Yeniden çalışma sonrasında, geri dönme, birincil bölgeden ikincil bir bölgeye yük devretmek için geçen süreyi de alır.

## <a name="capacity"></a>Kü

### <a name="how-is-capacity-ensured-in-the-target-region-for-azure-vms"></a>Azure VM 'Leri için hedef bölgede kapasite nasıl belirlenir?

Site Recovery Team ve Azure kapasite yönetimi ekibi, yeterli altyapı kapasitesi için plan yapın. Yük devretme başlattığınızda takımlar, Site Recovery tarafından korunan sanal makine örneklerinin hedef bölgeye dağıtılmasını sağlamaya de yardımcı olur.

### <a name="does-site-recovery-work-with-reserved-instances"></a>Site Recovery ayrılmış örneklerle birlikte çalışır mı?

Evet, olağanüstü durum kurtarma bölgesinde [ayrılmış Azure VM 'leri](https://azure.microsoft.com/pricing/reserved-vm-instances/) satın alabilir ve Site Recovery yük devretme işlemleri bunları kullanır. Ek bir yapılandırma gerekmez.

## <a name="security"></a>Güvenlik

### <a name="is-replication-data-sent-to-the-site-recovery-service"></a>Çoğaltılan veriler Site Recovery hizmetine gönderilir mi?

Hayır, Site Recovery çoğaltılan verileri engellemez ve sanal makinelerinizdeki neleri çalıştırdıklarınız hakkında bilgi içermez. Yalnızca çoğaltma ve yük devretme işlemlerini düzenlemek için gereken meta veriler Site Recovery hizmetine gönderilir.  

Site Recovery ISO 27001:2013, 27018, HIPAA ve DPA sertifikalı. Hizmet SOC2 ve Fedrampa JAB değerlendirmelerinde.

### <a name="does-site-recovery-encrypt-replication"></a>Site Recovery çoğaltma işlemini şifreleyebilir mi?

Evet, hem geçiş hem de [Azure 'da bekleyen şifreleme](https://docs.microsoft.com/azure/storage/storage-service-encryption) destekleniyor.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure 'Dan Azure 'a destek gereksinimlerini gözden geçirin](azure-to-azure-support-matrix.md).
- [Azure 'Dan Azure 'a çoğaltma ayarlayın](azure-to-azure-tutorial-enable-replication.md).
- Bu makaleyi okuduktan sonra sorularınız varsa, bunları [Azure kurtarma hizmetleri forumuna](https://social.msdn.microsoft.com/Forums/azure/home?forum=hypervrecovmgr)gönderin.
