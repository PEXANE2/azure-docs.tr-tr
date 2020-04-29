---
author: alkohli
ms.service: databox
ms.topic: include
ms.date: 12/07/2018
ms.author: alkohli
ms.openlocfilehash: e4b366075cb16f62a0e16b5b06da6fb19ffefdb9
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "67189036"
---
1. Data Box cihazda oturum açın. Kilidinin açık olduğundan emin olun.

    ![Data Box panosu](media/data-box-add-device-ip/data-box-connect-via-rest-1.png)

2. **Ağ arabirimlerini ayarla**' ya gidin. İstemciye bağlanmak için kullanılan ağ arabiriminin cihaz IP adresini bir yere unutmayın.

    ![Data Box panosu](media/data-box-add-device-ip/data-box-connect-via-rest-2.png)

3. **Bağlan ve Kopyala** ' ya gidip **rest**' e tıklayın.

    ![Data Box panosu](media/data-box-add-device-ip/data-box-connect-via-rest-3.png)

4. **Erişim depolama hesabı ve verileri karşıya yükle** iletişim kutusunda, **BLOB hizmeti uç noktası**' nı kopyalayın.

    ![Data Box panosu](media/data-box-add-device-ip/data-box-connect-via-rest-4.png)

5. **Not defteri 'ni** yönetici olarak başlatın ve ardından konumunda `C:\Windows\System32\Drivers\etc`bulunan **Hosts** dosyasını açın.
6. **Hosts** dosyanıza aşağıdaki girişi ekleyin:`<device IP address> <Blob service endpoint>`
7. Başvuru için aşağıdaki görüntüyü kullanın. **Hosts** dosyasını kaydedin.

    ![Data Box panosu](media/data-box-add-device-ip/data-box-connect-via-rest-5.png)
