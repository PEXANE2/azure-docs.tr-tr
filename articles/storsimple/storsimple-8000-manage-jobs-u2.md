---
title: StorSimple 8000 serisi için işleri görüntüleyin ve yönetin | Microsoft Dokümanlar
description: StorSimple Device Manager hizmeti İşler bıçağını ve en son, güncel ve zamanlanmış yedekleme işlerini izlemek için nasıl kullanılacağını açıklar.
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: ''
ms.assetid: ''
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 06/29/2017
ms.author: alkohli
ms.openlocfilehash: 462f8dafdffa7ee01e6ccf7945a1abfdff90db42
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79254878"
---
# <a name="use-the-storsimple-device-manager-service-to-view-and-manage-jobs-update-3-and-later"></a>İşleri görüntülemek ve yönetmek için StorSimple Device Manager hizmetini kullanın (3 ve sonraki güncelleştirmeyi güncelleştirin)

## <a name="overview"></a>Genel Bakış
**Jobs** blade, StorSimple Device Manager hizmetinize bağlı cihazlarda başlatılan işleri görüntülemek ve yönetmek için tek bir merkezi portal sağlar. Birden çok aygıt için zamanlanmış, çalışan, tamamlanmış, iptal edilmiş ve başarısız olan işleri görüntüleyebilirsiniz. Sonuçlar bir tabular biçimde sunulur.

![İşler bıçak](./media/storsimple-8000-manage-jobs-u2/jobs1.png)

Aşağıdaki gibi alanlarda filtre uygulayarak ilgilendiğiniz işleri hızlı bir şekilde bulabilirsiniz:

* **Durum** – İşler devam ediyor, başarılı olabilir, iptal edilebilir, başarısız olabilir, iptal edilebilir veya hatalarla başarılı olabilir.
* **Zaman aralığı** – İşler tarih ve saat aralığına göre filtrelenebilir. Aralıkları son 1 saat, son 24 saat, son 7 gün, son 30 gün, geçmiş yıl veya özel tarih.
* **Tür** – İş türü yedek, manuel yedekleme, yedekleme, klon hacmi, hacim kapsayıcıları üzerinde başarısız, yerel olarak sabitlenmiş hacim oluşturmak, ses düzeyini değiştirmek, güncellemeleri yüklemek, destek günlükleri toplamak ve bulut cihazı oluşturmak zamanlanabilir.
* **Cihazlar** – İşler, hizmetinize bağlı belirli bir cihazda başlatılır.
  
Filtre uygulanmış işler daha sonra aşağıdaki özniteliklere göre tablolanır:
  
* **Ad** – zamanlanmış yedekleme, manuel yedekleme, yedeklemeyi geri yükleme, klonklok ses birimi, birim kapsayıcıları üzerinde başarısız, yerel olarak sabitlenmiş birim oluşturma, ses düzeyini değiştir, güncelleştirmeleri yükleyin, destek günlükleri toplayın veya bulut cihazı oluşturun.
* **Durum** – çalışan, tamamlanmış, iptal edilmiş, başarısız olan, iptal eden veya hatalarla tamamlanan.
* **Entity** – İşler bir birim, bir yedekleme ilkesi veya aygıtla ilişkilendirilebilir. Örneğin, bir klon işi bir birimle ilişkilendirilirken, zamanlanmış yedekleme işi bir yedekleme ilkesiyle ilişkilidir. Bir aygıt işi, olağanüstü durum kurtarma (DR) veya geri yükleme işlemi sonucunda oluşturulur.
* **Aygıt** – İşin başlatıldıği aygıtın adı.
* **Başladı** – İşin başladığı zaman.
* **Süre** – İşi tamamlamak için gereken süre.

İş listesi her 30 saniyede bir yenilenir.

Bu sayfada iş ile ilgili aşağıdaki eylemleri gerçekleştirebilirsiniz:

* İş ayrıntılarını görüntüleme
* Bir işi iptal etme

## <a name="view-job-details"></a>İş ayrıntılarını görüntüleme
Herhangi bir işin ayrıntılarını görüntülemek için aşağıdaki adımları gerçekleştirin.

#### <a name="to-view-job-details"></a>İş ayrıntılarını görüntülemek için
1. StorSimple Device Manager hizmetinize gidin ve **ardından İşler'i**tıklatın.

2. **İşler** bıçağında, uygun filtrelere sahip bir sorgu çalıştırarak ilgilendiğiniz işi(ler) görüntüleyin. Tamamlanmış, çalışan veya iptal edilen işleri arayabilirsiniz.

    ![İş bıçağı](./media/storsimple-8000-manage-jobs-u2/jobs1.png)

2. Bir işi seçin ve tıklatın.

    ![İş bıçağı](./media/storsimple-8000-manage-jobs-u2/jobs3.png)

3. İş ayrıntıları blade, durumu, ayrıntıları, zaman istatistikleri ve veri istatistikleri görüntüleyebilirsiniz.
   
    ![İş ayrıntıları](./media/storsimple-8000-manage-jobs-u2/jobs4.png)

## <a name="cancel-a-job"></a>Bir işi iptal etme
Çalışan bir işi iptal etmek için aşağıdaki adımları gerçekleştirin.

> [!NOTE]
> Birim türünü değiştirmek için bir birimi değiştirme veya birim genişletme gibi bazı işler iptal edilemez.


### <a name="to-cancel-a-job"></a>Bir işi iptal etmek için
1. **İşler** sayfasında, uygun filtrelere sahip bir sorgu çalıştırarak iptal etmek istediğiniz çalışan iş(ler)i görüntüleyin. İşi seçin.

2. Bağlam menüsünü çağırmak için seçili işe sağ tıklayın ve **İptal'i**tıklatın.

    ![İş ayrıntıları](./media/storsimple-8000-manage-jobs-u2/jobs2.png)

3. Onayınız istendiğinde **Evet**’e tıklayın. Bu iş iptal edildi.

## <a name="next-steps"></a>Sonraki adımlar
* [StorSimple yedekleme ilkelerinizi nasıl yöneteceklerinizi](storsimple-8000-manage-backup-policies-u2.md)öğrenin.
* [StorSimple cihazınızı yönetmek için StorSimple Device Manager hizmetini](storsimple-8000-manager-service-administration.md)nasıl kullanacağınızı öğrenin.

