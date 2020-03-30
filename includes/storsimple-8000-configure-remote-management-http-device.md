---
author: alkohli
ms.service: storsimple
ms.topic: include
ms.date: 10/26/2018
ms.author: alkohli
ms.openlocfilehash: 473bc0a58fe49c7f454c81402b57ddce7fc745b2
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "67188432"
---
#### <a name="to-configure-remote-management-on-cloud-appliance"></a>Bulut gerecinde uzaktan yönetimi yapılandırmak için

1. StorSimple Cihaz Yöneticisi hizmetinizde **Cihazlar**’a tıklayın. Hizmete bağlı cihazlar listesinden bulut gerecinizi seçin ve tıklayın.
    ![StorSimple bulut gereci seçme](./media/storsimple-8000-configure-remote-management-http-device/sca-remote-manage1.png)

2. **Ayarlar > Güvenlik** seçeneğine gidip **Güvenlik ayarları** dikey penceresini açın.

     ![StorSimple güvenlik ayarları](./media/storsimple-8000-configure-remote-management-http-device/sca-remote-manage2.png)

3. **Uzaktan Yönetim** bölümüne gidin. **Uzaktan yönetim ** kutusuna tıklayın.
     ![StorBasit uzaktan yönetim](./media/storsimple-8000-configure-remote-management-http-device/sca-remote-manage3.png)

4. **Uzaktan yönetim** dikey penceresinde:

    1. **Uzaktan yönetimi etkinleştir** seçeneğinin etkin olduğundan emin olun.
    2. Varsayılan olan HTTPS üzerinden bağlanmaktır. HTTP kullanarak bağlanmayı seçebilirsiniz. HTTP üzerinden bağlanma yalnızca güvenilen ağlarda kabul edilebilir. HTTP’nin etkin olduğundan emin olun.
    3. Bıçağın üstündeki komut çubuğundan... ** Daha fazla** ve sonra uzaktan yönetim sertifikası indirmek için **sertifikayı karşıdan yükleme'yi** tıklatın. Bu dosyanın kaydedileceği konumu belirtebilirsiniz. Bu sertifika, bulut gerecine bağlanmak için kullandığınız istemci veya konak makineye yüklenmelidir.

        ![Uzaktan yönetim bıçağı](./media/storsimple-8000-configure-remote-management-http-device/sca-remote-manage4.png)
5. **Kaydet**’e tıklayın ve sorulduğunda değişiklikleri onaylayın.