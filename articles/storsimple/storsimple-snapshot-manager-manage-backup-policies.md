---
title: StorSimple Snapshot Manager yedekleme ilkeleri | Microsoft Dokümanlar
description: Zamanlanmış yedeklemeleri kontrol eden yedekleme ilkeleri oluşturmak ve yönetmek için StorSimple Snapshot Manager MMC snap-in'in nasıl kullanılacağını açıklar.
services: storsimple
documentationcenter: NA
author: twooley
manager: timlt
editor: ''
ms.assetid: 04415d0b-42f0-4737-8afa-257fb2dbe5d0
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 06/05/2017
ms.author: twooley
ms.openlocfilehash: 7b2df6c8384e78094d80a17d8982b0db9edf73e1
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75933353"
---
# <a name="use-storsimple-snapshot-manager-to-create-and-manage-backup-policies"></a>Yedekleme ilkeleri oluşturmak ve yönetmek için StorSimple Snapshot Manager'ı kullanın
## <a name="overview"></a>Genel Bakış
Yedekleme ilkesi, birim verilerini yerel olarak veya bulutta yedeklemek için bir zamanlama oluşturur. Bir yedekleme ilkesi oluşturduğunuzda, bekletme ilkesi de belirtebilirsiniz. (En fazla 64 anlık görüntü tutabilirsiniz.) Yedekleme ilkeleri hakkında daha fazla bilgi için [StorSimple 8000 serisindeki](storsimple-overview.md)Yedekleme [türlerine](storsimple-what-is-snapshot-manager.md#backup-types-and-backup-policies) bakın: karma bulut çözümü.

Bu öğreticide, aşağıdaki işlemlerin nasıl yapılacağı açıklanmaktadır:

* Yedekleme ilkesi oluşturma
* Yedekleme ilkesini edin
* Yedekleme ilkesini silme

## <a name="create-a-backup-policy"></a>Yedekleme ilkesi oluşturma
Yeni bir yedekleme ilkesi oluşturmak için aşağıdaki yordamı kullanın.

#### <a name="to-create-a-backup-policy"></a>Yedekleme ilkesi oluşturmak için
1. StorSimple Snapshot Manager'ı başlatmak için masaüstü simgesini tıklatın.
2. **Kapsam** bölmesinde, **Yedekleme İlkeleri'ni**sağ tıklatın ve **Yedekleme İlkesi Oluştur'u**tıklatın.

    ![Yedekleme ilkesi oluşturma](./media/storsimple-snapshot-manager-manage-backup-policies/HCS_SSM_Create_BU_policy.png)

    **İlke Oluştur** iletişim kutusu görüntülenir.

    ![İlke Oluşturma - Genel sekme](./media/storsimple-snapshot-manager-manage-backup-policies/HCS_SSM_Create_policy_general.png)
3. **Genel** sekmesinde, aşağıdaki bilgileri doldurun:

   1. **Ad** metin kutusuna, ilke için bir ad yazın.
   2. Birim **grubu** metin kutusuna, ilkeyle ilişkili birim grubunun adını yazın.
   3. Yerel **Anlık Görüntü** veya **Bulut Anlık Görüntü'nü**seçin.
   4. Saklamak için anlık görüntü sayısını seçin. **Tüm'ü**seçerseniz, 64 anlık görüntü korunur (en fazla).
4. **Zamanlama** sekmesine tıklayın.

    ![İlke Oluşturma - Zamanlama sekmesi](./media/storsimple-snapshot-manager-manage-backup-policies/HCS_SSM_Create_policy_schedule.png)
5. **Zamanlama** sekmesinde, aşağıdaki bilgileri doldurun:

   1. Bir sonraki yedeklemeyi zamanlamak için **Etkinleştir** onay kutusunu tıklatın.
   2. **Ayarlar**altında, **Bir kez**seçin , **Günlük**, **Haftalık**, veya **Aylık**.
   3. **Başlangıç** metin kutusunda, takvim simgesini tıklatın ve bir başlangıç tarihi seçin.
   4. **Gelişmiş Ayarlar**altında isteğe bağlı yineleme zamanlamaları ve bitiş tarihi ayarlayabilirsiniz.
   5. **Tamam**'a tıklayın.

Bir yedekleme ilkesi oluşturduktan sonra, **Sonuçlar** bölmesinde aşağıdaki bilgiler görüntülenir:

* **Ad** – yedekleme ilkesinin adı.
* **Yazın** – yerel anlık görüntü veya bulut anlık görüntüsü.
* **Birim Grubu** – ilkeyle ilişkili birim grubu.
* **Bekletme** – tutulan anlık görüntü sayısı; maksimum 64'dür.
* **Oluşturuldu** – bu iipolitikasının oluşturulduğu tarih.
* **Etkin** – politikanın şu anda geçerli olup olmadığı: **True,** etkin olduğunu gösterir; **False,** etkin olmadığını gösterir.

## <a name="edit-a-backup-policy"></a>Yedekleme ilkesini edin
Varolan bir yedekleme ilkesini yeniden sağlamak için aşağıdaki yordamı kullanın.

#### <a name="to-edit-a-backup-policy"></a>Yedekleme ilkesini sağlamak için
1. StorSimple Snapshot Manager'ı başlatmak için masaüstü simgesini tıklatın.
2. **Kapsam** bölmesinde, Yedekleme **İlkeleri** düğümünü tıklatın. Tüm yedekleme ilkeleri **Sonuçlar** bölmesinde görünür.
3. Yeniden görüntülemek istediğiniz ilkeyi sağ tıklatın ve sonra **Edit'i**tıklatın.

    ![Yedekleme ilkesini edin](./media/storsimple-snapshot-manager-manage-backup-policies/HCS_SSM_Edit_BU_policy.png)
4. **İlke Oluştur** penceresi göründüğünde, değişikliklerinizi girin ve ardından **Tamam'ı**tıklatın.

## <a name="delete-a-backup-policy"></a>Yedekleme ilkesini silme
Yedekleme ilkesini silmek için aşağıdaki yordamı kullanın.

#### <a name="to-delete-a-backup-policy"></a>Yedekleme ilkesini silmek için
1. StorSimple Snapshot Manager'ı başlatmak için masaüstü simgesini tıklatın.
2. **Kapsam** bölmesinde, Yedekleme **İlkeleri** düğümünü tıklatın. Tüm yedekleme ilkeleri **Sonuçlar** bölmesinde görünür.
3. Silmek istediğiniz yedekleme ilkesini sağ tıklatın ve sonra **Sil'i**tıklatın.
4. Onay iletisi göründüğünde **Evet'i**tıklatın.

    ![Yedekleme ilkesi onayını silme](./media/storsimple-snapshot-manager-manage-backup-policies/HCS_SSM_Delete_BU_policy.png)

## <a name="next-steps"></a>Sonraki adımlar
* [StorSimple çözümünüzü yönetmek için StorSimple Snapshot Manager'ı](storsimple-snapshot-manager-admin.md)nasıl kullanacağınızı öğrenin.
* [Yedekleme işlerini görüntülemek ve yönetmek için StorSimple Snapshot Manager'ı](storsimple-snapshot-manager-manage-backup-jobs.md)nasıl kullanacağınızı öğrenin.
