---
title: StorSimple Sanal dizisi için erişim denetim kayıtlarını yönetme | Microsoft Docs
description: StorSimple Sanal dizisindeki bir birime hangi ana bilgisayarların bağlanabilbileceğini belirleyen erişim denetimi kayıtlarının (ACRs) nasıl yönetileceğini açıklar.
services: storsimple
documentationcenter: ''
author: alkohli
manager: timlt
editor: ''
ms.assetid: e154bb4f-faab-4d92-a593-900c3ddc9595
ms.service: storsimple
ms.devlang: na
ms.topic: how-to
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/27/2017
ms.author: alkohli
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: bad0d7adfd77dff53b1582e63a91f2cd87a9233d
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85507628"
---
# <a name="use-storsimple-device-manager-to-manage-access-control-records-for-storsimple-virtual-array"></a>StorSimple Sanal dizisi için erişim denetim kayıtlarını yönetmek üzere StorSimple Aygıt Yöneticisi kullanma

## <a name="overview"></a>Genel Bakış

Erişim denetimi kayıtları (ACRs), StorSimple Sanal dizisindeki bir birime (StorSimple on-şirket içi sanal cihaz olarak da bilinir) hangi konaklara bağlanabileceği belirtmenizi sağlar. ACRs 'ler belirli bir birime ayarlanır ve konaklara ait Iscsı nitelenmiş adlarını (IQNs) içerir. Bir konak bir birime bağlanmaya çalıştığında, cihaz, ıQN adı için bu birimle ilişkili ACR 'yi denetler ve bir eşleşme varsa bağlantı oluşturulur. Aygıt Yöneticisi hizmetinizin **yapılandırma** bölümünde yer alan **erişim denetimi kayıtları** dikey penceresi, konaklara karşılık gelen tüm erişim denetim kayıtlarını görüntüler.

![Erişim denetim kayıtlarını yönetme](./media/storsimple-virtual-array-manage-acrs/ova-manage-acrs.png)

Bu öğreticide, aşağıdaki genel ACR ile ilgili görevler açıklanmaktadır:

* IQN 'yi al
* Erişim denetimi kaydı ekleme
* Erişim denetimi kaydını düzenleme
* Erişim denetimi kaydını silme

> [!IMPORTANT]
> 
> * Bir birime ACR atarken, bu birimi bozabileceğinden, birimin birden fazla kümelenmemiş konak tarafından eşzamanlı olarak erişilmediğinden emin olmanız gerekir.
> * Bir birimden bir ACR silinirken, silme işlemi okuma/yazma kesintiye neden olabileceğinden, karşılık gelen konağın birime erişmediğinden emin olun.


## <a name="get-the-iqn"></a>IQN 'yi al

Windows Server 2012 çalıştıran bir Windows konağının ıQN 'sini almak için aşağıdaki adımları gerçekleştirin.

[!INCLUDE [storsimple-get-iqn](../../includes/storsimple-get-iqn.md)]

## <a name="add-an-acr"></a>ACR ekleme

**Erişim denetimi kayıtları** dikey penceresini, storsimple Aygıt Yöneticisi hizmetinizin **yapılandırma** bölümünde, ACRS eklemek için kullanabilirsiniz. Genellikle, bir ACR 'yi bir birimle ilişkilendirirsiniz.

Bir ACR 'yi bir birimle ilişkilendirme hakkında daha fazla bilgi için, [birim ekleme](storsimple-virtual-array-deploy3-iscsi-setup.md#step-3-add-a-volume)bölümüne gidin.

> [!IMPORTANT]
> Bir birime ACR atarken, bu birimi bozabileceğinden, birimin birden fazla kümelenmemiş konak tarafından eşzamanlı olarak erişilmediğinden emin olmanız gerekir.


Bir ACR eklemek için aşağıdaki adımları gerçekleştirin.

#### <a name="to-add-an-acr"></a>ACR eklemek için

1. Hizmet giriş sayfasında, hizmetinizi seçin, hizmet adına çift tıklayın ve ardından **yapılandırma** bölümünde **erişim denetim kayıtları**' na tıklayın.
2. **Erişim denetimi kayıtları** dikey penceresinde **Ekle**' ye tıklayın.
3. **ACR Ekle** dikey penceresinde aşağıdakileri yapın:
   
    1. ACR’nize bir **Ad** verin.
    
    2. **Iscsı Başlatıcısı adı**altında Windows ana bilgisayarın IQN adını sağlayın. Windows Server konağının ıQN 'sini almak için aşağıdakileri yapın:
   
    3. Microsoft iSCSI başlatıcısını Windows konağında başlatın. iSCSI Başlatıcısı Özellikleri penceresinin **Yapılandırma** sekmesinde, **Başlatıcı Adı** alanından dizeyi seçip kopyalayın.
    Bu dizeyi, **Add ACR** dikey penceresindeki **IQN** alanına yapıştırın.
   
    6. ACR 'yi eklemek için **Ekle** ' ye tıklayın.  
   
        ![Erişim denetimi kayıtları ekleme](./media/storsimple-virtual-array-manage-acrs/ova-add-acrs.png)
4. Tablo listesi bu eklemeyi yansıtacak şekilde güncelleştirilir.

## <a name="edit-an-acr"></a>ACR 'yi düzenleme

ACRs 'yi düzenlemek için Azure portal Aygıt Yöneticisi hizmetinizin **yapılandırma** bölümünde bulunan **erişim denetim kayıtları** dikey penceresini kullanın.

> [!NOTE]
> Halen kullanılmakta olan bir ACR 'yi değiştirmemelisiniz. Kullanılmakta olan bir birimle ilişkili bir ACR 'yi düzenlemek için öncelikle birimi çevrimdışına almanız gerekir.


Bir ACR 'yi düzenlemek için aşağıdaki adımları gerçekleştirin.

#### <a name="to-edit-an-acr"></a>ACR 'yi düzenlemek için

1. Hizmet giriş sayfasında, hizmetinizi seçin, hizmet adına çift tıklayın ve ardından **yapılandırma** bölümünde **denetim kayıtlarına erişin**.
2. **Erişim denetimi kayıtları** dikey penceresinde, erişim denetimi kayıtlarının tablolu listesinden, değiştirmek istediğiniz ACR 'ye çift tıklayın.
3. **Erişim denetim kayıtlarını Düzenle** dikey penceresinde aşağıdakileri yapın:
   
    1. ACR için ıQN sağlayın.
   
    2. Değiştirilen ACR 'yi kaydetmek için dikey pencerenin en üstündeki **Kaydet** ' e tıklayın. Aşağıdaki onay iletisini görürsünüz:
   
        ![Erişim denetim kayıtlarını Düzenle](./media/storsimple-virtual-array-manage-acrs/ova-edit-acrs.png)

## <a name="delete-an-access-control-record"></a>Erişim denetimi kaydını silme

ACRs 'yi silmek için Azure portal **yapılandırma** sayfasını kullanın.

> [!NOTE]
> 
> * Halen kullanımda olan bir ACR 'yi silmemelisiniz. Kullanılmakta olan bir birimle ilişkili bir ACR 'yi silmek için öncelikle birimi çevrimdışına almanız gerekir.
> * Bir birimden bir ACR silinirken, silme işlemi okuma/yazma kesintiye neden olabileceğinden, karşılık gelen konağın birime erişmediğinden emin olun.


Bir erişim denetimi kaydını silmek için aşağıdaki adımları gerçekleştirin.

#### <a name="to-delete-an-access-control-record"></a>Bir erişim denetimi kaydını silmek için

1. Hizmet giriş sayfasında, hizmetinizi seçin, hizmet adına çift tıklayın ve ardından **yapılandırma** bölümünde **denetim kayıtlarına erişin**.

2. **Erişim denetimi kayıtları** dikey penceresinde, erişim denetimi kayıtlarının tablolu listesinden silmek istediğiniz ACR 'ye çift tıklayın.

3. Erişim denetim kayıtlarını Düzenle dikey penceresinde **Sil**' e tıklayın.
   
    ![ACRS 'yi silme](./media/storsimple-virtual-array-manage-acrs/ova-del-acrs.png)

4. Onay istendiğinde, silme işlemine devam etmek için **Sil** ' e tıklayın. Tablosal listesi, silme işlemini yansıtacak şekilde güncelleştirilir.
   
   ![Uyarı iletisi](./media/storsimple-virtual-array-manage-acrs/ova-del-acrs-warning.png)

## <a name="next-steps"></a>Sonraki adımlar

* [Birim ekleme ve ACRs yapılandırma](storsimple-virtual-array-deploy3-iscsi-setup.md#step-3-add-a-volume)hakkında daha fazla bilgi edinin.

