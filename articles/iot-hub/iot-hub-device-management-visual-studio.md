---
title: Azure IoT cihaz yönetimi w/ Visual Studio Cloud Explorer
description: Doğrudan yöntemler ve Twin'in istenen özellik yönetimi seçeneklerini içeren Azure IoT Hub aygıt yönetimi için Visual Studio için Bulut Gezgini'ni kullanın.
author: shizn
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 08/20/2019
ms.author: xshi
ms.openlocfilehash: 6fe5a45dda6632c56b3c6714827950e25e7d26af
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "73953193"
---
# <a name="use-cloud-explorer-for-visual-studio-for-azure-iot-hub-device-management"></a>Azure IoT Hub aygıt yönetimi için Visual Studio için Bulut Gezgini'ni kullanma

![Uçuça diyagram](media/iot-hub-device-management-visual-studio/iot-e2e-simple.png)

[Cloud Explorer,](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.CloudExplorerForVS) Azure kaynaklarınızı görüntülemenize, özelliklerini incelemenize ve Visual Studio içinden önemli geliştirici eylemleri gerçekleştirmenize olanak tanıyan kullanışlı bir Visual Studio uzantısıdır. Çeşitli görevleri gerçekleştirmek için kullanabileceğiniz yönetim seçenekleri ile birlikte gelir.

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-whole.md)]

| Yönetim seçeneği          | Görev                    |
|----------------------------|--------------------------------|
| Doğrudan yöntemler             | İleti göndermeyi başlatma veya durdurma veya aygıtı yeniden başlatma gibi bir aygıt eylemi yapın.                                        |
| Aygıt ikizini okuma           | Bir aygıtın bildirilen durumunu alın. Örneğin, aygıt LED'in yanıp söndüğünü bildiriyor.                                    |
| Aygıt ikizini güncelleştir         | Bir aygıtı, LED'i yeşile ayarlama veya telemetri gönderme aralığını 30 dakikaya ayarlama gibi belirli durumlara koyun.         |
| Buluttan cihaza iletiler   | Bildirimleri bir aygıta gönderin. Örneğin, "Bugün çok yağmur olasılığı yüksektir. Şemsiye getirmeyi unutma."              |

Bu seçenekleri kullanma konusundaki farklar ve kılavuzlar hakkında daha ayrıntılı açıklama için [Aygıttan buluta iletişim kılavuzuna](iot-hub-devguide-d2c-guidance.md) ve [Buluttan cihaza iletişim kılavuzuna](iot-hub-devguide-c2d-guidance.md)bakın.

Cihaz ikizleri; meta veriler, yapılandırmalar ve koşullar gibi cihaz durumu bilgilerini depolayan JSON belgelerdir. IoT Hub, bağlanan her aygıt için bir aygıt ikizini kalıcı hale getirir. Cihaz ikizleri hakkında daha fazla bilgi için [bkz.](iot-hub-node-node-twin-getstarted.md)

## <a name="what-you-learn"></a>Öğrenecekleriniz

Bu makalede, geliştirme bilgisayarınızdaçeşitli yönetim seçenekleriyle Visual Studio için Bulut Gezgini'ni nasıl kullanacağınızı öğreneceksiniz.

## <a name="what-you-do"></a>Ne yaparsınız

Bu makalede, çeşitli yönetim seçenekleriyle Visual Studio için Cloud Explorer'ı çalıştırın.

## <a name="what-you-need"></a>Ne gerekiyor

Aşağıdaki ön koşullara ihtiyacınız vardır:

- Etkin bir Azure aboneliği.

- Aboneliğiniz altında bir Azure IoT Hub'ı.

- Microsoft Visual Studio 2017 Güncelleştirme 9 veya daha sonra. Bu makale, [Visual Studio 2017 veya Visual Studio 2019'u kullansın.](https://www.visualstudio.com/vs/)

- Varsayılan olarak Azure İş Yükü ile seçilen Visual Studio Installer'ın Bulut Gezgini bileşeni.

## <a name="update-cloud-explorer-to-latest-version"></a>Bulut Gezgini'ni en son sürüme güncelleme

Visual Studio Installer for Visual Studio 2017'nin Cloud Explorer bileşeni yalnızca aygıttan buluta ve buluttan cihaza iletileri izlemeyi destekler. Visual Studio 2017'yi kullanmak için en son [Cloud Explorer'ı](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.CloudExplorerForVS)indirip kurun.

## <a name="sign-in-to-access-your-hub"></a>Hub'ınıza erişmek için oturum açın

1. Visual Studio'da Bulut Gezgini'ni açmak için**Bulut Gezgini'ni** **Görüntüle'yi** > seçin.

1. Aboneliklerinizi göstermek için Hesap Yönetimi simgesini seçin.

    ![Hesap Yönetimi simgesi](media/iot-hub-visual-studio-cloud-device-messaging/account-management-icon.png)

1. Azure'da oturum unursanız, hesaplarınız görünür. Azure'da ilk kez oturum açabilmek için **hesap ekle'yi**seçin.

1. Kullanmak istediğiniz Azure aboneliklerini seçin ve **Uygula'yı**seçin.

1. Aboneliğinizi genişletin, ardından **IoT Hub'larını**genişletin.  Her hub'ın altında, bu hub için aygıtlarınızı görebilirsiniz. Yönetim seçeneklerine erişmek için tek bir aygıta sağ tıklayın.

    ![Yönetim seçenekleri](media/iot-hub-device-management-visual-studio/management-options-vs2019.png)

## <a name="direct-methods"></a>Doğrudan yöntemler

Doğrudan yöntemleri kullanmak için aşağıdaki adımları yapın:

1. Cihazınıza sağ tıklayın ve **Aygıtı Doğrudan Çağır Yöntemi'ni**seçin.

1. **Doğrudan Çağır Yöntemi'ne**yöntem adını ve yükünü girin ve ardından **Tamam'ı**seçin.

    Sonuçlar **Çıktı'da**görünür.

## <a name="update-device-twin"></a>Aygıt ikizini güncelleştir

Aygıt ikizini sağlamak için aşağıdaki adımları yapın:

1. Cihazınıza sağ tıklayın ve **Aygıtı İkiz'i Edit'i**seçin.

   **Bir azure-iot-device-twin.json** dosyası aygıt ikiziçeriği ile açılır.

1. **Azure-iot-device-twin.json** dosyasında **etiketlerin** veya özelliklerin bazı durumlarını **yapın.istenilen** alanlar.

1. Aygıtı ikizi güncellemek için **Ctrl+S** tuşuna basın.

   Sonuçlar **Çıktı'da**görünür.

## <a name="send-cloud-to-device-messages"></a>Buluttan cihaza iletileri gönderme

IoT Hub'ınızdan cihazınıza bir ileti göndermek için aşağıdaki adımları izleyin:

1. Cihazınıza sağ tıklayın ve **C2D İleti gönder'i**seçin.

1. **C2D ileti gönder iletisini** girin ve **Tamam'ı**seçin.

   Sonuçlar **Çıktı'da**görünür.

## <a name="next-steps"></a>Sonraki adımlar

Cloud Explorer for Visual Studio için çeşitli yönetim seçenekleriyle nasıl kullanılacağını öğrendiniz.

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]
