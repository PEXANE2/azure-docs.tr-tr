---
title: 'Nasıl yapılır: özel komut parametrelerine doğrulama ekleme (Önizleme)'
titleSuffix: Azure Cognitive Services
description: Bu makalede, özel komutlarda bir parametreye nasıl doğrulama ekleneceğini açıkladık.
services: cognitive-services
author: don-d-kim
manager: yetian
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 10/09/2019
ms.author: donkim
ms.openlocfilehash: cf6e4e4f0bfab43fb738f8415022e55fcbcbd05a
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "76156463"
---
# <a name="how-to-add-validations-to-custom-command-parameters-preview"></a>Nasıl yapılır: özel komut parametrelerine doğrulama ekleme (Önizleme)

Bu makalede, parametrelere nasıl doğrulama ekleneceğini ve düzeltme isteğinizi öğreneceksiniz.

## <a name="prerequisites"></a>Ön koşullar

Aşağıdaki makalelerde bulunan adımları tamamlamış olmanız gerekir:

- [Hızlı başlangıç: özel komut oluşturma (Önizleme)](./quickstart-custom-speech-commands-create-new.md)
- [Hızlı başlangıç: parametrelerle özel komut oluşturma (Önizleme)](./quickstart-custom-speech-commands-create-parameters.md)

## <a name="create-a-settemperature-command"></a>Setsıcaklık komutu oluşturma

Doğrulamaları göstermek için, kullanıcının sıcaklığını ayarlamaya izin veren yeni bir komut oluşturalım.

1. Daha önce oluşturduğunuz özel komutlar uygulamanızı [konuşma Studio](https://speech.microsoft.com/) 'da açın
1. Yeni bir komut **Setsıcaklık** oluştur
1. Hedef sıcaklık için bir parametre ekleyin
1. Sıcaklık parametresi için bir doğrulama ekleyin
   > [!div class="mx-imgBorder"]
   > ![Aralık doğrulaması ekle](media/custom-speech-commands/validations-add-temperature.png)

   | Ayar           | Önerilen değer                                          | Açıklama                                                                                      |
   | ----------------- | -------------------------------------------------------- | ------------------------------------------------------------------------------------------------ |
   | Adı              | Sıcaklık                                              | Komut parametrunuz için açıklayıcı bir ad                                                    |
   | Gerekli          | true                                                     | Komutu tamamlamadan önce Bu parametre için bir değer gerekip gerekmediğini belirten onay kutusu |
   | Yanıt şablonu | "-Hangi sıcaklık beğensin?"                     | Bilinmiyorsa bu parametrenin değerini sormak için bir istem                              |
   | Tür              | Sayı                                                   | Parametre türü, örneğin sayı, dize veya tarih saat                                      |
   | Doğrulama        | En düşük değer: 60, en büyük değer: 80                             | Sayı parametreleri için, parametre için izin verilen değer aralığı                             |
   | Yanıt şablonu | "-Üzgünüz, yalnızca 60 ve 80 derece arasında ayarlayabiliyorum"      | Doğrulama başarısız olursa güncelleştirilmiş bir değer istemek için uyar                                       |

1. Örnek tümceler ekleyin

   ```
   set the temperature to {Temperature} degrees
   change the temperature to {Temperature}
   set the temperature
   change the temperature
   ```

1. Sonucu doğrulamak için bir tamamlama kuralı ekleyin

   | Ayar    | Önerilen değer                                           | Açıklama                                        |
   | ---------- | --------------------------------------------------------- | -------------------------------------------------- |
   | Kural Adı  | Onay Iletisi                                      | Kuralın amacını açıklayan bir ad          |
   | Koşullar | Gerekli parametre-sıcaklık                          | Kuralın ne zaman çalıştırılabilmesine belirleme koşulları    |
   | Eylemler    | SpeechResponse-"-ok, {sıcaklık} derece olarak ayarlanıyor" | Kural koşulu true olduğunda gerçekleştirilecek eylem |

> [!TIP]
> Bu örnek, sonucu doğrulamak için bir konuşma yanıtı kullanır. Komutu bir istemci eylemiyle tamamlamaya yönelik örnekler için bkz. [nasıl yapılır: Istemcideki komutları konuşma SDK 'sı Ile kullanma (Önizleme)](./how-to-custom-speech-commands-fulfill-sdk.md)

## <a name="try-it-out"></a>Deneyin

Test panelini seçin ve birkaç etkileşimi deneyin.

- Giriş: sıcaklığı 72 derece olarak ayarlayın
- Çıkış: "Tamam, ayar 72 derece"

- Giriş: sıcaklığı 45 derece olarak ayarlayın
- Çıkış: "Üzgünüz, yalnızca 60 ve 80 derece arasında ayarlayabiliyorum"
- Giriş: bunun yerine 72 derece yapın
- Çıkış: "Tamam, ayar 72 derece"

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Nasıl yapılır: özel komuta onay ekleme (Önizleme)](./how-to-custom-speech-commands-confirmations.md)
