---
author: alkohli
ms.service: storsimple
ms.topic: include
ms.date: 10/26/2018
ms.author: alkohli
ms.openlocfilehash: 658fd9178495f14274c85eab2129c9dcd3be7693
ms.sourcegitcommit: fad3aaac5af8c1b3f2ec26f75a8f06e8692c94ed
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/27/2020
ms.locfileid: "67188402"
---
| **Sınır tanımlayıcı** | **Sınırlı** | **Açıklamalar** |
| --- | --- | --- |
| Toplam Kapasite (bulut dahil) |Sanal cihaz başına 64 TB 'a kadar |Tam StorSimple Sanal dizisinin yükünü başka bir boş diziye devretmek için kullanabilirsiniz. Aynı cihaza geri yüklemeyi denerseniz, bu işlemi gerçekleştirmek için cihazda yeterli alana sahip olduğunuzdan emin olun. 32 TB 'yi aşduktan sonra aynı cihaza geri yükleyemezsiniz. |
| Cihaz başına en fazla depolama hesabı kimlik bilgisi sayısı |1 | |
| En fazla birim/paylaşım sayısı |16 | |
| Katmanlı paylaşımın en küçük boyutu |500 GB | |
| Katmanlı birimin en küçük boyutu |500 GB | |
| Katmanlı paylaşımın en büyük boyutu |20 TB | |
| Katmanlı birimin en büyük boyutu |5 TB | |
| Yerel olarak sabitlenmiş bir paylaşımın en küçük boyutu |50 GB | |
| Yerel olarak sabitlenmiş bir birimin en küçük boyutu |50 GB | |
| Yerel olarak sabitlenmiş bir paylaşımın en büyük boyutu |2 TB | |
| Yerel olarak sabitlenmiş bir birimin en büyük boyutu |200 GB | |
| Başlatıcılardan en fazla Iscsı bağlantısı sayısı |512 | |
| Cihaz başına en fazla erişim denetimi kaydı sayısı |64 | |
| Dosya sunucusu için *. Backups* klasöründeki sanal cihaz tarafından tutulan en fazla yedekleme sayısı |5 |Buna en son zamanlanmış (varsayılan yedekleme ilkesi tarafından oluşturulan) ve el ile yedeklemeler dahildir. |
| Cihaz tarafından tutulan en fazla zamanlanmış yedekleme sayısı |55 |30 günlük yedekleme<br>12 aylık yedekleme<br>13 yıllık yedekleme |
| Cihaz tarafından tutulan en fazla el ile yedekleme sayısı |45 | |
| Dosya sunucusu için paylaşımdaki en fazla dosya sayısı |1 milyon |Bir cihaz geri yükleme işlemi gerçekleştirirken geri yükleme süreleri cihazdaki tüm paylaşımlardaki dosya sayısıyla orantılıdır. |
| Bir Iscsı sunucusu için birim başına en fazla dosya sayısı |1 milyon | |
| Sanal dizi başına en fazla dosya sayısı |4.000.000 | |
| Geri yükleme kurtarma zamanı |Hızlı geri yükleme |Geri yükleme, ısı haritasına dayalıdır ve birimin boyutuna bağlıdır.<br>Yedekleme işlemleri, bir geri yükleme işlemi devam ederken ortaya çıkabilir. |

