---
title: include dosyası
description: include dosyası
services: storage
author: roygara
ms.service: storage
ms.topic: include
ms.date: 09/15/2018
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: f30518c3bfc9876cbddaf8295ff9e8b667a70200
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "74014542"
---
## <a name="overview"></a>Genel Bakış
Azure Depolama, lekelerin anlık görüntülerini alma olanağı sağlar. Anlık görüntüler, bu noktada leke durumunu yakalar. Bu makalede, anlık görüntüleri kullanarak sanal makine disklerinin yedeklerini koruyabileceğiniz bir senaryo açıklıyoruz. Azure Yedekleme ve Kurtarma Hizmeti'ni kullanmamayı seçtiğinizde ve sanal makine diskleriniz için özel bir yedekleme stratejisi oluşturmak istediğinizde bu metodolojiyi kullanabilirsiniz.

Azure sanal makine diskleri, Azure Depolama'da sayfa blob'ları olarak depolanır. Bu makalede sanal makine diskleri için bir yedekleme stratejisi tanımladığımız için, sayfa lekeleri bağlamında anlık görüntülere başvururuz. Anlık görüntüler hakkında daha fazla bilgi edinmek için [Blob'un Anlık Görüntüsü Oluşturma](https://docs.microsoft.com/rest/api/storageservices/Creating-a-Snapshot-of-a-Blob)bölümüne bakın.

## <a name="what-is-a-snapshot"></a>Anlık görüntü nedir?
Blob anlık görüntüsü, bir noktada yakalanan bir blobun salt okunur halidir. Anlık görüntü oluşturulduktan sonra okunabilir, kopyalanabilir veya silinebilir, ancak değiştirilemez. Anlık görüntüler, bir anda göründüğü gibi bir lekeyi yedeklemenin bir yolunu sağlar. REST sürümü 2015-04-05'e kadar, tüm anlık görüntüleri kopyalama olanağınız vardı. REST sürümü 2015-07-08 ve üzeri ile artımlı anlık görüntüleri de kopyalayabilirsiniz.

## <a name="full-snapshot-copy"></a>Tam anlık görüntü kopyası
Anlık görüntüler, temel blob yedeklerini tutmak için bir blob olarak başka bir depolama hesabına kopyalanabilir. Ayrıca, lekeyi önceki bir sürüme geri getirmek gibi taban lekesinin üzerinde anlık görüntü kopyalayabilirsiniz. Anlık görüntü bir depolama hesabından diğerine kopyalandığında, temel sayfa blob'uyla aynı alanı kaplar. Bu nedenle, anlık görüntüleri bir depolama hesabından diğerine kopyalamak yavaştır ve hedef depolama hesabında çok fazla alan tüketir.

> [!NOTE]
> Temel blob'u başka bir hedefe kopyalarsanız, blob'un anlık görüntüleri onunla birlikte kopyalanmaz. Benzer şekilde, bir kopyaile bir taban blob üzerine yazarsanız, temel blob ile ilişkili anlık görüntü etkilenmez ve temel blob adı altında bozulmadan kalır.
> 
> 

### <a name="back-up-disks-using-snapshots"></a>Anlık görüntüleri kullanarak diskleri yedekleme
Sanal makine diskleriniz için yedekleme stratejisi olarak, disk veya sayfa lekelerinin periyodik anlık görüntülerini alabilir ve [Kopya Blob](https://docs.microsoft.com/rest/api/storageservices/Copy-Blob) işlemi veya [AzCopy](../articles/storage/common/storage-use-azcopy.md)gibi araçları kullanarak bunları başka bir depolama hesabına kopyalayabilirsiniz. Anlık görüntü, hedef sayfa blob'una farklı bir adla kopyalayabilirsiniz. Ortaya çıkan hedef sayfa blob bir anlık değil, yazılabilir bir sayfa blob olduğunu. Bu makalenin ilerleyen saatlerinde, anlık görüntüleri kullanarak sanal makine disklerinin yedeklerini almak için adımları açıklarız.

### <a name="restore-disks-using-snapshots"></a>Anlık görüntüleri kullanarak diskleri geri yükleme
Diskinizi daha önce yedek anlık görüntülerden birinde yakalanan kararlı bir sürüme geri yükleme zamanı geldiğinde, temel sayfa blob'unun üzerinden anlık görüntü kopyalayabilirsiniz. Anlık görüntü temel sayfa blob'una yükseltildikten sonra, anlık görüntü kalır, ancak kaynağı hem okunabilen hem de yazIlebilen bir kopyayla üzerine yazılır. Bu makalenin ilerleyen saatlerinde, diskinizin önceki bir sürümünü anlık görüntüsünden geri yükleme adımlarını açıklarız.

### <a name="implementing-full-snapshot-copy"></a>Tam anlık görüntü kopyasını uygulama
Aşağıdakileri yaparak tam bir anlık görüntü kopyasını uygulayabilirsiniz,

* İlk olarak, [Snapshot Blob](https://docs.microsoft.com/rest/api/storageservices/Snapshot-Blob) işlemini kullanarak temel blob bir anlık görüntü almak.
* Ardından, [Kopya Blob'u](https://docs.microsoft.com/rest/api/storageservices/Copy-Blob)kullanarak anlık görüntünün hedef depolama hesabına kopyalanmasını.
* Temel lekenizin yedek kopyalarını korumak için bu işlemi yineleyin.

## <a name="incremental-snapshot-copy"></a>Artımlı anlık görüntü kopyası
[GetPageRanges](https://docs.microsoft.com/rest/api/storageservices/Get-Page-Ranges) API'deki yeni özellik, sayfanızın anlık görüntülerini yedeklemek için çok daha iyi bir yol sağlar. API, temel blob ve anlık görüntüler arasındaki değişikliklerin listesini döndürür, bu da yedekleme hesabında kullanılan depolama alanı miktarını azaltır. API, Premium Depolama'daki sayfa lekelerini ve Standart Depolamayı destekler. Bu API'yi kullanarak Azure VM'leri için daha hızlı ve daha verimli yedekleme çözümleri oluşturabilirsiniz. Bu API REST sürümü ile satışa sunulacak 2015-07-08 ve daha yüksek.

Artımlı Anlık Görüntü Kopyalama, bir depolama hesabından diğerine arasındaki farkı kopyalamanızı sağlar,

* Temel blob ve Anlık Görüntü VEYA
* Taban lekeherhangi iki anlık

Aşağıdaki koşulların yerine getirilmesi kaydıyla,

* Leke 1-1-2016 veya daha sonra oluşturuldu.
* Blob [putpage](https://docs.microsoft.com/rest/api/storageservices/Put-Page) veya [Copy Blob](https://docs.microsoft.com/rest/api/storageservices/Copy-Blob) ile iki anlık görüntü arasında üzerine yazılmadı.

**Not**: Bu özellik Premium ve Standart Azure Sayfa Blobs için kullanılabilir.

Anlık görüntüleri kullanarak özel bir yedekleme stratejiniz varsa, anlık görüntüleri bir depolama hesabından diğerine kopyalamak yavaş olabilir ve çok fazla depolama alanı tüketebilir. Anlık görüntünün tamamını yedek depolama hesabına kopyalamak yerine, ardışık anlık görüntüler arasındaki farkı bir yedekleme sayfası blob'una yazabilirsiniz. Bu şekilde, kopyalama zamanı ve yedeklemeleri depolama alanı önemli ölçüde azalır.

### <a name="implementing-incremental-snapshot-copy"></a>Artımlı Anlık Görüntü Kopyalama uygulama
Aşağıdakileri yaparak artımlı anlık görüntü kopyasını uygulayabilirsiniz,

* [Snapshot Blob'u](https://docs.microsoft.com/rest/api/storageservices/Snapshot-Blob)kullanarak temel blob'un anlık görüntüsünü alın.
* Anlık görüntü, [Kopya Blob'u](https://docs.microsoft.com/rest/api/storageservices/Copy-Blob)kullanarak aynı veya başka bir Azure bölgesinde bulunan hedef yedekleme depolama hesabına kopyalayın. Bu yedek sayfa blob olduğunu. Yedek sayfa lekesinin anlık görüntüsünü alın ve yedek hesapta saklayın.
* Snapshot Blob'u kullanarak temel lekenin başka bir anlık görüntüsünü alın.
* [GetPageRanges](https://docs.microsoft.com/rest/api/storageservices/Get-Page-Ranges)kullanarak temel blob ilk ve ikinci anlık arasındaki farkı alın. Farkı almak istediğiniz anlık görüntünün DateTime değerini belirtmek için yeni parametre **prevsnapshot'ı**kullanın. Bu parametre mevcut olduğunda, REST yanıtı yalnızca hedef anlık görüntü ile açık sayfalar da dahil olmak üzere önceki anlık görüntü arasında değiştirilen sayfaları içerir.
* Bu değişiklikleri yedekleme sayfası blob'una uygulamak için [PutPage'i](https://docs.microsoft.com/rest/api/storageservices/Put-Page) kullanın.
* Son olarak, yedek sayfa blob bir anlık görüntü almak ve yedek depolama hesabında saklayın.

Bir sonraki bölümde, Artımlı Anlık Görüntü Kopyalama'yı kullanarak disklerin yedeklerini nasıl koruyabileceğinizi daha ayrıntılı olarak açıklayacağız.

## <a name="scenario"></a>Senaryo
Bu bölümde, anlık görüntüleri kullanarak sanal makine diskleri için özel bir yedekleme stratejisi içeren bir senaryo açıklar.

Premium depolama alanı P30 diskekli bir DS serisi Azure VM düşünün. *Mypremiumdisk* adı verilen P30 diski *mypremiumaccount*adı verilen premium bir depolama hesabında saklanır. *Mybackupstdaccount* adlı standart bir depolama hesabı *mypremiumdisk*yedekleme depolamak için kullanılır. Her 12 saatte bir *mypremiumdisk'in* anlık görüntüsünü saklamak istiyoruz.

Depolama hesabı oluşturma hakkında bilgi edinmek için [bkz.](https://docs.microsoft.com/azure/storage/common/storage-quickstart-create-account)

Azure VM'leri yedekleme hakkında bilgi edinmek için [Azure VM yedeklemelerini planlayın.](../articles/backup/backup-azure-vms-introduction.md)

## <a name="steps-to-maintain-backups-of-a-disk-using-incremental-snapshots"></a>Artımlı anlık görüntüler kullanarak bir diskin yedeklemelerini koruma adımları
Aşağıdaki adımlar *mypremiumdisk* anlık görüntüleri almak ve *mybackupstdaccount*yedekleme korumak nasıl açıklayınız. Yedekleme *mybackupstdpageblob*denilen standart bir sayfa blob olduğunu. Yedekleme sayfası blob her zaman *mypremiumdisk*son anlık olarak aynı durumu yansıtır.

1. *mypremiumdisk_ss1*adlı *mypremiumdiskin* anlık görüntüsünü alarak, premium depolama diskiniz için yedek sayfa blob'unu oluşturun.
2. *Mybackupstpageblob*adlı bir sayfa blob olarak mybackupstdaccount bu anlık kopyalayın.
3. [Snapshot Blob](https://docs.microsoft.com/rest/api/storageservices/Snapshot-Blob) kullanarak *mybackupstdpageblob_ss1*adlı *mybackupstdpageblob* bir anlık görüntü alın ve *mybackupstdaccount*içinde saklayın.
4. Yedekleme penceresi sırasında, *mypremiumdisk*başka bir anlık oluşturmak , *mypremiumdisk_ss2*demek , ve *mypremiumaccount*saklayın .
5. mypremiumdisk_ss1 zaman damgası ayarlanmış **prevsnapshot** parametresi ile [mypremiumdisk_ss2'da GetPageRanges](https://docs.microsoft.com/rest/api/storageservices/Get-Page-Ranges) kullanarak, iki anlık görüntü, *mypremiumdisk_ss2* ve *mypremiumdisk_ss1*arasındaki artımlı değişiklikleri *alın.* *mypremiumdisk_ss2* *Mybackupstdaccount*yedekleme sayfası blob *mybackupstdpageblob* bu artımlı değişiklikler yazın. Artımlı değişikliklerde silinmiş aralıklar varsa, bunların yedek sayfa blob'undan temizlenmesi gerekir. Yedekleme sayfası blob artımlı değişiklikler yazmak için [PutPage](https://docs.microsoft.com/rest/api/storageservices/Put-Page) kullanın.
6. Yedek sayfa blob *mybackupstdpageblob*bir anlık görüntü alın , *mybackupstdpageblob_ss2*denir. Önceki anlık *mypremiumdisk_ss1* premium depolama hesabından silin.
7. Her yedekleme penceresi 4-6 adımlarını yineleyin. Bu şekilde, *mypremiumdisk* yedeklemelerini standart bir depolama hesabında muhafaza edebilirsiniz.

![Artımlı anlık görüntüleri kullanarak diski yedekleme](../articles/virtual-machines/windows/media/incremental-snapshots/storage-incremental-snapshots-1.png)

## <a name="steps-to-restore-a-disk-from-snapshots"></a>Anlık görüntülerden diski geri yükleme adımları
Aşağıdaki adımlar, nasıl premium disk, *mypremiumdisk* yedekleme depolama *hesabı mybackupstdaccount*önceki bir anlık için geri yüklemek için açıklar.

1. Premium diski geri yüklemek istediğiniz noktayı belirleyin. Bu anlık *mybackupstdpageblob_ss2*olduğunu varsayalım , hangi yedekleme depolama hesabı *mybackupstdaccount*saklanır .
2. Mybackupstdaccount olarak, yeni yedekleme temel sayfası blob *mybackupstdpageblobrestored*olarak *anlık mybackupstdpageblob_ss2* tanıtın.
3. *bu*geri yüklenen yedekleme sayfası blob bir anlık görüntü alın, mybackupstdpageblobrestored_ss1 denir.
4. Yeni premium disk *mypremiumdiskrestored*olarak *mybackupstdaccount* *mypremiumaccount* restore edilen geri sayfa blob *mybackupstdpageblobrestored* kopyalayın .
5. Gelecekteki artımlı yedeklemeler yapmak için *mypremiumdiskrestored_ss1* olarak adlandırılan *mypremiumdiskrestored*bir anlık görüntü alın.
6. DS serisi VM'yi geri yüklenen *diske* işaret edin ve eski *mypremiumdiski* VM'den ayırın.
7. Geri yüklenen disk *mypremiumdiskrestored*için önceki bölümde açıklanan Yedekleme işlemini başlatın , yedekleme sayfası blob olarak *mybackupstdpageblobrestored* kullanarak.

![Anlık görüntülerden diski geri yükleme](../articles/virtual-machines/windows/media/incremental-snapshots/storage-incremental-snapshots-2.png)

## <a name="next-steps"></a>Sonraki Adımlar
Bir blob anlık görüntü oluşturma ve VM yedekleme altyapıplanlama hakkında daha fazla bilgi edinmek için aşağıdaki bağlantıları kullanın.

* [Blob'un Anlık Görüntüsünü Oluşturma](https://docs.microsoft.com/rest/api/storageservices/Creating-a-Snapshot-of-a-Blob)
* [VM Yedekleme Altyapınızı planlayın](../articles/backup/backup-azure-vms-introduction.md)

