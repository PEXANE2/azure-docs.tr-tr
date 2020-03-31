---
title: StorSimple 5000-7000 serisi cihazlardan veri geçiş seçenekleri
description: StorSimple 5000-7000 serisinden veri geçirme seçeneklerine genel bir bakış sağlar.
services: storsimple
documentationcenter: NA
author: alkohli
manager: twooley
ms.assetid: ''
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 04/15/2019
ms.author: alkohli
ms.openlocfilehash: 44a05ecb273bdf7582300c3b6a9110e2ada0994c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77471831"
---
# <a name="options-to-migrate-data-from-storsimple-5000-7000-series"></a>StorSimple 5000-7000 serisindeki verileri geçirme seçenekleri 

> [!IMPORTANT]
> 9 Temmuz 2019'da StorSimple 5000/7000 serisi destek (EOS) durumuna ulaşacak. StorSimple 5000/7000 serisi müşterilerin belgede açıklanan alternatiflerden birine geçiş yapmanızı öneririz.

StorSimple 5000-7000 serisi Temmuz 2019'da [Desteğin sonuna](https://support.microsoft.com/lifecycle/search?alpha=StorSimple%205000%2F7000%20Series) ulaşıyor. StorSimple 5000-7000 serisini çalıştıran müşterilerin diğer Azure birinci taraf karma hizmetlerine yükseltme seçeneği vardır. Bu makalede, verileri geçirmek için kullanılabilir Azure karma seçenekleri açıklanmaktadır. 

## <a name="migration-options"></a>Geçiş seçenekleri

StorSimple 5000-7000 serisini kullanan müşterilerin Azure veya üçüncü taraf seçenekleri var.

### <a name="azure-options"></a>Azure seçenekleri

#### <a name="upgrade-to-storsimple-8000-series"></a>StorSimple 8000 Serisine Yükseltin

StorSimple 8000 serisine yükseltin ve böylece StorSimple platformunda devam edin.  Bu yükseltme yolu, müşterilerin 5000-7000 serisi aygıtlarını 8000 serisiyle değiştirmelerini gerektirir. Veriler, geçiş aracı kullanılarak 5000-7000 serisi aygıttan geçirilir. Geçiş başarıyla tamamlandıktan sonra, StorSimple 8000 serisi aygıtlar verileri Azure Blob Depolama'ya katmanlamaya devam edecektir. 

StorSimple 8000 serisini kullanarak verilerin nasıl geçirilen hakkında daha fazla bilgi [için, StorSimple 5000-7000 serisinden 8000 serisi aygıta veri geçirin.](storsimple-8000-migrate-from-5000-7000.md)

#### <a name="migrate-to-azure-file-sync"></a>Azure Dosya Eşitleme’ye Geçiş

Bu yepyeni geçiş seçeneği, müşterilerin kuruluşlarının dosya paylaşımlarını Azure Dosyalarında depolamalarına olanak tanır. Bu dosya paylaşımları daha sonra Azure Dosya Eşitlemi (AFS) kullanılarak şirket içi erişim için merkezileştirilir. AFS bir Windows Server ana bilgisayarda dağıtılabilir. Gerçek veri geçişi daha sonra ana bilgisayar kopyası olarak veya geçiş aracı kullanılarak gerçekleştirilir.

Verileri Azure Dosya Eşitleme'ye nasıl geçirilir hakkında daha fazla bilgi [için, StorSimple 5000-7000 serisindeki verileri Azure Dosya Eşitle'ye geçirin.](storsimple-5000-7000-afs-migration.md)

### <a name="third-party-options"></a>Üçüncü taraf seçenekleri

#### <a name="migrate-to-panzura-freedom-nas"></a>Panzura Özgürlük NAS'a göç edin

StorSimple 5000-7000 müşterileri verilerini Azure'da tutmak için Panzura Freedom NAS'a geçiş yapmayı seçebilir. Panzura Freedom çözümü, veri merkezlerini, ofisleri, genel ve özel bulutları kapsayan bir NAS çözümü sağlar. Çözüm, NFS, SMB ve mobil istemciler için yerel, karma ve bulut içi veri iş akışlarısağlar. 

Bu göç Panzura tarafından desteklenir ve müşteriler [Panzura web sitesinden](https://panzura.com/storsimple-migration/)göç desteği isteyerek başlatabilirsiniz.

#### <a name="migrate-to-cohesity"></a>Birlikteliğe geçiş

Tutarlılık, verileri mevcut StorSimple 5000-7000'inizden Azure'daki Uygunluk Veri Platformu'na geçirmenize olanak tanır. Uygunluk Veri Platformu, dosyaları, yedeklemeleri, nesneleri ve VM'leri tek bir bulut tabanlı çözümde birleştiren yazılım tanımlı web ölçeğinde bir çözümdür. Veri Platformu'na geçtikten sonra, tek bir cam bölmearacılığıyla verileri ve uygulamaları buluttan çeki doğru yönetebilir, koruyabilir ve sağlayabilirsiniz. Cohesity ile, üç düğüm gibi az ile başlayın. 

[Uygunluk Veri Platformu'na geçiş](https://info.cohesity.com/migrate-from-storsimple-to-cohesity.html)hakkında daha fazla bilgi edinin.

#### <a name="migrate-to-nasuni"></a>Nasuni'ye göç edin

Nasuni, StorSimple 5000-7000 müşterilerinin geçiş ini ve verilerini Azure'da tutmasını kolaylaştırır.  Nasuni, bulut ekonomisi ve ölçekle müşterilerine ön hazırlık çözümlerinden bekledikleri performansı ve güvenliği sağlayan, Azure tabanlı önde gelen bir NAS depolama çözümüdür.  Yüksek performanslı dosya depolamasına ek olarak, Nasuni ve Azure yedekleme ve DR'yi ele alırken, merkezi dosya depolama yönetimiyle dünya çapındaki verilerinizi paylaşmanıza ve bunlar üzerinde işbirliği yapmanıza olanak tanır. 

Nasuni, göçükolaylaştıracak deneyime sahiptir – bugün başlayın:https://info.nasuni.com/nasuni-storsimple-migration

#### <a name="migrate-to-talon-fast"></a>Talon FAST'a göç et

Talon, StorSimple 5000-7000 müşterilerinin, daha büyük bir işleve sahip Olan StorSimple platformunda (sınırsız bulut kaynakları tarafından desteklenen küçük yerinde ayak izi) çok değer verdikleri avantajlardan yararlanmaya devam etmesini kolaylaştırır.  Talon FAST çözümü yle müşteriler, yalnızca yazılımalanında daha küçük bir web izine sahip olurken ve küresel dosya kilitleme, genel ad alanı ve çok siteli işbirliği gibi avantajlar eklerken, verilerini Azure'da geçirip tutabilirler.  Talon, şirket içi dosya sunucusu iş yüklerini kullanıcı iş akışından veya deneyimden ödün vermeden konsolide, Azure tabanlı bir ayak izine geçirmek için küresel müşterilerle birlikte çalışan lider bir Azure ekosistem çözümüdür.  

Bulutlarla birleştirilmiş kuruluşa nasıl evrececeğiniz https://www.talonstorage.com/alliances/microsoft-storsimplehakkında daha fazla bilgi edinin.


## <a name="migration---frequently-asked-questions"></a>Geçiş - Sık sorulan sorular

### <a name="q-when-do-the-storsimple-5000-and-7000-series-devices-reach-end-of-service"></a>S. StorSimple 5000 ve 7000 serisi cihazlar ne zaman hizmet sona erer? 

A. StorSimple 5000-7000 serisi Temmuz 2019'da [hizmete son veriyor.](https://support.microsoft.com/lifecycle/search?alpha=StorSimple%205000%2F7000%20Series) Hizmetin sona ermesi, Microsoft'un Temmuz 2019'dan sonra bu aygıtların hem donanımı hem de yazılımı için artık destek sağlayamadığı anlamına gelir. Aygıtlarınızdan verileri geçirmek için bir plan oluşturmaya başlamanızı öneririz.

### <a name="q-what-happens-to-the-data-i-have-stored-in-azure"></a>S. Azure'da depolamış olduğum verilere ne olur?  

A. Verileri daha yeni bir hizmete aktardıktan sonra Azure'da kullanmaya devam edebilirsiniz. 


### <a name="q-what-happens-to-the-data-i-have-stored-locally-on-my-storsimple-device"></a>S. StorSimple aygıtımda yerel olarak depolamış olduğum verilere ne olur? 

A. Yerel aygıttaki veriler, geçiş belgelerinde açıklandığı şekilde yeni hizmete kopyalanabilir.

### <a name="q-what-happens-if-i-want-to-keep-my-storsimple-50007000-series-appliance"></a>S. StorSimple 5000/7000 serisi cihazımı saklamak istersem ne olur? 

A. Hizmetler çalışmaya devam etse de, Microsoft artık donanım ve yazılım desteği sağlayamaz. Geçiş, iş sürekliliği için şiddetle tavsiye edilir.

### <a name="q-what-options-are-available-to-migrate-data-from-storsimple-5000-7000-series-devices"></a>S. StorSimple 5000-7000 serisi cihazlardan veri geçirmek için hangi seçenekler kullanılabilir? 

A. Senaryolarına bağlı olarak, StorSimple 5000-7000 serisi kullanıcıları aşağıdaki geçiş seçeneklerine sahiptir. 

 - **8000 serisine yükseltme**: StorSimple platformunda devam etmek istediğinizde bu seçeneği kullanın. 
 - **Azure Dosya Eşitleme:** Azure yerel biçimine geçmek istediğinizde bu seçeneği kullanın. Dosya paylaşımlarının merkezi yönetimi için Azure Dosya Eşitlemeyi'ni kullanabilirsiniz. 

Burada listelenmeyen geçiş seçeneklerini tartışmak için Microsoft Destek'e başvurabilirsiniz.

### <a name="q-is-migration-to-other-storage-solutions-supported"></a>S. Diğer depolama çözümlerine geçiş desteklenir mi?

A. Evet. Verilerin ana bilgisayar kopyasını kullanarak diğer depolama çözümlerine geçiş desteklenir.

### <a name="q-is-migration-supported-by-microsoft"></a>S. Geçiş Microsoft tarafından desteklenir mi? 

A. 5000 veya 7000 serisinden geçiş tam olarak desteklenen bir işlemdir. Aslında, Microsoft geçiş başlamadan önce Destek'e ulaşmanızı önerir. Geçiş şu anda yardımcı bir işlemdir. StorSimple 5000-7000 serisi cihazınızdaki verileri aktarmak istiyorsanız, [Destek bileti açın.](storsimple-8000-contact-microsoft-support.md)

### <a name="q-what-is-the-pricing-model-for-both-the-migration-options"></a>S. Her iki geçiş seçeneği için fiyatlandırma modeli nedir?

A. Geçiş maliyeti seçtiğiniz seçene bağlı olarak değişir. Geçişin kendisi ücretsiz olsa da, StorSimple 8000 serisine yükseltmeye karar verirseniz, donanım aygıtının maliyeti olacaktır. 

Benzer şekilde, Azure Dosya Eşitlemeyi kullanırken, hizmetin abonelik ücretleri de geçerli olabilir. Her durumda, müşteriler de devam eden depolama maliyetleri ödemek zorunda kalacaktır. Bir tahmin için aşağıdakilere bakın: 
- [StorBasit fiyatlandırma](https://azure.microsoft.com/pricing/details/storsimple/)  
- [AFS fiyatlandırması]( https://azure.microsoft.com/pricing/details/storage/files/)

### <a name="q--how-long-does-it-take-to-complete-a-migration"></a>S.  Bir geçişi tamamlamak ne kadar sürer?

A. Verileri geçirme süresi, veri miktarına ve seçilen yükseltme seçeneğine bağlıdır. 

### <a name="q-what-is-the-end-of-support-date-for-storsimple-8000-series"></a>S. StorSimple 8000 serisi için Son Destek tarihi nedir?

A. StorSimple 8000 serisi için Destek Bitiş tarihi [burada](https://support.microsoft.com/lifecycle/search?alpha=Azure%20StorSimple%208000%20Series)yayınlanır.


## <a name="next-steps"></a>Sonraki adımlar
 - [StorSimple 5000-7000 serisindeki verileri 8000 serisi bir aygıta geçirin.](storsimple-8000-migrate-from-5000-7000.md)
 - [StorSimple 5000-7000 serisindeki verileri Azure Dosya Eşitleme'sine geçirin](storsimple-5000-7000-afs-migration.md)
