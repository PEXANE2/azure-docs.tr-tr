---
title: StorSimple Snapshot Manager Yönetimi | Microsoft Docs
description: StorSimple Snapshot Manager çözümü yönetim görevleri ve iş akışları hakkında daha fazla bilgi için genel bakış ve bağlantılar sağlar.
services: storsimple
documentationcenter: NA
author: twooley
manager: timlt
editor: ''
ms.assetid: 1cdbb61d-bd16-4be4-ade2-ceab11508acb
ms.service: storsimple
ms.devlang: NA
ms.topic: how-to
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 06/05/2016
ms.author: twooley
ms.openlocfilehash: f9a42a8f94f3e5cb63cd340b696a803d97491c76
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85512823"
---
# <a name="use-storsimple-snapshot-manager-to-administer-your-storsimple-solution"></a>StorSimple çözümünüzü yönetmek için StorSimple Snapshot Manager kullanın

## <a name="overview"></a>Genel Bakış
StorSimple Snapshot Manager, bir Microsoft Azure StorSimple ortamında veri koruma ve yedekleme yönetimini kolaylaştıran bir Microsoft Yönetim Konsolu (MMC) ek bileşenidir. StorSimple Snapshot Manager, veri merkezindeki ve buluttaki Microsoft Azure StorSimple verileri tek bir tümleşik depolama çözümü olarak yönetebilir, böylece Yedekleme süreçlerini basitleştirir ve maliyetleri azaltabilirsiniz.

StorSimple Snapshot Manager merkezi yönetim konsolu, yerel ve bulut verilerinin tutarlı, noktadan noktaya yedek kopyalarını oluşturmanızı sağlar. Örneğin, konsolunu kullanarak şunları yapabilirsiniz:

* Birimleri yapılandırma, yedekleme ve silme.
* Yedeklenen verilerin uygulamayla tutarlı olduğundan emin olmak için birim gruplarını yapılandırın.
* Yedekleme ilkelerini, verilerin önceden belirlenmiş bir zamanlamaya göre yedeklenmesi için yönetin.
* Bulutta depolanabilecek ve olağanüstü durum kurtarma için kullanılan, verilerin bağımsız kopyalarını oluşturun.

Bu makalede, StorSimple Snapshot Manager açıklayan öğreticiler ve sistem yönetimi görevlerini ve iş akışlarını tamamlayacak nasıl kullanılacağı açıklanır.

* StorSimple Snapshot Manager bileşenleri ve mimarisi hakkında daha fazla bilgi için bkz. [storsimple Snapshot Manager nedir?](storsimple-what-is-snapshot-manager.md) 
* StorSimple Snapshot Manager indirmek için [storsimple Snapshot Manager indirme sayfasına](https://www.microsoft.com/download/details.aspx?id=44220)gidin.
* StorSimple Snapshot Manager dağıtım yordamları için, [storsimple Snapshot Manager dağıtma](storsimple-snapshot-manager-deployment.md)bölümüne gidin.

> [!NOTE]
> Microsoft Azure StorSimple Sanal dizilerini (StorSimple şirket içi sanal aygıtlar olarak da bilinir) yönetmek için StorSimple Snapshot Manager kullanamazsınız.


## <a name="storsimple-snapshot-manager-tasks-and-workflows"></a>StorSimple Snapshot Manager görevleri ve iş akışları
StorSimple Snapshot Manager kullanarak geçerli, zamanlanmış ve tamamlanmış yedekleme işlerini izleyebilir ve yönetebilirsiniz. Ayrıca, StorSimple Snapshot Manager, 64 tamamlanmış yedeklemelerin kataloğunu sunmaktadır. Birimleri veya tek tek dosyaları bulmak ve geri yüklemek için kataloğunu kullanabilirsiniz. 

| BUNU YAPMAK ISTIYORSANıZ... | BU ÖĞRETICIYI KULLANıN... |
|:--- |:--- |
| StorSimple Snapshot Manager hakkında daha fazla bilgi edinin |[StorSimple Snapshot Manager nedir?](storsimple-what-is-snapshot-manager.md) |
| StorSimple Snapshot Manager 'yi yükler<br>StorSimple Snapshot Manager yeniden yükleyin<br>StorSimple Snapshot Manager kaldır |[StorSimple Snapshot Manager dağıtma](storsimple-snapshot-manager-deployment.md) |
| StorSimple Snapshot Manager menülerini ve özelliklerini kullanın:<ul><li>Menü çubuğu</li><li>Araç çubuğu</li><li>Kapsam bölmesi</li><li>Sonuçlar bölmesi</li><li>Eylemler bölmesi</li><li>Klavye gezintisi ve kısayolları</li></ul> |[StorSimple Snapshot Manager Kullanıcı arabirimi](storsimple-use-snapshot-manager.md) |
| StorSimple Snapshot Manager dahil edilen ortak MMC özelliklerini kullanın:<ul><li>Görüntüle</li><li>Buradan Yeni Pencere</li><li>Yenile</li><li>Listeyi dışarı aktar</li><li>Yardım</li></ul> |[StorSimple Snapshot Manager MMC menü eylemlerini kullanma](storsimple-snapshot-manager-mmc-menu.md) |
| Bir cihaz ekleme veya değiştirme<br>Cihaz bağlama<br>İçeri aktarılan birim gruplarını doğrula<br>Bağlı cihazları Yenile<br>Bir cihazın kimliğini doğrulama<br>Cihaz ayrıntılarını görüntüleme<br>Cihaz yapılandırmasını silme<br>Bir cihaz parolasını değiştirme<br>Hatalı bir cihazı değiştirme<br> |[StorSimple cihazlarını bağlamak ve yönetmek için StorSimple Snapshot Manager kullanma](storsimple-snapshot-manager-manage-devices.md) |
| Birimleri bağlama<br>Birimler hakkındaki bilgileri görüntüleme<br>Bir birimi silme<br>Birimleri yeniden Tara<br>Temel bir birimi yapılandırma ve yedekleme<br>Dinamik bir yansıtılmış birimi yapılandırma ve yedekleme |[Birimleri görüntülemek ve yönetmek için StorSimple Snapshot Manager kullanma](storsimple-snapshot-manager-manage-volumes.md) |
| Birim gruplarını görüntüle<br>Birim grubu oluşturma<br>Birim grubunu yedekleme<br>Birim grubunu düzenleme<br>Birim grubunu silme |[Toplu grupları oluşturmak ve yönetmek için StorSimple Snapshot Manager kullanma](storsimple-snapshot-manager-manage-volume-groups.md) |
| Yedekleme ilkesi oluşturma <br>Yedekleme ilkesini düzenleme<br>Yedekleme ilkesini silme |[Yedekleme ilkeleri oluşturmak ve yönetmek için StorSimple Snapshot Manager kullanma](storsimple-snapshot-manager-manage-backup-policies.md) |
| Zamanlanmış yedekleme işlerini görüntüleyin ve yönetin<br>Son yedekleme işlerini görüntüleyin ve yönetin<br>Şu anda çalışan yedekleme işlerini görüntüleyin ve yönetin |[Yedekleme işlerini görüntülemek ve yönetmek için StorSimple Snapshot Manager kullanma](storsimple-snapshot-manager-manage-backup-jobs.md) |
| Bir birimi geri yükleme<br>Birim veya birim grubunu kopyalama<br>Bir yedeklemeyi silme<br>Dosyayı kurtarma<br>StorSimple Snapshot Manager veritabanını geri yükleme |[Yedekleme kataloğunu yönetmek için StorSimple Snapshot Manager kullanma](storsimple-snapshot-manager-manage-backup-catalog.md) |

## <a name="next-steps"></a>Sonraki adımlar
[StorSimple Snapshot Manager indirin](https://www.microsoft.com/download/details.aspx?id=44220).

