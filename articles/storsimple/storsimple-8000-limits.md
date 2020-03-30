---
title: StorSimple 8000 serisi sistem limitleri | Microsoft Dokümanlar
description: StorSimple 8000 serisi bileşenleri ve bağlantıları için sistem sınırlarını ve önerilen boyutları açıklar.
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: ''
ms.assetid: c7392678-0924-46c6-9c59-1665cb9b6586
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 03/28/2017
ms.author: alkohli
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 70f2d9542082ddf7ecf1d1e7361b0ecdb14c5ef8
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "68963382"
---
# <a name="what-are-storsimple-8000-series-system-limits"></a>StorSimple 8000 serisi sistem limitleri nelerdir?

[!INCLUDE [storsimple-8000-eol-banner](../../includes/storsimple-8000-eol-banner.md)]

## <a name="overview"></a>Genel Bakış

StorSimple, veri merkeziniz için ölçeklenebilir ve esnek depolama sağlar. Ancak, StorSimple çözümünüzü planlarken, dağıtırken ve çalıştırırken aklınızda bulundurmanız gereken bazı sınırlar vardır. Aşağıdaki tabloda bu sınırlar açıklanır ve StorSimple çözümünüzden en iyi şekilde elde edilebilmeniz için bazı öneriler sağlar.

| Sınır tanımlayıcı | Sınır | Yorumlar |
| --- | --- | --- |
| Maksimum depolama hesabı kimlik bilgileri sayısı |64 | |
| Maksimum hacim kapları sayısı |64 | |
| Maksimum birim sayısı |255 | |
| Yerel olarak sabitlenmiş en fazla birim sayısı |32 | |
| Bant genişliği şablonu başına maksimum zamanlama sayısı |168 |Haftanın her günü (24*7) her saat için bir program. |
| Fiziksel aygıtlarda katmanlı bir ses düzeyinin maksimum boyutu |8100 ve 8600 için 64 TB |8100 ve 8600 fiziksel cihazlardır. |
| Azure'daki sanal aygıtlarda katmanlı bir birimin maksimum boyutu |8010 için 30 TB <br></br> 8020 için 64 TB |8010 ve 8020, Azure'da sırasıyla Standart Depolama ve Premium Depolama kullanan sanal aygıtlardır. |
| Fiziksel aygıtlarda yerel olarak sabitlenmiş bir ses düzeyinin maksimum boyutu |8100 için 8,5 TB <br></br> 8600 için 22,5 TB |8100 ve 8600 fiziksel cihazlardır. |
| Maksimum iSCSI bağlantısı sayısı |512 | |
| Başlatanlardan gelen maksimum iSCSI bağlantısı sayısı |512 | |
| Cihaz başına maksimum erişim kontrol kaydı sayısı |64 | |
| Yedekleme ilkesi başına maksimum birim sayısı |20 | |
| Zamanlama başına tutulan maksimum yedekleme sayısı (yedekleme ilkesinde) |64 | |
| Yedekleme ilkesi başına maksimum zamanlama sayısı |10 | |
| Birim başına tutulabilen herhangi bir türdeki maksimum anlık görüntü sayısı |256 |Bu numara, yerel anlık görüntüler ve bulut anlık görüntülerini içerir. |
| Herhangi bir cihazda bulunabilecek maksimum anlık görüntü sayısı |10,000 | |
| Yedekleme, geri yükleme veya klonlama için paralel olarak işlenebilen maksimum birim sayısı |16 |<ul><li>16'dan fazla birim varsa, işleme yuvaları kullanılabilir hale geldikçe sırayla işlenirler.</li><li>Klonlanmış veya geri yüklenen katmanlı bir birimin yeni yedekleri, işlem tamamlanana kadar oluşamaz. Ancak, yerel bir birim için, ses düzeyi çevrimiçi olduktan sonra yedeklemelere izin verilir.</li></ul> |
| Katmanlı birimler için geri yükleme ve klonlama süresi |< 2 dakika |<ul><li>Birim, ses boyutuna bakılmaksızın geri yükleme veya klon işleminden sonraki 2 dakika içinde kullanıma sunulmuştur.</li><li>Veri ve meta verilerin çoğu bulutta hala bulunduğundan, birim performansı başlangıçta normalden daha yavaş olabilir. Veriler buluttan StorSimple aygıtına akt'lar akt'ta performans artabilir.</li><li>Meta verileri indirmek için toplam süre ayrılan birim boyutuna bağlıdır. Meta veriler, ayrılan birim verilerin Tüberküloz başına 5 dakika hızında arka planda aygıta otomatik olarak getirilir. Bu hız, buluta internet bant genişliği etkilenebilir.</li><li>Tüm meta veriler aygıtta olduğunda geri yükleme veya klonlama işlemi tamamlanır.</li><li>Geri yükleme veya klon işlemi tam olarak tamamlanana kadar yedekleme işlemleri gerçekleştirilemez. |
| Yerel olarak sabitlenmiş birimler için kurtarma süresini geri yükleme |< 2 dakika |<ul><li>Birim, ses boyutuna bakılmaksızın geri yükleme işleminden sonraki 2 dakika içinde kullanıma hazır hale getirilir.</li><li>Veri ve meta verilerin çoğu bulutta hala bulunduğundan, birim performansı başlangıçta normalden daha yavaş olabilir. Veriler buluttan StorSimple aygıtına akt'lar akt'ta performans artabilir.</li><li>Meta verileri indirmek için toplam süre ayrılan birim boyutuna bağlıdır. Meta veriler, ayrılan birim verilerin Tüberküloz başına 5 dakika hızında arka planda aygıta otomatik olarak getirilir. Bu hız, buluta internet bant genişliği etkilenebilir.</li><li>Katmanlı birimlerin aksine, yerel olarak sabitlenmiş birimler için, birim verileri de aygıta yerel olarak indirilir. Tüm birim verileri aygıta getirildiğinde geri yükleme işlemi tamamlanır.</li><li>Geri yükleme işlemleri uzun olabilir. Geri yüklemeyi tamamlamak için gereken toplam süre, sağlanan yerel birimin boyutuna, Internet bant genişliğinize ve aygıttaki varolan verilere bağlıdır. Geri yükleme işlemi devam ederken yerel olarak sabitlenmiş birimdeki yedekleme işlemlerine izin verilir. |
| Bulut anlık görüntüleri için işleme oranı |15 dakika/TB |<ul><li>Bulut anlık görüntüsünü yüklemeye hazır hale getirmek için en az süre, yedekte ayrılan birim verilerin TB'si başına. </li><li> Toplam bulut anlık görüntüsü süresi, internet bant genişliğinden buluta etkilenen anlık görüntü yükleme süresine bu süre eklenerek hesaplanır. |
| Maksimum istemci okuma/yazma verimi (SSD katmanından sunulduğunda)* |Tek bir 10 GbE ağ arabirimi ile 920/720 MB/s |MPIO ve iki ağ arabirimi ile 2 kata kadar. |
| Maksimum istemci okuma/yazma verimi (HDD katmanından sunulduğunda)* |120/250 MB/sN | |
| Güncelleme 3 ve sonrası için maksimum istemci okuma/yazma verimi (bulut katmanından sunulduğunda)* |Katmanlı hacimler için 40/60 MB/sn<br><br>Hacim oluşturma sırasında seçilen arşiv seçeneğine sahip katmanlı birimler için 60/80 MB/s |Okuma iş boku, istemcilerin yeterli G/Ç kuyruk derinliği oluşturmalarına ve korumalarına bağlıdır. <br><br>Elde edilen hız, kullanılan temel depolama hesabının hızına bağlıdır. |

&#42; G/Ç türü başına maksimum verim yüzde 100 okuma ve yüzde 100 yazma senaryoları ile ölçüldü. Gerçek iş bilgili lik daha düşük olabilir ve G/Ç karışımı ve ağ koşullarına bağlıdır.

&#42;&#42; 3 Güncelleştirme'den önceki Performans numaraları daha düşük olabilir.

## <a name="next-steps"></a>Sonraki adımlar
[StorSimple sistem gereksinimlerini](storsimple-8000-system-requirements.md)gözden geçirin.

