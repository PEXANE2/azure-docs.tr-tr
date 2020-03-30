---
author: alkohli
ms.service: storsimple
ms.topic: include
ms.date: 10/26/2018
ms.author: alkohli
ms.openlocfilehash: 658fd9178495f14274c85eab2129c9dcd3be7693
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "67188402"
---
| **Sınır tanımlayıcı** | **Sınırı** | **Açıklamalar** |
| --- | --- | --- |
| Toplam kapasite (bulut dahil) |Sanal cihaz başına 64 TB'ye kadar |Başka bir boş dizi için tam bir StorSimple Sanal Dizi üzerinde başarısız olabilir. Aynı aygıta geri yüklemeyi denerseniz, bu işlemi tamamlamak için aygıtta yeterli alana sahip olduğundan emin olun. 32 TB'yi aştıktan sonra aynı cihaza geri yükleyemezsiniz. |
| Aygıt başına maksimum depolama hesabı kimlik bilgileri sayısı |1 | |
| Maksimum birim/hisse sayısı |16 | |
| Katmanlı bir payın minimum boyutu |500 GB | |
| Katmanlı bir birimin minimum boyutu |500 GB | |
| Katmanlı bir payın maksimum boyutu |20 TB | |
| Katmanlı bir birimin maksimum boyutu |5 TB | |
| Yerel olarak sabitlenmiş bir payın minimum boyutu |50 GB | |
| Yerel olarak sabitlenmiş bir birimin minimum boyutu |50 GB | |
| Yerel olarak sabitlenmiş bir payın maksimum boyutu |2 TB | |
| Yerel olarak sabitlenmiş bir birimin maksimum boyutu |200 GB | |
| Başlatanlardan gelen maksimum iSCSI bağlantısı sayısı |512 | |
| Cihaz başına maksimum erişim kontrol kaydı sayısı |64 | |
| Dosya sunucusu için *.backups* klasöründe sanal aygıt tarafından tutulan maksimum yedekleme sayısı |5 |Buna en son zamanlanmış (varsayılan yedekleme ilkesi tarafından oluşturulan) ve el ile yedeklemeler dahildir. |
| Aygıt tarafından tutulan maksimum zamanlanmış yedekleme sayısı |55 |30 günlük yedekleme<br>12 aylık yedekleme<br>13 yıllık yedeklemeler |
| Cihaz tarafından tutulan maksimum manuel yedekleme sayısı |45 | |
| Bir dosya sunucusu için hisse başına maksimum dosya sayısı |1 milyon |Aygıt geri yüklemesi gerçekleştirirken, geri yükleme süreleri aygıttaki tüm paylaşımlar daki dosya sayısıyla orantılıdır. |
| Bir iSCSI sunucusu için birim başına maksimum dosya sayısı |1 milyon | |
| Sanal dizi başına maksimum dosya sayısı |4 milyon | |
| Kurtarma süresini geri yükleme |Hızlı geri yükleme |Geri yükleme ısı haritasına dayanır ve hacim boyutuna bağlıdır.<br>Geri yükleme işlemi devam ederken yedekleme işlemleri oluşabilir. |

