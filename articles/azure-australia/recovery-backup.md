---
title: Azure Avustralya 'da yedekleme ve olağanüstü durum kurtarma
description: ABD kamu kurumları için Microsoft Azure ' de yedekleme ve olağanüstü durum kurtarma, ASD 'nin önemli 8 ' i ile ilgilidir.
author: galey801
ms.service: azure-australia
ms.topic: conceptual
ms.date: 07/22/2019
ms.author: grgale
ms.openlocfilehash: 078918cbfeffb1ff5f3626b5add96bc6622a90a7
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/26/2019
ms.locfileid: "68571529"
---
# <a name="backup-and-disaster-recovery-in-azure-australia"></a>Azure Avustralya 'da yedekleme ve olağanüstü durum kurtarma

Destekleyici altyapıyı içeren yedekleme ve olağanüstü durum kurtarma planlarının tüm organizasyonlar için kritik olması önemlidir. Bir yedekleme çözümüne sahip olmanın önemi, [Avustralya Cyber Güvenlik Merkezi 'Nin önemli 8](https://acsc.gov.au/publications/protect/essential-eight-explained.htm)' ine dahil olarak vurgulanır.

Microsoft Azure, esnekliği etkinleştiren iki hizmet sağlar: Azure Backup ve Azure Site Recovery. Bu hizmetler, çeşitli tasarım senaryolarında hem şirket içindeki hem de buluttaki verilerinizi korumanıza olanak sağlar. Azure Backup ve Azure Site Recovery her ikisi de ortak bir depolama ve yönetim kaynağı kullanır: Azure kurtarma hizmetleri Kasası. Bu kasa, Azure Backup ve Azure Site Recovery verileri yönetmek, izlemek ve ayırt etmek için kullanılır.

Bu makalede, [Avustralya sinyalleri Directorate (asd) bilgi güvenliği el ile (ISM) denetimleriyle](https://acsc.gov.au/infosec/ism/index.htm)Azure Backup ve Azure Site Recovery uygulamak için anahtar tasarım öğeleri ayrıntılı olarak açıklanır.

## <a name="azure-backup"></a>Azure Backup

![Azure Backup](media/backup-overview.png)

Azure Backup geleneksel bir şirket içi yedekleme çözümüne benzer ve hem şirket içi hem de Azure 'da barındırılan verileri yedekleme olanağı sağlar. Azure Backup, Azure 'a aşağıdaki veri türlerini yedeklemek için kullanılabilir:

* Dosyalar ve klasörler
* Üzerinde barındırılan desteklenen Windows ve Linux işletim sistemleri:
  * Hyper-V ve VMWare hiper yöneticileri
  * Fiziksel donanım
* Desteklenen Microsoft uygulamaları

### <a name="azure-site-recovery"></a>Azure Site Recovery

![Azure Site Recovery](media/asr-overview.png)

Azure Site Recovery, tek bir sanal makineden ya da çok katmanlı uygulamalardan oluşan iş yüklerini çoğaltır. Çoğaltma, Şirket içinden Azure 'a, Azure bölgeleri arasında veya Azure Site Recovery tarafından düzenlenen şirket içi konumlar arasında desteklenir. Şirket içi sanal makineler, Azure 'da veya desteklenen şirket içi bir Hiper yöneticide çoğaltılabilir. Yapılandırıldıktan sonra çoğaltmayı, yük devredilmesini ve geri dönmesini Azure Site Recovery.

## <a name="key-design-considerations"></a>Önemli tasarım konuları

Bir yedekleme veya olağanüstü durum kurtarma çözümü uygularken önerilen çözümün göz önünde bulundurulması gerekir:

* Yakalanacak verilerin kapsamı ve hacmi
* Verilerin tutulacağı süre
* Bu veriler güvenli bir şekilde depolanır ve yönetilir
* Verilerin depolandığı coğrafi konumlar
* Rutin sistem testi yordamları

ISM, bir çözüm tasarlarken yapılması gereken güvenlik konuları hakkında rehberlik sağlar. Microsoft Azure, bu güvenlik konularını ele almak için araçlar sağlar.

### <a name="data-sovereignty"></a>Veri egemenlik

Kuruluşların, bulut tabanlı depolama konumlarını kullanıma sunan verileri karşılayan verilerin korunduğundan emin olması gerekir. Azure Ilkesi, bir Azure kaynağının oluşturulabildiği izin verilen konumları kısıtlamak için gereken noktaları sağlar. Yerleşik Azure Ilkesi "Izin verilen konumlar", atanmış bir Azure Ilkesinin kapsamında oluşturulan tüm Azure kaynaklarının aday coğrafi konumlarda oluşturulabilirse emin olmak için kullanılır.

Azure kaynakları için coğrafi kısıtlama için Azure Ilke öğeleri şunlardır:

* allowedLocations
* allowedSingleLocation

Bu ilkeler, Azure yöneticilerinin bir aday konumlar listesine veya tek coğrafi konum olarak oluşturulmasını kısıtlamalarına olanak tanır.

### <a name="redundant-and-geographically-dispersed-storage"></a>Yedekli ve coğrafi olarak dağınık depolama

Azure kurtarma hizmeti kasasında depolanan veriler her zaman yedekli depolama alanında depolanır. Varsayılan olarak, kurtarma hizmeti Kasası Azure coğrafi olarak yedekli depolama (GRS) kullanır. GRS kullanılarak depolanan veriler, kurtarma hizmeti kasasının [İkincil eşlenmiş bölgesindeki](https://docs.microsoft.com/azure/best-practices-availability-paired-regions)diğer Azure veri Centres 'e çoğaltılır. Bu çoğaltılan veriler salt okunurdur ve yalnızca bir Azure Yük devretme olayı varsa yazılabilir yapılır. Azure veri merkezi 'nde veriler ayrı hata etki alanları ve yükseltme etki alanları arasında çoğaltılır ve donanım veya bakım tabanlı kesinti olma olasılığını en aza yükseltir. GRS, yıllık en az% 99.99999999999999 kullanılabilirlik sağlar.

Azure kurtarma hizmetleri Kasası, yerel olarak yedekli depolama (LRS) kullanımı için yapılandırılabilir. LRS, düşük maliyetli bir depolama seçeneğidir. Bu artıklık modeli, ayrı hata etki alanları ve yükseltme etki alanları arasında aynı çoğaltmayı kullanır, ancak coğrafi bölgeler arasında çoğaltılmaz. LRS depolamada bulunan veriler, GRS olarak dayanıklı olmadığı sürece, aylık olarak en az% 99,999999999 kullanılabilirlik sağlar.

Bant medyası gibi geleneksel site dışı depolama teknolojilerinin aksine, verilerin ek kopyaları otomatik olarak oluşturulur ve ek bir yönetim yükü gerektirmez.

### <a name="restricted-access-and-activity-monitoring"></a>Kısıtlı erişim ve etkinlik izleme

Yedekleme verileri bozulma, değişiklik ve onaylanmamış silme işleminden korunmalıdır. Hem Azure Backup hem de Azure Site Recovery ortak Azure Yönetim dokusunu kullanır. Bu doku, Azure 'da bulunan kaynaklara ayrıntılı denetim, günlüğe kaydetme ve rol tabanlı Access Control (RBAC) sağlar. Yedekleme verilerine erişim, yönetim personeli seçerek kısıtlanabilir ve yedekleme verilerini içeren tüm eylemler günlüğe kaydedilebilir ve denetlenebilir.

Hem Azure Backup hem de Azure Site Recovery yerleşik günlüğe kaydetme ve raporlama özelliklerine sahiptir. Yedekleme veya çoğaltma sırasında oluşan tüm sorunlar, Azure Yönetim dokusunu kullanılarak yöneticilere bildirilir.

Azure kurtarma hizmetleri Kasası 'nda aşağıdaki ek veri güvenliği ölçüleri de vardır:

* Yedekleme verileri, silme işlemi gerçekleştirildikten sonra 14 gün boyunca tutulur
* "Verileri silme ile Yedeklemeyi Durdur" gibi kritik işlemlere yönelik uyarılar ve bildirimler
* Kritik işlemler için güvenlik PIN gereksinimleri
* En düşük bekletme aralığı denetimleri yerinde

Bu en düşük bekletme aralığı denetimleri şunları içerir:

* Günlük bekletme için en az yedi günlük bekletme
* Haftalık bekletme için en az dört haftalık saklama
* Aylık bekletme için en az üç ay bekletme
* Yıllık bekletme için en az bir yıllık saklama

Azure 'da depolanan tüm yedekleme verileri, Azure 'ın Depolama Hizmeti Şifrelemesi (SSE) kullanılarak geri kalanında şifrelenir. Bu, varsayılan olarak tüm yeni ve mevcut depolama hesapları için etkinleştirilmiştir ve devre dışı bırakılamaz. Şifrelenmiş verilerin, alma sırasında otomatik olarak şifresi çözülür. Varsayılan olarak, SSE kullanılarak şifrelenen veriler, Microsoft tarafından sunulan ve yönetilen bir anahtar kullanılarak şifrelenir. Kuruluşlar, SSE ile kullanılmak üzere kendi şifreleme anahtarlarını sağlamayı ve yönetmeyi tercih edebilir. Bu, şifrelenmiş veriler için isteğe bağlı ek bir güvenlik katmanı sağlar. Bu anahtar, müşteri tarafından şirket içinde veya güvenli bir şekilde Azure Anahtar Kasası dahilinde depolanabilir.

### <a name="secure-data-transport"></a>Güvenli veri aktarımı

Azure Backup, veri aktarım sırasında AES 256 kullanılarak şifrelendi. Bu veriler, yedekleme ilk yapılandırıldığında yönetim personeli tarafından oluşturulan bir parola kullanılarak güvenlidir. Microsoft bu parola üzerinde erişime sahip değildir ve müşterinin bu parolanın güvenli bir şekilde depolandığından emin olması gerekir. Daha sonra veri aktarımı, güvenli bir HTTPS bağlantısı aracılığıyla şirket içi ortam ve Azure kurtarma hizmetleri Kasası arasında gerçekleşir.  Kurtarma Hizmetleri kasasındaki veriler daha sonra Azure SSE kullanarak Rest 'de şifrelenir.

Azure Site Recovery verileri her zaman aktarım sırasında şifrelenir. Tüm çoğaltılan veriler, HTTPS ve TLS kullanarak güvenli bir şekilde Azure 'a taşınır. Azure müşterisi bir ExpressRoute bağlantısı kullanarak Azure 'a bağlanırsa, bu özel bağlantı aracılığıyla Azure Site Recovery veriler gönderilir.  Bir Azure müşterisi, bir VPN bağlantısı kullanarak Azure 'a bağlandığında, veriler şirket içi ve kurtarma hizmetleri kasasıyla internet üzerinden güvenli bir şekilde çoğaltılır.

Bu güvenli ağ veri aktarımı, bant medyası gibi geleneksel site dışı yedekleme depolama çözümlerini yönetmeye yönelik güvenlik riskini ve risk azaltma gereksinimlerini ortadan kaldırır.

### <a name="data-retention-periods"></a>Veri saklama dönemleri

En az üç ay yedekleme saklama süresi önerilir, ancak daha uzun bekletme süreleri genellikle gereklidir. Azure Backup, bir yedeklemenin en fazla 9999 kopyasını sağlayabilir. Korunan bir örneğin tek bir Azure Backup günlük olarak alındıysa, bu, günlük yedekleme 27 yılından elde etmek için izin verir. Korunan bir örnek için tek aylık yedeklemeler 833 yıllık bekletme için izin verir. Yedekleme verileri yaşlanıyor ve daha az parçalı yedeklemeler zaman içinde korunurken, yedekleme verileri için toplam bekletme penceresi artar.  Azure, verilerin Azure kurtarma hizmetleri kasasında kalabileceği sürenin uzunluğunu, yalnızca örnek başına toplam yedekleme sayısını sınırlamaz. Ayrıca, eski veya yeni yedeklerden geri yükleme arasında herhangi bir performans farkı yoktur, her geri yükleme işleminin gerçekleşmesi için aynı süre sürer.

Azure kurtarma hizmetleri kasasında bir dizi varsayılan yedekleme ve bekletme ilkesi vardır.  Yönetim personeli, özel yedekleme ve bekletme ilkeleri de oluşturabilir.

![Azure Backup Ilkesi](media/create-policy.png)

Azure Backup ve bekletme ilkeleri yapılandırılırken yedekleme sıklığı ve uzun süreli saklama gereksinimleri arasında bir denge bulunması gerekir.

### <a name="backup-and-restore-testing"></a>Yedekleme ve geri yükleme testi

ISM, geri yükleme veya yük devretme gerektiğinde korunan verilerin geçerli olduğundan emin olmak için yedekleme verilerinin test edilmesini öneriyor. Azure Backup ve Azure Site Recovery, korumalı verileri yedeklendikten veya çoğaltıldıktan sonra test etme özelliğini de sağlar. Azure Backup tarafından yönetilen veriler, aday bir konuma geri yüklenebilir ve verilerin tutarlılığı daha sonra doğrulanabilir.

Azure Site Recovery yük devretme testini gerçekleştirmek için yerleşik bir özelliğe sahiptir. Kurtarma Hizmetleri kasasına çoğaltılan iş yükleri aday bir Azure ortamına geri yüklenebilir. Hedef geri yükleme ortamı, bir test gerçekleştirilirken üretim sistemleri üzerinde hiçbir etkisi olmadığından emin olmak için herhangi bir üretim ortamından tamamen yalıtılabilir. Test tamamlandıktan sonra, işlem maliyetlerini azaltmak için test ortamı ve tüm kaynaklar hemen silinebilir.

Yük devretme testi ve doğrulama, Azure platformunda yerleşik olarak bulunan Azure Otomasyonu hizmeti kullanılarak otomatikleştirilebilir. Bu sayede yük devretme testi, verilerin Azure 'a başarıyla çoğaltılmakta olduğundan emin olmak için otomatik olarak gerçekleşecek şekilde zamanlanır.

## <a name="next-steps"></a>Sonraki adımlar

[Azure Ilkesiyle güvenlik sağlamaya](azure-policy.md)yönelik makaleyi gözden geçirin.
