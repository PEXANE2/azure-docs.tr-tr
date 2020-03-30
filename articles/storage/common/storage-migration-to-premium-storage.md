---
title: VM'leri Azure Premium Depolamaalanına Geçirme | Microsoft Dokümanlar
description: Mevcut VM'lerinizi Azure Premium Depolama alanına taşıyın. Premium Depolama, Azure Sanal Makinelerde çalışan Yoğun I/İş yoğun iş yükleri için yüksek performanslı, düşük gecikmeli disk desteği sunar.
services: storage
author: roygara
ms.service: storage
ms.topic: article
ms.date: 06/27/2017
ms.author: rogarana
ms.reviewer: yuemlu
ms.subservice: common
ms.openlocfilehash: 7cb5a335af7093bc217578d57340b03b8b9c08b3
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75748337"
---
# <a name="migrating-to-azure-premium-storage-unmanaged-disks"></a>Azure Premium Depolamasına (Yönetilmeyen Diskler) geçiş

> [!NOTE]
> Bu makalede, yönetilmeyen standart diskleri yönetilmeyen premium diskler kullanan bir VM'ye nasıl geçirilir anlatılmaktadır. Yeni VM'ler için Azure Yönetilen Diskler kullanmanızı ve önceki yönetilmeyen disklerinizi yönetilen disklere dönüştürmenizi öneririz. Yönetilen Diskler, temel depolama hesaplarını işleyebilir, böylece bunu yapmak zorunda kalmamanızı sağlar. Daha fazla bilgi için lütfen [Yönetilen Disklere Genel Bakış'a](../../virtual-machines/windows/managed-disks-overview.md)bakın.
>

Azure Premium Depolama, Yoğun Yoğun Giş yükü çalıştıran sanal makineler için yüksek performanslı, düşük gecikmeli disk desteği sağlar. Uygulamanızın VM disklerini Azure Premium Depolama'ya geçirerek bu disklerin hızından ve performansından yararlanabilirsiniz.

Bu kılavuzun amacı, Azure Premium Depolama'nın yeni kullanıcılarının mevcut sistemlerinden Premium Depolama'ya sorunsuz bir geçiş yapmaya daha iyi hazırlanmalarına yardımcı olmaktır. Kılavuz, bu işlemdeki üç temel bileşene hitap eder:

* [Premium Depolamaya Geçiş Planı](#plan-the-migration-to-premium-storage)
* [Sanal Sabit Diskleri (VHD'ler) Premium Depolamaya Hazırlama ve Kopyalama](#prepare-and-copy-virtual-hard-disks-VHDs-to-premium-storage)
* [Premium Depolama'yı kullanarak Azure Sanal Makine Oluşturun](#create-azure-virtual-machine-using-premium-storage)

VM'leri diğer platformlardan Azure Premium Depolama'ya veya mevcut Azure VM'lerini Standart Depolama'dan Premium Depolama'ya geçirebilirsiniz. Bu kılavuz, her iki senaryo için adımları kapsar. Senaryonuza bağlı olarak ilgili bölümde belirtilen adımları izleyin.

> [!NOTE]
> Premium SSD'lerin bir özelliğe genel bakışı ve fiyatlandırması: [IaaS VM'ler için bir disk türü seçin.](../../virtual-machines/windows/disks-types.md#premium-ssd) Uygulamanız için en iyi performansı için yüksek IOPS gerektiren sanal makine diskini Azure Premium Depolama'ya geçirmenizi öneririz. Diskiniz yüksek IOPS gerektirmiyorsa, sanal makine disk verilerini SSD'ler yerine Sabit Disk Sürücüleri'nde (HDD) depolayan Standart Depolama'da saklayarak maliyetleri sınırlayabilirsiniz.
>

Geçiş işleminin bütünüyle tamamlanması, bu kılavuzda sağlanan adımlardan önce ve sonra ek eylemler gerektirebilir. Örnekler arasında sanal ağların veya uç noktaların yapılandırılması veya uygulamanızda bazı kapalı kalma süreleri gerektirebilecek kod değişiklikleri nin uygulamanın kendisi nde yapılması sayılabilir. Bu eylemler her uygulamaya özgüdür ve Premium Depolama'ya mümkün olduğunca sorunsuz bir şekilde geçiş yapmak için bu kılavuzda sağlanan adımlarla birlikte bunları tamamlamanız gerekir.

## <a name="plan-for-the-migration-to-premium-storage"></a><a name="plan-the-migration-to-premium-storage"></a>Premium Depolama'ya geçiş için plan
Bu bölüm, bu makaledeki geçiş adımlarını izlemeye hazır olduğunuzu ve VM ve Disk türleri hakkında en iyi kararı vermenize yardımcı olur.

### <a name="prerequisites"></a>Ön koşullar
* Bir Azure aboneliğiniz olmalıdır. Yoksa, bir aylık [ücretsiz deneme](https://azure.microsoft.com/pricing/free-trial/) aboneliği oluşturabilir veya daha fazla seçenek için [Azure Fiyatlandırması'nı](https://azure.microsoft.com/pricing/) ziyaret edebilirsiniz.
* PowerShell cmdlets'i çalıştırmak için Microsoft Azure PowerShell modülüne ihtiyacınız olacaktır. Yükleme noktası ve yükleme yönergeleri için bkz. [Azure PowerShell’i yükleme ve yapılandırma](/powershell/azure/overview).
* Premium Depolama'da çalışan Azure VM'leri kullanmayı planladığınızı, Premium Depolama özellikli VM'leri kullanmanız gerekir. Premium Depolama özellikli VM'ler ile hem Standart hem de Premium Depolama disklerini kullanabilirsiniz. Premium depolama diskleri gelecekte daha fazla VM türüyle kullanılabilir. Kullanılabilir tüm Azure VM disk türleri ve boyutları hakkında daha fazla bilgi için [sanal makineler için Boyutlar](../../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) ve [Bulut Hizmetleri Boyutları'na](../../cloud-services/cloud-services-sizes-specs.md)bakın.

### <a name="considerations"></a>Dikkat edilmesi gerekenler
Azure VM, uygulamalarınızın VM başına 256 TB'a kadar depolama alanına sahip olması için birkaç Premium Depolama diski eklemeyi destekler. Premium Depolama ile uygulamalarınız, okuma işlemleri için son derece düşük gecikmelerle VM başına 80.000 IOPS (saniyede giriş/çıkış işlemleri) ve VM başına saniyede 2000 MB disk üretimi elde edebilir. Çeşitli VM'ler ve Diskler seçenekleriniz vardır. Bu bölüm, iş yükünüze en uygun seçeneği bulmanıza yardımcı olmak içindir.

#### <a name="vm-sizes"></a>VM boyutları
Azure VM boyutu belirtimleri [sanal makineler için Boyutlar'da](../../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)listelenir. Premium Depolama ile çalışan sanal makinelerin performans özelliklerini gözden geçirin ve iş yükünüze en uygun VM boyutunu seçin. VM'nizde disk trafiğini yönlendirmek için yeterli bant genişliği olduğundan emin olun.

#### <a name="disk-sizes"></a>Disk boyutları
VM'inizle kullanılabilen beş tür disk vardır ve her birinin belirli IOP'leri ve iş elde edilme sınırları vardır. Kapasite, performans, ölçeklenebilirlik ve tepe yükleri açısından uygulamanızın ihtiyaçlarına göre VM'niz için disk türünü seçerken bu sınırları göz önünde bulundurun.

| Premium Diskler Türü  | P10   | P20   | P30            | P40            | P50            | 
|:-------------------:|:-----:|:-----:|:--------------:|:--------------:|:--------------:|
| Disk boyutu           | 128 GB| 512 GB| 1024 GB (1 TB) | 2048 GB (2 TB) | 4095 GB (4 TB) | 
| Disk başına IOPS       | 500   | 2300  | 5000           | 7.500           | 7.500           | 
| Disk başına aktarım hızı | Saniyede 100 MB | Saniyede 150 MB | Saniyede 200 MB | Saniyede 250 MB | Saniyede 250 MB |

İş yükünüze bağlı olarak, VM'iniz için ek veri disklerinin gerekli olup olmadığını belirleyin. VM'nize birkaç kalıcı veri diski ekleyebilirsiniz. Gerekirse, birimin kapasitesini ve performansını artırmak için diskler arasında şerit yapabilirsiniz. (Burada Disk Şeritleme [here](../../virtual-machines/windows/premium-storage-performance.md#disk-striping)nedir bakın.) [Depolama Alanları'nı][4]kullanarak Premium Depolama veri disklerini şeritlerseniz, kullanılan her disk için bir sütunla yapılandırmanız gerekir. Aksi takdirde, şeritlerli birimin genel performansı, diskler arasında trafiğin düzensiz dağılımı nedeniyle beklenenden düşük olabilir. Linux VMs için aynı ulaşmak için *mdadm* yardımcı programı kullanabilirsiniz. Ayrıntılar için [Linux'ta Yazılım RAID'i yapılandırın](../../virtual-machines/linux/configure-raid.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) makalesine bakın.

#### <a name="storage-account-scalability-targets"></a>Depolama hesabı ölçeklenebilirlik hedefleri

Premium Depolama hesapları aşağıdaki ölçeklenebilirlik hedeflerine sahiptir. Uygulama gereksinimleriniz tek bir depolama hesabının ölçeklenebilirlik hedeflerini aşıyorsa, uygulamanızı birden çok depolama hesabı kullanmak ve verilerinizi bu depolama hesapları arasında bölmek için oluşturun.

| Toplam Hesap Kapasitesi | Yerel Olarak Yedekli Depolama Hesabı için Toplam Bant Genişliği |
|:--- |:--- |
| Disk kapasitesi: 35 TB<br />Anlık görüntü kapasitesi: 10 TB |Gelen + Giden ler için saniyede 50 gigadir'e kadar |

Premium Depolama özellikleri hakkında daha fazla bilgi için, [premium sayfa blob depolama hesapları için Ölçeklenebilirlik hedeflerine](../blobs/scalability-targets-premium-page-blobs.md)bakın.

#### <a name="disk-caching-policy"></a>Disk önbelleğe alma ilkesi
Varsayılan olarak, disk önbelleğe alma ilkesi tüm Premium veri diskleri için *Salt Okunur* ve VM'ye bağlı Premium işletim sistemi diski için *Okuma-Yazma'dır.* Bu yapılandırma ayarı, uygulamanızın IO'ları için en iyi performansı elde etmek için önerilir. Yazma ağır veya yalnızca yazma veri diskleri (SQL Server günlük dosyaları gibi) için, daha iyi uygulama performansı elde edebilmek için disk önbelleği devre dışı kullanabilirsiniz. Varolan veri disklerinin önbellek ayarları Azure [portalı](https://portal.azure.com) veya *Set-AzureDataDisk* cmdlet'in *-HostCaching* parametresi kullanılarak güncellenebilir.

#### <a name="location"></a>Konum
Azure Premium Depolama'nın kullanılabildiği bir konum seçin. Kullanılabilir konumlarla ilgili güncel bilgiler için [Bölgeye göre Azure Hizmetleri'ne](https://azure.microsoft.com/regions/#services) bakın. Diskleri VM için depolayan Depolama hesabıyla aynı bölgede bulunan VM'ler, ayrı bölgelerde olup olmadıklarına göre çok daha iyi performans sağlar.

#### <a name="other-azure-vm-configuration-settings"></a>Diğer Azure VM yapılandırma ayarları
Azure VM oluştururken, belirli VM ayarlarını yapılandırmanız istenir. Unutmayın, VM'nin ömrü boyunca çok az ayar sabitlenirken, daha sonra başkalarını değiştirebilir veya ekleyebilirsiniz. Bu Azure VM yapılandırma ayarlarını gözden geçirin ve bunların iş yükü gereksinimlerinize uyacak şekilde uygun şekilde yapılandırıldığından emin olun.

### <a name="optimization"></a>İyileştirme
[Azure Premium Depolama: Yüksek Performans için Tasarım,](../../virtual-machines/windows/premium-storage-performance.md) Azure Premium Depolama'yı kullanarak yüksek performanslı uygulamalar oluşturmak için yönergeler sağlar. Uygulamanız tarafından kullanılan teknolojiler için geçerli olan en iyi performans uygulamalarıyla birlikte yönergeleri takip edebilirsiniz.

## <a name="prepare-and-copy-virtual-hard-disks-vhds-to-premium-storage"></a><a name="prepare-and-copy-virtual-hard-disks-VHDs-to-premium-storage"></a>Sanal sabit diskleri (VHD'ler) Premium Depolama'ya hazırlama ve kopyalama
Aşağıdaki bölümde VM'lerinizden VH'ler hazırlamak ve VHD'leri Azure Depolama alanına kopyalamak için yönergeler verilmektedir.

* [Senaryo 1: "Mevcut Azure VM'lerini Azure Premium Depolama alanına aktarıyorum."](#scenario1)
* [Senaryo 2: "VM'leri diğer platformlardan Azure Premium Depolama'ya aktarıyorum."](#scenario2)

### <a name="prerequisites"></a>Ön koşullar
VHD'leri geçişe hazırlamak için şunları yapmanız gerekir:

* Azure aboneliği, depolama hesabı ve VHD'nizi kopyalayabildiğiniz bu depolama hesabındaki bir kapsayıcı. Hedef depolama hesabının gereksiniminize bağlı olarak standart veya Premium Depolama hesabı olabileceğini unutmayın.
* Birden çok VM örneği oluşturmayı planlıyorsanız VHD'yi genelleştirmek için bir araçtır. Örneğin, Windows için sysprep veya Ubuntu için virt-sysprep.
* VHD dosyasını Depolama hesabına yüklemek için bir araç. [Bkz. AzCopy Command-Line Utility ile veri aktarımı](storage-use-azcopy.md) veya bir [Azure depolama gezgini](https://blogs.msdn.com/b/windowsazurestorage/archive/2014/03/11/windows-azure-storage-explorers-2014.aspx)kullanın. Bu kılavuzda, AzCopy aracını kullanarak VHD'nizi kopyalamanız açıklanmaktadır.

> [!NOTE]
> AzCopy ile senkron kopyalama seçeneğini seçerseniz, en iyi performans için, hedef depolama hesabıyla aynı bölgede bulunan bir Azure VM'den bu araçlardan birini çalıştırarak VHD'nizi kopyalayın. Bir VHD'yi farklı bir bölgedeki Bir Azure VM'den kopyalıyorsanız, performansınız daha yavaş olabilir.
>
> Sınırlı bant genişliği üzerinden büyük miktarda veri kopyalamak [için, verileri Blob Depolama'ya aktarmak için Azure İçe Alma/Dışa Aktarma hizmetini kullanmayı](../storage-import-export-service.md)düşünün; bu, sabit disk sürücülerini bir Azure veri merkezine göndererek verilerinizi aktarmanızı sağlar. Verileri yalnızca standart bir depolama hesabına kopyalamak için Azure İçe Alma/Dışa Aktarma hizmetini kullanabilirsiniz. Veriler standart depolama hesabınıza ulaştıktan sonra, verileri premium depolama hesabınıza aktarmak için [Kopya Blob API'sını](https://msdn.microsoft.com/library/azure/dd894037.aspx) veya AzCopy'yi kullanabilirsiniz.
>
> Microsoft Azure'un yalnızca sabit boyutlu VHD dosyalarını desteklediğini unutmayın. VHDX dosyaları veya dinamik VHDD'ler desteklenmez. Dinamik bir VHD'niz varsa, [Convert-VHD](https://technet.microsoft.com/library/hh848454.aspx) cmdlet'i kullanarak sabit boyuta dönüştürebilirsiniz.
>
>

### <a name="scenario-1-i-am-migrating-existing-azure-vms-to-azure-premium-storage"></a><a name="scenario1"></a>Senaryo 1: "Mevcut Azure VM'lerini Azure Premium Depolama alanına aktarıyorum."
Varolan Azure VM'lerini geçiriyorsanız, VM'yi durdurun, istediğiniz VHD türüne göre VHD'ler hazırlayın ve VHD'yi AzCopy veya PowerShell ile kopyalayın.

VM'nin temiz bir duruma geçiş yapması için tamamen aşağı olması gerekir. Geçiş tamamlanana kadar bir kesinti olacaktır.

#### <a name="step-1-prepare-vhds-for-migration"></a>1. Adım. VHD'leri geçiş için hazırlama
Mevcut Azure VM'lerini Premium Depolama alanına geçiriyorsanız, VHD'niz şu olabilir:

* Genelleştirilmiş bir işletim sistemi görüntüsü
* Benzersiz bir işletim sistemi diski
* Bir veri diski

Aşağıda VHD hazırlamak için bu 3 senaryolar üzerinden yürümek.

##### <a name="use-a-generalized-operating-system-vhd-to-create-multiple-vm-instances"></a>Birden çok VM örneği oluşturmak için genelleştirilmiş bir İşletim Sistemi VHD kullanın
Birden çok genel Azure VM örneği oluşturmak için kullanılacak bir VHD yüklüyorsanız, öncelikle bir sysprep yardımcı programı kullanarak VHD'yi genellemeniz gerekir. Bu, şirket içinde veya bulutta bulunan bir VHD için geçerlidir. Sysprep, VHD'den makineye özgü tüm bilgileri kaldırır.

> [!IMPORTANT]
> Genelleme yapmadan önce vm'nizi anlık görüntü olarak alın veya yedekleştirin. Çalışan sysprep duracak ve VM örneğini bulmak. Windows OS VHD'yi sysprep için aşağıdaki adımları izleyin. Sysprep komutunu çalıştırmanın sanal makineyi kapatmanızı gerektireceğini unutmayın. Sysprep hakkında daha fazla bilgi için [Sysprep Genel Bakış](https://technet.microsoft.com/library/hh825209.aspx) veya [Sysprep Teknik Referans'a](https://technet.microsoft.com/library/cc766049.aspx)bakın.
>
>

1. Yönetici olarak Komut İstemi pencerelerini açın.
2. Sysprep'i açmak için aşağıdaki komutu girin:

    ```
    %windir%\system32\sysprep\sysprep.exe
    ```

3. Sistem Hazırlama Aracında, Kutudan Kullanıma Sun Deneyimi Girin'i (OOBE) seçin, Genelle'yi onay kutusunu seçin, **Kapatma'yı**seçin ve aşağıdaki resimde gösterildiği gibi **Tamam'ı**tıklatın. Sysprep işletim sistemini genelleştirecek ve sistemi kapatacaktır.

    ![][1]

Bir Ubuntu VM için, aynı elde etmek için virt-sysprep kullanın. Daha fazla bilgi için [virt-sysprep](https://manpages.ubuntu.com/manpages/precise/man1/virt-sysprep.1.html) bakın. Diğer Linux işletim sistemleri için açık kaynak Koda sahip [Linux Server Provisioning yazılımlarından](https://www.cyberciti.biz/tips/server-provisioning-software.html) bazılarına da bakın.

##### <a name="use-a-unique-operating-system-vhd-to-create-a-single-vm-instance"></a>Tek bir VM örneği oluşturmak için benzersiz bir İşletim Sistemi VHD kullanın
VM üzerinde çalışan ve makineye özgü veri gerektiren bir uygulamanız varsa, VHD'yi genellemayın. Özgün bir Azure VM örneği oluşturmak için genelleştirilmeyen bir VHD kullanılabilir. Örneğin, VHD'nizde Etki Alanı Denetleyicisi varsa, sysprep'i yürütmek etki alanı denetleyicisi olarak etkisiz hale getirecektir. VM'nizde çalışan uygulamaları ve VHD'yi genellemeden önce sysprep çalıştırmanın bunlar üzerindeki etkisini gözden geçirin.

##### <a name="register-data-disk-vhd"></a>Veri diski VHD'yi kaydet
Azure'da geçirilecek veri diskiniz varsa, bu veri disklerini kullanan Sanal Kullanım Aygıtlarının kapatıldığından emin olmalısınız.

VHD'yi Azure Premium Depolama alanına kopyalamak ve sağlanan bir veri diski olarak kaydetmek için aşağıda açıklanan adımları izleyin.

#### <a name="step-2-create-the-destination-for-your-vhd"></a>2. Adım VHD'niz için hedef oluşturma
VHD'lerinizi korumak için bir depolama hesabı oluşturun. VHD'lerinizi nerede depolayacağınızda aşağıdaki noktaları göz önünde bulundurun:

* Hedef Premium depolama hesabı.
* Depolama hesabı konumu, son aşamada oluşturacağınız Premium Depolama özellikli Azure VM'lerle aynı olmalıdır. Yeni bir depolama hesabına kopyalayabilir veya gereksinimlerinize göre aynı depolama hesabını kullanmayı planlayabilirsiniz.
* Hedef depolama hesabının depolama hesabı anahtarını bir sonraki aşama için kopyalayın ve kaydedin.

Veri diskleri için, bazı veri disklerini standart bir depolama hesabında tutmayı seçebilirsiniz (örneğin, daha serin depolama alanına sahip diskler), ancak premium depolamayı kullanmak için üretim iş yükü için tüm verileri taşımanızı şiddetle öneririz.

#### <a name="step-3-copy-vhd-with-azcopy-or-powershell"></a><a name="copy-vhd-with-azcopy-or-powershell"></a>3. Adım. AzCopy veya PowerShell ile VHD kopyalayın
Bu iki seçeneklerden birini işlemek için konteyner yolunuzu ve depolama hesabı anahtarınızı bulmanız gerekir. Konteyner yolu ve depolama hesabı anahtarı **Azure Portal** > **Depolama'da**bulunabilir. Konteyner URL'si "https:\//myaccount.blob.core.windows.net/mycontainer/" gibi olacaktır.

##### <a name="option-1-copy-a-vhd-with-azcopy-asynchronous-copy"></a>Seçenek 1: AzCopy ile bir VHD kopyalayın (Asynchronous copy)

AzCopy'yi kullanarak VHD'yi Internet üzerinden kolayca yükleyebilirsiniz. VHD'lerin boyutuna bağlı olarak, bu işlem zaman alabilir. Bu seçeneği kullanırken depolama hesabı giriş/çıkış sınırlarını kontrol etmeyi unutmayın. Ayrıntılar [için standart depolama hesapları için ölçeklenebilirlik ve performans hedeflerine](scalability-targets-standard-account.md) bakın.

1. İndirin ve burada azcopy [yükleyin: AzCopy en son sürümü](https://aka.ms/downloadazcopy)
2. Azure PowerShell'i açın ve AzCopy'nin yüklü olduğu klasöre gidin.
3. VHD dosyasını "Kaynak"tan "Hedef"e kopyalamak için aşağıdaki komutu kullanın.

   ```azcopy
   AzCopy /Source: <source> /SourceKey: <source-account-key> /Dest: <destination> /DestKey: <dest-account-key> /BlobType:page /Pattern: <file-name>
   ```

    Örnek:

    ```azcopy
    AzCopy /Source:https://sourceaccount.blob.core.windows.net/mycontainer1 /SourceKey:key1 /Dest:https://destaccount.blob.core.windows.net/mycontainer2 /DestKey:key2 /Pattern:abc.vhd
    ```
 
   Aşağıdana komutu kullanılan parametrelerin açıklamaları şunlardır:

   * **/Source:** _ &lt;&gt;kaynak :_ VHD içeren klasörün veya depolama kapsayıcısının URL'sinin konumu.
   * **/SourceKey:** _ &lt;kaynak-hesap&gt;anahtarı :_ Kaynak depolama hesabının depolama hesabı anahtarı.
   * **/Dest:** _ &lt;&gt;hedef :_ VHD'yi kopyalamak için depolama kapsayıcısı URL'si.
   * **/DestKey:** _ &lt;dest-account-key&gt;:_ Hedef depolama hesabının depolama hesabı anahtarı.
   * **/Pattern:** _ &lt;dosya&gt;adı :_ Kopyalamak için VHD'nin dosya adını belirtin.

AzCopy aracını kullanma hakkında daha fazla bilgi için, [AzCopy Command-Line Utility ile veri aktarımı'na](storage-use-azcopy.md)bakın.

##### <a name="option-2-copy-a-vhd-with-powershell-synchronized-copy"></a>Seçenek 2: PowerShell ile Bir VHD kopyalayın (Senkronize kopya)

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

PowerShell cmdlet Start-AzStorageBlobCopy'i kullanarak VHD dosyasını da kopyalayabilirsiniz. VHD'yi kopyalamak için Azure PowerShell'de aşağıdaki komutu kullanın.  <> 'daki değerleri kaynak ve hedef depolama hesabınızdaki karşılık gelen değerlerle değiştirin. Bu komutu kullanmak için, hedef depolama hesabınızda vhds adında bir kapsayıcı olması gerekir. Kapsayıcı yoksa, komutu çalıştırmadan önce bir tane oluşturun.

```powershell
$sourceBlobUri = <source-vhd-uri>

$sourceContext = New-AzStorageContext  –StorageAccountName <source-account> -StorageAccountKey <source-account-key>

$destinationContext = New-AzStorageContext  –StorageAccountName <dest-account> -StorageAccountKey <dest-account-key>

Start-AzStorageBlobCopy -srcUri $sourceBlobUri -SrcContext $sourceContext -DestContainer <dest-container> -DestBlob <dest-disk-name> -DestContext $destinationContext
```

Örnek:

```powershell
C:\PS> $sourceBlobUri = "https://sourceaccount.blob.core.windows.net/vhds/myvhd.vhd"

C:\PS> $sourceContext = New-AzStorageContext  –StorageAccountName "sourceaccount" -StorageAccountKey "J4zUI9T5b8gvHohkiRg"

C:\PS> $destinationContext = New-AzStorageContext  –StorageAccountName "destaccount" -StorageAccountKey "XZTmqSGKUYFSh7zB5"

C:\PS> Start-AzStorageBlobCopy -srcUri $sourceBlobUri -SrcContext $sourceContext -DestContainer "vhds" -DestBlob "myvhd.vhd" -DestContext $destinationContext
```

### <a name="scenario-2-i-am-migrating-vms-from-other-platforms-to-azure-premium-storage"></a><a name="scenario2"></a>Senaryo 2: "VM'leri diğer platformlardan Azure Premium Depolama'ya aktarıyorum."
VHD'yi Azure olmayan Bulut Depolama'dan Azure'a taşıyorsanız, öncelikle VHD'yi yerel bir dizine dışa aktarmanız gerekir. VHD'nin kullanışlı olarak depolandığı yerel dizinin tam kaynak yolunu ve ardından Azure Depolama'ya yüklemek için AzCopy'yi kullanarak sahip olun.

#### <a name="step-1-export-vhd-to-a-local-directory"></a>1. Adım. VHD'yi yerel bir dizine aktarma
##### <a name="copy-a-vhd-from-aws"></a>AWS'den bir VHD kopyalama
1. AWS kullanıyorsanız, EC2 örneğini Bir Amazon S3 kovasında bir VHD'ye dışa aktarın. Amazon EC2 komut satırı arabirimi (CLI) aracını yüklemek için Amazon EC2 Örnekleri Nin İhracatı için Amazon belgelerinde açıklanan adımları izleyin ve EC2 örneğini VHD dosyasına aktarmak için oluşturma örneği-verme görev komutunu çalıştırın. **Create-instance-export-task** komutunu çalıştırırken Disk&#95;IMAGE&#95;FORMAT değişkeni için **VHD** kullandığınızdan emin olun. Dışa aktarılan VHD dosyası, bu işlem sırasında belirlediğiniz Amazon S3 kovasında kaydedilir.

    ```
    aws ec2 create-instance-export-task --instance-id ID --target-environment TARGET_ENVIRONMENT \
      --export-to-s3-task DiskImageFormat=DISK_IMAGE_FORMAT,ContainerFormat=ova,S3Bucket=BUCKET,S3Prefix=PREFIX
    ```

2. VHD dosyasını S3 kovasından indirin. VHD dosyasını seçin, ardından **Eylemler** > **İndir.**

    ![][3]

##### <a name="copy-a-vhd-from-other-non-azure-cloud"></a>Azure olmayan diğer bulutlardan bir VHD kopyalama
VHD'yi Azure olmayan Bulut Depolama'dan Azure'a taşıyorsanız, öncelikle VHD'yi yerel bir dizine dışa aktarmanız gerekir. VHD'nin depolandığı yerel dizinin tam kaynak yolunu kopyalayın.

##### <a name="copy-a-vhd-from-on-premises"></a>Şirket içinden bir VHD kopyalama
VHD'yi şirket içi ortamdan geçiriyorsanız, VHD'nin depolandığı tam kaynak yola ihtiyacınız olacaktır. Kaynak yolu bir sunucu konumu veya dosya paylaşımı olabilir.

#### <a name="step-2-create-the-destination-for-your-vhd"></a>2. Adım VHD'niz için hedef oluşturma
VHD'lerinizi korumak için bir depolama hesabı oluşturun. VHD'lerinizi nerede depolayacağınızda aşağıdaki noktaları göz önünde bulundurun:

* Hedef depolama hesabı, uygulama gereksiniminize bağlı olarak standart veya premium depolama alanı olabilir.
* Depolama hesabı bölgesi, son aşamada oluşturacağınız Premium Depolama özellikli Azure VM'lerle aynı olmalıdır. Yeni bir depolama hesabına kopyalayabilir veya gereksinimlerinize göre aynı depolama hesabını kullanmayı planlayabilirsiniz.
* Hedef depolama hesabının depolama hesabı anahtarını bir sonraki aşama için kopyalayın ve kaydedin.

Premium depolamayı kullanmak için üretim iş yükü için tüm verileri taşımanızı şiddetle öneririz.

#### <a name="step-3-upload-the-vhd-to-azure-storage"></a>3. Adım VHD'yi Azure Depolama'ya yükleme
VHD'niz yerel dizinde olduğuna göre, .vhd dosyasını Azure Depolama'ya yüklemek için AzCopy veya AzurePowerShell'i kullanabilirsiniz. Her iki seçenek de burada verilmiştir:

##### <a name="option-1-using-azure-powershell-add-azurevhd-to-upload-the-vhd-file"></a>Seçenek 1: .vhd dosyasını yüklemek için Azure PowerShell Ekle-AzureVhd'ı kullanma

```powershell
Add-AzureVhd [-Destination] <Uri> [-LocalFilePath] <FileInfo>
```

Uri \<>'ın **_"https://storagesample.blob.core.windows.net/mycontainer/blob1.vhd"_** olabileceğine bir örnek. Örnek \<FileInfo> **_"C:\path\to\upload.vhd"_** olabilir.

##### <a name="option-2-using-azcopy-to-upload-the-vhd-file"></a>Seçenek 2: .vhd dosyasını yüklemek için AzCopy kullanma

AzCopy'yi kullanarak VHD'yi Internet üzerinden kolayca yükleyebilirsiniz. VHD'lerin boyutuna bağlı olarak, bu işlem zaman alabilir. Bu seçeneği kullanırken depolama hesabı giriş/çıkış sınırlarını kontrol etmeyi unutmayın. Ayrıntılar [için standart depolama hesapları için ölçeklenebilirlik ve performans hedeflerine](scalability-targets-standard-account.md) bakın.

1. İndirin ve burada azcopy [yükleyin: AzCopy en son sürümü](https://aka.ms/downloadazcopy)
2. Azure PowerShell'i açın ve AzCopy'nin yüklü olduğu klasöre gidin.
3. VHD dosyasını "Kaynak"tan "Hedef"e kopyalamak için aşağıdaki komutu kullanın.

   ```azcopy
      AzCopy /Source: <source> /SourceKey: <source-account-key> /Dest: <destination> /DestKey: <dest-account-key> /BlobType:page /Pattern: <file-name>
   ```

   Örnek:

   ```azcopy
      AzCopy /Source:https://sourceaccount.blob.core.windows.net/mycontainer1 /SourceKey:key1 /Dest:https://destaccount.blob.core.windows.net/mycontainer2 /DestKey:key2 /BlobType:page /Pattern:abc.vhd
   ```

   Aşağıdana komutu kullanılan parametrelerin açıklamaları şunlardır:

   * **/Source:** _ &lt;&gt;kaynak :_ VHD içeren klasörün veya depolama kapsayıcısının URL'sinin konumu.
   * **/SourceKey:** _ &lt;kaynak-hesap&gt;anahtarı :_ Kaynak depolama hesabının depolama hesabı anahtarı.
   * **/Dest:** _ &lt;&gt;hedef :_ VHD'yi kopyalamak için depolama kapsayıcısı URL'si.
   * **/DestKey:** _ &lt;dest-account-key&gt;:_ Hedef depolama hesabının depolama hesabı anahtarı.
   * **/BlobType: sayfa:** Hedefin bir sayfa blob olduğunu belirtir.
   * **/Pattern:** _ &lt;dosya&gt;adı :_ Kopyalamak için VHD'nin dosya adını belirtin.

AzCopy aracını kullanma hakkında daha fazla bilgi için, [AzCopy Command-Line Utility ile veri aktarımı'na](storage-use-azcopy.md)bakın.

##### <a name="other-options-for-uploading-a-vhd"></a>VHD yüklemek için diğer seçenekler
Ayrıca aşağıdaki araçlardan birini kullanarak depolama hesabınıza bir VHD yükleyebilirsiniz:

* [Azure Depolama Kopyalama Blob API](https://msdn.microsoft.com/library/azure/dd894037.aspx)
* [Azure Depolama Gezgini Yükleme Blobs](https://azurestorageexplorer.codeplex.com/)
* [Depolama İthalat/İhracat Hizmeti REST API Referans](https://msdn.microsoft.com/library/dn529096.aspx)

> [!NOTE]
> Tahmini yükleme süresi 7 günden uzunsa, Alma/Dışa Aktarma Hizmetini kullanmanızı öneririz. Veri boyutu ve aktarım biriminden gelen zamanı tahmin etmek için [DataTransferSpeedCalculator'ı](https://github.com/Azure-Samples/storage-dotnet-import-export-job-management/blob/master/DataTransferSpeedCalculator.html) kullanabilirsiniz.
>
> Alma/Dışa Aktarma, standart bir depolama hesabına kopyalamak için kullanılabilir. AzCopy gibi bir aracı kullanarak standart depolama dan premium depolama hesabına kopyalamanız gerekir.
>
>

## <a name="create-azure-vms-using-premium-storage"></a><a name="create-azure-virtual-machine-using-premium-storage"></a>Premium Depolama'yı kullanarak Azure VM'leri oluşturun
VHD yüklendikten veya istenen depolama hesabına kopyalandıktan sonra, bu bölümdeki talimatları uygulayarak VHD'yi senaryonuza bağlı olarak işletim sistemi görüntüsü veya işletim sistemi diski olarak kaydedin ve ondan bir VM örneği oluşturun. Veri diski VHD oluşturulduktan sonra VM'ye bağlanabilir.
Bu bölümün sonunda örnek bir geçiş komut dosyası sağlanır. Bu basit komut dosyası tüm senaryolarla eşleşmez. Komut dosyasını belirli senaryonuzla eşleşecek şekilde güncelleştirmeniz gerekebilir. Bu komut dosyasının senaryonuza uygun olup olmadığını görmek için [Örnek Geçiş Komut Dosyası'nın](#a-sample-migration-script)altına bakın.

### <a name="checklist"></a>Denetim Listesi
1. Tüm VHD diskkopyalama tamamlanana kadar bekleyin.
2. Premium Depolama'nın göç ettiğiniz bölgede kullanılabilir olduğundan emin olun.
3. Kullanacakyeni VM serisine karar verin. Premium Depolama yeteneğine sahip olmalı ve boyutu bölgedeki kullanılabilirliğe ve ihtiyaçlarınıza bağlı olarak olmalıdır.
4. Kullanacağınız tam VM boyutuna karar verin. VM boyutunun sahip olduğunuz veri diski sayısını destekleyecek kadar büyük olması gerekir. Örneğin 4 veri diskininvarsa, VM'de 2 veya daha fazla çekirdek olmalıdır. Ayrıca, işleme gücü, bellek ve ağ bant genişliği gereksinimlerini düşünün.
5. Hedef bölgede bir Premium Depolama hesabı oluşturun. Bu, yeni VM için kullanacağınız hesaptır.
6. Disklerin ve karşılık gelen VHD blob'larının listesi de dahil olmak üzere geçerli VM ayrıntılarını kullanışlı bir şekilde elde edin.

Başvurunuzu kapalı kalma süresine hazırlayın. Temiz bir geçiş yapmak için, geçerli sistemdeki tüm işlemleri durdurmanız gerekir. Ancak o zaman, yeni platforma geçirebileceğiniz tutarlı bir duruma getirebilirsiniz. Kapalı kalma süresi, geçirilecek disklerde bulunan veri miktarına bağlıdır.

> [!NOTE]
> Özel bir VHD Disk'ten bir Azure Kaynak Yöneticisi VM oluşturuyorsanız, varolan diski kullanarak Kaynak Yöneticisi VM'yi dağıtmak için lütfen [bu şablona](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-specialized-vhd) bakın.
>
>

### <a name="register-your-vhd"></a>VHD'nizi kaydedin
OS VHD'den bir VM oluşturmak veya yeni bir VM'ye veri diski eklemek için önce bunları kaydetmeniz gerekir. VHD'nizin senaryosuna bağlı olarak aşağıdaki adımları izleyin.

#### <a name="generalized-operating-system-vhd-to-create-multiple-azure-vm-instances"></a>Birden çok Azure VM örneği oluşturmak için Genelleştirilmiş İşletim Sistemi VHD
Genelleştirilmiş işletim sistemi görüntüsü VHD depolama hesabına yüklendikten sonra, bir veya daha fazla VM örneği oluşturabilmeniz için bunu **Azure VM Görüntüsü** olarak kaydedin. VHD'nizi Azure VM işletim sistemi görüntüsü olarak kaydetmek için aşağıdaki PowerShell cmdlets'i kullanın. VHD'nin kopyalandığı konteyner URL'sinin tamamını sağlayın.

```powershell
Add-AzureVMImage -ImageName "OSImageName" -MediaLocation "https://storageaccount.blob.core.windows.net/vhdcontainer/osimage.vhd" -OS Windows
```

Bu yeni Azure VM Görüntüsünün adını kopyalayın ve kaydedin. Yukarıdaki örnekte, *OSImageName*olduğunu.

#### <a name="unique-operating-system-vhd-to-create-a-single-azure-vm-instance"></a>Tek bir Azure VM örneği oluşturmak için Benzersiz İşletim Sistemi VHD
Benzersiz IŞLETIM Sistemi VHD depolama hesabına yüklendikten sonra, bu hesaptan bir VM örneği oluşturabilmeniz için bunu Azure **İşLetim Diski** olarak kaydedin. VHD'nizi Azure İş Let'i olarak kaydetmek için bu PowerShell cmdlets'i kullanın. VHD'nin kopyalandığı konteyner URL'sinin tamamını sağlayın.

```powershell
Add-AzureDisk -DiskName "OSDisk" -MediaLocation "https://storageaccount.blob.core.windows.net/vhdcontainer/osdisk.vhd" -Label "My OS Disk" -OS "Windows"
```

Bu yeni Azure İşletim Sistemi Diskinin adını kopyalayın ve kaydedin. Yukarıdaki örnekte, *OSDisk*olduğunu.

#### <a name="data-disk-vhd-to-be-attached-to-new-azure-vm-instances"></a>Yeni Azure VM örneğine(ler) eklenecek veri diski VHD
Veri diski VHD depolama hesabına yüklendikten sonra, yeni DS Serisi, DSv2 serisi veya GS Serisi Azure VM örneğinize eklenebilecek şekilde bunu Azure Veri Diski olarak kaydedin.

VHD'nizi Azure Veri Diski olarak kaydetmek için bu PowerShell cmdlets'i kullanın. VHD'nin kopyalandığı konteyner URL'sinin tamamını sağlayın.

```powershell
Add-AzureDisk -DiskName "DataDisk" -MediaLocation "https://storageaccount.blob.core.windows.net/vhdcontainer/datadisk.vhd" -Label "My Data Disk"
```

Bu yeni Azure Veri Diskinin adını kopyalayın ve kaydedin. Yukarıdaki örnekte, *Bu DataDisk*olduğunu.

### <a name="create-a-premium-storage-capable-vm"></a>Premium Depolama özelliğine sahip bir VM oluşturun
İşletim sistemi veya işletim sistemi diski kaydedildikten sonra yeni bir DS serisi, DSv2 serisi veya GS serisi VM oluşturun. Kaydettiğiniz işletim sistemi görüntüsünü veya işletim sistemi disk adını kullanacaksınız. Premium Depolama katmanından VM türünü seçin. Aşağıdaki örnekte, *Standard_DS2* VM boyutunu kullanıyoruz.

> [!NOTE]
> Kapasite ve performans gereksinimlerinize ve kullanılabilir Azure disk boyutlarıyla eşleştiğinden emin olmak için disk boyutunu güncelleştirin.
>
>

Yeni VM oluşturmak için aşağıdaki adım PowerShell cmdlets adım izleyin. İlk olarak, ortak parametreleri ayarlayın:

```powershell
$serviceName = "yourVM"
$location = "location-name" (e.g., West US)
$vmSize ="Standard_DS2"
$adminUser = "youradmin"
$adminPassword = "yourpassword"
$vmName ="yourVM"
$vmSize = "Standard_DS2"
```

İlk olarak, yeni VM'lerinizi barındıracağınız bir bulut hizmeti oluşturun.

```powershell
New-AzureService -ServiceName $serviceName -Location $location
```

Ardından, senaryonuza bağlı olarak, kaydettiğiniz işletim sistemi görüntüsünden veya işletim sistemi diskinden Azure VM örneğini oluşturun.

#### <a name="generalized-operating-system-vhd-to-create-multiple-azure-vm-instances"></a>Birden çok Azure VM örneği oluşturmak için Genelleştirilmiş İşletim Sistemi VHD
Kaydettiğiniz **Azure İşletim Sistemi Görüntüsünü** kullanarak bir veya daha fazla yeni DS Serisi Azure VM örneğini oluşturun. Aşağıda gösterildiği gibi yeni VM oluştururken Bu OS Image adını VM yapılandırmasında belirtin.

```powershell
$OSImage = Get-AzureVMImage –ImageName "OSImageName"

$vm = New-AzureVMConfig -Name $vmName –InstanceSize $vmSize -ImageName $OSImage.ImageName

Add-AzureProvisioningConfig -Windows –AdminUserName $adminUser -Password $adminPassword –VM $vm

New-AzureVM -ServiceName $serviceName -VM $vm
```

#### <a name="unique-operating-system-vhd-to-create-a-single-azure-vm-instance"></a>Tek bir Azure VM örneği oluşturmak için Benzersiz İşletim Sistemi VHD
Kaydettiğiniz **Azure İşLetim Diskini** kullanarak yeni bir DS serisi Azure VM örneği oluşturun. Aşağıda gösterildiği gibi yeni VM oluştururken VM yapılandırmasında bu OS Disk adını belirtin.

```powershell
$OSDisk = Get-AzureDisk –DiskName "OSDisk"

$vm = New-AzureVMConfig -Name $vmName -InstanceSize $vmSize -DiskName $OSDisk.DiskName

New-AzureVM -ServiceName $serviceName –VM $vm
```

Bulut hizmeti, bölge, depolama hesabı, kullanılabilirlik kümesi ve önbelleğe alma ilkesi gibi diğer Azure VM bilgilerini belirtin. VM örneğinin ilişkili işletim sistemi veya veri diskleriyle birlikte bulunması gerektiğini, bu nedenle seçilen bulut hizmeti, bölge ve depolama hesabının tümü bu disklerin temel VHD'leriyle aynı konumda olmalıdır.

### <a name="attach-data-disk"></a>Veri diski ekleme
Son olarak, kayıtlı veri diski VHD'leri varsa, bunları yeni Premium Depolama özellikli Azure VM'sine takın.

Yeni VM'ye veri diski eklemek ve önbelleğe alma ilkesini belirtmek için aşağıdaki PowerShell cmdlet'i kullanın. Aşağıdaki örnekte önbelleğe alma ilkesi *ReadOnly*olarak ayarlanır.

```powershell
$vm = Get-AzureVM -ServiceName $serviceName -Name $vmName

Add-AzureDataDisk -ImportFrom -DiskName "DataDisk" -LUN 0 –HostCaching ReadOnly –VM $vm

Update-AzureVM  -VM $vm
```

> [!NOTE]
> Bu kılavuzda yer almayan uygulamanızı desteklemek için gerekli ek adımlar olabilir.
>
>

### <a name="checking-and-plan-backup"></a>Yedeklemeyi denetleme ve planlama
Yeni VM çalışmaya başladıktan sonra, aynı giriş kimliği ve parolayı kullanarak orijinal VM'dir ve her şeyin beklendiği gibi çalıştığını doğrulayın. Çizgili hacimler de dahil olmak üzere tüm ayarlar yeni VM'de bulunur.

Son adım, yeni VM için uygulamanın gereksinimlerine göre yedekleme ve bakım çizelgesi planlamaktır.

### <a name="a-sample-migration-script"></a><a name="a-sample-migration-script"></a>Örnek geçiş komut dosyası
Geçiş yapacak birden çok VM'niz varsa, PowerShell komut dosyaları üzerinden otomasyon yararlı olacaktır. Aşağıda, VM'nin geçişini otomatikleştiren örnek bir komut dosyası vereyim. Aşağıdaki komut dosyasının yalnızca bir örnek olduğunu ve geçerli VM diskleri hakkında birkaç varsayım yapıldığını unutmayın. Komut dosyasını belirli senaryonuzla eşleşecek şekilde güncelleştirmeniz gerekebilir.

Varsayımlar şunlardır:

* Klasik Azure VM'leri oluşturuyorsunuz.
* Kaynak Işletim Sistemi Diskleriniz ve kaynak Veri Diskleriniz aynı depolama hesabında ve aynı kapsayıcıdadır. İşletim Sistemi Diskleriniz ve Veri Diskleriniz aynı yerde değilse, Depolama hesapları ve kapsayıcılar üzerinden VHD'leri kopyalamak için AzCopy veya Azure PowerShell'i kullanabilirsiniz. Önceki adıma bakın: [AzCopy veya PowerShell ile VHD kopyalayın.](#copy-vhd-with-azcopy-or-powershell) Senaryonuzu karşılamak için bu komut dosyasını düzenlemek başka bir seçenektir, ancak daha kolay ve daha hızlı olduğu için AzCopy veya PowerShell kullanmanızı öneririz.

Otomasyon komut dosyası aşağıda verilmiştir. Metni bilgilerinizle değiştirin ve komut dosyasını belirli senaryonuzla eşleşecek şekilde güncelleştirin.

> [!NOTE]
> Varolan komut dosyasının kullanılması, kaynak VM'nizin ağ yapılandırmasını korumaz. Geçirilen VM'lerinizdeki ağ ayarlarını yeniden yorumlamanız gerekir.
>
>

```powershell
    <#
    .Synopsis
    This script is provided as an EXAMPLE to show how to migrate a VM from a standard storage account to a premium storage account. You can customize it according to your specific requirements.

    .Description
    The script will copy the vhds (page blobs) of the source VM to the new storage account.
    And then it will create a new VM from these copied vhds based on the inputs that you specified for the new VM.
    You can modify the script to satisfy your specific requirement, but please be aware of the items specified
    in the Terms of Use section.

    .Terms of Use
    Copyright © 2015 Microsoft Corporation.  All rights reserved.

    THIS CODE AND ANY ASSOCIATED INFORMATION ARE PROVIDED "AS IS" WITHOUT WARRANTY OF ANY KIND,
    EITHER EXPRESSED OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE IMPLIED WARRANTIES OF MERCHANTABILITY
    AND/OR FITNESS FOR A PARTICULAR PURPOSE. THE ENTIRE RISK OF USE, INABILITY TO USE, OR
    RESULTS FROM THE USE OF THIS CODE REMAINS WITH THE USER.

    .Example (Save this script as Migrate-AzureVM.ps1)

    .\Migrate-AzureVM.ps1 -SourceServiceName CurrentServiceName -SourceVMName CurrentVMName –DestStorageAccount newpremiumstorageaccount -DestServiceName NewServiceName -DestVMName NewDSVMName -DestVMSize "Standard_DS2" –Location "Southeast Asia"

    .Link
    To find more information about how to set up Azure PowerShell, refer to the following links.
    https://azure.microsoft.com/documentation/articles/powershell-install-configure/
    https://azure.microsoft.com/documentation/articles/storage-powershell-guide-full/
    https://azure.microsoft.com/blog/2014/10/22/migrate-azure-virtual-machines-between-storage-accounts/

    #>

    Param(
    # the cloud service name of the VM.
    [Parameter(Mandatory = $true)]
    [string] $SourceServiceName,

    # The VM name to copy.
    [Parameter(Mandatory = $true)]
    [String] $SourceVMName,

    # The destination storage account name.
    [Parameter(Mandatory = $true)]
    [String] $DestStorageAccount,

    # The destination cloud service name
    [Parameter(Mandatory = $true)]
    [String] $DestServiceName,

    # the destination vm name
    [Parameter(Mandatory = $true)]
    [String] $DestVMName,

    # the destination vm size
    [Parameter(Mandatory = $true)]
    [String] $DestVMSize,

    # the location of destination VM.
    [Parameter(Mandatory = $true)]
    [string] $Location,

    # whether or not to copy the os disk, the default is only copy data disks
    [Parameter(Mandatory = $false)]
    [Bool] $DataDiskOnly = $true,

    # how frequently to report the copy status in seconds
    [Parameter(Mandatory = $false)]
    [Int32] $CopyStatusReportInterval = 15,

    # the name suffix to add to new created disks to avoid conflict with source disk names
    [Parameter(Mandatory = $false)]
    [String]$DiskNameSuffix = "-prem"

    ) #end param

    #######################################################################
    #  Verify Azure PowerShell module and version
    #######################################################################

    #import the Azure PowerShell module
    Write-Host "`n[WORKITEM] - Importing Azure PowerShell module" -ForegroundColor Yellow
    $azureModule = Import-Module Azure -PassThru

    if ($azureModule -ne $null)
    {
        Write-Host "`tSuccess" -ForegroundColor Green
    }
    else
    {
        #show module not found interaction and bail out
        Write-Host "[ERROR] - PowerShell module not found. Exiting." -ForegroundColor Red
        Exit
    }


    #Check the Azure PowerShell module version
    Write-Host "`n[WORKITEM] - Checking Azure PowerShell module version" -ForegroundColor Yellow
    If ($azureModule.Version -ge (New-Object System.Version -ArgumentList "0.8.14"))
    {
        Write-Host "`tSuccess" -ForegroundColor Green
    }
    Else
    {
        Write-Host "[ERROR] - Azure PowerShell module must be version 0.8.14 or higher. Exiting." -ForegroundColor Red
        Exit
    }

    #Check if there is an azure subscription set up in PowerShell
    Write-Host "`n[WORKITEM] - Checking Azure Subscription" -ForegroundColor Yellow
    $currentSubs = Get-AzureSubscription -Current
    if ($currentSubs -ne $null)
    {
        Write-Host "`tSuccess" -ForegroundColor Green
        Write-Host "`tYour current azure subscription in PowerShell is $($currentSubs.SubscriptionName)." -ForegroundColor Green
    }
    else
    {
        Write-Host "[ERROR] - There is no valid Azure subscription found in PowerShell. Please refer to this article https://azure.microsoft.com/documentation/articles/powershell-install-configure/ to connect an Azure subscription. Exiting." -ForegroundColor Red
        Exit
    }


    #######################################################################
    #  Check if the VM is shut down
    #  Stopping the VM is a required step so that the file system is consistent when you do the copy operation.
    #  Azure does not support live migration at this time..
    #######################################################################

    if (($sourceVM = Get-AzureVM –ServiceName $SourceServiceName –Name $SourceVMName) -eq $null)
    {
        Write-Host "[ERROR] - The source VM doesn't exist in the current subscription. Exiting." -ForegroundColor Red
        Exit
    }

    # check if VM is shut down
    if ( $sourceVM.Status -notmatch "Stopped" )
    {
        Write-Host "[Warning] - Stopping the VM is a required step so that the file system is consistent when you do the copy operation. Azure does not support live migration at this time. If you'd like to create a VM from a generalized image, sys-prep the Virtual Machine before stopping it." -ForegroundColor Yellow
        $ContinueAnswer = Read-Host "`n`tDo you wish to stop $SourceVMName now? Input 'N' if you want to shut down the VM manually and come back later.(Y/N)"
        If ($ContinueAnswer -ne "Y") { Write-Host "`n Exiting." -ForegroundColor Red;Exit }
        $sourceVM | Stop-AzureVM

        # wait until the VM is shut down
        $VMStatus = (Get-AzureVM –ServiceName $SourceServiceName –Name $vmName).Status
        while ($VMStatus -notmatch "Stopped")
        {
            Write-Host "`n[Status] - Waiting VM $vmName to shut down" -ForegroundColor Green
            Sleep -Seconds 5
            $VMStatus = (Get-AzureVM –ServiceName $SourceServiceName –Name $vmName).Status
        }
    }

    # exporting the source vm to a configuration file, you can restore the original VM by importing this config file
    # see more information for Import-AzureVM
    $workingDir = (Get-Location).Path
    $vmConfigurationPath = $env:HOMEPATH + "\VM-" + $SourceVMName + ".xml"
    Write-Host "`n[WORKITEM] - Exporting VM configuration to $vmConfigurationPath" -ForegroundColor Yellow
    $exportRe = $sourceVM | Export-AzureVM -Path $vmConfigurationPath


    #######################################################################
    #  Copy the vhds of the source vm
    #  You can choose to copy all disks including os and data disks by specifying the
    #  parameter -DataDiskOnly to be $false. The default is to copy only data disk vhds
    #  and the new VM will boot from the original os disk.
    #######################################################################

    $sourceOSDisk = $sourceVM.VM.OSVirtualHardDisk
    $sourceDataDisks = $sourceVM.VM.DataVirtualHardDisks

    # Get source storage account information, not considering the data disks and os disks are in different accounts
    $sourceStorageAccountName = $sourceOSDisk.MediaLink.Host -split "\." | select -First 1
    $sourceStorageKey = (Get-AzStorageKey -StorageAccountName $sourceStorageAccountName).Primary
    $sourceContext = New-AzStorageContext –StorageAccountName $sourceStorageAccountName -StorageAccountKey $sourceStorageKey

    # Create destination context
    $destStorageKey = (Get-AzStorageKey -StorageAccountName $DestStorageAccount).Primary
    $destContext = New-AzStorageContext –StorageAccountName $DestStorageAccount -StorageAccountKey $destStorageKey

    # Create a container of vhds if it doesn't exist
    if ((Get-AzStorageContainer -Context $destContext -Name vhds -ErrorAction SilentlyContinue) -eq $null)
    {
        Write-Host "`n[WORKITEM] - Creating a container vhds in the destination storage account." -ForegroundColor Yellow
        New-AzStorageContainer -Context $destContext -Name vhds
    }


    $allDisksToCopy = $sourceDataDisks
    # check if need to copy os disk
    $sourceOSVHD = $sourceOSDisk.MediaLink.Segments[2]
    if ($DataDiskOnly)
    {
        # copy data disks only, this option requires deleting the source VM so that dest VM can boot
        # from the same vhd blob.
        $ContinueAnswer = Read-Host "`n`t[Warning] You chose to copy data disks only. Moving VM requires removing the original VM (the disks and backing vhd files will NOT be deleted) so that the new VM can boot from the same vhd. This is an irreversible action. Do you wish to proceed right now? (Y/N)"
        If ($ContinueAnswer -ne "Y") { Write-Host "`n Exiting." -ForegroundColor Red;Exit }
        $destOSVHD = Get-AzStorageBlob -Blob $sourceOSVHD -Container vhds -Context $sourceContext
        Write-Host "`n[WORKITEM] - Removing the original VM (the vhd files are NOT deleted)." -ForegroundColor Yellow
        Remove-AzureVM -Name $SourceVMName -ServiceName $SourceServiceName

        Write-Host "`n[WORKITEM] - Waiting until the OS disk is released by source VM. This may take up to several minutes."
        $diskAttachedTo = (Get-AzureDisk -DiskName $sourceOSDisk.DiskName).AttachedTo
        while ($diskAttachedTo -ne $null)
        {
            Start-Sleep -Seconds 10
            $diskAttachedTo = (Get-AzureDisk -DiskName $sourceOSDisk.DiskName).AttachedTo
        }

    }
    else
    {
        # copy the os disk vhd
        Write-Host "`n[WORKITEM] - Starting copying os disk $($disk.DiskName) at $(get-date)." -ForegroundColor Yellow
        $allDisksToCopy += @($sourceOSDisk)
        $targetBlob = Start-AzStorageBlobCopy -SrcContainer vhds -SrcBlob $sourceOSVHD -DestContainer vhds -DestBlob $sourceOSVHD -Context $sourceContext -DestContext $destContext -Force
        $destOSVHD = $targetBlob
    }


    # Copy all data disk vhds
    # Start all async copy requests in parallel.
    foreach($disk in $sourceDataDisks)
    {
        $blobName = $disk.MediaLink.Segments[2]
        # copy all data disks
        Write-Host "`n[WORKITEM] - Starting copying data disk $($disk.DiskName) at $(get-date)." -ForegroundColor Yellow
        $targetBlob = Start-AzStorageBlobCopy -SrcContainer vhds -SrcBlob $blobName -DestContainer vhds -DestBlob $blobName -Context $sourceContext -DestContext $destContext -Force
        # update the media link to point to the target blob link
        $disk.MediaLink = $targetBlob.ICloudBlob.Uri.AbsoluteUri
    }

    # Wait until all vhd files are copied.
    $diskComplete = @()
    do
    {
        Write-Host "`n[WORKITEM] - Waiting for all disk copy to complete. Checking status every $CopyStatusReportInterval seconds." -ForegroundColor Yellow
        # check status every 30 seconds
        Sleep -Seconds $CopyStatusReportInterval
        foreach ( $disk in $allDisksToCopy)
        {
            if ($diskComplete -contains $disk)
            {
                Continue
            }
            $blobName = $disk.MediaLink.Segments[2]
            $copyState = Get-AzStorageBlobCopyState -Blob $blobName -Container vhds -Context $destContext
            if ($copyState.Status -eq "Success")
            {
                Write-Host "`n[Status] - Success for disk copy $($disk.DiskName) at $($copyState.CompletionTime)" -ForegroundColor Green
                $diskComplete += $disk
            }
            else
            {
                if ($copyState.TotalBytes -gt 0)
                {
                    $percent = ($copyState.BytesCopied / $copyState.TotalBytes) * 100
                    Write-Host "`n[Status] - $('{0:N2}' -f $percent)% Complete for disk copy $($disk.DiskName)" -ForegroundColor Green
                }
            }
        }
    }
    while($diskComplete.Count -lt $allDisksToCopy.Count)

    #######################################################################
    #  Create a new vm
    #  the new VM can be created from the copied disks or the original os disk.
    #  You can ddd your own logic here to satisfy your specific requirements of the vm.
    #######################################################################

    # Create a VM from the existing os disk
    if ($DataDiskOnly)
    {
        $vm = New-AzureVMConfig -Name $DestVMName -InstanceSize $DestVMSize -DiskName $sourceOSDisk.DiskName
    }
    else
    {
        $newOSDisk = Add-AzureDisk -OS $sourceOSDisk.OS -DiskName ($sourceOSDisk.DiskName + $DiskNameSuffix) -MediaLocation $destOSVHD.ICloudBlob.Uri.AbsoluteUri
        $vm = New-AzureVMConfig -Name $DestVMName -InstanceSize $DestVMSize -DiskName $newOSDisk.DiskName
    }
    # Attached the copied data disks to the new VM
    foreach ($dataDisk in $sourceDataDisks)
    {
        # add -DiskLabel $dataDisk.DiskLabel if there are labels for disks of the source vm
        $diskLabel = "drive" + $dataDisk.Lun
        $vm | Add-AzureDataDisk -ImportFrom -DiskLabel $diskLabel -LUN $dataDisk.Lun -MediaLocation $dataDisk.MediaLink
    }

    # Edit this if you want to add more customization to the new VM
    # $vm | Add-AzureEndpoint -Protocol tcp -LocalPort 443 -PublicPort 443 -Name 'HTTPs'
    # $vm | Set-AzureSubnet "PubSubnet","PrivSubnet"

    New-AzureVM -ServiceName $DestServiceName -VMs $vm -Location $Location
```

#### <a name="optimization"></a><a name="optimization"></a>İyileştirme
Geçerli VM yapılandırmanız, Standart disklerle iyi çalışacak şekilde özelleştirilebilir. Örneğin, çizgili bir birimde çok sayıda disk kullanarak performansı artırmak için. Örneğin, Premium Depolama'da 4 diski ayrı ayrı kullanmak yerine, tek bir diske sahip olarak maliyeti en iyi duruma getirebilirsiniz. Bu gibi optimizasyonların servis talebi bazında ele alınması ve geçişten sonra özel adımlar gerektirmesi gerekir. Ayrıca, bu işlemin kurulumda tanımlanan disk düzenine bağlı veritabanları ve uygulamalar için iyi çalışmayabilir unutmayın.

##### <a name="preparation"></a>Hazırlık
1. Önceki bölümde açıklandığı gibi Basit Geçiş'i tamamlayın. Geçişten sonra yeni VM'de optimizasyonlar yapılacaktır.
2. En iyi duruma getirilmiş yapılandırma için gereken yeni disk boyutlarını tanımlayın.
3. Geçerli disklerin/birimlerin yeni disk özelliklerine eşlenemesini belirleyin.

##### <a name="execution-steps"></a>Yürütme adımları
1. Premium Depolama VM'de doğru boyutlarda yeni diskler oluşturun.
2. VM'ye giriş yapın ve geçerli birimdeki verileri o ses düzeyiyle eşleyen yeni diske kopyalayın. Bunu, yeni bir diskle eşlenebilen tüm geçerli birimler için yapın.
3. Ardından, yeni disklere geçmek için uygulama ayarlarını değiştirin ve eski birimleri ayırın.

Daha iyi disk performansı için uygulamayı aparat etmek için lütfen [yüksek performanslı](../../virtual-machines/windows/premium-storage-performance.md) makale tasarımımızın en iyi duruma getirilmesi uygulama performansı bölümüne bakın.

### <a name="application-migrations"></a>Uygulama geçişleri
Veritabanları ve diğer karmaşık uygulamalar geçiş için uygulama sağlayıcısı tarafından tanımlandığı gibi özel adımlar gerektirebilir. Lütfen ilgili başvuru belgelerine bakın. Örneğin genellikle veritabanları yedekleme ve geri yükleme yoluyla geçirilebilir.

## <a name="next-steps"></a>Sonraki adımlar
Geçiren sanal makineler için belirli senaryolar için aşağıdaki kaynaklara bakın:

* [Azure Sanal Makinelerini Depolama Hesapları arasında geçirme](https://azure.microsoft.com/blog/2014/10/22/migrate-azure-virtual-machines-between-storage-accounts/)
* [Azure'a bir Windows Server VHD oluşturun ve yükleyin.](../../virtual-machines/windows/upload-generalized-managed.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [Linux VHD Oluşturma ve Azure'a Yükleme](../../virtual-machines/linux/create-upload-generic.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Sanal Makineleri Amazon AWS'den Microsoft Azure'a geçirme](https://channel9.msdn.com/Series/Migrating-Virtual-Machines-from-Amazon-AWS-to-Microsoft-Azure)

Ayrıca, Azure Depolama ve Azure Sanal Makineleri hakkında daha fazla bilgi edinmek için aşağıdaki kaynaklara bakın:

* [Azure Depolama](https://azure.microsoft.com/documentation/services/storage/)
* [Azure Sanal Makineler](https://azure.microsoft.com/documentation/services/virtual-machines/)
* [IaaS VM’leri için disk türü seçme](../../virtual-machines/windows/disks-types.md)

[1]:./media/storage-migration-to-premium-storage/migration-to-premium-storage-1.png
[2]:./media/storage-migration-to-premium-storage/migration-to-premium-storage-1.png
[3]:./media/storage-migration-to-premium-storage/migration-to-premium-storage-3.png
[4]: https://technet.microsoft.com/library/hh831739.aspx
