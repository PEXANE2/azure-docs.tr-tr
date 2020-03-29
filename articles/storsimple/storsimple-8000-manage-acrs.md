---
title: StorSimple'daki erişim kontrol kayıtlarını yönetme | Microsoft Dokümanlar
description: StorSimple aygıtındaki bir birime hangi ana bilgisayarların bağlanabileceğini belirlemek için erişim denetim kayıtlarının (ACR) nasıl kullanılacağını açıklar.
services: storsimple
documentationcenter: ''
author: alkohli
manager: timlt
editor: ''
ms.assetid: ''
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/31/2017
ms.author: alkohli
ms.openlocfilehash: ade7da25d2307a382c17e7a3cbb26b601c34ef78
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "64693246"
---
# <a name="use-the-storsimple-manager-service-to-manage-access-control-records"></a>Erişim denetimi kayıtlarını yönetmek için StorSimple Manager hizmetini kullanın

## <a name="overview"></a>Genel Bakış
Erişim denetim kayıtları (ACR'ler), StorSimple aygıtındaki bir birime hangi ana bilgisayarların bağlanabileceğini belirtmenize olanak tanır. ACRs belirli bir hacim için ayarlanır ve ev sahiplerinin iSCSI Nitelikli Adları (IQNs) içerir. Bir ana bilgisayar bir birime bağlanmaya çalıştığında, aygıt IQN adı için bu birimle ilişkili ACR'yi denetler ve eşleşme varsa, bağlantı kurulur. StorSimple Device Manager servis bıçağınızın **Yapılandırma** bölümündeki erişim kontrol kayıtları, tüm erişim kontrol kayıtlarını ana bilgisayarların ilgili IQN'larıyla görüntüler.

Bu öğretici, acr ile ilgili aşağıdaki ortak görevleri açıklar:

* Erişim denetimi kaydı ekleme
* Erişim denetim kaydını edin
* Erişim denetimi kaydını silme

> [!IMPORTANT]
> * Bir ses birimine ACR atarken, birim birimin aynı anda kümelenmemiş birden fazla ana bilgisayar tarafından erişilemediğini, çünkü bu birimin bozuk olabileceğine dikkat edin.
> * Bir ACR'yi bir birimden silerken, silme işlemi okuma yazma kesintisine neden olabileceğinden, ilgili ana bilgisayarın ses birimine erişemediğinden emin olun.

## <a name="get-the-iqn"></a>IQN'yi alın

Windows Server 2012'yi çalıştıran bir Windows ana bilgisayarının IQN'sini almak için aşağıdaki adımları gerçekleştirin.

[!INCLUDE [storsimple-get-iqn](../../includes/storsimple-get-iqn.md)]


## <a name="add-an-access-control-record"></a>Erişim denetimi kaydı ekleme
ACRs eklemek için StorSimple Device Manager servis bıçağındaki **Yapılandırma** bölümünü kullanırsınız. Genellikle, bir ACR'yi tek bir ses düzeyiyle ilişkilendireceksiniz.

ACR eklemek için aşağıdaki adımları gerçekleştirin.

#### <a name="to-add-an-acr"></a>ACR eklemek için

1. StorSimple Device Manager hizmetinize gidin, servis adını çift tıklatın ve ardından **Yapılandırma** bölümünde **Access denetim kayıtlarını**tıklatın.
2. Access **denetim kayıtları** bıçak, tıklayın **+ ACR ekle**.

    ![ACR ekle'yi tıklatın](./media/storsimple-8000-manage-acrs/createacr1.png)

3. **ACR ekle** bıçaklarında aşağıdaki adımları yapın:

    1. ACR için bir ad kaynağı.
    
    2. Windows Server ana bilgisayarınızın IQN adını **iSCSI Başlatıcı Adı (IQN)** altında sağlayın.

    3. ACR oluşturmak için **Ekle'yi** tıklatın.

        ![ACR ekle'yi tıklatın](./media/storsimple-8000-manage-acrs/createacr2.png)

4.  Yeni eklenen ACR, AKR'ların tabular listesinde görüntülenir.

    ![ACR ekle'yi tıklatın](./media/storsimple-8000-manage-acrs/createacr5.png)


## <a name="edit-an-access-control-record"></a>Erişim denetim kaydını edin
ACR'leri düzenlemek için StorSimple Device Manager servis bıçağındaki **Yapılandırma** bölümünü kullanırsınız.

> [!NOTE]
> Yalnızca şu anda kullanımda olmayan AC'leri değiştirmeniz önerilir. Şu anda kullanılmakta olan bir birimle ilişkili bir ACR'yi düzenletmek için önce birimi çevrimdışı duruma çıkarmanız gerekir.

Bir ACR'yi gerçekleştirmek için aşağıdaki adımları gerçekleştirin.

#### <a name="to-edit-an-access-control-record"></a>Erişim denetim kaydını da
1.  StorSimple Device Manager hizmetinize gidin, servis adını çift tıklatın ve ardından **Yapılandırma** bölümünde **Access denetim kayıtlarını**tıklatın.

    ![Denetim kayıtlarına eriş](./media/storsimple-8000-manage-acrs/createacr1.png)

2. Erişim denetimi kayıtlarının tablo listesinde, değiştirmek istediğiniz ACR'yi tıklatın ve seçin.

    ![Erişim denetim kayıtlarını edin](./media/storsimple-8000-manage-acrs/editacr1.png)

3. **Edit erişim kontrol kayıt** bıçağında, başka bir ana bilgisayara karşılık gelen farklı bir IQN sağlayın.

    ![Erişim denetim kayıtlarını edin](./media/storsimple-8000-manage-acrs/editacr2.png)

4. **Kaydet**'e tıklayın. Onayınız istendiğinde **Evet**’e tıklayın. 

    ![Erişim denetim kayıtlarını edin](./media/storsimple-8000-manage-acrs/editacr3.png)

5. ACR güncelleştirildiğinde size bildirilir. Tabular liste de değişikliği yansıtacak şekilde güncellenir.

   
## <a name="delete-an-access-control-record"></a>Erişim denetimi kaydını silme
ACRs'i silmek için StorSimple Device Manager servis bıçağındaki **Yapılandırma** bölümünü kullanırsınız.

> [!NOTE]
> Yalnızca şu anda kullanılmamakta olan AC'leri silebilirsiniz. Şu anda kullanılmakta olan bir birimle ilişkili bir ACR'yi silmek için önce birimi çevrimdışı duruma çıkarmanız gerekir.

Bir erişim denetim kaydını silmek için aşağıdaki adımları gerçekleştirin.

#### <a name="to-delete-an-access-control-record"></a>Erişim denetim kaydını silmek için
1.  StorSimple Device Manager hizmetinize gidin, servis adını çift tıklatın ve ardından **Yapılandırma** bölümünde **Access denetim kayıtlarını**tıklatın.

    ![Denetim kayıtlarına eriş](./media/storsimple-8000-manage-acrs/createacr1.png)

2. Erişim denetimi kayıtlarının tablo listesinde, silmek istediğiniz ACR'yi tıklatın ve seçin.

    ![Denetim kayıtlarına eriş](./media/storsimple-8000-manage-acrs/deleteacr1.png)

3. Bağlam menüsünü çağırmak için sağ tıklatın ve **Sil'i**seçin.

    ![Denetim kayıtlarına eriş](./media/storsimple-8000-manage-acrs/deleteacr2.png)

4. Onay istendiğinde, bilgileri gözden geçirin ve sonra **Sil'i**tıklatın.

    ![Denetim kayıtlarına eriş](./media/storsimple-8000-manage-acrs/deleteacr3.png)

5. Silme işlemi tamamlandığında size bildirilir. Tabular listesi silmeyi yansıtacak şekilde güncelleştirilir.

    ![Denetim kayıtlarına eriş](./media/storsimple-8000-manage-acrs/deleteacr5.png)

## <a name="next-steps"></a>Sonraki adımlar
* [StorSimple birimlerini yönetme](storsimple-8000-manage-volumes-u2.md)hakkında daha fazla bilgi edinin.
* [StorSimple cihazınızı yönetmek için StorSimple Manager hizmetini kullanma](storsimple-8000-manager-service-administration.md)hakkında daha fazla bilgi edinin.

