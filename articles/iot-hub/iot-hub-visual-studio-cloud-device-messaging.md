---
title: Azure IoT Hub aygıt iletisini yönetmek için VS Cloud Explorer'ı kullanma
description: Azure IoT Hub'ında cihazları buluta buluta görüntülemek ve aygıt iletilerine bulut göndermek için Visual Studio için Cloud Explorer'ı nasıl kullanacağınızı öğrenin.
author: shizn
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 08/20/2019
ms.author: xshi
ms.openlocfilehash: c56bb7030b2ebc12e3afc24e2d8cb29ce2dda0bf
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74079487"
---
# <a name="use-cloud-explorer-for-visual-studio-to-send-and-receive-messages-between-your-device-and-iot-hub"></a>Cihazınız ve IoT Hub arasında ileti göndermek ve almak için Visual Studio için Bulut Gezgini'ni kullanın

![Uçuça diyagram](./media/iot-hub-visual-studio-cloud-device-messaging/e-to-e-diagram.png)

[Cloud Explorer,](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.CloudExplorerForVS) Azure kaynaklarınızı görüntülemenize, özelliklerini incelemenize ve Visual Studio içinden önemli geliştirici eylemleri gerçekleştirmenize olanak tanıyan kullanışlı bir Visual Studio uzantısıdır. Bu makalede, aygıtınız ve hub'ınız arasında ileti göndermek ve almak için Bulut Gezgini'nin nasıl kullanılacağı üzerinde duruluyor.

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-partial.md)]

## <a name="what-you-learn"></a>Öğrenecekleriniz

Bu makalede, aygıttan buluta iletileri izlemek ve buluttan cihaza iletigöndermek için Visual Studio için Cloud Explorer'ı nasıl kullanacağınızı öğreneceksiniz. Aygıttan buluta iletiler, cihazınızın topladığı ve ioT Hub'ınıza gönderdiği sensör verileri olabilir. Buluttan cihaza iletiler, IoT Hub'ınızın cihazınıza gönderdiği komutlar olabilir. Örneğin, cihazınıza bağlı bir LED'i kırpın.

## <a name="what-you-do"></a>Ne yaparsınız

Bu makalede, aşağıdaki görevleri yapmak:

- Aygıttan buluta iletileri izlemek için Visual Studio için Cloud Explorer'ı kullanın.

- Buluttan aygıta ileti göndermek için Visual Studio için Cloud Explorer'ı kullanın.

## <a name="what-you-need"></a>Ne gerekiyor

Aşağıdaki ön koşullara ihtiyacınız vardır:

- Etkin bir Azure aboneliği.

- Aboneliğiniz altında bir Azure IoT Hub'ı.

- Microsoft Visual Studio 2017 Güncelleştirme 9 veya daha sonra. Bu makalede [Visual Studio 2019](https://www.visualstudio.com/vs/)kullanabilirsiniz.

- Azure İş Yükü ile varsayılan olarak seçilen Visual Studio Installer'ın Bulut Gezgini bileşeni.

## <a name="update-cloud-explorer-to-latest-version"></a>Bulut Gezgini'ni en son sürüme güncelleme

Visual Studio Installer for Visual Studio 2017'nin Cloud Explorer bileşeni yalnızca aygıttan buluta ve buluttan cihaza iletileri izlemeyi destekler. Visual Studio 2017'yi kullanmak için en son [Cloud Explorer'ı](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.CloudExplorerForVS)indirip kurun.

## <a name="sign-in-to-access-your-hub"></a>Hub'ınıza erişmek için oturum açın

Hub'ınıza erişmek için aşağıdaki adımları izleyin:

1. Visual Studio'da Bulut Gezgini'ni açmak için**Bulut Gezgini'ni** **Görüntüle'yi** > seçin.

1. Aboneliklerinizi göstermek için Hesap Yönetimi simgesini seçin.

    ![Hesap Yönetimi simgesi](media/iot-hub-visual-studio-cloud-device-messaging/account-management-icon.png)

1. Azure'da oturum unursanız, hesaplarınız görünür. Azure'da ilk kez oturum açabilmek için **hesap ekle'yi**seçin.

1. Kullanmak istediğiniz Azure aboneliklerini seçin ve **Uygula'yı**seçin.

1. Aboneliğinizi genişletin, ardından **IoT Hub'larını**genişletin.  Her hub'ın altında, bu hub için aygıtlarınızı görebilirsiniz.

    ![Cihaz Listesi](media/iot-hub-visual-studio-cloud-device-messaging/hub-device-list.png)

## <a name="monitor-device-to-cloud-messages"></a>Aygıttan buluta iletileri izleme

Cihazınızdan IoT Hub'ınıza gönderilen iletileri izlemek için aşağıdaki adımları izleyin:

1. IoT Hub'ınıza veya cihazınıza sağ tıklayın ve **D2C İletisi'ni İzlemeye Başlat'ı**seçin.

    ![D2C İletisi İzlemeye Başla](media/iot-hub-visual-studio-cloud-device-messaging/start-monitoring-d2c-message-vs2019.png)

1. İzlenen iletiler **Çıktı**altında görünür.

    ![D2C İleti Sonucunu İzleme](media/iot-hub-visual-studio-cloud-device-messaging/monitor-d2c-message-result-vs2019.png)

1. İzlemeyi durdurmak için herhangi bir IoT Hub veya cihazına sağ tıklayın ve **İzlemeyi Durdur D2C İletisini**seçin.

## <a name="send-cloud-to-device-messages"></a>Buluttan cihaza iletileri gönderme

IoT Hub'ınızdan cihazınıza bir ileti göndermek için aşağıdaki adımları izleyin:

1. Cihazınıza sağ tıklayın ve **C2D İleti gönder'i**seçin.

1. İletiyi giriş kutusuna girin.

    ![C2D Iletisi Gönder](media/iot-hub-visual-studio-cloud-device-messaging/send-c2d-message-test.png)

    Sonuçlar **Çıktı**altında görünür.

    ![C2D Ileti Sonucu Gönder](media/iot-hub-visual-studio-cloud-device-messaging/send-c2d-message-result-vs2019.png)

## <a name="next-steps"></a>Sonraki adımlar

IoT aygıtınızla Azure IoT Hub'ınız arasında aygıttan buluta iletileri izlemeyi ve buluttan cihaza iletigöndermeyi öğrendiniz.

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]