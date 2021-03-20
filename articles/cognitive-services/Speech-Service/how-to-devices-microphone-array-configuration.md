---
title: Bir mikrofon dizisi nasıl yapılandırılır-konuşma hizmeti
titleSuffix: Azure Cognitive Services
description: Konuşma cihazları SDK 'sının kullanabilmesi için bir mikrofon dizisinin nasıl yapılandırılacağını öğrenin.
services: cognitive-services
author: mswellsi
manager: yanbo
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 05/01/2020
ms.author: wellsi
ms.openlocfilehash: cf0580c96f5bf78f0444b2bb39088f2a417fd658
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2021
ms.locfileid: "95025070"
---
# <a name="how-to-configure-a-microphone-array"></a>Mikrofon Dizisini yapılandırma

Bu makalede, bir [mikrofon dizisinin](./speech-devices-sdk-microphone.md)nasıl yapılandırılacağını öğreneceksiniz. Bu, çalışma açısını ayarlamayı ve konuşma cihazları SDK 'Sı için hangi mikrofonun kullanıldığını seçmenizi içerir.

Konuşma cihazları SDK 'Sı, [kılavuzumuza](./speech-devices-sdk-microphone.md)göre tasarlanan bir mikrofon dizisiyle en iyi şekilde çalışmaktadır. Mikrofon dizisi yapılandırması, Işletim sistemi tarafından sağlanabilir veya aşağıdaki yöntemlerden biri aracılığıyla sağlanabilir.

Konuşma cihazları SDK 'Sı başlangıçta, sabit bir yapılandırma kümesinden seçim yaparak mikrofon dizilerini destekler.

```java
private static String DeviceGeometry = "Circular6+1"; // "Circular6+1", "Linear4",
private static String SelectedGeometry = "Circular6+1"; // "Circular6+1", "Circular3+1", "Linear4", "Linear2"
```

Windows 'da, mikrofon dizisi yapılandırması ses sürücüsü tarafından sağlanır.

V 1.11.0 'den konuşma cihazları SDK 'Sı Ayrıca bir [json dosyasından](https://aka.ms/sdsdk-micarray-json)yapılandırmayı destekler.


## <a name="windows"></a>Windows
Windows 'da, mikrofon dizisi geometrisi bilgileri, ses sürücüsünden otomatik olarak alınır. Bu nedenle, `DeviceGeometry` ve özellikleri  `SelectedGeometry` `MicArrayGeometryConfigFile` isteğe bağlıdır. İçin kullanılarak sunulan [json dosyasını](https://aka.ms/sdsdk-micarray-json) `MicArrayGeometryConfigFile` yalnızca beamor aralığını almak için kullanırız.

Kullanılarak bir mikrofon dizisi belirtilirse `AudioConfig::FromMicrophoneInput` , belirtilen mikrofonu kullanırız. Bir mikrofon belirtilmemişse veya `AudioConfig::FromDefaultMicrophoneInput` çağrılırsa, Windows üzerinde ses ayarları 'nda belirtilen varsayılan mikrofonu kullanırız.
Konuşma cihazları SDK 'sında Microsoft ses yığını yalnızca 16 KHz 'ın tam sayı olan örnek ücretler için aşağı örneklemeyi destekler.

## <a name="linux"></a>Linux
Linux 'ta, mikrofon geometrisi bilgisinin sağlanması gerekir. `DeviceGeometry`Ve kullanımı `SelectedGeometry` desteklenmeye devam eder. Ayrıca, özelliği kullanılarak JSON dosyası aracılığıyla da kullanılabilir `MicArrayGeometryConfigFile` . Windows 'a benzer şekilde, beamoluşturan aralığı JSON dosyası tarafından sağlanarak yapılabilir.

Kullanılarak bir mikrofon dizisi belirtilirse `AudioConfig::FromMicrophoneInput` , belirtilen mikrofonu kullanırız. Bir mikrofon belirtilmemişse veya `AudioConfig::FromDefaultMicrophoneInput` çağrılırsa, *varsayılan* adlı alsa cihazından kayıt yaptık. Varsayılan olarak, *varsayılan* olarak her zaman kart 0 cihaz 0 ' a işaret eder, ancak kullanıcılar bu `asound.conf` dosyayı dosyada değiştirebilir. 

Konuşma cihazları SDK 'sında Microsoft ses yığını yalnızca 16 KHz 'ın tam sayı olan örnek ücretler için altörnekleme destekler. Ayrıca, aşağıdaki biçimler desteklenir: 32-bit IEEE little endian float, 32-bit little endian işaretli int, 24 bit little endian imzalı int, 16 bit little endian imzalı tamsayı ve 8 bit işaretli tamsayı.

## <a name="android"></a>Android
Şu anda yalnızca [Roobo v1](./speech-devices-sdk-quickstart.md?pivots=platform-android%253fpivots%253dplatform-android) , konuşma cihazları SDK 'sı tarafından desteklenir. Bu davranış önceki sürümlerden farklıdır, çünkü Now `MicArrayGeometryConfigFile` özelliği, beamoluşturan aralığı IÇEREN json dosyasını belirtmek için kullanılabilir.

## <a name="microphone-array-configuration-json"></a>Mikrofon dizisi yapılandırma JSON

Microphone dizi geometrisi yapılandırması için JSON dosyası [JSON şemasını](https://aka.ms/sdsdk-micarray-json)izlemelidir. Şemayı izleyen bazı örnekler aşağıda verilmiştir.


```json
{
    "micArrayType": "Linear",
    "geometry": "Linear4"
}
```


Veya


```json
{
    "micArrayType": "Planar",
    "horizontalAngleBegin": 0,
    "horizontalAngleEnd": 360,
    "numberOfMicrophones": 4,
    "micCoord": [
        {
            "xCoord": 0,
            "yCoord": 0,
            "zCoord": 0
        },
        {
            "xCoord": 40,
            "yCoord": 0,
            "zCoord": 0
        },
        {
            "xCoord": -20,
            "yCoord": -35,
            "zCoord": 0
        },
        {
            "xCoord": -20,
            "yCoord": 35,
            "zCoord": 0
        }
    ]
}
```

Veya

```json
{
    "micArrayType": "Linear",
    "horizontalAngleBegin": 70,
    "horizontalAngleEnd": 110
}
```


## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Konuşma cihazınızı seçin](get-speech-devices-sdk.md)