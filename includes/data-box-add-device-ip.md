---
author: alkohli
ms.service: databox
ms.topic: include
ms.date: 12/07/2018
ms.author: alkohli
ms.openlocfilehash: 6b716640bc8117ccc2d473b337ee3e555e8d9b0a
ms.sourcegitcommit: 6a902230296a78da21fbc68c365698709c579093
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/05/2020
ms.locfileid: "93376194"
---
1. Data Box cihazda oturum açın. Kilidinin açık olduğundan emin olun.

    ![Data Box panosu](media/data-box-add-device-ip/data-box-connect-via-rest-1.png)

2. **Ağ arabirimlerini ayarla** ' ya gidin. İstemciye bağlanmak için kullanılan ağ arabiriminin cihaz IP adresini bir yere unutmayın.

    ![Data Box panosu 2](media/data-box-add-device-ip/data-box-connect-via-rest-2.png)

3. **Bağlan ve Kopyala** ' ya gidip **rest** ' e tıklayın.

    ![Data Box panosu 3](media/data-box-add-device-ip/data-box-connect-via-rest-3.png)

4. **Erişim depolama hesabı ve verileri karşıya yükle** iletişim kutusunda, **BLOB hizmeti uç noktası** ' nı kopyalayın.

    ![Data Box panosu 4](media/data-box-add-device-ip/data-box-connect-via-rest-4.png)

5. **Not defteri 'ni** yönetici olarak başlatın ve ardından konumunda bulunan **Hosts** dosyasını açın `C:\Windows\System32\Drivers\etc` .
6. **Hosts** dosyanıza aşağıdaki girişi ekleyin:`<device IP address> <Blob service endpoint>`
7. Başvuru için aşağıdaki görüntüyü kullanın. **Hosts** dosyasını kaydedin.

    ![Data Box panosu 5](media/data-box-add-device-ip/data-box-connect-via-rest-5.png)
