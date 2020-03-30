---
author: alkohli
ms.service: storsimple
ms.topic: include
ms.date: 10/26/2018
ms.author: alkohli
ms.openlocfilehash: 93f4f74d435cc14130668da102d1246c5fad5872
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "67188769"
---
| Sınır tanımlayıcı | Sınır | Yorumlar |
| --- | --- | --- |
| Maksimum depolama hesabı kimlik bilgileri sayısı |64 | |
| Maksimum hacim kapları sayısı |64 | |
| Maksimum birim sayısı |255 | |
| Bant genişliği şablonu başına maksimum zamanlama sayısı |168 |Haftanın her günü, her saat için bir program. |
| Fiziksel aygıtlarda katmanlı bir ses düzeyinin maksimum boyutu |StorSimple 8100 ve StorSimple 8600 için 64 TB |StorSimple 8100 ve StorSimple 8600 fiziksel cihazlardır. |
| Azure'daki sanal aygıtlarda katmanlı bir birimin maksimum boyutu |StorSimple 8010 için 30 TB <br></br> StorSimple 8020 için 64 TB |StorSimple 8010 ve StorSimple 8020, Azure'da sırasıyla Standart depolama ve Premium depolama yı kullanan sanal aygıtlardır. |
| Fiziksel aygıtlarda yerel olarak sabitlenmiş bir ses düzeyinin maksimum boyutu |StorSimple 8100 için 9 TB <br></br> StorSimple 8600 için 24 TB |StorSimple 8100 ve StorSimple 8600 fiziksel cihazlardır. |
| Maksimum iSCSI bağlantısı sayısı |512 | |
| Başlatanlardan gelen maksimum iSCSI bağlantısı sayısı |512 | |
| Cihaz başına maksimum erişim kontrol kaydı sayısı |64 | |
| Yedekleme ilkesi başına maksimum birim sayısı |24 | |
| Yedekleme ilkesi başına tutulan maksimum yedekleme sayısı |64 | |
| Yedekleme ilkesi başına maksimum zamanlama sayısı |10 | |
| Birim başına tutulabilen herhangi bir türdeki maksimum anlık görüntü sayısı |256 |Bu tutar, yerel anlık görüntüleri ve bulut anlık görüntülerini içerir. |
| Herhangi bir cihazda bulunabilecek maksimum anlık görüntü sayısı |10,000 | |
| Yedekleme, geri yükleme veya klonlama için paralel olarak işlenebilen maksimum birim sayısı |16 |<ul><li>16'dan fazla birim varsa, işleme yuvaları kullanılabilir hale geldikçe sırayla işlenirler.</li><li>Klonlanmış veya geri yüklenen katmanlı bir birimin yeni yedekleri, işlem tamamlanana kadar oluşamaz. Yerel bir birim için, ses düzeyi çevrimiçi olduktan sonra yedeklemelere izin verilir.</li></ul> |
| Katmanlı birimler için geri yükleme ve klonlama süresi |<2 dakika |<ul><li>Birim, ses boyutuna bakılmaksızın, geri yükleme veya klon işleminden sonraki 2 dakika içinde kullanıma hazır hale getirilir.</li><li>Veri ve meta verilerin çoğu bulutta hala bulunduğundan, birim performansı başlangıçta normalden daha yavaş olabilir. Veriler buluttan StorSimple aygıtına akt'lar olarak performans artabilir.</li><li>Meta verileri indirmek için toplam süre ayrılan birim boyutuna bağlıdır. Meta veriler, ayrılan birim verilerin Tüberküloz başına 5 dakika hızında arka planda aygıta otomatik olarak getirilir. Bu hız, buluta internet bant genişliği etkilenebilir.</li><li>Tüm meta veriler aygıtta olduğunda geri yükleme veya klonlama işlemi tamamlanır.</li><li>Geri yükleme veya klon işlemi tam olarak tamamlanana kadar yedekleme işlemleri gerçekleştirilemez. |
| Yerel olarak sabitlenmiş birimler için kurtarma süresini geri yükleme |<2 dakika |<ul><li>Birim, ses boyutuna bakılmaksızın geri yükleme işleminden sonraki 2 dakika içinde kullanıma hazır hale getirilir.</li><li>Veri ve meta verilerin çoğu bulutta hala bulunduğundan, birim performansı başlangıçta normalden daha yavaş olabilir. Veriler buluttan StorSimple aygıtına akt'lar olarak performans artabilir.</li><li>Meta verileri indirmek için toplam süre ayrılan birim boyutuna bağlıdır. Meta veriler, ayrılan birim verilerin Tüberküloz başına 5 dakika hızında arka planda aygıta otomatik olarak getirilir. Bu hız, buluta internet bant genişliği etkilenebilir.</li><li>Katmanlı birimlerin aksine, yerel olarak sabitlenmiş birimler varsa, birim verileri de aygıta yerel olarak indirilir. Tüm birim verileri aygıta getirildiğinde geri yükleme işlemi tamamlanır.</li><li>Geri yükleme işlemleri uzun olabilir ve geri yüklemeyi tamamlamak için toplam süre, sağlanan yerel birimin boyutuna, Internet bant genişliğinize ve aygıttaki varolan verilere bağlıdır. Geri yükleme işlemi devam ederken yerel olarak sabitlenmiş birimdeki yedekleme işlemlerine izin verilir. |
| İnce geri yükleme kullanılabilirliği |Son failover | |
| SSD katmanından servis edildiğinde maksimum istemci okuma/yazma verimi* |Tek bir 10 gigabit Ethernet ağ arabirimi ile 920/720 MB/sn |MPIO ve iki ağ arabirimi ile en fazla iki kez. |
| HDD katmanından servis edildiğinde maksimum istemci okuma/yazma verimi* |120/250 MB/sn | |
| Bulut katmanından servis edildiğinde maksimum istemci okuma/yazma verimi* |11/41 MB/sn |Okuma iş boku, istemcilerin yeterli G/Ç kuyruk derinliği oluşturmalarına ve korumalarına bağlıdır. |

&#42;G/Ç türü başına maksimum verim yüzde 100 okuma ve yüzde 100 yazma senaryoları ile ölçüldü. Gerçek iş bilgili lik daha düşük olabilir ve G/Ç karışımı ve ağ koşullarına bağlıdır.

