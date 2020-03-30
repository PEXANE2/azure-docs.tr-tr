---
title: StorSimple Snapshot Manager yönetimi | Microsoft Dokümanlar
description: StorSimple Snapshot Manager çözüm yönetimi görevleri ve iş akışları hakkında daha fazla bilgiye genel bir bakış ve bağlantılar sağlar.
services: storsimple
documentationcenter: NA
author: twooley
manager: timlt
editor: ''
ms.assetid: 1cdbb61d-bd16-4be4-ade2-ceab11508acb
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 06/05/2016
ms.author: twooley
ms.openlocfilehash: b8fe87a2df633af310bc661308813a60987e77d6
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75933275"
---
# <a name="use-storsimple-snapshot-manager-to-administer-your-storsimple-solution"></a>StorSimple çözümünüzü yönetmek için StorSimple Snapshot Manager'ı kullanın

## <a name="overview"></a>Genel Bakış
StorSimple Snapshot Manager, Microsoft Azure StorSimple ortamında veri korumayı ve yedekleme yönetimini kolaylaştıran bir Microsoft Yönetim Konsolu (MMC) snap-in'dir. StorSimple Snapshot Manager ile, microsoft Azure StorSimple verilerini veri merkezinde ve bulutta tek bir entegre depolama çözümü olarak yönetebilir, böylece yedekleme işlemlerini basitleştirebilir ve maliyetleri azaltabilirsiniz.

StorSimple Snapshot Manager merkezi yönetim konsolu, yerel ve bulut verilerinin tutarlı, zamanında yedekleme kopyalarını oluşturmanıza olanak tanır. Örneğin, konsolu aşağıdakiler için kullanabilirsiniz:

* Birimleri yapılandırın, yedekleyin ve silin.
* Yedeklenen verilerin uygulama tutarlı olduğundan emin olmak için birim gruplarını yapılandırın.
* Verilerin önceden belirlenmiş bir zamanlamada yedeklenebilmeleri için yedekleme ilkelerini yönetin.
* Bulutta depolanabilen ve olağanüstü durum kurtarma için kullanılabilen bağımsız veri kopyalarını oluşturun.

Bu makalede, StorSimple Snapshot Manager açıklayan öğreticiler ve sistem yönetimi görevleri ve iş akışları tamamlamak için nasıl kullanılacağı bağlantılar sağlar.

* StorSimple Snapshot Manager bileşenleri ve mimarisi hakkında daha fazla bilgi için [Bkz. StorSimple Snapshot Manager nedir?](storsimple-what-is-snapshot-manager.md) 
* StorSimple Snapshot Manager'ı indirmek için [StorSimple Snapshot Manager indirme sayfasına](https://www.microsoft.com/download/details.aspx?id=44220)gidin.
* StorSimple Snapshot Manager dağıtım yordamları [için, StorSimple Snapshot Manager dağıt'a](storsimple-snapshot-manager-deployment.md)gidin.

> [!NOTE]
> Microsoft Azure StorBasit Sanal Dizileri (storsimple şirket içi sanal aygıtlar olarak da bilinir) yönetmek için StorSimple Snapshot Manager'ı kullanamazsınız.


## <a name="storsimple-snapshot-manager-tasks-and-workflows"></a>StorSimple Snapshot Manager görevleri ve iş akışları
Geçerli, zamanlanmış ve tamamlanmış yedekleme işlerini izlemek ve yönetmek için StorSimple Snapshot Manager'ı kullanabilirsiniz. Ayrıca, StorSimple Snapshot Manager 64'e kadar tamamlanmış yedeklemenin bir kataloğunu sağlar. Birimleri veya dosyaları bulmak ve geri yüklemek için kataloğu kullanabilirsiniz. 

| BUNU YAPMAK ISTIYORSANıZ... | BU ÖĞRETICI KULLANıN ... |
|:--- |:--- |
| StorSimple Snapshot Manager hakkında daha fazla bilgi edinin |[StorSimple Snapshot Manager nedir?](storsimple-what-is-snapshot-manager.md) |
| StorSimple Snapshot Manager'ı yükle<br>StorSimple Snapshot Manager'ı yeniden yükleyin<br>StorSimple Snapshot Yöneticisini Kaldır |[StorSimple Snapshot Manager'ı dağıt](storsimple-snapshot-manager-deployment.md) |
| StorSimple Snapshot Manager menülerini ve özelliklerini kullanın:<ul><li>Menü çubuğu</li><li>Araç çubuğu</li><li>Kapsam bölmesi</li><li>Sonuç bölmesi</li><li>Eylemler bölmesi</li><li>Klavye gezintisi ve kısayollar</li></ul> |[StorSimple Snapshot Manager kullanıcı arabirimi](storsimple-use-snapshot-manager.md) |
| StorSimple Snapshot Manager'da yer alan ortak MMC özelliklerini kullanın:<ul><li>Görünüm</li><li>Buradan Yeni Pencere</li><li>Yenile</li><li>İhracat Listesi</li><li>Yardım</li></ul> |[StorSimple Snapshot Manager'da MMC menü eylemlerini kullanma](storsimple-snapshot-manager-mmc-menu.md) |
| Aygıt ekleme veya değiştirme<br>Cihaz bağlama<br>İthal hacim gruplarını doğrulama<br>Bağlı cihazları yenileyin<br>Aygıtın kimliğini doğrulatın<br>Cihaz ayrıntılarını görüntüleme<br>Aygıt yapılandırması silme<br>Aygıt parolası değiştirme<br>Başarısız bir aygıtı değiştirme<br> |[StorSimple aygıtlarını bağlamak ve yönetmek için StorSimple Snapshot Manager'ı kullanın](storsimple-snapshot-manager-manage-devices.md) |
| Montaj hacimleri<br>Birimler hakkındaki bilgileri görüntüleme<br>Bir birim silme<br>Yeniden tazyik hacimleri<br>Temel bir birimi yapılandırma ve yedekleme<br>Dinamik aynalı bir birimi yapılandırma ve yedekleme |[Birimleri görüntülemek ve yönetmek için StorSimple Snapshot Manager'ı kullanın](storsimple-snapshot-manager-manage-volumes.md) |
| Birim gruplarını görüntüleme<br>Birim grubu oluşturma<br>Birim grubunu yedekleme<br>Birim grubunu edin<br>Birim grubunu silme |[Birim grupları oluşturmak ve yönetmek için StorSimple Snapshot Manager'ı kullanın](storsimple-snapshot-manager-manage-volume-groups.md) |
| Yedekleme ilkesi oluşturma <br>Yedekleme ilkesini edin<br>Yedekleme ilkesini silme |[Yedekleme ilkeleri oluşturmak ve yönetmek için StorSimple Snapshot Manager'ı kullanın](storsimple-snapshot-manager-manage-backup-policies.md) |
| Zamanlanmış yedekleme işlerini görüntüleme ve yönetme<br>Son yedekleme işlerini görüntüleme ve yönetme<br>Şu anda çalışan yedekleme işlerini görüntüleme ve yönetme |[Yedekleme işlerini görüntülemek ve yönetmek için StorSimple Snapshot Manager'ı kullanın](storsimple-snapshot-manager-manage-backup-jobs.md) |
| Ses düzeyini geri yükleme<br>Bir birim veya hacim grubunu klonlama<br>Yedeklemeyi silme<br>Dosyayı kurtarma<br>StorSimple Snapshot Manager veritabanını geri yükleme |[Yedekleme kataloğunu yönetmek için StorSimple Snapshot Manager'ı kullanın](storsimple-snapshot-manager-manage-backup-catalog.md) |

## <a name="next-steps"></a>Sonraki adımlar
[StorSimple Snapshot Manager'ı indirin.](https://www.microsoft.com/download/details.aspx?id=44220)

