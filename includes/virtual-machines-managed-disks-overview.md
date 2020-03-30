---
title: include dosyası
description: include dosyası
services: virtual-machines
author: roygara
ms.service: virtual-machines
ms.topic: include
ms.date: 11/06/2019
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: 72b6c1f86b18df172994827ec78eb109fe82454e
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "75913767"
---
## <a name="benefits-of-managed-disks"></a>Yönetilen disklerin yararları

Yönetilen diskleri kullanarak elde ettiğiniz bazı avantajların üzerinden geçelim.

### <a name="highly-durable-and-available"></a>Son derece dayanıklı ve kullanılabilir

Yönetilen diskler %99,999 kullanılabilirlik için tasarlanmıştır. Yönetilen diskler, verilerinizin üç kopyasını sağlayarak bunu başarır ve yüksek dayanıklılık sağlar. Bir veya iki yineleme de sorun yaşarsa, kalan yinelemeler verilerinizin kalıcıolmasını ve hatalara karşı yüksek tolerans sağlamaya yardımcı olur. Bu mimari, Azure'un sektör lideri siyatlı yıllık hata oranıyla hizmet (IaaS) diskleri olarak altyapı için kurumsal kalitede dayanıklılık sağlamasına sürekli olarak yardımcı olmuştur.

### <a name="simple-and-scalable-vm-deployment"></a>Basit ve ölçeklenebilir VM dağıtımı

Yönetilen **diskleri** kullanarak, tek bir abonelikte binlerce VM oluşturmanıza olanak tanıyan, bölge başına abonelikte bir türde en fazla 50.000 **VM** disk oluşturabilirsiniz. Bu özellik ayrıca, Bir Market görüntüsü kullanarak sanal makine ölçeğinde 1.000 VM'ye kadar oluşturmanıza izin vererek [sanal makine ölçek kümelerinin](../articles/virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md) ölçeklenebilirliğini daha da artırır.

### <a name="integration-with-availability-sets"></a>Kullanılabilirlik kümeleriyle entegrasyon

Yönetilen diskler, [kullanılabilirlik kümesindeki VM](../articles/virtual-machines/windows/manage-availability.md#use-managed-disks-for-vms-in-an-availability-set) disklerinin tek bir hata noktasını önlemek için birbirinden yeterince yalıtılmış olmasını sağlamak için kullanılabilirlik kümeleriyle tümleştirilir. Diskler otomatik olarak farklı depolama ölçek birimlerine (pullar) yerleştirilir. Bir damga donanım veya yazılım hatası nedeniyle başarısız olursa, yalnızca bu pullarda diskleri olan VM örnekleri başarısız olur. Örneğin, beş VM üzerinde çalışan bir uygulamanız olduğunu ve VM'lerin kullanılabilirlik kümesinde olduğunu varsayalım. Bu VM'lerin disklerinin tümü aynı damgada depolanmayacak, bu nedenle bir damga düşerse, uygulamanın diğer örnekleri çalışmaya devam eder.

### <a name="integration-with-availability-zones"></a>Kullanılabilirlik Bölgeleri ile Entegrasyon

Yönetilen diskler, uygulamalarınızı veri merkezi hatalarından koruyan yüksek kullanılabilirlik sunan [Kullanılabilirlik Bölgelerini](../articles/availability-zones/az-overview.md)destekler. Kullanılabilirlik Alanları, Azure bölgesi içinde fiziksel olarak benzersiz konumlardır. Her alan bağımsız güç, soğutma ve ağ bağlantısı ile donatılmış bir veya daha fazla veri merkezinden oluşur. Esnekliği sağlamak için, tüm etkin bölgelerde en az üç ayrı bölge vardır. Kullanılabilirlik Bölgeleri ile Azure, endüstrinin en iyi %99,99'luk VM çalışma süresi SLA'sını sunar.

### <a name="azure-backup-support"></a>Azure Yedekleme desteği

[Azure Yedekleme,](../articles/backup/backup-overview.md) bölgesel felaketlere karşı korunmak için zamantabanlı yedeklemeler ve yedekleme bekletme ilkeleri içeren bir yedekleme işi oluşturmak için kullanılabilir. Bu, istediğiniz zaman kolay VM restorasyonları yapmanızı sağlar. Şu anda Azure Yedekleme, dört tebibayta kadar disk boyutlarını destekler.  Azure Yedekleme, yönetilen disklerin yedeklenmesini ve geri yüklenmesini destekler. Azure VM yedekleme desteği hakkında [daha fazla bilgi edinin.](../articles/backup/backup-support-matrix-iaas.md)

### <a name="granular-access-control"></a>Taneli erişim kontrolü

Yönetilen bir disk için belirli izinleri bir veya daha fazla kullanıcıya atamak için [Azure rol tabanlı erişim denetimini (RBAC)](../articles/role-based-access-control/overview.md) kullanabilirsiniz. Yönetilen diskler, disk için okuma, yazma (oluşturma/güncelleştirme), silme ve paylaşılan [erişim imzası (SAS) URI'yi](../articles/storage/common/storage-dotnet-shared-access-signature-part-1.md) alma gibi çeşitli işlemleri ortaya çıkarır. Yalnızca bir kişinin işini gerçekleştirmek için ihtiyaç duyduğu işlemlere erişim izni verebilirsiniz. Örneğin, bir kişinin yönetilen bir diski depolama hesabına kopyalamasını istemiyorsanız, yönetilen disk için dışa aktarma eylemine erişim vermemeyi seçebilirsiniz. Benzer şekilde, bir kişinin yönetilen bir diski kopyalamak için SAS URI kullanmasını istemiyorsanız, yönetilen diske bu izni vermemeyi seçebilirsiniz.

### <a name="upload-your-vhd"></a>VHD'nizi yükleyin

 Doğrudan yükleme, vhd'nizi Azure yönetilen bir diske aktarmanızı kolaylaştırır. Daha önce, verilerinizi bir depolama hesabında düzenlemeyi içeren daha ilgili bir işlemi izlemeniz gerekiyordu. Şimdi, daha az adım var. Şirket içi VM'leri Azure'a yüklemek, büyük yönetilen disklere yüklemek daha kolaydır ve yedekleme ve geri yükleme işlemi basitleştirilir. Ayrıca, verileri doğrudan yönetilen disklere VM'lere eklemeden yüklemenize izin vererek maliyeti azaltır. 32 TiB boyutuna kadar vhds yüklemek için doğrudan yükleme kullanabilirsiniz.

 vHD'nizi Azure'a nasıl aktarırış edebilirsiniz öğrenmek için [CLI](../articles/virtual-machines/linux/disks-upload-vhd-to-managed-disk-cli.md) veya [PowerShell](../articles/virtual-machines/windows/disks-upload-vhd-to-managed-disk-powershell.md) makalelerine bakın.

## <a name="encryption"></a>Şifreleme

Yönetilen diskler iki farklı şifreleme türü sunar. Bunlardan ilki, depolama hizmeti tarafından gerçekleştirilen Server Side Encryption (SSE) tir. İkincisi, SANAL'larınız için işletim sistemi ve veri disklerinde etkinleştirebileceğiniz Azure Disk Şifrelemesi (ADE).

### <a name="server-side-encryption"></a>Sunucu tarafı şifrelemesi

[Azure Sunucu tarafı Şifreleme,](../articles/virtual-machines/windows/disk-encryption.md) kuruluş güvenliği ve uyumluluk taahhütlerinizi karşılamak için verilerinizi sağlar ve verilerinizi korur. Yönetilen disklerin kullanılabildiği tüm bölgelerdeki tüm yönetilen diskler, anlık görüntüler ve görüntüler için varsayılan olarak sunucu tarafı şifrelemesi etkinleştirilir. Azure'un anahtarlarınızı sizin için yönetmesine izin verebilirsiniz, bunlar platform tarafından yönetilen anahtarlardır veya anahtarları kendiniz yönetebilirsiniz, bunlar müşteri tarafından yönetilen anahtarlardır. Daha fazla bilgi için [Yönetilen Diskler SSS sayfasını](../articles/virtual-machines/windows/faq-for-disks.md#managed-disks-and-storage-service-encryption) ziyaret edin.

### <a name="azure-disk-encryption"></a>Azure Disk Şifrelemesi

Azure Disk Şifreleme, IaaS Virtual Machine tarafından kullanılan işletim sistemi ve Veri disklerini şifrelemenize olanak tanır. Bu şifreleme yönetilen diskleri içerir. Windows için sürücüler, endüstri standardı BitLocker şifreleme teknolojisi kullanılarak şifrelenir. Linux için diskler DM-Crypt teknolojisi kullanılarak şifrelenir. Şifreleme işlemi, disk şifreleme anahtarlarını denetlemenize ve yönetmenize olanak tanımak için Azure Key Vault ile tümleşiktir. Daha fazla bilgi için [IaaS VM'ler için Azure Disk Şifreleme'ye](../articles/security/azure-security-disk-encryption-overview.md)bakın.

## <a name="disk-roles"></a>Disk rolleri

Azure'da üç ana disk rolü vardır: veri diski, işletim sistemi ve geçici disk. Bu roller, sanal makinenize bağlı disklerle eşlenir.

![Disk rolleri iş başında](media/virtual-machines-managed-disks-overview/disk-types.png)

### <a name="data-disk"></a>Veri diski

Veri diski, uygulama verilerini veya tutmanız gereken diğer verileri depolamak için sanal bir makineye bağlı yönetilen bir disktir. Veri diskleri SCSI sürücüleri olarak kaydedilir ve seçtiğiniz bir harfle etiketlenir. Her veri diski maksimum 32.767 adet türbayt (GİB) kapasitesine sahiptir. Sanal makinenin boyutu, ona kaç veri diski ekebileceğinizi ve diskleri barındırmak için kullanabileceğiniz depolama türünü belirler.

### <a name="os-disk"></a>İşletim sistemi diski

Her sanal makinenin bağlı bir işletim sistemi diski vardır. Bu işletim sistemi diski, VM oluşturulduğunda seçilen önceden yüklenmiş bir işletim sistemi vardır. Bu disk önyükleme hacmini içerir.

Bu disk maksimum 2.048 GiB kapasiteye sahiptir.

### <a name="temporary-disk"></a>Geçici disk

Her VM, yönetilen bir disk olmayan geçici bir disk içerir. Geçici disk, uygulamalar ve işlemler için kısa süreli depolama sağlar ve yalnızca sayfa veya takas dosyaları gibi verileri depolamak için tasarlanmıştır. Geçici diskteki veriler bir [bakım olayı](../articles/virtual-machines/windows/manage-availability.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json#understand-vm-reboots---maintenance-vs-downtime) sırasında veya [bir VM'yi yeniden dağıttığınızda](../articles/virtual-machines/troubleshooting/redeploy-to-new-node-windows.md?toc=%2Fazure%2Fvirtual-machines%2Fwindows%2Ftoc.json)kaybolabilir. Azure Linux VM'lerinde geçici disk varsayılan olarak /dev/sdb ve Windows VM'lerde geçici disk D'dir: varsayılan olarak. VM'nin başarılı bir standart yeniden başlatması sırasında, geçici diskteki veriler devam edecektir.

## <a name="managed-disk-snapshots"></a>Yönetilen disk anlık görüntüleri

Yönetilen disk anlık görüntüsü, varsayılan olarak standart yönetilen disk olarak depolanan yönetilen bir diskin salt okunur kilitlenme tutarlı tam kopyasıdır. Anlık görüntülerle, yönetilen disklerinizi istediğiniz zaman yedekleyebilirsiniz. Bu anlık görüntüler kaynak diskten bağımsız olarak bulunur ve yeni yönetilen diskler oluşturmak için kullanılabilir. 

Anlık görüntüler, kullanılan boyuta göre faturalandırılır. Örneğin, 64 GiB ve 10 GiB fiili kullanılan veri boyutu ile yönetilen bir disk anlık görüntü oluşturursanız, bu anlık görüntü yalnızca kullanılan veri boyutu için faturalandırılır 10 GiB. [Azure kullanım raporuna](https://docs.microsoft.com/azure/billing/billing-understand-your-bill)bakarak anlık görüntülerinizin kullanılan boyutunu görebilirsiniz. Örneğin, anlık görüntünün kullanılan veri boyutu 10 GiB ise, **günlük** kullanım raporu tüketilen miktar olarak 10 GiB/(31 gün) = 0,3226 gösterir.

Yönetilen diskler için anlık görüntü oluşturma hakkında daha fazla bilgi edinmek için aşağıdaki kaynaklara bakın:

* [Windows'da yönetilen bir diskin anlık görüntüsünü oluşturma](../articles/virtual-machines/windows/snapshot-copy-managed-disk.md)
* [Linux'ta yönetilen bir diskin anlık görüntüsünü oluşturma](../articles/virtual-machines/linux/snapshot-copy-managed-disk.md)

### <a name="images"></a>Görüntüler

Yönetilen diskler de yönetilen özel bir görüntü oluşturmayı destekler. Özel VHD'nizden bir depolama hesabında niçin veya doğrudan genelleştirilmiş (sysprepped) bir VM'den görüntü oluşturabilirsiniz. Bu işlem tek bir görüntü yakalar. Bu resim, hem işletim sistemi hem de veri diskleri de dahil olmak üzere bir VM ile ilişkili tüm yönetilen diskleri içerir. Bu yönetilen özel görüntü, herhangi bir depolama hesabını kopyalamaya veya yönetmeye gerek kalmadan özel resminizi kullanarak yüzlerce VM oluşturulmasını sağlar.

Resim oluşturma hakkında daha fazla bilgi için aşağıdaki makalelere bakın:

* [Azure'da genelleştirilmiş bir VM'nin yönetilen görüntüsünü nasıl yakalarım?](../articles/virtual-machines/windows/capture-image-resource.md)
* [Azure CLI'yi kullanarak Linux sanal makinelerini genelleştirme ve yakalama](../articles/virtual-machines/linux/capture-image.md)

#### <a name="images-versus-snapshots"></a>Görüntüler ve anlık görüntüler

Görüntüler ve anlık görüntüler arasındaki farkı anlamak önemlidir. Yönetilen disklerle, ayrılmış genelleştirilmiş bir VM'nin görüntüsünü alabilirsiniz. Bu resim, VM'ye bağlı tüm diskleri içerir. Bir VM oluşturmak için bu görüntüyü kullanabilirsiniz ve tüm diskleri içerir.

Anlık görüntü, anlık görüntünün alındığı anda bir diskin kopyasıdır. Yalnızca bir disk için geçerlidir. Bir diski (işletim sistemi diski) olan bir VM'iniz varsa, anlık görüntü veya görüntüsünü alabilir ve anlık görüntüden veya görüntüden bir VM oluşturabilirsiniz.

Anlık görüntü, içerdiği disk dışında herhangi bir diskhakkında farkındalığa sahip değildir. Bu, şeritleme gibi birden çok diskin koordinasyonunu gerektiren senaryolarda kullanılmasını sorunlu hale getirir. Anlık görüntülerin birbiriyle eşgüdüm içinde hareket edebilmesi gerekir ve bu şu anda desteklenmez.

## <a name="disk-allocation-and-performance"></a>Disk ayırma ve performans

Aşağıdaki diyagram, üç düzeyli bir sağlama sistemi kullanarak diskler için bant genişliği ve IOPS'nin gerçek zamanlı tahsisini gösterir:

![Bant genişliği ve IOPS tahsisini gösteren üç düzeyli sağlama sistemi](media/virtual-machines-managed-disks-overview/real-time-disk-allocation.png)

İlk düzey sağlama, disk başına IOPS ve bant genişliği atamasını ayarlar.  İkinci düzeyde, bilgi işlem sunucusu ana bilgisayar, yalnızca sunucunun SSD'sinde depolanan ve önbelleğe alınmış (ReadWrite ve ReadOnly) disklerin yanı sıra yerel ve geçici diskleri içeren verilere uygulayarak SSD sağlama uygular. Son olarak, İşlem ana bilgisayarın Azure Depolama'nın arka ucuna gönderdiği tüm G/Ç için VM ağ sağlama üçüncü düzeyinde gerçekleşir. Bu şema ile, VM'nin performansı, VM'nin yerel SSD'yi nasıl kullandığından, bağlı disk sayısına ve iliştirdiği disklerin performansı ve önbelleğe alma türüne kadar çeşitli etkenlere bağlıdır.

Bu sınırlamalara örnek olarak, Bir Standard_DS1v1 VM'nin SSD ve ağ düzeylerindeki sınırlamalar nedeniyle önbelleğe alınmış olsun veya olmasın, bir P30 diskin 5.000 IOPS potansiyeline ulaşması engellenir:

![Standard_DS1v1 örnek ayırma](media/virtual-machines-managed-disks-overview/example-vm-allocation.png)

Azure, ağ trafiğinin diğer düşük önceliğine göre öncelik alan disk trafiği için öncelikli ağ kanalı kullanır. Bu, ağ çekişmeleri durumunda disklerin beklenen performansını korumalarına yardımcı olur. Benzer şekilde, Azure Depolama da otomatik yük dengeleme ile arka plandaki kaynak çekişmelerini ve diğer sorunları işler. Azure Depolama, bir disk oluşturduğunuzda gerekli kaynakları ayırır ve trafik düzeyini işlemek için kaynakların proaktif ve reaktif dengelemesini uygular. Bu, disklerin beklenen IOPS ve iş elde iş leri hedeflerini sürdürebilmesini sağlar. Gerektiğinde performans ve kurulum uyarılarını izlemek için VM düzeyi ve Disk düzeyi ölçümlerini kullanabilirsiniz.

İstediğiniz performansı elde etmek için VM + Disk yapılandırmalarını optimize etmek için en iyi uygulamaları öğrenmek [için yüksek performanslı](../articles/virtual-machines/windows/premium-storage-performance.md) makale tasarımımıza bakın

## <a name="next-steps"></a>Sonraki adımlar

Yönetilen disklerde daha fazla ayrıntıya giren bir video istiyorsanız, şunlara göz atın: [Yönetilen Disklerle Daha İyi Azure VM Esnekliği.](https://channel9.msdn.com/Blogs/Azure/Managed-Disks-for-Azure-Resiliency)

Azure'un sunduğu tek tek disk türleri, gereksinimlerinize uygun olan tür hakkında daha fazla bilgi edinin ve disk türleri hakkındaki makalemizde performans hedefleri hakkında bilgi edinin.
