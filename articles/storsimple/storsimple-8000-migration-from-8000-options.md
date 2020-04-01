---
title: StorSimple 8000 serisi cihazlardan veri geçiş seçenekleri
description: StorSimple 8000 serisinden veri geçirme seçeneklerine genel bir bakış sağlar.
services: storsimple
author: priestlg
ms.service: storsimple
ms.topic: article
ms.date: 03/25/2020
ms.author: v-grpr
ms.openlocfilehash: f712486c8035bbc913fbd229759f3415a1005449
ms.sourcegitcommit: ced98c83ed25ad2062cc95bab3a666b99b92db58
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/31/2020
ms.locfileid: "80438326"
---
# <a name="options-to-migrate-data-from-storsimple-8000-series"></a>StorSimple 8000 serisinden veri geçirme seçenekleri

> [!IMPORTANT]
> 31 Aralık 2022'de StorSimple 8000 serisi destek sonu (EOS) durumuna ulaşacak. StorSimple 8000 serisi müşterilerinin belgede açıklanan alternatiflerden birine geçiş lerini öneririz.

StorSimple 8000 serisi Aralık 2022'de [Desteğin sonuna](https://support.microsoft.com/lifecycle/search?alpha=Azure%20StorSimple%208000%20Series) ulaşıyor. StorSimple 8000 serisini çalıştıran müşterilerin diğer Azure birinci taraf karma hizmetlerine yükseltme seçeneği vardır. Bu makalede, verileri geçirmek için kullanılabilir Azure karma seçenekleri açıklanmaktadır.

## <a name="migration-options"></a>Geçiş seçenekleri

StorSimple 8000 serisini kullanan müşterilerin Azure veya üçüncü taraf seçenekleri var.

### <a name="azure-options"></a>Azure seçenekleri

#### <a name="migrate-to-azure-file-sync"></a>Azure Dosya Eşitleme’ye Geçiş

Bu yepyeni geçiş seçeneği, müşterilerin kuruluşlarının dosya paylaşımlarını Azure Dosyalarında depolamalarına olanak tanır. Bu dosya paylaşımları daha sonra Azure Dosya Eşitlemi (AFS) kullanılarak şirket içi erişim için merkezileştirilir. AFS bir Windows Server ana bilgisayarda dağıtılabilir. Gerçek veri geçişi daha sonra ana bilgisayar kopyası olarak veya geçiş aracı kullanılarak gerçekleştirilir.

Verileri Azure Dosya Eşitleme'ye nasıl geçirilir hakkında daha fazla bilgi [için, StorSimple 8100 ve 8600 geçişini Azure Dosya Eşitle'ye](https://docs.microsoft.com/azure/storage/files/storage-files-migration-storsimple-8000)gidin.

<!-- 03/25/2020 v-grpr (priestlg) - As per request, commenting out this section because the information that will go into this section is forthcoming
### Third-party options

#### Migrate to Panzura Freedom NAS

StorSimple 5000-7000 customers can choose to migrate to Panzura Freedom NAS to keep their data in Azure. Panzura Freedom solution provides a NAS solution that spans datacenters, offices, public and private clouds. The solution enables local, hybrid, and in-cloud data workflows for NFS, SMB, and mobile clients. 

This migration is supported by Panzura and customers can get started by requesting migration support from the [Panzura website](https://panzura.com/storsimple-migration/).

#### Migrate to Cohesity

Cohesity enables you to migrate data from your current StorSimple 5000–7000 to the Cohesity Data Platform on Azure. The Cohesity Data Platform is a software-defined web-scale solution that consolidates files, backups, objects, and VMs onto a single cloud-native solution. After migration to the Data Platform, you can manage, protect, and provision data and apps from cloud to core through a single pane of glass. With Cohesity, start with as few as three nodes. 

Learn more on [migration to the Cohesity Data Platform](https://info.cohesity.com/migrate-from-storsimple-to-cohesity.html).

#### Migrate to Nasuni

Nasuni makes it easy for StorSimple 5000-7000 customers to migrate and keep their data in Azure.  Nasuni is a leading Azure-based NAS storage solution, giving customers the performance and security they expect from on-prem solutions, with cloud economics and scale.  In addition to high performance file storage, Nasuni and Azure handle backup and DR, while allowing you to share and collaborate on your data around the globe with centralized file storage management. 

Nasuni has the experience to make your migration easy – get started today: https://info.nasuni.com/nasuni-storsimple-migration

#### Migrate to Talon FAST

Talon makes it easy for StorSimple 5000-7000 customers to continue to leverage the benefits they valued so much in the StorSimple platform (small on-site footprint backed by unlimited cloud resources) with even greater function.  With the Talon FAST solution, customers can migrate and keep their data in Azure, while now having an even smaller software-only onsite footprint and adding benefits such as global file locking, global namespace, and multi-site collaboration.  Talon is a leading Azure ecosystem solution, working with global customers to migrate their on-premises file server workloads into a consolidated, Azure-based footprint without compromising user workflow or experience.  

Learn more about how to evolve to a cloud-consolidated enterprise at https://www.talonstorage.com/alliances/microsoft-storsimple.
-->

## <a name="migration---frequently-asked-questions"></a>Geçiş - Sık sorulan sorular

### <a name="q-when-do-the-storsimple-8000-series-devices-reach-end-of-service"></a>S. StorSimple 8000 serisi cihazlar hizmet sonu ne zaman ulaşır?

A. StorSimple 8000 serisi Aralık 2022'de [desteğin sonuna](https://support.microsoft.com/[lifecycle/search?alpha=Azure%20StorSimple%208000%20Series) ulaştı. Desteğin sona ermesi, Microsoft'un Aralık 2022'den sonra bu aygıtların hem donanımı hem de yazılımı için artık destek sağlayamadığı anlamına gelir. Aygıtlarınızdan verileri geçirmek için bir plan oluşturmaya başlamanızı öneririz.

### <a name="q-what-happens-to-the-data-i-have-stored-in-azure"></a>S. Azure'da depolamış olduğum verilere ne olur?  

A. Verileri daha yeni bir hizmete aktardıktan sonra Azure'da kullanmaya devam edebilirsiniz.

### <a name="q-what-happens-to-the-data-i-have-stored-locally-on-my-storsimple-device"></a>S. StorSimple aygıtımda yerel olarak depolamış olduğum verilere ne olur?

A. Yerel aygıttaki veriler, geçiş belgelerinde açıklandığı şekilde yeni hizmete kopyalanabilir.

### <a name="q-what-happens-if-i-want-to-keep-my-storsimple-8000-series-appliance"></a>S. StorSimple 8000 serisi cihazımı saklamak istersem ne olur?

A. Hizmetler çalışmaya devam etse de, Microsoft artık donanım ve yazılım desteği sağlayamaz. Geçiş, iş sürekliliği için şiddetle tavsiye edilir.

### <a name="q-what-options-are-available-to-migrate-data-from-storsimple-8000-series-devices"></a>S. StorSimple 8000 serisi cihazlardan veri geçirmek için hangi seçenekler kullanılabilir?

A. Senaryolarına bağlı olarak, StorSimple 8000 serisi kullanıcıları aşağıdaki geçiş seçeneklerine sahiptir:

* **Azure Dosya Eşitleme:** Azure yerel biçimine geçmek istediğinizde bu seçeneği kullanın. Dosya paylaşımlarının merkezi yönetimi için Azure Dosya Eşitlemeyi'ni kullanabilirsiniz.

* **Diğer seçenekler**: Burada listelenmeyen geçiş seçeneklerini tartışmak için Microsoft Destek'e başvurabilirsiniz.

### <a name="q-is-migration-to-other-storage-solutions-supported"></a>S. Diğer depolama çözümlerine geçiş desteklenir mi?

A. Evet. Verilerin ana bilgisayar kopyasını kullanarak diğer depolama çözümlerine geçiş desteklenir.

### <a name="q-is-migration-supported-by-microsoft"></a>S. Geçiş Microsoft tarafından desteklenir mi?

A. 8000 serisinden geçiş tam olarak desteklenen bir işlemdir. Aslında, Microsoft geçiş başlamadan önce Destek'e ulaşmanızı önerir. Geçiş şu anda yardımcı bir işlemdir. StorSimple 8000 serisi cihazınızdan veri aktarmak istiyorsanız, [StorSimple desteğine başvurun.](mailto:storsimp@microsoft.com)

### <a name="q-what-is-the-pricing-model-for-migration-to-azure-file-sync"></a>S. Azure Dosya Eşitlemi için fiyatlandırma modeli nedir?

A. Azure Dosya Eşitlemeyi kullanırken, hizmetin abonelik ücretleri geçerli olabilir. Müşteriler ayrıca devam eden depolama maliyetlerini de ödemek zorunda kalacaktır. Bir tahmin için [AFS fiyatlandırmasına]( https://azure.microsoft.com/pricing/details/storage/files/) bakın.

### <a name="q-how-long-does-it-take-to-complete-a-migration"></a>S. Bir geçişi tamamlamak ne kadar sürer?

A. Verileri geçirme süresi, veri miktarına ve seçilen yükseltme seçeneğine bağlıdır.

## <a name="next-steps"></a>Sonraki adımlar

* [StorSimple 8000 serisindeki verileri Azure Dosya Eşitleme'ye geçirme](https://docs.microsoft.com/azure/storage/files/storage-files-migration-storsimple-8000)
