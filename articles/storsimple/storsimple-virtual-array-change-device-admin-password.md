---
title: StorSimple Virtual Array cihaz yönetici şifredeğiştir | Microsoft Dokümanlar
description: Aygıt yöneticisi parolasını değiştirmek için Azure portalının veya StorSimple Virtual Array web Kullanıcı Arabirimi'nin nasıl kullanılacağını açıklar.
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: ''
ms.assetid: 11490814-d9fd-4dc7-9c3b-55dd2c23eaf1
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 02/27/2017
ms.author: alkohli
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 5308badf439254062a8aefca1840eb21bc234ace
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "60580409"
---
# <a name="change-the-storsimple-virtual-array-device-administrator-password-via-storsimple-device-manager"></a>StorSimple Device Manager ile StorSimple Virtual Array aygıt yöneticisi parolasını değiştirme

## <a name="overview"></a>Genel Bakış

StorSimple Virtual Array'e erişmek için Windows PowerShell arabirimini kullandığınızda, aygıt yöneticisi parolasını girmeniz gerekir. StorSimple aygıtı ilk olarak sağlandığında ve başlatıldığında, varsayılan parola *Password1'dir.* Verilerinizin güvenliği için varsayılan parolailk oturum açğınızda sona erer ve bu parolayı değiştirmeniz gerekir.

Aygıt üretim ortamınızda dağıtıldıktan sonra istediğiniz zaman aygıt yöneticisi parolasını değiştirmek için yerel web UI'yi veya Azure portalını da kullanabilirsiniz. Bu yordamların her biri bu makalede açıklanmıştır.

 ![cihazlar bıçak](./media/storsimple-virtual-array-change-device-admin-password/ova-devices-blade.png)

## <a name="use-the-azure-portal-to-change-the-password"></a>Parolayı değiştirmek için Azure portalını kullanma

Azure portalı üzerinden aygıt yöneticisi parolasını değiştirmek için aşağıdaki adımları gerçekleştirin.

#### <a name="to-change-the-device-administrator-password-via-the-azure-portal"></a>Azure portalı üzerinden aygıt yöneticisi parolasını değiştirmek için

1. Hizmet açılış sayfasında, hizmetinizi seçin, hizmet adını çift tıklatın ve ardından **Yönetim** bölümünde **Cihazlar'ı**tıklatın. Bu, tüm StorSimple Sanal Dizi aygıtlarınızı listeleyen **Cihazlar** bıçağını açar.

2. **Cihazlar** bıçağında, parola değişikliği gerektiren aygıta çift tıklayın.

3. Cihazınızın **Ayarlar** bıtır'ına **güvenlik**'i tıklatın.

4. Güvenlik **Ayarları** bıçak, aşağıdakileri yapın:
   
   1. Aygıt Yöneticisi **Parola** bölümüne aşağı kaydırın. 8 ila 15 karakter içeren bir yönetici parolası sağlayın.
   2. Parola’yı onaylayın.
   3. Dikey pencerenin en üstündeki **Kaydet**'e tıklayın.

Aygıt yöneticisi parolası artık güncelleştirildi. Aygıta yerel olarak erişmek için bu değiştirilmiş parolayı kullanabilirsiniz.

![Güvenlik ayarları blade](./media/storsimple-virtual-array-change-device-admin-password/ova-change-device-pwd.png)

## <a name="use-the-local-web-ui-to-change-the-password"></a>Parolayı değiştirmek için yerel web UI'ı kullanma

Yerel web UI aracılığıyla aygıt yöneticisi parolasını değiştirmek için aşağıdaki adımları gerçekleştirin.

#### <a name="to-change-the-device-administrator-password-via-the-local-web-ui"></a>Yerel web UI üzerinden aygıt yöneticisi parolasını değiştirmek için

1. Yerel web UI'sinde, cihazınız için **Bakım** > **Parlemi değişikliğini** tıklatın.
   
    ![parolayı değiştir1](./media/storsimple-virtual-array-change-device-admin-password/image40.png)
2. Geçerli **parolayı**girin.
3. Yeni Bir **Parola**Sağlayın. Parola en az 8 karakter uzunluğunda olmalıdır. Aşağıdakilerden 4'ün 3'ünü içermelidir: büyük harf, küçük harf, sayısal ve özel karakterler.
   
    Parolanızın son 24 parolayla aynı olamayacağını unutmayın.
4. Onaylamak için parolayı yeniden girin.
   
    ![parolayı değiştir2](./media/storsimple-virtual-array-change-device-admin-password/image41.png)
5. Sayfanın alt kısmında **Uygula'yı**tıklatın. Yeni parola şimdi uygulanır. Parola değişikliği başarılı olmazsa, aşağıdaki hatayı görürsünüz:
   
    ![parola hatası](./media/storsimple-virtual-array-change-device-admin-password/image42.png)
   
    Parola başarıyla güncelleştirildikten sonra size bildirilir. Daha sonra aygıta yerel olarak erişmek için bu değiştirilmiş parolayı kullanabilirsiniz.


## <a name="next-steps"></a>Sonraki adımlar
[StorSimple Sanal Dizinizi nasıl yöneteceklerinizi](storsimple-ova-web-ui-admin.md)öğrenin.

