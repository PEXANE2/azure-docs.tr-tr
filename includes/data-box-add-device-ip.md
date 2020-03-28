---
author: alkohli
ms.service: databox
ms.topic: include
ms.date: 12/07/2018
ms.author: alkohli
ms.openlocfilehash: e4b366075cb16f62a0e16b5b06da6fb19ffefdb9
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "67189036"
---
1. Veri Kutusu aygıtında oturum açın. Kilidi niçin kilitlendikinden emin olun.

    ![Veri Kutusu panosu](media/data-box-add-device-ip/data-box-connect-via-rest-1.png)

2. Ağ **arabirimlerini ayarla'ya**gidin. İstemciye bağlanmak için kullanılan ağ arabirimi için aygıt IP adresini not alın.

    ![Veri Kutusu panosu](media/data-box-add-device-ip/data-box-connect-via-rest-2.png)

3. **Bağlan'a gidin ve Birleştir'i kopyalayın** ve **Geri Le'yi**tıklatın.

    ![Veri Kutusu panosu](media/data-box-add-device-ip/data-box-connect-via-rest-3.png)

4. Access **Depolama hesabından ve veri yükleme** iletişim kutusundan **Blob Service Endpoint'i**kopyalayın.

    ![Veri Kutusu panosu](media/data-box-add-device-ip/data-box-connect-via-rest-4.png)

5. **Not Defteri'ni** yönetici olarak başlatın ve 'de `C:\Windows\System32\Drivers\etc`bulunan **ana bilgisayar** dosyasını açın.
6. **Ev sahipleri** dosyanıza aşağıdaki girişi ekleyin:`<device IP address> <Blob service endpoint>`
7. Başvuru için aşağıdaki resmi kullanın. Ana **bilgisayar** dosyasını kaydedin.

    ![Veri Kutusu panosu](media/data-box-add-device-ip/data-box-connect-via-rest-5.png)
