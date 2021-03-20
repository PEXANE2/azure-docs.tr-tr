---
author: alkohli
ms.service: databox
ms.topic: include
ms.date: 12/07/2018
ms.author: alkohli
ms.openlocfilehash: d01cf90c42efbe611748027d0ea47ff8af3e5621
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2021
ms.locfileid: "94553135"
---
1. Data Box cihazda oturum açın. Kilidinin açık olduğundan emin olun.

    ![Ekran görüntüsü, pano 'nun kilitli olarak görüntülendiğini gösterir.](media/data-box-add-device-ip/data-box-connect-via-rest-1.png)

2. **Ağ arabirimlerini ayarla**' ya gidin. İstemciye bağlanmak için kullanılan ağ arabiriminin cihaz IP adresini bir yere unutmayın.

    ![Ekran görüntüsü, ı P adresini görebileceğiniz ağ ayarlarını gösterir.](media/data-box-add-device-ip/data-box-connect-via-rest-2.png)

3. **Bağlan ve Kopyala** ' ya gidip **rest**' e tıklayın.

    ![Ekran görüntüsü, erişim ayarı olarak REST ' i seçebileceğiniz Bağlan ve Kopyala bölmesini gösterir.](media/data-box-add-device-ip/data-box-connect-via-rest-3.png)

4. **Erişim depolama hesabı ve verileri karşıya yükle** iletişim kutusunda, **BLOB hizmeti uç noktası**' nı kopyalayın.

    ![Ekran görüntüsü, blob hizmeti uç noktasını kopyalayabileceğiniz depolama hesabı ve karşıya yükleme verileri iletişim kutusunu gösterir.](media/data-box-add-device-ip/data-box-connect-via-rest-4.png)

5. **Not defteri 'ni** yönetici olarak başlatın ve ardından konumunda bulunan **Hosts** dosyasını açın `C:\Windows\System32\Drivers\etc` .
6. **Hosts** dosyanıza aşağıdaki girişi ekleyin:`<device IP address> <Blob service endpoint>`
7. Başvuru için aşağıdaki görüntüyü kullanın. **Hosts** dosyasını kaydedin.

    ![Ekran görüntüsünde, ı P adresi ve BLOB hizmeti uç noktası eklenmiş bir Not Defteri belgesi gösterilir.](media/data-box-add-device-ip/data-box-connect-via-rest-5.png)
