---
title: StorSimple 'tan Azure Dosya Eşitleme geçirin
author: fauhse
ms.service: storage
ms.topic: conceptual
ms.date: 11/26/2019
ms.author: fauhse
ms.subservice: files
description: StorSimple, yaşam sonu ürünüdür ve Azure Dosya Eşitleme geçirilecek çözümdür. Geçiş kavramı hakkında bilgi edinin ve özelleştirilmiş geçiş yardımı için AzureFiles@microsoft.com 'a ulaşın.
ms.openlocfilehash: 1cc88080522a62085d9a515223512ef25c20a9e4
ms.sourcegitcommit: 8bd85510aee664d40614655d0ff714f61e6cd328
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/06/2019
ms.locfileid: "74895088"
---
# <a name="storsimple-migration-to-azure-file-sync"></a>Azure Dosya Eşitleme 'e StorSimple geçişi

StorSimple, kullanımdan kaldırılmıştır bir Microsoft ürünüdür. Bu ürün ve bulut hizmeti için genişletilmiş destek, 2022 sonuna kadar son olacak.
StorSimple 'tan doğrudan geçiş için planlamaya başlamak önemlidir.

Varsayılan ve stratejik uzun vadeli Azure hizmeti StorSimple cihazları ' a geçirilebilir, Azure Dosya Eşitleme. StorSimple üzerinde bir süper özellik kümesine sahip genel kullanıma açık bir Azure hizmetidir.

## <a name="full-cloud-side-migration-with-limited-downtime"></a>Sınırlı kapalı kalma süresine sahip tam bulut tarafı geçişi
Bu makalede, bir geçişin nasıl ayarlanacağı kavramı açıklanır.
Kullanıcıların, StorSimple ve üzerinde geçiş yapmak için gereken Azure Dosya Eşitleme, kendi kendine devam etmek zorunda olmadığı unutulmamalıdır.

> [!IMPORTANT]
> Microsoft, müşterilerine kendi geçişlerinde yardımcı olmaya kararlıdır. Özelleştirilmiş bir geçiş planı için e-posta AzureFiles@microsoft. com ve geçiş sırasında yardım.

## <a name="migration-approach"></a>Geçiş yaklaşımı
Azure Dosya Eşitleme geçişi, şirket içi ve sınırlı kapalı kalma süresine en az etkiyle bulut tarafında çalışır.
Aşağıdaki kavram StorSimple 8000 serisi gereçlerine yöneliktir.
StorSimple 7000 serisinden geçiş yapmanız gerekiyorsa, ilk adım Microsoft 'tan eşleşen bir 8000 serisi ödünç veren cihazına ücretsiz yükseltmeyi gerektirir.
AzureFiles@microsoft.com ulaşmak için, uygun sayıda ödünç verilen cihaz düzenlemenize yardımcı olacak.

### <a name="general-approach"></a>Genel yaklaşım
![Alternatif](media/storage-sync-files-storsimple-migration/storsimple-docs-overview-concept.png "Şirket içi StorSimple gerecini değiştirerek, geçici bir Sanal Gereç ve Windows Server aracılığıyla bulut tarafı geçişini yeni bir şirket içi Windows Server 'a göstermek")

1. Şirket içi StorSimple gerecinizin bir birim kopyasını alın ve bunu geçici bir StorSimple Sanal gerecine bağlayın.
2. Sanal Gereci Iscsı aracılığıyla geçici bir Azure VM 'sine bağlayın.
3. Azure Dosya Eşitleme geçici Windows Server VM 'sine yüklemek-bu geçiş için belirli bir kayıt defteri anahtarının Ayrıca, sunucuda eşitleme yapılandırıldıktan sonra ayarlanması gerekir.
    * StorSimple biriminizdeki paylaşımların sayısına bağlı olarak, çok sayıda Azure dosya paylaşımı olarak dağıtın. (Depolama hesabı başına bir Azure dosya paylaşımının dağıtılmasını öneririz.)
    * Windows Server bulut VM 'de ve bir Azure dosya paylaşımında bireysel bir paylaşımdan eşitleme yapılandırın. (1:1 eşleme)
    * İsteğe bağlı olarak, bulut katmanlaması etkinken bir yerel sunucuyu şirket içi performans önbelleği olarak ekleyin. Şirket içi StorSimple 'ı, Windows Server ve Azure Dosya Eşitleme bulut katmanlaması tarafından desteklenen daha zengin, yerel bir önbellek ile değiştirmek istiyorsanız bu adım gereklidir. Şirket içi Windows Server 'niz bir fiziksel makine veya küme ya da bir sanal makine olabilir. Veri kümesi boyutu olarak dağıtılmış kadar depolama alanı olması gerekmez. Yalnızca en sık erişilen dosyaları yerel olarak önbelleğe almak için yeterli depolama alanı gerektirir.

## <a name="minimizing-downtime"></a>Kesinti süresini en aza indirme
Yukarıdaki 3. adımdan sonra, StorSimple şirket içi gereç, kullanıcılar ve uygulamalar tarafından etkin bir şekilde kullanılmaya devam etmektedir. Bu nedenle, ilk birim Klondan eşitlenen dosyalar kümesi eşitleme tamamlandığında biraz daha güncel değildir.
Kapalı kalma süresini en aza indirme yaklaşımı, her yinelemede eşitlemenin daha hızlı ve daha hızlı tamamlanır ve bu da birim klonları arasındaki değişiklikler tarafından etkin hale getirilir.
Bir birim kopyasının eşitlenmesi, kapalı kalma süresi için kabul edilebilir bulduğunuz sürede bitebileceğiniz sürece bu işlemi yineleyebilirsiniz.
Bu durumda, kullanıcıların ve uygulamaların StorSimple gerecinizde herhangi bir değişiklik yapmasını engelleyin. Kapalı kalma süresi başlar.
Başka bir birim kopyası alın ve sunucuya bağlı olan sunucu (ler) e eşitlemeye izin verin.
Kullanıcılarınızın ve uygulamalarınızın yeni, Azure Dosya Eşitleme desteklenen, Windows Server 'larınızla erişmesini sağlayın.
Eski StorSimple gerecinden uygulama ve kullanıcılara şeffaf yeni Windows Server 'a daha fazla yük açmak için bir DFS ad alanı dağıtımı/ayarlamayı düşünün.
Geçişiniz tamamlanmıştır.

## <a name="migration-goal"></a>Geçiş hedefi
Geçiş işlemi tamamlandıktan sonra, geçici StorSimple Sanal Gereci ve Azure VM 'nin sağlaması geri alınabilir.

Ayrıca, Kullanıcı ve uygulamalarınız bunun yerine Windows Server 'a zaten eriştiğinden StorSimple şirket içi gerecinin sağlanması kaldırılabilir.
İle birlikte kalan özellikler aşağıdaki görüntüde gösterilmiştir. Standart bir Azure Dosya Eşitleme dağıtımı, bir dizi Azure dosya paylaşımını ve bunlara bağlı Windows Server 'ı Azure Dosya Eşitleme aracılığıyla sunar. Tek bir sunucunun farklı yerel klasörleri aynı anda farklı dosya paylaşımlarına bağlayabileceği unutulmamalıdır.
Ayrıca, bir Azure dosya paylaşımının birçok farklı sunucuya eşitlenmesi ve bu durumda şube ofislerde önbelleğe alınmış veriler olması gerekir. Ayrıca, şirket içi depolama alanınızı daha verimli bir şekilde kullanmak için bulut katmanlama ilkelerinizi iyileştirebileceğinizi kontrol edin.

![Alternatif](media/storage-sync-files-storsimple-migration/storsimple-docs-goal.PNG "Geçiş tamamlandıktan sonra hedefi gösteren bir çizim. Bulutta veya Windows Server 'da dosyalara erişen kullanıcılar ve uygulamalar ile şirket içi Windows Server ile eşitlenen birçok dosya paylaşımını gösterir.")

## <a name="next-steps"></a>Sonraki adımlar
Azure dosyaları ve Azure Dosya Eşitleme hakkında bilgi edinin. Başarılı bir geçiş için Azure Dosya Eşitleme terminoloji ve dağıtım deseninin anlaşılması önemlidir. Bu genel bakış makalesindeki her adım için daha ayrıntılı bilgi bulabilirsiniz. Geçişinizi planlama ve yürütme sırasında özelleştirilmiş yardım için Microsoft 'a ulaştığınızdan emin olun.

> [!IMPORTANT]
> Microsoft, müşterilerine kendi geçişlerinde yardımcı olmaya kararlıdır. Özelleştirilmiş bir geçiş planı için e-posta AzureFiles@microsoft. com ve geçiş sırasında yardım.

## <a name="additional-resources"></a>Ek kaynaklar
Hedef hizmet olarak Azure Dosya Eşitleme, Azure Dosya Eşitleme yeni bir deyişle, okumanız önerdiğimiz iki temel belge vardır.
* [Azure Dosya Eşitleme-genel bakış](storage-sync-files-planning.md)
* [Azure Dosya Eşitleme dağıtım kılavuzu](storage-sync-files-deployment-guide.md)

Azure dosyaları, Azure 'daki bir hizmet olarak dosya paylaşımları sunan bir depolama hizmetidir. VM veya ilişkili VM depolaması için ödeme yapma veya sürdürme gereksinimi yoktur.
* [Azure dosyaları-genel bakış](storage-files-introduction.md)
* [Azure dosyaları-Azure dosya paylaşımının dağıtımı](storage-how-to-create-file-share.md)