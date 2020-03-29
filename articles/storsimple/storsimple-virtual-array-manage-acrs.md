---
title: StorSimple Virtual Array için erişim kontrol kayıtlarını yönetme | Microsoft Dokümanlar
description: StorSimple Virtual Array'de hangi ana bilgisayarların bir birime bağlanabileceğini belirlemek için erişim denetim kayıtlarının (ACR' ler) nasıl yönetilenini açıklar.
services: storsimple
documentationcenter: ''
author: alkohli
manager: timlt
editor: ''
ms.assetid: e154bb4f-faab-4d92-a593-900c3ddc9595
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/27/2017
ms.author: alkohli
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 1dfc1b0e0576402624bfe62de0e206d9bd7cd1b0
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "64724422"
---
# <a name="use-storsimple-device-manager-to-manage-access-control-records-for-storsimple-virtual-array"></a>StorSimple Virtual Array için erişim kontrol kayıtlarını yönetmek için StorSimple Device Manager'ı kullanın

## <a name="overview"></a>Genel Bakış

Erişim denetim kayıtları (ACR'ler), StorSimple Virtual Array'de (StorSimple şirket içi sanal aygıt olarak da bilinir) hangi ana bilgisayarların bir birime bağlanabileceğini belirtmenize olanak tanır. ACRs belirli bir hacim için ayarlanır ve ev sahiplerinin iSCSI Nitelikli Adları (IQNs) içerir. Bir ana bilgisayar bir birime bağlanmaya çalıştığında, aygıt IQN adı için bu birimle ilişkili ACR'yi denetler ve eşleşme varsa, bağlantı kurulur. Aygıt Yöneticisi hizmetinizin **Yapılandırma** bölümündeki **Access denetim kayıtları,** tüm erişim kontrol kayıtlarını ana bilgisayarların ilgili IQN'larıyla görüntüler.

![Erişim denetimi kayıtlarını yönetme](./media/storsimple-virtual-array-manage-acrs/ova-manage-acrs.png)

Bu öğretici, acr ile ilgili aşağıdaki ortak görevleri açıklar:

* IQN'yi alın
* Erişim denetimi kaydı ekleme
* Erişim denetim kaydını edin
* Erişim denetimi kaydını silme

> [!IMPORTANT]
> 
> * Bir ses birimine ACR atarken, birim birimin aynı anda kümelenmemiş birden fazla ana bilgisayar tarafından erişilemediğini, çünkü bu birimin bozuk olabileceğine dikkat edin.
> * Bir ACR'yi bir birimden silerken, silme işlemi okuma yazma kesintisine neden olabileceğinden, ilgili ana bilgisayarın ses birimine erişemediğinden emin olun.


## <a name="get-the-iqn"></a>IQN'yi alın

Windows Server 2012'yi çalıştıran bir Windows ana bilgisayarının IQN'sini almak için aşağıdaki adımları gerçekleştirin.

[!INCLUDE [storsimple-get-iqn](../../includes/storsimple-get-iqn.md)]

## <a name="add-an-acr"></a>ACR ekleme

ACRs eklemek için StorSimple Device Manager hizmetinin **Yapılandırma** bölümünde **Access control records** blade'i kullanırsınız. Genellikle, bir ACR'yi tek bir ses düzeyiyle ilişkilendirin.

Bir ACR'yi bir birimle ilişkilendirme hakkında bilgi [için, bir birim eklemeye](storsimple-virtual-array-deploy3-iscsi-setup.md#step-3-add-a-volume)gidin.

> [!IMPORTANT]
> Bir ses birimine ACR atarken, birim birimin aynı anda kümelenmemiş birden fazla ana bilgisayar tarafından erişilemediğini, çünkü bu birimin bozuk olabileceğine dikkat edin.


ACR eklemek için aşağıdaki adımları gerçekleştirin.

#### <a name="to-add-an-acr"></a>ACR eklemek için

1. Hizmet açılış sayfasında, hizmetinizi seçin, hizmet adını çift tıklatın ve ardından **Yapılandırma** bölümünde **Access denetim kayıtlarını**tıklatın.
2. Access **denetim kayıtları** bıçak, **Ekle'yi**tıklatın.
3. **ACR ekle** bıçağında aşağıdakileri yapın:
   
    1. ACR’nize bir **Ad** verin.
    
    2. **iSCSI Başlatıcı Adı**altında, Windows ana bilgisayarınızın IQN adını sağlayın. Windows Server ana bilgisayarınızın IQN'sini almak için aşağıdakileri yapın:
   
    3. Microsoft iSCSI başlatıcısını Windows konağında başlatın. iSCSI Başlatıcısı Özellikleri penceresinin **Yapılandırma** sekmesinde, **Başlatıcı Adı** alanından dizeyi seçip kopyalayın.
    **ACR ekle** bıçağındaki **IQN** alanına bu dizeyi yapıştırın.
   
    6. ACR eklemek için **Ekle'yi** tıklatın.  
   
        ![Erişim denetimi kayıtları ekleme](./media/storsimple-virtual-array-manage-acrs/ova-add-acrs.png)
4. Tabular listesi bu eklemeyi yansıtacak şekilde güncelleştirilir.

## <a name="edit-an-acr"></a>Bir ACR'yi edin

ACRs'leri düzenlemek için Azure portalındaki Aygıt Yöneticisi hizmetinin **Yapılandırma** bölümündeki **Access denetim kayıt** bıçaklarını kullanırsınız.

> [!NOTE]
> Şu anda kullanılmakta olan bir ACR'yi değiştirmemelisiniz. Şu anda kullanılmakta olan bir birimle ilişkili bir ACR'yi düzenlemek için önce birimi çevrimdışı duruma çıkarmanız gerekir.


Bir ACR'yi gerçekleştirmek için aşağıdaki adımları gerçekleştirin.

#### <a name="to-edit-an-acr"></a>ACR'yi sağlamak için

1. Hizmet açılış sayfasında, hizmetinizi seçin, hizmet adını çift tıklatın ve ardından **Yapılandırma** bölümünde **Access denetim kayıtları.**
2. Access **denetim kayıtları** bıçak, erişim kontrol kayıtlarının tablo listesinden, değiştirmek istediğiniz ACR'yi çift tıklatın.
3. Erişim kontrol kayıtları blade **edit,** aşağıdakileri yapın:
   
    1. ACR için IQN'yi tedarik edin.
   
    2. Değiştirilmiş ACR'yi kaydetmek için bıçağın üst kısmında **kaydet'i** tıklatın. Aşağıdaki onay iletisini görürsünüz:
   
        ![Erişim denetim kayıtlarını edin](./media/storsimple-virtual-array-manage-acrs/ova-edit-acrs.png)

## <a name="delete-an-access-control-record"></a>Erişim denetimi kaydını silme

ACRs'i silmek için Azure portalındaki **Yapılandırma** sayfasını kullanırsınız.

> [!NOTE]
> 
> * Şu anda kullanılmakta olan bir ACR'yi silmemelisiniz. Şu anda kullanılmakta olan bir birimle ilişkili bir ACR'yi silmek için, önce birimi çevrimdışı duruma çıkarmanız gerekir.
> * Bir ACR'yi bir birimden silerken, silme işlemi okuma yazma kesintisine neden olabileceğinden, ilgili ana bilgisayarın ses birimine erişemediğinden emin olun.


Bir erişim denetim kaydını silmek için aşağıdaki adımları gerçekleştirin.

#### <a name="to-delete-an-access-control-record"></a>Erişim denetim kaydını silmek için

1. Hizmet açılış sayfasında, hizmetinizi seçin, hizmet adını çift tıklatın ve ardından **Yapılandırma** bölümünde **Access denetim kayıtları.**

2. Access **denetim kayıtları** bıçak, erişim kontrol kayıtlarının tablo listesinden, silmek istediğiniz ACR'yi çift tıklatın.

3. Access control records blade'i düzenle'de **Sil'i**tıklatın.
   
    ![ACR'leri Silme](./media/storsimple-virtual-array-manage-acrs/ova-del-acrs.png)

4. Onay istendiğinde, silme işlemine devam etmek için **Sil'i** tıklatın. Tabular listesi silmeyi yansıtacak şekilde güncelleştirilir.
   
   ![Uyarı iletisi](./media/storsimple-virtual-array-manage-acrs/ova-del-acrs-warning.png)

## <a name="next-steps"></a>Sonraki adımlar

* [Birim ekleme ve AcR'leri yapılandırma](storsimple-virtual-array-deploy3-iscsi-setup.md#step-3-add-a-volume)hakkında daha fazla bilgi edinin.

