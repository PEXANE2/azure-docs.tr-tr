---
title: StorSimple Sanal dizisi cihaz yönetici parolasını değiştirme | Microsoft Docs
description: Cihaz Yöneticisi parolasını değiştirmek için Azure portal veya StorSimple Sanal dizisi Web Kullanıcı arabirimi 'nin nasıl kullanılacağını açıklar.
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: ''
ms.assetid: 11490814-d9fd-4dc7-9c3b-55dd2c23eaf1
ms.service: storsimple
ms.devlang: NA
ms.topic: how-to
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 02/27/2017
ms.author: alkohli
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 8bc8846d546faec194617ccb753cdbd105e16bf9
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85513627"
---
# <a name="change-the-storsimple-virtual-array-device-administrator-password-via-storsimple-device-manager"></a>StorSimple Sanal dizisi Cihaz Yöneticisi parolasını StorSimple ile değiştirin Aygıt Yöneticisi

## <a name="overview"></a>Genel Bakış

StorSimple Sanal dizisine erişmek için Windows PowerShell arabirimini kullandığınızda, bir cihaz yöneticisi parolası girmeniz gerekir. StorSimple cihazı ilk sağlandıysa ve başlatıldığında varsayılan parola *Parola1*olur. Verilerinizin güvenliği için, ilk kez oturum açtığınızda varsayılan parolanın süresi dolar ve bu parolayı değiştirmeniz gerekir.

Cihaz yönetici parolasını, cihaz üretim ortamınızda dağıtıldıktan sonra dilediğiniz zaman değiştirmek için yerel Web Kullanıcı arabirimini veya Azure portal de kullanabilirsiniz. Bu yordamların her biri bu makalede açıklanmıştır.

 ![cihazlar dikey penceresi](./media/storsimple-virtual-array-change-device-admin-password/ova-devices-blade.png)

## <a name="use-the-azure-portal-to-change-the-password"></a>Parolayı değiştirmek için Azure portal kullanın

Azure portal aracılığıyla cihaz yöneticisi parolasını değiştirmek için aşağıdaki adımları gerçekleştirin.

#### <a name="to-change-the-device-administrator-password-via-the-azure-portal"></a>Azure portal aracılığıyla cihaz yöneticisi parolasını değiştirmek için

1. Hizmet giriş sayfasında, hizmetinizi seçin, hizmet adına çift tıklayın ve ardından **Yönetim** bölümünde **cihazlar**' a tıklayın. Bu, tüm StorSimple Sanal dizi cihazlarınızı listeleyen **cihazlar** dikey penceresini açar.

2. **Cihazlar** dikey penceresinde, parola değişikliği gerektiren cihaza çift tıklayın.

3. Cihazınızın **Ayarlar** dikey penceresinde **güvenlik**' e tıklayın.

4. **Güvenlik ayarları** dikey penceresinde aşağıdakileri yapın:
   
   1. **Cihaz Yöneticisi parolası** bölümüne gidin. 8 ile 15 arasında karakter içeren bir yönetici parolası sağlayın.
   2. Parola’yı onaylayın.
   3. Dikey pencerenin en üstündeki **Kaydet**'e tıklayın.

Cihaz yönetici parolası artık güncelleştirildi. Bu değiştirilmiş parolayı, cihaza yerel olarak erişmek için kullanabilirsiniz.

![Güvenlik ayarları dikey penceresi](./media/storsimple-virtual-array-change-device-admin-password/ova-change-device-pwd.png)

## <a name="use-the-local-web-ui-to-change-the-password"></a>Parolayı değiştirmek için yerel Web Kullanıcı arabirimini kullanma

Yerel Web Kullanıcı arabirimi aracılığıyla cihaz yöneticisi parolasını değiştirmek için aşağıdaki adımları gerçekleştirin.

#### <a name="to-change-the-device-administrator-password-via-the-local-web-ui"></a>Yerel Web Kullanıcı arabirimi aracılığıyla cihaz yöneticisi parolasını değiştirmek için

1. Yerel Web Kullanıcı arabiriminde cihazınız için **bakım**  >  **parolası değiştirme** ' ye tıklayın.
   
    ![Parola1 Değiştir](./media/storsimple-virtual-array-change-device-admin-password/image40.png)
2. **Geçerli parolayı**girin.
3. Yeni bir **parola**belirtin. Parola en az 8 karakter uzunluğunda olmalıdır. Aşağıdakilerin 3/4 ' ü içermesi gerekir: büyük harf, küçük harf, sayısal ve özel karakterler.
   
    Parolanızın son 24 parolayla aynı olamayacağını unutmayın.
4. Onaylamak için parolayı yeniden girin.
   
    ![paSsWorD2 Değiştir](./media/storsimple-virtual-array-change-device-admin-password/image41.png)
5. Sayfanın alt kısmındaki **Uygula**' ya tıklayın. Yeni parola artık uygulandı. Parola değişikliği başarılı olmazsa, şu hatayı görürsünüz:
   
    ![parola hatası](./media/storsimple-virtual-array-change-device-admin-password/image42.png)
   
    Parola başarıyla güncelleştirildikten sonra bilgilendirirsiniz. Daha sonra bu değiştirme parolasını, cihaza yerel olarak erişmek için kullanabilirsiniz.


## <a name="next-steps"></a>Sonraki adımlar
[StorSimple Sanal dizinizi yönetmeyi](storsimple-ova-web-ui-admin.md)öğrenin.

