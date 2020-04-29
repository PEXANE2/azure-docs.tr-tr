---
title: 'Hızlı başlangıç: parametrelerle özel komut oluşturma (Önizleme)-konuşma hizmeti'
titleSuffix: Azure Cognitive Services
description: Bu makalede, özel komutlar uygulamasına parametreler ekleyeceksiniz.
services: cognitive-services
author: don-d-kim
manager: yetian
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 12/09/2019
ms.author: donkim
ms.openlocfilehash: 96312bac369cfa5fe3cb8a00fd63ecfbec624918
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "80348523"
---
# <a name="quickstart-create-a-custom-command-with-parameters-preview"></a>Hızlı başlangıç: parametrelerle özel komut oluşturma (Önizleme)

[Önceki makalede](./quickstart-custom-speech-commands-create-new.md), parametreleri olmayan komutlara yanıt vermek için yeni bir özel komutlar projesi oluşturduk.

Bu makalede, bu uygulamayı parametreleri ile genişletecektir ve bu sayede birden çok cihazı açmayı ve kapatmayı işleyebilir.

## <a name="create-parameters"></a>Parametre oluştur

1. [Daha önce oluşturduğumuz](./quickstart-custom-speech-commands-create-new.md) projeyi açın
1. Komut şimdi üzerinde işlem yaptığından ve kapatıcağından, komutu "TurnOnOff" olarak yeniden adlandırın.
   - Komutun adının üzerine gelin ve adı değiştirmek için Düzenle simgesini seçin
1. Kullanıcının cihazı açmak mı yoksa kapatmak mı istediğini göstermek için yeni bir parametre oluştur
   - Parametreler bölümünün `+` yanındaki simgeyi seçin

   > [!div class="mx-imgBorder"]
   > ![Parametre oluştur](media/custom-speech-commands/create-on-off-parameter.png)

   | Ayar            | Önerilen değer     | Açıklama                                                                                               |
   | ------------------ | ------------------- | --------------------------------------------------------------------------------------------------------- |
   | Adı               | Mikrofon               | Parametrağınız için açıklayıcı bir ad                                                                     |
   | Geneldir          | unchecked           | Bu parametre için bir değerin, projedeki tüm komutlara Global olarak uygulanıp uygulanmadığı belirten onay kutusu |
   | Gerekli           | checked             | Komutu tamamlamadan önce Bu parametre için bir değer gerekip gerekmediğini belirten onay kutusu          |
   | Yanıt şablonu  | "-Açık veya kapalı mı?"      | Bilinmiyorsa bu parametrenin değerini sormak için bir istem                                       |
   | Tür               | Dize              | Parametre türü, örneğin sayı, dize veya tarih saat                                               |
   | Yapılandırma      | Dize listesi         | Dizeler için, bir dize listesi girişleri olası değerler kümesiyle sınırlandırır                                      |
   | Dize listesi değerleri | açık, kapalı             | Dize listesi parametresi için, olası değerler ve bunların eşanlamlıları kümesi                                |

   - Ardından, cihazların adını `+` temsil etmek üzere ikinci bir parametre eklemek için simgeyi yeniden seçin. Bu örnek için bir TV ve fan

   | Ayar            | Önerilen değer       | Açıklama                                                                                               |
   | ------------------ | --------------------- | --------------------------------------------------------------------------------------------------------- |
   | Adı               | SubjectDevice         | Parametrağınız için açıklayıcı bir ad                                                                     |
   | Geneldir          | unchecked             | Bu parametre için bir değerin, projedeki tüm komutlara Global olarak uygulanıp uygulanmadığı belirten onay kutusu |
   | Gerekli           | checked               | Komutu tamamlamadan önce Bu parametre için bir değer gerekip gerekmediğini belirten onay kutusu          |
   | Yanıt şablonu  | "-Hangi cihaz?"     | Bilinmiyorsa bu parametrenin değerini sormak için bir istem                                       |
   | Tür               | Dize                | Parametre türü, örneğin sayı, dize veya tarih saat                                               |
   | Yapılandırma      | Dize listesi           | Dizeler için, bir dize listesi girişleri olası değerler kümesiyle sınırlandırır                                      |
   | Dize listesi değerleri | TV, fan               | Dize listesi parametresi için, olası değerler ve bunların eşanlamlıları kümesi                                |
   | Eş anlamlılar (TV)      | televizyon, telefon     | Bir dize listesi parametresinin olası her değeri için isteğe bağlı eş anlamlılar                                      |

## <a name="add-sample-sentences"></a>Örnek cümleler ekleyin

Parametreler ile, olası tüm birleşimleri kapsayan örnek cümleler eklemek yararlıdır. Örneğin:

1. Tam parametre bilgisi-`"turn {OnOff} the {SubjectDevice}"`
1. Kısmi parametre bilgileri-`"turn it {OnOff}"`
1. Parametre bilgisi yok-`"turn something"`

Farklı miktarda bilgi içeren örnek cümleler, özel komutlar uygulamasının, kısmi bilgilerle hem tek kararlı çözümler hem de birden çok yönlü çözümlerin çözümlenme yapmasına izin verir.

Göz önünde bulundurularak, parametreleri aşağıda önerildiği şekilde kullanmak için örnek cümleleri düzenleyin.

> [!TIP]
> Örnek cümleler düzenleyicisinde, parametrelerinizi belirtmek için küme ayraçları kullanın. - `turn {OnOff} the {SubjectDevice}`Daha önce oluşturulmuş parametrelere başvurmak için sekme tamamlamayı kullanın.

> [!div class="mx-imgBorder"]
> ![Parametrelere sahip örnek cümleler](media/custom-speech-commands/create-parameter-sentences.png)

```
turn {OnOff} the {SubjectDevice}
{SubjectDevice} {OnOff}
turn it {OnOff}
turn something {OnOff}
turn something
```

## <a name="add-parameters-to-completion-rule"></a>Tamamlama kuralına parametreler ekleyin

[Önceki hızlı](./quickstart-custom-speech-commands-create-new.md)başlangıçta oluşturduğunuz tamamlama kuralını değiştirin:

1. Yeni bir koşul ekleyin ve gerekli parametreyi seçin. Her ikisini `OnOff` de seçin`SubjectDevice`
1. Ve `OnOff` `SubjectDevice`kullanmak için konuşma yanıtı eylemini düzenleyin:

   ```
   - Ok, turning {OnOff} the {SubjectDevice}
   ```

## <a name="try-it-out"></a>Deneyin

Test sohbeti panelini açın ve birkaç etkileşimi deneyin.

- Giriş: TV 'yi kapatma
- Çıkış: Tamam, TV 'yi kapatma

- Giriş: televizyon kapatma
- Çıkış: Tamam, TV 'yi kapatma

- Giriş: kapat
- Çıkış: hangi cihaz?
- Giriş: TV
- Çıkış: Tamam, TV 'yi kapatma

## <a name="next-steps"></a>Sonraki adımlar
> [!div class="nextstepaction"]
> [Hızlı başlangıç: özel sesle özel komutlar kullanma (Önizleme)](./quickstart-custom-speech-commands-select-custom-voice.md)
