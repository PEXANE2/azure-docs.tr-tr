---
author: alkohli
ms.service: storsimple
ms.topic: include
ms.date: 10/26/2018
ms.author: alkohli
ms.openlocfilehash: 473bc0a58fe49c7f454c81402b57ddce7fc745b2
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2021
ms.locfileid: "67188432"
---
#### <a name="to-configure-remote-management-on-cloud-appliance"></a>Bulut gerecinde uzaktan yönetimi yapılandırmak için

1. StorSimple Cihaz Yöneticisi hizmetinizde **Cihazlar**’a tıklayın. Hizmete bağlı cihazlar listesinden bulut gerecinizi seçin ve tıklayın.
    ![StorSimple bulut gereci seçme](./media/storsimple-8000-configure-remote-management-http-device/sca-remote-manage1.png)

2. **Ayarlar > Güvenlik** seçeneğine gidip **Güvenlik ayarları** dikey penceresini açın.

     ![StorSimple güvenlik ayarları](./media/storsimple-8000-configure-remote-management-http-device/sca-remote-manage2.png)

3. **Uzaktan Yönetim** bölümüne gidin. **Uzaktan yönetim** kutusuna tıklayın.
     ![StorSimple uzaktan yönetimi](./media/storsimple-8000-configure-remote-management-http-device/sca-remote-manage3.png)

4. **Uzaktan yönetim** dikey penceresinde:

    1. **Uzaktan yönetimi etkinleştir** seçeneğinin etkin olduğundan emin olun.
    2. Varsayılan olan HTTPS üzerinden bağlanmaktır. HTTP kullanarak bağlanmayı seçebilirsiniz. HTTP üzerinden bağlanma yalnızca güvenilen ağlarda kabul edilebilir. HTTP’nin etkin olduğundan emin olun.
    3. Dikey pencerenin üst kısmındaki komut çubuğundan... öğesine tıklayın **. Daha fazla bilgi** ve ardından uzak yönetim sertifikasını indirmek için **sertifikayı indir** ' e tıklayın. Bu dosyanın kaydedileceği konumu belirtebilirsiniz. Bu sertifika, bulut gerecine bağlanmak için kullandığınız istemci veya konak makineye yüklenmelidir.

        ![Uzaktan Yönetim dikey penceresi](./media/storsimple-8000-configure-remote-management-http-device/sca-remote-manage4.png)
5. **Kaydet**’e tıklayın ve sorulduğunda değişiklikleri onaylayın.