---
title: Azure disk yedekleme 'ye Genel Bakış
description: Azure disk yedekleme çözümü hakkında bilgi edinin.
ms.topic: conceptual
ms.date: 01/07/2021
ms.openlocfilehash: 4db2a5f3f02322f18fcf9203c3560905cde86996
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2021
ms.locfileid: "98915517"
---
# <a name="overview-of-azure-disk-backup-in-preview"></a>Azure disk yedekleme 'ye Genel Bakış (önizlemede)

>[!IMPORTANT]
>Azure disk yedekleme, bir hizmet düzeyi sözleşmesi olmadan önizlemededir ve üretim iş yükleri için önerilmez. Daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/). Bölge kullanılabilirliği için bkz. [destek matrisi](disk-backup-support-matrix.md).
>
>Önizlemeye kaydolmak için [Bu formu doldurun](https://forms.office.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbR1vE8L51DIpDmziRt_893LVUNFlEWFJBN09PTDhEMjVHS05UWFkxUlUzUS4u) .

Azure disk yedekleme, yönetilen disklerde verilerinizi koruyan yerel, bulut tabanlı bir yedekleme çözümüdür. Bu, birkaç adımda yönetilen diskler için korumayı yapılandırmanızı sağlayan basit, güvenli ve ekonomik bir çözümdür. Verilerinizi bir olağanüstü durum senaryosunda kurtarmanıza de sağlar.

Azure disk yedekleme, anlık görüntülerin düzenli olarak oluşturulmasını otomatikleştirerek yönetilen diskler için anlık görüntü yaşam döngüsü yönetimi sağlayan ve yedekleme ilkesini kullanarak yapılandırılan süre için bir anahtar, bir çözüm sunar. Disk anlık görüntülerini sıfır altyapı maliyetiyle ve özel betik oluşturmaya veya herhangi bir yönetim yüküne gerek kalmadan yönetebilirsiniz. Bu, günde birden çok yedekleme desteği olan [Artımlı anlık görüntüleri](../virtual-machines/disks-incremental-snapshots.md) kullanarak yönetilen bir diskin zaman içinde yedeğini alan çökme ile tutarlı bir yedekleme çözümüdür. Ayrıca, aynı zamanda aracılı bir çözümdür ve üretim uygulaması performansını etkilemez. Hem işletim sistemi hem de veri disklerinin (paylaşılan diskler dahil), çalışmakta olan bir Azure sanal makinesine bağlı olup olmadığına bakılmaksızın yedekleme ve geri yükleme desteği sağlar.

Veri diskleri dahil olmak üzere sanal makinenin uygulamayla tutarlı yedeklemesini veya bir sanal makinenin tamamını yedekten geri yükleme, bir dosya veya klasörü geri yükleme veya ikincil bir bölgeye geri yükleme seçeneği için [Azure VM yedekleme](backup-azure-vms-introduction.md) çözümünü kullanın. Azure Backup, [Azure VM yedekleme](./backup-azure-vms-introduction.md) çözümlerine ek olarak, disk yedekleme 'yi kullanarak yönetilen disklerin yedeklenmesi için yan yana destek sunar. Bu, sanal makinelerin bir gününde bir uygulamayla tutarlı yedeklemeler ve ayrıca işletim sistemi disklerinin veya kilitlenme tutarlılığı olan belirli bir veri diskinin daha sık yedeklemesi gerektiğinde ve üretim uygulaması performansını etkilemezseniz yararlıdır.

Azure disk yedekleme, kuruluşların her ölçekte yedeklemeleri yönetmek, izlemek, çalıştırmak ve analiz etmek için Azure 'da **tek bir birleştirilmiş yönetim deneyimi** sunan yedekleme merkezi ile tümleşiktir.

## <a name="key-benefits-of-disk-backup"></a>Disk yedeklemesinin önemli avantajları

Azure disk yedekleme, [Artımlı anlık görüntü](../virtual-machines/disks-incremental-snapshots.md) kullanan aracısız ve kilitlenmeyle tutarlı bir çözümdür ve aşağıdaki avantajları sunar:

- Sanal makineyi kesintiye uğramadan daha sık ve hızlı yedeklemeler.
- , Üretim uygulamasının performansını etkilemez.
- Özel betikler çalıştırmak veya aracıları yüklemek gerektirmediğinden dolayı güvenlik konusu yoktur.
- Tüm sanal makineleri yedeklemeye kıyasla belirli diskleri yedeklemeye yönelik ekonomik bir çözüm.

Azure disk yedekleme çözümü aşağıdaki senaryolarda kullanışlıdır:

- Uygulama quiescent olmadan gün başına sık sık yedeklemeler gerekir
- Bir küme senaryosunda çalışan uygulamalar: hem Windows Server yük devretme kümesi hem de Linux kümeleri paylaşılan disklere yazıyor
- Uygulama üzerinde güvenlik veya performans sorunları nedeniyle aracısız yedekleme için özel gereksinim
- İş kolu uygulamaları Birim Gölge Kopyası Hizmeti (VSS) desteklemediğinden, sanal makinenin uygulamayla tutarlı yedeklemesi uygulanabilir değildir.

Şu durumlarda Azure disk yedeklemesi 'ni göz önünde bulundurun:

- görev açısından kritik bir uygulama, kurtarma noktası hedefini karşılamak için günde birden çok yedekleme gerektiren bir Azure sanal makinesinde çalışıyor, ancak üretim ortamını veya uygulama performansını etkilemeden
- Kuruluşunuz veya sektör Yönetmeliği, güvenlik sorunları nedeniyle aracıları yüklemeyi kısıtlıyor
- uygulamayla tutarlı yedekleme sağlamak için özel ön ve son betik yürütme ve Linux sanal makinelerinde dondurma ve çözme işlemini çağırma, üretim iş yükü kullanılabilirliği üzerinde vadesi dolan ek yükü
- Azure Kubernetes Service (AKS kümesi) üzerinde çalışan Kapsayıcılı uygulamalar, kalıcı depolama olarak yönetilen diskleri kullanıyor. Bugün yönetilmesi zor olan otomasyon betikleri aracılığıyla yönetilen diski yedeklemeniz gerekir.
- yönetilen bir disk, bir dosya paylaşma olarak kullanılan veya veritabanı yedekleme dosyalarını içeren kritik iş verilerini tutuyor ve Azure VM yedeklemesine yatırım yaparak yedekleme maliyetini iyileştirmek istiyorsunuz
- Çok sayıda Linux ve Windows tek disksiz sanal makineniz (yani, yalnızca bir işletim sistemi diskine ve veri diskine bağlı olmayan bir sanal makine), Web sunucusu veya eyalet-daha az makineler barındırmakta ya da uygulama yapılandırma ayarları ile bir hazırlama ortamı görevi görür ve işletim sistemi diskini korumak için uygun maliyetli bir yedekleme çözümüne ihtiyacınız vardır. Örneğin, sanal makineyi yükseltmeden veya düzeltme eki uygulamadan önce hızlı bir isteğe bağlı yedekleme tetiklenmesi için
- bir sanal makine, Azure VM yedekleme çözümü tarafından desteklenmeyen bir işletim sistemi yapılandırması çalıştırıyor (örneğin, Windows 2008 32-bit sunucu)

## <a name="how-the-backup-and-restore-process-works"></a>Yedekleme ve geri yükleme işleminin nasıl çalıştığı

- Azure yönetilen diskler için yedeklemeyi yapılandırmanın ilk adımı bir [Yedekleme Kasası](backup-vault-overview.md)oluşturuyor. Kasa, farklı iş yükleri genelinde yapılandırılmış yedeklemelerin birleştirilmiş bir görünümünü sunar.

- Ardından yedekleme sıklığını ve saklama süresini yapılandırmanıza olanak tanıyan bir yedekleme ilkesi oluşturun.

- Yedeklemeyi yapılandırmak için yedekleme kasasına gidin, bir yedekleme ilkesi atayın, yedeklenmesi gereken yönetilen diski seçin ve anlık görüntülerin depolanacağı ve yönetildiği bir kaynak grubu sağlayın. Azure Backup, yedekleme sıklığına göre diskin artımlı bir anlık görüntüsünü oluşturan zamanlanmış yedekleme işlerini otomatik olarak tetikler. Daha eski anlık görüntüler, yedekleme ilkesi tarafından belirtilen saklama süresine göre silinir.

- Azure Backup yönetilen diskin [Artımlı anlık görüntülerini](../virtual-machines/disks-incremental-snapshots.md#restrictions) kullanır. Artımlı anlık görüntüler, son anlık görüntüden itibaren diskte delta değişiklikleri için faturalandırılan yönetilen disklerin uygun maliyetli, zaman içinde bir yedeklemesinden oluşur. Bunlar her zaman, ana disklerin depolama türünden bağımsız olarak standart HDD depolaması olan en düşük maliyetli depolama alanı üzerinde depolanır. Diskin ilk anlık görüntüsü, diskin kullanılan boyutunu kaplayacaktır ve son anlık görüntüden bu yana arka arkaya Artımlı anlık görüntüler depolama Delta değişikliklerini diskte saklar.

- Yönetilen bir diskin yedeklemesini yapılandırdıktan sonra, yedekleme kasasında bir yedekleme örneği oluşturulur. Yedekleme örneğini kullanarak, yedekleme işlemlerinin sistem durumunu bulabilir, isteğe bağlı yedeklemeleri tetikleyebilir ve geri yükleme işlemleri yapabilirsiniz. Ayrıca, tek bir cam görünümü bölmesi sağlayan yedekleme merkezi 'ni kullanarak birden çok kasada ve yedekleme örneklerinde yedeklemelerin sistem durumunu görüntüleyebilirsiniz.

- Geri yüklemek için, diski geri yüklemek istediğiniz kurtarma noktasını seçmeniz yeterlidir. Geri yüklenen diskin anlık görüntüden oluşturulacağı kaynak grubunu belirtin. Azure Backup, anlık görüntüler aboneliğinizde yerel olarak depolandığından bir anında geri yükleme deneyimi sağlar.

- Yedekleme Kasası, diğer Azure kaynaklarına erişmek için yönetilen kimlik kullanır. Yönetilen bir diskin yedeklemesini yapılandırmak ve geçmiş yedeklemeden geri yüklemek için, yedekleme kasasının yönetilen kimliği, kaynak diskte bir izin kümesi, anlık görüntülerin oluşturulduğu ve yönetildiği anlık görüntü kaynak grubu ve yedeklemeyi geri yüklemek istediğiniz hedef kaynak grubu gerektirir. Azure rol tabanlı erişim denetimi (Azure RBAC) kullanarak yönetilen kimliğe izin verebilirsiniz. Yönetilen kimlik, yalnızca Azure kaynaklarıyla kullanılabilecek özel bir türün hizmet sorumlusundan oluşur. [Yönetilen kimlikler](../active-directory/managed-identities-azure-resources/overview.md)hakkında daha fazla bilgi edinin.

- Şu anda Azure disk yedekleme, yönetilen disklerin işletimsel yedeklemesini destekler ve yedeklemeleri Yedekleme Kasası depolamasına kopyalamaz. Desteklenen ve desteklenmeyen senaryoların ve bölge kullanılabilirliğinin ayrıntılı bir listesi için [destek matrisine](disk-backup-support-matrix.md)bakın.

## <a name="pricing"></a>Fiyatlandırma

Azure Backup, Azure disklerini korumak için bir anlık görüntü yaşam döngüsü yönetimi çözümü sunar. Azure Backup tarafından oluşturulan disk anlık görüntüleri, Azure aboneliğinizdeki kaynak grubunda depolanır ve **anlık görüntü depolama** ücretleri uygulanır. Anlık görüntü fiyatlandırması hakkında daha fazla bilgi için [yönetilen disk fiyatlandırmasını](https://azure.microsoft.com/pricing/details/managed-disks/) ziyaret edebilirsiniz. Anlık görüntüler yedekleme kasasına kopyalanmadığından, Azure Backup **korunan bir örnek** ücreti ödemez ve **yedekleme depolama** maliyeti uygulanmaz. Ayrıca, artımlı anlık görüntüler son anlık görüntüden sonra Delta değişikliklerini kaplar ve üst yönetilen disklerin depolama türünden bağımsız olarak her zaman standart depolamada depolanır ve standart depolama fiyatlandırmasına göre ücretlendirilir. Bu, Azure disk yedeklemesini uygun maliyetli bir çözüm yapar.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Disk Yedekleme destek matrisi](disk-backup-support-matrix.md)
