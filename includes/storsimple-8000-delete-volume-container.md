---
title: include dosyası
description: include dosyası
services: storsimple
author: alkohli
ms.service: storsimple
ms.topic: include
ms.date: 06/08/2018
ms.author: alkohli
ms.custom: include file
ms.openlocfilehash: e683d17422321b780a1c01b3011292f2e2c631cb
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "67188418"
---
Bir birim kapsayıcısını silmek için,
 - birim kapsayıcısındaki birimleri silin. Birim kapsayıcıda ilişkili birimler varsa, önce bu birimleri çevrimdışına alın. Birim çevrimdışı [olarak ses düzeyini al'daki](../articles/storsimple/storsimple-8000-manage-volumes-u2.md#take-a-volume-offline)adımları izleyin. Birimler çevrimdışı olduktan sonra bunları silebilirsiniz. 
 - ilişkili yedekleme ilkelerini ve bulut anlık görüntülerini silin. Birim kapsayıcının ilişkili yedekleme ilkeleri ve bulut anlık görüntüleri olup olmadığını denetleyin. Bu nedenle, [yedekleme ilkelerini silin.](../articles/storsimple/storsimple-8000-manage-backup-policies-u2.md#delete-a-backup-policy) Bu, bulut anlık görüntülerini de siler. 
 
Birim kapsayıcısının ilişkili birimleri, yedekleme ilkeleri ve bulut anlık görüntüleri yoksa, bunu silebilirsiniz. Bir birim kapsayıcısını silmek için aşağıdaki yordamı gerçekleştirin.

#### <a name="to-delete-a-volume-container"></a>Birim kapsayıcısını silmek için
1. StorSimple Cihaz Yöneticisi hizmetinize gidin ve **Cihazlar**’a tıklayın. Aygıtı seçin ve tıklatın ve ardından **Ayarlar > > Birim kapsayıcılarını yönet'e**gidin.

    ![Hacim kapları bıçak](./media/storsimple-8000-create-volume-container/createvolumecontainer2.png)

2. Birim kapsayıcıların tabular listesinden, silmek istediğiniz birim kapsayıcısını seçin, sağ tıklayın **...** ve sonra **Sil'i**seçin.

    ![Ses kapsayıcısı silme](./media/storsimple-8000-delete-volume-container/deletevolumecontainer1.png)

3. Bir birim kapsayıcıda ilişkili birimler, yedekleme ilkeleri ve bulut anlık görüntüleri yoksa, silinebilir. Onay istendiğinde, birim kapsayıcısını silmenin etkisini belirten onay kutusunu gözden geçirin ve seçin. Ardından birim kapsayıcısını silmek için **Sil'i** tıklatın.

    ![Silme işlemini onaylama](./media/storsimple-8000-delete-volume-container/deletevolumecontainer2.png)

Birim kapsayıcıların listesi silinen birim kapsayıcıyı yansıtacak şekilde güncelleştirilir.

![](./media/storsimple-8000-delete-volume-container/deletevolumecontainer5.png)


