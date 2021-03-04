---
title: Azure IoT Hub kullanarak ses yardımcısı uygulamasını yapılandırma
description: Azure IoT Hub kullanarak ses yardımcısı uygulamasını yapılandırma
author: elqu20
ms.author: v-elqu
ms.service: azure-percept
ms.topic: how-to
ms.date: 02/15/2021
ms.custom: template-how-to
ms.openlocfilehash: b22ef4ee0a8b5978bb2ec1c02fadf368815f3014
ms.sourcegitcommit: 4b7a53cca4197db8166874831b9f93f716e38e30
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/04/2021
ms.locfileid: "102095791"
---
# <a name="configure-voice-assistant-application-using-azure-iot-hub"></a>Azure IoT Hub kullanarak ses yardımcısı uygulamasını yapılandırma

Bu makalede IoT Hub kullanarak ses Yardımcısı uygulamanızın nasıl yapılandırılacağı açıklanır. Demo şablonu kullanarak bir ses Yardımcısı oluşturma sürecinde size kılavuzluk eden adım adım bir öğretici için bkz. [Azure Percept Studio ve Azure Percept Audio ile kod No-Code Voice Yardımcısı](./tutorial-no-code-speech.md)oluşturma.

## <a name="update-your-voice-assistant-configuration"></a>Ses Yardımcısı yapılandırmanızı güncelleştirme

1. [Azure Portal](https://portal.azure.com) açın ve arama çubuğuna **IoT Hub** yazın. IoT Hub sayfasını açmak için simgeye tıklayın.

1. IoT Hub sayfasında, cihazınızın sağlanacağı IoT Hub seçin.

1. IoT Hub bağlı tüm cihazları görüntülemek için sol gezinti menüsünde **otomatik cihaz yönetimi** altında **IoT Edge** öğesini seçin.

1. Ses Yardımcısı uygulamanızın dağıtıldığı aygıtı seçin.

1. **Modülleri ayarla**' ya tıklayın.

    :::image type="content" source="./media/manage-voice-assistant-using-iot-hub/set-modules.png" alt-text="Set modülleri vurgulanmış şekilde cihaz sayfasının ekran görüntüsü.":::

1. **Container Registry kimlik bilgileri** bölümünde aşağıdaki girdinin bulunduğunu doğrulayın. Gerekirse kimlik bilgilerini ekleyin.

    |Name|Adres|Kullanıcı adı|Parola|
    |----|-------|--------|--------|
    |azureedgedevices|azureedgedevices.azurecr.io|devkitprivatedev çekme|

1. **IoT Edge modüller** bölümünde **azureearspeechclientmodule**' yi seçin.

    :::image type="content" source="./media/manage-voice-assistant-using-iot-hub/modules.png" alt-text="Cihazdaki tüm IoT Edge modüllerinin listesini gösteren ekran görüntüsü.":::

1. **Modül ayarları** sekmesine tıklayın. Aşağıdaki yapılandırmayı doğrulayın:

    Görüntü URI 'SI|Yeniden başlatma Ilkesi|İstenen durum
    ---------|--------------|--------------
    mcr.microsoft.com/azureedgedevices/azureearspeechclientmodule:preload-devkit|Her|çalıştıran

    Ayarlarınız eşleşmiyorsa, bunları düzenleyin ve **Güncelleştir**' e tıklayın.

1. **Ortam değişkenleri** sekmesine tıklayın. Tanımlı ortam değişkeni olmadığını doğrulayın.

1. **Module Ikizi ayarları** sekmesine tıklayın. **SpeechConfigs** bölümünü aşağıdaki şekilde güncelleştirin:

    ```
    "speechConfigs": {
        "appId": "<Application id for custom command project>",
        "key": "<Speech Resource key for custom command project>",
        "region": "<Region for the speech service>",
        "keywordModelUrl": "https://aedsamples.blob.core.windows.net/speech/keyword-tables/computer.table",
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

    :::image type="content" source="./media/manage-voice-assistant-using-iot-hub/project.png" alt-text="Konuşma Studio 'daki proje sayfasının ekran görüntüsü.":::

1. Sol taraftaki menü panelinde **Ayarlar** ' a tıklayın.
1. **AppID** ve **anahtar** , **genel** ayarlar sekmesinde yer alır.

    :::image type="content" source="./media/manage-voice-assistant-using-iot-hub/general-settings.png" alt-text="Konuşma projesi genel ayarlarının ekran görüntüsü.":::

1. **Bölgenizi** bulmak için, ayarlar içindeki **lusıs kaynakları** sekmesini açın. **Yazma kaynağı** seçimi bölge bilgilerini içerir.

    :::image type="content" source="./media/manage-voice-assistant-using-iot-hub/luis-resources.png" alt-text="Konuşma projesi LUSıS kaynaklarının ekran görüntüsü.":::

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

