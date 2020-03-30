---
title: StorSimple Snapshot Manager yedekleme işleri | Microsoft Dokümanlar
description: Zamanlanmış, şu anda çalışan ve tamamlanmış yedekleme işlerini görüntülemek ve yönetmek için StorSimple Snapshot Manager MMC snap-in'in nasıl kullanılacağını açıklar.
services: storsimple
documentationcenter: NA
author: twooley
manager: timlt
editor: ''
ms.assetid: bf4dcff6-c819-4766-b9d9-9922831cb200
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 06/05/2017
ms.author: twooley
ms.openlocfilehash: 094b996cd3227903995c7a74ef14ed8c0561f59d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75933345"
---
# <a name="use-storsimple-snapshot-manager-to-view-and-manage-backup-jobs"></a>Yedekleme işlerini görüntülemek ve yönetmek için StorSimple Snapshot Manager'ı kullanın

## <a name="overview"></a>Genel Bakış
**Kapsam** bölmesindeki **İşler** düğümü, etkileşimli olarak veya yapılandırılmış bir ilke tarafından başlattığınız **Zamanlanmış**, **Son 24 saat**ve **Çalışan** yedekleme görevlerini gösterir. 

Bu öğretici, zamanlanmış, en son ve şu anda çalışan yedekleme işleri hakkında bilgi görüntülemek için **İşler** düğümündeki leri nasıl kullanabileceğinizi açıklar. (İşlerin ve ilgili bilgilerin listesi **Sonuçlar** bölmesinde görünür.) Ayrıca, listelenen bir işi sağ tıklatabilir ve kullanılabilir eylemleri listeleyen bir bağlam menüsü görebilirsiniz.

## <a name="view-scheduled-jobs"></a>Zamanlanmış işleri görüntüleme
Zamanlanmış yedekleme işlerini görüntülemek için aşağıdaki yordamı kullanın.

#### <a name="to-view-scheduled-jobs"></a>Zamanlanmış işleri görüntülemek için
1. StorSimple Snapshot Manager'ı başlatmak için masaüstü simgesini tıklatın. 
2. **Kapsam** **bölmesinde, İşler** düğümünü genişletin ve **Zamanlanan'ı**tıklatın. Sonuçlar **bölmesinde** aşağıdaki bilgiler görünür:
   
   * **Ad** – zamanlanan anlık görüntünün adı
   * **Sonraki Çalıştırma** – bir sonraki zamanlanan anlık görüntünün tarih ve saati
   * **Son Çalıştırma** – en son zamanlanan anlık görüntünün tarih ve saati
     
     > [!NOTE]
     > Yalnızca bir kerelik anlık görüntüler için **Sonraki Çalıştırma** ve **Son Çalıştırma** aynı olacaktır.
     
     ![Zamanlanmış yedekleme işleri](./media/storsimple-snapshot-manager-manage-backup-jobs/HCS_SSM_Jobs_scheduled.png) 
3. Belirli bir iş üzerinde ek eylemler gerçekleştirmek için **Sonuçlar** bölmesinde iş adını sağ tıklatın ve menü seçeneklerinden seçim yapın.

## <a name="view-recent-jobs"></a>En son işleri görüntüleme
Yedeklemeyi görüntülemek ve son 24 saat içinde tamamlanan işleri geri yüklemek için aşağıdaki yordamı kullanın.

#### <a name="to-view-recent-jobs"></a>Son işleri görüntülemek için
1. StorSimple Snapshot Manager'ı başlatmak için masaüstü simgesini tıklatın.
2. **Kapsam** **bölmesinde, İşler** düğümünü genişletin ve **Son 24 saat'i**tıklatın. **Sonuçlar** bölmesi, son 24 saat (en fazla 64 iş) için yedekleme işleri gösterir. Belirttiğiniz **Görünüm** seçeneklerine bağlı olarak, Sonuçlar **bölmesinde** aşağıdaki bilgiler görünür:
   
   * **Ad** – zamanlanan anlık görüntünün adı.
   * **Başlangıç** – anlık görüntünün başladığı tarih ve saat.
   * **Durduruldu** – anlık görüntünün bittiği veya sonlandırıldığu tarih ve saat.
   * **Geçen** – **Başlangıç** ve **Durdurma** süreleri arasındaki süre.
   * **Durum** – son tamamlanan işin durumu. **Başarı,** yedeklemenin başarıyla oluşturulduğunu gösterir. **Başarısız,** işin başarılı çalışmadığını gösterir.
   * **Bilgi** – başarısızlığın nedeni.
   * **Bayt işlenir (MB)** – işlenen hacim grubundan (MB'lerde) veri miktarı. 
     
     ![Son 24 saatte yapılan işler](./media/storsimple-snapshot-manager-manage-backup-jobs/HCS_SSM_Jobs_Last_24_hours.png) 
3. Belirli bir iş üzerinde ek eylemler gerçekleştirmek için **Sonuçlar** bölmesinde iş adını sağ tıklatın ve menü seçeneklerinden seçim yapın.
   
    ![İşi silme](./media/storsimple-snapshot-manager-manage-backup-catalog/HCS_SSM_Delete_backup.png)

## <a name="view-currently-running-jobs"></a>Şu anda çalışan işleri görüntüleme
Şu anda çalışan işleri görüntülemek için aşağıdaki yordamı kullanın.

#### <a name="to-view-currently-running-jobs"></a>Şu anda çalışan işleri görüntülemek için
1. StorSimple Snapshot Manager'ı başlatmak için masaüstü simgesini tıklatın.
2. **Kapsam** **bölmesinde, İşler** düğümünü genişletin ve **Çalıştır'ı**tıklatın. Belirttiğiniz **Görünüm** seçeneklerine bağlı olarak, **Sonuçlar** bölmesinde aşağıdaki bilgiler görünür:
   
   * **Ad** – zamanlanan anlık görüntünün adı.
   * **Başlangıç** – anlık görüntünün başladığı tarih ve saat.
   * **Checkpoint** – yedeklemenin geçerli eylemi.
   * **Durum** – tamamlanma yüzdesi.
   * **Geçen** - yedekleme başladığından bu yana geçen süre. 
   * **Ortalama iş ortası (MB)** – işlenen toplam veri baytlarının, işlenmek üzere geçen toplam süreye (MBs) oranı.
   * **İşlenen baytlar (MB)** – işlenen toplam bayt veri (MB'lerde).
   * **Bayt yazılı (MB)** – toplam bayt veri yazılı (MBs). Meta verilerin yanı sıra verileri de içerir ve bu nedenle genellikle İşlenen Baytlardan daha büyüktür.
     
     ![Şu anda çalışan işler](./media/storsimple-snapshot-manager-manage-backup-jobs/HCS_SSM_Jobs_running.png)
3. Belirli bir iş üzerinde ek eylemler gerçekleştirmek için **Sonuçlar** bölmesinde iş adını sağ tıklatın ve menü seçeneklerinden seçim yapın.

## <a name="next-steps"></a>Sonraki adımlar
* [StorSimple çözümünüzü yönetmek için StorSimple Snapshot Manager'ı](storsimple-snapshot-manager-admin.md)nasıl kullanacağınızı öğrenin.
* [Yedekleme kataloğunu yönetmek için StorSimple Snapshot Manager'ı](storsimple-snapshot-manager-manage-backup-catalog.md)nasıl kullanacağınızı öğrenin.

