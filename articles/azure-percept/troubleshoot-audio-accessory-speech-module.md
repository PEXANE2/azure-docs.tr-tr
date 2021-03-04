---
title: Azure Percept ses ve konuşma modülleriyle ilgili sorunları giderme
description: Taslak deneyimi sırasında bulunan daha yaygın sorunlardan bazıları için sorun giderme ipuçları alın
author: mimcco
ms.author: mimcco
ms.service: azure-percept
ms.topic: how-to
ms.date: 02/18/2021
ms.custom: template-how-to
ms.openlocfilehash: a3877ea680e7b4c705f127c54e0fa10c45d3b51d
ms.sourcegitcommit: 4b7a53cca4197db8166874831b9f93f716e38e30
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/04/2021
ms.locfileid: "102097984"
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

[yerel ana bilgisayar dosyası yolu], ana bilgisayarınızdaki. txt dosyasını kopyalamak istediğiniz konumu ifade eder. [Uzak Kullanıcı adı], [görsel taslak deneyimi](./quickstart-percept-dk-set-up.md)SıRASıNDA seçilen SSH kullanıcı adıdır. Azure Percept DK oluşturma deneyiminde bir SSH oturumu oluşturmadıysanız, uzak kullanıcı adınız köküdür.

## <a name="checking-runtime-status-of-the-speech-module"></a>Konuşma modülünün çalışma zamanı durumu denetleniyor

**Azureearspeechclientmodule** çalışma zamanı durumunun **çalışır** durumda olup olmadığını denetleyin. Cihaz modüllerinizin çalışma zamanı durumunu bulmak için [Azure Portal](https://portal.azure.com/) açın ve **tüm kaynaklar**  ->  **\<your IoT hub>**  ->  **IoT Edge** gidin  ->  **\<your device ID>** . Tüm yüklü modüllerin çalışma zamanı durumunu görmek için **modüller** sekmesine tıklayın.

:::image type="content" source="./media/troubleshoot-audio-accessory-speech-module/over-the-air-iot-edge-device-page.png" alt-text="Azure portal uç cihaz sayfası.":::

**Azureearspeechclientmodule** çalışma zamanı durumu **çalışıyor** olarak listelenmiyorsa, **modülleri ayarla**  ->  **azureearspeechclientmodule**' ye tıklayın. **Modül ayarları** sayfasında, **istenen durumu** **çalışıyor** olarak ayarlayın ve **Güncelleştir**' e tıklayın.

## <a name="understanding-ear-som-led-indicators"></a>Ear SoM göstergelerini anlama

Cihazın hangi durumda olduğunu anlamak için LED göstergelerini kullanabilirsiniz. Genellikle modülün *Güç açma* sonrasında tamamen başlatılması 2 dakika sürer. Başlatma adımlarını izleyerek, şunları görürsünüz:

1. 1 merkezden beyaz ışığı-cihaz açık. 
2. 1 Merkez beyaz yanıp sönen-kimlik doğrulama devam ediyor. 
3. Cihaz kimlik doğrulamasından ve kullanıma hazırlandıktan sonra, üç LED 'in tümü mavi olarak değişir.

|GELIŞTIRMESINE|   LED durumu|  Ear SoM durumu|
|---|------------|----------------| 
|L02|   1x beyaz, statik açık |Açma |
|L02|   1x beyaz, 0,5 Hz yanıp sönen|  Kimlik doğrulama devam ediyor |
|L01 & L02 & L03|   3x mavisi, statik|     Anahtar sözcük bekleniyor|
|L01 & L02 & L03|   LED dizisi yanıp sönme, 20 fps | Dinleme veya konuşuyor|
|L01 & L02 & L03|   LED dizisi yarış, 20 fps|    Düğün|
|L01 & L02 & L03|   3x kırmızı, statik | Mikrofon|

## <a name="next-steps"></a>Sonraki adımlar

Azure Percept DK sorunlarını giderme hakkında daha fazla bilgi için bkz. [genel sorun giderme kılavuzu](./troubleshoot-dev-kit.md) .
