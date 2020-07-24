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
ms.openlocfilehash: 30bbd06e36ed1e03caa391165a8abc275f1899a7
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87103064"
---
Bir birim kapsayıcısını silmek için şunları yapmanız gerekir
 - birim kapsayıcısında birimleri silin. Birim kapsayıcısının ilişkili birimleri varsa, önce bu birimleri çevrimdışına alın. [Birimi çevrimdışına alma](../articles/storsimple/storsimple-8000-manage-volumes-u2.md#take-a-volume-offline)bölümündeki adımları izleyin. Birimler çevrimdışı olduktan sonra bunları silebilirsiniz. 
 - ilişkili yedekleme ilkelerini ve bulut anlık görüntülerini silin. Birim kapsayıcısının ilişkili yedekleme ilkelerine ve bulut anlık görüntülerine sahip olup olmadığını denetleyin. Öyleyse, [yedekleme ilkelerini silin](../articles/storsimple/storsimple-8000-manage-backup-policies-u2.md#delete-a-backup-policy). Bu, bulut anlık görüntülerini da silecektir. 
 
Birim kapsayıcısının ilişkili birimleri, yedekleme ilkeleri ve bulut anlık görüntüleri yoksa, bunu silebilirsiniz. Bir birim kapsayıcısını silmek için aşağıdaki yordamı gerçekleştirin.

#### <a name="to-delete-a-volume-container"></a>Bir birim kapsayıcısını silmek için
1. StorSimple Cihaz Yöneticisi hizmetinize gidin ve **Cihazlar**’a tıklayın. Cihazı seçip tıklatın ve ardından **ayarlar > > birim Kapsayıcılarını Yönet**' e gidin.

    ![Birim kapsayıcıları dikey penceresi](./media/storsimple-8000-create-volume-container/createvolumecontainer2.png)

2. Birim kapsayıcılarının tablosal listesinden silmek istediğiniz birim kapsayıcısını seçin, **...** öğesine sağ tıklayın ve ardından **Sil**' i seçin.

    ![Birim kapsayıcısını Sil](./media/storsimple-8000-delete-volume-container/deletevolumecontainer1.png)

3. Bir birim kapsayıcısının ilişkili birimleri, yedekleme ilkeleri ve bulut anlık görüntüleri yoksa, bu durumda silinebilir. Onay istendiğinde, birim kapsayıcısını silmenin etkilerini belirten onay kutusunu işaretleyin ve seçin. Birim kapsayıcısını silmek için **Sil** ' e tıklayın.

    ![Silme işlemini onaylama](./media/storsimple-8000-delete-volume-container/deletevolumecontainer2.png)

Birim kapsayıcıları listesi, silinen birim kapsayıcısını yansıtacak şekilde güncelleştirilir.

![Birim kapsayıcısı sayfasının ekran görüntüsü. Birim kapsayıcılarının tablosal listesi artık silinen kapsayıcıyı içermiyor.](./media/storsimple-8000-delete-volume-container/deletevolumecontainer5.png)


