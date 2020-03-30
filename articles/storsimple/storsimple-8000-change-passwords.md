---
title: StorSimple şifrelerinizi değiştirin | Microsoft Dokümanlar
description: StorSimple Snapshot Manager ve aygıt yöneticisi parolalarınızı değiştirmek için StorSimple Device Manager hizmetini nasıl kullanacağınızı açıklar.
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
ms.date: 07/03/2017
ms.author: alkohli
ms.openlocfilehash: ab874bbdcd47a4bfa9abfba721afa46d0f23a338
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79268034"
---
# <a name="use-the-storsimple-device-manager-service-to-change-your-storsimple-passwords"></a>StorSimple parolalarınızı değiştirmek için StorSimple Device Manager hizmetini kullanın

## <a name="overview"></a>Genel Bakış
Azure portalı **Aygıt ayarları** seçeneği, StorSimple Aygıt Yöneticisi hizmeti tarafından yönetilen bir StorSimple aygıtında yeniden yapılandırabileceğiniz tüm aygıt parametrelerini içerir. Bu öğretici, aygıt yöneticinizi veya StorSimple Snapshot Manager parolanızı değiştirmek için **Aygıt ayarları** altında **Güvenlik** seçeneğini nasıl kullanabileceğinizi açıklar.

## <a name="change-the-device-administrator-password"></a>Cihaz yöneticisi parolasını değiştirme
StorSimple aygıtına erişmek için Windows PowerShell arabirimini kullandığınızda, aygıt yöneticisi parolasını girmeniz gerekir. İlk StorSimple aygıtı bir hizmete kaydedildiğinde, bu arabirimin varsayılan *parolası Password1'dir.* Verilerinizin güvenliği için, kayıt işleminin sonunda bu parolayı değiştirmeniz gerekir. Bu parolayı değiştirmeden kayıt işleminden çıkamazsınız. Daha fazla bilgi için [Bkz. Adım 3: StorSimple için Windows PowerShell üzerinden aygıtı yapılandırın ve](storsimple-8000-deployment-walkthrough-u2.md#step-3-configure-and-register-the-device-through-windows-powershell-for-storsimple)kaydedin.

Kayıt sırasında ilk olarak Windows PowerShell arabirimi üzerinden ayarlanan parola daha sonra Azure portalı üzerinden değiştirilebilir. Aygıt yöneticisi parolasını değiştirmek için aşağıdaki adımları gerçekleştirin.

#### <a name="to-change-the-device-administrator-password"></a>Aygıt yöneticisi parolasını değiştirmek için
1. StorSimple Cihaz Yöneticisi hizmetinize gidin ve **Cihazlar**’a tıklayın.

2. Aygıtların tabular listesinden, parolasını değiştirmeyi planladığınız aygıtı seçin ve tıklatın.

    ![](./media/storsimple-8000-change-passwords/changepwd1.png)

3. **Ayarlar** bıçağında, **Güvenlik > Aygıt ayarlarına**gidin.

    ![](./media/storsimple-8000-change-passwords/changepwd2.png)

4. Güvenlik **ayarları** çubuğunda, aygıt yöneticisi parolasını değiştirmek için **Parola'yı** tıklatın.

    ![](./media/storsimple-8000-change-passwords/changepwd3.png)

5. **Parola** bıyıcayında, 8 ila 15 karakter içeren bir yönetici parolası sağlayın. Parola, büyük harf, küçük harf, sayısal ve özel karakterlerin 3 veya daha fazlasının bir leşimi olmalıdır.

6. Parola’yı onaylayın.

    ![](./media/storsimple-8000-change-passwords/changepwd4.png)

7. **Kaydet'i** tıklatın ve onay istendiğinde **Evet'i**tıklatın.

    ![](./media/storsimple-8000-change-passwords/changepwd6.png)

Aygıt yöneticisi parolası şimdi güncelleştirilmelidir. Windows PowerShell arabirimine erişmek için bu değiştirilmiş parolayı kullanabilirsiniz.

## <a name="set-the-storsimple-snapshot-manager-password"></a>StorSimple Snapshot Manager parolasını ayarlama
StorSimple Snapshot Manager yazılımı Windows ana bilgisayarınıza bulunur ve yöneticilerin yerel ve bulut anlık görüntüleri biçiminde StorSimple cihazınızın yedeklerini yönetmelerine olanak tanır.

StorSimple Snapshot Manager'da bir aygıtı yapılandırırken, depolama aygıtınızın kimliğini doğrulamak için aygıtIP adresini ve parolasını sağlamanız istenir.

Azure portalı üzerinden StorSimple Snapshot Manager'ın parolasını ayarlayabilir veya değiştirebilirsiniz. StorSimple Snapshot Manager parolasını ayarlamak veya değiştirmek için aşağıdaki adımları gerçekleştirin.

#### <a name="to-set-the-storsimple-snapshot-manager-password"></a>StorSimple Snapshot Manager parolasını ayarlamak için
1. StorSimple Cihaz Yöneticisi hizmetinize gidin ve **Cihazlar**’a tıklayın.

2. Aygıtların tabular listesinden, StorSimple Snapshot Manager parolasını ayarlamayı veya değiştirmeyi istediğiniz cihazı seçin ve tıklatın.

     ![](./media/storsimple-8000-change-passwords/changepwd1.png)

3. **Ayarlar** bıçağında, **Güvenlik > Aygıt ayarlarına**gidin.

     ![](./media/storsimple-8000-change-passwords/changepwd2.png)

4. Güvenlik **ayarları** çubuğunda, StorSimple Snapshot Manager parolasını ayarlamak veya değiştirmek için **Parola'yı** tıklatın.

     ![](./media/storsimple-8000-change-passwords/changepwd3.png) 

5. **Parola** bıçağına 14 veya 15 karakterlik bir parola girin. Parolanın büyük harf, küçük harf, sayısal ve özel karakterlerin 3 veya daha fazla bir leşimini içerdiğinden emin olun.

6. Parola’yı onaylayın.

     ![](./media/storsimple-8000-change-passwords/changepwd5.png)

7. **Kaydet'i** tıklatın ve onay istendiğinde **Evet'i**tıklatın.

     ![](./media/storsimple-8000-change-passwords/changepwd6.png)

StorSimple Snapshot Manager parolası şimdi güncelleştirilmelidir.

## <a name="next-steps"></a>Sonraki adımlar
* [StorSimple güvenliği](storsimple-8000-security.md)hakkında daha fazla bilgi edinin.
* [Aygıt yapılandırmanızı değiştirme](storsimple-8000-modify-device-config.md)hakkında daha fazla bilgi edinin.
* [StorSimple cihazınızı yönetmek için StorSimple Device Manager hizmetini kullanma](storsimple-8000-manager-service-administration.md)hakkında daha fazla bilgi edinin.

