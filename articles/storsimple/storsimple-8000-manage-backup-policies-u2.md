---
title: StorSimple 8000 serisi yedekleme politikalarını yönet | Microsoft Dokümanlar
description: StorSimple 8000 serisi bir cihazda el ile yedeklemeler, yedekleme zamanlamaları ve yedekleme bekletme oluşturmak ve yönetmek için StorSimple Device Manager hizmetini nasıl kullanabileceğinizi açıklar.
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
ms.date: 07/05/2017
ms.author: alkohli
ms.openlocfilehash: 607379f8645226a031646376df9ca18f4d3164bf
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79267800"
---
# <a name="use-the-storsimple-device-manager-service-in-azure-portal-to-manage-backup-policies"></a>Yedekleme ilkelerini yönetmek için Azure portalındaki StorSimple Device Manager hizmetini kullanma


## <a name="overview"></a>Genel Bakış

Bu öğretici, StorSimple birimleriniz için yedekleme işlemlerini ve yedekleme tutma işlemlerini denetlemek için StorSimple Device Manager hizmeti **Yedekleme ilkesinin** nasıl kullanılacağını açıklar. Ayrıca, el ile yedeklemenin nasıl tamamlanır olduğunu da açıklar.

Bir birimi yedeklediğinizde, yerel bir anlık görüntü veya bulut anlık görüntüsü oluşturmayı seçebilirsiniz. Yerel olarak sabitlenmiş bir birimi yedekliyoruzsa, bir bulut anlık görüntüsü belirtmenizi öneririz. Yerel olarak sabitlenmiş bir birimin çok sayıda yerel anlık görüntüsünün alınması, çok fazla karmaşaya sahip bir veri setiile birleştiğinde, yerel alanın hızla bitebileceği bir duruma neden olur. Yerel anlık görüntüleri almayı seçerseniz, en son durumu yedeklemek için daha az günlük anlık görüntü almanızı, bunları bir gün tutmanızı ve sonra silmenizi öneririz.

Yerel olarak sabitlenmiş bir birimin bulut anlık görüntüsünü aldığınızda, yalnızca değiştirilen verileri çoğaltılıp sıkıştırıldığı buluta kopyalarsınız.

## <a name="the-backup-policy-blade"></a>Yedekleme ilkesi blade

StorSimple aygıtınız için **Yedekleme ilkesi,** yedekleme ilkelerini yönetmenize ve yerel ve bulut anlık görüntülerini zamanlamanıza olanak tanır. Yedekleme ilkeleri, bir birim koleksiyonu için yedekleme zamanlamalarını ve yedekleme bekletme işlemlerini yapılandırmak için kullanılır. Yedekleme ilkeleri, aynı anda birden çok cildin anlık görüntüsünü almanızı sağlar. Bu, yedekleme ilkesi tarafından oluşturulan yedeklemelerin kilitlenme tutarlı kopyaları olacağı anlamına gelir.

Yedekleme ilkeleri tabular listesi, varolan yedekleme ilkelerini aşağıdaki alanlardan birine veya birkaçına göre filtrelemenize de olanak tanır:

* **İlke adı** – İlkeyle ilişkili ad. Farklı türde ilkeler şunlardır:

  * Kullanıcı tarafından açıkça oluşturulan zamanlanmış ilkeler.
  * Başlangıçta StorSimple Snapshot Manager'da oluşturulan içe aktarılan ilkeler. Bunlar, ilkelerin aktarıldığı StorSimple Snapshot Manager ana bilgisayarını açıklayan bir etikete sahiptir.

  > [!NOTE]
  > Otomatik veya varsayılan yedekleme ilkeleri artık birim oluşturma sırasında etkinleştirilir.

* **Son başarılı yedekleme** – Bu ilkeyle alınan son başarılı yedeklemenin tarih ve saati.

* **Sonraki yedekleme** – Bu ilke tarafından başlatılacak bir sonraki zamanlanmış yedeklemenin tarih ve saati.

* **Birimler** – İlkeyle ilişkili birimler. Yedekleme ilkesiyle ilişkili tüm birimler, yedeklemeler oluşturulduğunda birlikte gruplandırılır.

* **Zamanlamalar** – Yedekleme ilkesiyle ilişkili zamanlama sayısı.

Yedekleme ilkeleri için sık kullanılan işlemler şunlardır:

* Yedekleme ilkesi ekleme
* Zamanlama ekleme veya değiştirme
* Ses düzeyi ekleme veya kaldırma
* Yedekleme ilkesini silme
* Manuel yedekleme alın

## <a name="add-a-backup-policy"></a>Yedekleme ilkesi ekleme

Yedeklemelerinizi otomatik olarak zamanlamak için bir yedekleme ilkesi ekleyin. İlk bir birim oluşturduğunuzda, biriminizle ilişkili varsayılan yedekleme ilkesi yoktur. Birim verilerini korumak için bir yedekleme ilkesi eklemeniz ve atamanız gerekir.

StorSimple aygıtınız için yedekleme ilkesi eklemek için Azure portalında aşağıdaki adımları gerçekleştirin. İlkeyi ekledikten sonra bir zamanlama tanımlayabilirsiniz [(bkz.](#add-or-modify-a-schedule)

[!INCLUDE [storsimple-8000-add-backup-policy-u2](../../includes/storsimple-8000-add-backup-policy-u2.md)]

## <a name="add-or-modify-a-schedule"></a>Zamanlama ekleme veya değiştirme

StorSimple cihazınızda varolan bir yedekleme ilkesine bağlı bir zamanlama ekleyebilir veya değiştirebilirsiniz. Zamanlama eklemek veya değiştirmek için Azure portalında aşağıdaki adımları gerçekleştirin.

[!INCLUDE [storsimple-8000-add-modify-backup-schedule](../../includes/storsimple-8000-add-modify-backup-schedule-u2.md)]


## <a name="add-or-remove-a-volume"></a>Ses düzeyi ekleme veya kaldırma

StorSimple cihazınızda yedekleme ilkesine atanan bir birim ekleyebilir veya kaldırabilirsiniz. Bir birim eklemek veya kaldırmak için Azure portalında aşağıdaki adımları gerçekleştirin.

[!INCLUDE [storsimple-8000-add-volume-backup-policy-u2](../../includes/storsimple-8000-add-remove-volume-backup-policy-u2.md)]


## <a name="delete-a-backup-policy"></a>Yedekleme ilkesini silme

StorSimple cihazınızdaki yedekleme ilkesini silmek için Azure portalında aşağıdaki adımları gerçekleştirin.

[!INCLUDE [storsimple-8000-delete-backup-policy](../../includes/storsimple-8000-delete-backup-policy.md)]

## <a name="take-a-manual-backup"></a>Manuel yedekleme alın

Tek bir birim için isteğe bağlı (manuel) yedekleme oluşturmak için Azure portalında aşağıdaki adımları gerçekleştirin.

[!INCLUDE [storsimple-8000-create-manual-backup](../../includes/storsimple-8000-create-manual-backup.md)]

## <a name="next-steps"></a>Sonraki adımlar

[StorSimple cihazınızı yönetmek için StorSimple Device Manager hizmetini kullanma](storsimple-8000-manager-service-administration.md)hakkında daha fazla bilgi edinin.

