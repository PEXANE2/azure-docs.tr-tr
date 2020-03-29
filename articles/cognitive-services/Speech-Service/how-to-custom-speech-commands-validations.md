---
title: 'Nasıl Yapılır: Özel Komut parametrelerine doğrulama ekleme (Önizleme)'
titleSuffix: Azure Cognitive Services
description: Bu makalede, Özel Komutlar'da bir parametreye doğrulamanın nasıl ekleyeceğini açıklıyoruz.
services: cognitive-services
author: don-d-kim
manager: yetian
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 10/09/2019
ms.author: donkim
ms.openlocfilehash: cf6e4e4f0bfab43fb738f8415022e55fcbcbd05a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "76156463"
---
# <a name="how-to-add-validations-to-custom-command-parameters-preview"></a>Nasıl Yapılır: Özel Komut parametrelerine doğrulama ekleme (Önizleme)

Bu makalede, parametrelere doğrulama eklemeyi ve düzeltme istemini öğreneceksiniz.

## <a name="prerequisites"></a>Ön koşullar

Aşağıdaki makalelerdeki adımları tamamlamış olmalısınız:

- [Quickstart: Özel Komut Oluşturma (Önizleme)](./quickstart-custom-speech-commands-create-new.md)
- [Quickstart: Parametreler (Önizleme) ile Özel Komut oluşturma](./quickstart-custom-speech-commands-create-parameters.md)

## <a name="create-a-settemperature-command"></a>SetTemperature Komutu Oluşturma

Doğrulamaları göstermek için, kullanıcının sıcaklığı ayarlamasına izin veren yeni bir Komut oluşturalım.

1. [Konuşma Stüdyosu'nda](https://speech.microsoft.com/) daha önce oluşturduğunuz Özel Komutlar uygulamasını açın
1. Yeni bir Komut **SetTemperature** oluşturma
1. Hedef sıcaklık için bir parametre ekleme
1. Sıcaklık parametresi için doğrulama ekleme
   > [!div class="mx-imgBorder"]
   > ![Aralık doğrulama ekleme](media/custom-speech-commands/validations-add-temperature.png)

   | Ayar           | Önerilen değer                                          | Açıklama                                                                                      |
   | ----------------- | -------------------------------------------------------- | ------------------------------------------------------------------------------------------------ |
   | Adı              | Sıcaklık                                              | Komut parametreniz için açıklayıcı bir ad                                                    |
   | Gerekli          | true                                                     | Komutu tamamlamadan önce bu parametre için bir değer gerekip gerekmediğini belirten onay kutusu |
   | Yanıt şablonu | "- Hangi sıcaklıkta istersin?"                     | Bilinmediğinde bu parametrenin değerini sormak için bir istem                              |
   | Tür              | Sayı                                                   | Sayı, Dize veya Tarih Saati gibi parametre türü                                      |
   | Doğrulama        | Min Değeri: 60, Maksimum Değer: 80                             | Sayı parametreleri için, parametre için izin verilen değer aralığı                             |
   | Yanıt şablonu | "- Üzgünüm, sadece 60 ile 80 derece arasında ayarlayabilirim"      | Doğrulama başarısız olursa güncelleştirilmiş bir değer istemek için komut istemi                                       |

1. Bazı örnek cümleler ekleme

   ```
   set the temperature to {Temperature} degrees
   change the temperature to {Temperature}
   set the temperature
   change the temperature
   ```

1. Sonucu onaylamak için tamamlama kuralı ekleme

   | Ayar    | Önerilen değer                                           | Açıklama                                        |
   | ---------- | --------------------------------------------------------- | -------------------------------------------------- |
   | Kural Adı  | Onay Mesajı                                      | Kuralın amacını açıklayan bir ad          |
   | Koşullar | Gerekli Parametre - Sıcaklık                          | Kuralın ne zaman çalıştırılacakolabileceğini belirleyen koşullar    |
   | Eylemler    | SpeechResponse - "- Tamam, {Sıcaklık} dereceye ayar" | Kural koşulu doğru olduğunda yapılacak eylem |

> [!TIP]
> Bu örnek, sonucu onaylamak için bir konuşma yanıtı kullanır. Komutu bir istemci eylemiyle tamamlama ile ilgili örnekler için bkz: [Nasıl Yapılır: Konuşma SDK ile istemci deki Komutları Nasıl Yerine Getirin (Önizleme)](./how-to-custom-speech-commands-fulfill-sdk.md)

## <a name="try-it-out"></a>Deneyin

Test panelini seçin ve birkaç etkileşim deneyin.

- Giriş: Sıcaklığı 72 dereceye ayarlayın
- Çıktı: "Tamam, 72 dereceye ayar"

- Giriş: Sıcaklığı 45 dereceye ayarlayın
- Çıktı: "Üzgünüm, ben sadece 60 ve 80 derece arasında ayarlayabilirsiniz"
- Giriş: bunun yerine 72 derece yapmak
- Çıktı: "Tamam, 72 dereceye ayar"

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Nasıl Yapılır: Özel Komuta onay ekleme (Önizleme)](./how-to-custom-speech-commands-confirmations.md)
