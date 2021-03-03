---
title: Azure IoT Hub kullanarak ses yardımcısı uygulamasını yapılandırma
description: Azure IoT Hub kullanarak ses yardımcısı uygulamasını yapılandırma
author: elqu20
ms.author: v-elqu
ms.service: azure-percept
ms.topic: how-to
ms.date: 02/15/2021
ms.custom: template-how-to
ms.openlocfilehash: ec3e06b2d161785b5e6978cdf4cc6415fc0eb592
ms.sourcegitcommit: b4647f06c0953435af3cb24baaf6d15a5a761a9c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/02/2021
ms.locfileid: "101663885"
---
# <a name="configure-voice-assistant-application-using-azure-iot-hub"></a>Azure IoT Hub kullanarak ses yardımcısı uygulamasını yapılandırma

Bu makalede IoT Hub kullanarak ses Yardımcısı uygulamanızın nasıl yapılandırılacağı açıklanır. Demo şablonu kullanarak bir ses Yardımcısı oluşturma sürecinde size kılavuzluk eden adım adım bir öğretici için bkz. [Azure Percept Studio ve Azure Percept Audio ile kod No-Code Voice Yardımcısı](./tutorial-no-code-speech.md)oluşturma.

## <a name="update-your-voice-assistant-configuration"></a>Ses Yardımcısı yapılandırmanızı güncelleştirme

1. [Azure Portal](https://portal.azure.com/?feature.canmodifystamps=true&Microsoft_Azure_Iothub=aduprod&microsoft_azure_marketplace_ItemHideKey=Microsoft_Azure_ADUHidden#home) açın ve arama çubuğuna **IoT Hub** yazın. IoT Hub sayfasını açmak için simgeye tıklayın.

1. IoT Hub sayfasında, cihazınızın sağlanacağı IoT Hub seçin.

1. IoT Hub bağlı tüm cihazları görüntülemek için sol gezinti menüsünde **otomatik cihaz yönetimi** altında **IoT Edge** öğesini seçin.

1. Ses Yardımcısı uygulamanızın dağıtıldığı aygıtı seçin.

1. **Modülleri ayarla**' ya tıklayın.

    :::image type="content" source="./media/manage-voice-assistant-using-iot-hub/set-modules.png" alt-text="Görüntüyle.":::

1. **Container Registry kimlik bilgileri** bölümünde aşağıdaki girdinin bulunduğunu doğrulayın. Gerekirse kimlik bilgilerini ekleyin.

    |Name|Adres|Kullanıcı adı|Parola|
    |----|-------|--------|--------|
    |azureedgedevices|azureedgedevices.azurecr.io|devkitprivatedev çekme|

1. **IoT Edge modüller** bölümünde **azureearspeechclientmodule**' yi seçin.

    :::image type="content" source="./media/manage-voice-assistant-using-iot-hub/modules.png" alt-text="Görüntüyle.":::

1. **Modül ayarları** sekmesine tıklayın. Aşağıdaki yapılandırmayı doğrulayın:

    |Görüntü URI 'SI|Yeniden başlatma Ilkesi|İstenen durum|
    |---------|--------------|--------------|
    |azureedgedevices.azurecr.io/azureearspeechclientmodule:preload-devkit |Her|çalıştıran|

    Ayarlarınız eşleşmiyorsa, bunları düzenleyin ve **Güncelleştir**' e tıklayın.

1. **Ortam değişkenleri** sekmesine tıklayın. Tanımlı ortam değişkeni olmadığını doğrulayın.

1. **Kapsayıcı oluşturma seçenekleri** sekmesine tıklayın. **Hostconfig** ayarlarınızın aşağıda gösterilenler ile eşleştiğini doğrulayın. Eşleşmiyorsa, ayarlarınızı güncelleştirin.

    ```
    {
        "HostConfig": {
            "Privileged": true,
            "Binds": [
                "/dev:/dev"
            ]
        }
    }
    ```

1. **Module Ikizi ayarları** sekmesine tıklayın. **SpeechConfigs** bölümünü aşağıdaki şekilde güncelleştirin:

    ```
    "speechConfigs": {
        "appId": "<Application id for custom command project>",
        "key": "<Speech Resource key for custom command project>",
        "region": "<Region for the speech service>",
        "keywordModelUrl": "https://aedspeechscenarios.blob.core.windows.net/keyword-tables/computer.table",
        "keyword": "computer"
    }
    ```

    > [!NOTE]
    > Yukarıda kullanılan anahtar sözcük, varsayılan genel kullanıma açık bir anahtar sözcüktür. Kendinizinkini kullanmak isterseniz, oluşturulan bir tablo dosyasını blob depolamaya yükleyerek kendi özel anahtar sözcüğünü ekleyebilirsiniz. Blob depolamanın anonim kapsayıcı erişimi veya anonim blob erişimiyle yapılandırılması gerekir.

## <a name="how-to-find-out-appid-key-and-region"></a>Uygulama kimliği, anahtar ve bölge nasıl bulunur

**Uygulama kimliği**, **anahtar** ve **bölgenizi** bulmak için [konuşma Studio](https://speech.microsoft.com/)'ya gidin:

1. Oturum açın ve uygun konuşma kaynağını seçin.
1. **Konuşma Studio** giriş sayfasında, **sesli yardımcılar** altında **özel komutlar** ' a tıklayın.
1. Hedef projenizi seçin.

    :::image type="content" source="./media/manage-voice-assistant-using-iot-hub/project.png" alt-text="Görüntüyle.":::

1. Sol taraftaki menü panelinde **Ayarlar** ' a tıklayın.
1. **AppID** ve **anahtar** , **genel** ayarlar sekmesinde yer alır.

    :::image type="content" source="./media/manage-voice-assistant-using-iot-hub/general-settings.png" alt-text="Görüntüyle.":::

1. **Bölgenizi** bulmak için, ayarlar içindeki **lusıs kaynakları** sekmesini açın. **Yazma kaynağı** seçimi bölge bilgilerini içerir.

    :::image type="content" source="./media/manage-voice-assistant-using-iot-hub/luis-resources.png" alt-text="Görüntüyle.":::

1. **SpeechConfigs** bilgilerinizi girdikten sonra **Güncelleştir**' e tıklayın.

1. **Modülleri ayarla** sayfasının en üstündeki **rotalar** sekmesine tıklayın. Aşağıdaki değere sahip bir yönlendirmeye sahip olduğunuzdan emin olun:

    ```
    FROM /messages/modules/azureearspeechclientmodule/outputs/* INTO $upstream
    ```

    Mevcut değilse rotayı ekleyin.

1. **Gözden Geçir ve Oluştur**’a tıklayın.

1. **Oluştur**’a tıklayın.

## <a name="next-steps"></a>Sonraki adımlar

Ses Yardımcısı yapılandırmanızı güncelleştirdikten sonra uygulamayla etkileşim kurmak için Azure Percept Studio 'daki tanıtım 'e dönün.