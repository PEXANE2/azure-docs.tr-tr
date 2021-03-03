---
title: Azure Percept ses ve konuşma modülleriyle ilgili sorunları giderme
description: Taslak deneyimi sırasında bulunan daha yaygın sorunlardan bazıları için sorun giderme ipuçları alın
author: mimcco
ms.author: mimcco
ms.service: azure-percept
ms.topic: how-to
ms.date: 02/18/2021
ms.custom: template-how-to
ms.openlocfilehash: 1e2ad920afb55066f07430568f976154f6d7cae1
ms.sourcegitcommit: b4647f06c0953435af3cb24baaf6d15a5a761a9c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/02/2021
ms.locfileid: "101680129"
---
# <a name="azure-percept-audio-and-speech-module-troubleshooting"></a>Azure Percept ses ve konuşma modülü sorunlarını giderme

Ses Yardımcısı uygulama sorunlarını gidermek için aşağıdaki yönergeleri kullanın.

## <a name="collecting-speech-module-logs"></a>Konuşma modülü günlüklerini toplama

Bu komutları çalıştırmak için, [Azure PERCEPT DK Wi-Fi erişim noktasına bağlanın ve SSH üzerinden Dev Kit 'e bağlanın](./how-to-ssh-into-percept-dk.md) ve SSH terminaline komutları girin.

```console
 iotedge logs azureearspeechclientmodule
```

Daha fazla analiz için herhangi bir çıktıyı bir. txt dosyasına yönlendirmek için aşağıdaki sözdizimini kullanın:

```console
[command] > [file name].txt
```

Çıktıyı bir. txt dosyasına yönlendirdikten sonra SCP aracılığıyla dosyayı ana bilgisayara kopyalayın:

```console
scp [remote username]@[IP address]:[remote file path]/[file name].txt [local host file path]
```

[yerel ana bilgisayar dosyası yolu], ana bilgisayarınızdaki. txt dosyasını kopyalamak istediğiniz konumu ifade eder. [Uzak Kullanıcı adı], [kurulum deneyimi](./quickstart-percept-dk-set-up.md)SıRASıNDA seçilen SSH kullanıcı adıdır. OOBE sırasında bir SSH oturum açma ayarı oluşturmadıysanız, uzak kullanıcı adınız köküdür.

## <a name="checking-runtime-status-of-the-speech-module"></a>Konuşma modülünün çalışma zamanı durumu denetleniyor

**Azureearspeechclientmodule** çalışma zamanı durumunun **çalışır** durumda olup olmadığını denetleyin. Cihaz modüllerinizin çalışma zamanı durumunu bulmak için [Azure Portal](https://portal.azure.com/?feature.canmodifystamps=true&Microsoft_Azure_Iothub=aduprod&microsoft_azure_marketplace_ItemHideKey=Microsoft_Azure_ADUHidden#home) açın ve **tüm kaynaklar**  ->  **\<your IoT hub>**  ->  **IoT Edge** gidin  ->  **\<your device ID>** . Tüm yüklü modüllerin çalışma zamanı durumunu görmek için **modüller** sekmesine tıklayın.

:::image type="content" source="./media/troubleshoot-audio-accessory-speech-module/over-the-air-iot-edge-device-page.png" alt-text="Azure portal uç cihaz sayfası.":::

**Azureearspeechclientmodule** çalışma zamanı durumu **çalışıyor** olarak listelenmiyorsa, **modülleri ayarla**  ->  **azureearspeechclientmodule**' ye tıklayın. **Modül ayarları** sayfasında, **istenen durumu** **çalışıyor** olarak ayarlayın ve **Güncelleştir**' e tıklayın.

:::image type="content" source="./media/troubleshoot-audio-accessory-speech-module/firmware-desired-status-stopped.png" alt-text="Azure portal modülleri ayarlama ekranı.":::

## <a name="understanding-ear-som-led-indicators"></a>Ear SoM göstergelerini anlama

Cihazın hangi durumda olduğunu anlamak için LED göstergelerini kullanabilirsiniz. Genellikle modülün *Güç açma* sonrasında tamamen başlatılması 2 dakika sürer. Başlatma adımlarını izleyerek, şunları görürsünüz:

1. 1 sol yeşil ışık-cihaz açık. 
2. 1 sol yeşil ışık ve Merkez, yanıp sönmüş yeşil-kimlik doğrulama devam ediyor. 
3. Cihaz kimlik doğrulamasından ve kullanıma hazırlandıktan sonra, üç LED 'in tümü mavi olarak değişir.

|LED durumu                  |Ear SoM durumu            |
|----------------------------|---------------------------|
|1x yeşil (sol tarafta)         |açma |
|1x yeşil (sol tarafta) <br> 1x yanıp sönen yeşil (orta LED) |kimlik doğrulama devam ediyor |
|3x kapalı                      |başlatma tamamlandı |
|3x mavisi                     |kullanıma hazırlanıyor |
|3x yanıp sönen mavi            |tanınan anahtar sözcük |
|3x yarış mavisi              |işlerken |
|3x Red                      |mikrofon |

## <a name="next-steps"></a>Sonraki adımlar

Azure Percept DK sorunlarını giderme hakkında daha fazla bilgi için bkz. [genel sorun giderme kılavuzu](./troubleshoot-dev-kit.md) .